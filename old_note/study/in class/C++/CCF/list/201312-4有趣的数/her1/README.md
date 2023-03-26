#include <iostream>
#include <cstdio>
#include <cstring>
#include <string>
#include <vector>
#include <deque>
#include <list>
using namespace std;
long long f[2000][3][2]; // f[seq_k to place][0: to place 0 , 1: ethier 0 or 1, 2 : must be 1][3 is placed ?
1 : 0]
int dp(int n, int p1, int p3)
{
long long &now = f[n][p1][p3];
if (now != -1)
return now;
if (n == 0)
{
if (p1 == 2 && p3 == 1)
{
now = 1;
}else
{
now = 0;
}
return now;
}
now = 0;
if (p1 == 0)
{
now += dp(n-1, 1, p3); // go 0
}else if (p1 == 1)
{
now += dp(n-1, 1, p3); // go 0
now += dp(n-1, 2, p3); // go 1
}else // p1 == 2
{
now += dp(n-1, 2, p3); // go 1
}
if (p3 == 0)
{
now += dp(n-1, p1, p3); // go 2;
now += dp(n-1, p1, 1); // go 3;
}else
{
now += dp(n-1, p1, 1); // go 3;
}
now %= 1000000007;
}
int main()
{
int n;
cin >> n;
memset(f, -1, sizeof(f));
int ans = dp(n - 1, 0, 0); // seq[n] is 2
cout << ans << endl;
return 0;
}
