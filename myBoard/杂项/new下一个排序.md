# 下一个序列号
当前序列不存在下一个排列时，函数返回false，否则返回true
next_permutation（node,node+n,cmp）可以对结构体num按照自定义的排序方式cmp进行排序。
```c++
	for(int i=1;i<=4;i++)id[i]=i;
	ll ans=jud();
	while(next_permutation(id+1,id+5)){
		ans=min(ans,jud());
	}
```

```c++
//'A'<'a'<'B'<'b'<...<'Z'<'z'.
int cmp(char a,char b) {
    if(tolower(a)!=tolower(b))//tolower 是将大写字母转化为小写字母.
        return tolower(a)<tolower(b);
    else return a<b;
}


do{
    printf("%s\n",ch);
    while(next_permutation(ch,ch+strlen(ch),cmp));
}
```