---
keywords: Experience Platform;Luma Web データ；Data Science Workspace；人気の高いトピック；レシピ；デモデータ；デモ Web データ；Luma データ
solution: Experience Platform
title: Luma Web スキーマとデータセットの作成
type: Tutorial
description: このチュートリアルでは、Luma デモ傾向モデルに必要な前提条件とアセットについて説明します。
exl-id: a791e532-1116-4407-b745-fd6c2ac0d8f7
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: tm+mt
source-wordcount: '465'
ht-degree: 3%

---

# Luma 傾向モデルのスキーマとデータセットの作成

このチュートリアルでは、その他すべてに必要な前提条件とアセットについて説明します [!DNL Adobe Experience Platform] [!DNL Data Science Workspace] チュートリアル 完了したら、次のスキーマとデータセットを組織と共に使用できるようになります。

**スキーマ:**

- Luma Web データスキーマ
- 傾向モデルのスコアリング結果のスキーマ

**データセット:**

- Luma Web データセット
- 傾向モデルのトレーニングデータセット
- 傾向モデルのスコアリングデータセット
- 傾向モデルのスコアリング結果のデータセット

## アセットをダウンロード {#assets}

次のチュートリアルでは、カスタム Luma 購入傾向モデルを使用します。 先に進む前に [必要なアセットのダウンロード](https://experienceleague.adobe.com/docs/platform-learn/assets/DSW-course-sample-assets.zip?lang=en) zip フォルダー。 このフォルダーには次が含まれます。

- 購入傾向モデルノートブック
- データをトレーニングおよびスコアリングデータセットに取り込むために使用されるノートブック（Luma Web データのサブセット）
- 730,000 人の Luma ユーザーの Web データを含むデモ JSON ファイル
- Web データとモデルの理解に役立つ、オプションの Python 3 EDA（探索的データ分析）ノートブック。

>[!NOTE]
>
> 任意のチュートリアルで、独自のスキーマとデータを使用できます。 ただし、アセットで提供されているデモモデルは、適切な設定ファイルと要件ファイルが提供されていない限り、機能しません。 このデモ傾向モデルは、Luma Web データを操作するように設計されています。

### Luma Web データスキーマの作成とデータの取り込み

モデルを作成するには、モデルのトレーニングとスコアリングに使用される Platform のデータセットが必要です。 次の [Data Science Workspace コース](https://experienceleague.adobe.com/?recommended=ExperiencePlatform-U-1-2021.1.dsw&amp;lang=ja) では、Luma スキーマの作成、および購入傾向モデルで使用されるデータの取り込みに関する手順を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/333312)

### トレーニング、スコアリングおよびスコアリング結果のデータセットの作成

Recipe Builder ノートブックを実行するか、API を使用してモデルをトレーニングおよびスコアリングするには、トレーニング/スコアリングに使用するデータセットとスキーマを指定する必要があります。 次のビデオチュートリアルでは、トレーニング、スコアリング、スコアリングの結果のデータセット、および Luma 購入傾向モデルで使用するスコアリング結果スキーマの設定手順を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/333426)

## 次の手順

このチュートリアルでは、Luma 傾向モデルに必要なスキーマとデータセットを正常に作成しました。 これで、次のチュートリアルに進み、を使用してモデルを作成する準備が整いました。 [recipe builder ノートブック](../jupyterlab/create-a-model.md) チュートリアル

さらに、提供された Exploratory Data Analysis(EDA) ノートブックを使用して、データを調査できます。 このノートブックは、Luma データのパターンを理解し、データの整合性を確認し、予測傾向モデルに関連するデータを要約するのに役立ちます。 Exploratory Data Analysis の詳細については、 [EDA ドキュメント](../jupyterlab/eda-notebook.md).
