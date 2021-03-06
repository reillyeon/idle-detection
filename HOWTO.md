# How to use the IdleDetector API

The IdleDetector API is currently available in Chrome 84 and later behind a flag or by registering your domain for [the Origin Trial](https://developers.chrome.com/origintrials/#/view_trial/551690954352885761) (available until November 10th, 2020).

1) Download Chrome ([Android](https://play.google.com/store/apps/details?id=com.android.chrome), [Desktop](https://www.google.com/chrome/)).
2) Navigate to `chrome://flags` and enable `Experimental Web Platform features`.
3) Navigate to a test page, e.g. https://reillyeon.github.io/idle.html
4) Go idle (e.g. stop moving your mouse, typing on your keyboard or lock your screen)

Here is an example of how to use the API:

```javascript
async function main() {
  // feature detection.
  if (!window.IdleDetector) {
    console.log("IdleDetector is not available :(");
    return;
  }
  
  log("IdleDetector is available! Go idle!");
  
  try {
    await IdleDetector.requestPermission();

    let idleDetector = new IdleDetector();
    idleDetector.addEventListener('change', e => {
      console.log(`[${new Date().toLocaleString()}] idle change: ${idleDetector.userState}, ${idleDetector.screenState}`);
    });
    // There is a minimum limit here of 60 seconds.
    await idleDetector.start({threshold: 60000});
  } catch (e) {
    // deal with initialization errors.
    // permission denied, running outside of top-level frame, etc
    console.log(`Initialization error: ${e}.`);
  }
};
```

You should see something along the lines of:

```
[4/4/2019, 2:59:54 PM] idle change: active, unlocked
[4/4/2019, 3:00:54 PM] idle change: idle, unlocked
[4/4/2019, 3:01:07 PM] idle change: active, unlocked
[4/4/2019, 3:02:25 PM] idle change: idle, unlocked
[4/4/2019, 3:02:41 PM] idle change: active, unlocked
[4/4/2019, 3:03:56 PM] idle change: idle, unlocked
[4/4/2019, 3:12:56 PM] idle change: idle, locked
[4/4/2019, 3:17:38 PM] idle change: active, unlocked
```
