[Alan Zucconi的游戏教程](https://www.alanzucconi.com/2015/02/24/to-voronoi-and-beyond/)
**the Manhattan distance** 曼哈顿都市的最近距离是绕建筑距离。
即$$D=|\Delta x|+|\Delta dy|$$
因此，所有点之间的距离都是..正交直线的sum
**Minkowski distance** General form of 所有距离包括Euclidean distance
$$D=   (|\Delta x|^n+|\Delta y|^n)^{1/n}$$
而Voronoi的生成，就是找到空间每个位置、归属到离‘位置’最近的点。Minkowski的n系数决定‘直的或是弯的’

（2025年03月29日） Voronoi填充模型
|需要边缘吗？是的，但它们不参与连接。
幻想流程：1.在平面内（随机分布）!=->进行三角形Delaunay切割 2.进行多层的，并对点进行连接（Voronoi） 3.线转实体
##### Delaunay
对于一个面，获取它的corners of face, 转换为edges of corners即，重复两次修改sort index则获得两条相邻边。我当前错误的算法，似乎总能根据sort index不同，获取穿过两个点的线。
我们假设对于三角形A-B-C，获取了A-B边。
evaluate index为法向量；evaluate index为边的vector表示，即边的坐标位置。
从几何中，我们知道两条向量的交点；从圆几何上，我们知道外心是法线交点。
	表达为：外心定义为三角形三条边垂直平分线的交点，由于任意两边只有一个交点，固这一点就是外心。
(https://paulbourke.net/geometry/pointlineplane/)::求解Intersection point of two line segments in 2 dimensions, 初中数学知识。
它提出u_a通用项表示<|当只知道四个点，希望求解连线交点时，u_a是其中两点连线的斜率。
在blender中，点的位置表示为position是vector，边的vector则是从起点指向终点的向量，即方向向量。
在blender中，解算两个方向向量为x-y值，则可以作为起点。