---
layout: post
title:  "Angular E2E installation"
date:   2022-12-22 12:00:00 +0100
categories: installation angular-e2e
permalink: /:year/:categories/:title.html
---

<h1>Angular E2E installation on Ubuntu 20.04 LTS on WSL2</h1>

<a href="https://learn.microsoft.com/pl-pl/windows/dev-environment/javascript/nodejs-on-wsl">Install Node.js on Windows Subsystem for Linux (WSL2)</a>

        sudo apt-get install curl
        curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash

<a href="https://www.browserstack.com/guide/how-to-perform-end-to-end-testing-in-angular">How to perform End to End Testing in Angular</a>

<h2>Errors during running Protractor</h2>

Remember about installing Java and Google Chrome <br>
<a href="https://scriptverse.academy/tutorials/protractor-setup.html">scriptverse.academy</a> <br>
<span style="color:red">E/launcher - Process exited with error code 199</span>
      
        sudo apt-get install default-jre
        java --version

<a href="https://scottspence.com/posts/use-chrome-in-ubuntu-wsl">scottspence.com</a> <br>
<span style="color:red">WebDriverError: unknown error: cannot find Chrome binary</span>
        
        wget https://dl.google.com/linux/direct google-chrome-stable_current_amd64.deb
        sudo apt -y install ./google-chrome-stable_current_amd64.deb
        google-chrome --version

<span style="color:red">(unknown error: DevToolsActivePort file doesn't exist)</span>

<h2>Hello World</h2>
<a href="https://www.protractortest.org/#/browser-setup">Headless mode is very important!</a>

conf.js

        exports.config = {
        framework: 'jasmine',
        seleniumAddress: 'http://localhost:4444/wd/hub',
        capabilities: {
                browserName: 'chrome',
                chromeOptions: {
                args: [ "--headless", "--disable-gpu", "--window-size=800,600" ]
                }
        },
        specs: ['myFirstTestSpec.js']
        }

myFirstTestSpec.js

        describe('Protractor Testing', function() {
        it('to check the page title', function() {
                browser.ignoreSynchronization = true;
                browser.get('https://example.com/');
                browser.driver.getTitle().then(function(pageTitle) {
                expect(pageTitle).toEqual('Example Domain');
                })
        })
        })