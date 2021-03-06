# CogniCrypt_SAST for Android

This repository contains CogniCrypt_SAST for Android which extends [CogniCrypt_SAST](https://github.com/CROSSINGTUD/CryptoAnalysis) to be able to run on Android Application.
It relies on [FlowDroid](https://github.com/secure-software-engineering/FlowDroid) to compute an android-specific callgraph before the actual static analysis is executed.

## Releases

You can checkout a pre-compiled version of CogniCrypt_SAST for Android [here](https://github.com/CROSSINGTUD/CryptoAnalysis-Android/releases).

## Checkout
This repository uses git submodules, to checkout this repository use the following command for git

```git clone --recurse-submodules git@github.com:CROSSINGTUD/CryptoAnalysis-Android.git```

 and Build

### Prerequisite for Build

The code of this repository depends on [FlowDroid](https://github.com/secure-software-engineering/FlowDroid) to be installed as a maven artifact on your machine.
Follow the commands on their readme and run `mvn install` to install FlowDroid into your local maven repository.

## Build

CogniCrypt_SAST for Android uses maven as build tool. To compile this project `cd` into the newly checked out folder and run

```mvn package -DskipTests=true```

Once build, a packaged  `jar` artifact including all dependency is found in `CryptoAnalysis-Android/build/CryptoAnalysis-Android-1.0.0-jar-with-dependencies.jar` 

## Usage

CogniCrypt_SAST for Android can be started via the class `main.CogniCryptAndroid`. It requires three arguments in this order: 
* The absolute path to the .apk file
* The absolute path to the android SDK platforms. The platforms are obtainable via [Android Studio](https://developer.android.com/studio/releases/platforms). Under the Android SDK location you find a folder `platforms`. Supply CogniCrypt_SAST with the path to this folder.
* The absolute path to the directory of the CrySL rules (contents of file [JCA-CrySL-rules.zip](https://github.com/CROSSINGTUD/CryptoAnalysis/releases/tag/v1.0.0). More information about the format is found [here](https://github.com/CROSSINGTUD/CryptoAnalysis/).)

```
java -cp CryptoAnalysis-Android/build/CryptoAnalysis-Android-1.0.0-jar-with-dependencies.jar -Xmx8g -Xss60m main.CogniCryptAndroid \
      <path-to-apk> <path-to-android-platforms> <path-to-crysl-rules-binary>
```
The analysis generates an output folder in `cognicrypt-reports/<apk-filename>/`. The folder contains a file `CogniCrypt-Report.txt` along with the `.jimple` output of the classes the analysis found misuses in. More information about the format of the report is provided [here](https://github.com/CROSSINGTUD/CryptoAnalysis/).

Note, depending on the analyzed application, the analysis may require a lot of memory and a large stack size. Remember to set the necessary heap size (e.g. -Xmx8g) and stack size (e.g. -Xss60m).
