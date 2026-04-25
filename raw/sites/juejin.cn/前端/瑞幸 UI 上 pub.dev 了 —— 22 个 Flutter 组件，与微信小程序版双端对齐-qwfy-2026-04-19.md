---
---

# 瑞幸 UI 上 pub.dev 了 —— 22 个 Flutter 组件，与微信小程序版双端对齐

## 核心要点

- **项目**：瑞幸咖啡 UI 组件库，已发布到 pub.dev（Flutter 版）和 npm（微信小程序版）
- **核心理念**：以 `DESIGN.md` 作为跨端"单一真相"，一套设计语言同时驱动 WeChat 小程序和 Flutter
- **组件数量**：22 个组件，双端完全对齐

## 双端对照表

| 平台 | 包名 | 分发 | 仓库 |
|------|------|------|------|
| 微信小程序 | `lkcn-ui` | npm | [qwfy5287/lkcn-ui](https://github.com/qwfy5287/lkcn-ui) |
| Flutter | `lkcn_ui` | pub.dev | [qwfy5287/lkcn-ui-flutter](https://github.com/qwfy5287/lkcn-ui-flutter) |

> **命名坑**：pub.dev 要求 snake_case，所以 npm 的 `lkcn-ui` 到 pub.dev 成了 `lkcn_ui`

**版本号策略**：MAJOR.MINOR 对齐 + PATCH 独立（如 npm `1.2.3` + pub `1.2.1` 表示 API 对齐，Flutter 单独修了两个 bug）

## 设计语言的「跨端翻译」——5 个关键决策

### 1. Design Token：CSS 变量 → Dart const class

**小程序版（CSS 变量）**：
```css
page {
  --lkcn-blue: #1A6EFF;
  --lkcn-radius-md: 24rpx;
}
```

**Flutter 版（const class）**：
```dart
class LkcnColors {
  static const Color primary = Color(0xFF002FA7); // 克莱因蓝
  static const Color accentOrange = Color(0xFFFF6A3D);
  static const Color accentGold = Color(0xFFC9A66B);
}
class LkcnRadius {
  static const double md = 12;
  static const double pill = 999;
}
```

**使用方式**：
```dart
Container(
  decoration: BoxDecoration(
    color: LkcnColors.primary,
    borderRadius: BorderRadius.circular(LkcnRadius.md),
  ),
)
```

> **优缺点**：编译期常量、IDE 自动补全、类型安全；但换肤不能像 CSS 变量"覆盖即生效"，需上 `ThemeExtension`（首版暂不实现）

### 2. 单位：rpx → logical pixels

换算规则：**rpx = lpt × 2**

| 小程序 | Flutter |
|--------|---------|
| 28rpx | 14 lpt |
| 24rpx | 12 lpt |
| 16rpx | 8 lpt |

### 3. 组件 API：kebab-case → PascalCase / enum

| 小程序 | Flutter |
|--------|---------|
| `<lkcn-button>` | `LkcnButton` |
| `type="primary"` | `LkcnButtonType.primary` |
| `bind:tap="onClick"` | `onTap: () {}` |

> Flutter 的 enum 更严格——传不存在的 type 字符串，小程序默默 fallback，Flutter 直接编译不过

### 4. 插槽：`<slot>` → Widget 参数

```dart
LkcnCard(
  title: '我的资产',
  child: Column(children: [...]), // 主内容
  footer: Row(...), // footer 槽
)
```

### 5. Demo 组织：`pages/demo-*` → `example/lib/demos/*`

```
example/
├── lib/
│   ├── main.dart          # 按 原子/交互/容器/业务 分组的索引页
│   └── demos/
│       ├── button_demo.dart
│       ├── product_card_demo.dart
│       └── ... (21 个)
└── pubspec.yaml           # path: ../ 引用主包
```

> `cd example && flutter run` 即可运行，支持 iOS/Android/macOS/Web 四端

## 几个还原得比较得意的组件

### LkcnStepper：加购从 `+` 展开到 `[-] n [+]`

瑞幸菜单页最有辨识度的微交互：
```dart
LkcnStepper(
  value: _quantity,
  onChanged: (v) => setState(() => _quantity = v),
)
```
弹性动画走 `LkcnMotion.bounce`（即 `Cubic(0.34, 1.56, 0.64, 1)`），与 WXSS cubic-bezier 常量完全一致。

### LkcnPrice：三段式价格渲染

"符号小 + 整数大 + 小数小"的层次：
```dart
LkcnPrice(value: 9.9, original: 32, prefix: '预估到手')
```
内部将 `9.9` 拆成 `9` 和 `.9` 两段不同字号，`¥` 给第三种字号，原价走 `TextDecoration.lineThrough`。

### LkcnCouponScroll：票据左侧半圆缺口

小程序版靠 CSS `clip-path`，Flutter 用 `CustomPainter` 手画 path：
```dart
final path = Path()
  ..moveTo(r, 0)
  ..lineTo(size.width - r, 0)
  // ...
  ..lineTo(0, size.height * 0.5 + 6)
  ..arcToPoint( // ← 半圆缺口
    Offset(0, size.height * 0.5 - 6),
    radius: const Radius.circular(6),
    clockwise: false,
  )
  ..close();
```

### LkcnMembershipPlan：会员订阅全流程

方案选择器 + 订阅 CTA + 协议勾选，三件事一个 Widget 解决：
```dart
LkcnMembershipPlan(
  plans: const [
    LkcnPlan(name: '连续包月', price: 9.9, badge: '爆款天天 9.9 起'),
    LkcnPlan(name: '月卡', price: 19.9),
  ],
  agreement: '开通会员代表接受',
  agreementLinks: const [
    LkcnAgreementLink(text: '《会员服务协议》'),
    LkcnAgreementLink(text: '《自动续费协议》'),
  ],
  onSubscribe: (plan, agreed) {
    // agreed = false 时可以弹 toast 提示勾选
  },
)
```

## 快速上手

**pubspec.yaml**：
```yaml
dependencies:
  lkcn_ui: ^0.1.0
```

**业务代码示例**：
```dart
import 'package:flutter/material.dart';
import 'package:lkcn_ui/lkcn_ui.dart';

class MenuPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: LkcnColors.pageBg,
      body: ListView(
        padding: const EdgeInsets.all(16),
        children: [
          LkcnProductCard(
            image: 'https://.../coconut-latte.png',
            title: '生椰拿铁',
            tags: const ['全球销量第一', 'IIAC 金奖'],
            price: 9.9,
            originalPrice: 32,
            pricePrefix: '预估到手',
            onAdd: () {},
          ),
          const SizedBox(height: 16),
          LkcnButton.cta(
            text: '立即开通连续包月 ¥9.9',
            size: LkcnButtonSize.large,
            block: true,
            round: true,
            onTap: () {},
          ),
        ],
      ),
    );
  }
}
```

## 22 个组件速览

每个组件 API 尽量与 npm 版同名、同语义：
- 小程序 `bind:add` → Flutter `onAdd`
- 小程序 `custom-class` → Flutter 通过 `child`/`padding` 参数调

## 一些数据

- `flutter analyze` 通过（`example && flutter analyze`）
- `lib/` 下所有 `.dart` 文件
- 最低 Dart SDK：`^3.11.3`
- 最低 Flutter SDK：`>=3.22.0`

## 跨端维护经验

> **跨端组件库的真正难点不在代码，在保持纪律。**

- 使用 `[wx]`、`[flutter]`、`[design]` 标签标记变更来源

## 后续计划

- 实现 `ThemeExtension` 换肤
- 补充 `analyze` 和 `test`
- 完善 `pub publish` 流程

## 求职信息

- **岗位**：前端优先，全栈也可胜任
- **坐标**：厦门
- **案例集**：[my.feishu.cn/wiki/XUmGw8…](https://my.fe

[... summary truncated for context management ...]