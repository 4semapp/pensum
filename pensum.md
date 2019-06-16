# Android

- [Android](#android)
  - [Activity](#activity)
    - [Lifetime cycle](#lifetime-cycle)
    - [onCreate](#oncreate)
    - [onStart](#onstart)
    - [onResume](#onresume)
    - [onStop](#onstop)
    - [onDestroy](#ondestroy)
  - [Fragment](#fragment)
  - [Adapter](#adapter)
  - [Saving State](#saving-state)
  - [Intent](#intent)
  - [View](#view)
  - [Resources](#resources)
  - [Layout Classes](#layout-classes)
  - [Master-Detail](#master-detail)
  - [Anko](#anko)
    - [Toast](#toast)
    - [Logger](#logger)
- [Kotlin](#kotlin)
  - [let, apply, wiht, run, also](#let-apply-wiht-run-also)
  - [Data Classes](#data-classes)
  - [Extension Methods](#extension-methods)
  - [Properties](#properties)
  - [Operator overloads](#operator-overloads)
  - [Rest](#rest)
  - [kttp](#kttp)
  - [doAsync](#doasync)


## Activity

- https://developer.android.com/reference/kotlin/android/app/Activity.html
- https://developer.android.com/guide/components/activities/activity-lifecycle

> An activity is a single, focused thing that the user can do. Almost all activities interact with the user, so the Activity class takes care of creating a window for you in which you can place your UI with `setContentView`. 

### Lifetime cycle

> Activities in the system are managed as activity stacks. When a new activity is started, it is usually placed on the top of the current stack and becomes the running activity -- the previous activity always remains below it in the stack, and will not come to the foreground again until the new activity exits. There can be one or multiple activity stacks visible on screen.
> 
> An activity has essentially four states:
> 
> If an activity is in the foreground of the screen (at the highest position of the topmost stack), it is active or running. This is usually the activity that the user is currently interacting with.
>
> If an activity has lost focus but is still presented to the user, it is visible. It is possible if a new non-full-sized or transparent activity has focus on top of your activity, another activity has higher position in multi-window mode, or the activity itself is not focusable in current windowing mode. Such activity is completely alive (it maintains all state and member information and remains attached to the window manager).
>
> If an activity is completely obscured by another activity, it is stopped or hidden. It still retains all state and member information, however, it is no longer visible to the user so its window is hidden and it will often be killed by the system when memory is needed elsewhere.
The system can drop the activity from memory by either asking it to finish, or simply killing its process, making it destroyed. When it is displayed again to the user, it must be completely restarted and restored to its previous state.

![](activity_lifecycle.png)

There are three key loops you may be interested in monitoring within your activity:

> * The entire lifetime of an activity happens between the first call to onCreate(Bundle) through to a single final call to onDestroy(). An activity will do all setup of "global" state in onCreate(), and release all remaining resources in onDestroy(). For example, if it has a thread running in the background to download data from the network, it may create that thread in onCreate() and then stop the thread in onDestroy().
> * The visible lifetime of an activity happens between a call to onStart() until a corresponding call to onStop(). During this time the user can see the activity on-screen, though it may not be in the foreground and interacting with the user. Between these two methods you can maintain resources that are needed to show the activity to the user. For example, you can register a BroadcastReceiver in onStart() to monitor for changes that impact your UI, and unregister it in onStop() when the user no longer sees what you are displaying. The onStart() and onStop() methods can be called multiple times, as the activity becomes visible and hidden to the user.
> * The foreground lifetime of an activity happens between a call to onResume() until a corresponding call to onPause(). During this time the activity is in visible, active and interacting with the user. An activity can frequently go between the resumed and paused states -- for example when the device goes to sleep, when an activity result is delivered, when a new intent is delivered -- so the code in these methods should be fairly lightweight.

```java
 public class Activity extends ApplicationContext {

     protected void onCreate(Bundle savedInstanceState);
     protected void onStart();
     protected void onRestart();
     protected void onResume();
     protected void onPause();
     protected void onStop();
     protected void onDestroy();
 }
 
```

### onCreate

https://developer.android.com/guide/components/activities/activity-lifecycle#oncreate

> You must implement this callback, which fires when the system first creates the activity. On activity creation, the activity enters the Created state. 
> 
> In the ``onCreate()`` method, you perform basic application startup logic that should happen only once for the entire life of the activity. For example, your implementation of onCreate() might bind data to lists, associate the activity with a ViewModel, and instantiate some class-scope variables. 
> 
> This method receives the parameter ``savedInstanceState``, which is a Bundle object containing the activity's previously saved state. If the activity has never existed before, the value of the Bundle object is ``null``.

### onStart

- https://developer.android.com/guide/components/activities/activity-lifecycle#onstart

> When the activity enters the Started state, the system invokes this callback. The ``onStart()`` call makes the activity visible to the user, as the app prepares for the activity to enter the foreground and become interactive. For example, this method is where the app initializes the code that maintains the UI.
>
> The ``onStart()`` method completes very quickly and, as with the Created state, the activity does not stay resident in the Started state. Once this callback finishes, the activity enters the Resumed state, and the system invokes the ``onResume()`` method.

### onResume

> When the activity enters the Resumed state, it comes to the foreground, and then the system invokes the `onResume()` callback. This is the state in which the app interacts with the user. The app stays in this state until something happens to take focus away from the app. Such an event might be, for instance, receiving a phone call, the user’s navigating to another activity, or the device screen’s turning off.
> 
> When an interruptive event occurs, the activity enters the Paused state, and the system invokes the ``onPause()`` callback.
>
> If the activity returns to the Resumed state from the Paused state, the system once again calls ``onResume()`` method. For this reason, you should implement ``onResume()`` to initialize components that you release during ``onPause()``, and perform any other initializations that must occur each time the activity enters the Resumed state.

### onStop

> When your activity is no longer visible to the user, it has entered the Stopped state, and the system invokes the ``onStop()`` callback. This may occur, for example, when a newly launched activity covers the entire screen. The system may also call ``onStop()`` when the activity has finished running, and is about to be terminated.

### onDestroy

> ``onDestroy()`` is called before the activity is destroyed. The system invokes this callback either because:
>
> - the activity is finishing (due to the user completely dismissing the activity or due to finish() being called on the activity), or
the system is temporarily destroying the activity due to a configuration change (such as device rotation or multi-window mode)
>
> Instead of putting logic in your Activity to determine why it is being destroyed you should use a ViewModel object to contain the relevant view data for your Activity. If the Activity is going to be recreated due to a configuration change the ViewModel does not have to do anything since it will be preserved and given to the next Activity instance. If the Activity is not going to be recreated then the ViewModel will have the onCleared() method called where it can clean up any data it needs to before being destroyed.
>
> You can distinguish between these two scenarios with the isFinishing() method.

> If the activity is finishing, onDestroy() is the final lifecycle callback the activity receives. If onDestroy() is called as the result of a configuration change, the system immediately creates a new activity instance and then calls onCreate() on that new instance in the new configuration.

## Fragment
> A Fragment represents a behavior or a portion of user interface in a FragmentActivity. You can combine multiple fragments in a single activity to build a multi-pane UI and reuse a fragment in multiple activities. You can think of a fragment as a modular section of an activity, which has its own lifecycle, receives its own input events, and which you can add or remove while the activity is running
>
> A fragment must always be hosted in an activity and the fragment's lifecycle is directly affected by the host activity's lifecycle. For example, when the activity is paused, so are all fragments in it, and when the activity is destroyed, so are all fragments. However, while an activity is running, you can manipulate each fragment independently, such as add or remove them. 
> 
> When you perform such a fragment transaction, you can also add it to a back stack that's managed by the activity—each back stack entry in the activity is a record of the fragment transaction that occurred. The back stack allows the user to reverse a fragment transaction, by pressing the Back button.

## Adapter

- https://stackoverflow.com/questions/3674951/whats-the-role-of-adapters-in-android
- https://abhiandroid.com/ui/adapter

> An Adapter object acts as a bridge between an AdapterView and the underlying data for that view. The Adapter provides access to the data items. The Adapter is also responsible for making a View for each item in the data set.
>
> It holds the data and send the data to an Adapter view then view can takes the data from the adapter view and shows the data on different views like as ListView, GridView, Spinner etc.
>
> Whenever we have a list of single items which is backed by an Array, we can use ArrayAdapter. For instance, list of phone contacts, countries or names.

## Saving State

- https://www.google.com/search?q=saving+state&rlz=1C1GGRV_enDK754DK754&oq=saving+state&aqs=chrome..69i57j0l5.5175639j0j7&sourceid=chrome&ie=UTF-8

> Preserving and restoring an activity’s UI state in a timely fashion across system-initiated activity or application destruction is a crucial part of the user experience. In these cases the user expects the UI state to remain the same, but the system destroys the activity and any state stored in it.
> 
> To bridge the gap between user expectation and system behavior, use a combination of `ViewModel` objects, the `onSaveInstanceState()` method, and/or local storage to persist the UI state across such application and activity instance transitions. Deciding how to combine these options depends on the complexity of your UI data, use cases for your app, and consideration of speed of retrieval versus memory usage.
> 
> Regardless of which approach you take, you should ensure your app meets users expectations with respect to to their UI state, and provides a smooth, snappy UI (avoids lag time in loading data into the UI, especially after frequently occurring configuration changes, like rotation). In most cases you should use both ViewModel and `onSaveInstanceState()`.

## Intent

- https://developer.android.com/reference/android/content/Intent
- https://developer.android.com/guide/components/intents-common

> An intent is an abstract description of an operation to be performed.
>
> An Intent provides a facility for performing late runtime binding between the code in different applications. Its most significant use is in the launching of activities, where it can be thought of as the glue between activities. It is basically a passive data structure holding an abstract description of an action to be performed.


> Using intents, we can for example open a webpage, open the camera app, and send sms' and emails.
>
> An intent allows you to start an activity in another app by describing a simple action you'd like to perform (such as "view a map" or "take a picture") in an Intent object. This type of intent is called an implicit intent because it does not specify the app component to start, but instead specifies an action and provides some data with which to perform the action.
>
> When you call `startActivity()` or `startActivityForResult()` and pass it an implicit intent, the system resolves the intent to an app that can handle the intent and starts its corresponding Activity. If there's more than one app that can handle the intent, the system presents the user with a dialog to pick which app to use.

## View

- https://developer.android.com/reference/android/view/View

> This class represents the basic building block for user interface components. A View occupies a rectangular area on the screen and is responsible for drawing and event handling. View is the base class for widgets, which are used to create interactive UI components (buttons, text fields, etc.). The ViewGroup subclass is the base class for layouts, which are invisible containers that hold other Views (or other ViewGroups) and define their layout properties.
>
> All of the views in a window are arranged in a single tree. You can add views either from code or by specifying a tree of views in one or more XML layout files. There are many specialized subclasses of views that act as controls or are capable of displaying text, images, or other content.
>
> Once you have created a tree of views, there are typically a few types of common operations you may wish to perform:
>
> * Set properties: for example setting the text of a TextView. The available properties and the methods that set them will vary among the different subclasses of views. Note that properties that are known at build time can be set in the XML layout files.
> * Set focus: The framework will handle moving focus in response to user input. To force focus to a specific view, call requestFocus().
> * Set up listeners: Views allow clients to set listeners that will be notified when something interesting happens to the view. For example, all views will let you set a listener to be notified when the view gains or loses focus. You can register such a listener using setOnFocusChangeListener(android.view.View.OnFocusChangeListener). Other view subclasses offer more specialized listeners. For example, a Button exposes a listener to notify clients when the button is clicked.
> * Set visibility: You can hide or show views using `setVisibility(int)`.

## Resources

- https://developer.android.com/reference/android/content/res/Resources
- https://developer.android.com/guide/topics/resources/providing-resources
- https://developer.android.com/guide/topics/resources/available-resources

> The Android resource system keeps track of all non-code assets associated with an application. You can use this class to access your application's resources. You can generally acquire the Resources instance associated with your application with getResources().
>
> Using application resources makes it easy to update various characteristics of your application without modifying code, and—by providing sets of alternative resources—enables you to optimize your application for a variety of device configurations (such as for different languages and screen sizes). This is an important aspect of developing Android applications that are compatible on different types of devices.

> You should always externalize app resources such as images and strings from your code, so that you can maintain them independently. You should also provide alternative resources for specific device configurations, by grouping them in specially-named resource directories. At runtime, Android uses the appropriate resource based on the current configuration. For example, you might want to provide a different UI layout depending on the screen size or different strings depending on the language setting.

## Layout Classes

> Much as with Widgets, there is no Layout base class, therefore it could be loosely defined as any class which extends ViewGroup and provides the ability to define the positioning of child Views within it. Usually, only ViewGroup subclasses which are appended with the word "Layout" (as in LinearLayout, RelativeLayout) are referred to as "layouts", other classes extending ViewGroup are usually just referred to as "view containers".

## Master-Detail

- https://en.wikipedia.org/wiki/Master%E2%80%93detail_interface

> In computer user interface design, a master–detail interface displays a master list and the details for the currently selected item. The original motivation for master detail was that such a view table on old 1980s 80-character-wide displays could only comfortably show about four columns on the screen at once, while a typical data entity will have some twenty fields. The solution is that the detail shows all twenty fields and the master shows only the commonly recognised three to five that will fit on the screen in one row without scrolling.

> A master area can be a form, list or tree of items, and a detail area can be a form, list or tree of items typically placed either below or next to the master area.[1] Selecting an item from the master list causes the details of that item to be populated in the detail area.[2][3]

## Anko

> Anko is a Kotlin library which makes Android application development faster and easier. It makes your code clean and easy to read, and lets you forget about rough edges of the Android SDK for Java.

- Anko Commons
  - Intents;
  - Dialogs and toasts;
  - Logging;
  - Resources and dimensions.
- Anko Layout DSL
  - Anko Layouts is a DSL for writing dynamic Android layouts.

### Toast

> A toast provides simple feedback about an operation in a small popup. It only fills the amount of space required for the message and the current activity remains visible and interactive. Toasts automatically disappear after a timeout.

### Logger



# Kotlin

## let, apply, wiht, run, also

https://proandroiddev.com/the-difference-between-kotlins-functions-let-apply-with-run-and-else-ca51a4c696b8

## Data Classes
## Extension Methods
## Properties
## Operator overloads
## Rest
## kttp
## doAsync