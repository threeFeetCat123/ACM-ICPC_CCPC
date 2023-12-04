#KMP


```C++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn=1e6+9;
const int mod=1e9+7;
int nex[maxn];
char str[maxn],ptr[maxn];
//预处理nex数组
//nex[i]为该位置的boarder 
void get_next(char str[],int nex[],int len){
	nex[1]=0;// dep[1]=0;
	int k=0;
	for(int i=2;i<=len;i++){
		while(k&&str[i]!=str[k+1])k=nex[k];
		if(str[i]==str[k+1])k++;
		nex[i]=k;
		// dep[i]=dep[nex[i]]+1;
	}
}
void match(char s[],int f[],int len){
	int k=0;
	for(int i=1;i<=len;i++){
		while(k&&(s[k+1]!=s[i]||k==strlen(str+1))k=nex[k];
		if(s[k+1]==s[i])k++;
		f[i]=k;
	}
}
int n,m;
ll num[maxn];
int main(){
	cin>>n;
    while(~scanf("%s",str+1)){
    	int len=strlen(str+1);
		get_next(str,nex,len);
    	ll ans=1;
    	for(int i=1;i<=len;i++){
    		num[i]=num[nex[i]]+1;
		}
		
		//快速跳nex，到 最靠近长度为一半，但<=一半的地方 
    	for(int i=1;i<=len;i++){
    		int now=i;
    		while(now*2>i){
    			if(nex[now]>now/2){
    				int period=now-nex[now];
    				now=now%period+(i/period-((now%period)?1:0))/2*period;
				}else now=nex[now];
			}
			ans=(ans*(num[now]+1))%mod;
		}
		printf("%lld\n",ans);
	}
    return 0;
}
```