# zenoh-roborio-build

Build system for creating Zenoh Java thick JAR artifacts compatible with FRC RoboRIO (ARMv7).

## What This Builds

This repository contains a GitHub Actions workflow that:
1. Builds the Zenoh JNI native library for ARMv7 (RoboRIO architecture)
2. Builds a thick/fat JAR that includes:
   - Zenoh Java classes
   - All runtime dependencies (commons-net, guava, kotlin-stdlib)
   - The ARMv7 native library

## Usage in RoboRIO Projects

To use the built artifact in your FRC RoboRIO project, add the following to your `build.gradle`:

```gradle
repositories {
    mavenCentral()
    ivy {
        url "https://github.com/drakeerv/zenoh-roborio-build/releases/download/"
        patternLayout {
            artifact "zenoh-rio-build-[revision]/[artifact]-[revision]-[classifier].[ext]"
            artifact "zenoh-rio-build-[revision]/[artifact]-[revision].[ext]"
        }
        metadataSources { artifact() }
        content {
            includeGroup "zenoh"
        }
    }
}

dependencies {
    implementation "zenoh:zenoh-java:1.7.2"
}
```

The thick JAR includes all dependencies, so you don't need to add commons-net, guava, or kotlin-stdlib separately.

## Building

The build is triggered manually via GitHub Actions workflow dispatch:

1. Go to Actions tab
2. Select "Build Zenoh for RoboRIO" workflow
3. Click "Run workflow"
4. Enter the Zenoh version (e.g., `1.7.2`)
5. Click "Run workflow"

The workflow will:
- Build the native library for ARMv7
- Build the Java classes
- Create a fat JAR with all dependencies
- Publish a release with the JAR artifact

## Artifact Structure

The released JAR (`zenoh-java-{version}.jar`) contains:
- `io/zenoh/**` - Zenoh Java classes
- `org/apache/commons/net/**` - Commons Net library
- `com/google/common/**` - Guava library  
- `kotlin/**` - Kotlin standard library
- `arm-unknown-linux-gnueabi/arm-unknown-linux-gnueabi.zip` - Native library for ARMv7

## License

This build system is for building artifacts from the [eclipse-zenoh/zenoh-java](https://github.com/eclipse-zenoh/zenoh-java) project. Please refer to that project for licensing information.