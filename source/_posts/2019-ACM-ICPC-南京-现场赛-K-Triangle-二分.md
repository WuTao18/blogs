---
title: 2019 ACM-ICPC 南京 现场赛 K. Triangle (二分)
date: 2019-10-28 22:32:42
tags:
- ACM刷题
- 计算几何
- 二分
categories: 竞赛
mathjax: true
---

## 题意

给定一个三角形和一个点 $p$，如果该点不在三角形边上直接输出 $-1$，否则在三角形上找一点 $q$，使得线段 $pq$ 平分三角形面积。

## 思路

看完题想都没想直接二分了。

就是比赛时复制粘贴的时候改错了，贡献了三发罚时。

板子来自 kuangbin 的模板。

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef double db;
const db eps = 1e-8;
const db inf = 1e20;
const db pi = acos(-1.0);

int sgn(db x){
	if(fabs(x) < eps)return 0;
	if(x < 0)return -1;
	else return 1;
}

struct Point{
	db x, y;
	Point(){}
	Point(db _x, db _y){
		x = _x;
		y = _y;
	}
	void input(){
		scanf("%lf%lf", &x, &y);
	}
	bool operator == (Point b)const{
		return sgn(x-b.x) == 0 && sgn(y-b.y) == 0;
	}
	Point operator -(const Point &b)const{
		return Point(x-b.x, y-b.y);
	}
	//叉积
	db operator ^(const Point &b)const{
		return x*b.y - y*b.x;
	}
	//点积
	db operator *(const Point &b)const{
		return x*b.x + y*b.y;
	}
	//返回两点的距离
	db dis(Point p){
		return hypot(x-p.x, y-p.y);
	}
	Point operator +(const Point &b)const{
		return Point(x+b.x, y+b.y);
	}
	Point operator *(const db &k)const{
		return Point(x*k, y*k);
	}
	Point operator /(const db &k)const{
		return Point(x/k, y/k);
	}
};

struct Line{
	Point s,e;
	Line(){}
	Line(Point _s,Point _e){
		s = _s;
		e = _e;
	}
	// 点在线段上的判断
	bool pointonseg(Point p){
		return sgn((p-s)^(e-s)) == 0 && sgn((p-s) * (p-e)) <= 0;
	}
	
	// 求两直线的交点
	Point crosspoint(Line v){
		double a1 = (v.e-v.s)^(s-v.s);
		double a2 = (v.e-v.s)^(e-v.s);
		return Point((s.x*a2-e.x*a1)/(a2-a1),(s.y*a2-e.y*a1)/(a2-a1));
	}
};

// 求点a和点b的中点
Point get_mid(Point a, Point b) {
    return (a + b) * 0.5;
}

int main() {
    int T;
    scanf("%d", &T);
    while(T--) {
        Point a, b, c, p;
        Line ab, bc, ac;
        a.input(), b.input(), c.input(), p.input();
        ab = Line(a, b); bc = Line(b, c); ac = Line(a, c);
        db area = fabs((b - a) ^ (c - a) * 0.5);  // 三角形面积
        if(ab.pointonseg(p)) {  // 点p在线段ab上
            if(a.dis(p) < b.dis(p)) {  // 点p靠近点a，则另一点一定在线段bc上
                Point l = c, r = b;
                Point mid = get_mid(l, r);
                int times = 1000;
                while(times--) {
                    mid = get_mid(l, r);
                    db s = fabs((mid - p) ^ (b - p));
                    if(sgn(s - area) > 0) {
                        l = mid;
                    } else if(sgn(s - area) == 0) {
                        break;
                    } else {
                        r = mid;
                    }
                }
                printf("%.10lf %.10lf\n", mid.x, mid.y);
            } else {  // 点p靠近点b，则另一点一定在线段ac上
                Point l = c, r = a;
                Point mid = get_mid(l, r);
                int times = 1000;
                while(times--) {
                    mid = get_mid(l, r);
                    db s = fabs((mid - p) ^ (a - p));
                    if(sgn(s - area) > 0) {
                        l = mid;
                    } else if(sgn(s - area) == 0) {
                        break;
                    } else {
                        r = mid;
                    }
                }
                printf("%.10lf %.10lf\n", mid.x, mid.y);
            }
        } else if(bc.pointonseg(p)) {  // 另外两种情况复制粘贴就行了
            if(b.dis(p) < c.dis(p)) {
                Point l = a, r = c;
                Point mid = get_mid(l, r);
                int times = 1000;
                while(times--) {
                    mid = get_mid(l, r);
                    db s = fabs((mid - p) ^ (c - p));
                    if(sgn(s - area) > 0) {
                        l = mid;
                    } else if(sgn(s - area) == 0) {
                        break;
                    } else {
                        r = mid;
                    }
                }
                printf("%.10lf %.10lf\n", mid.x, mid.y);
            } else {
                Point l = a, r = b;
                Point mid = get_mid(l, r);
                int times = 1000;
                while(times--) {
                    mid = get_mid(l, r);
                    db s = fabs((mid - p) ^ (b - p));
                    if(sgn(s - area) > 0) {
                        l = mid;
                    } else if(sgn(s - area) == 0) {
                        break;
                    } else {
                        r = mid;
                    }
                }
                printf("%.10lf %.10lf\n", mid.x, mid.y);
            }
        } else if(ac.pointonseg(p)) {
            if(a.dis(p) < c.dis(p)) {
                Point l = b, r = c;
                Point mid = get_mid(l, r);
                int times = 1000;
                while(times--) {
                    mid = get_mid(l, r);
                    db s = fabs((mid - p) ^ (c - p));
                    if(sgn(s - area) > 0) {
                        l = mid;
                    } else if(sgn(s - area) == 0) {
                        break;
                    } else {
                        r = mid;
                    }
                }
                printf("%.10lf %.10lf\n", mid.x, mid.y);
            } else {
                Point l = b, r = a;
                Point mid = get_mid(l, r);
                int times = 1000;
                while(times--) {
                    mid = get_mid(l, r);
                    db s = fabs((mid - p) ^ (a - p));
                    if(sgn(s - area) > 0) {
                        l = mid;
                    } else if(sgn(s - area) == 0) {
                        break;
                    } else {
                        r = mid;
                    }
                }
                printf("%.10lf %.10lf\n", mid.x, mid.y);
            }
        } else {
            printf("-1\n");
        }
    }

    return 0;
}
```

赛后发现直接推也可以，也不用写这么长的代码了 (~~虽然大部分是复制粘贴~~)。~~感觉在演队友~~。