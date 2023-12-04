# ISAP
**O(V^2^E)**
```c++
int gap[N],dep[N];
void bfs(){
	for(int i=0;i<=pnum;i++){
		dep[i]=gap[i]=0;
	}
	queue<int>q;
	q.push(T);
	dep[T]=1;
	gap[1]++;
	while(!q.empty()){
		int now=q.front();q.pop();
		for(int i=fir[now];i;i=l[i].nex){
			if(l[i].w)continue;
			int to=l[i].to;
			if(dep[to]==0){
				dep[to]=dep[now]+1;
				gap[dep[to]]++;
				q.push(to);
			}
		}
	}
}
int cur[N];
ll addflw(int now,ll w){
	if(now==T)return w;
	ll ans=0;
	for(int &i=cur[now];i;i=l[i].nex){
		if(l[i].w==0)continue;
		int to=l[i].to;
		if(dep[to]+1!=dep[now])continue;
		ll tmp=addflw(to,min(w,l[i].w));
		l[i].w-=tmp;l[i^1].w+=tmp;
		ans+=tmp;w-=tmp;
		if(w==0)return ans;
	}
	cur[now]=fir[now];
	if(--gap[dep[now]]==0)dep[S]=pnum;
	gap[++dep[now]]++;
	return ans;
}

ll isap(){
	ll ans=0;
	bfs();
	memcpy(cur,fir,sizeof(fir));
	while(dep[S]<pnum){
		ans+=addflw(S,1e18);
	}
	return ans;
}
```
