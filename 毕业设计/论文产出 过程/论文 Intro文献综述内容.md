2025-5-19 17:12:48
|为什么我的工作是有价值的，直接计算力不就好了？
-将轴零件和孔零件的轴线对齐，高质量对齐到deg arcmin以下是成功插入的必要条件
-系统的各处存在误差，通过数学建模和力-插入深度信息，能够完成准确度较低的对齐，在插入中进行调整以完成高质量对齐是必须的。指导插入中调整方向的，依赖操作员经验；然而大量调整中，操作员无法{准确采样‘相关数据’|在每次判断中遵循相同的规则}，并且操作员不能捕捉更细微的信息。因此，引入深度学习方法是必要的。

2025-5-25 03:49:28
&micro-assembly关键词，对应CCD等的微组装和我的任务不是一个。
-机器人能够完成简单的Peg-in-hole任务
	significant tech. related to robot-guided...
	第2页peg-in-hole mating, error in pose can result in low quality of the bolt/hole or bolt/nut mating operation.
	当前的motion control依托于视觉信息
-光学对齐能够实现高质量的轴孔alignment
	Flip-chip alignment with a dual imaging system using a visual servoing method
	在micro-assembly中，使用visual servoing method，可以将orientation精度提高到0.001rad级别。
	然而在这一精度的对齐，仍然无法单独满足clearance fit precisioin peg-in-hole的需要，因此compliant structure是必要的
-另一种方法是分析force/torque information（引用Robotic Multiple Peg-in-Hole Assembly with Deviation Estimation and Fast Insertion Control）。
-force/torque information可以和视觉信息一起使用，然而optical measuring equipment会增加成本
-当单独使用force/torque information时候，对force contact model的要求提高了。合适的compliant struccture可以使在force contact model中忽略lateral的轴线不对齐的影响，
-在可以平面移动的机构中，磁悬浮轴承或空气轴承，由于没有直接接触，是最合适的【找一个引用】。此时peg-in-hole assembly中shaft and hole contact，gripper和basement不产生接触能够任意平移，且attitude仍能调整。force contact model得到简化。
-The mapping from the force information to the relative pose可以通过model-based或model-free方法完成（Feature-Based Compliance Control for Precise  Peg-in-Hole Assembly）。这些方法被设计用于直接求解relative pose，然而在attitude adjustment中，在每次调整后重新计算当前的relative pose仍然是常见的（同一个引用）。因此，不妨设计一个model-based方法，用于预测下一步的调整方向，并在每步调整后都带入新数据到模型中进行计算。显然，由于降低了计算结果的要求，model可以进一步简化，前期的测量可以减少。

（忽然转向CNN）（引用19日的点2<--否决）
-在仅需要知道预期的调整方向后，model的任务从mapping转为了classification任务。对于time series classification, convolutional neural networks是适用的（引用：A review on time series data mining）。
	在我们的应用中，sequence中，每step的六个力数据被index起来，相邻的6个index构成一组数据。
-为了多组数据共同判断预期的调整方向，一个简单的方法是采用k-nearest neighbors classifier (k-NN) (引用A review on distance based time series classification)，即根据临近k组数据的判断结果的多数决定最终结果。
-我们注意到：在peg-in-hole assembly的不同插入深度阶段，model的预测准确度不同。因此，多种判据被引入用于划分当前的模型预测准确度区间。当前区间的预期准确度降低时，需要k-NN的多数提出同样结果，即置信度更好的结果，adjustment才会进行。
（引用19日的点1）根据model的classification和adjustment过程，在peg-in-hole assembly的前期完成粗对齐，在后期的判断错误虽然增加，但是总体上向正确方向调整，逐渐完成精密对齐。

【结算】
Simple peg-in-hole tasks can be performed by robots [引用占位]. However, 对于complex tasks，optical alignment techniques可能产生incorrect information. 结果是，pose errors during peg-in-hole mating may degrade the quality of bolt/hole or bolt/nut assembly operations. visual information is used to guide motion control. Optical alignment techniques, such as visual servoing systems, can achieve high-precision orientation control (accuracy up to 0.0012 rad) []. However, without additional compliance mechanisms, this level of precision is insufficient for clearance-fit (20μm) peg-in-hole assembly of shaft and hole with large diameter (36.2mm). Another approach is analyzing force/torque information to predict relative pose []. Force/torque analysis can also work together with optical aligment techniques. However, optical measurement systems could increase costs of the equipment. 
When force/torque data is used independently, the force contact model need to be more precise. A compliant structure can mitigate lateral misalignments of the shaft and the hole, simplifying the model. To allow free planer motion, non-contact bearings (e.g., magnetic or air bearings) are valid solutions. These bearings eliminate direct contact between the gripper and base, allowing free translation while maintaining attitude adjustability. This further simplifies the force contact model.
The mapping from force/torque data to relative pose can be achieved via model-based or model-free methods []. Although these methods directly estimate relative pose, iterative recalibration after each adjustment remains used. Thus, a model-based approach is proposed to predict adjustment directions. At each step, the prediction is updated with new data. By eliminate the demand for relative position, the force contact model is allowed with more tolerance.
When only the adjustment direction is required, the task is simplified from mapping to classification. For time-series classification, convolutional neural networks (CNNs) are suitable [A review on time series data mining]. In this application, force data from six sensors in a step is indexed, with adjacent six-index segments forming input groups.
To improve decision robustness, a k-nearest neighbors (k-NN) classifier predictions from multiple data groups [A review on distance-based time series classification]. The adjacent groups (with 1 偏移) is 根据多数表决决定output结果 (predicted adjustment direction). Notably, prediction accuracy varies with insertion depth. Thus, multiple criteria are introduced to assess model 准确度区间. When 当前区间的准确度 decreases, the k-NN classifier requires 更多组数据的同样输出 to 触发 adjustments.
As a result, coarse alignment is achieved in early stages in peg-in-hole assembly, while fine alignment progresses gradually despite increased prediction errors in later stages [].

#### 轴孔装配的作用

【继续】（然后转向imitation learning向专家学习）
系统的各处存在误差，通过数学建模和力-插入深度信息，能够完成准确度较低的对齐，在插入中进行调整以完成高质量对齐是必须的。指导插入中调整方向的，依赖操作员经验；然而大量调整中，操作员无法{准确采样‘相关数据’|在每次判断中遵循相同的规则}，并且操作员不能捕捉更细微的信息。因此，引入深度学习方法是必要的。
...

#### 本文要{内容介绍}