# Tagit Core Common Gradle Scripts (tagit-core-gradle)
The home of the common Gradle Scripts used by Tagit and Mobeix Java frameworks, libraries and applications.

## How to Use

### gradle.properties
Define a property `tagitGradleVersion` for use inside the Gradle scripts. The value of this property is the branch name in the GitHub repositories.
```
tagitGradleVersion = release/v7.4.3
```

### build.gradle
Read the raw content from the GitHub repository using the `tagitGradleVersion` property.

#### TagitCore Libraries

**From**
```
apply from: "${rootDir}/gradle/tagit.gradle"
```
**To**
```
apply from: "https://raw.githubusercontent.com/tagitmobile/tagit-core-gradle/${tagitGradleVersion}/gradle/tagit.gradle"
```

#### TagitCore Rest Web Applications

**From**
```
apply from: "${rootDir}/gradle/tagit-rest.gradle
```
**To**
```
apply from: "https://raw.githubusercontent.com/tagitmobile/tagit-core-gradle/${tagitGradleVersion}/gradle/tagit-rest.gradle"
```
#### Angular Web Applications

**From**
```
apply from: "${rootDir}/gradle/tagit-angular.gradle
```
**To**
```
apply from: "https://raw.githubusercontent.com/tagitmobile/tagit-core-gradle/${tagitGradleVersion}/gradle/tagit-angular.gradle"
```
#### Java Libraries
For publishing Maven artifacts:
**From**
```
apply from: "${rootDir}/gradle/publications.gradle"
```
**To**
```
apply from: "https://raw.githubusercontent.com/tagitmobile/tagit-core-gradle/${tagitGradleVersion}/gradle/publications.gradle"
```

## License
These Gradle Scripts are released under version 2.0 of the [Apache License](https://www.apache.org/licenses/LICENSE-2.0).
