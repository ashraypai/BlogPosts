# How to setup your AR app to be launched on the Google Play Store

In order to publish an app on the google play store, there are certain standards that are to be followed. This blog covers all the steps that you will need to successfully publish your app. The main focus here is setting up unity in such a way that, after building your apk and uploading it on google play console, you will not receive any compliance errors. This blog does not tell you the steps that are to be followed inside of google play console,however you can reffer to this [documentation](https://support.google.com/googleplay/android-developer/answer/9859152) from google which will guide you through the setps that are to be followed inside of google play console.

## 1. Prerequisites 
Make sure to have the following software downloaded and installed.
1) [Unity hub](https://unity3d.com/get-unity/download) and [Unity 2019.4.15f1 and above](https://unity3d.com/get-unity/download/archive). (In "Add Modules" make sure to have "Android build support" installed) 
2) [Android studio 4.1.3](https://developer.android.com/studio).


## 2. Quick guide for downloading and assigning Dev Kits:
Below are some developments kits that are required to successfully build the app:

1) Download links:
    - NDK: https://dl.google.com/android/repository/android-ndk-r19-windows-x86_64.zip
    - Gradle: https://gradle.org/releases/

2) Once it's downloaded, extract the zip files and place them in your desired location.

3) Using Android Studio download the SDK for version 8.0 and above.

4) In Unity go to Edit -> Preferences -> External Tools and enter the path of the respective Dev Kits.
    - Generally, for the JDK the path is going to be: D:/"SOME_LOCATION"/2019.4.15f1/Editor/Data/PlaybackEngines/AndroidPlayer/OpenJDK 
    - For NDK and Gradle the path is the location where you downloaded and extracted the files.
    - For SDK the location will be mentioned in the Android Studio. 

    ![](Img_and_Vid/1.png) 

**Helpful to know**
- NDK (The Native Development Kit) is a set of tools that allows you to use C and C++ code with Android, and provides platform libraries you can use to manage native activities and access physical device components, such as sensors and touch input etc.
- SDK (A software development kit) is a collection of software development tools in one installable package. They facilitate the creation of applications by having a compiler, debugger and perhaps a software framework.
- JDK (Java Development Kit) is a software development environment that offers a collection of tools and libraries necessary for developing Java applications.
- Gradle is a build automation tool for multi-language software development. It controls the development process in the tasks of compilation and packaging to testing, deployment, and publishing. 

#
## 3. Setting up Unity's player settings and downloading AR packages.

1)	Click on File -> Build Settings, this will open a Build settings window. Select the option called Android and then click on switch platform. Now wait for it to load.

    ![](Img_and_Vid/Step1.gif)

2) Select Player Settings and make the following changes:
    -  Change the default company name to your company name.
    -  Select "Other Settings???. 

        a) Remove Vulcan from Graphics API.

        b) Uncheck Multithreaded Rendering.

        c) Change Minimum API level to Android 8.0 and above 

        d) Change Scripting Backend to IL2CPP, why ? To view ARM64 architecture.

        e) Change Api Compatibility Level to .NET 4.x

        f) Check the box for ARM64 under Target Architecture.

    ![](Img_and_Vid/Step2.gif)

    - Select "Publishing Settings???.

        a) Click on Keystore Manager, select Keystore... and click on Create a new key.

        b) Select the location where you want to store the key and hit enter.

        C) Enter the password for the keystore.

        d) Enter the name of your key and password for the key and click on add key.

        e) Confirm to set the created key to the project by clicking on "Yes".

        f) [Note: this step is only if you are using unity version 2019.x, this is not required for unity version 2020] Check the box for Custom Main Gradle Template and Custom Launcher Gradle Template.

    ![](Img_and_Vid/Step3.gif)

3) Click on Windows -> Package manager and install ARFoundation version 4.1.3, ARCore XR Plugin version 4.1.3.

    ![](Img_and_Vid/Step4.gif)


4) Click on File -> Build Settings -> Player Settings -> XR Plug-in Management and check the box for ARCore.

    ![](Img_and_Vid/Step5.gif)

