---
keywords: Experience Platform;小売販売レシピ;データ科学ワークスペース;人気のあるトピック;レシピ;事前ビルド レシピ
solution: Experience Platform
title: 小売販売レシピ
description: 小売販売レシピを使用すると、特定の期間にシードされたすべての店舗の売上高を予測できます。正確な予測モデルを使用すると、小売業者は、需要および価格設定ポリシーの関係を見い出し、販売と売上高を最大化するために最適化された価格決定をおこなうことができます。
exl-id: ff01fcd1-fca6-4957-8470-a974fd1520aa
source-git-commit: 5d98dc0cbfaf3d17c909464311a33a03ea77f237
workflow-type: tm+mt
source-wordcount: '603'
ht-degree: 88%

---

# 小売販売レシピ

>[!NOTE]
>
>データサイエンスワークスペースは購入できなくなりました。
>
>このドキュメントは、以前に データ Science ワークスペース の利用資格を持つ既存のお客様を対象としています。

小売販売レシピを使用すると、特定の期間にシードされたすべての店舗の売上高を予測できます。正確な予測モデルを使用すると、小売業者は、需要および価格設定ポリシーの関係を見い出し、販売と売上高を最大化するために最適化された価格決定をおこなうことができます。

このドキュメントは、次のような質問に対する回答になります。
* このレシピは誰のために作られたものですか？
* このレシピは何の役に立つのですか？
* どこから始めればよいですか？

## このレシピは誰のために作られたものですか？

現在のマーケットで競争力を維持するために、小売業者は多くの課題に直面しています。ブランドは、小売ブランドの年間売上高を増やそうとしていますが、運用コストを最小限に抑えるために多くの決断が必要です。供給が多すぎると在庫コストが上がり、供給が少なすぎると他のブランドに顧客を奪われるリスクが高まります。今後数ヶ月の供給を増やす必要があるのでしょうか。週別の販売目標を維持するために、商品の最適な価格設定をどのように決めればよいのでしょうか。

## このレシピは何の役に立つのですか？

小売売上高予測レシピでは、機械学習を使用して販売傾向を予測します。このレシピは、過去の豊富な小売データとカスタマイズされた勾配ブースト回帰機械学習アルゴリズムを活用して、1 週間前に販売を予測することでこれを実現します。このモデルでは、過去の購入履歴を利用し、予測精度を高めるために、データサイエンティストが事前に決定した設定パラメーターをデフォルトとして使用します。

## どこから始めればよいですか？

[こちらのチュートリアル](../jupyterlab/create-a-model.md)に従って、作業を開始できます。

このチュートリアルでは、Jupyter ノートブックで 小売 Sales レシピを作成し、そのノートブックを使用してAdobe Experience Platformでレシピの作成レシピ ワークフローについて説明します。

## データスキーマ

このレシピでは、[XDM スキーマ](../../xdm/schema/field-dictionary.md)を使用してデータをモデル化します。このレシピに使用するスキーマを次に示します。

| フィールド名 | タイプ |
| --- | --- |
| date | 文字列 |
| store | 整数 |
| storeType | 文字列 |
| weeklySales | 数値 |
| storeSize | 整数 |
| temperature | 数値 |
| regionalFuelPrice | 数値 |
| markdown | 数値 |
| cpi | 数値 |
| unemployment | 数値 |
| isHoliday | ブール値 |


## アルゴリズム

まず、*DSWRetailSales* スキーマのトレーニングデータセットが読み込まれます。ここから、[勾配ブースト回帰アルゴリズム](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.GradientBoostingRegressor.html)を使用して、モデルをトレーニングします。勾配ブースト回帰では、弱い学習者（少なくともランダムなチャンスより若干良い学習者）が、以前の学習者の弱点の改善に重点を置いた一連の学習者を形成できるという考え方を使用します。これらを組み合わせて、強力な予測モデルを作成できます。

このプロセスには、損失関数、弱い学習者、加法モデルという 3 つの要素が含まれます。

損失関数は、予測結果を予測できるという点で予測モデルがどの程度良いかを示す尺度です。最小二乗回帰がこのレシピで使用されます。

勾配ブースト回帰では、デシジョンツリーが弱い学習者として使用されます。通常、レイヤー、ノード、分割の数が制限されたツリーは、学習者が弱い状態を保つために使用されます。

最後に、加法モデルを使用します。損失関数を使用して損失を計算した後、損失を減らすツリーが選択され、重み付けされ、より難しい観測結果のモデリングが改善されます。次に、重み付けされたツリーの出力を既存のツリーのシーケンスに追加し、モデルの最終的な出力（今後の売上高）を改善します。
