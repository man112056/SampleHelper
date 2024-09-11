# Publishing Android Library to Local Maven Repository

This document explains how to configure and publish your Android library to a local Maven repository using Gradle.

## 1. Gradle Plugin Setup

In your **app-level `build.gradle`** file, apply the `maven-publish` plugin. Place it at the top:

```gradle
plugins {
    id 'maven-publish'
}
```

This plugin allows your library to be published to a Maven repository.

## 2. Publishing Configuration

Add the following `publishing` block to the **app-level `build.gradle`** file to define how the library is published:

```gradle
publishing {
    publications {
        release(MavenPublication) {
            groupId = 'com.mandroid'     // Library group ID (modifiable)
            artifactId = 'my-library'    // Library artifact ID (modifiable)
            version = '1.0'              // Library version (modifiable)

            afterEvaluate {
                from components.release  // Publishing the release variant (modifiable to 'debug')
            }
        }
    }

    repositories {
        maven {
            name = 'myrepo'               // Repository name (modifiable)
            url = layout.buildDirectory.dir("repo")  // Directory for the local Maven repo
        }
    }
}
```

### Key Fields:
- **groupId**: Defines the group ID of the library (modifiable).
- **artifactId**: Defines the artifact name of the library (modifiable).
- **version**: Defines the version number (modifiable).
- **from components.release**: Specifies which build variant to publish (`release` or `debug`).

## 3. Publishing the Library

Run the following command to publish the library to the local Maven repository:

```bash
./gradlew publishReleasePublicationToMyrepo
```

### Command Explanation:
- **`publishReleasePublicationToMyrepo`**: Auto-generated from the combination of:
    - `release`: The publication name.
    - `myrepo`: The repository name.

### Customization:
- If you change the publication or repository names in the `publishing` block, the command will update accordingly.

For example, if you rename `release` to `myLibraryRelease`, the command will be:
```bash
./gradlew publishMyLibraryReleasePublicationToMyrepo
```

## 4. Using the Library

Once published, you can reference the library in another project by adding the following to the consuming project's `build.gradle`:

```gradle
repositories {
    maven {
        url uri('<path_to_your_project>/build/repo')
    }
}

dependencies {
    implementation 'com.mandroid:my-library:1.0'
}
```

## 5. Unmodifiable Keywords

- **`publishing {}`**: Defines the publishing configuration, cannot be changed.
- **`MavenPublication`**: Specifies the publication type, required for publishing to a Maven repository.
- **`from components.release`**: Specifies which build type to publish. This part is required to publish a specific build type.

### Screenshot

Here is a screenshot of the library in action:
<img alt="Library Screenshot" src="src/main/resources/screenshots/screenshot.png"/>
