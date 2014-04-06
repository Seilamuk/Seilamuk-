If you are upgrading from the old SearchBox SDK, please perform the following steps:

**1** In the manifest, do the following:

a.	Remove the following permission:
```java
<uses-permission android:name="android.permission.READ_PHONE_STATE"/>
```
b.	remove the EULA activity from the application node
```java
<activity android:name="com.startapp.android.eula.EULAActivity" 
          android:theme="@android:style/Theme.Translucent" 
          android:configChanges="keyboard|keyboardHidden|orientation" />
```
**2**	Within your main activity:

a.	Remove all imports which contain the package: ``com.searchboxsdk.android``

b.	Remove the following static function from the ``onCreate`` method:
```java
StartAppSearch.init(this, "<Your Developer Id>", "<Your App ID>");
```

c.	Remove the following static function from the ``onCreate`` method after the ``setContentView`` (Only if you had implemented it)
```java
StartAppSearch.showSearchBox(this);
```
**3**	Repeat steps 2.a, 2.c for each activity that the SearchBox had been implemented in.

**4**	 If you been using Obfuscation – remove the following proguard configuration: 
```
-optimizations !code/simplification/arithmetic,!field/*,!class/merging/* -keep class com.searchboxsdk.** { 
*; 
} 
-keep class com.startapp.android.eula.** { 
*; 
} 
-dontwarn com.searchboxsdk.android.**
```