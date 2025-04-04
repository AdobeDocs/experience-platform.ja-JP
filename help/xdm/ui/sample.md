---
solution: Experience Platform
title: UI での XDM スキーマのサンプルデータの生成
description: Adobe Experience Platform ユーザーインターフェイスで既存のスキーマに基づいてサンプル JSON データを生成する方法を説明します。
exl-id: e60eedb2-2245-42cd-b574-43caf9e3426c
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '402'
ht-degree: 14%

---

# UI での XDM スキーマのサンプルデータの生成 {#generate-sample-data-for-an-xdm-schema}

>[!CONTEXTUALHELP]
>id="platform_xdm_downloadsamplefile"
>title="サンプルファイルをダウンロード"
>abstract="選択したスキーマの構造に準拠するサンプル JSON オブジェクトを生成します。このオブジェクトは、このスキーマを使用したデータセットへの取り込みに対してデータが正しく書式設定されていることを確認するためのテンプレートとして機能できます。サンプル JSON ファイルは、ブラウザーによってダウンロードされます。"

データをAdobe Experience Platformに取り込むには、データの形式と構造が既存のエクスペリエンスデータモデル（XDM）スキーマに準拠している必要があります。 特定のデータセットのスキーマの複雑さによっては、データセットが取り込み時に予想するデータの正確な形状を決定することが困難な場合があります。

Experience Platform UI で定義したスキーマについては、スキーマの構造に準拠するサンプル JSON オブジェクトを生成できます。 このオブジェクトは、対象のスキーマを使用するデータセットに取り込まれるすべてのデータのテンプレートとして機能できます。

Experience Platform UI で、左側のナビゲーションから **[!UICONTROL スキーマ]** を選択します。 「**[!UICONTROL 参照]**」タブの下で、サンプルデータを生成するスキーマを見つけます。 リストから選択すると、右側のパネルが更新されて、スキーマの詳細が表示されます。 ここから、「**[!UICONTROL サンプルファイルをダウンロード]**」を選択します。

![ 選択したスキーマと「サンプルファイルをダウンロード」がハイライト表示されたスキーマワークスペースの「参照」タブ。](../images/ui/sample/sample-data.png)

ブラウザーでサンプルの JSON ファイルをダウンロードします。 これで、このファイルを、このスキーマを使用するデータセットに取り込む際のデータの構造化方法の参照として使用できるようになりました。

## 次の手順

このガイドでは、Experience Platform UI で XDM スキーマからサンプル JSON ファイルを生成する方法について説明しました。 スキーマレジストリ API を使用してサンプルデータを生成する方法については、[ サンプルデータエンドポイントガイド ](../api/sample-data.md) を参照してください。

データの取り込みを開始する準備が整ったら、[CSV ファイルの XDM へのマッピング ](../../ingestion/tutorials/map-csv/overview.md) に関するチュートリアルを参照して、フラットデータファイル（CSV など）を XDM スキーマにマッピングし、Experience Platformに取り込む方法を確認してください。 または、[ ソース接続 ](../../sources/home.md) を確立して、外部ソースからデータを取り込み、XDM にマッピングすることもできます。

UI の [!UICONTROL  スキーマ ] ワークスペースの機能について詳しくは、[[!UICONTROL  スキーマ ] ワークスペースの概要 ](./overview.md) を参照してください。
