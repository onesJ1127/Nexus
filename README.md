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
    // 判断 translate-x 是否是靠右（开启状态）
    const classList = slider?.className || '';
    return classList.includes('translate-x-[24px]') || classList.includes('translate-x-[28px]');
  }

  function triggerRealClick(el) {
    const event = new MouseEvent('click', {
      view: window,
      bubbles: true,
      cancelable: true
    });
    el.dispatchEvent(event);
  }

  function toggleIfNeeded() {
    const button = document.getElementById(buttonId);
    if (!button) {
      console.warn('[VPN] 找不到开关按钮');
      return;
    }

    if (!isButtonEnabled(button)) {
      console.log('[VPN] 当前为关闭状态，尝试点击开启...');
      triggerRealClick(button);
    } else {
      console.log('[VPN] 已经是开启状态');
    }
  }

  function waitForDomAndStart() {
    const checkReady = setInterval(() => {
      const btn = document.getElementById(buttonId);
      if (btn) {
        console.log('[VPN] 找到按钮，开始监听开关状态...');
        clearInterval(checkReady);
        toggleIfNeeded(); // 初次尝试
        setInterval(toggleIfNeeded, 2000); // 每2秒检查一次
      }
    }, 500);
  }

  waitForDomAndStart();
})();