5) In the Hierarchy :
    - Select the Main Camera and delete it.
    - Right click on the Hierarchy and select XR -> AR Session Origin.
    - Select the AR Camera from the Hierarchy and change the tag as "Main Camera".
    - Right click on the Hierarchy again and select XR -> AR Session.

    ![](Img_and_Vid/Step6.gif)

6) [Skip this step if you are using Unity version 2020]Editing Gradle files: 
    - Open the Gradle files mainTemplate.gradle and launcherTemplate.gradle which can be found in your project folder : Assets/Plugins/Android/
    - Delete the following lines from BOTH the files : // GENERATED BY UNITY. REMOVE THIS COMMENT TO PREVENT OVERWRITING WHEN EXPORTING AGAIN
    - Add the following code at the TOP to BOTH the files :

        buildscript 
        
        {

            repositories 

            {
                google()
                jcenter()
            }
    
            dependencies 
            {
                // Must be Android Gradle Plugin 3.6.0 or later. For a list of
                // compatible Gradle versions refer to:
                // https://developer.android.com/studio/releases/gradle-plugin
                classpath 'com.android.tools.build:gradle:3.6.0'
            }
        }

        allprojects 

        {

            repositories 
            
            {
                google()
                jcenter()
                flatDir 
                    {
                        dirs 'libs'
                    }
            }
        }

#

**That's it! Now the project is ready for development and APK can be easily built and uploaded on Play Store.
**


#

## 4. Testing

1) In the Hierarchy right click, 3D Object -> Cube 

2) Reduce the scale of cube to (0.5, 0.5, 0.5)

3) Drag it in Z-direction till it is seen in the Game view.

4) [ Make sure your phone is connected and Debug mode is enabled and allowed on your phone] Click on File -> Build Settings -> Add Open Scene -> Build and Run.

5) Give a name to your APK and save it.

6) Wait for the build to be completed and the app to launch in your phone.

![](Img_and_Vid/Step7.gif)

7) After launching if you are able to see the cube and it is tracked when you move the phone, this would mean that all the setting are prefect and you should not have any problem.

![](Img_and_Vid/Step8.gif)

**Personal suggestion**

Once you have the project tested and ready, save and exit it. 
Navigate to the location of this project and rename it as a Template.
Create a git hub repository and upload/push this project there.

**PRO TIP** : While pushing unity projects to git hub you will need only the following folders: Assets , Packages, ProjectSettings and UserSettings. Rest of them can be deleted, they are generated by unity itself the next time you launch.

Next time if you want to create a new AR Project, just make another copy/ branch of the template and start working directly ,you will not have to repeat this entire process of downloading all the packages and build settings and project setting all over again!!

## Summary

To summarize 
- Download unity and android studio, along with the development kits i.e. SDK, NDK, JDK and gradle. 
- Change the build settings in accordance with google play store guidelines . It is important to select scripting Backend to IL2CPP, changing Api Compatibility Level to .NET 4.x and check the box for ARM64 under Target Architecture.
- Download the ARFoundation package and ARCore XR plugin
- Create a testing scene with 3D object, AR Session Origin and AR Session.
- Test the project by building and running the apk.

Congratulations! You have successfully set up unity to create amazing apps/games.  
If you are interested in creating your own AR and VR apps, you can learn more about it on https://dineshpunni.com/immersive-insiders and if you have any questions don't hesitate to reach out!

-Ashray H Pai

Teleportation in real world is considered as a science fiction, but I would say otherwise. Even though you cannot physically teleport from one place to another, you can do it virtually and this is why I love immersive technologies.
Various experiences like playing sports, travelling, flying a plane, etc cannot be physically experienced by all and I always had the passion to bridge this gap using the latest technologies.

Find me on:

[Discord Channel](https://discord.gg/rWxZvxEu)

[GitHub](https://github.com/ashraypai)

[LinkedIn](https://www.linkedin.com/in/ashray-pai-06a53513a)

[YouTube](https://youtube.com/playlist?list=PLlzEaSubsR5lKOaclT2E4LSfBqLFmkSIs)

