# angel_static

[![version 1.1.4](https://img.shields.io/badge/pub-1.1.4-brightgreen.svg)](https://pub.dartlang.org/packages/angel_static)
[![build status](https://travis-ci.org/angel-dart/static.svg?branch=master)](https://travis-ci.org/angel-dart/static)

Static server middleware for Angel.

# Installation
In `pubspec.yaml`:

```yaml
dependencies:
    angel_static: ^1.1.0
```

# Usage
To serve files from a directory, your app needs to have a
`VirtualDirectory` mounted on it.

```dart
import 'dart:io';
import 'package:angel_framework/angel_framework.dart';
import 'package:angel_static/angel_static.dart';

main() async {
  final app = new Angel();

  // Normal static server
  await app.configure(new VirtualDirectory(source: new Directory('./public')));

  // Send Cache-Control, ETag, etc. as well
  await app.configure(new CachingVirtualDirectory(source: new Directory('./public')));

  await app.startServer();
}
```

# Options
The `VirtualDirectory` API accepts a few named parameters:
- **source**: A `Directory` containing the files to be served. If left null, then Angel will serve either from `web` (in development) or
    `build/web` (in production), depending on your `ANGEL_ENV`.
- **indexFileNames**: A `List<String>` of filenames that should be served as index pages. Default is `['index.html']`.
- **publicPath**: To serve index files, you need to specify the virtual path under which
    angel_static is serving your files. If you are not serving static files at the site root,
    please include this.
- **debug**: Print verbose debug output.
- **callback**: Runs before sending a file to a client. Use this to set headers, etc. If it returns anything other than `null` or `true`,
then the callback's result will be sent to the user, instead of the file contents.
- **streamToIO**: If set to `true`, files will be streamed to `res.io`, instead of added to `res.buffer`.. Default is `false`.