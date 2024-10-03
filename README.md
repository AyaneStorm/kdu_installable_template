# kdu_installable_template
Template for the Kakadu installable package for SecondLife/Firestorm/Alchemy...

## Instructions
There is absolutely no instructions anywhere for building SecondLife / Firestorm / Alchemy with Kakadu, but I managed to do it. If you're interested, do as follow:
- Acquire a Kakadu license (can't help you with this, and won't share mine, it's private, please don't ask)
- Install the Visual Studio 2019 Build Tools and optionnaly Visual Studio itself (won't work with VS2022 build tools with current Kakadu version 8.4.1)
- Install Mingw64, make sure that you can create .tar.bz2 files.
- Extract Kakadu library somewhere
- In coresys directory you'll find a VS2019 solution and project
- Set coresys VS2019 project configuration to `<ConfigurationType>StaticLibrary</ConfigurationType>` instead of `<ConfigurationType>DynamicLibrary</ConfigurationType>`
- Set linker additional dependencies to:
    `..\srclib_ht\Win-x86-64\kdu_ht2019R.lib;%(AdditionalDependencies) for Release`
    `..\srclib_ht\Win-x86-64\kdu_ht2019D.lib;%(AdditionalDependencies) for Debug`
  (It is simpler to edit that in the project properties in Visual Studio, but that's optional)
- Prepare Kakadu sources for high speed Kakadu if your license allows it (see Enabling_HT.txt in Kakadu library)
- In VS 2019 build tools, cd to Kakadu coresys folder and type:
    `msbuild.exe coresys_2019.sln -t:coresys -p:Configuration="Release" -p:Platform="x64"` for Release, or
    `msbuild.exe coresys_2019.sln -t:coresys -p:Configuration="Debug" -p:Platform="x64"` for Debug
- You'll obtain a kdu_84R.lib or a kdu_84D.lib in coresys/../../bin_x64/
- Git clone this repository: https://github.com/AyaneStorm/kdu_installable_template
- In kdu-8.4.1-windows-233220159-template directory:
  - Copy and rename kdu_84R.lib to lib/Release/kdu_x64.lib
  - Copy and rename kdu_84D.lib to lib/Debug/kdu_x64.lib
  - Copy all the .h files of Kakadu into include/ (all, not only coresys headers)
  - Copy the Kakadu license into LICENSES/
- In Mingw64, type:
    `cd kdu-8.4.1-windows-233220159-template/ && tar -cvjSf kdu-8.4.1-windows-233220159.tar.bz2 *`
    `mv kdu-8.4.1-windows-233220159.tar.bz2 ../ && cd .. && md5sum kdu-8.4.1-windows-233220159.tar.bz2`
- You should obtain a `kdu-8.4.1-windows-233220159.tar.bz2` file and its MD5 checksum (keep that checksum for later)
- Go to you my_autobuild.xml file, and look for the `kdu` key.
- The Windows related part should look like this (change paths where needed):
    ```
    <key>windows64</key>
    <map>
    <key>archive</key>
    <map>
        <key>hash</key>
        <string>the MD5 hash you obtained previously</string>
        <key>hash_algorithm</key>
        <string>md5</string>
        <key>url</key>
        <string>file:///C:/path/to/kakadu/kdu-8.4.1-windows-233220159.tar.bz2</string>
    </map>
    <key>name</key>
    <string>windows64</string>
    </map>
    ```
- Just reconfigure with --kdu and recompile.
