<p align="center">
<h3 align="center">Varflex</h3>

------

<p align="center">
Save variables to files and load their value by object mapper. It is useful for people with basic understanding of java, gradle, workflow and is designed for lazy people
</p>

<p align="center">
<img alt="License" src="https://img.shields.io/github/license/CKATEPTb-minecraft/Varflex">
<a href="#Download"><img alt="Sonatype Nexus (Snapshots)" src="https://img.shields.io/nexus/s/dev.ckateptb.minecraft/Varflex?label=repo&server=https://repo.animecraft.fun/"></a>
<img alt="Publish" src="https://img.shields.io/github/workflow/status/CKATEPTb-minecraft/Varflex/Publish/production">
<a href="https://docs.gradle.org/7.5/release-notes.html"><img src="https://img.shields.io/badge/Gradle-7.5-brightgreen.svg?colorB=469C00&logo=gradle"></a>
<a href="https://discord.gg/P7FaqjcATp" target="_blank"><img alt="Discord" src="https://img.shields.io/discord/925686623222505482?label=discord"></a>
</p>

------

# Versioning

We use [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html) to manage our releases.

# Features

- [X] Easy to use
- [X] Automatic serialize
- [X] Automatic deserialize
- [X] Comment support
- [X] Yaml support
- [X] Xml support
- [X] Json support
- - [X] Hocon support
- - [X] Gson support
- - [X] Jackson support
- [X] Documented

# Download

Download from our repository or depend via Gradle:

```kotlin
repositories {
    maven("https://repo.animecraft.fun/repository/maven-snapshots/")
}

dependencies {
    implementation("dev.ckateptb.minecraft:Varflex:<version>")
}
```

# How To

* Import the dependency [as shown above](#Download)
* Add Varflex as a dependency to your `plugin.yml`
```yaml
name: ...
version: ...
main: ...
depend: [ Varflex ]
authors: ...
description: ...
```
* Create you config class with fields that should be saved (extends GsonConfig/HoconConfig/JacksonConfig/XmlConfig/YamlConfig)
```java
import dev.ckateptb.common.tableclothconfig.hocon.HoconConfig;
import lombok.Getter;
import lombok.Setter;
import org.spongepowered.configurate.objectmapping.meta.Comment;
import org.spongepowered.configurate.objectmapping.meta.Matches;
import org.spongepowered.configurate.objectmapping.meta.Setting;

import java.io.File;
import java.nio.file.Paths;
import java.util.List;

@Getter // Lombok's annotation. Generate Getters for all field
@Setter // Lombok's annotation. Generate Setters for all field
public class HoconExampleConfig extends HoconConfig {
    // Create field and set default value
    @Setting("example.custom.key") // Path for key separated by dot
    private List<String> list = List.of("Hello", "programmer", "world!");
    @Comment(value = "Wow, its comment", override = true)
    private boolean isDev = true;
    @Matches("^H") // Regex for matching
    private String helloWorldString = "Hello World";

    @Override
    public File getFile() { // File location (will be auto-created)
        return Paths.get("config", "config.conf").toFile();
    }
}
```
```toml
# Example auto-generated config file
example.custom.key=[
    "Hello",
    "programmer",
    "world!"
]
hello-world-string="Hello World"
# Wow, its comment
is-dev=true

```
* Initialize configuration class
```java
public class MyPlugin extends JavaPlugin {
    public void onEnable() {
        HoconExampleConfig config = new HoconExampleConfig(); // Create new instance
        config.load(); // Load saved config file if present
        config.save(); // Save changes to config file if changes present
    }
}
```
* Nothing more, easy to do.