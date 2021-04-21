---
solution: Experience Platform
title: UIでのXDMスキーマのサンプルデータの生成
description: Adobe Experience Platformユーザーインターフェイスの既存のスキーマに基づいてサンプルJSONデータを生成する方法を学びます。
topic-legacy: user guide
exl-id: e60eedb2-2245-42cd-b574-43caf9e3426c
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---

# XDMスキーマのサンプルデータをUIに生成する

データをAdobe Experience Platformに取り込むには、データの形式と構造が既存のExperience Data Model(XDM)スキーマに準拠している必要があります。 特定のデータセットのスキーマの複雑さに応じて、取り込み時にデータセットが予測するデータの正確な形状を判断するのは困難な場合があります。

Experience PlatformUIで定義するスキーマに対して、スキーマの構造に適合するサンプルJSONオブジェクトを生成できます。 このオブジェクトは、該当するスキーマを使用するデータセットに取り込まれるデータのテンプレートとして使用できます。

Platform UIの左側のナビゲーションで「**[!UICONTROL スキーマ]**」を選択します。 「**[!UICONTROL 参照]**」タブで、サンプルデータを生成するスキーマを探します。 リストから選択すると、右側のパネルが更新され、スキーマの詳細が表示されます。 ここから、「**[!UICONTROL サンプルファイル]**&#x200B;をダウンロード」を選択します。

![](../images/ui/sample/sample-data.png)

サンプルのJSONファイルがブラウザーによってダウンロードされます。 このスキーマを使用するデータセットに取り込む際に、データの構造化方法のリファレンスとしてこのファイルを使用できます。

## 次の手順

このガイドでは、プラットフォームUIのXDMスキーマからサンプルのJSONファイルを生成する方法について説明しました。 スキーマレジストリAPIを使用してサンプルデータを生成する方法については、[サンプルデータエンドポイントガイド](../api/sample-data.md)を参照してください。

データを取り込む開始の準備が整ったら、[CSVファイルをXDM](../../ingestion/tutorials/map-a-csv-file.md)にマッピングする方法のチュートリアルを参照し、フラットデータファイル（CSVなど）をXDMスキーマにマップしてPlatformに取り込む方法を確認してください。 または、[ソース接続](../../sources/home.md)を確立して、外部ソースからデータを取り込み、XDMにマッピングすることもできます。

UIの[!UICONTROL スキーマ]ワークスペースの機能について詳しくは、[[!UICONTROL スキーマ]ワークスペースの概要](./overview.md)を参照してください。
