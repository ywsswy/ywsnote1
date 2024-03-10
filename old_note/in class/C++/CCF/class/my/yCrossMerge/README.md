//交叉拼接、涂色、不翻转
#include<iostream>
#include<iomanip>
#include<string>
#include<sstream>
#include<map>
#include<list>
#include<vector>
#include<algorithm>
#include<cmath>
using namespace std;
//ma是一组被涂色的线段<每一段起点，终点>,h和t是这次要涂色的起点，终点，调用前记得判断起点终点相同的问题
void yCrossMerge(map<int, int> &ma, const int h, const int t) {
    int head = min(h, t);
    int tail = max(h, t);
    map<int, int>::iterator itm = ma.begin();
    int size = ma.size();
    int i = 0;
    for (; i < size; i++, itm++) {
        if (itm->second + 1 < h) {
            continue;
        } else {
            head = min(head, itm->first);
            break;
        }
    }
    while (i < size) {
        if (itm->first - 1 > tail) {
            break;
        } else {
            tail = max(tail, itm->second);
            itm++;
            map<int, int>::iterator itbuf = itm;
            itm--;
            ma.erase(itm);
            itm = itbuf;
            i++;
        }
    }
    ma[head] = tail;
    return;
}
int main() {
    int gn = 0;
    cin >> gn;
    vector<map<int, int> > gve1; //vector[row](0s)
    gve1.resize(101);
    for (int i = 0; i < gn; i++) {
        int x1 = 0;
        int y1 = 0;
        int x2 = 0;
        int y2 = 0;
        cin >> x1 >> y1 >> x2 >> y2;
        for (int j = y1; j < y2; j++) {
            yCrossMerge(gve1[j], x1, x2 - 1);
        }
    }
    int gtotal = 0;
    for (int i = 0; i < 101; i++) {
        map<int, int>::iterator itm = gve1[i].begin();
        int size = gve1[i].size();
        for (int j = 0; j < size; j++, itm++) {
            gtotal += (itm->second - itm->first + 1);
        }
    }
    cout << gtotal << endl;
    return 0;
}

