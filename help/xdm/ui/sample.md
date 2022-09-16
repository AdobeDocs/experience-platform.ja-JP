---
solution: Experience Platform
title: UI での XDM スキーマのサンプルデータの生成
description: Adobe Experience Platformユーザーインターフェイスで、既存のスキーマに基づいてサンプルの JSON データを生成する方法を説明します。
topic-legacy: user guide
exl-id: e60eedb2-2245-42cd-b574-43caf9e3426c
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---

# UI での XDM スキーマのサンプルデータの生成

データをAdobe Experience Platformに取り込むには、データの形式と構造が既存の Experience Data Model(XDM) スキーマに準拠している必要があります。 特定のデータセットのスキーマが複雑な場合は、取得時にデータセットが予想するデータの正確な形状を判断するのが困難な場合があります。

Experience PlatformUI で定義したスキーマに対して、スキーマの構造に合ったサンプルの JSON オブジェクトを生成できます。 このオブジェクトは、対象のスキーマを使用するデータセットに取り込まれるデータのテンプレートとして使用できます。

Platform UI で、「 **[!UICONTROL スキーマ]** をクリックします。 以下 **[!UICONTROL 参照]** 「 」タブで、サンプルデータを生成するスキーマを探します。 リストから選択すると、右側のレールが更新され、スキーマの詳細が表示されます。 ここからを選択します。 **[!UICONTROL サンプルファイルをダウンロード]**.

![](../images/ui/sample/sample-data.png)

サンプルの JSON ファイルがブラウザーによってダウンロードされます。 これで、このスキーマを使用するデータセットに取り込む際に、データを構造化する方法のリファレンスとしてこのファイルを使用できます。

## 次の手順

このガイドでは、Platform UI で XDM スキーマからサンプル JSON ファイルを生成する方法について説明しました。 スキーマレジストリ API を使用してサンプルデータを生成する方法については、 [サンプルデータエンドポイントガイド](../api/sample-data.md).

データの取り込みを開始する準備が整ったら、 [CSV ファイルの XDM へのマッピング](../../ingestion/tutorials/map-a-csv-file.md) フラットデータファイル（CSV など）を XDM スキーマにマッピングし、Platform に取り込む方法を説明します。 または、 [ソース接続](../../sources/home.md) を使用して、外部ソースからデータを取り込み、XDM にマッピングします。

の機能の詳細については、 [!UICONTROL スキーマ] UI のワークスペース： [[!UICONTROL スキーマ] workspace の概要](./overview.md).
