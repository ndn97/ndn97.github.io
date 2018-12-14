---
layout: post
title: Cloud9 Desktop App for Windows
tags: Cloud9 AWS Desktop
---

Cloud9 IDE is very productive even with its usage only through browser, its intergrated terminal for quickly running commands and manage AES environment is helpful. As detailed in the earlier [post](https://nageshdn.com/2018/12/04/Cloud9-Environment-Setup.html) on setting up AWS cloud9 environment as main IDE in cloud/browser, got to know cloud9 IDE running as MacOS native app , this  is based on [Nativefier](https://github.com/jiahaog/nativefier), which convert web apps to native apps as required for MacOS,Linux,Windows.


Here are the steps to convert Cloud9 IDE to Windows native App using Nativefier,by which no need to remember the http link of IDE and app will be conveniently available as native app on the taskbar/desktop for improved desktop experience.
 
1. Prepare your cloud9 env and obtainIDE address https://region.console.aws.amazon.com/cloud9/ide/XXX
2. Login to IDE from browser, go into terminal mode (CTRL-T) and install natifier app by running (npm install nativefier -g)
3. go to /tmp path and validate natifier is working ( nativefier --help )
4. run the command for windows,64 bit OS
    nativefier --single-instance --icon=./AWSCloud9.icns --internal-urls "" --name="AWS Cloud9" --disable-dev-tools --hide-window-frame --file-download-options '{"saveAs":true}' --platform="windows" --arch="x64" https://region.console.aws.amazon.com/cloud9/ide/XXX
5. Step 4 , will create AWS Cloud9-win32-x64  folder,  zip it for download (zip -r cloud9.zip AWS\ Cloud9-win32-x64)
6. copy the zip folder to S3 (aws s3 cp cloud9.zip <some-bucket-you-own>)
7. Download cloud9.zip from S3 bucket and extract zip
8. create shortcut/pin to taskbar from the executable (AWS Cloud9.exe)


Cloud9 Running in Browser:
![Cloud9 Running in Browser](/assets/screenshots/cloud9_web.png)

Cloud9 Running as Native Windows App:
![Cloud9 Running as Native Windows App](/assets/screenshots/cloud9_native.png):



