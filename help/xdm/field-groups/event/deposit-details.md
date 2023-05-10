---
title: 預金詳細スキーマフィールドグループ
description: このドキュメントでは、「預金詳細」スキーマフィールドグループの概要を説明します。
exl-id: a40d17b3-cb76-4b63-9328-735fc7c55672
source-git-commit: f5df893260f0772ad54ccdb00d99ed8f328d35a9
workflow-type: tm+mt
source-wordcount: '109'
ht-degree: 7%

---

# [!UICONTROL 預金の詳細] スキーマフィールドグループ

[!UICONTROL 預金の詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md). フィールドグループには、 `personalFinances.deposits` フィールドをスキーマに追加します。

![](../../images/field-groups/deposit-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `account` | [[!UICONTROL 金融口座]](../../data-types/financial-account.md) | 預金に関連付けられた金融口座を表します。 |
| `transaction` | [[!UICONTROL トランザクション]](../../data-types/transaction.md) | 預金に関連する金融トランザクションを記述します。 |
| `mobileDeposit` | [!UICONTROL ブール値] | 預金がモバイルプラットフォームを通じて行われたかどうかを示します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/experience-event/industry-verticals/experienceevent-deposit-details.schema.json).
