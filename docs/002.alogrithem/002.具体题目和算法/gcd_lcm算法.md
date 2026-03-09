# gcd lcm

* lcm 基于gcd

## gcd 的两种算法

* 更相减损术
  * a - b = c ; 然后用大的值减小的 b - c ...d；直到被减数等于 差，差就是gcd;
  * 当然可以提前处理掉 公共的2的倍数，转换成奇数。
* 辗转相除法
  * a % b = c 余 r ;
  * 一个结论是 gcd(a,b) = gcd(b,r) 直到余数为0，被除数就是gcd

```c++
//如果需要自己实现建议用这种方式
int gcd(int a, int b) {
    return b ? gcd(b,a % b):a;
}
```

## lcm 算法

* `lcm(a,b) = a * b / gcd(a,b)`
