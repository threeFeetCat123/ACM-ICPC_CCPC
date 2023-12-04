#	GPAM
给定$ A , B$ 两个字符串，对于每个 $A,B$ 的公共回文子串 $S$，求 $∑resA[S]∗resB[S] $的值，其中 $resA[S] $和 $resB[S] $表示串 $S$ 在$A,B $中出现的次数。


两个字符串，求它们的公共回文子串对数
```c++
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
char c[N];
struct PAM{
	int son[N][26];
	int link[N],len[N];
	ll siz[N][2];
	int cnt;
	void init(){
		cnt=1;
		link[0]=link[1]=1;
		len[1]=-1;
	}
	int getlink(int now,int i){
		while(c[i-len[now]-1]!=c[i]){
			now=link[now];
		}
		return now;
	}
	int insert(char c,int las,int i,int id){
		int now=getlink(las,i);
		int sonid=c-'A';
		if(son[now][sonid]==0){
			int q=++cnt;
			link[q]=son[getlink(link[now],i)][sonid];
			son[now][sonid]=q;
			len[q]=len[now]+2;
		}
		las=son[now][sonid];
		siz[las][id]++;
		return las;
	}
	void add(){
		for(int i=2;i<=cnt;i++){
			addline(link[i],i);
		}
	}
	ll ans=0;
	void dfs(int now){
		
		for(int i=fir[now];i;i=l[i].nex){
			int to=l[i].to;
			dfs(to);
			siz[now][0]+=siz[to][0];
			siz[now][1]+=siz[to][1];
		}
		if(now<2)return;
		ans+=siz[now][0]*siz[now][1];
	}
	void solv(){
		add();
		dfs(0);
		printf("%lld\n",ans);
	}
}pam;



int main(){
	scanf("%s",c+1);
	int len=strlen(c+1);
	pam.init();
	int las=0;
	for(int i=1;i<=len;i++){
		las=pam.insert(c[i],las,i,0);
	}
	scanf("%s",c+1);
	len=strlen(c+1);
	las=0;
	for(int i=1;i<=len;i++){
		las=pam.insert(c[i],las,i,1);
	}
	pam.solv();
	return 0;
}
```