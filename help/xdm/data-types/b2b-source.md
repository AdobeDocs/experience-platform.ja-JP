---
title: B2B ソースデータタイプ
description: このドキュメントでは、B2B ソースエクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: 01b7d41c-1ab6-4cbc-b9b3-77b6af69faf3
source-git-commit: b5cdd72238f7b4519de1c789f4294b9698415327
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 6%

---

# [!UICONTROL B2B ソー] スデータタイプ（ベータ版）

>[!IMPORTANT]
>
>このデータ型は、現在ベータ版のリアルタイム顧客データプラットフォーム B2B エディションの一部です。 ドキュメントと機能は変更される場合があります。

[!UICONTROL B2B ソー] スは、B2B エンティティ ( [アカウント](../classes/b2b/business-account.md)、オポチュニティ [、キャンペーン](../classes/b2b/business-opportunity.md)など ) の複合識別子を表す標準のエクスペリエンスデータモデル (XDM) データ型で [](../classes/b2b/business-campaign.md)す。

文字列ベースの識別子のみに依存する場合、複数のシステム間で ID が重複する場合があります（例えば、1 つの CRM システムで文字列 ID を割り当てる場合は、同じ ID でまったく異なる機会を参照する場合もあります）。 これにより、[ リアルタイム顧客プロファイル ](../../profile/home.md) でデータを結合する際にデータが競合する可能性があります。

[!UICONTROL B2B Source] データ型を使用すると、エンティティの元の文字列 ID を使用し、ソース固有のコンテキスト情報と組み合わせて、元のソースに関係なく、Platform システムで完全に一意の状態を維持できます。

![B2B ソース構造](../images/data-types/b2b-source.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `sourceID` | 文字列 | ソースレコードの一意の ID。 |
| `sourceInstanceID` | 文字列 | ソースデータのインスタンス ID または組織 ID。 |
| `sourceKey` | 文字列 | `sourceId`、`sourceInstanceId` および `sourceType` を組み合わせた一意の識別子を、次の形式で連結します。`[sourceID]@$[sourceInstanceID].[sourceType]`.<br><br>Marketoなどの一部のソースコネクタは、特定の識別子に対してこの値を自動的に連結します。その他は、[ データ準備 `concat` 関数 ](../../data-prep/functions.md#string) を使用して手動で連結する必要があります。例：`concat(id,"@${ORG_ID}.Marketo")` |
| `sourceType` | 文字列 | ソースデータを提供するプラットフォームの名前。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/datatypes/b2b/b2b-source.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/b2b/b2b-source.schema.json)
