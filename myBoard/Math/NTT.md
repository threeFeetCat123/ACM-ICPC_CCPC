# NTT

大数乘法 **a*b=c**
```c++
#include<bits/stdc++.h>
using namespace std;
const int maxn=1e7+9;
const int p=998244353,g=3,gi=332748118;
typedef long long ll;
int r[maxn<<2],bit,num=1;
ll a[maxn],b[maxn];
int n,m,inv;
void butt(int num,int bit){
	for(int i=0;i<num;i++){
		r[i]=(r[i>>1]>>1)|((i&1)<<(bit-1));
	}
}
ll ksm(ll x,int n){
	ll ans=1;
	while(n){
		if(n&1)ans=ans*x%p;
		x=x*x%p;
		n>>=1;
	}
	return ans;
}
void ntt(ll a[],int opt){
	for(int i=0;i<num;i++){
		if(i<r[i])swap(a[i],a[r[i]]);
	}
	for(int mid=1;mid<num;mid<<=1){
		ll wn=ksm(opt==1?g:gi,(p-1)/(mid<<1));
		for(int R=mid<<1,j=0;j<num;j+=R){
			ll w=1;
			for(int k=0;k<mid;k++,w=w*wn%p){
				ll x=a[j|k],y=w*a[j|k|mid]%p;
				a[j|k]=(x+y)%p;
				a[j|k|mid]=(x-y+p)%p;
			}
		}
	}
	if(opt==-1)for(int i=0;i<num;i++)a[i]=a[i]*inv%p;
}
string str; 
int main(){
	cin>>str;
	int len=str.size();
	n=len;
	for(int i=0;i<len;i++){
		a[n-i-1]=str[i]^48;
	}
	cin>>str;
	len=str.size();
	m=len;
	for(int i=0;i<len;i++){
		b[m-i-1]=str[i]^48;
	}
	for(;num<n+m;bit++,num<<=1);
	butt(num,bit);
	inv=ksm(num,p-2);
	ntt(a,1);
	ntt(b,1);
	for(int i=0;i<num;i++){
		a[i]=a[i]*b[i]%p;
	}
	ntt(a,-1);
	n=n+m;
	for(int i=0;i<n;i++){
		a[i+1]+=a[i]/10;
		a[i]%=10;	
	}
	while(a[n]==0)n--;
	for(int i=n;i>=0;--i){
		printf("%lld",a[i]);
	}
	return 0;
}
```