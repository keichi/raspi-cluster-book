# スパコンプログラミングの基礎

この章では，ベクトルの内積を求めるプログラムを例に，
スーパコンピュータにおけるプログラミングの基礎を説明します．

### サンプルプログラム

下記のソースコードは，100万次元のベクトルaとbの内積を1,000回求めるプログラムです．
配列aとbはそれぞれベクトルaとbに対応し，
実際の内積の計算は`compute()`関数の中で実行されています．
forループの中で配列aとbの要素を1つずつ掛け合わせ，その結果を変数`sum`に加算しています．

```c
#include <stdio.h>

#define TRIALS (1000)
#define ARRAY_SIZE (1000 * 1000)

double a[ARRAY_SIZE];
double b[ARRAY_SIZE];

void compute()
{
    double sum = 0;

    for (int i = 0; i < ARRAY_SIZE; i++) {
        sum += a[i] * b[i];
    }
}

int main(int argc, char *argv[])
{
    for (int i = 0; i < ARRAY_SIZE; i++) {
        a[i] = 1.0;
        b[i] = 2.0;
    }

    for (int i = 0; i < TRIALS; i++) {
        compute();
    }

    return 0;
}
```

### 実行時間の計測

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
以降ではこのプログラムを並列化し，複数のRaspberry Pi上の複数のCPUコアを利用す
ることで，高速化します．

### 参考文献

- [計算科学技術特論A (2019)](https://www.r-ccs.riken.jp/library/event/tokuronA_2019.html)
