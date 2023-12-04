#tarjan求强连通分量
```c++
int dfn[N],low[N],dfncnt;
int top,stk[N],instk[N];
int col,bel[N],w[N],wc[N];
void tarjan(int now){
	dfn[now]=low[now]=++dfncnt;
	stk[++top]=now;
	instk[now]=1;
	for(int i=fir[now];i;i=l[i].nex){
		int to=l[i].to;
		if(!dfn[to]){
			tarjan(to);
			low[now]=min(low[now],low[to]);
		}else if(instk[to]){
			low[now]=min(low[now],dfn[to]);
		}
	}
	if(dfn[now]==low[now]){
		col++;
		while(stk[top]!=now){
			instk[stk[top]]=0;
			//强连通分量的权重 
			wc[col]+=w[stk[top]];
			bel[stk[top--]]=col;	
		}
		wc[col]+=w[stk[top]]; 
		sccw[col]=wc[col];
		instk[now]=0;bel[stk[top--]]=col;
	}
}
```