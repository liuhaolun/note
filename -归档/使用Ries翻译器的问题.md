有时按钮是反向的，直边指向左侧

“智能增强”在鼠标移动到Ries图标后立刻弹起，误导“智能增强”而非“点击图标”是切换功能开关

12月30日 [擅长倒立花泽类](https://space.bilibili.com/7326622/) 被翻译为 擅长倒立花泽类(倒立)， [海绵宝宝文鸯儿](https://space.bilibili.com/284528279/)被翻译为 海绵宝宝 SpongeBob(文鸯儿)，对用户名的处理不够好。

12月30日 “母语”选项只能为中文

12月31日 版本更新，触发弹出dashboard，缺少changelog

1月2日 打开“智能增强”时，通过“点击图标”关闭翻译后，新建标签页仍然翻译为打开状态。

（2025年01月2日） 受到催促，上面的发过去了。

1月8日 https://ries.ai/introduce-in-one-page 页面不存在 （1月9日已解决）

1月8日 “1人正在看”翻译为"1 people are watching"，应为"1 person is watching"

1月8日 我认为更好的风格：![[plus/Pasted image 20250108140201.png]]
	实现方法：`<xt-mark w="renovate" xt-annotation="v. 修复">Renovating</xt-mark>`
	此外在css中定义：
	```
	xt-mark[xt-annotation]::after {
    content: attr(xt-annotation);
    margin-left: .25em;
    font-size: .85em;
    opacity: .8}
	```

1月8日 打开"auto input"选项后，加载标签页的过程是：1.替换文字2.显示图标。无法手动中断替换过程。

（2025年01月15日） 在受到催促后，提交了。