---
author: Olcay Bayram
layout: post
categories: Testing
published: true
title: Automate Microsoft Login
tags:
  - selenium
  - webdriver
  - automation
  - microsoft
  - azure
  - azure ad
subtitle: Selenium Webdriver With Javascript
---
If you have a protected website by Azure Active Directory, you need to pass Microsoft login first to run your functional UI tests.

I used both id and name locators for the elements and they have some weird naming. I am not sure but maybe the purpose is to prevent any bot attacks. We need to use `driver.wait()` method of the webdriver to wait until the element is found but with a timeout limit in milliseconds.

It is not a good practice to have animations on web but Microsoft Online login has some sliding animations. This causes another bad practice like sleeping the driver for the animation to be completed. Please, keep in mind never let the driver `sleep()`, but let her `wait()`. We do not have anything to wait for at this step, so I used `driver.sleep()` for 1 second. It is not preferred because it will add that duration to your test run no matter what.

```js
module.exports = {

    elements: {
        usernameInput: by.id('i0116'),
        passwordInput: by.name('passwd'),
        submitButton: by.id('idSIButton9')
    },

    /**
     * Sign in to Azure AD with the given credentials
     * @param {string} username
     * @param {string} password
     */
    signin: function (username, password) {
        var locateTimeout = 1000; //in milliseconds

        var usernameElement = driver.wait(until.elementLocated(elements.usernameInput), locateTimeout);
        usernameElement.sendKeys(username);
        usernameElement.sendKeys(selenium.Key.ENTER);

        var passwordElement = driver.wait(until.elementLocated(elements.passwordInput), locateTimeout);
        passwordElement.sendKeys(password);

        driver.sleep(1000); //bad bad bad

        var submitButtonElement = driver.wait(until.elementLocated(elements.submitButton), locateTimeout);
        submitButtonElement.click();

        var staySignedInButtonElement = driver.wait(until.elementLocated(elements.submitButton), locateTimeout);
        staySignedInButtonElement.click();
    }
};
```
<!--more-->

The purpose of this post is not anything related to hacking. This would help you to pass Microsoft login if you know the credentials already. If you do not know them, you cannot use this code to bypass any security measures.

The code is supposed to be used on a test environment to test your own software with automated UI tests.
