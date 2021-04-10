1. Changes as per https://docs.moodle.org/dev/Setting_up_your_development_environment_for_Moodle_Mobile_2#Compiling_using_AOT

2. Change to the build.gradle to force minSdkversion 22
ext.cdvMinSdkVersion = 22
to build with android studio. This can also be done in config.xml


3. Changing all ~ and ^ package depencies in package.json (except cordova-android itself) to the exact package. Done by replacing
"~ with "
and
"^ with "
in package.json

or else, all sorts of dependency issues while building the 2019 release in 2021.

The build directory takes up more than 1 GB, lots of dependencies are downloaded. 

4. Customization - Changes as per the Configuration heading
https://web.archive.org/web/20201129221613/http://blog.vinodsingh.com/2020/05/how-to-customize-moodle-mobile-app.html

config.xml
reduced SplashScreenDelay
and SplashShowOnlyFirstTime true
also, very important,
content src="https://sssvidyavahini.org" would make the app like a simple webview.
We don't want that, so leave the content src as it is, 
but change the siteurl in src/config.json
and onlyallowlistedsites true.

android-minSdkVersion" value="22"

google-services.json - changed the package name

and src/config.json version, ensured it is different, but same number of digits.

5. Changing logo and splash screen as per above url and also resources/android/icon-background.png and icon-foreground.png
and in src dir assets/img

 

Build steps
-----------
Set up environment variables as per
https://cordova.apache.org/docs/en/10.x/guide/platforms/android/

JDK tar.gz needed oracle login, created as my official email

Set JAVA_HOME
https://docs.oracle.com/cd/E19182-01/821-0917/inst_jdk_javahome_t/index.html

in .bashrc, added the following.

export JAVA_HOME=/home/mac/Downloads/jdk1.8.0_281
export PATH=$JAVA_HOME/bin:$PATH
export ANDROID_SDK_ROOT=/home/mac/Android/Sdk
export PATH=$ANDROID_SDK_ROOT/tools:$ANDROID_SDK_ROOT/tools/bin:$ANDROID_SDK_ROOT/platform-tools:$PATH


https://cordova.apache.org/docs/en/10.x/guide/platforms/android/#opening-a-project-in-android-studio
Need to edit www folder outside android studio,
then copy over changes by doing cordova build - in our case, npx etc as below.

Installed node 11 last version as per https://www.sitepoint.com/quick-tip-multiple-versions-node-nvm/
nvm install 11.15.0

Then the following steps from
https://docs.moodle.org/dev/Setting_up_your_development_environment_for_Moodle_Mobile_2

sudo apt-get install libsecret-1-dev

Then, make the changes above. After that, see
steps to build.txt in root dir

nvm install node 11.15.0
# https://www.sitepoint.com/quick-tip-multiple-versions-node-nvm/
nvm use 11

sudo apt-get install libsecret-1-dev

npm install 
# cordova.plugins.diagnostic: Diagnostic plugin - ERROR: ENOENT: no such file or directory, open '/home/mac/StudioProjects/LMSappBuildTrial/config.xml'
# added 1829 packages from 1022 contributors and audited 1958 packages in 98.592s

npx cordova prepare 
# Current working directory is not a Cordova-based project.
# https://stackoverflow.com/questions/21276294/cordova-current-working-directory-is-not-a-cordova-based-project
mkdir www
npm install cordova
# config.xml was missing, so copied over with git
# No platforms added to this project. Please use `cordova platform add <platform>`.
npx ionic cordova platform add android --verbose
#Source path does not exist: resources/android/icon/drawable-hdpi-smallicon.png
#Error: Source path does not exist: resources/android/icon/drawable-hdpi-smallicon.png
# editing gitignore file to add the resources
npx cordova prepare
# ignoring the warning about conflict

npx gulp

npm start
# just to test - waiting till transpile started etc. then Ctrl+C
# Chrome doesn't open the http://localhost:8100 link as it tries https first - Firefox at least tries.

# https://docs.moodle.org/dev/Setting_up_your_development_environment_for_Moodle_Mobile_2#Compiling_using_AOT
cp -v "changes made to moodleapp files/inside node_modules dir/@angular/platform-browser-dynamic/esm5/platform-browser-dynamic.js" "node_modules/@angular/platform-browser-dynamic/esm5"
cp -v "changes made to moodleapp files/inside node_modules dir/@ionic/app-scripts/dist/util/config.js" "node_modules/@ionic/app-scripts/dist/util/config.js"
# edited gitignore to not ignore this config.js, did git add -f.

npm run ionic:build -- --prod
# this takes a lot of RAM and a lot of time. 
# if possible, avoid swapping by closing all apps and clearning memory before doing this.
# REMEMBER to use nvm use 11 if doing this in a fresh terminal
#PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND 
# 3682 mac       20   0 3142252 2.215g  28948 R 131.5 59.6   6:34.48 node
#   build prod finished in 3128.04 s
# npx cordova run android
# Could not find an installed version of Gradle either in Android Studio,
#or on your system to install the gradle wrapper. Please include gradle 
#in your path, or install Android Studio
# So, I just imported the platforms/android directory using the Import Gradle project option in Android Studio 4.1.3, and built from there.


Earlier missteps are noted below.
--------------------------------
npm install
#npx cordova prepare gave
#No platforms added to this project... so
#npx ionic cordova platform add android --verbose
#gave
#[WARN] cordova-res was not found on your PATH. Please install it globally:
#             npm i -g cordova-res
npm i -g cordova-res
npx ionic cordova platform add android --verbose
# Failed to restore plugin "cordova-plugin-inappbrowser". You might need to try adding it again. Error: CordovaError: Failed to fetch plugin git+https://github.com/#moodlemobile/cordova-plugin-inappbrowser.git#moodle via registry.

# maybe https://forum.ionicframework.com/t/failed-to-restore-plugin-cordova-plugin-statusbar-in-ionic-cordova-platform-add-android/108151/2
#But said Wrote custom version '27.1.0' to /home/mac/Downloads/LMSAppBuildTrial/platforms/android/app/build.gradle
#npx ionic cordova platform add android --nofetch might be useful at some time
npx cordova prepare
#Lots of failed to restore plugin errors.
# these are probably due to not using nvm use 11
npx gulp
npm start
# just to check if it is running, Ctrl+C to stop
npm run ionic:build -- --prod

(ignore the node sass error)


Then build from inside Android Studio by importing the platforms/android/app as in
https://cordova.apache.org/docs/en/10.x/guide/platforms/android/#opening-a-project-in-android-studio




