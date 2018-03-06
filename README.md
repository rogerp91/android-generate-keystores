# Android: Generate Release/Debug Keystores

## Generate Keystores

### Debug Keystore

```bash
$ keytool -genkey -v -keystore debug.keystore -storepass android -alias androiddebugkey -keypass android -keyalg RSA -keysize 2048 -validity 10000 -dname "C=US, O=Android, CN=Android Debug"
```

A debug keystore which is used to sign an Android app during development needs a specific alias and password combination as dictated by Google.

- Keystore name: "debug.keystore"
- Keystore password: "android"
- Key alias: "androiddebugkey"
- Key password: "android"
- CN: "CN=Android Debug,O=Android,C=US"

### Release Keystore

```bash
$ keytool -genkey -v -keystore release.keystore -alias alias_name -keyalg RSA -keysize 2048 -validity 10000
```

## Get Key Fingerprints

To get the keystore certificate fingerprints you can do:

```
keytool -list -v -keystore [keystore path] -alias [alias-name] -storepass [storepass] -keypass [keypass] 
```

Ex.:

```bash
$ keytool -list -v -keystore debug.keystore -alias androiddebugkey -storepass android -keypass android 
```

or 

```bash
$ keytool -list -v -keystore debug.keystore
```

## Verify APK signature

```bash
$ jarsigner -verify -verbose -certs my_application.apk
```

## Verify APK keystore signature

1. Unzip apk
2. Get MD5 and SHA1 of certificate of APK: `$ keytool -printcert -file ./META-INF/ANDROID_.RSA`
3. Get MD5 and SHA1 of keystore: `$ keytool -list -keystore signing-key.keystore`
4. Compare MD5/SHA1 and if they are the same, so the APK was signed with `signing-key.keystore`.

## Problem comes from latest version of Keytool in JDK8.151 and JDK9

```bash
$ keytool -J-Duser.language=en -exportcert -alias androiddebugkey -keystore debug.keystore -list -v -storepass android
$ keytool -J-Duser.language=en It was enough to force output in english by using the parameter -J-Duser.language=en
```

## Reference

- https://coderwall.com/p/r09hoq/android-generate-release-debug-keystores
- https://developer.android.com/studio/publish/app-signing.html
