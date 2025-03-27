[Alan Zucconi的游戏教程](https://www.alanzucconi.com/2015/02/24/to-voronoi-and-beyond/)
**the Manhattan distance** 曼哈顿都市的最近距离是绕建筑距离。
即$$D=|\Delta x|+|\Delta dy|$$
因此，所有点之间的距离都是..正交直线的sum
**Minkowski distance** General form of 所有距离包括Euclidean distance
$$D=   (|\Delta x|^n+|\Delta y|^n)^{1/n}$$
而Voronoi的生成，就是找到空间每个位置、归属到离‘位置’最近的点。Minkowski的n系数决定‘直的或是弯的’
