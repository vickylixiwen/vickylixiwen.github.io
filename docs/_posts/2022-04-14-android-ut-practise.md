---
layout: post
title:  "Android UT Practise"
date:   2022-04-14 
categories: unit test
---

#### 单元测试的分类
本地测试(Local tests): 只在本地机器JVM上运行，以最小化执行时间，这种单元测试不依赖于Android框架，或者即使有依赖，也很方便使用模拟框架来模拟依赖，以达到隔离Android依赖的目的，模拟框架如google推荐的Mockito；

仪器化测试(Instrumented tests): 在真机或模拟器上运行的单元测试，由于需要跑到设备上，比较慢，这些测试可以访问仪器（Android系统）信息.
The Android testing API provides hooks into the Android component and application life cycle. These hooks are called the instrumentation API and allow your tests to control the life cycle and user interaction events.

app/src
     ├── androidTestjava (仪器化单元测试、UI测试)
     ├── main/java (业务代码)
     └── test/java  (本地单元测试)


#### How to run the test
Run your local unit tests via Gradle with the ```gradlew connectedCheck``` command.


Activity testing
以前，一般都是用Android Test Support Library下的ActivityTestRule(或者Robolectric里的ActivityController)来测试activity的生命周期。随着AndroidX Test library的到来，ActivityScenario的API已经开始取代ActivityTestRule。[参考](https://medium.com/stepstone-tech/better-tests-with-androidxs-activityscenario-in-kotlin-part-1-6a6376b713ea)

ActivityScenario does’t clean up device state automatically and may leave the activity keep running after the test finishes. Call close() in your test to clean up the state or use try-with-resources statement. This is optional but highly recommended to improve the stability of your tests.



#### ActivityScenarioRule



#### 添加依赖
dependencies {
  // Required -- JUnit 4 framework
  testImplementation "junit:junit:$jUnitVersion"
  // Optional -- Robolectric environment
  testImplementation "androidx.test:core:$androidXTestVersion"
  // Optional -- Mockito framework
  testImplementation "org.mockito:mockito-core:$mockitoVersion"
  // Optional -- mockito-kotlin
  testImplementation "org.mockito.kotlin:mockito-kotlin:$mockitoKotlinVersion"
  // Optional -- Mockk framework
  testImplementation "io.mockk:mockk:$mockkVersion"

  androidTestImplementation "androidx.test:runner:$androidXTestVersion"
  androidTestImplementation "androidx.test:rules:$androidXTestVersion"
  // Optional -- UI testing with Espresso
  androidTestImplementation "androidx.test.espresso:espresso-core:$espressoVersion"
  // Optional -- UI testing with UI Automator
  androidTestImplementation "androidx.test.uiautomator:uiautomator:$uiAutomatorVersion"
  // Optional -- UI testing with Compose
  androidTestImplementation "androidx.compose.ui:ui-test-junit4:$compose_version"
}
Note: testImplementation添加的是local tests的依赖， androidTestImplementation添加的是Instrumented tests的依赖


#### Types of test doubles测试替身的种类(source: )
There are various types of test doubles:

Fake	A test double that has a "working" implementation of the class,   but it is implemented in a way that makes it good for tests but unsuitable for production.
Example: an in-memory database.

Fakes don't require a mocking framework and are lightweight. They are preferred.

Mock	A test double that behaves how you program it to behave and that has expectations about its interactions. Mocks will fail tests if their interactions don't match the requirements that you define. Mocks are usually created with a mocking framework to achieve all this.
Example: Verify that a method in a database was called exactly once.

Stub	A test double that behaves how you program it to behave but doesn't have expectations about its interactions. Usually created with a mocking framework. Fakes are preferred over stubs for simplicity.
Dummy	A test double that is passed around but not used,   such as if you just need to provide it as a parameter.
Example: an empty function passed as a click callback.

Spy	A wrapper over a real object which also keeps track of some additional information,   similar to mocks. They are usually avoided for adding complexity. Fakes or mocks are therefore preferred over spies.
Shadow	Fake used in Robolectric.

#### Misc errors 
    *Error: The app for your currently selected variant (Unknown output) is not signed. Please specify a signing configuration for this variant (debug)

    Solution: Follow the step below: Build Variants (lower-left corner) > Active Build Variant > change it back to Debug


    Calling startActivity() from outside of an Activity context requires the FLAG_ACTIVITY_NEW_TASK flag. Is this really what you want?
    Regarding Activity jump,   there is a startActivity method in Context. Activity inherits from Context and overloads the startActivity method. If you use the startActivity method of Activity,   there will be no restrictions. If you use the startActivity method of Context,   you need to start a new task. This exception is encountered because the startActivity method of Context is used. The solution is to add a flag. Code:

        ```
        Intent intent = new Intent();
        intent.setClass(context,   class);
        intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK );
        context.startActivity(intent);
        ```
    * error: androidx.test.ext.junit.runners.AndroidJUnit4.throwInitializationError(AndroidJUnit4.java:129)
    Solution: Change test class to AndroidJUnit4ClassRunner from AndroidJUnit4







    * error from ActivityScenarioRule(NewHomeActivity::class.java):
        java.lang.AssertionError: Activity never becomes requested state "[DESTROYED,   RESUMED,   STARTED,   CREATED]" (last lifecycle transition = "PRE_ON_CREATE")





#### How to run test under other variants
    androidTest默认执行的是debug的build type,   需要改到其他的variants的话，添加需要的testBuildType到(kotlin)
    android {
        ...
        testBuildType = "beta"
    }


#### 通过run test打出来的包，带test后缀
需要先assemble assembleBetaAndroidTest
./gradlew assembleBetaAndroidTest -x test


    UserPrefs.Companion.get(context).setId("1");


try for free plan need to double  check on live
