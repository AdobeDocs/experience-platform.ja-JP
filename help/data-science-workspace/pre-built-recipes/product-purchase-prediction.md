---
keywords: Experience Platform;product purchase recipe;Data Science Workspace;popular topics
solution: Experience Platform
title: 製品の購入レシピ
topic: overview
translation-type: tm+mt
source-git-commit: e08460bc76d79920bbc12c7665a1416d69993f34

---


# 製品の購入レシピ

## 概要

商品購入予測レシピを使用すると、特定のタイプの顧客購入イベント（例えば商品購入）の可能性を予測できます。

![](../images/pre-built-recipes/ppp_bigpicture.png)

次のドキュメントは、次のような質問に対する回答です。
* このレシピは誰のために作られているのですか。
* このレシピは何をするのか。

## このレシピは誰のために作られているのですか。

貴社のブランドが、効果的でターゲットを絞った顧客向けプロモーションを通して、商品ラインの四半期別売上高を促進しようとしています。 しかし、すべての顧客が似ているわけではなく、お金の価値を求めている。 誰のターゲットだ？ プロモーションの邪魔者を見つけることなく、最も反応する可能性の高い顧客はどれか。 各顧客に対するプロモーションをカスタマイズする方法を教えてください。 どのチャネルに頼り、いつプロモーションを送る必要があるか。

## このレシピは何をするのか。

製品購入予測レシピは、機械学習を利用して顧客の購入行動を予測します。 これを行うには、カスタマイズされたランダムフォレスト分類子と2層エクスペリエンスデータモデル(XDM)を適用して、購入イベントの確率を予測します。 このモデルは、顧客プロファイル情報と過去の購入履歴を組み込んだ入力データを利用し、予測精度を高めるために、データサイエンティストが決定した事前に決定された設定パラメータをデフォルトに使用します。

## データスキーマ

このレシピでは、 [XDMスキーマを使用してデータをモデル化します](../../xdm/home.md) 。 このレシピで使用するスキーマを次に示します。

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

最初に、ProductPrediction ** スキーマのトレーニングデータセットが読み込まれます。 ここから、モデルは [ランダムフォレスト分類子を使用してトレーニングされ](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)ます。 ランダムフォレスト分類子は、複数のアルゴリズムを組み合わせて予測パフォーマンスを向上させるアルゴリズムを指す、アンサンブルアルゴリズムの一種です。 アルゴリズムの背後にある考え方は、ランダムフォレスト分類子が複数のデシジョンツリーを構築し、それらを結合して、より正確で安定した予測を作成することです。

このプロセス開始では、トレーニングデータのサブセットをランダムに選択する一連のデシジョンツリーを作成します。 その後、各決定木の結果を平均化する。