# Gradle Autowrapper Plugin

The plugin that will make sure you're executing your build with the correct Gradle version!

Working on large, multi multi-project products with gradle, I've learned that managing
Gradle configurations across several directly and indirectly related build files can be a bit cumbersome. Resulting with not
well though of copy-paste of build scripts and the overall neglect of older build files.

This plugin is the first step of solving such problems - making sure the build is executed with the correct binaries.
The Autowrapper plugin provides a simple way to ensure that the execution uses a new enough or even exact version of
Gradle. In the case of a mismatch version - it will generate the correct version of Gradle Wrapper.

![Screenshot](/docs/screenshot.jpg?raw=true "Screenshot")
(In this screenshot you can see the result of a strict, failFast configuration. Autowrapper recognizes that the executing
Gradle is 2.6 and the required version is 2.5 thus failing the build and generating the correct wrapper file. The second
 execution passes with the right version)

## Usage

Currently there is no available dependency you can pull from the various repositories, for that the maven plugin is appearent
in the plugin's build script

To build the project and make it available as a maven local artifact execute the following command from the clone directory
```shell
./gradlew clean build install
```

To apply the plugin in your build add the following to the build script
```gradle
apply plugin: 'autowrapper'

buildscript {
	repositories {
		mavenLocal()
	}
	dependencies {
		classpath 'com.boazj.gradle:gradle-autowrapper-plugin:1.0-rc1'
	}
}
```

To configure the autowrapper execution add the following configuration to the build script (see properties listed in the table below)
```gradle
autowrapper {

}
```

Property          | Description                                                                    | Type          | Default Value
-------------     | -------------                                                                  | ------------- | -------------
gradleVersion     | The required gradle version                                                    | String        | The executing Gradle's version
strict            | Whether or not a newer version then gradleVersion is acceptable                | boolean       | false
failFast          | Whether or not the build should be actively failed on gradle version mismatch  | boolean       | true
autoGen           | Whether or not the plugin should generate the correct wrapper version          | boolean       | true
quiet             | Whether or not the plugin should output to the log                             | boolean       | false
archiveBase       | [See Documentation](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.wrapper.Wrapper.html#org.gradle.api.tasks.wrapper.Wrapper:archiveBase)
archivePath       | [See Documentation](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.wrapper.Wrapper.html#org.gradle.api.tasks.wrapper.Wrapper:archivePath)
distributionBase  | [See Documentation](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.wrapper.Wrapper.html#org.gradle.api.tasks.wrapper.Wrapper:distributionBase)
distributionPath  | [See Documentation](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.wrapper.Wrapper.html#org.gradle.api.tasks.wrapper.Wrapper:distributionPath)
distributionUrl   | [See Documentation](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.wrapper.Wrapper.html#org.gradle.api.tasks.wrapper.Wrapper:distributionUrl)
jarFile           | [See Documentation](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.wrapper.Wrapper.html#org.gradle.api.tasks.wrapper.Wrapper:jarFile)
scriptFile        | [See Documentation](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.wrapper.Wrapper.html#org.gradle.api.tasks.wrapper.Wrapper:scriptFile)


## Road Map
- [X] Add an "only enforce" mode in which the plugin does not regenerate the wrapper to the required version
- [ ] Add a simplified way to create a distribution locator which will provide a simple way to access custom mirrors for the Gradle binaries
- [ ] Add a locator for obtaining a centralized Gradle version instead of a per build script version
- [ ] Add cli override to some of the properties
- [ ] Publish to repository