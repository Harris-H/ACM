$MEX$问题



1.构造序列$b$使$\forall i\in[1,n]:MEX\{b_1,b_2\dots b_i\}=a_i$

给出的$a[i]\leq a[i+1],i\in[1,n)$

显然$a[i]>i$是无解的。

我们用一个数组标记出现过那些数字。

然后用一个$num$记录当前未出现的最小整数。

如果$a[i-1]!=a[i]$显然$b[i]=a[i-1]$.

否则取$num$

```C++
if(a[i]>i) puts("-1"),exit(0);
if(i>1&&a[i]>a[i-1])
		b[i]=a[i-1];
else {
		while(vis[num]) num++;
		   b[i]=num++;
	 }
```

