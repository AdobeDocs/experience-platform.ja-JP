---
title: 入金詳細スキーマフィールドグループ
description: 「入金詳細」スキーマフィールドグループについて説明します。
exl-id: a40d17b3-cb76-4b63-9328-735fc7c55672
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 4%

---

# [!UICONTROL  入金詳細 ] スキーマフィールドグループ

[!UICONTROL  入金詳細 ] は、[[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループです。 フィールドグループは、財務預金に関する詳細を取得するスキーマに単一の `personalFinances.deposits` フィールドを提供します。

![](../../images/field-groups/deposit-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `account` | [[!UICONTROL  金融口座 ]](../../data-types/financial-account.md) | 預金に関連付けられている金融口座を表します。 |
| `transaction` | [[!UICONTROL  トランザクション ]](../../data-types/transaction.md) | 預金に関連付けられている財務トランザクションを表します。 |
| `mobileDeposit` | [!UICONTROL  ブール値 ] | 預金がモバイルプラットフォームを通じて行われたかどうかを示します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[ 公開 XDM リポジトリ ](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/experience-event/industry-verticals/experienceevent-deposit-details.schema.json) を参照してください。
