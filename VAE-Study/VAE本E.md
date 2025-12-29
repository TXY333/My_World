## VAE的思路
![[Pasted image 20250924200152.png]]
怎么理解呢？首先考虑条件变量，对这个不可预测定义的抽象的潜在变量进行边缘化积分。
![[Pasted image 20250924200427.png]]
即然log无法直接穿过积分，导致了一个问题，那么为什么一定要用log？
首先考虑对数似然的方法，对于一些独立样本来说，他能做到积化和，这样很有利于减小数值误差。
![[Pasted image 20250924203136.png]]
![[Pasted image 20250924203150.png]]

![[Pasted image 20250924211345.png]]
![[Pasted image 20250924212549.png]]
![[Pasted image 20250924212718.png]]

![[Pasted image 20250924213229.png]]
![[Pasted image 20250924214657.png]]