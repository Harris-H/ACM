# exkmp

http://acm.hdu.edu.cn/showproblem.php?pid=2328

求$n$个串的最长公共子串$lcs$

考虑用最短的串作为$T$串，然后枚举$T$的所有后缀，与所有其他串进行$exkmp$，每次找到最大值，然后整体取最小即可。

时间复杂度：$O(n|s|^2)$

---

求一个串的前缀等于另一个串的后缀 最大长度。

exkmp的完全匹配，更新mx值要注意完全匹配后缀。

```cpp
int mx=0;
	for(int i=0;i<n;i++){
		if(e[i]+i==n&&mx<e[i]) mx=e[i];//complete match
	} 
```

---

用exkmp求最小循环节

```cpp
int cycle(string a){ //minimum cycle
	int n=a.size();
	Gnt(a,nt);
	int t=n;
	for(int i=1;i<n;i++)
		if(i+nt[i]==n){
			t=n%i?n:i;
			break;
		}
	return t;
}
```



找到最大长度子串既是前缀，又是后缀，还是中间串。

注意到一个性质：

若$s_0s_1....s_{n-1}$

若$next[n-1]=pos$

则对于整个串的$\le pos$的前后缀长度是$next[pos]$

因为要满足整个串的前缀等于后缀，及$s_0..s_{pos-1}$的前缀等于它的后缀。

---

$s_0s_1...s_{n-1}$  从某个位置切割，然后交换两个部分，等价于从某个位置开始读字符串(循环)。

$123456$。切割3，4中间等价于从$4$开始读取$456321$。

---

退到前后缀大于0最小长度位置

```cpp
for(int i=1;i<n;i++){
		int j=i+1;
		while(p[j-1]) j=p[j-1];
		if(p[i]) p[i]=j;
		ans+=(i+1)-j;
	}
```

---

多边形对称轴问题可转换为边-角-边 的kmp问题。

判断文本串是否存在(模式串)回文串，可先反转模式串，然后进行kmp即可。