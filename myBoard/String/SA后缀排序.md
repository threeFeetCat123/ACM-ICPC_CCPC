#SA后缀排序
```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn=1e6+9;
char s[maxn];
int sa[maxn],rk[maxn],height[maxn],c[maxn],k1[maxn],k2[maxn];
/*
s[]:字符串 
suff[i]:首字母下标为i的后缀 
sa[i]:排名为i的后缀的首字母的下标 
rk[i]:首字母下标为i的排名 
height[i]:排名为i的后缀和排名为i-1的后缀的最长公共前缀 
h[i]:下标为i的后缀和排名排在它前面的后缀的最长公共前缀 
x[]:第一关键字 
y[]:第二关键字 
c[]:桶 

sa[rk[i]]=i;
rk[sa[i]]=1;
LCP(i,j)=suff[sa[i]]和suff[sa[j]]的最长公共前缀; 
height[i]=LCP(i,i-1);
h[i]=height[rk[i]];
height[i]=h[sa[i]];
*/ 
void get_SA(char s[]){
	int len=strlen(s+1);
	int m=200;
	for(int i=1;i<=m;i++)c[i]=0;
	for(int i=1;i<=len;i++)c[k1[i]=s[i]]++;
	for(int i=2;i<=m;i++)c[i]+=c[i-1];
	for(int i=len;i;i--)sa[c[k1[i]]--]=i;
	for(int k=1;k<=len;k<<=1){
		int num=0;
		for(int i=len-k+1;i<=len;i++)k2[++num]=i;
		for(int i=1;i<=len;i++)if(sa[i]>k)k2[++num]=sa[i]-k;
		for(int i=1;i<=m;i++)c[i]=0;
		for(int i=1;i<=len;i++)c[k1[i]]++;
		for(int i=2;i<=m;i++)c[i]+=c[i-1];
		for(int i=len;i;i--)sa[c[k1[k2[i]]]--]=k2[i],k2[i]=0;
		swap(k1,k2);k1[sa[1]]=1;num=1;
		for(int i=2;i<=len;i++)k1[sa[i]]=(k2[sa[i]]==k2[sa[i-1]]&&k2[sa[i]+k]==k2[sa[i-1]+k])?num:++num;
		if(num==len)break;
		m=num;
	}
}
void get_height(char s[]){
	int len=strlen(s+1);
	int k=0;
	for(int i=1;i<=len;i++)rk[sa[i]]=i;
	for(int i=1;i<=len;i++){
		if(rk[i]==1)continue;
		if(k)k--;
		int j=sa[rk[i]-1];
		int r=max(j,i);
		while(r+k<=len&&s[i+k]==s[j+k])++k;
		height[rk[i]]=k;
	}
}

int main(){
	int tst;
	cin>>tst;
	while(tst--){
		scanf("%s",s+1);
		get_SA(s);
		get_height(s);
		int len=strlen(s+1);
		ll ans=len-sa[1]+1;
		for(int i=2;i<=len;i++){
			ans+=len-sa[i]+1-height[i];
		}
		cout<<ans<<endl;
	}
	return 0;
}

```