<h1>
  <img src="https://axeptio.imgix.net/2024/07/e444a7b2-ea3d-4471-a91c-6be23e0c3cbb.png" alt="Descrizione immagine" width="80" style="vertical-align: middle; margin-right: 10px;" />
  Axeptio Android SDK Documentation
</h1>

Welcome to the Axeptio Mobile SDK Samples project! This repository demonstrates how to implement the **Axeptio Android SDK** in your mobile applications.
# 📑 Table of Contents
1. [Overview](#1-overview)
2. [Getting Started](#2-getting-started)
3. [Axeptio SDK Implementation](#3-axeptio-sdk-implementation)
4. [Initialize the SDK](#4-initialize-the-sdk)
5. [Responsibilities: Mobile App vs SDK](#5-responsibilities-mobile-app-vs-sdk)
6. [Get Stored Consents](#6-get-stored-consents) 

<br><br>

# 1. 👨‍💻Overview
This project includes two modules:
- `samplejava`: Demonstrates how to use the Axeptio SDK with Java and XML.
- `samplekotlin`: Shows the integration of the Axeptio SDK with Kotlin and Compose.

Both modules can be built using either the **brands** or **publishers** variants, depending on your specific needs.

<br><br><br>

# 2. Getting Started
To begin testing the Axeptio SDK sample applications, follow these steps:

#### Clone the repository

First, clone the repository to your local development environment:
```bash
git clone https://github.com/axeptio/sample-app-android
```
#### Configure your Github access token
To properly configure access to the Axeptio SDK, you need to add your GitHub token in the `settings.gradle.kts` file to fetch the SDK from the private repository. The library is not available on a public Maven repository, so it is crucial to configure the private repository correctly to avoid errors. You can also consider publishing the Axeptio SDK to a public repository to simplify integration, reducing the process complexity. Here’s how to configure the private repository in the `settings.gradle.kts` file:
```kotin
maven {
    url = uri("https://maven.pkg.github.com/axeptio/axeptio-android-sdk")
    credentials {
        username = "[GITHUB_USERNAME]"  // Enter your GitHub username
        password = "[GITHUB_TOKEN]"    // Enter your GitHub token
    }
}
```
#### Ensure Proper Configuration in Axeptio Backoffice
Before proceeding with the integration, ensure that your project is correctly configured in the Axeptio backoffice. Specifically, verify that your clientId and configurationId are set up correctly. This is critical for the SDK to function as expected. If these values are not correctly configured, the SDK will not initialize properly, leading to errors during integration.

#### Select the appropriate sample module
Choose the module corresponding to your preferred programming language and UI framework:

- **samplejava**: Java and XML integration
- **samplekotlin**: Kotlin and Compose integration

#### Choose your build variant:
Depending on your use case, select the appropriate build variant:

- **publishers**
- **brands**

<br><br><br>

# 3. 💻Axeptio SDK Implementation
The Axeptio SDK provides consent management functionality for Android applications, enabling seamless integration for handling user consent.

#### Gradle Implementation
The SDK is hosted on GitHub Packages and is compatible with Android SDK versions **>= 26**.

Follow these steps to integrate the Axeptio SDK into your Android project:
- **Add the Maven repository to your `settings.gradle` file**

   Ensure the provided GitHub token has the `read:packages` scope enabled. Add the following configuration to your `settings.gradle` file.
 - **Kotlin DSL**
```kotlin
// Start dependency resolution management block
dependencyResolutionManagement {
    repositories {
        // Add Google's Maven repository to the project
        google()
        
        // Add Maven Central repository
        mavenCentral()
        
        // Add the GitHub Packages repository for the Axeptio SDK
        maven {
            // Set the URL of the GitHub repository hosting the Axeptio SDK
            url = uri("https://maven.pkg.github.com/axeptio/axeptio-android-sdk")
            
            // Configure credentials for accessing the GitHub Packages repository
            credentials {
                // Provide your GitHub username here
                username = "[GITHUB_USERNAME]"
                
                // Provide your GitHub token here, ensuring the 'read:packages' scope is enabled
                password = "[GITHUB_TOKEN]"
            }
        }
    }
}
```
 - **Groovy**
```groovy
repositories {
    maven {
        url = uri("https://maven.pkg.github.com/axeptio/axeptio-android-sdk")
        credentials {
            username = "[GITHUB_USERNAME]"
            password = "[GITHUB_TOKEN]"
        }
    }
}
```
- **Add the SDK dependency to your `build.gradle` file**
After adding the repository, include the Axeptio SDK as a dependency in your project.
 - **Kotlin DSL**
```kotlin
dependencies {  
    implementation("io.axept.android:android-sdk:2.0.3")
}
```
 - **Groovy**
```groovy
dependencies {
    implementation 'io.axept.android:android-sdk:2.0.3'
}
```
For more detailed instructions, refer to the [GitHub Documentation](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-gradle-registry#using-a-published-package)

<br><br><br>

# 4. Initialize the SDK
To initialize the Axeptio SDK, you must call the initialization method inside the `onCreate()` method of your main activity. This call should be made before invoking any other Axeptio SDK functions. The SDK can be configured for either **Publishers** or **Brands** using the `AxeptioService` enum during initialization.
#### Kotlin Implementation
```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    
    // Check if the SDK is already initialized to prevent multiple initializations
    if (!AxeptioSDK.instance().isInitialized()) {
        // Initialize the Axeptio SDK with the required configuration
        AxeptioSDK.instance().initialize(
            activity = this@MainActivity,  // Context of the current activity
            targetService = AxeptioService.PUBLISHERS_TCF,  // Choose the target service: Publishers or Brands
            clientId = "your_client_id",  // Replace with your actual client ID
            cookiesVersion = "your_cookies_version",  // Specify the version of cookies management
            token = "optional_consent_token"  // Optional: Provide an existing consent token if available
        )
    }
}
```
#### Java Implementation
```java
@Override
protected void onCreate(@Nullable Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    
    // Check if the SDK is already initialized to prevent multiple initializations
    if (!AxeptioSDK.instance().isInitialized()) {
        // Initialize the Axeptio SDK with the required configuration
        Axeptio axeptio = AxeptioSDK.instance();
        axeptio.initialize(
            MainActivity.this,  // Context of the current activity
            AxeptioService.PUBLISHERS_TCF,  // Choose the target service: Publishers or Brands
            "your_project_id",  // Replace with your actual project ID
            "your_configuration_id",  // Provide your configuration ID
            "optional_consent_token"  // Optional: Provide an existing consent token if available
        );
    }
}
```
#### Consent Popup Behavior
Once the SDK is initialized, the consent popup will automatically display if the user's consent is either expired or has not yet been registered. The SDK takes care of managing the consent state automatically.

#### Transferring User Consents (Publishers)
For publishers, you can transfer a user's consent information by providing their Axeptio token. This token allows the SDK to automatically update the user's consent preferences in the SharedPreferences, following the TCFv2 (Transparency and Consent Framework) IAB (Interactive Advertising Bureau) specifications.

#### Preventing Multiple Initializations of the SDK
Calling `initialize()` multiple times during the same session can cause crashes. To avoid this, always check if the SDK has already been initialized before making the call:
- **Kotlin**
```kotlin
if (!AxeptioSDK.instance().isInitialized()) {
    AxeptioSDK.instance().initialize(
        activity = this@MainActivity,
        targetService = AxeptioService.PUBLISHERS_TCF,
        clientId = "your_client_id",
        cookiesVersion = "your_cookies_version",
        token = "optional_consent_token"
    )
}
```
- **Java**
```java
if (!AxeptioSDK.instance().isInitialized()) {
    AxeptioSDK.instance().initialize(
        MainActivity.this,
        AxeptioService.PUBLISHERS_TCF,
        "your_project_id",
        "your_configuration_id",
        "optional_consent_token"
    );
}
```
By checking the `isInitialized()` method, you ensure that the SDK is initialized only once, preventing potential issues caused by multiple initializations.

#### Handling the "INSTALL_FAILED_INVALID_APK" Error
This error can occur during installation, typically due to issues with the APK or dependencies. The best solution is to perform a **clean build** to ensure that all libraries are properly integrated. To do so, execute the following command in your terminal:
```bash
./gradlew clean build
```
This will clean the project and rebuild it, resolving any issues related to corrupted or improperly linked files. After completing the build, try reinstalling the app.

##### Key Consideration 
- **Client ID** and **Configuration ID** should be properly configured according to your specific project setup.
- The **Axeptio token** is optional, but it allows for better management of user consent states across different sessions.
- Always ensure that you check for SDK initialization before calling `initialize()` to prevent multiple initializations that could cause crashes.

The integration of the Axeptio SDK into your mobile application involves clear delineation of responsibilities between the mobile app and the SDK itself. Below are the distinct roles for each in handling user consent and tracking.
<br><br><br>
# 5. Responsibilities: Mobile App vs SDK
#### **Mobile Application Responsibilities:**

1. **Managing App Tracking Transparency (ATT) Flow:**
   - The mobile app is responsible for initiating and managing the ATT authorization process on iOS 14 and later. This includes presenting the ATT request prompt at an appropriate time in the app's lifecycle.

2. **Controlling the Display Sequence of ATT and CMP:**
   - The app must determine the appropriate sequence for displaying the ATT prompt and the Axeptio consent management platform (CMP). Specifically, the app should request ATT consent before invoking the Axeptio CMP.

3. **Compliance with App Store Privacy Labels:**
   - The app must ensure accurate and up-to-date declarations of data collection practices according to Apple’s privacy label requirements, ensuring full transparency to users about data usage.

4. **Event Handling and User Consent Updates:**
   - The app is responsible for handling SDK events such as user consent actions. Based on these events, the app must adjust its behavior accordingly, ensuring that user consent is respected across sessions.

#### **Axeptio SDK Responsibilities:**

1. **Displaying the Consent Management Interface:**
   - The Axeptio SDK is responsible for rendering the user interface for the consent management platform (CMP) once triggered. It provides a customizable interface for users to give or revoke consent.

2. **Storing and Managing User Consent Choices:**
   - The SDK securely stores and manages user consent choices, maintaining a persistent record that can be referenced throughout the app's lifecycle.

3. **Sending Consent Status via APIs:**
   - The SDK facilitates communication of the user's consent status through APIs, allowing the app to be updated with the user’s preferences.

4. **No Implicit Handling of ATT Permissions:**
   - The Axeptio SDK does **not** manage the App Tracking Transparency (ATT) permission flow. It is the host app's responsibility to request and handle ATT permissions explicitly before displaying the consent management interface. The SDK functions only once the ATT permission is granted (or bypassed due to platform restrictions).
<br><br><br>

# 6. 🔑Get Stored Consents
You can retrieve the consents stored by the Axeptio SDK in **SharedPreferences**. The following example demonstrates how to access these values within your app:
- **Kotlin Examples**
```kotlin
// Access SharedPreferences to retrieve stored consent values
val sharedPref = context.getSharedPreferences("MyPrefs", Context.MODE_PRIVATE)

// Retrieve a specific consent value by key (replace "key" with the actual key you're using)
val consentValue = sharedPref.getString("key", "default_value")
```
In this example, replace `key` with the actual key used to store consent information, and `default_value` with the value you want to return if no consent is found.
For more detailed information about the stored values, cookies, and how to handle them according to the Axeptio SDK, please refer to the [Axeptio Documentation](https://support.axeptio.eu/hc/en-gb/articles/8558526367249-Does-Axeptio-deposit-cookies)
