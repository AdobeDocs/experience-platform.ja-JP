---
title: Common Analytics Plugins 拡張機能のリリースノート
description: Adobe Experience Platform の Common Analytics Plugins タグ拡張機能に関する最新のリリースノートです。
exl-id: 5ea4b709-4e21-4f5d-be99-e72e4889ed99
source-git-commit: c0aa12e9d50e3d1a05a8692a153242f2e6c044ca
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 97%

---

# Common Analytics Plugins のリリースノート

## 2022年6月03日（PT）

### Common Analytics Plugins 拡張機能 3.0.7

#### 機能

* cookie を設定するプラグインで secure フラグが使用されるようになりました。

## 2021 年 6 月 23 日（PT）

### Common Analytics Plugins 拡張機能 3.0.6

#### バグの修正

* 特殊文字を使用すると getPercentPageViewed が壊れる問題を修正しました

## 2021 年 5 月 20 日（PT）

### Common Analytics Plugins 拡張機能 3.0.5

#### バグの修正

* 汎用の初期化アクションを使用すると getTimeParting が正しく初期化されない問題を修正しました

## 2021 年 3 月 26 日（PT）

### Common Analytics Plugins 拡張機能 3.0.4

#### バグの修正

* getPageLoadTime がウィンドウオブジェクトの変数を正しく設定しない問題を修正しました
* クエリ文字列に queryParam が存在しない場合、getQueryParam が &quot;&quot; ではなく未定義を返す問題を修正しました
* 初期化アクションで誤ったバージョン番号が表示される問題を修正しました

## 2021 年 3 月 19 日（PT）

### Common Analytics Plugins 拡張機能 3.0.2

#### 機能

* すべてのプラグインが更新され、バージョン情報がコンテキストデータとして自動的に含まれるようになりました。
* getPercentPageViewed プラグインの追加
* 次のプラグインにデータ要素を追加しました。
   * getGeoCoordinates
   * getNewRepeat
   * getPageName
   * getResponsiveLayout
   * getTimeParting
   * getTimeSinceLastVisit
   * getVisitDuration
   * getVisitNum
* スタイルが新しくなりました

## 2020 年 4 月 9 日（PT）

### Common Analytics Plugins 拡張機能 2.2.0

#### バグの修正

* 拡張機能ビューでの表現を修正しました

#### 機能

* 初期化アクションのドキュメントを更新しました

## 2019 年 12 月 5 日（PT）

### Common Analytics Plugins 拡張機能 2.1.1

#### バグの修正

* バージョン 2.0.X との後方互換性が失われる問題を修正しました
* ドキュメントのリンクが誤ったドキュメントを示す問題を修正しました
* 初期化アクションで `getTimeSinceLastVisit` が 2 回表示される問題を修正しました

## 2019 年 11 月 15 日（PT）

### Common Analytics Plugins 拡張機能 2.1.0

#### バグの修正

* 下位互換性をサポートするために個別プラグインアクションを再導入しました 
* `cleanStr` プラグインの問題を修正しました
* `getResponsiveLayout` プラグインの問題を修正しました
* `getPageName` プラグインの問題を修正しました

#### 機能

* `getTimeParting` のバージョンを更新しました
* `numberSuite` のバージョンを更新しました
* `getNewRepeat` のバージョンを更新しました
* すべてのプラグインに関するドキュメントを更新しました

## 2019 年 10 月 30 日（PT）

### Common Analytics Plugins 拡張機能 2.0.3

#### バグの修正

* ドキュメントのリンクが壊れる問題を修正しました

## 2019 年 10 月 11 日（PT）

### Common Analytics Plugins 拡張機能 2.0.2

#### 機能

* 拡張機能に 15 個のプラグインを追加しました
* 実装を容易にするため、新しい初期化アクションを作成しました。

## 2019 年 7 月 11 日（PT）

### Common Analytics Plugins 拡張機能 1.0.4

#### 機能

*  7 つのプラグインでリリースされた拡張機能
* 個別アクションで各プラグインを初期化する
