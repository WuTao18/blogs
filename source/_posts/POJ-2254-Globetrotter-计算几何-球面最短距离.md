---
title: POJ 2254 Globetrotter (计算几何 - 球面最短距离)
date: 2019-10-19 22:37:12
tags:
- ACM刷题
- 计算几何
categories: 竞赛
mathjax: true
---

题目链接：[POJ 2254](http://poj.org/problem?id=2254)

## Description
> As a member of an ACM programming team you'll soon find yourself always traveling around the world: Zürich, Philadelphia, San José, Atlanta,... from 1999 on the Contest Finals even will be on a different continent each year, so one day you might get to Japan or Australia.
> 
> At the contest site it would be interesting to know how many miles you are away from home. For this sake, your job is to write a program to compute the geographical distance between two given locations on the Earth's surface.
> 
> We assume that the Earth is a perfect sphere with a radius of exactly 6378 km. The geographical distance between A and B is the length of the geodetic line segment connecting A and B.
> 
> The geodetic line segment between two points on a sphere is the shortest connecting curve lying entirely in the surface of the sphere.
> 
> The value of pi is approximately 3.141592653589793.



## Input
> The input will consist of two parts: a list of cities and a list of queries.
> 
> **City List**
> 
> The city list consists of up to 100 lines, one line per city. Each line will contain a string ci and two real numbers lati and longi, representing the city name, its latitude and its longitude, respectively.
> 
> The city name will be shorter than 30 characters and will not contain white-space characters.
> 
> The latitude will be between -90 (South Pole) and +90 (North Pole). The longitude will be between -180 and +180 where negative numbers denote locations west of the meridian and positive numbers denote locations east of the meridian. (The meridian passes through Greenwich, London.)
> 
> The city list will be terminated by a line consisting of a single "#".
> 
> **Query List**
> 
> Each line will contain two city names A and B.
> 
> The query list will be terminated by the line "# #".
 
## Output
> For each query, print a line saying "A - B" where A and B are replaced by the city names. Then print a line saying x km" where x is replaced by the geographical distance (in km) between the two cities, rounded to the nearest integer.
> 
> If one of the cities in the query didn't occur in the city list, print a line saying "Unknown" instead. Print a blank line after each query.


## Sample Input

```markdown
Ulm		48.700	10.500
Freiburg		47.700	9.500	
Philadelphia	39.883	-75.250
SanJose		37.366	-121.933
NorthPole		90	0
SouthPole		-90	0
#
Ulm Philadelphia
Ulm SanJose
Freiburg Philadelphia
Freiburg SanJose
Ulm Freiburg
SanJose Philadelphia
Ulm LasVegas
Ulm Ulm
Ulm NorthPole
Ulm SouthPole
NorthPole SouthPole
# #
```

## Sample Output

```markdown
Ulm - Philadelphia
6536 km

Ulm - SanJose
9367 km

Freiburg - Philadelphia
6519 km

Freiburg - SanJose
9412 km

Ulm - Freiburg
134 km

SanJose - Philadelphia
4023 km

Ulm - LasVegas
Unknown

Ulm - Ulm
0 km

Ulm - NorthPole
4597 km

Ulm - SouthPole
15440 km

NorthPole - SouthPole
20037 km
```

## Source

[Ulm Local 1997](http://poj.org/searchproblem?field=source&key=Ulm+Local+1997)

## Solution

### 题意

给定一些城市的经纬度，然后给出若干个询问，每个询问包含两个城市，求这两个城市的球面最短距离。

### 思路

已知两点经纬度求球面最短距离的公式：

$AB = R\cdot arccos(cos(wA)cos(wB)cos(jB-jA)+sin(wA)sin(wB))$

其中 $wA$ 和 $jA$ 代表 $A$ 的纬度和经度，$wB$ 和 $jB$ 代表 $B$ 的纬度和经度。

证明见：[关于已知两点经纬度求球面最短距离的公式推导](https://blog.csdn.net/liminlu0314/article/details/8553926)

## Code

```cpp
#include <iostream>
#include <cstdio>
#include <map>
#include <cmath>
#include <algorithm>
#include <cstring>
#include <string>
using namespace std;
typedef long long ll;
typedef double db;  
const db pi = 3.141592653589793;  
const db r = 6378.0;

class Point {
public:
    double j, w;
    Point(double j = 0, double w = 0) : j(j), w(w) {}
    void input() {
        scanf("%lf%lf", &w, &j);
        w = w * pi / 180.0;
        j = j * pi / 180.0;
    }
    db dis(Point a) {
        return r * acos(cos(w) * cos(a.w) * cos(a.j - j) + sin(w) * sin(a.w));
    }
};

map<string, Point> mp;

int main() {
    string s1, s2;
    while((cin >> s1) && s1[0] != '#') {
        Point tmp;
        tmp.input();
        mp[s1] = tmp;
    }
    Point p1, p2;
    while(cin >> s1 >> s2) {
        if(s1[0] == '#' && s2[0] == '#') {
            break;
        }
        cout << s1 << " - " << s2 << endl;
        if(mp.find(s1) != mp.end() && mp.find(s2) != mp.end()) {
            p1 = mp[s1], p2 = mp[s2];
            double ans = p1.dis(p2);
            printf("%.0lf km\n\n", ans);
        } else {
            printf("Unknown\n\n");
        }
    }
    return 0;
}
```
