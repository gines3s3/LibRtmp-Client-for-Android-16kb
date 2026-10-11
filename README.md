# LibRtmp-Client-for-Android-16kb

[![](https://jitpack.io/v/gines3s3/LibRtmp-Client-for-Android-16kb.svg)](https://jitpack.io/#gines3s3/LibRtmp-Client-for-Android-16kb)

Recompiled and configured with Android NDK r28 and flexible page size flags (`-DANDROID_SUPPORT_FLEXIBLE_PAGE_SIZES=ON`) to support [Android 16 KB Page Sizes](https://developer.android.com/guide/practices/page-sizes) and pass Google Play Store requirements.

### Integration with `androidx.media3:media3-datasource-rtmp`

AndroidX Media3 [RTMP DataSource module](https://github.com/androidx/media/tree/release/libraries/datasource_rtmp) depends on `io.antmedia:rtmp-client`, which uses an old NDK binary that fails Google Play 16 KB page size static checks.

You can resolve the Google Play Console 16 KB warning by excluding `rtmp-client` and adding this library:

**Kotlin DSL (`build.gradle.kts`):**
```kts
repositories {
    mavenCentral()
    maven { url = uri("https://jitpack.io") }
}

dependencies {
    implementation("androidx.media3:media3-datasource-rtmp:1.5.1") {
        exclude("io.antmedia", "rtmp-client")
    }
    implementation("com.github.gines3s3:LibRtmp-Client-for-Android-16kb:v3.2.1-16kb")
}
```

**Groovy (`build.gradle`):**
```groovy
repositories {
    mavenCentral()
    maven { url 'https://jitpack.io' }
}

dependencies {
    implementation('androidx.media3:media3-datasource-rtmp:1.5.1') {
        exclude group: 'io.antmedia', module: 'rtmp-client'
    }
    implementation 'com.github.gines3s3:LibRtmp-Client-for-Android-16kb:v3.2.1-16kb'
}
```

---

# Librtmp Client for Android
It is probably the smallest (~60KB) RTMP client for Android. It calls librtmp functions over JNI interface.
With all CPU architectures (arm, arm7, arm8, x86, x86-64) its total size is around 300KB.

It compiles the librtmp library without SSL.

##### To read streams, you can call below functions of **RtmpClient** from Java #####

For live streams add **" live=1"** at the end of the URL when calling the **_open_** function:

* *public native int open(String url, boolean isPublishMode);*
* *public native int read(byte[] data, int offset, int size);*
* *public native int write(byte[] data);*
* *public native int seek(int seekTime);*
* *public native int pause(int pause);*
* *public native int close();*
* *public native int isConnected();*: Call this function to query the connection status. Returns 1 if connected, returns 0 if not connected.

Don't forget to call the **_close_** function after you are done to avoid memory leaks.


##### To publish streams, you can call below functions of **RtmpMuxer** from Java #####
* *public native int open(String url, int width, int height);* : Call this function with the URL you plan to publish. Width and height are optional video parameters.
* *public native int writeVideo(byte[] data, int offset, int length, int timestamp);*: Write h264 NAL units with this function.
* *public native int writeAudio(byte[] data, int offset, int length, int timestamp);*: Write AAC frames with this function.
* *public native int close();*: Call this function to close publishing.
* *public native int isConnected();*: Call this function to query connection status.

To save FLV files locally as well:
* *public native void file_open(String filename);*: Call with full file path.
* *public native void write_flv_header(boolean is_have_audio, boolean is_have_video);*: After opening file, call this function.
* *public native void file_close();*: Call to close the local file.
