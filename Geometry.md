##Geometry
<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[Leetcode:Max Points on a Line](#Anchor1)  
-[Outer Points of Rectangles](#Anchor2)  
-[*TODO*::Overlapped Rectangles Area](#Anchor3) 

-------
<a name="Anchor1" id="Anchor1"></a>
-**[Leetcode:Max Points on a Line](http://oj.leetcode.com/problems/max-points-on-a-line/)**([Back to Index](#AnchorIndex))  
对于i、j两个点，看点k是否在i、j确定的直线上。有两种情况：

    * 该直线的斜率无法求出，则判断i、j、k三点横坐标是否相同
    * 三点中任意两点直线斜率相同

避免重复计算：遍历点k时，先计算出后面需要用到的几个数。

```cpp
class Solution {
public:

    int maxPoints(vector<Point> &points) {
        if (points.size() == 0)
            return 0;
        if (points.size() == 1)
            return 1;
        int result = 0;
        for (int i = 0; i < points.size(); i++) {
            for (int j = i + 1; j < points.size(); j++) {
                bool flag = false;
                int cnt = 0;
                int a, b, c, d;
                if (points[i].x == points[j].x)
                    flag = true;
                else {
                    a = points[j].x * points[i].y;
                    b = points[i].x * points[j].y;
                    c = points[j].x - points[i].x;
                    d = points[j].y - points[i].y;
                }
                for (int k = 0; k < points.size(); k++) {
                    if (flag && points[k].x == points[i].x)
                        cnt++;
                    if (!flag && (a + d * points[k].x == b + c * points[k].y))
                        cnt++;
                }
                result = max(result, cnt);
            }
        }
        return result;
    }
};
```

-------
<a name="Anchor2" id="Anchor2"></a>
-**[Outer Points of Rectangles]**([Back to Index](#AnchorIndex))   

给定输入为vector< Rectangle >，其中Rectangle的成员需要包括int x1,int x2和int y，也即在x-y坐标系中，所有Rectangles的底边都在x轴上，因此依靠这3个int值即可唯一确定Rectangle。现要求所有Rectangles组成的图形的外围点集。注意给定的Rectangle已按照x1排序。    
例如，对于输入：    
Rectangle 1:(-4,2,2)  
Rectangle 2:(-3,1,1)  
Rectangle 3:(-3,-1,3)  
Rectangle 4:(0,2,2)  
Rectangle 5:(2,4,1)  
输出点集：  
(-4,0),(-3,2),(-3,3),(-1,3),(-1,1),(0,1),(0,2),(2,2),(2,1),(4,1),(4,0)  

对于每个点(矩形的顶点)是否能形成外点取决于该点属于的Retangles区间。例如对于点(a,b)属于2个Retangles区间，也即Retangle 1:(x11,x12,y1)和Rectangle 2:(x21,x22,y2)其中x11<=a<=x12,x21<=a<=x22,如果b>=max(y1,y2)，则点(a,b)是外点，且可能额外生成一个交点(a,max(y1,y2))，这个点也是外点。因此问题就转变为对于每个点，如何维持该点所属Retangles区间，考虑使用set。set的底层实现是RBT，对于动态的维护一个集合的最值比较适合，每次操作代价仅为logn。具体做法是：
*  如果该点是某矩形的第一个点（左上角），则计算该点是否是外点，并决定是否生成额外的外点，然后将该矩形加入set，并更新当前最大值  
*  如果该点是某矩形的第二个点（右上角），则在set中查找该矩形，删除它并更新当前最大值，然后计算该点是否是外点，并决定是否生成额外的外点  

注意对Rectangle的"=="和"<"进行重载。

代码如下：
```cpp
#include <iostream>
#include <algorithm>
#include <set>

using namespace std;

class Rectangle{
public:
    int x1;
    int x2;
    int y;
    Rectangle(int _x1, int _x2, int _y){
        x1 = _x1;
        x2 = _x2;
        y = _y; 
    }
    
    bool operator == (const Rectangle & r) const{
        if(x1 == r.x1 && x2 == r.x2 && y == r.y){
            return true;
        }else{
            return false;
        }
    }
    
    bool operator < (const Rectangle & r) const{
        if(y > r.y){//descending
            return true;
        }else{
            return false;
        }
    }
};

class Point{
public:
    int x;
    int y;
    int id;//1 or 2
    Rectangle * rec;
    Point(int _x, int _y, int _id,Rectangle* _rec){
        x = _x;
        y = _y;
        id = _id;
        rec = _rec;
    }
};

bool cmp(const Point* p1, const Point* p2){
    if(p1->x < p2->x || (p1->x == p2->x && p1->y < p2->y)){
        return true;
    }else{
        return false;
    }
}

int main(){ 
    vector<Rectangle*> inputVec;
    inputVec.push_back(new Rectangle(-4,-2,2));
    inputVec.push_back(new Rectangle(-3,1,1));
    inputVec.push_back(new Rectangle(-3,-1,3));
    inputVec.push_back(new Rectangle(0,2,2));
    inputVec.push_back(new Rectangle(2,4,1));
    
    vector<Point*> pointVec;
    for(int i = 0; i <= (int)(inputVec.size()-1); i++){
        pointVec.push_back(new Point(inputVec[i]->x1,inputVec[i]->y,1,inputVec[i]));
        pointVec.push_back(new Point(inputVec[i]->x2,inputVec[i]->y,2,inputVec[i]));
    }
    
    // sort points
    sort(pointVec.begin(),pointVec.end(),cmp);
    
    set<Rectangle> recSet; // RBT
    recSet.clear();
    
    vector<Point*> ret;// answer
    ret.clear();
    
    int currentMaxHeight = 0;//current Highest
    
    for(int i = 0; i <= (int)(pointVec.size()-1); i++){
        if(pointVec[i]->id == 1){
            if(pointVec[i]->y > currentMaxHeight){
                ret.push_back(new Point(pointVec[i]->x,currentMaxHeight,0,NULL));
                ret.push_back(pointVec[i]);
            }else if(pointVec[i]->y == currentMaxHeight){
                ret.push_back(pointVec[i]);
            }
            recSet.insert(*(pointVec[i]->rec));
            currentMaxHeight = (*(recSet.begin())).y;//update
        }else if(pointVec[i]->id == 2){
            set<Rectangle>::iterator itr = recSet.find(*(pointVec[i]->rec));
            if(itr != recSet.end()){
                recSet.erase(itr);
            }
            if(recSet.empty()){
                currentMaxHeight = 0;
            }else{
                currentMaxHeight = (*(recSet.begin())).y;//update
            }
            
            if(pointVec[i]->y > currentMaxHeight){
                ret.push_back(pointVec[i]);
                ret.push_back(new Point(pointVec[i]->x,currentMaxHeight,0,NULL));
            }else if(pointVec[i]->y == currentMaxHeight){
                ret.push_back(pointVec[i]);
            }
        }
    }
    
    // output
    for(int i = 0; i <= (int)(ret.size()-1); i++){
        cout << "[" << ret[i]->x << "," << ret[i]->y << "]" << endl;
    }
}
```

-------
<a name="Anchor3" id="Anchor3"></a>
-**[Overlapped Rectangles Area]**([Back to Index](#AnchorIndex))

在二维平面中存在若干个矩形，这些矩形可能有交叠，给定每个矩形的右上角及左下角坐标，求这些矩形的总面积（交叠部分只计算一次）。  

可以利用切割的思想进行，如下图所示：
![Overlapped Rectangles Area](Image/Overlapped Rectangles Area.png "Overlapped Rectangles Area") 

首先分别获得这些矩形的坐标集合setx和sety，从而确定了分割的基准线。接下来对setx和sety中的值进行排序，这样做的目的是在接下来的算法中对于x和y每次能够选择相邻的基准线，从而获得最小的矩形。对小矩形进行判断，看是否在输入给定的任意一个矩形中，如果在，则其面积应该计算在最后的总和中；如果不在任意一个矩形中，则抛弃该矩形。

算法实现如下：
```cpp
class Rect{
    public:
    int x1,y1,x2,y2;
    Rect(int _x1, int _y1, int _x2, int _y2):x1(_x1),y1(_y1),x2(_x2),y2(_y2){}
    ~Rect();
}

int CalculateTotalArea(Vector<Rect> inputRects){
    int n = inputRects.size();
    if(n <= 0) return 0;    
    set<int> setx, sety;
    // collect setx and sety
    for(int i = 0; i <= n-1; i++){
        setx.insert(inputRects[i].x1);
        setx.insert(inputRects[i].x2);
        sety.insert(inputRects[i].y1);
        sety.insert(inputRects[i].y2);
    }
    vector<int> vecx,vecy;
    for(set<int>::iterator itr = setx.begin(); itr != setx.end(); itr++){
        vecx.push_back(*itr);
    }
    sort(vecx.begin(),vecx.end());
    for(set<int>::iterator itr = sety.begin(); itr != sety.end(); itr++){
        vecy.push_back(*itr);
    }
    sort(vecy.begin(),vecy.end());

    int totalArea = 0;
    for(int i = 0; i <= vecx.size()-2; i++){
        for(int j = 0; j <= vecy.size()-2; j++){
            Rect smallRect(vecx[i],vecy[j],vecx[i+1],vecy[j+1]);
            if(judgeSmallRect(inputRects, smallRect)){
                totalArea += (vecx[i+1] - vecx[i]) * (vecy[j+1] - vecy[j]);
             }
        }
    }

    return totalArea;
}

bool judgeSmallRect(vector<int> inputRects, Rect smallRect){
    for(int i = 0; i <= (int)inputRects.size()-1; i++){
        if(smallRect.x1 <= inputRects[i].x1 && smallRect.x1 >= inputRects[i].x2 && smallRect.x2 <= inputRects[i].x1 && smallRect.x2 >= inputRects[i].x1 && smallRect.y1 <= inputRects[i].y1 && smallRect.y1 >= inputRects[i].y2 && smallRect.y2 <= inputRects[i].y1 && smallRect.y2 >= inputRects[i].y2){
            return true;
        }
    }
    return false;
}
```