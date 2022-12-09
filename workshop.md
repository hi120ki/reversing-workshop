# CTF Reversing ワークショップ

## 0. ワークショップの概要

CTFのReversing分野でよく出題される実行可能ファイルの解析問題の基礎と解析ツールの使い方を学びます。

このワークショップはこの5章からなります。

1. CTFのReversing分野の紹介
2. 実行可能ファイルとは
3. ELFファイル解析の基本
4. ELFファイルを解析しながら問題を解いてみる
5. 実行可能ファイルの解析が役に立つ場面の紹介

**Start**ボタンをクリックして次に進みましょう

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
