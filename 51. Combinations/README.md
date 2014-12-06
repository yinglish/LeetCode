这道题算中等难度真的不为过。

要注意，其结果有一个隐含的要求，每一项都是**有序的**（从小到大）。

看那例子：n=4, k=2，我们可以根据规律列个表：

|pos 0 | pos 1|
|------|------|
|1|2|
|1|3|
|1|4|
|2|3|
|2|4|
|3|4|

两个循环貌似就可以搞定：
```cpp
for (int i=1; i<=n; ++i)
    for (int j=i+1; j<=n; ++j)
        retv.push_back({i, j});
```

但这属于 k=2 的特殊情况。如果 k = 3 呢。

|pos 0|pos 1|pos 2|
|-----|-----|-----|
|1|2|3|
|1|2|4|
|1|3|4|
|2|3|4|

貌似就需要三个循环了：
```cpp
for (int i=0; i<=n; ++i)
    for (int j=i+1; j<=n; ++j)
        for (int k=j+1; k<=n; ++k)
            retv.push_back({i, j, k});
```

这不扯了么？k 等于多少，就要循环几次。有没有能够控制循环次数的循环？

有，但你可以想象这样的循环有多么不近人情，而且，这不是典型的**递归**的干活么。。。嗯，就是递归。（为啥需要递归，这时候真需要！）

`retv.push_back({i, j, k})`, 这样的东西无论如何也要改，可以先将 retv 的每一个 vec 初始化为 k 长度，然后使用 `vec[当前循环次数] = 当前循环参数;`。

```cpp
void combine(int i, int n, int k)
{
    while (i<=n)
    {
        vec[vec.size() - k] = i++;
        if (k == 1) retv.push_back(vec);
        else combine(i, n, k-1);
    }
}
```

这就是核心算法了，然后调此递归即可：`combine(1, n, k);`

-----

我的AC使用的是一种"倒着填"的类似方案，我觉得要更简洁一些。但写思路的时候，发现上面那样写，更加符合从 多次循环 到 递归 的思想。更近人情。