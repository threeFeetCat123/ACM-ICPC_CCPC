#PAM

Given a string s, find the number of ways to split s to substrings such that if there are k substrings (p1, p2, p3, ..., pk) in partition, then pi = pk - i + 1 for all i (1 ≤ i ≤ k) and k is even.


```c++
char c[N],s[N];
struct PAM{
	int son[N][26];
	int len[N],fail[N];
	//int num[N]; num[i]:节点i代表的回文串的后缀回文串数量（包括自己） 
	//int siz[N]; siz[i]:节点i代表的回文串在原串出现的次数 
	//for(int i=cnt;i>1;i--)siz[fail[i]]+=siz[i];
	ll diff[N],anc[N],g[N],f[N];
	//diff:i与fail[i]长度相差值
	//anc:等差数列的头节点的fail节点
	//g：树上等差数列的转移和 
	//f:字符串的dp转移 
	 
	int cnt=1;
	void init(){
		cnt=1;
		fail[0]=fail[1]=1;
		len[1]=-1;
		f[0]=1;
	}
	int getfail(int now,int i){
		while(c[i-len[now]-1]!=c[i]){
			now=fail[now];
		}
		return now;
	}
	int insert(char c,int las,int i){
		int now=getfail(las,i);
		int sonid=c-'a';
		if(son[now][sonid]==0){
			int q=++cnt;
			fail[q]=son[getfail(fail[now],i)][sonid];
			son[now][sonid]=q;
			len[q]=len[now]+2;
			diff[q]=len[q]-len[fail[q]];
			anc[q]=diff[q]==diff[fail[q]]?anc[fail[q]]:fail[q];
			//num[q]=num[fail[q]]+1;
		}
		las=son[now][sonid];
		//siz[las]++;
		return las;
	}
	void trans(int las,int i){
		for(int now=las;now>1;now=anc[now]){
			g[now]=f[i-len[anc[now]]-diff[now]];
			if(diff[now]==diff[fail[now]]){
				g[now]=(g[now]+g[fail[now]])%mod;
			}
			if(i%2==0){
				f[i]=(f[i]+g[now])%mod;
			}
		}
	}
	void insert(char c[]){
		int len=strlen(c+1);
		int las=0;
		for(int i=1;i<=len;i++){
			las=insert(c[i],las,i);
			trans(las,i);
		}
		printf("%lld\n",f[len]);
	}
}pam;



int main(){
	scanf("%s",s+1);
	int len=strlen(s+1);
	for(int i=1;i<=len/2;i++){
		c[(i<<1)-1]=s[i];
		c[(i<<1)]=s[len+1-i];
	}
	pam.init();
	pam.insert(c);
	return 0;
}
```
```C++
输入：abcdcdab
输出: 1
```
