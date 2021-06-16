# Forge-Class-Remapper
**Forge-Class-Remapper** helps you remap your mods to use Mojang classnames on 1.17+.
This is intended is for helping you update your mods from 1.16 or lower to 1.17 or higher.
Forge 1.17 moved to using Mojang classnames as Mojang mappings are now the default mappings used with Forge.
For more information, see [here](https://github.com/MinecraftForge/MCPConfig/blob/master/Mojang.md).
This script only automates what would otherwise be very hard to do manually for large-scale mods.

## How to use this

### 1. Make a backup!
This script changes your mod's sourcecode.
You should make a backup in case anything goes wrong.
If not already, using Git or another version control system is highly recommended, even if private.
You have been warned.

### 2. Update your mappings to `official` (if not already)
This script only remaps classnames and *not* methods or fields.
For best results, you should be using `official` mappings in ForgeGradle before running this script using the **old** version of Minecraft.
For example, on 1.16.5, this can be achieved by running
```groovy
gradlew -PUPDATE_MAPPINGS_CHANNEL="official" -PUPDATE_MAPPINGS="1.16.5" updateMappings
```
Then, change the `mappings` line in your `build.gradle` to be
```groovy
    mappings channel: 'official', version: '1.16.5'
```

### 3. Add this line to your `build.gradle`
This script can be used by adding the following line to your `build.gradle`:
```groovy
apply from: 'https://raw.githubusercontent.com/SizableShrimp/Forge-Class-Remapper/main/classremapper.gradle'
```
This should be near the top of the file below other plugins. For example:
```groovy
// Only edit below this line, the above code adds and enables the necessary things for Forge to be setup.
apply plugin: 'eclipse'
apply plugin: 'maven-publish'
apply from: 'https://raw.githubusercontent.com/SizableShrimp/Forge-Class-Remapper/main/classremapper.gradle'
```

### 4. Run this command
After adding the above line, you are now ready to update your classnames by running a Gradle task. The gradle task can be run as follows:
```groovy
gradlew -PDUPATE_CLASSNAMES=true updateClassnames
```
By default, this will only apply to the `main` sourceset. 
If you have an `api` sourceset or other sourcesets, you can add these as well using the `UPDATE_SOURCESETS` property.
This is a semicolon-separated list that can be defined, for example, as `-PUPDATE_SOURCESETS=main;api` and would be inserted into the above command.

### 5. Update your mappings version
You should now update your mappings `version` to match the Minecraft version you are updating to.
For example, if you are now developing for 1.17.0, your `mappings` line should look like this:
```groovy
    mappings channel: 'official', version: '1.17'
```

### 6. Done!
Afterwards, your mod should now be on Mojang classnames!
You can now delete the `apply from:` line from your `build.gradle`, and continue updating your mod.
Happy modding!

*If you have any issues, feel free to contact me on Discord @ `SizableShrimp#0755` or ask in [the Forge discord](https://discord.gg/UvedJ9m).*