[命令](https://technastic.com/adb-shell-commands-list/)
1. adb shell pm list packages -s
2. adb shell disable-user --user 0 \[package name]

（2025年02月18日）
```
.\adb -s 192.168.1.100:39089 shell
pm list packages -d
```
我原本禁用了三个：
package:com.meizu.powersave
%% package:com.android.browser
package:com.meizu.mstore %%
都可以启用，只启用了第一个