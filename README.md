## Fastlane iOS Example

## Overview
- use different apple account
- build multiple targets with a single command
- get version string
- slack message
- appicon with badge
- upload ipa and plist to the server


## Setting

**Appfile**
If you are using different apple accounts, set Appfile.
You can register an ID for each lane.
it will match with lane in Fastfile.
```
for_lane :first_lane do
     app_identifier "ONE"
     apple_id "ONE"
     team_id "ONE"
end

for_lane :second_lane do
     app_identifier "TWO"
     apple_id "TWO"
     team_id "TWO"
end
```


**.env**
if you subscript environment values in .env file, you can access in Fastfile.
```
  SLACK_URL=""  # slack hook address
 
  RELEASE_TARGET=""
  ADHOC_TARGET=""
  ENTERPRISE_TARGET=""
 
  # git checkout for removing badge
  BADGE_REMOVE_COMMIT="../{PROJECT_LOCATION}/Assets.xcassets/AppIcon.appiconset"
```

**.exp**
upload ipa and manifest to server using shell script.
example use pem file to connect to AWS EC2.


## Installation


if you want to add badge on app icon, install badge plugin
```
fastlane add_plugin badge
```

## Usage
#### ios all
make enterprise ipa, adhoc ipa and upload App
```
fastlane ios all
```

#### ios release
Deploy a new version to the App Store

```
fastlane ios release
```

#### ios adhoc
Ad-Hoc Build
```
fastlane ios adhoc
```


#### ios enterprise
Enterprise Build
```
fastlane ios enterprise
```


## About Fastlane
For more information about Fastlane -> https://fastlane.tools/

Fastlane github -> https://github.com/fastlane/fastlane

Fastlane plugin -> https://docs.fastlane.tools/plugins/available-plugins/

