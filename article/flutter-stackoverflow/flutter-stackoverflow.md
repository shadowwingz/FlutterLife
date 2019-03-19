### GridView 顶部有多余空白

解决：

用 `MediaQuery.removePadding` 包裹 GridView，设置 `removeTop` 为 true：

```dart
MediaQuery.removePadding(
    context: context,
    child: GridView.count(
      crossAxisCount: this.crossAxisCount,
      children: widgets,
    ),
    removeTop: true,
  )
```

[Unexpected top padding in ListView put inside scaffold with no appBar](https://github.com/flutter/flutter/issues/14842#issuecomment-371344881)

### GridView 直接放到 Column 中显示不出来

解决：

GridView 外面嵌套一个 Expanded

[Flutter Gridview in Column. What's solution..?](https://stackoverflow.com/a/51167158)

### Column 在 Center 中不居中

不需要使用 Center，直接设置 Column 的 `mainAxisAlignment` 为 `MainAxisAlignment.center` 即可居中。

[Cannot Center Column Widget](https://stackoverflow.com/a/53581804)

### GridView 和 ListView 去除拉出回弹效果 overscroll

首先，自定义一个 ScrollBehavior：

```dart
class NoOverScrollBehavior extends ScrollBehavior {
  @override
  Widget buildViewportChrome(
      BuildContext context, Widget child, AxisDirection axisDirection) {
    return child;
  }
}
```

然后在 GridView 和 ListView 的外层包裹一个 ScrollConfiguration，然后把 NoOverScrollBehavior 设置进去：

```dart
ScrollConfiguration(
    behavior: NoOverScrollBehavior(),
    child: GridView.count(
      crossAxisCount: this.crossAxisCount,
      children: widgets,
    ));
```

[How to remove scroll glow?](https://stackoverflow.com/a/51119796)