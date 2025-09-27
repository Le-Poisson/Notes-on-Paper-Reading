## QFAST: Conflating Search and Numerical Optimization for Scalable Quantum Circuit Synthesis 阅读笔记

论文核心思想：论文讲述了分解量子门的方法。对于一个很大的量子门（黑箱，可以执行输入得到输出），在这个量子门后面添加2个量子比特或3个量子比特的量子门（带位置参数、大小参数等），然后计算目前矩阵到单位矩阵 I 的距离，距离使用 Frobenius norm 计算表示。假设要分解的门为 $U_T$，现在构造出的门为 $U_C$，目标代价为 $||U_T^\dagger U_C - I||$，这个范数通过计算矩阵中每一项的平方和再开方得出。