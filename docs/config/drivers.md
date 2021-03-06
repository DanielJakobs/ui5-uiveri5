# Drivers
All supported browsers need the respective webdrivers. When using local execution (seleniumAddress not provided), the respective webdriver and possibly selenium.jar is downloaded automatically on every execution and is kept in the 'selenium' folder under the installation path.
The respective webdriver version is specified in basic profile and can be overwritten on command line:
```
$ uiveri5 --config.connectionConfigs.direct.binaries.selenium.version=3.11
```
and in conf.js:
 ```javascript
connectionConfigs: {
    direct: {
        binaries: {
            selenium: {
                version: "3.11"
            }
        }
    }
}
```
## Generic webdriver options
Browser size and location can be specified in browsers.capabilities.remoteWebDriverOptions. The following are listed in descending priority:
* maximized - maximizes the browser window
* position - offset of the browser relative to the upper left screen corner
* viewportSize - inner size of the browser window (actual page display area)
* browserSize - outer size of the browser window (including window toolbars)

```javascript
browsers: [{
  capabilities: {
    remoteWebDriverOptions: {
      maximized: true,
      position: {
        x: 0,
        y: 0
      },
      viewportSize: {
        width: 1920,
        height: 1067
      },
      browserSize: {
        width: 1920,
        height: 1067
      }
    }
  }
}]
```

######  Maximize option may be not supported for all browsers. In such case "browserSize" option can be used for setting browser window size. Example: ######
```javascript
browsers: [{
  capabilities: {
    remoteWebDriverOptions: {
      maximized: false,
      browserSize: {
        width: 1920,
        height: 1067
      }
    }
  }
}]
```

## Selenium
By default, the respective webdriver that starts the required browser is started directly. It could be started by using selenium jar, just have to enable it with setting _useSeleniumJar_ to true. Then selenium command line arguments could be provided in conf.js:
```javascript
browsers: [{
  browserName: 'chrome',
  capabilities: {
    seleniumOptions: {
        args: ['-debug', '-log','selenium.log']
    },
  }
}]
```
List the available arguments by executing:
```
$ java -jar selenium-server-standalone-3.0.1.jar -help
```

## Chrome
Chrome uses the chromedriver that is updated regularly so by default we use the latest version.
All chromedriver options from: [ServiceBuilder](https://github.com/SeleniumHQ/selenium/blob/selenium-3.6.0/javascript/node/selenium-webdriver/chrome.js) could be specified under the 'chromedriverOptions' key.
All chrome options from: [Options](https://github.com/SeleniumHQ/selenium/blob/selenium-3.6.0/javascript/node/selenium-webdriver/chrome.js) could be specified under the 'chromeOptions' key.
```javascript
browsers: [{
  browserName: 'chrome',
  capabilities: {
    chromedriverOptions: {
      loggingTo: ['chromedriver.log']
    },
    chromeOptions: {
      args: ['start-maximized']
    }
  }
}]
```


## Firefox
Firefox uses the geckodriver that is updated regularly so by default we use the latest version.
All geckodriverdriver options from: [ServiceBuilder](https://github.com/SeleniumHQ/selenium/blob/selenium-3.6.0/javascript/node/selenium-webdriver/firefox/index.js) could be specified under the 'geckodriverOptions' key.
All firefox options from: [Options](https://github.com/SeleniumHQ/selenium/blob/selenium-3.6.0/javascript/node/selenium-webdriver/firefox/index.js) could be specified under the 'firefoxOptions' key.
```javascript
browsers: [{
  browserName: 'firefox',
  capabilities: {
    geckodriverOptions: {
      setFirefoxBinary: ['/path/to/firefox']
    },
    firefoxOptions: {
      addArguments: ['-private']
    }
  }
}]
```
Geckodriver expects to find firefox executable on the system path or at the default location for respective platform. In some installation or upgrade scnearious, it is possible that firefox binary is placed in different location and geckodriver would not be able to find it. One workaround is to add the path to the binary in the PATH environment variable. Another workaround is to provide the path to the firefox binary in the geckodriverOptions.

## Internet Explorer
Internet explorer uses the iedriver which is only available on Windows. Currently, you need to specify an exact version (automatic latest version detection is not implemented).
All iedriver options from: [ServiceBuilder](https://github.com/SeleniumHQ/selenium/blob/master/javascript/node/selenium-webdriver/ie.js) could be specified under the 'iedriverOptions' key.
All IE options from: [Options](https://github.com/SeleniumHQ/selenium/blob/master/javascript/node/selenium-webdriver/ie.js) could be specified under the 'ieOptions' key.
There is a [browser configuration](https://github.com/SeleniumHQ/selenium/wiki/InternetExplorerDriver#required-configuration) that should be followed before you start testing on IE. It is preferable to modify your browser's security settings as described [here](https://github.com/seleniumQuery/seleniumQuery/wiki/seleniumQuery-and-IE-Driver#protected-mode-exception-while-launching-ie-driver). This is the only way to overcome security limitations when selenium jar is used. When you don't use selenium jar, you can enable the 'introduceFlakinessByIgnoringProtectedModeSettings' option, but keep in mind that it is reported to cause driver instability.
```javascript
browsers: [{
  browserName: 'firefox',
  capabilities: {
    iedriverOptions: {
      introduceFlakinessByIgnoringProtectedModeSettings: ['true']
    },
    ieOptions: {
      addArguments: ['-foreground']
    }
  }
}]
```

## Edge
Microsoft Edge requires a webdriver that is distributed as a native installation. Please make sure you have the correct version installed as explained in: [Microsoft Edge WebDriver](https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/). The release version should match the first part of your OS build eg: for OS build number 15063.0000 choose driver Release 15063. The downloaded driver should be moved to <uiveri5-installation-folder>/selenium/ without renaming.

## Safari
Safari10 includes native webdriver that is bundled with the Safari browser. Please make sure you have enabled it as explained in [Testing with WebDriver in Safari](https://developer.apple.com/documentation/webkit/testing_with_webdriver_in_safari)
At this time, there are no meaningfull safaridriverOptions that could be provided under the 'safaridriverOptions' key. If you anyway wish to override the 'addArguments', please make sure not to remove the '-legacy' argument as the webdriverjs version we use require it.
All Safari options from: [Options](https://github.com/SeleniumHQ/selenium/blob/master/javascript/node/selenium-webdriver/safari.js) could be specified under the 'safariOptions' key.
```javascript
browsers: [{
  browserName: 'safari',
  capabilities: {
    safariOptions: {
      setTechnologyPreview: true
    }
  }
}]
```


