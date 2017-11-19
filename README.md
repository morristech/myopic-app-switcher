# Myopic
 [ ![Download](https://api.bintray.com/packages/mguellsegarra/myopic/cat.mguellsegarra%3Amyopic/images/download.svg) ](https://bintray.com/mguellsegarra/myopic/cat.mguellsegarra%3Amyopic/_latestVersion)  
  
🕵️  Replace Android app switcher thumbnail with blur or with your customized image overlay  
  
![myopic](https://github.com/mguellsegarra/myopic-app-switcher/raw/master/myopic_gif.gif)

## Why this library

After some research, I only found two ways to achieve the modification of the thumbnail that an app has in Android App Switcher:

- Overriding Activity `onCreateThumbnail` method. **Not working anymore** [Source 1](https://stackoverflow.com/questions/11848132/is-there-a-way-to-change-the-thumbnail-of-an-app-in-the-android-task-switcher-l)

- Setting `WindowManager.LayoutParams.FLAG_SECURE` flag in your activity. [Source 1](https://stackoverflow.com/questions/9822076/how-do-i-prevent-android-taking-a-screenshot-when-my-app-goes-to-the-background) [Source 2](https://stackoverflow.com/questions/22435952/android-thumbnail-when-it-goes-to-background). This is the official and the best way to secure your application. You'll prevent Android to take screenshots of your app, so you won't be able to see the thumbnail of your app in the app switcher. Instead you'll see a black window. However, applying this flag, you wan't be able to customize your app's thumbnail while in app switcher, and most important, you won't be able to take screenshots while using your app, and sometimes this is something that you don't want to loose.

## Code

Myopic internally handles `onPause()` and `onResume()` methods in the activity lifecyle in order to insert an overlaying view when the app goes to the background.

This overlaying view can be an automatic generated blurred image of your current screen or an image that you'll have to specify by passing a drawable id.

The blur is accomplished thanks to [BlurKit](https://github.com/wonderkiln/blurkit-android) library.

## Adding the library

Add JCenter in gradle project file:

```groovy
    repositories {
        jcenter()
    }
```

Add this line to your module project:

```groovy
compile 'cat.mguellsegarra:myopic:0.14'
```

## Usage

Initialize Myopic in your sample application:

```java
public class MyopicSampleApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        // Default blur mode
        Myopic.init(this);

        // Specific blur radius, by default is 25
        // Myopic.initBlurMode(this, 5);

        // Specific drawable PNG
        // Myopic.initOverlayMode(this, R.drawable.app_switcher_background);
    }
}
```

Extend your activities or your own base activity from `MyopicActivity`:

```java
public class MainActivity extends MyopicActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

Et voilà!

## Contributing 

Feel free to open any pull request if you'd like to add new features or improve Myopic :)

## License 

The MIT License (MIT)

Copyright (c) 2017 Marc Güell Segarra
