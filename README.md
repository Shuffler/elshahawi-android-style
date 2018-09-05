# 1. Project guidelines

## 1.1 File naming

### 1.1.1 Class files
Class names are written in [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase).

For classes that extend an Android component, the name of the class should end with the name of the component; for example: `SignInActivity`, `SignInFragment`, `ImageUploaderService`, `ChangePasswordDialog`.

### 1.2.2 Resources files

Resources file names are written in __lowercase_underscore__.

#### 1.2.2.1 Drawable files

Naming conventions for drawables:


| Asset Type   | Prefix            |		Example               |
|--------------| ------------------|-----------------------------|
| Icon         | `ic_`	            | `ic_star_32dp.png`               |
| Image        	| `img_`             | `img_logo.png`          |
| Button background      | `btn_`	            | `btn_send_pressed.9.png`    |
| Divider      | `divider_`        | `divider_dark.png`  |
| Selector	| `selector_`		| `selector_btn_send.xml`

Naming conventions for selector states:

| State	       | Suffix          | Example                     |
|--------------|-----------------|-----------------------------|
| Normal       | `_normal`       | `btn_order_normal.9.png`    |
| Pressed      | `_pressed`      | `btn_order_pressed.9.png`   |
| Focused      | `_focused`      | `btn_order_focused.9.png`   |
| Disabled     | `_disabled`     | `btn_order_disabled.9.png`  |
| Selected     | `_selected`     | `btn_order_selected.9.png`  |


#### 1.2.2.2 Layout files

Layout files should match the name of the Android components that they are intended for:

| Component        | Class Name             | Layout Name                   |
| ---------------- | ---------------------- | ----------------------------- |
| Activity         | `UserProfileActivity`  | `user_profile_activity.xml`   |
| Fragment         | `SignUpFragment`       | `sign_up_fragment.xml`        |
| Dialog           | `ChangePasswordDialog` | `change_password_dialog.xml`  |
| AdapterView item | ---                    | `visit_list_item.xml`             |
| View / ViewGroup | `SliderView`           | `slider_view.xml`             |
| Partial layout   | ---                    | `partial_stats_bar.xml`       |

#### 1.2.2.3 Menu files

Similar to layout files, menu files should match the name of the component. For example, if we are defining a menu file that is going to be used in the `UserActivity`, then the name of the file should be `user_activity.xml`

#### 1.2.2.4 Values files

Resource files in the values folder should be __plural__, e.g. `strings.xml`, `styles.xml`, `colors.xml`, `dimens.xml`, `attrs.xml`

# 2 Code guidelines

## 2.1 Java language rules

### 2.1.1 Don't ignore exceptions

You must never do the following:

```java
void setServerPort(String value) {
    try {
        serverPort = Integer.parseInt(value);
    } catch (NumberFormatException e) { }
}
```

