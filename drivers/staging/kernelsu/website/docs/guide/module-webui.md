# Module WebUI

In addition to executing boot scripts and modifying system files, KernelSU's modules also support displaying UI interfaces and interacting with users.

The module can write HTML + CSS + Javascript pages through any web technology. KernelSU's manager will display these pages through WebView. It also provides some APIs for interacting with the system, such as executing shell commands.

## webroot directory

Web resource files should be placed in the `webroot` subdirectory of the module root directory, and there **MUST** be a file named `index.html`, which is the module page entry. The simplest module structure containing a web interface is as follows:

```txt
❯ tree .
.
|-- module.prop
`-- webroot
    `-- index.html
```

:::warning
When installing the module, KernelSU will automatically set the permissions and SELinux context of this directory. If you don’t know what you are doing, please do not set the permissions of this directory yourself!
:::

If your page contains css and javascript, you need to place it in this directory as well.

## Javascript API

If it is just a display page, it is no different from a normal web page. More importantly, KernelSU provides a series of system API that allow you to implement the unique functions of the module.

KernelSU provides a Javascript library and [publishes it on npm](https://www.npmjs.com/package/kernelsu), which you can use in the javascript code of your web pages.

For example, you can execute a shell command to obtain a specific configuration or modify a property:

```javascript
import { exec } from 'kernelsu';

const { errno, stdout } = exec("getprop ro.product.model");
```

For another example, you can make the web page display full screen, or display a toast.

[API documentation](https://www.npmjs.com/package/kernelsu)

If you find that the existing API does not meet your needs or is inconvenient to use, you are welcome to give us suggestions [here](https://github.com/tiann/KernelSU/issues)!

## Some tips

1. You can use `localStorage` normally to store some data, but it will be lost after the Manager App is uninstalled. If you need to save persistently, you can write data to some directory yourself.
2. For simple pages, I recommend you use [parceljs](https://parceljs.org/) for packaging. It requires zero configuration and is very convenient to use. However, if you are a front-end master or have your own preferences, then just choose one you like!
