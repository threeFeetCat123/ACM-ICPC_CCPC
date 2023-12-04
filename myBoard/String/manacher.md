#Manacher
```c++
int n;
char c[maxn];
char tmp[maxn];
int p[maxn];
void manache(char c[],int p[]){
	int len=strlen(c+1);
	int cnt=-1;
	tmp[++cnt]='$';
	tmp[++cnt]='$';
	for(int i=1;i<=len;i++){
		tmp[++cnt]=c[i];
		tmp[++cnt]='$';
	}
	int r=0,id=0;
	for(int i=1;i<=cnt;i++){
		if(i<r){
			p[i]=min(r-i,p[(id<<1)-i]);
		}else p[i]=1;
		while(tmp[i-p[i]]==tmp[i+p[i]]){
			p[i]++;
		}
		if(i+p[i]>r){
			r=i+p[i];
			id=i;
		}
	}
	int ans=0;
	for(int i=1;i<=cnt;i++){
		ans=max(ans,p[i]);
	}
	cout<<ans-1<<endl;
}
int main(){
	scanf("%s",c+1);
	manache(c,p);
	return 0;
}
```