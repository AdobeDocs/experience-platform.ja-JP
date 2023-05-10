---
title: B2B ソースデータタイプ
description: このドキュメントでは、B2B ソースエクスペリエンスデータモデル (XDM) データタイプの概要を説明します。
exl-id: 01b7d41c-1ab6-4cbc-b9b3-77b6af69faf3
source-git-commit: e602f78470fe4eeb2a42e6333ba52096d8a9fe8a
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 2%

---

# [!UICONTROL B2B ソース] データタイプ

[!UICONTROL B2B ソース] は、B2B エンティティ ( [アカウント](../classes/b2b/business-account.md)、 [商談](../classes/b2b/business-opportunity.md)、または [campaign](../classes/b2b/business-campaign.md)) をクリックします。

文字列ベースの識別子のみに依存する場合、複数のシステム間で ID が重複する場合があります（例えば、1 つの CRM システムで文字列 ID を割り当てる場合は、同じ ID でまったく異なるオポチュニティを参照する場合があります）。 その結果、 [リアルタイム顧客プロファイル](../../profile/home.md).

この [!UICONTROL B2B ソース] データ型を使用すると、エンティティの元の文字列 ID を使用し、それをソース固有のコンテキスト情報と組み合わせて、元のソースに関係なく、Platform システム内で完全に一意の状態を維持できます。

![B2B ソース構造](../images/data-types/b2b-source.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `sourceID` | 文字列 | ソースレコードの一意の ID。 |
| `sourceInstanceID` | 文字列 | ソースデータのインスタンスまたは組織 ID。 |
| `sourceKey` | 文字列 | で構成される一意の識別子 `sourceId`, `sourceInstanceId`、および `sourceType` を次の形式で連結します。 `[sourceID]@[sourceInstanceID].[sourceType]`.<br><br>Marketoなどの一部のソースコネクタは、特定の識別子に対してこの値を自動的に連結します。 その他は、 [データ準備 `concat` 関数](../../data-prep/functions.md#string)例： `concat(id,"@${ORG_ID}.Marketo")` |
| `sourceType` | 文字列 | ソースデータを提供するプラットフォームの名前。 |

{style="table-layout:auto"}

データタイプについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/datatypes/b2b/b2b-source.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/datatypes/b2b/b2b-source.schema.json)
