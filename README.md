  

	位置式PID 
连续形式公式如下：
u(t)=K_p [e(t)+1/T_i  ∫_0^t▒e(τ)dτ+T_d  (de(t))/dt]
也可以写成如下形式：
u(t)=K_p e(t)+K_i ∫_0^t▒e(τ)dτ+K_d  (de(t))/dt
为了方便软件编程实现，一般转换成离散形式，即用连加代替积分，有差分代替微分;

u(t)≈u(k);
e(t)≈e(k);
∫_0^t▒〖e(t)dt〗≈∑_(j=0)^k▒〖Te(j)〗;
(de(t))/dt  ≈  (e(k)-e(k-1))/T;

式中 T为采样周期

离散型PID公式：
u(k)=K_p e(k)+K_i*T∑_(j=0)^k▒〖e(j)〗+K_d/T[e(k)-e(k-1)]

位置PID结构简单，但是由于有积分项，容易产生积分饱和的现象，而且它每次输出的都是全量，此全量均和过去的输出有关，易产生累计误差。需要对其进行改进，由此产生的改进型PID控制器——增量型PID控制器。其区别在于，控制器输出的不是全量，而只是增量，每次输出均与过去的所有状态无关，而且它没有积分项，运算量小，容易实现手动到自动的无冲击切换。
————————————————


	增量型PID控制器：

∆u=u(k)-u(k-1)   
   =K_p [e(k)-e(k-1) ]+K_i*T e(k)+K_d/T [e(k)-2e(k-1)+e(k-2) ]     

可以表示为更一般的形式：

∆u=(K_p+K_i*T+K_d/T)*e(k)+(-K_p-2*K_d/T)*e(k-1)+K_d/T*e(k-2)

参数调整一般规则：
由各个参数的控制规律可知，比例P使反应变快，微分D使反应提前，积分I使反应滞后。在一定范围内，P，D值越大，调节的效果越好。各个参数的调节原则如下：
1、在输出不振荡时，增大比例增益P。
2、在输出不振荡时，减小积分时间常数Ti。
3、输出不振荡时，增大微分时间常数Td。

	确定比例增益P 
确定比例增益P 时，首先去掉PID的积分项和微分项，一般是令Ti=0、Td=0（具体见PID的参数设定说明），使PID为纯比例调节。输入设定为系统允许的最大值的60%~70%，由0逐渐加大比例增益P，直至系统出现振荡；再反过来，从此时的比例增益P逐渐减小，直至系统振荡消失，记录此时的比例增益P，设定PID的比例增益P为当前值的60%~70%。比例增益P调试完成。
	确定积分时间常数Ti
比例增益P确定后，设定一个较大的积分时间常数Ti的初值，然后逐渐减小Ti，直至系统出现振荡，之后在反过来，逐渐加大Ti，直至系统振荡消失。记录此时的Ti，设定PID的积分时间常数Ti为当前值的150%~180%。积分时间常数Ti调试完成。 
	确定微分时间常数Td 
微分时间常数Td一般不用设定，为0即可。若要设定，与确定 P和Ti的方法相同，取不振荡时的30%。 
	系统空载、带载联调，再对PID参数进行微调，直至满足要求
