# Nexus
Nexus网页端自动检测按钮


## ✅ 使用方法

1. 打开浏览器的开发者工具（按 F12）
2. 切换到控制台（Console）
3. 粘贴以下脚本并回车执行
![image](https://github.com/user-attachments/assets/1617c004-390a-4292-8787-670e7de16a37)
## 📜 脚本代码

```js
(function ensureVPNAlwaysOn() {
  const buttonId = 'connect-toggle-button';

  function isButtonEnabled(button) {
    if (!button) return false;
    const slider = button.querySelector('div');
    const classList = slider?.className || '';
    return classList.includes('translate-x-[24px]') || classList.includes('translate-x-[28px]');
  }

  function triggerRealClick(el) {
    ['pointerdown', 'pointerup', 'click'].forEach(type => {
      el.dispatchEvent(new MouseEvent(type, {
        view: window,
        bubbles: true,
        cancelable: true
      }));
    });
  }

  function toggleIfNeeded() {
    const button = document.getElementById(buttonId);
    if (!button) {
      console.warn('[VPN] 找不到按钮');
      return;
    }

    if (!isButtonEnabled(button)) {
      console.log('[VPN] 状态关闭，尝试点击开启...');
      triggerRealClick(button);
    } else {
      console.log('[VPN] 状态显示为开启，继续检查后台连接状态...');
    }

    // 可选：检测连接状态 DOM
    const connectedText = document.body.innerText.includes('Connected') || document.body.innerText.includes('已连接');
    if (!connectedText) {
      console.warn('[VPN] 看起来并未真正连接成功，尝试重新点击...');
      triggerRealClick(button);
    }
  }

  function waitForDomAndStart() {
    const checkReady = setInterval(() => {
      const btn = document.getElementById(buttonId);
      if (btn) {
        console.log('[VPN] 找到按钮，开始监测...');
        clearInterval(checkReady);
        toggleIfNeeded(); // 初次点击
        setInterval(toggleIfNeeded, 3000); // 每 3 秒检查一次
      }
    }, 500);
  }

  waitForDomAndStart();
})();

