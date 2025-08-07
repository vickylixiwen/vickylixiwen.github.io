---
layout: post
title:  "Jetpack Compose Learning"
date:   2022-06-22 
categories: Compose

---

#### 一些背景知识
- 什么是compose

- 为什么要用Compose
用了一段时间的compose后，感觉对我最大的好处就是可以很直观的看到具体的UI界面，不需要等到DEV把整个功能都完成后，接入整个APP，才能提测，我可以直接根据DEV准备的preview class和一些sample数据，提前准备我的测试数据，测试一些边界的场景，提前介入开始测试

#### Terms
Composition: a description of the UI built by Jetpack Compose when it executes composables.

Initial composition: creation of a Composition by running composables the first time.

Recomposition: re-running composables to update the Composition when data changes.

Semantics: UI tests in Compose use semantics to interact with the UI hierarchy. Semantics,   as the name implies,   give meaning to a piece of UI.

remember: Composable functions can use the remember API to store an object in memory. A value computed by remember is stored in the Composition during initial composition,   and the stored value is returned during recomposition. remember can be used to store both mutable and immutable objects.

#### Preview
主要用于在split/design窗口看预览图,   找到带有@Preview annotation的方法， 如下图：
![元素snippet](/assets/compose_preview.png "compose preview")

{% highlight kotlin %}
@Preview
@Composable
fun UserInfo(@PreviewParameter(SampleUserProvider::class) user:User) {
    Text(user.name+ " "+user.age)
}
{% endhighlight %}

#### 如何测试Composables
- 安装dependency:
`// Test rules and transitive dependencies:
androidTestImplementation("androidx.compose.ui:ui-test-junit4:$compose_version")
// Needed for createComposeRule,   but not createAndroidComposeRule:
debugImplementation("androidx.compose.ui:ui-test-manifest:$compose_version")`

- 获取Rule
`    @get:Rule
    val composeTestRule = createComposeRule()
    // use createAndroidComposeRule<YourActivity>() if you need access to an activity
`

- 给Compose设置内容:
根据compose的实际参数，通过setContent传参生成一个可测试的compose，大部分的compose可以像下面的snippet一样setContent：

{% highlight kotlin %}
        composeTestRule.setContent {
        RallyTopAppBar(
            allScreens = allScreens,  
            onTabSelected = { },  
            currentScreen = RallyScreen.Accounts
        )
    }
{% endhighlight %}


- 如何debug
{% highlight kotlin %}
        composeTestRule.onRoot().printToLog("TAG")
{% endhighlight %}

`打印出来看到的就是这样的一个semantics tree
        |-Node #3 at (l=42.0,   t=105.0,   r=105.0,   b=168.0)px
        | Role = 'Tab'
        | Selected = 'false'
        | StateDescription = 'Not selected'
        | ContentDescription = 'Overview'
        | Actions = [OnClick]
        | MergeDescendants = 'true'
        | ClearAndSetSemantics = 'true'
`

- 常用的方法：
{% highlight kotlin %}
    composeTestRule.onNodeWithText("Continue").performClick()
    composeTestRule.onNodeWithText("Welcome").assertIsDisplayed()
{% endhighlight %}

- SemanticsMatcher:
当没法直接使用onNodeWithText,   onNodeWithContentDescription时，可能需要使用SemanticsMatcher


- 通过gradlew运行测试
`./gradlew :emma:connectedGoogleDebugAndroidTest`


#### 我踩过的坑
- API target：尽量使用新一点的version，至少在Android 9 上跑起来是会报错的
我曾经在Android9上测试composeTestRule,   会一直fail，后来尝试在Android 10， 11， 12 的模拟器上测试，就能顺利跑通，所以在选用device的时候一定要注意device的version，太老的version应该是不能跑composeTestRule的
