# Android

- [Android](#android)
  - [Activity](#activity)
    - [Lifetime cycles](#lifetime-cycles)
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
  - [ViewModels](#viewmodels)
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

### Lifetime cycles

// TODO

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



## Intent

> An intent is an abstract description of an operation to be performed.
>
> An Intent provides a facility for performing late runtime binding between the code in different applications. Its most significant use is in the launching of activities, where it can be thought of as the glue between activities. It is basically a passive data structure holding an abstract description of an action to be performed.

## View
## ViewModels
## Resources
## Layout Classes
## Master-Detail
## Anko
### Toast
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

View

-   Basic Building Block of UI

ViewGroup

-   contains other views

Layout Classes

-   Handle View positioning behavior

-   Invisible Viewgroup

-   provide positioning flexibility

1.  FrameLayout

-   blocked-out area

-   has one direct child (generally)

3.  Scroll Layout

-   scrollable area

5.  LinearLayout

-   horizontal/vertical arrangement

-   weighted distribution ← lookup

7.  RelativeLayout

-   relative positioning

-   relative to each other or parent

startActivity(intent)

-   Starts an activity with the given intent

onActivityResult(requestCode, resultCode, data)

-   A

-   callback

-   Starts activity B with the an intent(activity A) and a requestCode

-   calls onActivityResult(..) after B uses "finish()"

-   B

-   setResult(resultCode, intent) ← must be called

-   finish() ← called after setResult(..)

Context

View → ViewGroup → Layout  (special invisible layout group)

OnActivityResult → Activity B → setResult(..)/finish() --> Activity A calls OnActiv...

Lifetime Cycles

onCreate

-   called if creating or recreating an old activity

onStart

-   Called after activity is attached on inflated

-   can set event listeners here

onPause

-   activity remains in memory unless space is needed

onDelete

-   deletes activity from memory

-   finish() kills the activity completely

Context

onCreate → onStart → onResume → application runs → onPause → onStop → onDelete

-   onStop/onPause → call stopped Activity (kills application process if memory needed) → onCreate

-   onRestart → onStart

Viewmodel vs Saved Instance state

![](https://lh6.googleusercontent.com/pJIjWpIu8Unjf4Lk0MXp9NKXEXCHD_bkZgFwCEXcNuBaB1l1eujynzB8YroE16upiCbLaYS_SEZJgGVhFthjrsp8QPojyezBdmzTSMGknoYuUzzEfyetSfOe8HxkrgOchxgH2Gcb)

-   Local persistence: Stores all data you don't want to lose if you open and close the activity.

-   Example: A collection of song objects, which could include audio files and metadata.

-   [ViewModel](https://developer.android.com/reference/androidx/lifecycle/ViewModel.html): Stores in memory all the data needed to display the associated UI Controller.

-   Example: The song objects of the most recent search and the most recent search query.

SavedInstanceState

-   used mainly for keeping state of inputs in screen rotating

-   Cannot be used with finish() as finish kills the activity

-   used for storing smaller amounts of data

viewmodel

-   used for storing bigger amounts of data ex. bitmaps or list of users
