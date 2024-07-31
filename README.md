# react-native-read-nfc-passport

Read data from NFC passport, Vietnam CCCD for iOS and Android, reference from `NFCPassportReader` and `jmrtd`

## Installation

```sh
npm install react-native-read-nfc-passport
```

```sh
yarn add react-native-read-nfc-passport
```

## Configuration

### IOS

1. In [apple developer site](https://developer.apple.com/), enable capability for NFC

![enable capability](./images/enable-capability.png "enable capability")

2. in Xcode, add `NFCReaderUsageDescription` into your `info.plist`, for example:

```
<key>NFCReaderUsageDescription</key>
<string>We need to use NFC</string>
```

More info on Apple's [doc](https://developer.apple.com/documentation/bundleresources/information_property_list/nfcreaderusagedescription?language=objc)

Additionally, if writing ISO7816 tags add application identifiers (aid) into your `info.plist` as needed like this.
```
<key>com.apple.developer.nfc.readersession.iso7816.select-identifiers</key>
<array>
  <string>A000000151000000</string>
  <string>D2760000850100</string>
  <string>D2760000850101</string>
</array>
```

3. in Xcode's `Signing & Capabilities` tab, make sure `Near Field Communication Tag Reading` capability had been added, like this:

![xcode-add-capability](./images/xcode-capability.png "xcode capability")

If this is the first time you toggle the capabilities, the Xcode will generate a `<your-project>.entitlement` file for you:

![xcode-add-entitlement](./images/xcode-entitlement.png "xcode entitlement")

4. in Xcode, review the generated entitlement. It should look like this:

![edit entitlement](./images/edit-entitlement.png "edit entitlement")

More info on Apple's [doc](https://developer.apple.com/documentation/bundleresources/entitlements/com_apple_developer_nfc_readersession_formats?language=objc)

5. in Project Xcode, install Package Dependencies NFCPassportReader (Use version 1.1.9)

![package dependencies](./images/package-dependencies.png "package dependencies")

Add (+) NFCPassportReader in `TARGETS` => `Build phases` => `Link Binary With Libraries`

![link binary](./images/link-binary.png "link binary")

### Android

Simple add `uses-permission` into your `AndroidManifest.xml`:

```xml
 <uses-permission android:name="android.permission.NFC" />
```

## Usage

### Android

```js
import { scanNfcAndroid, cancelScanNfcAndroid } from 'react-native-read-nfc-passport';

// documentNumber: Last 9 digits of cccd
// dateOfBirth: yymmdd
// dateOfExpiry: yymmdd

const onReadNfc = async () => {
  try {
    const data = await scanNfcAndroid({
      documentNumber: 'xxxxxxxxx',
      dateOfBirth: 'xxxxxx',
      dateOfExpiry: 'xxxxxx',
    });
    console.log('readPassport', data);
  } catch (error) {
    try {
      await cancelScanNfcAndroid();
    } catch (err) {
      console.log(err);
    }
  }
};
```

### IOS

```js
import { scanNfcIos } from 'react-native-read-nfc-passport';

// documentNumber: Last 9 digits of cccd
// dateOfBirth: yymmdd
// dateOfExpiry: yymmdd

const onReadNfc = async () => {
  try {
    const data = await scanNfcIos({
      documentNumber: 'xxxxxxxxx',
      dateOfBirth: 'xxxxxx',
      dateOfExpiry: 'xxxxxx',
    });
    console.log('readPassport', data);
  } catch (error) {
      console.log(error);
  }
};
```

## License

MIT

---
