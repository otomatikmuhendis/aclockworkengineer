---
author: Olcay Bayram
layout: post
categories: Testing
published: true
title: Follow A Link To A New Tab
tags:
  - selenium
  - webdriver
  - automation
subtitle: Selenium Webdriver With Javascript
---

This will tell about my age but back then we did not have tabs on an internet browser. We only had windows. When you click a link on a webpage with the `target` attribute set as `_blank`, it was openning a new window. Now, it opens a new tab. Even folders on a file explorer were openning a new window on Windows 95.

Moving formard, we had a test scenario to check contents of a webpage that opens on a new tab. We had to follow a link from our parent website, to the target but Selenium Webdriver was not doing it by default. So the code below helped us to track new tabs on the browser. If there is a new tab, just switch the driver to continue on that one.


```js
//Get all the tab handles first
var windowHandles = await driver.getAllWindowHandles()

//Click that anchor with target=_blank
await driver.findElement(By.id('externalLink')).click()

//Never ever let the driver sleep, because they may crash
//But there is not any change on the parent page to wait for
await driver.sleep(20000) //in milliseconds

//Keep the previous handles
const handlesThen = windowHandles

//Get the current handles
const handlesNow = await driver.getAllWindowHandles()

//If there are new handles in our list, that must be our target
var newWindow
if (handlesNow.length > handlesThen.length) {
    newWindow = handlesNow.find(handle => (!handlesThen.includes(handle)))
}

//Switch the driver, the previous one was already sleepy
await driver.switchTo().window(newWindow)
```
