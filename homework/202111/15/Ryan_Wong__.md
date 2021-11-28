# 决策树原理 # 
决策树是这样一种数据结构：用一系列逻辑规则对树根的输入数据进行处理，每个非叶子节点（决策点）都需要依据当前规则做不同决策，分裂成不同的子节点，最终得到每个叶子节点的不同输出。  
其越靠近根的决策点所使用的规则应该分类能力越强，决策树生成原理如下归纳：  

    1.正则化：预处理输入数据，获取相互正交的特征。
    2.规则选择：使用基于信息论的算法，如ID3，每次决策都选取 熵最大 或 熵增斜率最大 的特征。
    3.递归生成：从根向下BFS/DFS生成节点，若任意决策规则都不再可分当前节点，则当前节点停止往下生成。
    4.剪枝