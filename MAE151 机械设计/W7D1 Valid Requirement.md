**basic requir.**
- verifiable
- consistent
- unambiguous -- #可改善 不能用一个requirement完成两个功能，其中一个是‘免费赠送的’；我有时看到免费赠送就接下来了
**Develop the TEST plan**
def::Entry condition --- 希望处于的测试环境，至少确保{条件对齐|可低成本多重复}
def::Exit condition --- 
- [ ] 本课程要求：设计报告、专利申请、poster<---完整设计流程
~~其中的一步 -- repeat for the nonbehavioral tests~~ 物理特性检测
**Behavioral test procedure**
	谁-状态-动作-预期|实际
	rotor-ACTIVATE-rotate-/plate_rotate/pin1_up/pin2_down->Actual, timing=0.1s
A(nalysis)-I(nspection)-D(emonstration)-T(physical test)四种检验，符合物理、直觉（视觉）-触摸感受-数学
	Demonstration中一般是scaling模型
	Ananlysis是纯虚拟
	Physical test就是‘中航发拿发动机供暖’
**Fault Tree Analysis | Failure Modes and Effects Analysis**
Top-down|Buttom-up
术语对于初学者没有意义。
& #可改善 请千万别再因为懒而不做验证、期待一次成功了。

最大的‘驾车人为错误’归因为{疲劳驾驶}|>{驾驶员疲劳监控}；至于为什么不是{酒驾}，因为‘刑法规定了酒驾’从而降低了‘饮酒后驾车的可能’。
	注意到：第一归因可能已经被解决了