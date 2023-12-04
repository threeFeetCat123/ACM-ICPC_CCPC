#tarjant LCA

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=5e5+9;
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
int n,q,rot;
struct line{
	int to,nex;
}l[N<<1];
int fir[N],cntline;
void addline(int fr,int to){
	cntline++;
	l[cntline].to=to;
	l[cntline].nex=fir[fr];
	fir[fr]=cntline;
}
vector<pair<int,int> >vec[N];
int ans[N];
int fa[N];
int find(int x){
	if(fa[x]==x)return x;
	return fa[x]=find(fa[x]);
}
void dfs(int now,int f){
	for(int i=fir[now];i;i=l[i].nex){
		int to=l[i].to;
		if(to==f)continue;
		dfs(to,now);
		fa[to]=now;
	}
	int r=vec[now].size()-1;
	for(int i=0;i<=r;i++){
		int id=vec[now][i].second;
		int to=vec[now][i].first;
		ans[id]=find(to);
	}
}
void solv(){
	n=read(),q=read(),rot=read();
	for(int i=1;i<=n;i++)fa[i]=i;
	for(int i=1;i<n;i++){
		int fr=read(),to=read();
		addline(fr,to);
		addline(to,fr);
	}
	for(int i=1;i<=q;i++){
		int a=read(),b=read();
		vec[a].push_back({b,i});
		vec[b].push_back({a,i});
	}
	dfs(rot,0);
	for(int i=1;i<=q;i++){
		printf("%d\n",ans[i]);
	}
}


int main(){
	solv();
	return 0;
}
```