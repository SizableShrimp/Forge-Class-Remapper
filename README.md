# Forge-Class-Remapper
**Forge-Class-Remapper** helps you remap your Forge mod to use Mojang classnames on 1.17+ or MCP classnames on 1.16 and lower.
This is intended is for helping you update your mod from 1.16 or lower to 1.17 or higher, or in the opposite direction.
Forge 1.17 moved to using Mojang classnames as Mojang mappings are now the default mappings used with Forge.
Forge 1.16 and lower uses MCP classnames.
For more information, see [here](https://github.com/MinecraftForge/MCPConfig/blob/master/Mojang.md).
This script only automates what would otherwise be very hard to do manually, **remap ALL the classnames for you.**
This script also remaps your access transformer files.

## Note: You MUST run this BEFORE updating/downgrading your Forge version
This script has to be the first thing you run, otherwise it won't work.

## How to use this

### 1. Make a backup!
This script changes your mod's sourcecode.
You should make a backup in case anything goes wrong.
If not already, using Git or another version control system is highly recommended, even if private.
You have been warned.

### 2. Update your ForgeGradle version (if not already)
This script uses tasks and other things from ForgeGradle 5. You must be on ForgeGradle 5 for this to work correctly.
Update this line in your `build.gradle` and change the old number to `5.1.+`.
```groovy
        classpath group: 'net.minecraftforge.gradle', name: 'ForgeGradle', version: '5.1.+', changing: true
```
Go to `gradle/wrapper/gradle-wrapper.properties` and change the line starting with `distributionUrl` to be:
```properties
distributionUrl=https\://services.gradle.org/distributions/gradle-7.3.1-bin.zip
```

### 3. Update your mappings to `official` (if not already)
This script only remaps classnames and *not* methods or fields.
For best results, you should be using `official` mappings in ForgeGradle before running this script using the **old** version of Minecraft.
For example, on 1.16.5, this can be achieved by running the following command:
```groovy
gradlew -PUPDATE_MAPPINGS_CHANNEL="official" -PUPDATE_MAPPINGS="1.16.5" updateMappings
```
Then, change the `mappings` line in your `build.gradle` to this line:
```groovy
    mappings channel: 'official', version: '1.16.5'
```

### 4. Add this line to your `build.gradle`
This script can be used by adding the following line to your `build.gradle`:
```groovy
apply from: 'https://raw.githubusercontent.com/SizableShrimp/Forge-Class-Remapper/main/classremapper.gradle'
```
This should be near the top of the file below ALL other plugins. Here is an example:
```groovy
apply plugin: 'net.minecraftforge.gradle'
// Only edit below this line, the above code adds and enables the necessary things for Forge to be setup.
apply plugin: 'eclipse'
apply plugin: 'maven-publish'
apply from: 'https://raw.githubusercontent.com/SizableShrimp/Forge-Class-Remapper/main/classremapper.gradle'
```

### 5. Run this command
After adding the above line, you are now ready to update your classnames by running a Gradle task. The gradle task can be run as follows:
```groovy
gradlew -PUPDATE_CLASSNAMES=true updateClassnames
```
By default, this will only apply to the `main` sourceset. 
If you have an `api` sourceset or other sourcesets, you can add these as well using the `UPDATE_SOURCESETS` property.
This is a semicolon-separated list that can be defined, for example, as `-PUPDATE_SOURCESETS="main;api"` and would be inserted into the above command.

If you are **backporting** a mod from 1.17 or later to 1.16 or earlier, add `-PREVERSED=true`. This will convert the classnames from Mojang back to MCP.

### 6. Update your mappings version
You should now update your mappings `version` to match the Minecraft version you are updating to.
For example, if you are now developing for 1.17.1, your `mappings` line should look like this:
```groovy
    mappings channel: 'official', version: '1.17.1'
```
You should now also update your Forge and ForgeGradle versions, if not already.

### 7. Done!
Afterwards, your mod should now have new classnames!
You can now delete the `apply from:` line from your `build.gradle` and continue updating/backporting your mod.
Happy modding!

*If you have any issues, feel free to contact me on Discord @ `SizableShrimp#0755` or ask in [the Forge discord](https://discord.gg/UvedJ9m).*
