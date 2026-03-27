---
title: RL | Temporal-Difference Learning
draft: false
tags:
  - example-tag
---
 
### TD Prediction

TD(0) 的更新方法（又叫一步TD）
$$
V(S_t) \leftarrow V(S_t) + \alpha\left[R_{t+1} + \gamma V(S_{t+1} )- V(S_t)\right]
$$

> [!code] 用 TD(0) 估计 $v_\pi$
> 
> > 输入：策略 $\pi$
> > 
> > 参数：学习率 $\alpha$
> > 
> > 初始化所有 $V(s)$, 除了 $V(terminal) = 0$
> > Loop for each episode：
> > 
> > 	Initialize $S$
> > 	
> > 	Loop for each step of episode：
> > 	
> > 		$A\leftarrow$ action given by $\pi$ for $S$
> > 		
> > 		Take action $A$ , observe $R, S'$
> > 		
> > 		$V(S)\leftarrow V(S)+\alpha[R + \gammaV(S') - V(S)]$
> > 		
> > 		$S\leftarrow S'$
> > 		
> > 	until S is terminal
> 
> 

TD error
$$
\delta_t\; \dot{=}\; R_{t+1}+\gamma V(S_{t+1}) - V(S)
$$

