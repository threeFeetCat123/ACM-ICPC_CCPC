
#广义后缀自动机（GSAM）


给定 n 个由小写字母组成的字符串 *s~1~*,*s~2~*,*s~3~*...，求本质不同的子串个数。（不包含空串）


```c++

// struct Trie{
// 	int son[N][26];
// 	int fa[N];
// 	int cnt;
// 	void insert(char c[],int slen){
// 		int now=0;
// 		for(int i=1;i<=slen;i++){
// 			int sonid=c[i]-'a';
// 			if(son[now][sonid]==0)son[now][sonid]=++cnt,fa[cnt]=now;
// 			now=son[now][sonid];
// 		}
// 	}
// }tr;

struct GSAM{
	int son[N<<1][26];
	int len[N<<1],link[N<<1];
	int cnt;
	void init(){
		link[0]=-1;
	}
	int insert(char c,int last){
		int sonid=c-'a';
		if(son[last][sonid]){
			int now=last,q=son[now][sonid];
			if(len[q]==len[now]+1)return q;
			else{
				int cop=++cnt;
				link[cop]=link[q];
				len[cop]=len[now]+1;
				memcpy(son[cop],son[q],sizeof(son[cop]));
				while(now!=-1&&son[now][sonid]==q){
					son[now][sonid]=cop;
					now=link[now];
				}
				link[q]=cop;
				return cop;
			}
		}else{
			int now=last,p=++cnt;
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
					memcpy(son[cop],son[q],sizeof(son[cop]));
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
    //bfs建GSAM，需要Trie
	// int pos[N];
	// void build(){
	// 	queue<pair<int,int> >q;
	// 	for(int i=0;i<26;i++){
	// 		if(tr.son[0][i]){
	// 			q.push({i,tr.son[0][i]});
	// 		}
	// 	}
	// 	pos[0]=0;
	// 	while(!q.empty()){
	// 		int now=q.front().second,sonid=q.front().first;
	// 		q.pop();
	// 		pos[now]=insert(sonid,pos[tr.fa[now]]);
	// 		for(int i=0;i<26;i++){
	// 			if(tr.son[now][i]){
	// 				q.push({i,tr.son[now][i]});
	// 			}
	// 		}
	// 	}
	// }
	ll solv(){
		ll ans=0;
		for(int i=1;i<=cnt;i++){
			ans+=len[i]-len[link[i]];
		}
		return ans;
	}
}gsam;
int n;
char c[N];
int main(){
	n=read();
	gsam.init();
	for(int i=1;i<=n;i++){
		int last=0; 
		scanf("%s",c+1);
		int len=strlen(c+1);
		for(int j=1;j<=len;j++){
			last=gsam.insert(c[j],last);
		}
	}
	ll ans=gsam.solv();
	printf("%lld\n%lld\n",ans,gsam.cnt+1);
	return 0;
} 
```