# スパコンプログラミングの基礎

この章では，ベクトルの内積を求めるプログラムを例に，
スーパコンピュータにおけるプログラミングの基礎を説明します．

## サンプルプログラム

下記のソースコードは，100万次元のベクトルaとbの内積を1,000回求めるプログラムです．
この計算自体には意味はありませんが，内積をはじめとする様々な線形代数の演算は，
シミュレーションや機械学習に欠かせません．

本ソースコードでは，配列`a`と`b`はそれぞれベクトルaとbに対応し，
内積の計算は`compute()`関数の中で実行しています．
forループの中で配列`a`と`b`の要素を1つずつ掛け合わせ，その結果を変数`sum`に積算しています．

```c
#include <stdio.h>

#define TRIALS (1000)
#define ARRAY_SIZE (1000 * 1000)

// ARRAY_SIZE個の要素を持つ配列aとbを定義
double a[ARRAY_SIZE];
double b[ARRAY_SIZE];

// 内積を計算する関数
void compute()
{
    double sum = 0;

    // aのi番目の要素とbのi番目の要素を乗算し，sumに加算
    for (int i = 0; i < ARRAY_SIZE; i++) {
        sum += a[i] * b[i];
    }
}

// プログラムの開始地点となる関数
int main(int argc, char *argv[])
{
    // 配列aとbを初期化
    for (int i = 0; i < ARRAY_SIZE; i++) {
        a[i] = 1.0;
        b[i] = 2.0;
    }

    // 内積計算をTRIALS回繰り返す
    for (int i = 0; i < TRIALS; i++) {
        compute();
    }

    return 0;
}
```

## 実行時間の計測

まずは，このプログラムを実行し，実行時間を計測してみましょう．
上記のプログラムを`dot.c`というファイル名で保存した後，下記のコマンドで
コンパイルします．

```text
$ gcc -o dot dot.c
```

生成された実行ファイルを実行します．`time`は，後に続くコマンドの実行時間を
計測するコマンドです．

```text
$ time ./dot
```

約27秒かかりました．

```text
real	0m26.886s
user	0m26.824s
sys	0m0.060s
```

このプログラムは1つのRaspberry Pi上の1つのCPUコアしか活用できません．
以降ではこのプログラムを並列化し，複数のRaspberry Pi上の複数のCPUコアを利用することで，高速化します．

## 参考文献

- [計算科学技術特論A (2019)](https://www.r-ccs.riken.jp/library/event/tokuronA_2019.html)
