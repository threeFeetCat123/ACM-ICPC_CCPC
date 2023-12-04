#AC自动机

给你一个文本串**S**和 n 个模式串**T~1...n~，请你分别求出每个模式串 T~i~ 在 S 中出现的次数
```c++
#include<bits/stdc++.h>
using namespace std;
const int maxn=2e6+9;
struct Trie{
	int son[26],cnt;
}trie[maxn]; 
struct line{
	int to,nex;
}l[maxn];
int cnt,root;
int fir[maxn],fail[maxn],num[maxn],id[maxn];
void add(int fr,int to){
	cnt++;
	l[cnt].to=to;
	l[cnt].nex=fir[fr];
	fir[fr]=cnt;
} 
void insert(char s[],int idd){
	int now=0;
	int len=strlen(s+1);
	for(int i=1;i<=len;i++){
		int t=s[i]-'a';
		if(trie[now].son[t]==0)trie[now].son[t]=++root;
		now=trie[now].son[t];
	}
	trie[now].cnt++;
	id[idd]=now;
}

void buildFail(){
	queue<int>q;
	for(int i=0;i<26;i++){
		if(trie[0].son[i]){
			add(0,trie[0].son[i]);
			fail[trie[0].son[i]]=0;
			q.push(trie[0].son[i]);
		}
	}
	while(!q.empty()){
		int now=q.front();
		q.pop();
		//cout<<"now:  "<<now<<endl;
		for(int i=0;i<26;i++){
			int Son=trie[fail[now]].son[i];
			if(trie[now].son[i]){
				//cout<<Son<<endl;
				fail[trie[now].son[i]]=Son;
				add(Son,trie[now].son[i]);
				q.push(trie[now].son[i]);
			}else{
				trie[now].son[i]=Son;
			}
		}
	}
}

void query(char s[]){
	int len=strlen(s+1);
	int now=0;
	for(int i=1;i<=len;i++){
		int t=s[i]-'a';
		now=trie[now].son[t];
		//cout<<"now:   "<<now<<endl;
		num[now]++;
	}
}

void dfs(int now){
	//cout<<now<<"    "<<num[now]<<endl;
	for(int i=fir[now];i;i=l[i].nex){
		int to=l[i].to;
		dfs(to);
		num[now]+=num[to];
	}
}
int n;
char str[maxn];
int main(){
	scanf("%d",&n);
	for(int i=1;i<=n;i++){
		scanf("%s",str+1);
		insert(str,i);
	}
	scanf("%s",str+1);
	buildFail();
	query(str);
	dfs(0);
	for(int i=1;i<=n;i++){
		printf("%d\n",num[id[i]]);
	}
	return 0;
}
```