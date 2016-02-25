To get started with DroidActivator, you need to perform the following steps:
  * Install and configure an Apache / Php / MySql environment
  * Install and configure the backend php application
  * Add activation records to the database
  * Link DroidActivator's Library to your app
  * Call the activation process from within your Android code

## Installing an Apache / Php / MySql environment ##
The backend is based on the classic combination of Apache - Php - Mysql. If you don't already have it installed on your server, you will find lots of tutorials on the web. One of the most popular solutions on Windows is [WAMP](http://www.wampserver.com/). Using Apache is not mandatory, you can use any php-enabled web server. Php 5.0 and above is supported.

## Installing the backend ##
The backend is written in php and is contained in the "da\_backend" folder. Download the latest da\_backend\_xx.zip file from the Downloads section, decompress it and copy the resulting "da\_backend" folder to your web server's web folder.

## Configuring the backend ##
Open the **config.php** file (located inside the da\_backend folder) with a text editor and replace the default configuration with your HOST, USERNAME, PASSWORD, DATABASE values.

Open your browser and type http://[yourserveraddress]/da_backend/admin.php

if everything is ok, you should be asked for the Admin password.

Type <b>da</b> and submit (you can change the default password in the config.php file)

an empty database is created and a a page is shown where you can manage activation records.

## Adding activation records ##
Press the **add record** button. Fill the fields with activation data. User Id, App Name and Activation code are mandatory. You can leave the other fields blank.
  * Type an e-mail address as  the UserId
  * Type the App Name exactly as declared in the "android:label" attribute of your Android App's Manifest
  * Type an 8-digit random code in the Activation Code field.

Then, save the record.

Now the backend is ready to respond to the activation requests coming from your Android app.


## Linking DroidActivator's library to your app ##
  * Download the latest Android JAR library (droidactivator\_xx.jar) from the Downloads section and put it in a folder called "libs" inside your Android app's folder
  * In Eclipse, open your project's Properties -> Java Build Path-> Libraries, and add the Jar. DroidActivator's classes should now be available to your app.
  * Open your app's Manifest file and check you have declared the following permissions:

```
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.INTERNET" />
```


## Call the activation process from within your code ##


Open your main activity onCreate() method and implement the following code:

```
public void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
			
	// instantiate DroidActivator supplying your server's IP address 
	// and a Runnable to be run after the activation cycle
	DroidActivator.newInstance(this, "http://yourserveraddress",new Runnable() {	
		public void run() {startMyApp();}
	});

	// perform the Activation cycle – your startup method is called when finished
	DroidActivator.doActivationCycle(this);
		
}


// start your app from here
private void startMyApp() {

	// test the activator variables and do whatever is appropriate	
	if (DroidActivator.isActivated()) {
		switch (DroidActivator.getLevel()) {
		case 0:
			//startInDemoMode();
			break;
		case 1:
			//startInBasicMode();
			break;
		case 2:
			//startInProMode();
			break;
		}
	}
	else {
		//startInDemoMode();
	}
}
```

As the server address, provide only `http://` followed by the IP address or DNS name of your server

**right->** `http://151.13.2.58`

**right->** `http://mydomain.org`

**wrong->** `http://151.13.2.58/da_backend/check.php`

please do not change the backend folder or file names as they are hard-coded in the jar library.

When your app starts up, DroidActivator's Activation dialog should appear. Have fun!

_**Note: this is a quick and dirty implementation to get started. A better implementation pattern, using a separated ActivationActivity, is explained in the [User Guide](UserGuide#In_your_code.md).**_