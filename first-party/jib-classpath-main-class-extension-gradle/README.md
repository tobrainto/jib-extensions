# Jib Classpath Main-Class Extension

An extension to fetch the classpath and main class from an image entrypoint.

Often, you want to know the main class that Jib automatically inferred and/or the exact classpath string that Jib generated, so that you can construct your custom start-up command ([#xxx](#), [#xxx](#)) based on the values. This extension parses the entrypoint, identifies the values, and set the environment variables `JIB_JAVA_CLASSPATH` and `JIB_JAVA_MAIN_CLASS` in the image.

## Examples

Check out the [genenal instructions](../../README.md#using-jib-plugin-extensions) for applying a Jib plugin extension.

```gradle
// should be at the top of build.gradle
buildscript {
  dependencies {
    classpath('com.google.cloud.tools:jib-classpath-main-class-extension-gradle:0.1.0')
  }
}

...

jib {
  ...
  pluginExtensions {
    pluginExtension {
      implementation = 'com.google.cloud.tools.jib.gradle.extension.classpathmainclass.JibClasspathMainClassExtension'
    }
  }
}
```

## Future Work

In addition to fetching the classpath and main class into environment variables, the extension can have an additional feature to directly construct an entrypoint while using keywords. For example,

```gradle
  pluginExtensions {
    pluginExtension {
      implementation = 'com.google.cloud.tools.jib.gradle.extension.classpathmainclass.JibClasspathMainClassExtension'
      configuration {
        # Just for demonstration. Note Jib has `jib.container.extraClasspath` to prepend extra classpath.
        entrypoint = ['tini', '--', 'java', '-cp', '/prepended:JIB_JAVA_CLASSPATH:/appended', 'JIB_JAVA_MAIN_CLASS']
      }
    }
  }
```

or

```gradle
        entrypoint = ['sh', '-c', 'java -cp /prepended:JIB_JAVA_CLASSPATH:/appended JIB_JAVA_MAIN_CLASS']
```

Help wanted!
