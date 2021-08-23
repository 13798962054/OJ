```C
#include <stdio.h>
typedef unsigned long ulong;
ulong dpCalc(ulong rNum, ulong n) {
    ulong dp[11][2] = { 0 };
    int len = 0;
    ulong r_no,tmp;
    dp[0][0] = 0;
    dp[0][1] = 1;
    while (rNum) {
        r_no = rNum % 10;
        ++len;
        if (!n) {
            if (!r_no) {
                if (len == 1) {
                    dp[1][0] = 0;
                }
                else {
                    tmp = len-1;
                    while (tmp > 0) {
                        dp[len][0] += dp[tmp--][1];
                    }
                }
            }
            // 不為0時
            else {
                dp[len][0] = r_no * dp[len - 1][1] + dp[len - 1][0];
            }
        }
        else {
            if (n > r_no) {
                dp[len][0] = dp[len - 1][1] * r_no + dp[len - 1][0];
            }
            else if (n == r_no) {
                dp[len][0] = dp[len - 1][1] * r_no - 1;
            }
            else {
                dp[len][0] = dp[len - 1][1] * (r_no - 1) + dp[len - 1][0];
            }
        }
        dp[len][1] = 9 * dp[len - 1][1];
        rNum /= 10;
    }
    return dp[len][0];
}
int main() {
    ulong l, r, n;
    ulong rst;
    scanf("%lu %lu %lu", &l, &r, &n);
    rst = dpCalc(r, n) - dpCalc(l-1, n);
    printf("%lu\n", rst);
    return 0;
}
```
