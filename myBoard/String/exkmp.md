#exkmp

给定两个字符串a,b,你要求出两个数组：
- b 的 z 函数数组 z，即 b 与 b 的每一个后缀的 LCP 长度。
- b 与 a 的每一个后缀的 LCP 长度数组 p。

对于一个长度为 n 的数组 a，设其权值为$xor^n_ {i=1} i*(a_ {i}+1)$

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn=2e7+9;
ll read(){
	ll ans=0,f=1;
	char c=getchar();
	while(c<'0'||c>'9'){
		if(c=='-')f=-1;
		c=getchar();
	}
	while(c>='0'&&c<='9'){
		ans=(ans<<1)+(ans<<3)+c-'0';
		c=getchar();
	}
	return ans*f;
}
int n;
char str[maxn],ptr[maxn];
int z[maxn],p[maxn];
ll ans1,ans2;
void get_z(char s[],int z[],int len){
	int r=0,id;
	z[1]=len;
	for(int i=2;i<=len;i++){
		if(i<=r){
			z[i]=min(z[i-id+1],r-i+1);
		}
		while(i+z[i]<=len&&s[z[i]+1]==s[i+z[i]])z[i]++;
		if(i+z[i]-1>r){
			r=i+z[i]-1;
			id=i;
		}
	}
}
void exkmp(char str[],int slen,int p[],char ptr[],int plen,int z[]){
	int r=0,id;
	for(int i=1;i<=slen;i++){
		if(i<=r)p[i]=min(z[i-id+1],r-i+1);
		while(i+p[i]<=slen&&str[i+p[i]]==ptr[p[i]+1])p[i]++;
		if(i+p[i]-1>r){
			id=i;
			r=i+p[i]-1;
		}
	}
}
int main(){
	scanf("%s",str+1);
	scanf("%s",ptr+1);
	int slen=strlen(str+1),plen=strlen(ptr+1);
	get_z(ptr,z,plen);
	exkmp(str,slen,p,ptr,plen,z);
	for(int i=1;i<=plen;i++){
		ans1^=1ll*i*(z[i]+1);
	}
	for(int i=1;i<=slen;i++){
		ans2^=1ll*i*(p[i]+1);
	}
	
	cout<<ans1<<endl<<ans2<<endl;
	return 0;
}
```
```
输入
aaaabaa
aaaaa

输出
6
21
```