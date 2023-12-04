#Hash

##一维
```c++
ll Prime_Pool[]={1998585857ll,23333333333ll};
ll Seed_Pool[]={911,146527,19260817,91815541};
ll Mod_Pool[]={29123,290182597,998244353
,1000000009,4294967291ll,23333333377ll};
struct Hass{
	ll bas[N],mod;
	ll sum[N];
	Hass(ll _bas,ll _mod){
		bas[0]=1;
		mod=_mod;
		for(int i=1;i<N;i++){
			bas[i]=(bas[i-1]*_bas)%mod;
		}
	}
	void init(char c[],int len){
		for(int i=1;i<=len;i++){
			sum[i]=(sum[i-1]*bas[1]+c[i])%mod;
		}
	}
	ll getHash(int l,int r){
		ll ans=sum[r]-sum[l-1]*bas[r-l+1]%mod;
		if(ans<0)ans+=mod;
		return ans;
	}
}hs(Seed_Pool[0],Mod_Pool[2]);
```

##二维
```c++
ll Prime_Pool[]={1998585857ll,23333333333ll};
ll Seed_Pool[]={911,146527,19260817,91815541};
ll Mod_Pool[]={29123,290182597,998244353,1000000009,4294967291ll,23333333377ll};
char a[N][N];
struct Hass{
	__int128 sum[N][N];
	ll bas1[N],bas2[N];
	ll mod;
	Hass(ll a,ll b,ll c){
		bas1[0]=1;
		bas2[0]=1;
		mod=c;
		for(int i=1;i<N;i++){
			bas1[i]=(bas1[i-1]*a)%mod;
			bas2[i]=(bas2[i-1]*b)%mod;
		}
	}
	void insert(int n,int m){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=m;j++){
				sum[i][j]=(sum[i-1][j]*bas1[1]+sum[i][j-1]*bas2[1]-sum[i-1][j-1]*bas1[1]*bas2[1])%mod+mod;
				sum[i][j]=(sum[i][j]+(a[i][j]-'a'))%mod;
			}
		}
	}
	ll query(int x1,int y1,int x2,int y2){
		x1--,y1--;
		ll ans=(sum[x2][y2]-sum[x1][y2]*bas1[x2-x1]-sum[x2][y1]*bas2[y2-y1]+sum[x1][y1]*bas1[x2-x1]*bas2[y2-y1])%mod;
		ans=(ans+mod)%mod;
		return ans;
	}
}hs(Seed_Pool[0],Seed_Pool[1],Mod_Pool[2]),hid(Seed_Pool[0],Seed_Pool[1],Mod_Pool[2]);
```

