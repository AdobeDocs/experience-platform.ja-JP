---
title: B2B Source データタイプ
description: B2B Source Experience Data Model （XDM）データタイプについて説明します。
exl-id: 01b7d41c-1ab6-4cbc-b9b3-77b6af69faf3
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '236'
ht-degree: 3%

---

# [!UICONTROL B2B Source] データタイプ

[!UICONTROL B2B Source] は、B2B エンティティ（[account](../classes/b2b/business-account.md)、[opportunity](../classes/b2b/business-opportunity.md)、[campaign](../classes/b2b/business-campaign.md) など）の複合識別子を表す標準のエクスペリエンスデータモデル（XDM）データタイプです。

文字列ベースの識別子のみに依存する場合、複数のシステム間で ID 間が重複する可能性があります。例えば、あるオポチュニティで 1 つの CRM システムに文字列 ID を指定しても、その ID が、まったく別のオポチュニティを参照する場合があります。 これにより、[ リアルタイム顧客プロファイル ](../../profile/home.md) でデータを結合する際に、データの競合が発生する可能性があります。

[!UICONTROL B2B Source] データタイプを使用すると、エンティティの元の文字列 ID を使用し、それをソース固有のコンテキスト情報と組み合わせて、元のソースに関係なく、Platform システム内で完全に一意なままにすることができます。

![B2B Sourceの構造 ](../images/data-types/b2b-source.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `sourceID` | 文字列 | ソースレコードの一意の ID。 |
| `sourceInstanceID` | 文字列 | ソースデータのインスタンスまたは組織 ID。 |
| `sourceKey` | 文字列 | `sourceId`、`sourceInstanceId`、`sourceType` を次の形式で連結した一意の ID:`[sourceID]@[sourceInstanceID].[sourceType]`。<br><br>Marketoなどの一部のソースコネクタでは、この値が特定の識別子に対して自動的に連結されます。 その他は、[Data Prep `concat` 関数 ](../../data-prep/functions.md#string) を使用して手動で連結する必要があります。例：`concat(id,"@${ORG_ID}.Marketo")` |
| `sourceType` | 文字列 | ソースデータを提供するプラットフォームの名前。 |

{style="table-layout:auto"}

データタイプについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/datatypes/b2b/b2b-source.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/datatypes/b2b/b2b-source.schema.json)
