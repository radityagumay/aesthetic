# Aesthetic

Aesthetic is an easy to use, fast, Rx-powered theme engine for Android applications.

[ ![jCenter](https://api.bintray.com/packages/drummer-aidan/maven/aesthetic/images/download.svg) ](https://bintray.com/drummer-aidan/maven/aesthetic/_latestVersion)
[![Build Status](https://travis-ci.org/afollestad/aesthetic.svg)](https://travis-ci.org/afollestad/aesthetic)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/b085aa9e67d441bd960f1c6abce5764c)](https://www.codacy.com/app/drummeraidan_50/aesthetic?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=afollestad/aesthetic&amp;utm_campaign=Badge_Grade)
[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg?style=flat-square)](https://www.apache.org/licenses/LICENSE-2.0.html)

<img src="https://raw.githubusercontent.com/afollestad/aesthetic/master/images/showcase.png" />

---

# Table of Contents

1. [Gradle Dependency](https://github.com/afollestad/aesthetic#gradle-dependency)
2. [Integration](https://github.com/afollestad/aesthetic#integration)
3. [Basics](https://github.com/afollestad/aesthetic#basics) 
    1. [Basic Theme Colors](https://github.com/afollestad/aesthetic#basic-theme-colors)
    2. [Status Bar](https://github.com/afollestad/aesthetic#status-bar)
    3. [Navigation Bar](https://github.com/afollestad/aesthetic#navigation-bar)
    4. [Text Colors](https://github.com/afollestad/aesthetic#text-colors)
    5. [Activity Styles](https://github.com/afollestad/aesthetic#activity-styles)
    6. [Window Background](https://github.com/afollestad/aesthetic#window-background)
    7. [View Backgrounds](https://github.com/afollestad/aesthetic#view-backgrounds)
4. [Drawer Layouts](https://github.com/afollestad/aesthetic#drawer-layouts)
5. [Tab Layouts](https://github.com/afollestad/aesthetic#tab-layouts)
6. [Bottom Navigation](https://github.com/afollestad/aesthetic#bottom-navigation)
7. [Collapsible Toolbar Layouts](https://github.com/afollestad/aesthetic#collapsible-toolbar-layouts)

---

# Gradle Dependency

The Gradle dependency is available via [jCenter](https://bintray.com/drummer-aidan/maven/assent/view).
jCenter is the default Maven repository used by Android Studio.

Add this to your module's `build.gradle` file:

```gradle
dependencies {
    // ... other dependencies
    compile 'com.afollestad:aesthetic:0.0.1'
}
```

---

# Integration

The easiest way to integrate the library is to have your Activities extend `AestheticActivity`. 
This allows the library to handle lifecycle changes for you:

```java
public class MainActivity extends AestheticActivity {
  
  @Override
  public void onCreate(@Nullable Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    // setContentView(...), etc.
    
    // If we haven't set any defaults, do that now
    if (Aesthetic.isFirstTime()) {
        Aesthetic.get()
            ...
            .apply();
    }
  }
}
```

If you don't want to extend `AestheticActivity`, there are a few methods you need to call:

```java
public class AestheticActivity extends AppCompatActivity {

  @Override
  protected void onCreate(@Nullable Bundle savedInstanceState) {
    Aesthetic.attach(this); // MUST come before super.onCreate(...)
    super.onCreate(savedInstanceState);
  }

  @Override
  protected void onResume() {
    super.onResume();
    Aesthetic.resume(this);
  }

  @Override
  protected void onPause() {
    Aesthetic.pause();
    super.onPause();
  }
}

```

You should also call `Aesthetic.destroy()` when your app is exiting.

```java
public class App extends Application {

  @Override
  public void onTerminate() {
    Aesthetic.destroy();
    super.onTerminate();
  }
}
```

---

# Basics

### Basic Theme Colors

The primary color and accent color are the two base theme colors used by apps. Generally, you 
see the primary color on things such as `Toolbar`'s, and the accent color on widgets such as 
`EditText`'s, `CheckBox`'s, `RadioButton`'s, `Switch`'s, `SeekBar`'s, `ProgressBar`'s, etc.

```java
Aesthetic.get()
    .primaryColorRes(R.color.md_indigo)
    .accentColorRes(R.color.md_yellow)
    .apply();
```

You use `Aesthetic.get()` to retrieve the current attached `Aesthetic` instance, set theme properties, 
and `apply()` theme. **This will trigger color changes in the visible screen without recreating it, and 
the properties will be persisted automatically.**

### Status Bar

The status bar is the bar on the top of your screen that shows notifications, the time, etc. (I'm 
sure you're aware of that). Per the Material Design guidelines, the status bar color should be a slightly
 darker version of the primary color. You can manually set the color:

```java
Aesthetic.get()
    .statusBarColor(R.color.md_indigo_dark)
    .apply();
```

Or you can have it automatically generated from the primary color (**you need to set the primary color first**):

```java
Aesthetic.get()
    .statusBarColorAuto()
    .apply();
```

By default, Aesthetic will automatically use light status bar mode (on Android Marshmallow and above) 
if your status bar color is light. You can modify this behavior:

```java
// AUTO is the default. ON forces light status bar mode, OFF forces it to stay disabled.
Aesthetic.get()
    .lightStatusBarMode(AutoSwitchMode.AUTO)
    .apply();
```

### Navigation Bar

By default, the navigation bar on the bottom of your screen is black. You can set it to any color 
you wish, although generally it should be the same as your primary color if not black or transparent:

```java
Aesthetic.get()
    .navBarColor(R.color.md_indigo)
    .apply();
```

You can automatically set it to the primary color, also (**you need to set the primary color first**):

```java
Aesthetic.get()
    .navBarColorAuto()
    .apply();
```

### Text Colors

You can customize text colors which are used on `TextView`'s, `EditText`'s, etc.

```java
Aesthetic.get()
    .primaryTextColorRes(android.R.color.black)
    .primaryTextColorInverseRes(android.R.color.white)
    .secondaryTextColorRes(R.color.dark_gray)
    .secondaryTextInverseColorRes(R.color.lesser_white)
    .apply();
```

Take this layout:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:orientation="vertical">

  <TextView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="Hello, world!"
    android:textColor="?android:textColorPrimary"
    android:textSize="24sp"/>

  <TextView
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="My name is Aidan."
    android:textColor="?android:textColorSecondary"
    android:textSize="16sp"/>

</LinearLayout>
```

The first `TextView` will use whatever value you set to `primaryTextColorRes` automatically. The 
second `TextView` will use whatever value you set to `secondaryTextColorRes` automatically. Aesthetic
handles swapping stock attributes with your dynamic values during view inflation.

For `EditText`'s, the primary text color is always used as the *text color*, while the secondary text 
color is always used for the *hint text color*.

### Activity Styles

Aesthetic allows you to change the actual styles.xml theme applied to Activity's:

```java
// Apply an overall light theme
Aesthetic.get()
    .activityTheme(R.style.Theme_AppCompat_Light_NoActionBar)
    .isDark(false)
    .apply();

// Apply an overall dark theme
Aesthetic.get()
    .activityTheme(R.style.Theme_AppCompat_NoActionBar)
    .isDark(false)
    .apply();
```

`isDark` is important, it's used as a hint by this library for various things. For an example, 
with a `Switch` widget, the unchecked state is either light gray or dark gray based on whether it's 
being used with a dark theme or light theme.

**When `activityTheme` is changed, `apply()` will recreate the visible `Activity`. This is the ONLY 
property which requires a recreate.**

### Window Background

Aside from changing the entire base theme of an `Activity`, you can also change just the window background:

```java
Aesthetic.get()
    .windowBgColorRes(R.color.window_background_gray)
    .apply();
```

### View Backgrounds

When you set stock or AppCompat attributes to the background of certain views, Aesthetic will
swap out the attribute with your dynamic theme colors at inflation time:

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="match_parent"
  android:layout_height="196dp"
  android:background="?colorAccent"
  android:orientation="vertical" />
```

Above, `?colorAccent` is an attribute provided by AppCompat. Aesthetic will automatically set the 
background to whatever `accentColor` you have set.

You could also use: `?colorPrimary`, `?colorPrimaryDark`, `?android:windowBackground`, 
`?android:textColorPrimary`, `?android:textColorPrimaryInverse`, 
`?android:textColorSecondary`, `?android:textColorSecondaryInverse`.

---

# Drawer Layouts

When your `Activity` has a `DrawerLayout` at its root, your status bar color will get set to the 
`DrawerLayout` instead of the `Activity`, and the `Activity`'s status bar color will be made 
transparent per the Material Design guidelines (so that the drawer goes behind the status bar).
 
If you use `NavigationView`, it will be themed automatically, also. You can customize behavior:


```java
// Checked nav drawer item will use your set primary color
Aesthetic.get()
    .navViewMode(NavigationViewMode.SELECTED_PRIMARY)
    .apply();

// Checked nav drawer item will use your set accent color
Aesthetic.get()
    .navViewMode(NavigationViewMode.SELECTED_ACCENT)
    .apply();
```

In addition, unselected nav drawer items will be shades of white or black based on the set `isDark` value.

---

# Tab Layouts

Tab Layouts from the Design Support library are automatically themed. 

You can customize background theming behavior:

```java
// The background of the tab layout will match your primary theme color. This is the default.
Aesthetic.get()
    .tabLayoutBgMode(TabLayoutBgMode.PRIMARY)
    .apply();

// The background of the tab layout will match your accent theme color.
Aesthetic.get()
    .tabLayoutBgMode(TabLayoutBgMode.ACCENT)
    .apply();
```

And indicator (underline) theming behavior:

```java
// The selected tab underline will match your primary theme color.
Aesthetic.get()
    .tabLayoutIndicatorMode(TabLayoutIndicatorMode.PRIMARY)
    .apply();

// The selected tab underline will match your accent theme color. This is the default.
Aesthetic.get()
    .tabLayoutIndicatorMode(TabLayoutIndicatorMode.ACCENT)
    .apply();
```

*The color of icons and text in your tab layout will automatically be white or black, depending on 
what is more visible over the set background color.*

---

# Bottom Navigation

Bottom Navigation Views from the Design Support library are automatically themed.

You can customize background theming behavior:

```java
// The background of the bottom tabs will match your primary theme color.
Aesthetic.get()
    .bottomNavBgMode(BottomNavBgMode.PRIMARY)
    .apply();

// The background of the bottom tabs will match your status bar theme color.
Aesthetic.get()
    .bottomNavBgMode(BottomNavBgMode.PRIMARY_DARK)
    .apply();

// The background of the bottom tabs will match your accent theme color.
Aesthetic.get()
    .bottomNavBgMode(BottomNavBgMode.ACCENT)
    .apply();

// The background of the bottom tabs will be dark gray or white depending on the isDark() property.
// This is the default.
Aesthetic.get()
    .bottomNavBgMode(BottomNavBgMode.BLACK_WHITE_AUTO)
    .apply();
```

You can also customize icon/text theming behavior:

```java
// The selected tab icon/text color will match your primary theme color.
Aesthetic.get()
    .bottomNavIconTextMode(BottomNavIconTextMode.SELECTED_PRIMARY)
    .apply();

// The selected tab icon/text color will match your accent theme color. This is the default.
Aesthetic.get()
    .bottomNavIconTextMode(BottomNavIconTextMode.SELECTED_ACCENT)
    .apply();

// The selected tab icon/text color will be black or white depending on which is more visible 
// over the background of the bottom tabs.
Aesthetic.get()
    .bottomNavIconTextMode(BottomNavIconTextMode.BLACK_WHITE_AUTO)
    .apply();
```

---

# Collapsible Toolbar Layouts

Collapsible Toolbar Layouts are automatically themed, as seen in the sample project.

<img src="https://raw.githubusercontent.com/afollestad/aesthetic/master/images/collapsing_appbar.png" width="600" />

In the sample layout, we automatically set the accent color to the expanded view. The collapsed toolbar 
color will match whatever color your toolbar uses, which is the primary theme color by default. You'll 
also notice that the icons and title color are updated to be most visible over the background color.