# 求1-n与P互质的数字数量
```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
ll n,p;
ll cnt_pri(ll n,ll x){
	vector<ll>p;
	for(ll i=2;i*i<=x;i++){
		if(x%i==0){
			p.push_back(i);
			while(x%i==0){
				x/=i;
			}
		}
	}
	if(x>1){
		p.push_back(x);
	}
	int len=p.size();
	ll ans=0;
	for(int i=1;i<1<<len;i++){
		ll tmp=1,t=0;
		for(int j=0;j<len;j++){
			if(i>>j&1){
				tmp*=p[j];
				t++;
			}
		}
		if(t&1){
			ans+=n/tmp;
		}else{
			ans-=n/tmp;
		}
	}
	return n-ans;
	
}



int main(){
	scanf("%lld %lld",&n,&p);
	cout<<cnt_pri(n,p);
	return 0;
}
```