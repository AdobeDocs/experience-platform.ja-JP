---
solution: Experience Platform
title: UIでのXDM スキーマのサンプルデータの生成
description: Adobe Experience Platform ユーザーインターフェイスの既存のスキーマに基づいて、サンプル JSON データを生成する方法について説明します。
exl-id: e60eedb2-2245-42cd-b574-43caf9e3426c
source-git-commit: 67ae12b0a410d50c25f4e044b8430b70249670eb
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 13%

---

# UI での XDM スキーマのサンプルデータの生成 {#generate-sample-data-for-an-xdm-schema}

>[!CONTEXTUALHELP]
>id="platform_xdm_downloadsamplefile"
>title="サンプルファイルをダウンロード"
>abstract="選択したスキーマの構造に準拠するサンプル JSON オブジェクトを生成します。このオブジェクトは、このスキーマを使用したデータセットへの取り込みに対してデータが正しく書式設定されていることを確認するためのテンプレートとして機能できます。サンプル JSON ファイルは、ブラウザーによってダウンロードされます。"

データをAdobe Experience Platformに取り込むには、データのフォーマットと構造が、既存のExperience Data Model （XDM）スキーマに準拠している必要があります。 特定のデータセットのスキーマの複雑さによっては、取り込み時にデータセットが期待するデータの正確な形状を判断するのが難しい場合があります。

Experience Platform UIで定義した任意のスキーマに対して、スキーマの構造に準拠するサンプル JSON オブジェクトを生成できます。 このオブジェクトは、スキーマを使用するデータセットに取り込まれるあらゆるデータのテンプレートとして機能します。

>[!NOTE]
>
>**削除**&#x200B;や&#x200B;**JSON構造をコピー**&#x200B;などのアクションが見つからない場合は、カスタム （テナント定義）リソースを使用して、テーブル行メニューまたは詳細ビュー（**[!UICONTROL More]**）からアクセスしていることを確認してください。 アクションの可用性は、権限と使用制限によっても異なります。 [&#x200B; スキーマ、クラス、フィールドグループ、およびデータタイプの管理：アクションと削除](./explore.md#xdm-resource-actions)を参照してください。

Experience Platform UIで、左側のナビゲーションで「**[!UICONTROL Schemas]**」を選択します。 「**[!UICONTROL Browse]**」タブで、サンプルデータを生成するスキーマを見つけます。 リストから選択すると、右側のパネルが更新され、スキーマに関する詳細が表示されます。 ここから **[!UICONTROL Download sample file]** を選択します。 

![&#x200B; スキーマを選択してサンプルファイルをダウンロードしたスキーマワークスペースの「参照」タブがハイライト表示されます。](../images/ui/sample/sample-data.png)

サンプル JSON ファイルがブラウザーによってダウンロードされます。 このファイルを、このスキーマを使用するデータセットに取り込む際にデータを構造化する方法のリファレンスとして使用できるようになりました。

## 次の手順

このガイドでは、Experience Platform UIでXDM スキーマからサンプル JSON ファイルを生成する方法について説明しました。 Schema Registry APIを使用してサンプルデータを生成する方法については、[&#x200B; サンプルデータエンドポイントガイド &#x200B;](../api/sample-data.md)を参照してください。

データの取り込みを開始する準備ができたら、[&#x200B; フラットデータファイル（CSVなど）をXDM スキーマにマッピングしてExperience Platformに取り込む方法については、](../../ingestion/tutorials/map-csv/overview.md)のチュートリアルを参照してください。 または、[&#x200B; ソース接続](../../sources/home.md)を確立して、外部ソースからデータを取り込み、XDMにマッピングすることもできます。

UIの[!UICONTROL Schemas] ワークスペースの機能について詳しくは、[[!UICONTROL Schemas] ワークスペースの概要](./overview.md)を参照してください。
