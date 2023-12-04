#RMQ ST
```c++
int n,a[N];
int st[N][22],lg[N];
void init(){
	lg[0]=-1;
	for(int i=1;i<=n;i++){
		lg[i]=lg[i>>1]+1;
		st[i][0]=a[i];
	}
	for(int i=1;i<22;i++){
		int len=1<<i;
		for(int j=1;j+len-1<=n;j++){
			st[j][i]=max(st[j][i-1],st[j+(len>>1)][i-1]);
		}
	}
}
int query(int l,int r){
	int k=lg[r-l+1];
	return max(st[l][k],st[r-(1<<k)+1][k]);
}

```