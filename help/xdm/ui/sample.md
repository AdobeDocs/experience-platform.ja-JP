---
solution: Experience Platform
title: UI での XDM スキーマのサンプルデータの生成
description: Adobe Experience Platformユーザーインターフェイスの既存のスキーマに基づいて、サンプルの JSON データを生成する方法を説明します。
topic-legacy: user guide
exl-id: e60eedb2-2245-42cd-b574-43caf9e3426c
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---

# UI での XDM スキーマのサンプルデータの生成

データをAdobe Experience Platformに取り込むには、データの形式と構造が既存のエクスペリエンスデータモデル (XDM) スキーマに準拠している必要があります。 特定のデータセットのスキーマが複雑な場合は、取得時にデータセットが予測するデータの正確な形状を判断するのが困難な場合があります。

Experience PlatformUI で定義する任意のスキーマに対して、スキーマの構造に準拠するサンプルの JSON オブジェクトを生成できます。 このオブジェクトは、対象のスキーマを使用するデータセットに取り込まれるデータのテンプレートとして使用できます。

Platform UI で、左のナビゲーションで「**[!UICONTROL スキーマ]**」を選択します。 「**[!UICONTROL 参照]**」タブで、サンプルデータを生成するスキーマを探します。 リストから選択すると、右側のレールが更新され、スキーマに関する詳細が表示されます。 ここから、「**[!UICONTROL サンプルファイルをダウンロード]**」を選択します。

![](../images/ui/sample/sample-data.png)

サンプルの JSON ファイルがブラウザーによってダウンロードされます。 これで、このスキーマを使用するデータセットに取り込む際に、データを構造化する方法のリファレンスとしてこのファイルを使用できます。

## 次の手順

このガイドでは、Platform UI で XDM スキーマからサンプル JSON ファイルを生成する方法について説明しました。 スキーマレジストリ API を使用してサンプルデータを生成する方法については、『[ サンプルデータエンドポイントガイド ](../api/sample-data.md)』を参照してください。

データの取り込みを開始する準備が整ったら、[CSV ファイルの XDM](../../ingestion/tutorials/map-a-csv-file.md) へのマッピングのチュートリアルを参照して、フラットデータファイル（CSV など）を XDM スキーマにマッピングし、Platform に取り込む方法を確認してください。 または、[ ソース接続 ](../../sources/home.md) を確立して、外部ソースからデータを取り込み、XDM にマッピングすることもできます。

UI の [!UICONTROL  スキーマ ] ワークスペースの機能について詳しくは、「[[!UICONTROL  スキーマ ] ワークスペースの概要 ](./overview.md)」を参照してください。
