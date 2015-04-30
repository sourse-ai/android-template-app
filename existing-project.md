---
title: Existing Project
layout: default
---


# Configure Access to the Push Video Repository

The android support library and Google Play services need to be added if they are not used already. AppCompat-v7 references support-v4 so may be used in place of support-v4.

Gradle:

    allprojects {
        repositories {
            jcenter()
            maven {
                url "http://107.178.209.67:8081/nexus/content/repositories/releases/"
            }
        }
    }

Maven:

    <repositories>
      <repository>
        <!-- Access to the Incoming release repo is not restricted -->
        <id>incoming-release-repo</id>
        <name>Incoming release repo</name>
        <url>http://107.178.209.67:8081/nexus/content/repositories/releases/</url>
      </repository>
    </repositories>

## Add the library dependencies

Gradle:

    dependencies {
        compile 'com.incoming-media:incoming-push-video-sdk:1.1.2'
        compile 'com.android.support:support-v4:21.0.3'
        compile 'com.google.android.gms:play-services-location:6.5.87'
        compile 'com.google.android.gms:play-services-base:6.5.87'
    }

Maven:

    <dependency>
      <groupId>com.incoming-media</groupId>
      <artifactId>incoming-push-video-sdk</artifactId>
      <version>1.1.2</version>
      <type>aar</type>
     </dependency>

    <dependency>
      <groupId>com.android.support</groupId>
      <artifactId>support-v4</artifactId>
      <version>${android.support.version}</version>
      <type>aar</type>
    </dependency>

    <dependency>
      <groupId>com.google.android.gms</groupId>
      <artifactId>play-services-base</artifactId>
      <type>aar</type>
      <version>${play-services.version}</version>
    </dependency>

    <dependency>
      <groupId>com.google.android.gms</groupId>
      <artifactId>play-services-location</artifactId>
      <type>aar</type>
      <version>${play-services.version}</version>
    </dependency>

# Configure the SDK Initialization Code

The Push Video SDK may be configured in the application launch Activity onCreate(),

    // Optionally activate SDK logging
    LogIncoming.setDevBuild(true);
    PushVideo.initialize(getApplicationContext(), ”API ENDPOINT”, “PROJECT KEY”);
    
or in the main Application class onCreate():

    // Optionally activate SDK logging
    LogIncoming.setDevBuild(true);
    PushVideo.initialize(this, ”API ENDPOINT”, “PROJECT KEY”);

# Update the Manifest


Add the permissions, video player activities and broadcast receiver configuration to the application section of the Android manifest.

##Permissions

    <!-- Begin PVN permission configuration -->
    <!-- Permissions required for GCM -->
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    <uses-permission android:name="android.permission.WAKE_LOCK"/>
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE"/>
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
    <!-- Replace com.example.incomingpvntemplate with application package name -->
    <permission android:name="com.example.incomingpvntemplate.permission.C2D_MESSAGE" android:protectionLevel="signature"/>

    <!-- Replace com.incoming.example.incomingpvntemplate with application package name -->
    <uses-permission android:name="com.incoming.example.incomingpvntemplate.permission.C2D_MESSAGE"/>

    <!-- Required to store downloaded videos -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
    <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

    <!-- Check Wi-fi state for download -->
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="com.google.android.gms.permission.ACTIVITY_RECOGNITION"/>

    <!-- End PVN permission configuration -->

##Broadcast Receiver Configuration And Video Player Activity Coinfiguration

     <application android:allowBackup="true" android:icon="@drawable/ic_launcher" android:label="@string/app_name" android:theme="@style/AppTheme">

        <!-- Begin PVN configuration -->

        <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version"/>

        <activity android:configChanges="orientation" android:exported="true" android:name="com.incoming.au.uiframework.VideoViewPlayer" android:screenOrientation="sensorLandscape">
        </activity>

        <receiver android:name="com.incoming.au.sdk.notification.NotificationBroadcastReceiver">
            <intent-filter>
                <action android:name="com.incoming.notif.check"/>
                <action android:name="com.incoming.feed.check" />
                <action android:name="com.incoming.download.trigger"/>
                <action android:name="com.android.vending.INSTALL_REFERRER"/>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
                <action android:name="com.google.android.c2dm.intent.RECEIVE"/>

                <!-- Replace com.incoming.example.incomingpvntemplate with application package name -->
                <category android:name="com.incoming.example.incomingpvntemplate"/>
            </intent-filter>
        </receiver>

        <service android:name="com.incoming.au.sdk.PVNIntentService"/>

        <!-- End PVN configuration -->


