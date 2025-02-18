<h1>
  <img src="https://axeptio.imgix.net/2024/07/e444a7b2-ea3d-4471-a91c-6be23e0c3cbb.png" alt="Descrizione immagine" width="80" style="vertical-align: middle; margin-right: 10px;" />
  Axeptio Android SDK Documentation
</h1>

Welcome to the Axeptio Mobile SDK Samples project! This repository demonstrates how to implement the **Axeptio Android SDK** in your mobile applications.
# 📑 Table of Contents
1. [Overview](#1-overview)
2. [Getting Started](#2-getting-started)
3. [Axeptio SDK Implementation](#3-axeptio-sdk-implementation)

<br><br>

# 1. 👨‍💻Overview
This project includes two modules:
- `samplejava`: Demonstrates how to use the Axeptio SDK with Java and XML.
- `samplekotlin`: Shows the integration of the Axeptio SDK with Kotlin and Compose.

Both modules can be built using either the **brands** or **publishers** variants, depending on your specific needs.

<br><br><br>

# 2. Getting Started
To begin testing the Axeptio SDK sample applications, follow these steps:

### Clone the repository

First, clone the repository to your local development environment:
```bash
git clone https://github.com/axeptio/sample-app-android
```
### Configure your Github access token
Add your Github access token for the Axeptio SDK in the `settings.gradle.kts file`. This is necessary for fetching the SDK from the repository.
```kotin
maven {
    url = uri("https://maven.pkg.github.com/axeptio/axeptio-android-sdk")
    credentials {
        username = "" // TODO: Enter your GitHub username
        password = "" // TODO: Enter your GitHub package token
    }
}
```
### Select the appropriate sample module
Choose the module corresponding to your preferred programming language and UI framework:

- **samplejava**: Java and XML integration
- **samplekotlin**: Kotlin and Compose integration

### Choose your build variant:
Depending on your use case, select the appropriate build variant:

- **publishers**
- **brands**

<br><br><br>

# 3. 💻Axeptio SDK Implementation
The Axeptio SDK provides consent management functionality for Android applications, enabling seamless integration for handling user consent.

### Gradle Implementation
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



