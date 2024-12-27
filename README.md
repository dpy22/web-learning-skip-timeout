# web-learning-skip-timeout

## 概述
跳过网络学堂超时页面和登录页面

## 简介
众所周知，网络学堂在无操作30分钟后会自动登出，此时任何交互都会跳转到登录超时页面，需要手动点击按钮跳转到登录页面，输入用户名和密码后再重新登录，比较麻烦。

本脚本可以检测当前是否在登录超时页面，若是，则自动跳转到登录页面。

还可以检测当前是否在登录页面，若是，则自动输入脚本中的用户名和密码，并点击登录按钮。

这意味着，当你进入网络学堂时，将自动进入本学期课程仪表盘，若登录超时，任何交互也会自动进入本学期课程仪表盘。

## 用法
1. 浏览器安装插件**篡改猴**
2. 打开浏览器“管理扩展”，勾选**开发人员模式**
3. 复制以下代码，在篡改猴中添加新脚本，粘贴代码
```javascript
// ==UserScript==
// @name         Tsinghua Learn Auto Login
// @namespace    http://tampermonkey.net/
// @version      1.0
// @description  自动检测登录状态并重定向，填充用户名和密码后点击登录
// @author       Parker
// @match        https://learn.tsinghua.edu.cn/*
// @grant        none
// ==/UserScript==

(function() {
    'use strict';

    // 检测页面中是否存在特定的文本
    function checkLoginStatus() {
        const textToCheck = "您未登录或登录失效";
        const elements = document.body.getElementsByTagName("*");
        for (let i = 0; i < elements.length; i++) {
            if (elements[i].textContent.includes(textToCheck)) {
                window.location.href = "https://learn.tsinghua.edu.cn/f/login";
                return;
            }
        }
    }

    // 填充用户名和密码
    function fillCredentials() {
        const usernameInput = document.getElementsByName("i_user")[0];
        const passwordInput = document.getElementsByName("i_pass")[0];
        if (usernameInput && passwordInput) {
            usernameInput.value = "";  // 注意，此处应填充你的网络学堂用户名
            passwordInput.value = "";  // 注意，此处应填充你的网络学堂密码
            return true;
        }
        return false;
    }

    // 点击登录按钮
    function clickLoginButton() {
        const loginButton = document.getElementById("loginButtonId");
        if (loginButton) {
            loginButton.click();
        }
    }

    // 监听页面加载完成
    window.addEventListener('load', function() {
        // 检查是否需要重定向
        checkLoginStatus();
        // 如果已经在登录页面，填充用户名和密码后点击登录按钮
        if (window.location.href.includes("/f/login")) {
            if (fillCredentials()) {
                clickLoginButton();
            }
        }
    });
})();
```
4. 将你的网络学堂用户名和密码填充到函数fillCredentials()
5. 保存脚本，自动开始运行

## 警告
**请勿在不信任的电脑上安装此脚本**
