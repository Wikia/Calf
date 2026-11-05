# Maven repository branch

This branch is a plain Maven repository, not source code. It holds builds of the
`feature/tab-bar-painter-images-and-appearance` branch of this fork, consumed by the Sidekick app
while the changes wait for an upstream Calf release:

```kotlin
maven("https://raw.githubusercontent.com/Wikia/Calf/maven") {
    content { includeGroup("com.mohamedrejeb.calf") }
}
```

Publishing a new version (on a Mac, iOS targets need Xcode), from the source branch checkout:

```bash
VERSION=<version> ANDROID_HOME=$HOME/Library/Android/sdk ./gradlew -I <path to this branch>/no-signing.init.gradle.kts \
  :calf-core:publishToMavenLocal :calf-ui:publishToMavenLocal
```

then in this branch:

```bash
rsync -a --exclude 'maven-metadata-local.xml' ~/.m2/repository/com/mohamedrejeb/calf/ com/mohamedrejeb/calf/
git add com && git commit -m "Publish <version>" && git push
```

`no-signing.init.gradle.kts` disables the PGP signing tasks the upstream build requires.
