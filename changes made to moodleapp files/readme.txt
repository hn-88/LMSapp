1. Changes as per https://docs.moodle.org/dev/Setting_up_your_development_environment_for_Moodle_Mobile_2#Compiling_using_AOT

2. Change to the build.gradle to force minSdkversion 22
ext.cdvMinSdkVersion = 22
to build with android studio.


3. Changing all ~ and ^ package depencies in package.json (except cordova-android itself) to the exact package. Done by replacing
"~ with "
and
"^ with "
in package.json

or else, all sorts of dependency issues while building the 2019 release in 2021.

The build directory takes up more than 1 GB, lots of dependencies are downloaded. 
