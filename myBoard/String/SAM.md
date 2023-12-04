#后缀自动机（SAM）
给出几个由小写字母构成的单词，求它们最长的公共子串的长度。

```c++
struct SAM{
	int son[N<<1][26];
	int len[N<<1],link[N<<1];
	int cnt;
	int mi[N<<1],ma[N<<1];
	void init(){
		link[0]=-1;
		cnt=0;
		memset(mi,0x3f,sizeof(mi));
	}
	int insert(char c,int last){
		int now=last,p=++cnt;
		int sonid=c-'a';
		len[p]=len[now]+1;
		while(now!=-1&&!son[now][sonid]){
			son[now][sonid]=p;
			now=link[now];
		}
		if(now==-1)link[p]=0;
		else{
			int q=son[now][sonid];
			if(len[q]==len[now]+1)link[p]=q;
			else{
				int cop=++cnt;
				memcpy(son[cop],son[q],sizeof(son[q]));
				link[cop]=link[q];
				len[cop]=len[now]+1;
				while(now!=-1&&son[now][sonid]==q){
					son[now][sonid]=cop;
					now=link[now];
				}
				link[q]=link[p]=cop;
			}
		}
		return p;
	}
	int topu[N<<1],tot[N<<1];
	void sort1(){
		for(int i=1;i<=cnt;i++)tot[len[i]]++;
		for(int i=1;i<=cnt;i++)tot[i]+=tot[i-1];
		for(int i=1;i<=cnt;i++)topu[tot[len[i]]--]=i;
	}

	void solv(char c[],int slen){
		int now=0,l=0;
		for(int i=1;i<=slen;i++){
			int sonid=c[i]-'a';
			while(now!=-1&&!son[now][sonid]){
				now=link[now];
				l=len[now];
			}
			if(now!=-1){
				l++;
				now=son[now][sonid];
				l=min(l,len[now]);
				ma[now]=max(ma[now],l);
			}else{
				now=0,l=0;
			}
		}
		for(int i=cnt;i;i--){
			int now=topu[i];
			ma[link[now]]=max(ma[link[now]],min(len[link[now]],ma[now]));
			mi[now]=min(mi[now],ma[now]);
			ma[now]=0;
		}
	}
	int ans=0;
	void sakura(){
		for(int i=1;i<=cnt;i++){
			ans=max(ans,mi[i]);
		}
		if(ans==1061109567)ans=0;
		printf("%d\n",ans);
	}
}sam;
int tst;
char c[N];
int main(){
	tst=read()-1;
	scanf("%s",c+1);
	int n=strlen(c+1);
	int las=0;
	sam.init();
	for(int i=1;i<=n;i++){
		las=sam.insert(c[i],las);
	}
	sam.sort1();
	while(tst--){
		scanf("%s",c+1);
		n=strlen(c+1);
		sam.solv(c,n);
	}
	sam.sakura();
	return 0;
}


```

```
输入
3
abcb
bca
acbc

输出
2
```


给定 N (N<=10^5^)个字符集为小写英文字母的字符串，所有字符串的长度和小于 10^5^，求出每个字符串「独特值」(只属于自己的本质不同的字串)。

```c++
struct GSAM{
	int son[N<<1][26];
	int len[N<<1],link[N<<1];
	int siz[N<<1],tot[N<<1],Topu[N<<1],cnt;
	int T[N<<1];
	void init(){
		link[0]=-1;
	}
	int insert(char c,int last,int id){
		int now=last,sonid=c-'a';
		if(son[now][sonid]){
			int q=son[now][sonid];
			if(len[q]==len[now]+1){
				if(T[q])T[q]=-1;
				else T[q]=id;
				return q;
			}
			else{
				int cop=++cnt;
				link[cop]=link[q];
				len[cop]=len[now]+1;
				memcpy(son[cop],son[q],sizeof(son[q]));
				while(now!=-1&&son[now][sonid]==q){
					son[now][sonid]=cop;
					now=link[now];
				}
				link[q]=cop;
				if(T[cop])T[q]=-1;
				else T[cop]=id;
				return cop;
			}
		}else{
			int p=++cnt;
			if(T[p])T[p]=-1;
			else T[p]=id;
			len[p]=len[now]+1;
			while(now!=-1&&son[now][sonid]==0){
				son[now][sonid]=p;
				now=link[now];	
			}
			if(now==-1)link[p]=0;
			else{
				int q=son[now][sonid];
				if(len[q]==len[now]+1)link[p]=q;
				else{
					int cop=++cnt;
					link[cop]=link[q];
					len[cop]=len[now]+1;
					memcpy(son[cop],son[q],sizeof(son[q]));
					while(now!=-1&&son[now][sonid]==q){
						son[now][sonid]=cop;
						now=link[now];
					}
					link[q]=link[p]=cop;
					
				}
			}
			return p;
		}
	}
	
	void sort1(){
		for(int i=1;i<=cnt;i++)tot[len[i]]++;
		for(int i=1;i<=cnt;i++)tot[i]+=tot[i-1];
		for(int i=1;i<=cnt;i++)Topu[tot[len[i]]--]=i;
	}
	int ans[N];
	void solv(int n){
		sort1();
		for(int i=cnt;i;i--){
			int now=Topu[i];
			//cout<<T[now]<<" "<<i<<endl;
			if(T[link[now]]&&T[link[now]]!=-1){
				if(T[now]!=T[link[now]]){
					T[link[now]]=-1;
				}
			}else if(T[link[now]]!=-1){
				T[link[now]]=T[now];
			}
			if(T[now]!=-1){
				ans[T[now]]+=len[now]-len[link[now]];
			}
		}
		for(int i=1;i<=n;i++){
			printf("%d\n",ans[i]);
		}
	}
	
	
}sam; 
int n;
char c[N];
int main(){
	n=read();
	sam.init();
	for(int k=1;k<=n;k++){
		scanf("%s",c+1);
		int len=strlen(c+1);
		int last=0;
		for(int i=1;i<=len;i++){
			last=sam.insert(c[i],last,k);
		}
	}
	sam.solv(n);
	return 0;
}
```

```
输入
3
amy
tommy
bessie

输出
3
11
19
```
