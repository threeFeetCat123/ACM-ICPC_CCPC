# 自适应Simpson
```c++
#include<cstdio>
#include<cmath>
typedef long double ld;
const ld eps=1e-8;
//设个差不多的EPS，洗把脸就水过了
ld a,b,c,d;

ld F(ld x){
    //换积分的函数
	return (c*x+d)/(a*x+b);
}

ld simpson(ld l,ld r){
	ld mid=(l+r)/2.0;
	return (r-l)*(F(l)+4.0*F(mid)+F(r))/6.0;
}

ld ASR(ld l,ld r,ld ans,ld eps){
	ld mid=(l+r)/2.0;
	ld lans=simpson(l,mid);
	ld rans=simpson(mid,r);
	ld t=fabs(lans+rans-ans);
	if(t<=eps*15){
		return lans+rans+t/15;
	}else{
		return ASR(l,mid,lans,eps/2)+ASR(mid,r,rans,eps/2);
	}
}

int main(){
	ld L,R;
	scanf("%Lf %Lf %Lf %Lf %Lf %Lf",&a,&b,&c,&d,&L,&R);
	printf("%.6Lf",ASR(L,R,simpson(L,R),eps));
}
```
```
输入
1 2 3 4 5 6 
输出
2.732937
```