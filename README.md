# Fastlane으로 빌드 자동화 설정하기

외주 개발을 하게 되면서 서로 다른 애플 개발자 계정으로 enterprise, adhoc빌드, 스토어 업로드를 처리하다보니

빌드를 내는데 걸리는 시간이 많아졌고, 상당히 귀찮은 작업이 되었다.

당시 개발하던 mac의 사양도 높지 않아서 빌드속도도 느릴뿐더러 xcode 스토어 업로드에서 마지막에 알수없는 에러가 발생하였는데

한시간 넘게 계속 재시도하니까 업로드에 성공한적도 있었다.

fastlane을 사용하면 이러한 문제도 해결할 수 있고, 추가적으로 좋은 기능들도 이용할 수 있다.
  
## Overview
- fastlane 설치 및 파일설명
- Appfile
- .env
- Fastfile

- Action

- Plugin

- build multiple targets with a single command
- get version string
- slack message
- appicon with badge
- upload ipa and plist to the server


##fastlane 설치

1. 터미널에서 다음 명령어를 입력한다.

sudo gem install fastlane --verbose`

2. 해당 프로젝트로 이동한 후 다음 명령어를 입력한다.

`fastlane init`

3. 생성된 Appfile, Fastfile, .env파일을 작성한다.

Appfile에서 애플 개발자 계정을 설정하고 Fastfile로 빌드할 lane을 작성한다.(가장 중요!) 

.env파일은 전역변수를 설정하여 Fastfile에서 편리하게 사용할 수 있다.

 
##파일 설정

**Appfile**

여러 개발자계정으로 빌드를 할 경우에 Fastfile에서 설정한 lane이름으로 분기하여 id를 설정할 수 있다.

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

각각의 프로젝트에서 Fastfile을 다시 작성하는것보다 .env파일을 사용하여 프로젝트이름이나 url등을 변수로 선언하면

약간의 Fastfile을 수정하는것만으로 다른 프로젝트에서도 용이하게 사용할 수 있다.


```
  SLACK_URL=""  # slack hook address
 
  RELEASE_TARGET=""
  ADHOC_TARGET=""
  ENTERPRISE_TARGET=""
 
  # git checkout for removing badge
  BADGE_REMOVE_COMMIT="../{PROJECT_LOCATION}/Assets.xcassets/AppIcon.appiconset"
```

**Fastfile**

Fastfile은 lane시작 전에 수행할 코드를 넣는 before_all do ~ end과 여러 lane들 그리고, lane작업 후에 호출되는 

after_all do ~ end과 error do ~ end로 구성할 수 있다.

보통 before_all에 인증서 관련한 명령과 pod install수행 구문들을 넣어주고 after_all이나 error 에 슬랙 메시지, git관련 명령
들을 수행하면 된다.


## Action
Action은 Fastlane에서 기본적으로 내장되어있는 기능으로 test, build, screenshot, code signing등이 있다.
https://docs.fastlane.tools/actions/


## Plugin
플러그인은 부가적인 option이며 fastlane에서 자체적으로 제공하는것이 아니라 오픈소스 컨트리뷰터들이 만들어서 제공하는 기능이다.

사용하려면 'fastlane add_plugin [plugin명]' 으로 따로 설치해야한다.

fastlane은 루비로 되어있어서 루비를 잘 다룬다면 plugin을 만들어 쓸 수 있을 것 같다.

즐겨쓰는 plugin으로는 badge 인데 앱 아이콘이 비슷해서 헷갈리지 않아서 유용하다.
https://github.com/HazAT/fastlane-plugin-badge

```
fastlane add_plugin badge
```



## 빌드실행

설정이 끝나고 터미널에서 `fastlane [lane명]` 을 입력하면 빌드가 된다. 
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

