---
layout: post
title: Cloud9 Native Desktop App on Windows
tags: Email SES AWS 
---

Cloud9 IDE is very productive even with its usage only through browser, best part is intergrated terminal for quickly checking on ec2 instances. As detailed in the earlier post on setting up AWS cloud9 environment as main IDE in cloud/browser, got to know cloud9 IDE running as MacOS native app , this  is based on Nativefier, which convert webapps to native apps as required for MacOS,Linux,Windows.


Here is the steps for  to convert Cloud9 IDE to Windows Native App using Nativefier,by which no need to remember the http link of IDE and App will be  conveniently available as native app on the taskbar and desktop experience.
 
1. Prepare your cloud9 env and obtain the IDE address (https://region.console.aws.amazon.com/cloud9/ide/XXX)
2. Login to IDE from browser, go into terminal mode (CTRL-T) and install natifier app by running (npm install nativefier -g)
3. go to /tmp path and validate natifier is working ( nativefier --help )
4. run the command 
    nativefier --single-instance --icon=./AWSCloud9.icns --internal-urls "" --name="AWS Cloud9" --disable-dev-tools --hide-window-frame --file-download-options '{"saveAs":true}' --platform="windows" --arch="x64" https://region.console.aws.amazon.com/cloud9/ide/XXX
5. Step 4 , will create AWS Cloud9-win32-x64  folder,  zip it for download (zip -r cloud9.zip AWS\ Cloud9-win32-x64)
6. copy the zip folder to S3 (aws s3 cp cloud9.zip <some-bucket-yours>)
7. Download the cloud9.zip from S3 bucket and extract the zip
8. create shortcut/pin to taskbar from the executable (AWS Cloud9.exe)


![Cloud9 Running in Browser](/assets/screenshots/cloud9_web.png):




![Cloud9 Running as Native Windows App](/assets/screenshots/cloud9_native.png):



