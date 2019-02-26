# Google Maps Nearby Scanner : making the project into a library.

# SOMETHING NEW ADDED : OPEN SAMPLE IMPORT AND YOU WILL UNDERSTAND

Conversion of this existing android project into a library which can be imported into another application or project.

The library build has the gradle modified library, which is basically a library project and not an application, the original folder has the original codes => application which has not been modified and AAR file has the library file which has to be imported :+1: .

## Getting Started

What is a library and a package in terms of android ?

They are more or less the same thing. A library is usually a reusable chunk of code that you may want to include in other programs.
A package often is a library that has been prepared in some way to be installed using a package manager for example rubygems or npm.

Some programming languages refer to techniques to namespace code within a program as packages, for example Java in others like Go the language's namespacing technique is actually the same mechanism used to define libraries.

* A Library is always some chunk of code that can be reused.
* A package is sometimes a mechanism to distribute libraries.
* Some programming languages refer to namespaces as packages, others refer to this as modules (a module).
* Some programming languages use their namespacing technique to provide a mechanism to redistribute code (e.g. Go).
* So in general the lines between what a package and a library is are quite blurred, and the exact definition of each word probably depends on the programming language you are using.

An Android library is structurally the same as an Android app module. It can include everything needed to build an app, including source code, resource files, and an Android manifest. However, instead of compiling into an APK that runs on a device, an Android library compiles into an Android Archive (AAR) file that you can use as a dependency for an Android app module. Unlike JAR files, AAR files can contain Android resources and a manifest file, which allows you to bundle in shared resources like layouts and drawables in addition to Java classes and methods.
A library module is useful in the following situations:

* When you're building multiple apps that use some of the same components, such as activities, services, or UI layouts.
* When you're building an app that exists in multiple APK variations, such as a free and paid version and you need the same core components in both.

Please go through the following links for more detail view on android library : [ANDROID LIBRARY TUTORIALS](https://developer.android.com/studio/projects/android-library.html).

## Convert an existing app module to a library module

If you have an existing app module with all the code you want to reuse, you can turn it into a library module as follows:

1. Open the module-level build.gradle file.
2. Delete the line for the applicationId. Only an Android app module can define this.
3. At the top of the file, you should see the following:

	```
	apply plugin: 'com.android.application'
	```
4. Change it to the following:

	```
	apply plugin: 'com.android.library'
	```
5.Save the file and click Tools > Android > Sync Project with Gradle Files.
That's it. The entire structure of the module remains the same, but it now operates as an Android library and the build will now create an AAR file instead of an APK.

Hold on, we are NOT done yet, we have to build the AAR file, select the library module in the Project window and then click Build > Build APK.

## Import this library or add it as a dependency in another application

To use your Android library's code in another app module, proceed as follows:

1. Add the library to your project in either of the following ways (if you created the library module within the same project, then it's already there and you can skip this step):
	* Add the compiled AAR (or JAR) file (the library must be already built):
		- Click File > New > New Module.
		- Click Import .JAR/.AAR Package then click Next.
		- Enter the location of the compiled AAR or JAR file then click Finish.
	* Import the library module to your project (the library source becomes part of your project):
     		- Click File > New > Import Module.
		- Enter the location of the library module directory then click Finish.

	The library module is copied to your project, so you can actually edit the library code. If you want to maintain a single version of the library code, then this is probably not what you want and you should instead add the compiled AAR file as described above.
2. Make sure the library is listed at the top of your settings.gradle file, as shown here for a library named "my-library-module":
	```include ':app', ':my-library-module'```
3. Open the app module's build.gradle file and add a new line to the dependencies block as shown in the following snippet:
	```dependencies {
		compile project(":my-library-module")
		}```
4. Click Sync Project with Gradle Files.

In this example above, the compile configuration adds the library named my-library-module as a build dependency for the entire app module. If you instead want the library only for a specific build variant, then instead of compile, use buildVariantNameCompile. For example, if you want to include the library only in your "pro" product flavor, it looks like this:

	```productFlavors {
	    pro { ... }
	}
	dependencies {
	    proCompile project(":my-library-module")
	}```
Any code and resources in the Android library is now accessible to your app module, and the library AAR file is bundled into your APK at build time.

However, if you want to share your AAR file separately, you can find it in project-name/module-name/build/outputs/aar/ and you can regenerate it by clicking Build > Make Project.



