<h1 align="center">Ringer Interactive SDK - Android</h1>

<p align="center">
Ringer is an Android SDK that allows the mobile app to save and update contacts along with notifications. The end result is such that the app can push fullscreen images to call recipients' mobile phones, and the sdk can provide information on the call recipient's device, such as OS version of Device, Device Name, and Timezone of Device.
</p>

## Precondition 

##### 1. Minimum SDK Version Supported is 7 Nouget
##### 2. You have to use Hilt (Dependency Injection) to use this SDK


## How to integrate SDK in your application

### Step 1
If you are already using the Firebase you can use the following code.
Add the function below in the FirebaseMessagingService

```onMessageReceived

        try {
            LibrarySDKMessagingService().sendNotification(this,remoteMessage)
        } catch (e: Exception) {
            e.printStackTrace()
        }
```

### Step 2
Now add below line in your project level build

```
	allprojects {

   		repositories {
   			...
   			maven { url 'https://jitpack.io' }
   		}
         }

```

### Step 3
If you are not using Firebase, please use the following code.

```gradle
implementation ('com.github.RingerInteractive:Ringer-SDK-Android:1.0.0'){
        transitive = true
        // Use the consuming application's FireBase module, so exclude it
        // from the dependency. (not totally necessary if you use compileOnly
        // when declaring the dependency in the library project).
        exclude group: 'com.google.firebase'
        // Exclude the "plain java" json module to fix build warnings.
        exclude group: 'org.json', module: 'json'
    }
    implementation("com.google.firebase:firebase-messaging:22.0.0") {
        // Exclude the "plain java" json module to fix build warnings.
        exclude group: 'org.json', module: 'json'
    }
```

### Step 3.1
In the Manifest File add the following code.
To continue to get notified

```Manifest

    <uses-permission android:name="android.permission.READ_CONTACTS"/>
    <uses-permission android:name="android.permission.WRITE_CONTACTS"/>

        <service
            android:name="com.ringer.interactive.firebase.LibrarySDKMessagingService"
            android:exported="true">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
        </service>
```

##### Now Setup is complete. The next step iis to Add Credentials in order to use this SDK.

### Step 3.2
#### Add username and password in a string.xml file in your project

```string.xml
<string name="ringer_user_name">Your Registered Email Address</string>
<string name="ringer_password">Your Password</string>
```

After adding these credentials you will have access to the SDK.

### Step 3.3

In your MainActivity, call the following function to continue.
This is required to use the SDK.

```YourActivity

   InitializeToken(this,resources.getString(R.string.ringer_user_name),resources.getString(R.string.ringer_password),"YOUR APP NAME","Your Number")

```

### Step 3.4

Final Step to Complete the SDK Setup
This step is for the permissions you need granted.

```YourActivity

    override fun onRequestPermissionsResult(
        requestCode: Int, permissions: Array<out String>,
        grantResults: IntArray
    ) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults)

        RingerInteractive().onRequestPermissionsResult(requestCode, permissions, grantResults,this)

    }

```


## How to use sample app
##### 1.Download lasted [Releases](https://raw.githubusercontent.com/RingerInteractive/Ringer-SDK-Sample-App/main/Ringer-Sample-Release.apk) and install on your device
##### 2.Build APK from [Ringer-Sample-App](https://github.com/RingerInteractive/Ringer-SDK-Sample-App)

##### After install you should grant those required permission 
1. Default call handler
```YourActivity
  offerReplacingDefaultDialer(this, applicationContext.packageName, REQUEST_CODE_SDK)
 ```
<p align="center"><img src="https://raw.githubusercontent.com/RingerInteractive/Ringer-SDK-Sample-App/main/default_call.png" width="400" alt="Default call"></p>

2. Access Contact
```YourActivity
  ActivityCompat.requestPermissions(this, arrayOf(Manifest.permission.READ_CONTACTS, Manifest.permission.WRITE_CONTACTS), PERMISSIONS_REQUEST_READ_CONTACTS)

 ```
<p align="center"><img src="https://raw.githubusercontent.com/RingerInteractive/Ringer-SDK-Sample-App/main/access_contact.png" width="400" alt="access contact"></p>


3. Appear On Top
```YourActivity
   ActivityCompat.requestPermissions(this,arrayOf(Manifest.permission.SYSTEM_ALERT_WINDOW),PERMISSIONS_REQUEST_CALL_LOG)
 ```
<p align="center"><img src="https://raw.githubusercontent.com/RingerInteractive/Ringer-SDK-Sample-App/main/ontop_permission.png" width="400" alt="on top"></p>

4. Allow app notification
<p align="center"><img src="https://raw.githubusercontent.com/RingerInteractive/Ringer-SDK-Sample-App/main/allow_notification.png" width="400" alt="on top"></p>

5. Main Screen
<p align="center"><img src="https://raw.githubusercontent.com/RingerInteractive/Ringer-SDK-Sample-App/main/main_screen.png" width="400" alt="on top"></p>

5. Open Admin portal with provide account and make phone call to test 








