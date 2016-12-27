# node-red-bootup

Bundling (Application on macOS) for start Node-RED

## Limitations;

This package does not include Node-RED and Node.js.  
Install them yourself.

- [Node.js](https://nodejs.org/en/download/)
- [Node-RED](https://nodered.jp/docs/getting-started/installation.html) (`sudo npm install -g --unsafe-perm node-red`)

## Build this app;

- Required;
    - `convert` in ImageMagick (for Create ics file from SVG file)

```
$ mkdir Node-RED.app && cd Node-RED.app

$ mkdir -p Contents/{MacOS,Resources}

$ cat << EOT > Contents/Info.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple Computer//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
  <dict>
    <key>CFBundleExecutable</key>
    <string>boot.bash</string>
    <key>CFBundlePackageType</key>
    <string>APPL</string>
    <key>CFBundleIconFile</key>
    <string>app.icns</string>
    <key>CFBundleSignature</key>
    <string>????</string>
  </dict>
</plist>
EOT

$ cat << EOT > Contents/MacOS/boot.bash
#!/bin/bash
open -a Terminal "/usr/local/lib/node_modules/node-red/red.js"
sleep 5
open http://127.0.0.1:1880
EOT
$ chmod +x Contents/MacOS/boot.bash

$ mkdir app.iconset
$ curl -o node-red-icon-2.svg http://nodered.org/brand/media/node-red-icon-2.svg
$ for sz in 16 32 64 128 256 512 ; do convert -background none -resize ${sz}x${sz} node-red-icon-2.svg app.iconset/icon_${sz}x${sz}.png ; done
$ for sz in 16 32 64 128 256 512 ; do convert -background none -resize $((${sz}*2))x$((${sz}*2)) node-red-icon-2.svg app.iconset/icon_${sz}x${sz}@2x.png ; done
$ iconutil -c icns app.iconset
$ mv app.icns Contents/Resources/
$ rm -rf app.iconset node-red-icon-2.svg

$ cd ..
$ open .
```

## References;

- Mac OS XでPerlスクリプトをアプリケーション化する | 普通的生活 : https://www.nkozawa.com/blog/archives/2405
- 技術/MacOSX/シェルスクリプトを".app"("Bundle")化する - Glamenv-Septzen.net : http://www.glamenv-septzen.net/view/1201

## LICENSE;

MIT License  
Copyright (c) 2016 Kohei MATSUSHITA

EOT