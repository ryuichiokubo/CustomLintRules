# CustomLintRules
A basic, Gradle-based project template for writing custom Android Lint rules.

## How to start?
### Step 1
Clone this repository and open it in the IDE of your choice.

```shell
git clone https://github.com/a11n/CustomLintRules.git
```

### Step 2
In order to write your own custom Lint rules create a new class and make it extend `Detector`. See `HelloWorldDetector` for reference.

Depending on your use case you might also want to implement one of or more of `Detector`'s interfaces:

* **JavaScanner:**
  Specialized interface for detectors that scan Java source file parse trees
* **XmlScanner:**
  Specialized interface for detectors that scan XML files 
* **ResourceFolderScanner:**
  Specialized interface for detectors that scan resource folders
* ...

I recommend to have a look at the default set of detectors to get an impression of the anatomy of `Detector`s. [1]

### Step 3
Detectors scan the development artifacts for one or more issues to report. Last but not least you need to register your custom issue(s) in the `CustomIssueRegistry`.

### Step 4
Finally you might want to have a unit test for your custom Lint rules in place. I've created a JUnit rule for that purpose and your convenience [2]. `HelloWorldDetectorTest` demonstrates how to test custom Lint rules.

That’s it.

## How to integrate?
In order to apply your custom Lint rules to your project you basically have two options.

### 1. Copy your rules to a specific folder

By default, Lint will look for custom Lint rules in `~/.android/lint` and will consider them during analysis. To apply your custom Lint rules just assemble your rules and copy the resulting *.jar to that folder:

```shell
./gradlew clean assemble
```

On Unix based systems you can simply use `./buildAndInstall` for your convenience.

### 2. Wrap your rules into an AAR bundle

A more advanced option is the new AAR format, which allows to bundle custom Lint rules [3]. You can include the resulting *.aar file into your Android application project. When executing `./gradlew lint` in your Android application project your custom Lint rules will be considered automatically.

To wrap your custom Lint rules into an AAR container just execute:
```shell
./gradlew aarWrapper:assemble
```

You will find the AAR file in `aarWrapper/build/outputs/aar/`. Just copy it to the `libs` folder of your Android application project.

## Remark
As Google points out very significant, the Lint API **is not final and may change in future releases** [4].

This project refers to the most recent (March 2015), stable version (24.1.3) of the Lint API [5].

## Credits
Approaches used in this project have (partly) been adopted from this blog post [6].
Thank you Cheng Yang for sharing your ideas and findings.

## References
1. https://android.googlesource.com/platform/tools/base/+/master/lint/libs/lint-checks/src/main/java/com/android/tools/lint/checks (visited 2015-04-25)
2. https://github.com/a11n/lint-junit-rule (visited 2015-04-25)
3. http://tools.android.com/tech-docs/new-build-system/aar-format (visited 2015-04-25)
4. http://tools.android.com/tips/lint/writing-a-lint-check (visited 2015-04-25)
5. http://mvnrepository.com/artifact/com.android.tools.lint/lint-api/24.1.3 (visited 2015-04-25)
6. https://engineering.linkedin.com/android/writing-custom-lint-checks-gradle (visited 2015-04-25)