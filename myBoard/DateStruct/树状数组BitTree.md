# 树状数组BitTree
## 单点修改，区间查询
```c++
int c[N]; 
//单点修改
void add(int pos,int k){
    for (int i = pos;i <= n;i += i&-i) c[i] += k;
}
//区间查询
int query(int x){
    int ans = 0;
    for (int i = x;i;i -= i&-i) ans += c[i];
    return ans;
}
int query(int l,int r){
    return query(r)-query(l-1);
}
```

## 区间修改，单点查询(差分)
```c++
int c[N]; 
//区间修改
void add(int pos,int k){
    for (int i = pos;i <= n;i += i&-i) c[i] += k;
}

void add(int l,int r,int x){
    add(l,x);
    add(r+1,-x);
}
//单点查询
int query(int x){
    int ans = 0;
    for (int i = x;i;i -= i&-i) ans += c[i];
    return ans;
}
```
## 区间修改，区间查询(没验证)
```c++
int c1[N],c2[N]; 
//区间修改
void add(int pos,int k)
{
    for (int i = pos;i <= n;i += i&-i) c1[i] += k,c2[i] += pos * k;
}
void add(int l,int r,int k){
    add(l,k);
    add(r+1,-k);
}
//区间查询
int query(int pos)
{
    int ans = 0;
    for (int i = pos;i;i -= i&-i)
    {
        ans += (pos + 1) * c1[i]- c2[i];
    }
    return ans;
}
int query(int l,int r){
    return query(r)-query(l-1);
}

```

## 树状数组维护插入、删除、查询（logN）
```c++
int c[N]; 
//插入、删除（权重1）
void add(int pos,int k){
    for (int i = pos;i < N;i += i&-i) c[i] += k;
}
//查询x位置之前有多少个元素
int query(int x){
    int ans = 0;
    for (int i = x;i;i -= i&-i) ans += c[i];
    return ans;
}
//查询当前第k个元素的id
int findk(int k){
	int ans=0,cnt=0;
	for(int i=20;i>=0;i--){
		ans+=1<<i;
		if(ans>n||cnt+c[ans]>=k)ans-=1<<i;
		else cnt+=c[ans];
	}
	return ans+1;
}

```

