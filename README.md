# Flutter ZXing

Flutter ZXing is a Flutter plugin for scanning and generating QR codes using the ZXing (Zebra Crossing) barcode scanning library. The plugin is implemented using the Dart FFI (Foreign Function Interface) and the [zxing-cpp](https://github.com/zxing-cpp/zxing-cpp) library, and allows you to easily integrate barcode scanning and generation functionality into your Flutter apps.

## Demo Screenshots

<pre>
<img alt="01_scanner_screen" src="https://user-images.githubusercontent.com/11523360/222677044-a15841a7-e617-44bb-b3a0-66b2d5b57dce.png" width="240">&nbsp; <img alt="02_creator_screen" src="https://user-images.githubusercontent.com/11523360/222677058-60a676fd-c229-4b51-8780-f40155cb5db6.png" width="240">&nbsp;
</pre>

## Features

- Scan QR codes and barcodes from the camera stream (on mobile platforms only), image file or URL
- Scan multiple barcodes at once from the camera stream (on mobile platforms only), image file or URL
- Generate QR codes with customizable content and size
- Return the position points of the scanned barcode
- Customizable scanner frame size and color, and ability to enable or disable features like torch and pinch to zoom

## Supported Formats

| Linear product | Linear industrial | Matrix             |
|----------------|-------------------|--------------------|
| UPC-A          | Code 39           | QR Code            |
| UPC-E          | Code 93           | Micro QR Code      |
| EAN-8          | Code 128          | rMQR Code          |
| EAN-13         | Codabar           | Aztec              |
| DataBar        | DataBar Expanded  | DataMatrix         |
|                | ITF               | PDF417             |
|                |                   | MaxiCode (partial) |

## Supported platforms

Flutter ZXing supports the following platforms:

- Android (minimum API level 21)
- iOS (minimum iOS 11.0)
- MacOS (minimum osx 10.15) (beta)
- Linux (working, but [no camera support](https://pub.dev/packages/camera)) (alpha)
- Windows (working, but [no camera support](https://pub.dev/packages/camera)) (alpha)
- Web (not working yet)

Note that flutter_zxing relies on the Dart FFI (Foreign Function Interface) feature, which is currently only available for the mobile and desktop platforms. As a result, the plugin is not currently supported on the web platform.

In addition, flutter_zxing uses the camera plugin to access the device's camera for scanning barcodes. This plugin is only supported on mobile platforms, and is not available for desktop platforms. Therefore, some features of flutter_zxing, such as scanning barcodes using the camera, are not available on desktop platforms.

## Generated by [ChatGPT](https://chat.openai.com/chat)

This README file was mainly generated using ChatGPT, a tool that generates human-like text.

## ZXScanner

ZXScanner is a free QR code and barcode scanner app for Android and iOS. It is built using Flutter and the [flutter_zxing](https://github.com/khoren93/flutter_zxing) plugin.

<p align="center">
    <a href="https://github.com/khoren93/flutter_zxing/tree/main/zxscanner">
        <img src="https://user-images.githubusercontent.com/11523360/178162663-57ec28ac-7075-43ab-ac31-35058298c73e.png" alt="ZXScanner logo" height="100" >
    </a>
</p>

## Getting Started

### Cloning the flutter_zxing project

To clone the flutter_zxing project from Github which includes submodules, use the following command:

```bash
git clone --recursive https://github.com/khoren93/flutter_zxing.git
```

### Installing dependencies

Use Melos to install the dependencies of the flutter_zxing project. Melos is a tool that helps you manage multiple Dart packages in a single repository. To install Melos, use the following command:

```bash
flutter pub global activate melos
```

To install the dependencies of the flutter_zxing project, use the following command:

```bash
melos bootstrap
```

To allow the building on iOS and MacOS, you need to run the following command:

```bash
cd scripts
sh update_ios_macos_src.sh
```

To run the integration tests:

```bash
cd example
flutter test integration_test
```

Now you can run the flutter_zxing example app on your device or emulator.

## Usage

### To read barcode

```dart
import 'package:flutter_zxing/flutter_zxing.dart';

// Use ReaderWidget to quickly read barcode from camera image
@override
Widget build(BuildContext context) {
  return Scaffold(
    body: ReaderWidget(
      onScan: (result) async {
        // Do something with the result
      },
    ),
  );
}

// Or use flutter_zxing plugin methods 
// To read barcode from camera image directly
await zx.startCameraProcessing(); // Call this in initState

cameraController?.startImageStream((image) async {
    Code result = await zx.processCameraImage(image);
    if (result.isValid) {
        debugPrint(result.text);
    }
    return null;
});

zx.stopCameraProcessing(); // Call this in dispose

// To read barcode from XFile, String, url or Uint8List bytes
XFile xFile = XFile('Your image path');
Code? resultFromXFile = await zx.readBarcodeImagePath(xFile);

String path = 'Your local image path';
Code? resultFromPath = await zx.readBarcodeImagePathString(path);

String url = 'Your remote image url';
Code? resultFromUrl = await zx.readBarcodeImageUrl(url);

Uint8List bytes = Uint8List.fromList(yourImageBytes);
Code? resultFromBytes = await zx.readBarcode(bytes);
```

### To create barcode

```dart
import 'package:flutter_zxing/flutter_zxing.dart';
import 'dart:typed_data';
import 'package:image/image.dart' as imglib;

// Use WriterWidget to quickly create barcode
@override
Widget build(BuildContext context) {
  return Scaffold(
    body: WriterWidget(
      onSuccess: (result, bytes) {
        // Do something with the result
      },
      onError: (error) {
        // Do something with the error
      },
    ),
  );
}

// Or use FlutterZxing to create barcode directly
final Encode result = zx.encodeBarcode(
    contents: 'Text to encode',
    params: EncodeParams(
        format: Format.QRCode,
        width: 120,
        height: 120,
        margin: 10,
        eccLevel: EccLevel.low,
    ),
);
if (result.isValid) {
    final img = imglib.Image.fromBytes(width, height, result.data);
    final encodedBytes = Uint8List.fromList(imglib.encodeJpg(img));
    // use encodedBytes as you wish
}
```

## License

MIT License. See [LICENSE](https://github.com/khoren93/flutter_zxing/blob/master/LICENSE).
