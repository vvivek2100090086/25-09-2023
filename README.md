Question:https://codeforces.com/problemset/problem/1776/N
          
          
          // This Code was made by Chinese_zjc_.
          #include <bits/stdc++.h>
          // #define debug
          int n, h[100005], c[100005];
          std::string s;
          struct node
          {
              double v;
              int lg;
              node() : v(), lg() {}
              node(double x) : v(x), lg()
              {
                  work();
              }
              void work()
              {
                  if (v == 0)
                      return;
                  while (v < 0.5)
                      v *= 2.0, --lg;
                  while (v > 1.0)
                      v *= 0.5, ++lg;
              }
              node &operator+=(const node &X)
              {
                  if (X.v == 0)
                      return *this;
                  if (v == 0)
                      return *this = X;
                  if (lg - X.lg > 75)
                      return *this;
                  if (X.lg - lg > 75)
                      return *this = X;
                  node x = X;
                  while (x.lg < lg)
                      x.v *= 0.5, ++x.lg;
                  while (x.lg > lg)
                      x.v *= 2.0, --x.lg;
                  v += x.v;
                  work();
                  return *this;
              }
              node &operator*=(const node &X)
              {
                  v *= X.v;
                  lg += X.lg;
                  work();
                  return *this;
              }
          } dp[100005];
          signed main()
          {
              std::ios::sync_with_stdio(false);
              std::cout << std::fixed << std::setprecision(10);
              std::cin >> n >> s;
              // s = std::string((n - 1) / 2, '<') + std::string((n - 1) - (n - 1) / 2, '>');
              int x = 1 + std::count(s.begin(), s.end(), '<'), y = 1;
              h[x] = std::max(h[x], y);
              c[y] = std::max(c[y], x);
              for (auto i : s)
              {
                  if (i == '<')
                      --x;
                  else
                      ++y;
                  h[x] = std::max(h[x], y);
                  c[y] = std::max(c[y], x);
              }
              dp[c[1]] = 1;
              for (int i = 1; i <= h[1]; ++i)
              {
                  dp[c[i] + 1] = 0;
                  int cnt = 0;
                  for (int j = c[i]; j; --j)
                  {
                      if (j <= c[i] - 500 && i <= h[j] - 500)
                          ++cnt;
                      if (cnt > 500)
                          break;
                      dp[j] += dp[j + 1];
                      dp[j] *= 1.0 / (c[i] + h[j] - i - j + 1);
                  }
              }
              double res = dp[1].lg + std::log2(dp[1].v);
              for (int i = 1; i <= n; ++i)
                  res += std::log2(i);
              std::cout << res << std::endl;
              return 0;
          }
