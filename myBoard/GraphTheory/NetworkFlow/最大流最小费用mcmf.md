#MCMF
```c++
ll dis[N],pre[N],fl[N];
int inq[N];
int spfa(){
	for(int i=0;i<=pnum;i++){
		inq[i]=0;
		dis[i]=1e18;
		pre[i]=fl[i]=0;
	}
	fl[S]=1e18;
	dis[S]=0;
	queue<int>q;
	inq[S]=1;
	q.push(S);
	while(!q.empty()){
		int now=q.front();q.pop();
		inq[now]=0;
		for(int i=fir[now];i;i=l[i].nex){
			int to=l[i].to;
			if(l[i].w==0)continue;
			if(dis[to]>dis[now]+l[i].c){
				dis[to]=dis[now]+l[i].c;
				pre[to]=i;
				fl[to]=min(fl[now],l[i].w);
				if(inq[to]==0){
					inq[to]=1;
					q.push(to);
				}
			}
		}
	}
	return dis[T]<1e18;
}
ll ansfl,anscos;
void mcmf(){
	while(spfa()){
		int now=T;
		for(int i=pre[now];i;i=pre[now]){
			l[i].w-=fl[T];
			l[i^1].w+=fl[T];
			now=l[i^1].to;
		}
		ansfl+=fl[T];
		anscos+=fl[T]*dis[T];
	}
}
```