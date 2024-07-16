---
title: 見積依頼詳細スキーマ フィールド グループ
description: 見積依頼詳細スキーマフィールドグループについて説明します。
exl-id: 19be76fa-d212-4b00-815a-d3869c1054e2
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 6%

---

# [!UICONTROL  見積依頼詳細 ] スキーマフィールドグループ

[!UICONTROL  見積依頼の詳細 ] は、[[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループです。 フィールドグループは、スキーマに単一の `quotes` オブジェクトを提供します。これは、保険契約、医療、製造オーダー、ハイテクなど、様々なタイプの見積もりに対するリクエストプロセスの詳細を収集します。

![](../../images/field-groups/quote-request-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `discount` | [[!UICONTROL 通貨]](../../data-types/currency.md) | 訪問者に表示される見積もりの割引額。 |
| `premium` | [[!UICONTROL 通貨]](../../data-types/currency.md) | 訪問者に表示される見積もりのプレミアム金額。 |
| `location` | [!UICONTROL 文字列] | 訪問者の場所の近くの販売店を検索するために使用される郵便番号。 |
| `requestID` | [!UICONTROL 文字列] | 見積もり依頼の一意の ID。 |
| `selectedRetailer` | [!UICONTROL 文字列] | 見積もり依頼で選択した販売店（該当する場合）。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[ 公開 XDM リポジトリ ](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/experience-event/experienceevent-quote-request-details.schema.json) を参照してください。
