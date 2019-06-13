Android

-   Activity(done)

-   Fragment

-   adapter

-   Lifetime cycles (done)

-   Saving State (done)

-   Intent (done)

-   View

-   ViewModels (done)

-   Resources

-   Layout Classes

-   Master-detail

-   Adapters

-   recyclerview

-   Listview

-   Anko

-   Toast

-   Logger

Kotlin

-   Data classes

-   Extension Methods

-   properties

-   operator overloads

-   rest

-   khttp (- voli)

-   doAsync

-   runOnUiThread

Extras

-   Service (android)

-   difference l[et, apply, with, run, also](https://proandroiddev.com/the-difference-between-kotlins-functions-let-apply-with-run-and-else-ca51a4c696b8)

Activity

a single focused thing, a user can do. ex. read mail, write mail, login etc.

Activity UI

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
