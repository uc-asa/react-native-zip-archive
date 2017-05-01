# React Native Zip Archive [![npm](https://img.shields.io/npm/dm/react-native-zip-archive.svg)](https://www.npmjs.com/package/react-native-zip-archive) [![npm](https://img.shields.io/npm/v/react-native-zip-archive.svg)](https://www.npmjs.com/package/react-native-zip-archive)

Zip archive utility for react-native

## Manual Installation
Fos iOS

Open you project in xcode and also open folder node_modules/react-native-zip-archive
image

Open your build phase of project then goto add link binary with libraries
image
image

Clean your project and then build it will work.

Fos android

Within your project android/settings.gradle add these lines in the bottom
include ':react-native-zip-archive'
project(':react-native-zip-archive').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-zip-archive/android')
image

Open MainActivity.java and make changes
image

Within your project android/app/build.gradle add dependencies.
compile project(':react-native-zip-archive')
image


## Installation

```bash
npm install react-native-zip-archive --save
react-native link react-native-zip-archive
```

## Usage

import it into your code

```js
import { zip, unzip, unzipAssets, subscribe } from 'react-native-zip-archive'
```

you may also want to use something like [react-native-fs](https://github.com/johanneslumpe/react-native-fs) to access the file system (check its repo for more information)

```js
import { MainBundlePath, DocumentDirectoryPath } from 'react-native-fs'
```

## API

**zip(source: string, target: string): Promise**

> zip source to target

Example

```js
const targetPath = `${DocumentDirectoryPath}/myFile.zip`
const sourcePath = DocumentDirectoryPath

zip(sourcePath, targetPath)
.then((path) => {
  console.log(`unzip completed at ${path}`)
})
.catch((error) => {
  console.log(error)
})
```

**unzip(source: string, target: string): Promise**

> unzip from source to target

Example

```js
const sourcePath = `${DocumentDirectoryPath}/myFile.zip`
const targetPath = DocumentDirectoryPath

unzip(sourcePath, targetPath)
.then((path) => {
  console.log(`unzip completed at ${path}`)
})
.catch((error) => {
  console.log(error)
})
```

**unzipAssets(assetPath: string, target: string): Promise**

> unzip file from Android `assets` folder to target path

*Note: Android only.*

`assetPath` is the relative path to the file inside the pre-bundled assets folder, e.g. `folder/myFile.zip`. Do not pass an absolute directory.

```js
const assetPath = `${DocumentDirectoryPath}/myFile.zip`
const targetPath = DocumentDirectoryPath

unzipAssets(assetPath, targetPath)
.then(() => {
  console.log('unzip completed!')
})
.catch((error) => {
  console.log(error)
})
```

**subscribe(callback: ({progress: number})): EmitterSubscription**

> Subscribe to unzip progress callbacks. Useful for displaying a progress bar on your UI during the unzip process.

Your callback will be passed an object with the following fields:

- `progress` (number)  a value from 0 to 1 representing the progress of the unzip method. 1 is completed.

*Note: Remember to unsubscribe! Run .remove() on the object returned by this method.*

```js
componentWillMount() {
  this.zipProgress = subscribe((e) => {
    this.setState({ zipProgress: e.progress })
  })
}

componentWillUnmount() {
  // Important: Unsubscribe from the progress events
  this.zipProgress.remove()
}
```
