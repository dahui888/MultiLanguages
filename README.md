# 国际化框架

> 码云地址：[Gitee](https://gitee.com/getActivity/MultiLanguages)

> [点击此处下载Demo](MultiLanguages.apk)

![](MultiLanguages.gif)

#### 集成步骤

```groovy
dependencies {
    implementation 'com.hjq:language:5.0'
}
```

#### 初始化框架

```java
// 在 Application 中初始化
MultiLanguages.init(this);
```

> 重写 Application 的 attachBaseContext 方法

```java
@Override
protected void attachBaseContext(Context base) {
    // 国际化适配（绑定语种）
    super.attachBaseContext(MultiLanguages.attach(base));
}
```

> 重写**基类** BaseActivity 的 attachBaseContext 方法

```java
@Override
protected void attachBaseContext(Context newBase) {
    // 国际化适配（绑定语种）
    super.attachBaseContext(MultiLanguages.attach(newBase));
}
```

> 只要是 Context 的子类都需要重写，Service 也雷同，这里不再赘述

#### 语种设置

```java
// 设置当前的语种（返回 true 需要重启 App）
MultiLanguages.setAppLanguage(Context context, Locale locale);

// 获取当前的语种
MultiLanguages.getAppLanguage(Context context);
```

#### 使用案例

```java
@Override
public void onClick(View v) {
    // 是否需要重启
    boolean restart;
    switch (v.getId()) {
        // 跟随系统
        case R.id.btn_language_auto:
            restart = MultiLanguages.setSystemLanguage(this);
            break;
        // 简体中文
        case R.id.btn_language_cn:
            restart = MultiLanguages.setAppLanguage(this, Locale.CHINA);
            break;
        // 繁体中文
        case R.id.btn_language_tw:
            restart = MultiLanguages.setAppLanguage(this, Locale.TAIWAN);
            break;
        // 英语
        case R.id.btn_language_en:
            restart = MultiLanguages.setAppLanguage(this, Locale.ENGLISH);
            break;
        default:
            restart = false;
            break;
    }

    if (restart) {
        // 我们可以充分运用 Activity 跳转动画，在跳转的时候设置一个渐变的效果
        startActivity(new Intent(this, LanguageActivity.class));
        overridePendingTransition(R.anim.activity_alpha_in, R.anim.activity_alpha_out);
        finish();
    }
}
```

#### 其他 API

```java
// 将 App 语种设置为系统语种（返回 true 需要重启 App）
MultiLanguages.setSystemLanguage(Context context);
// 获取系统的语种
MultiLanguages.getSystemLanguage();

// 对比两个语言是否是同一个语种（比如：中文的简体和繁体，英语的美式和英式）
MultiLanguages.equalsLanguage(Locale locale1, Locale locale2);
// 对比两个语言是否是同一个地方的（比如：中国大陆用的中文简体，中国台湾用的中文繁体）
MultiLanguages.equalsCountry(Locale locale1, Locale locale2);

// 获取某个语种下的 String
MultiLanguages.getLanguageString(Context context, Locale locale, int stringId);
// 获取某个语种下的 Resources 对象
MultiLanguages.getLanguageResources(Context context, Locale locale);
```
