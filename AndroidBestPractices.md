Android Development Best Practices

Always maintain the code quality.
Detecting and fixing memory leaks in android.
Always include unit tests.
Always include functional UI tests. Functional tests check the functionality of your app from the user’s point of view. 
Use Proguard in your release version.
Use debugging tools. 
Use strings.xml.
Create separate layouts for UI elements that will be re-used.
Place launcher icons in mipmap-folders. 
  When building separate APKs for different densities, drawable folders for other densities get stripped. 
  This will make the icons appear blurry on devices that use launcher icons of higher density. 
  Since mipmap folders do not get stripped, it’s always best to use them for including the launcher icons.
Use shapes and selectors instead of images as much as possible.
Avoid deep levels in layouts(Constraint Layout)
Use a HTTP library like Fast Android Networking, Volley, or Retrofit, depending on your use case.
Use the Parcelable class instead of Serializable when passing data in Intents or Bundles.
  Serialization of an object that implements the Parcelable interface is much faster than using Java’s default serialization.
  A class that implements the Serializable interface is marked as serializable,
  and Java serializes it using reflection (which makes it slow). 
  When using the Parcelable interface, the whole object doesn’t get serialized automatically. 
  Rather, you can selectively add data from the object to a Parcel using which the object is later deserialized.
Perform file operations off the UI thread. 
  File operations should always be performed on a worker thread, typically by using an AsyncTask/Loader.
  They take time, and if done on the UI thread can make the interface feel sluggish.
  In situations where they block the UI thread for 5 seconds, an ANR warning will be triggered and shown to the user.
Understand Bitmaps.
  (https://developer.android.com/topic/performance/graphics/)
  Learn How The Android Image Loading Library Glide and Fresco Works?
Understand Context. 
Use styles to avoid duplicate attributes in layout XMLs.
Use RxJava
Learn the activity lifecycle.
In general, use proven libraries instead of your own solutions.
Learn to create a pub/sub library.
  https://www.toptal.com/ruby-on-rails/the-publish-subscribe-pattern-on-rails
Keep testing on various OS versions.
