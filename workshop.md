# CTF Reversing ワークショップ

## 0. ワークショップの概要

CTFのReversing分野でよく出題される実行可能ファイルの解析問題の基礎と解析ツールの使い方を学びます。

このワークショップはこの5章からなります。

1. CTFのReversing分野の紹介
2. 実行可能ファイルとは
3. ELFファイル解析の基本
4. ELFファイルを解析しながら問題を解いてみる
5. 実行可能ファイルの解析が役に立つ場面の紹介

**開始**ボタンをクリックして次に進みましょう

## 1. CTFのReversing分野の紹介

ReversingはReverse Engineeringとも言われ、機械や製品・プログラムの内部の構造や動作を解析することを指します。

特にCTFではプログラムの動作を解析する問題がよく出題され、例としては

- 実行可能ファイル(Executable file)
  - Linux ELF
  - Windows PE
- モバイルアプリ
  - APK(Android Application Package)
- スクリプトファイル
- pcapファイル
- ファームウェア

このようなプログラムであったり、ファイルを解析します。

## 2. 実行可能ファイルとは

実行可能ファイルとはコンピュータが処理を実行するための命令を含むファイルです。

- Windowsではexe(Windows PEとも呼ばれます)
- 多くのLinux系やBSD系のOSではELF(Executable and Linkable Formatの略)

が著名な形式です。

その中でもCTFではLinux ELFファイルがよく出題されます。

このCloud Shell上でLinux ELFファイルを実際に作ってみて、処理を実行してみましょう。

## 2-1. Linux ELFファイルを実際に作ってみる

<walkthrough-editor-spotlight spotlightId="file-explorer">エクスプローラー</walkthrough-editor-spotlight>の中から`hello.c`をクリックし、表示します。

```cpp
#include <stdio.h>

int main(void) {
  printf("Hello World!\n");
  return 0;
}
```

このような内容が表示されます。

これはC言語で書かれたソースコードで、「Hello World!」と表示するものです。

このコマンドを画面下部のターミナルにコピペして、エンターキーを押してみましょう。

```
gcc hello.c -o hello
```

ターミナルでは何も表示されませんが、<walkthrough-editor-spotlight spotlightId="file-explorer">エクスプローラー</walkthrough-editor-spotlight>では`hello`というファイルが新しくできています。

では同じようにこのコマンドを画面下部のターミナルにコピペして、エンターキーを押してみましょう。

```
./hello
```

すると

```
~/cloudshell_open/reversing-workshop$ ./hello
Hello World!
```

このようにソースコードに記述されていた、「Hello World!」と表示する処理が実行されました!

次のセクションでは`gcc`コマンド・`hello`ファイルの正体を解説します。

## 2-2. gccコマンドとELFファイルとは

`gcc`はコンパイラ・アセンブラ・リンカが搭載されたプログラムで、先程の`gcc hello.c -o hello`コマンドを実行した際にはこのような順番処理が実行されていました。

1. コンパイラはソースコードに記述されたプログラムコードを機械語(=アセンブリ)へ変換します。
2. アセンブラは機械語(=アセンブリ)をオブジェクトファイルへ変換します。
3. リンカはオブジェクトファイルを共有ライブラリとリンクし実行可能ファイルへ変換します。

ということで先程生成された`hello`ファイルは実行可能ファイル、つまりコンピュータが処理を実行するための命令を含むファイルです。

ですので`./hello`というコマンドを実行することで、ソースコード`hello.c`からコンパイラ・アセンブラ・リンカによって変換された実行可能ファイル`hello`の中に含まれる命令が実行されました。

次はこの実行可能ファイルを解析する代表的な手法

1. 表層解析
2. 静的解析
3. 動的解析

について解説していきます。

## 3. ELFファイル解析の基本
