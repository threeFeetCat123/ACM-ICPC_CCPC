#KM最大边权匹配
$O(n^3)$
```c++
//INF= 1e9+7 若求最小权值匹配，应该为-1e9+7;
//const int N = 607;//最多图的点数
ll w[N][N];//边权
bool visa[N],visb[N];//访问标记，是否在交错树中
int match[N];//右部点匹配的左部点（一个只能匹配一个嘛）
int n,delta;
int la[N],lb[N];//左、右部点的顶标
int p[N];
ll c[N];

void bfs(int x){
	int a,y=0,y2=0;
	for(int i=1;i<=n;++i){
		p[i]=0;c[i]=1e9;
	}
	match[y]=x;
	do{
		a=match[y],delta=1e9,visb[y]=1;
		for(int b=1;b<=n;++b){
			if(!visb[b]){
				if(c[b]>la[a]+lb[b]-w[a][b]){
					c[b]=la[a]+lb[b]-w[a][b];
					p[b]=y;
				}
				if(c[b]<delta){
					delta=c[b];y2=b;
				}
			}
		}
		for(int b=0;b<=n;++b){
			if(visb[b]){
				la[match[b]]-=delta;lb[b]+=delta;
			}else{
				c[b]-=delta;
			}
		}
		y=y2;
	
	}while(match[y]);
	while(y)match[y]=match[p[y]],y=p[y];
}



int KM(){
	for(int i=1;i<=n;i++){
		match[i]=la[i]=lb[i]=0;
	}
	for(int i=1;i<=n;i++){
		for(int j=1;j<=n;j++){
			visb[j]=0;
		}
		bfs(i);
	}
	ll res=0;
	for(int i=1;i<=n;i++){
		res+=w[match[i]][i];
	}
	return res;
}

int main(){
	while(~scanf("%d",&n)){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				w[i][j]=read();
			}
		}
		printf("%d\n",KM());
	}
	return 0;
}
```