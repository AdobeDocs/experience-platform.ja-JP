---
keywords: Experience Platform;product purchase recipe;Data Science Workspace;popular topics
solution: Experience Platform
title: 製品購入レシピ
topic: overview
translation-type: tm+mt
source-git-commit: f548fb6431b7bc71c205a2b2b7ca3884e57340b1

---


# 製品購入レシピ

## 概要

製品購入予測イベントを使用すると、特定のタイプの顧客購入レシピ（製品購入など）の確率を予測できます。

![](../images/pre-built-recipes/ppp_bigpicture.png)

次のドキュメントは、
* このレシピは誰のために作られたものですか。
* このレシピは何をするの？

## このレシピは誰のために作られたものですか。

貴社のブランドは、顧客に対して効果的でターゲットを絞ったプロモーションを通じて、商品ラインの四半期別の売り上げを促進しようとしています。 しかし、すべての顧客が似ていて、お金の価値を求めているわけではありません。 誰のターゲット? プロモーションの侵入を見つけずに対応する可能性が最も高い顧客はどれか。 各顧客に対してプロモーションをカスタマイズする方法を教えてください。 どのチャネルに頼り、いつプロモーションを送るべきか。

## このレシピは何をするの？

製品購入予測レシピは、機械学習を利用して顧客の購入行動を予測します。 これを行うには、カスタマイズされたランダムフォレスト分類子と2層のエクスペリエンスデータモデル(XDM)を適用して、購入イベントの確率を予測します。 このモデルは、顧客のプロファイル情報と過去の購入履歴を組み込んだ入力データを利用し、予測精度を高めるために、データサイエンティストが決定した事前に決定された設定パラメータをデフォルトとして使用します。

## データスキーマ

このレシピは [XDMスキーマ](../../xdm/home.md) を使用してデータをモデル化します。 このスキーマに使用するレシピを次に示します。

| フィールド名 | タイプ |
--- | ---
| userId | 文字列 |
| genderRatio | 数値 |
| ageY | 数値 |
| ageM | 数値 |
| optinEmail | Boolean |
| optinMobile | Boolean |
| optinAddress | Boolean |
| created | 整数 |
| totalOrders | 数値 |
| totalItems | 数値 |
| orderDate1 | 数値 |
| shippingDate1 | 数値 |
| totalPrice1 | 数値 |
| tax1 | 数値 |
| orderDate2 | 数値 |
| shippingDate2 | 数値 |
| totalPrice2 | 数値 |


## アルゴリズム

まず、ProductPredictionスキーマのトレーニングデータセッ **トが読み込まれます** 。 ここから、モデルはランダムなフォレスト分類子を使用し [てトレーニングされま](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)す。 ランダムフォレスト分類子は、複数のアルゴリズムを組み合わせて予測性能を向上させるアルゴリズムを参照する、一種の合成アルゴリズムです。 アルゴリズムの背後にある考え方は、ランダムフォレスト分類子が複数のデシジョンツリーを構築し、それらを結合して、より正確で安定した予測を作成することです。

このプロセス開始では、トレーニングデータのサブセットをランダムに選択する一連のデシジョンツリーを作成します。 その後、各デシジョンツリーの結果を平均化する。