_While you may think that your code will never encounter this error condition or that it is not important to handle it, ignoring exceptions like above creates mines in your code for someone else to trip over some day. You must handle every Exception in your code in some principled way. The specific handling varies depending on the case._ - ([Android code style guidelines](https://source.android.com/source/code-style.html))

See alternatives [here](https://source.android.com/source/code-style.html#dont-ignore-exceptions).

### 2.1.2 Don't catch generic exception

You should not do this:

```java
try {
    someComplicatedIOFunction();        // may throw IOException
    someComplicatedParsingFunction();   // may throw ParsingException
    someComplicatedSecurityFunction();  // may throw SecurityException
    // phew, made it all the way
} catch (Exception e) {                 // I'll just catch all exceptions
    handleError();                      // with one generic handler!
}
```

See the reason why and some alternatives [here](https://source.android.com/source/code-style.html#dont-catch-generic-exception).
If you still decide to do this, write a comment why this is a good idea.

### 2.1.3 Avoid Serialization and Deserialization

_Serialization and deserialization of objects is a CPU-intensive procedure and is likely to slow down your application. While performance loss won't be visible on small objects beware of serialization and deserialization of large lists of objects.
Preferable approach is to use [Parcelable](https://developer.android.com/reference/android/os/Parcelable.html)._

### 2.1.4 Avoid Synchronized

Synchronization has hight performance cost in java and improper synchronization can also cause a deadlock.
So use synchronization if you are truly dealing with multithreaded processes and doing so follow the guidelines.
You can read more detailed guidelines here: [Java Language Best Practices](http://docs.oracle.com/cd/A97688_16/generic.903/bp/java.htm#1006832)

#### 2.1.4.1 Synchronize Critical Sections Only

If only certain operations in the method must be synchronized, use a synchronized block with a lock instead of synchronizing the entire method. For example:
```
private final Object lock = new Object();
    ...
    private void doSomething()
    {
     // perform tasks that do not require synchronicity
        ...
        synchronized (lock)
            {
            ...
            }
        ...
    }
```

#### 2.1.4.2 Know Which Java Objects Already Have Synchronization Built-in
Some Java objects (such as Hashtable, Vector, and StringBuffer) already have synchronization built into many of their APIs. They may not require additional synchronization.

## 2.2 Java style rules

### 2.2.1 Fields definition and naming

Fields should be defined at the __top of the file__ and they should follow the naming rules listed below.

* Static final fields (constants) are ALL_CAPS_WITH_UNDERSCORES.
* Other fields start with a lower case letter.

Example:

```java
public class MyClass {
    public static final int SOME_CONSTANT = 42;
    private static MyClass myClass;
    public int publicField;
    int packagePrivateField;
    private int privateField;
    protected int protectedField;
}
```

### 2.2.2 Treat acronyms as words

| Good           | Bad            |
| -------------- | -------------- |
| `XmlHttpRequest` | `XMLHTTPRequest` |
| `getCustomerId`  | `getCustomerID`  |
| `String url`     | `String URL`     |
| `long id`        | `long ID`        |

### 2.2.3 Use spaces for indentation

Use __4 space__ indents for blocks:

```java
if (x == 1) {
    x++;
}
```

Use __8 space__ indents for line wraps:

```java
Instrument i =
        someLongExpression(that, wouldNotFit, on, one, line);
```

### 2.2.4 Use standard brace style

Braces go on the same line as the code before them.

```java
class MyClass {
    int func() {
        if (something) {
            // ...
        } else if (somethingElse) {
            // ...
        } else {
            // ...
        }
    }
}
```

Braces around the statements are required unless the condition and the body fit on one line.

If the condition and the body fit on one line and that line is shorter than the max line length, then braces are not required, e.g.

```java
if (condition) body();
```

This is __bad__:

```java
if (condition)
    body();  // bad!
```

### 2.2.5 Annotations

#### 2.2.5.1 Annotations practices

According to the Android code style guide, the standard practices for some of the predefined annotations in Java are:

* `@Override`: The @Override annotation __must be used__ whenever a method overrides the declaration or implementation from a super-class. For example, if you use the `@inheritDoc` Javadoc tag, and derive from a class (not an interface), you must also annotate that the method `@Override`s the parent class's method.

* `@SuppressWarnings`: The `@SuppressWarnings` annotation should only be used under circumstances where it is impossible to eliminate a warning. If a warning passes this "impossible to eliminate" test, the `@SuppressWarnings` annotation must be used, so as to ensure that all warnings reflect actual problems in the code.

More information about annotation guidelines can be found [here](http://source.android.com/source/code-style.html#use-standard-java-annotations).

#### 2.2.5.2 Annotations style

__Classes, Methods and Constructors__

When annotations are applied to a class, method, or constructor, they are listed after the documentation block and should appear as __one annotation per line__ .

```java
/* This is the documentation block about the class */
@AnnotationA
@AnnotationB
public class MyAnnotatedClass { }
```

__Fields__

Annotations applying to fields should be listed __on the same line__, unless the line reaches the maximum line length.

```java
@Nullable @Mock DataManager mDataManager;
```

### 2.2.6 Limit variable scope

_The scope of local variables should be kept to a minimum (Effective Java Item 29). By doing so, you increase the readability and maintainability of your code and reduce the likelihood of error. Each variable should be declared in the innermost block that encloses all uses of the variable._

_Local variables should be declared at the point they are first used. Nearly every local variable declaration should contain an initializer. If you don't yet have enough information to initialize a variable sensibly, you should postpone the declaration until you do._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#limit-variable-scope))

### 2.2.7 Logging guidelines

VERBOSE and DEBUG logs __must__ be disabled on release builds. It is also recommended to disable INFORMATION, WARNING and ERROR. If you decide to leave them enabled, you have to make sure that they are not leaking private information such as email addresses, user ids, etc.

### 2.2.8 Class member ordering

There is no single correct solution for this but using a __logical__ and __consistent__ order will improve code learnability and readability. It is recommendable to use the following order:

1. Inner interfaces
2. Constants
3. Static fields
4. Static methods
5. Fields
6. Constructors
7. Overridden methods and callbacks
8. Public methods
9. Private methods
10. Inner classes

Example:
```java
public class MainFragment extends Fragment {

    public interface Callback {
        void onPhotoSent();
    }

    public static final String TAG = "MainFragment";
    private static final int DEFAULT_TIMEOUT = 20;

    public static String publicStaticString;
    private static String privateStaticString;

    public static Fragment newInstance() {
        return new MainFragment();
    }

    private String title;
    private TextView textView;

    public void setTitle(String title) {
        this.title = title;
    }

    @Override
    public void onCreate() {
        ...
    }

    private void setUpView() {
        ...
    }

    static class AnInnerClass {
        ...
    }

}
```

If your class is extending an __Android component__ such as an Activity or a Fragment, it is a good practice to order the override methods so that they __match the component's lifecycle__. For example, if you have an Activity that implements `onCreate()`, `onDestroy()`, `onPause()` and `onResume()`, then the correct order is:

```java
public class MainActivity extends Activity {

	//Order matches Activity lifecycle
    @Override
    public void onCreate() {}

    @Override
    public void onResume() {}

    @Override
    public void onPause() {}

    @Override
    public void onDestroy() {}

}
```

### 2.2.9 Parameter ordering in methods

When programming for Android, it is quite common to define methods that take a `Context`. If you are writing a method like this, then the __Context__ must be the __first__ parameter.

The opposite case are __callback__ interfaces that should always be the __last__ parameter.

Examples:

```java
// Context always goes first
public User loadUser(Context context, int userId);

// Callbacks always go last
public void loadUserAsync(Context context, int userId, UserCallback callback);
```

### 2.2.10 String constants, naming, and values

Many elements of the Android SDK such as `SharedPreferences`, `Bundle`, or `Intent` use a key-value pair approach so it's very likely that even for a small app you end up having to write a lot of String constants.

When using one of these components, you __must__ define the keys as a `static final` fields and they should be prefixed as indicated below.

| Element            | Field Name Prefix |
| -----------------  | ----------------- |
| SharedPreferences  | `PREF_`             |
| Bundle             | `BUNDLE_`           |
| Fragment Arguments | `ARG_`         |
| Intent Extra       | `EXTRA_`            |
| Intent Action      | `ACTION_`           |
| Activity result extras | `RESULT_`           |

Example:

```java
// Note the value of the field is the same as the name to avoid duplication issues
static final String PREF_EMAIL = "PREF_EMAIL";
static final String BUNDLE_AGE = "BUNDLE_AGE";
static final String ARG_USER_ID = "ARG_USER_ID";

static final String EXTRA_SURNAME = "EXTRA_SURNAME";
static final String RESULT_SURNAME = "RESULT_SURNAME";

// Intent action keys use full package name as value
static final String ACTION_OPEN_USER = "com.myapp.action.ACTION_OPEN_USER";
```

Constant can be avoided if string key is used only once to access value and only internally in a class:

```java
public class DetailsFragment extends Fragment {

  public static DetailsFragment newInstance(String id) {
    DetailsFragment f = new DetailsFragment();
    Bundle args = new Bundle();
    args.put("id", id);
    f.setArguments(args);
    return f;
  }

  @Override
  public void onCreate(Bundle savedInstanceState) {
    String id = getArguments().getString("id");
    ...
  }

}
```


### 2.2.11 Arguments in Fragments and Activities

When data is passed into an `Activity `or `Fragment` via an `Intent` or a `Bundle`, the keys for the different values __must__ follow the rules described in the section above.

When an `Activity` or `Fragment` expects arguments, it should provide a `public static` method that facilitates the creation of the relevant `Intent` or `Fragment`.

In the case of __Activities__ the method is usually called `getStartIntent()`:

```java
public static Intent getIntent(Context context, User user) {
	Intent intent = new Intent(context, ThisActivity.class);
	intent.putParcelableExtra("user", user);
	return intent;
}
```

Then the getter can be used to retrieve extas:

```java
private User getUserExtra() {
    return getIntent().getParcelableExtra("user");
}
```

For __Fragments__ it is named `newInstance()` and handles the creation of the Fragment with the right arguments:

```java
public static UserFragment newInstance(User user) {
    UserFragment fragment = new UserFragment;
    Bundle args = new Bundle();
    args.putParcelable("user", user);
    fragment.setArguments(args)
    return fragment;
}
```

### 2.2.12 Android View subclass naming

It is very common to instantiate views in Fragment's `onCreateView()` method and declare them as class variables. But the naming of these variable could be tricky in some cases.
To avoid confusion and to group different views by their type a simple rule should be applied: View variable name should be prefixed with its type shortening. For example:

```java
    // tv stands for TextView
    private TextView tvName, tvEmail;
    // iv stands for ImageView
    private ImageView ivLogo;
    // b stands for Button
    private Button bSubmit;
    // w stands for Wrapper
    private LinearLayout wUserData
```

### 2.2.13 Android View declaration

Views should be declared in single line declaration.

__Bad:__
```java
    private TextView tvName;
    private TextView tvEmail;
```
__Good:__
```java
    private TextView tvName, tvEmail;
```


### 2.2.14 Line length limit

Code lines should not exceed __100 characters__. If the line is longer than this limit there are usually two options to reduce its length:

* Extract a local variable or method (preferable).
* Apply line-wrapping to divide a single line into multiple ones.

There are two __exceptions__ where it is possible to have lines longer than 100:

* Lines that are not possible to split, e.g. long URLs in comments.
* `package` and `import` statements.

Line length is not a strick rule, but it's recommended to follow it to enhance readability.



### 2.2.15 RxJava chains styling

Rx chains of operators require line-wrapping. Every operator must go in a new line and the line should be broken before the `.`

```java
public Observable<Location> syncLocations() {
    return mDatabaseHelper.getAllLocations()
            .concatMap(new Func1<Location, Observable<? extends Location>>() {
                @Override
                 public Observable<? extends Location> call(Location location) {
                     return mRetrofitService.getLocation(location.id);
                 }
            })
            .retry(new Func2<Integer, Throwable, Boolean>() {
                 @Override
                 public Boolean call(Integer numRetries, Throwable throwable) {
                     return throwable instanceof RetrofitError;
                 }
            });
}
```

## 2.3 XML style rules

### 2.3.1 Use self closing tags

When an XML element doesn't have any contents, you __must__ use self closing tags.

This is good:

```xml
<TextView
    android:id="@+id/tvProfile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

This is __bad__ :

```xml
<!-- Don\'t do this! -->
<TextView
    android:id="@+id/tv_view_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
</TextView>
```


### 2.3.2 Resources naming

Resource IDs and names are written in __lowercase_underscore__.

#### 2.3.2.2 Styles and Themes

Style names are written in __UpperCamelCase__ and should be prefixed with project name or project name shortening. Also style names should be divided by `.` into logical pieces, for example:
```xml
<style name="Htg.Theme.Light"/>
<style name="Htg.Text.Header1"/>
<style name="Htg.Button.Flat"/>
```

#### 2.3.2.3 Colors

Color names are written in __lowercase_underscore__. Color code should be __UPPERCASE__, for example:
```xml
<color name="grey_light">#F0F0F0</color>
```

Use prefixes to designate color groups for text `text_`, toolbar `toolbar_`, status bar `status_`, tabs `tab_`. Use buttons prefix `btn_` for clickable items and selectors. Before adding new color, check if it doesn't already exist in this color group. Do NOT use colors from a different color group.

For colors with a specified alpha value, append opacity percentage as a two digit number at the end of a color name. E.g.
```xml
<color name="btn_red_67">#AAFD3C30</color>.
```

Determine percentage from hex value [here](http://online.sfsu.edu/chrism/hexval.html).

### 2.3.4 Use design time layout attributes

These are attributes which are used when the layout is rendered in the tool, but have no impact on the runtime.
Use these attributes to display sample data when editing the layout. This makes layout previews much more informative.

To achieve this use __tools__ namespace:

```xml
//Make views visible even if they're not visible initially.
<LinearLayout
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:visibility="gone"
        tools:visibility="visible">

        //Set sample text if non english string resource is used.
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="@string/login_user_name"
            tools:text="Username"/>

        //Set sample text if text field is empty initially.
        <EditText
            android:id="@+id/etUserName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            tools:text="John Dao"/>

    </LinearLayout>
```

### 2.3.5 Use of dimension resources

All components align to an 8dp square baseline grid. Iconography in toolbars align to a 4dp square baseline grid. Dimensions that can be reused must be stored in dimension resources.

#### 2.3.5.1 Use screen margin dimension resources

Use screen margin dimensions for content container that matches screen size. This will provide more appropriate and distinct layout for large screen configurations with minimal effort when separate layout file is not required.

| Dimension | Phone | 7" | 10" |
| --- | --- | --- | --- |
| `screen_margin_horizontal` | `0dp` | `64dp` | `80dp` |
| `screen_margin_vertical` | `0dp` | `40dp` | `64dp`|

Screen margin dimensions can also be __used as padding__ to achieve same visual result when content container is top level container, for example:

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:tools="http://schemas.android.com/tools"
             android:layout_width="match_parent"
             android:layout_height="match_parent"
             android:paddingLeft="@dimen/screen_margin_horizontal"
             android:paddingRight="@dimen/screen_margin_horizontal"
             android:paddingTop="@dimen/screen_margin_vertical"
             android:paddingBottom="@dimen/screen_margin_vertical">
             ...
```

## 2.4 Tests style rules

### 2.4.1 Unit tests

Test classes should match the name of the class the tests are targeting, followed by `Test`. For example, if we create a test class that contains tests for the `DatabaseHelper`, we should name it `DatabaseHelperTest`.

Test methods are annotated with `@Test` and should generally start with the name of the method that is being tested, followed by a precondition and/or expected behaviour.

* Template: `@Test void methodNamePreconditionExpectedBehaviour()`
* Example: `@Test void signInWithEmptyEmailFails()`

Precondition and/or expected behaviour may not always be required if the test is clear enough without them.

Sometimes a class may contain a large amount of methods that at the same time require several tests for each method. In this case, it's recommended to split up the test class into multiple ones. For example, if the `DataManager` contains a lot of methods we may want to divide it into `DataManagerSignInTest`, `DataManagerLoadUsersTest`, etc. Generally you will be able to see which tests belong together because they have common [test fixtures](https://en.wikipedia.org/wiki/Test_fixture).

### 2.4.2 Espresso tests

Every Espresso test class usually targets an Activity, therefore the name should match the name of the targeted Activity followed by `Test`, e.g. `SignInActivityTest`.

When using the Espresso API it is a common practice to place chained methods in new lines.

```java
onView(withId(R.id.view))
        .perform(scrollTo())
        .check(matches(isDisplayed()))
```
