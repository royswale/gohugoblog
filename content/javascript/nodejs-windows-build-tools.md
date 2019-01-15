---
title: "nodejs windows build tools"
date: 2018-12-07T14:11:26+08:00
draft: false
---

https://github.com/nodejs/node-gyp
https://github.com/Microsoft/nodejs-guidelines/blob/master/windows-environment.md#compiling-native-addon-modules

open powershell with administrator privilege

```bash
npm i -g --production windows-build-tools
```

got this

```bash
Status from the installers:
---------- Visual Studio Build Tools ----------
2018-12-07T14:06:07 : Verbose : [InstalledProductsProviderImpl]: Stream was closed
2018-12-07T14:06:07 : Verbose : [InstallerImpl]: Rpc connection was closed.
2018-12-07T14:06:07 : Verbose : [InstallerImpl]: Stream was closed
2018-12-07T14:06:07 : Verbose : [SetupUpdaterImpl]: Rpc connection was closed.
2018-12-07T14:06:07 : Verbose : [SetupUpdaterImpl]: Stream was closed
------------------- Python --------------------
Python 2.7.15 is already installed, not installing again.


Could not install Visual Studio Build Tools.
```

https://www.bountysource.com/issues/65071541-could-not-install-visual-studio-build-tools

```bash
npm install --global --production windows-build-tools@4.0.0
```

![nodejs-windows-build-tools-20181211151150](http://qiniu.xingtan.xyz/nodejs-windows-build-tools-20181211151150.png)

work smoothly