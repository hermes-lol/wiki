# 生产事故-那些年遇到过的OOM - 程语有云

> Author: mylibs
> Date: 2025-12-08
> Source: https://www.cnblogs.com/mylibs/p/19321491/production-accident-vl01

---

入职多年，面对生产环境，尽管都是小心翼翼，慎之又慎，还是难免捅出篓子。轻则满头大汗，面红耳赤。重则系统停摆，损失资金。每一个生产事故的背后，都是宝贵的经验和教训，都是项目成员的血泪史。为了更好地防范和遏制今后的各类事故，特开此专题，长期更新和记录大大小小的各类事故。有些是亲身经历，有些是经人耳传口授，但无一例外都是真实案例。
注意：为了避免不必要的麻烦和商密问题，文中提到的特定名称都将是化名、代称。 0x00 大纲
目录0x00 大纲0x01 案例一0x02 案例二0x03 案例三0x04 案例四0x05 案例五0x06 案例六0x07 案例七0x08 案例八
0x01 案例一
事故时间：2018年6月13日
故障类型：java.lang.OutOfMemoryError: Java heap space
事故经过：某考务管理系统，前期收集考生报名信息时允许上传ZIP附件提交相关材料，后台服务会解析压缩包并从中获取相关文件。
系统运行后不久，考务群就陆续有人反馈报名网站打不开，无法访问等等。让运维重启系统后，又恢复正常，跑了一段时间以后，又有人说无法访问。仔细检查故障时的日志，发现故障时间点都是发生在有人上传ZIP文件的时候。
从服务器上提取了一部分样本，发现压缩文件里面包含若干个TXT文件，TXT文件中是重复的字符，类似AAA...该TXT文件原始数据巨大且单调重复，导致压缩后的ZIP却非常小，真是个天才！直觉告诉我们这是被恶意攻击了，遂暂时关闭了文件上传接口，改为通过表单录入信息报名。
事后复盘当时的代码，发现处理ZIP文件时没有释放到磁盘临时文件，都是在内存中直接解压并读取解压后的文本数据，这就给了攻击者可乘之机。但是后来专门去研究了下这方面的安全漏洞，发现这是一种ZIP炸弹（ZIP of Death or ZIP Bomb），即使是释放到磁盘，也有可能造成磁盘资源耗尽。除了构造简单重复内容，还能通过递归嵌套，目录穿越等构造恶意的ZIP并释放巨量数据，有兴趣的朋友可以去自行查阅。
解决方案：禁止上传嵌套压缩包，只允许上传单级压缩文件；检查文件大小；检查文件路径。
0x02 案例二
事故时间：2021年6月30日
故障类型：java.lang.OutOfMemoryError: Metaspace
事故经过：某报文处理服务，需要同时处理多种渠道的XML报文，使用了 JAXB (Java Architecture for XML Binding) 和 XSD (XML Schema Definition) 进行报文编/解组和格式检查。
随着业务越来越繁重，某次上线后，生产服务频繁出现java.lang.OutOfMemoryError: Metaspace内存异常。最后经查是因为应用启动时，一次性加载了全量的XSD和Document对象，大量的加载类填满了Metaspace。
应用JVM参数-XX:MaxMetaspaceSize、-XX:MetaspaceSize均设置为256MB，当时的加载代码如下：
SchemaFactory schemaFactory = SchemaFactory.newInstance(XMLConstants.W3C_XML_SCHEMA_NS_URI);
return schemaFactory.newSchema(schemaSources); 一次性初始化了所有的XSD源。老规矩，先救命再治病： 临时解决方案：评估最大Metaspace并扩容
最终解决方案：服务拆分，分块加载 0x03 案例三
事故时间：2022年3月17日
故障类型：java.lang.StackOverflowError
事故经过：某营销管理系统对接第三方接口上送的数据，并进行解析处理，触发对应的业务流程。其中一个业务处理是给编号为0-N的直连机构推送通知，按照接口约定，其中N由第三方接口指定，且最大值不会超过255。
管理后台采用了类似这样的代码进行处理：
public static void process(int corpNum) { try { System.out.println("发送通知给企业，当前编号: " + corpNum); sendSms(corpNum); } catch (RuntimeException e) { System.err.println("发送通知给企业失败，当前编号: " + corpNum); } if (corpNum != 0) { process(corpNum - 1); }
} 上线之后系统一直运行良好，直到有一天，第三方接口上送数据时传了个-1，嚯！系统直接崩了，打电话过去对方说是配置有误，导致参数填写错误。这边喜提java.lang.StackOverflowError。
其实测试之初应该可以避免的，但是负责该业务的开发过于信任第三方上送的数据，没有考虑到意外的参数范围，狠狠的交了一笔学费。
解决方案：增加严格的参数校验，同时修改尾递归写法为循环发送。
0x04 案例四
事故时间：2022年4月15日
故障类型：java.lang.OutOfMemoryError: unable to create new native thread
事故经过：某接口服务配置了无界线程池作为业务线程池。该接口业务非常简单，收集各个上游服务的度量指标 (Metrics) ，简单记录日志并写入数据库，轻量、高频、无长时间阻塞，一切都那么完美。
然而，某天突然运维报告服务不可用，查询日志发现服务已经凉了有段时间，死因是java.lang.OutOfMemoryError: unable to create new native thread。还好留下了堆栈，一通分析，发现是有段时间应用日志所在磁盘空间写满，导致线程得不到释放，高频调用之下，最终无法创建新线程，导致服务被压垮。
那么为什么写日志会阻塞线程呢？当时应用使用的是logback日志实现，查看其配置，使用的是AsyncAppender异步记录器：
<appender name="file.async" class="ch.qos.logback.classic.AsyncAppender"> <!-- 不丢失日志 --> <discardingThreshold>0</discardingThreshold> <appender-ref ref="file.log"/>
</appender> 这里的配置正是压死骆驼的最后一根稻草。日志首先被写入BlockingQueue内存队列，再由工作线程异步写入磁盘。如果磁盘写满导致下游FileAppender无法正常工作，而AsyncAppender的队列又被填满，就会导致对Logger的调用发生阻塞。
官方文档里对于discardingThreshold是这样描述的： In light of the discussion above and in order to reduce blocking, by default, when less than 20% of the queue capacity remains, AsyncAppender will drop events of level TRACE, DEBUG and INFO keeping only events of level WARN and ERROR. This strategy ensures non-blocking handling of logging events (hence excellent performance) at the cost loosing events of level TRACE, DEBUG and INFO when the queue has less than 20% capacity. Event loss can be prevented by setting the discardingThreshold property to 0 (zero). 设置为0，虽然可以防丢，但也让logback没有退路可言。
解决方案：为接口配置有界线程池，并调整discardingThreshold为合理数值。
0x05 案例五
事故时间：2022年5月25日
故障类型：java.lang.OutOfMemoryError: Java heap space
事故经过：某后台管理系统，由于存在敏感数据，需要在本地安装安全控件来辅助访问，该系统在首页上提供了多个版本的控件安装包下载。
上线之初系统运行都挺正常，但是某天突然有用户反馈系统无法访问，浏览器提示502网关错误。查阅发现服务已挂，应用日志提示java.lang.OutOfMemoryError: Java heap space，使用MAT(Memory Analyzer Tool)工具分析dump文件，发现存在大量的byte[]内存占用。
结合应用日志，发现服务异常之时正在调用某个文件下载方法，该方法使用FileInputStream读取文件到内存中，并使用byte[]数组存储文件内容， subsequent to将该byte[]数组写入到Response的输出流完成下载，关键代码如下：
public static byte[] readFileContent(File file) { long fileLength = file.length(); byte[] fileContent = new byte[(int) fileLength]; try (FileInputStream in = new FileInputStream(file)) { in.read(fileContent); return fileContent; } catch (Exception e) { logger.error(e.getMessage(), e); return null; }
} 短短几行代码却让人虎躯一震，没有判断文件的大小就直接完整读取，危险！而且没有使用缓冲流的方式进行读写。事实证明问题恰恰就是出在这里，某个版本的控件由于打包时体积偏大（约200多MB），导致多个用户同时下载时，堆区内存一下子就被控件文件数据填满，进而发生OOM异常。
解决方案：将控件安装包文件挂载到FTP上并提供外链，不经过应用服务器下载。
0x06 案例六
事故时间：2023年3月10日
故障类型：java.lang.OutOfMemoryError: Java heap space
事故经过：A公司开发人员在开发某开放接口时，需要调用C公司的一个基础数据接口服务。然而，从14时许开始，A公司的接口调用就开始出现异常，返回错误码500，错误信息为java.lang.OutOfMemoryError: Java heap space。
C公司开发人员向A公司开发人员反映某开放接口从14时许开始无法访问和使用。该系统为某基础数据接口服务，基于HTTP协议进行通信。
按照惯例，首先排查网络是否异常，经运维人员检查，证明网络连通性没有问题。A公司开发组于14时30分通知运维人员重启应用服务，期间短暂恢复正常。但是，很快，十分钟后，电话再次响起，告知服务又出现异常，无法访问。
在日志中搜索，找到了若干处内存溢出错误java.lang.OutOfMemoryError: Java heap space，但是令人费解的是每次出现OOM错误的位置居然都不一样。最后发现是应用启动脚本中，-Xmn参数设置成与-Xmx参数一样的大小，导致堆区大小失衡，进而引发内存异常。
该问题的排查过程在生产事故-记一次特殊的OOM排查一文中有详细的分析过程，这里就不再赘述了。
0x07 案例七
事故时间：2024年4月28日
故障类型：java.lang.OutOfMemoryError: Java heap space
事故经过：某报表分析系统，其业务大体上为导入各种CSV/XLS/XLSX文件进行解析，校验并计算各项统计数据，对于异常的数据可以在首页上监控告警并提示。
有天运营的妹子突然找过来说她登录不了系统了，刚开始听到的我认为只是简单的浏览器问题，可以秀一波操作了，结果到了工位上一看，发现登录页面验证码出不来了。做过前后端分离项目的朋友都知道，这种情况下，后端服务非死即伤。强装镇定，安抚一下妹子，说我得去查查日志看看咋回事。
远程到服务器，发现后端应用确实已经灰飞烟灭，查看GC日志，发现有若干java.lang.OutOfMemoryError: Java heap space错误。找到那段时间的应用日志，最终问题定位到了某个SQL语句上，该SQL是个单表查询语句，但是返回的记录行数竟然有10w+。
追查源头，发现就是首页上的监控告警。前端定时器每隔20秒调用一次后端服务扫描该表的记录，筛选出状态异常的数据并返回，但是没有做分页限制，导致某个业务人员上传了一个超大的Excel表，但是有个关键数据项填写错误，该批数据10w+行记录全部被系统标记为异常，当有多个运营人员登录系统并进入首页后，就会反复触发该查询语句，进而导致内存溢出。
解决方案：限制首页监控查询行数，同时优化监控逻辑，建立查询缓存，防止短时间内重复扫描业务表。
0x08 案例八
事故时间：2024年12月5日
故障类型：java.lang.OutOfMemoryError: GC Overhead limit exceeded
事故经过：某查询接口服务，上线后基本稳定运行，三个月后有用户反映查询缓慢。
遂查之，发现GC日志中频繁出现java.lang.OutOfMemoryError: GC Overhead limit exceeded报告。第一时间做了堆栈快照，发现内存中有大量的List容器未释放，MAT分析Incoming references指向了ThreadLocalMap，基本可以定位到是ThreadLocal中的数据没有及时清理，无法被GC回收，导致的内存泄露，最终频繁Full GC也无法回收足够空间。
解决方案：严格遵循使用后释放的原则，及时移除ThreadLocal中的数据引用。