---
keywords: Experience Platform;luma web データ；データサイエンス Workspace；人気のトピック；レシピ；デモデータ；デモ web データ；luma データ
solution: Experience Platform
title: Luma web スキーマとデータセットの作成
type: Tutorial
description: このチュートリアルでは、Luma デモ傾向モデルに必要な前提条件とアセットを提供します。
exl-id: a791e532-1116-4407-b745-fd6c2ac0d8f7
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 0%

---

# Luma 傾向モデルのスキーマとデータセットの作成

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。
>
>このドキュメントは、Data Science Workspaceの以前の使用権限を持つ既存のお客様を対象としています。

このチュートリアルでは、他のすべての [!DNL Adobe Experience Platform] [!DNL Data Science Workspace] チュートリアルに必要な前提条件とアセットを提供します。 完了すると、ユーザーと組織が次のスキーマとデータセットを使用できるようになります。

**スキーマ：**

- Luma web データスキーマ
- 傾向モデルスコアリング結果スキーマ

**データセット：**

- Luma web データセット
- 傾向モデルトレーニングデータセット
- 傾向モデルスコアリングデータセット
- 傾向モデルスコアリング結果データセット

## アセットのダウンロード {#assets}

次のチュートリアルでは、カスタム Luma の購入傾向モデルを使用します。 先に進む前に、[ 必要なアセットをダウンロード ](https://experienceleague.adobe.com/docs/platform-learn/assets/DSW-course-sample-assets.zip)zip フォルダーを作成します。 このフォルダーには次が含まれます。

- 購入傾向モデルノートブック
- データをトレーニングおよびスコアリングデータセット（Luma web データのサブセット）に取り込むために使用されるノートブック
- 730,000 人の Luma ユーザーの web データを含んだデモ JSON ファイル
- Web データとモデルを理解するのに役立つ、オプションの Python 3 EDA （探索的データ分析）ノートブック。

>[!NOTE]
>
> どのチュートリアルでも、独自のスキーマとデータを使用できます。 ただし、アセットで提供されるデモモデルは、適切な設定ファイルと要件ファイルが提供されていない限り機能しません。 このデモ傾向モデルは、Luma web データを操作するように設計されました。

### Luma web データスキーマの作成とデータの取り込み

モデルを作成するには、モデルのトレーニングとスコアリングに使用するデータセットをExperience Platformに用意する必要があります。 [Data Science Workspace コース ](https://experienceleague.adobe.com/?lang=ja&recommended=ExperiencePlatform-U-1-2021.1.dsw&amp;lang=ja) の次のビデオチュートリアルでは、Luma スキーマを作成し、購入の傾向モデルで使用されるデータを取り込む方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3447155?captions=jpn)

### トレーニング、スコアリングおよびスコアリング結果のデータセットを作成

レシピビルダーノートブックを実行したり、API を使用してモデルのトレーニングとスコアリングを行ったりするには、トレーニング/スコアリングに使用するデータセットとスキーマを指定する必要があります。 次のビデオチュートリアルでは、トレーニング、スコアリング、スコアリング結果の各データセットのほか、Luma の購入傾向モデルで使用されるスコアリング結果スキーマの設定について説明します。

>[!VIDEO](https://video.tv.adobe.com/v/3447422?captions=jpn)

## 次の手順

このチュートリアルでは、Luma 傾向モデルに必要なスキーマとデータセットを正常に作成しました。 次のチュートリアルに進み、「[ レシピビルダーノートブック ](../jupyterlab/create-a-model.md) チュートリアルを使用してモデルを作成する準備が整いました。

さらに、提供された探索的データ分析（EDA）ノートブックを使用してデータを調べることができます。 このノートブックを使用すると、Luma データのパターンを理解し、データのサニティを確認し、予測傾向モデルの関連データの概要を確認できます。 探索的データ分析について詳しくは、[EDA ドキュメント ](../jupyterlab/eda-notebook.md) を参照してください。
