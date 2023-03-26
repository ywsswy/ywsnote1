# auto的等价替换
```
for (auto &&b : a)
{
  b.first

for (std::map<xxx>::iterator &&it = a.begin(); it !=a.end(); it++)
{
  std::pair<xxx> &&b = *it;
  b.first
```