#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <set>
#include <map>
#include <vector>
#include <string>
#include <queue>
#include <cctype>
using namespace std;
const int INF = 2147483647;
const int maxn = 200000 + 10;
struct Edge
{
    int from, to, weight;
    void input()
    {
        scanf("%d%d%d", &from, &to, &weight);
    }
    bool operator < (const Edge & t) const
    {
        return weight < t.weight;
    }
};
int fa[maxn];
Edge edges[maxn];
int n, m;
void init()
{
    for(int i = 1; i <= n; i++)
    {
        fa[i] = i;
    }
}
int findset(int u)
{
    return fa[u] == u ? u : fa[u] = findset(fa[u]);
}
bool checkset(int x, int y)
{
    return findset(x) == findset(y);
}
void unionset(int x, int y)
{
    int p1 = findset(x), p2 = findset(y);
    if(p1 == p2)
    {
        return;
    }
    fa[p1] = p2;
}
int Kruskal()
{
    sort(edges, edges + m);
    for(int i = 0; i < m; i++)
    {
        if(!checkset(edges[i].from, edges[i].to))
        {
            unionset(edges[i].from, edges[i].to);
        }
        if(checkset(1, n))
        {
            return edges[i].weight;
        }
    }
}
int main()
{
    scanf("%d%d", &n, &m);
    init();
    for(int i = 0; i < m; i++)
    {
        edges[i].input();
    }
    printf("%d\n", Kruskal());
    return 0;
}
