# CodePush Server [source](https://github.com/lisong/code-push-server)

[![NPM](https://nodei.co/npm/code-push-server.svg?downloads=true&downloadRank=true&stars=true)](https://nodei.co/npm/code-push-server/)

[![NPM Version](https://img.shields.io/npm/v/code-push-server.svg)](https://npmjs.org/package/code-push-server)
[![Node.js Version](https://img.shields.io/node/v/code-push-server.svg)](https://nodejs.org/en/download/)
[![Linux Status](https://img.shields.io/travis/lisong/code-push-server/master.svg?label=linux)](https://travis-ci.org/lisong/code-push-server)
[![Windows Status](https://img.shields.io/appveyor/ci/lisong/code-push-server/master.svg?label=windows)](https://ci.appveyor.com/project/lisong/code-push-server)
[![Coverage Status](https://img.shields.io/coveralls/lisong/code-push-server/master.svg)](https://coveralls.io/github/lisong/code-push-server)
[![Dependency Status](https://img.shields.io/david/lisong/code-push-server.svg)](https://david-dm.org/lisong/code-push-server)
[![Known Vulnerabilities](https://snyk.io/test/npm/code-push-server/badge.svg)](https://snyk.io/test/npm/code-push-server)
[![Licenses](https://img.shields.io/npm/l/code-push-server.svg)](https://spdx.org/licenses/MIT)

CodePush Server is a CodePush progam server! We can use this to build our's. We can use cloud storage to store the files, because it's simple and quick! Or you can use [local/s3/oss] storage, just modify config.js file, it's simple configure.

## Correct use of code-push hot update

-   Apple App allows hot updates[Apple's developer agreement](https://developer.apple.com/programs/information/), In order not to affect the user experience, it is stipulated that silent updates must be used. Google Play can't use silent updates, and you must have a box to tell users that the app has an update. China's android market must use silent update (if the box prompts, the app will be rejected by "please upload the latest version of the binary application package").
-
-   react-native has different bundle for different platforms. When using code-push-server, each platform must have distinguish application(eg. CodePushDemo-ios and CodePushDemo-android)
-   react-native-code-push only update the resource files, not Java and Objective C，so when you update the core system and need to change the version(ios modify the CFBundleShortVersionString in Info.plist, android modify versionName in build.gradle), the recompile the app to the app store.
-   It is recommended to use code-push release-react to publish the application(eg. code-push release-react CodePushDemo-ios ios -d Production)
-   Each time you submit a new version to App Store，you should also publish an initial version to code-push-server based on the submitted version. (Because each time you release a version to code-push-server，code-puse-server will be compared with the initial version to generate a patch version)

### start code-push-server

```shell
$ cd docker
$ docker-compose up -d # or docker-compose start
$ docker-compose exec server yarn start # or yarn dev
```

### shell login

```shell
$ code-push login http://localhost:3000
```

### [web](http://www.code-push.com)

access：http://localhost:3000

### client eg.

[ReactNative CodePushDemo](https://github.com/lisong/code-push-demo-app)

[Cordova CodePushDemo](https://github.com/lisong/code-push-cordova-demo-app)

## HOW TO INSTALL code-push-server

-   [docker](./docker/README.md) (recommend)
-   [manual operation](./docs/README.md)

## HOW TO USE

-   [react-native-code-push](https://github.com/Microsoft/react-native-code-push)
-   [code-push](https://github.com/Microsoft/code-push)

## ISSUES

[code-push-server normal problem](https://github.com/lisong/code-push-server/issues/135)

## License

MIT License [read](https://github.com/lisong/code-push-server/blob/master/LICENSE)
