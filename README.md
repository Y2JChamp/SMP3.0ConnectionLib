[![](https://jitpack.io/v/insanediv/SMP3.0ConnectionLib.svg)](https://jitpack.io/#insanediv/SMP3.0ConnectionLib)
[![Jenkins](https://img.shields.io/jenkins/s/https/jenkins.qa.ubuntu.com/view/Precise/view/All%20Precise/job/precise-desktop-amd64_default.svg)]()

# SMP3.0 Connection Library
This project aims at providing a simple set of instruments to cope with SAP Mobile Platform (SMP3.0) login and user onboarding process.
This library is intended as a lightweight alternative to the heavy [SAP MAFLogon framework](https://github.com/SAP/sap_mobile_native_android).

You can include this library as a gradle dependency as follows.

**Add it in your root build.gradle at the end of repositories:**
```groovy
allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}
```
**Add the dependency to your module build.gradle file**
```groovy
	dependencies {
	        compile 'com.github.insanediv:SMP3.0ConnectionLib:1.0.8'
	}
```

## Basic usage
```java
	  // Reference to context
        Context context = this;
        // Referencce to Ion for networking
        Ion ion = Ion.getDefault(context);

        // http(s)://<smp_host>:<port>
        String smpServiceRoot = "https://smp.host.org";
        // Application connectionid as defined in the SMP Cockpit
        String appid = "appid";
        // Ignore cookies during connection id retrieval
        boolean ignoreCookies = true;
        
        // Credentials
        String username = "username";
        String password = "password";


        try {
            SmpIntegration smpIntegration = new SmpIntegration(this, Ion.getDefault(this),
                    smpServiceRoot, appid, ignoreCookies);
            smpIntegration.setDelegate(new SmpConnectionEventsDelegate() {
                @Override
                public void onLoginError(Exception e, Response<String> result) {
                    //Your magic happens here..
                }

                @Override
                public void onRegistrationError(Exception e, Response<String> result) {
                    //Your magic happens here..
                }

                @Override
                public void onConnectionSuccess(String xsmpappcid) {
                    //Your magic happens here..
                }

                @Override
                public void onNetworkError(Exception e, Response<String> result) {
                    //Your magic happens here..
                }
            });

            // Starts connection attempt. Events are back reported to the delegate
            smpIntegration.connect(username, password);
        } catch (SmpExceptionInvalidInput ex) {
            // INVALID PARAMETERS ARE PROVIDED
            ex.printStackTrace();
        }
```
