#倍增LCA
给定一棵有根多叉树，请求出指定两个点直接最近的公共祖先。
```c++
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
int lg[N];
int dep[N],f[N][25];
void dfs(int now,int f){
	f[now][0]=f;
	dep[now]=dep[f]+1;
	for(int i=1;i<=lg[dep[now]];i++){
		f[now][i]=f[f[now][i-1]][i-1];
	}
	for(int i=fir[now];i;i=l[i].nex){
		int to=l[i].to;
		if(to==f)continue;
		dfs(to,now);
	}
}
int LCA(int a,int b){
	if(dep[a]<dep[b])swap(a,b);
	while(dep[a]>dep[b]){
		a=f[a][lg[dep[a]-dep[b]]-1];
	}
	if(a==b)return a;
	for(int i=lg[dep[a]]-1;i>=0;i--){
		if(f[a][i]!=f[b][i]){
			a=f[a][i];b=f[b][i];
		}
	}
	return f[a][0];
}

void solv(){
	n=read(),q=read(),rot=read();
	for(int i=1;i<=n;i++){
		lg[i]=lg[i-1]+(1<<lg[i-1]==i);
	}
	for(int i=1;i<n;i++){
		int fr=read(),to=read();
		addline(fr,to);
		addline(to,fr);
	}
	dfs(rot,0);
	for(int i=1;i<=q;i++){
		int a=read(),b=read();
		printf("%d\n",LCA(a,b));
	}
}

```