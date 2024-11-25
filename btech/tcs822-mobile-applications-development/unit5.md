# Unit 5

## Publishing Android Application

It is the final phase of android application development.

The source code (.java files) written in java is converted by the java compiler (javac) into .class files. The dex compiler then converts these files into .dex files which are then packaged by AAPT (android asset packaging tool).

If the application is to be distributed on the google play store, a signing process must be completed, which adds additional private key information to the .apk file.

![[android-application-development-and-deployment-phases.png]]

### Process of publishing an android application

It consists of 2 phases:

1. preparing the application for release
2. releasing the application to the users

#### Preparing the application for release

It consists of the following tasks:

##### specify a suitable name and icon for the application

choose a suitable name for the application before the distribution, because after the distribution name cannot be changed.

name and icon is set in the `AndroidManifest.xml` file.

```xml
<application>
	...
	android:icon="@mipmap/xxx"
	android:label="@string/name"
	...
</application>
```

##### provide proper versioning of the application

- helps to identify which version of the release is installed.
- helps to resolve problems.
- helps to determine comp
- helps in automation

following two attributes are used to control version:

- `versionCode`: it is a positive integer value used by the android system internally.
	- is not known to the users.
	- higher value indicates latest version.
	- prevents users from installing olders versions.

- `versionName`: is is a string in the format `<major>.<minor>.<point>`
	- major value is changed when a new feature or module is added.
	- minor value is changed when new small featues are added.
	- point value is changed when bug fixes and small changes are made.
	- example, `1.2.1`

these values are set directly in `AndroidManifest.xml` file directly.

##### remove unwanted resources, garbage code and logging code

the `debuggable` attribute in the android manifest file must be set to `false`, exmaple: `android:debuggable = "false"`. this removes unwanted code or resources, debug symbols which we used duing development but are not required by the public. this also improves performance.

##### configure the application with required permissions in the `AndroidManifest.xml` file

for various types of operations, specific permissions are required which must be set in the android manifest file.

```xml
<uses-permission android:name="android.permissions.SEND_SMS"/>
<uses-permission android:name="android.permissions.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permissions.WRITE_EXTERNAL_STORAGE"/>
```

##### update all application resources like graphics and multimedia files

update the application resource like graphics and media files for release.

##### prepare remote servers used by the application if any

if the android application is using remote connections, such as a remote database, then check the connectivity and ensure it works before releasing.

##### generate a self signed certificate, digitally sign the appliation and build and apk

any android application has to be digitally signed by the developer before it is released in the market. all android applications require a digital certificate and a private key to upload the application in google play.

the certificate is used to identify the developer. new versions of the applications also use the same certificate. the same key is also used by the developer to publish multiple applications.

the signing key is done in two modes:

1. debug
2. release

##### test the release version

the release version is tested on real devices in real scenarios to ensure that the user interface is user friendly and working properly.

#### Releasing the application to the users

following are few major ways in which applications can be released to the public:

- google play store:
	- it is the official marketplace for distributing android applications.
	- supports publicity, selling and distribution of android applications throughout the world.
	- to release an application on google play
		- create a google developer account by paying registration fees and signing the documents
		- the app details such as product description, feedback, content rating, pricing information are provided through wizards.
		- the app size limit is 100mb, if the app is greater than that expansion files are used.
		- the app can be published using alpha, beta or production channels.
		- aplha version is used for testing and available only for a limited number of users.
		- any interested users who have been given permission can test the beta version and send feedback.
		- production version is available to all the users in selected regions.

- email:
	- one of simple and fastest ways
	- unssafe as anyone can pirate it

- download from website:
	- host a webiste to allow downloading the application
	- hard to track the users

- bluetooth:
	- only authorized users can access the application
