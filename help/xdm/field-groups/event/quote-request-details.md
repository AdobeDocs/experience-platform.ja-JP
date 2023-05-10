---
title: 見積依頼詳細スキーマフィールドグループ
description: このドキュメントでは、「Quote Request Details」スキーマ・フィールド・グループの概要を説明します。
exl-id: 19be76fa-d212-4b00-815a-d3869c1054e2
source-git-commit: f5df893260f0772ad54ccdb00d99ed8f328d35a9
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 6%

---

# [!UICONTROL 見積依頼の詳細] スキーマフィールドグループ

[!UICONTROL 見積依頼の詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md). フィールドグループには、 `quotes` スキーマに対するオブジェクト。保険契約、医療、製造オーダー、ハイテク注文など、様々なタイプの見積もりの要求プロセスの詳細をキャプチャします。

![](../../images/field-groups/quote-request-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `discount` | [[!UICONTROL 通貨]](../../data-types/currency.md) | 訪問者に表示される見積書の割引額。 |
| `premium` | [[!UICONTROL 通貨]](../../data-types/currency.md) | 訪問者に表示される見積もりのプレミアム金額。 |
| `location` | [!UICONTROL 文字列] | 訪問者の場所の近くで小売業者を見つけるのに使用される郵便番号。 |
| `requestID` | [!UICONTROL 文字列] | 見積もり依頼の一意の ID。 |
| `selectedRetailer` | [!UICONTROL 文字列] | 見積もり依頼に対して選択した小売業者（該当する場合）。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/experience-event/experienceevent-quote-request-details.schema.json).
