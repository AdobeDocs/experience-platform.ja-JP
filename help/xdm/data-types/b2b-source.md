---
title: B2Bソースデータタイプ
description: このドキュメントでは、B2Bソースエクスペリエンスデータモデル(XDM)データタイプの概要を説明します。
exl-id: 01b7d41c-1ab6-4cbc-b9b3-77b6af69faf3
source-git-commit: b5cdd72238f7b4519de1c789f4294b9698415327
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 6%

---

# [!UICONTROL B2Bソー] スデータタイプ（ベータ版）

>[!IMPORTANT]
>
>このデータ型は、現在ベータ版であるリアルタイム顧客データプラットフォームB2Bエディションの一部として使用できます。 ドキュメントと機能は変更される場合があります。

[!UICONTROL B2Bソー] スは、B2Bエンティティの複合識別子(アカウン [ト](../classes/b2b/business-account.md)、オポチュニティ [、キャンペーン](../classes/b2b/business-opportunity.md)など)を表す標準のエクスペリエンスデータモデル(XDM)データ型 [](../classes/b2b/business-campaign.md)です。

文字列ベースの識別子のみに依存する場合、複数のシステム間でIDが重複する可能性があります（例えば、1つのCRMシステムで文字列IDを割り当てる場合は、同じIDがまったく異なるオポチュニティを参照する可能性があります）。 これにより、[リアルタイム顧客プロファイル](../../profile/home.md)にデータを結合する際にデータが競合する可能性があります。

[!UICONTROL B2B Source]データ型を使用すると、エンティティの元の文字列IDを使用し、ソース固有のコンテキスト情報と組み合わせて、元のソースに関係なく、Platformシステム内で完全に一意の状態を維持できます。

![B2Bソース構造](../images/data-types/b2b-source.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `sourceID` | 文字列 | ソースレコードの一意のID。 |
| `sourceInstanceID` | 文字列 | ソースデータのインスタンスIDまたは組織ID。 |
| `sourceKey` | 文字列 | `sourceId`、`sourceInstanceId`および`sourceType`を組み合わせて構成される一意の識別子を、次の形式で連結します。`[sourceID]@$[sourceInstanceID].[sourceType]`と入力します。<br><br>Marketoなどの一部のソースコネクタは、特定の識別子に対してこの値を自動的に連結します。その他は、[データ準備`concat`関数](../../data-prep/functions.md#string)を使用して手動で連結する必要があります。次に例を示します。`concat(id,"@${ORG_ID}.Marketo")` |
| `sourceType` | 文字列 | ソースデータを提供するプラットフォームの名前。 |

{style=&quot;table-layout:auto&quot;}

データタイプについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/datatypes/b2b/b2b-source.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/b2b/b2b-source.schema.json)
