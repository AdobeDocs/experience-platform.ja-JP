---
title: 残高転送スキーマフィールドグループ
description: 「残高移動」スキーマ・フィールド・グループの詳細を説明します。
exl-id: be0d2ed6-6547-432a-af2f-409c33e268d4
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 3%

---

# [!UICONTROL 残高移動] スキーマフィールドグループ

[!UICONTROL 残高移動] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md). フィールドグループには、 `personalFinances.balanceTransfers` スキーマに対するオブジェクト。このスキーマは、勘定間の財務残高移動に関する詳細をキャプチャします。

![](../../images/field-groups/balance-transfers.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountFrom` | [[!UICONTROL 金融口座]](../../data-types/financial-account.md) | 残高の振り替え元の金融口座を表します。 |
| `accountTo` | [[!UICONTROL 金融口座]](../../data-types/financial-account.md) | 残高の振り替え先の金融口座を表します。 |
| `transaction` | [[!UICONTROL トランザクション]](../../data-types/transaction.md) | 残高移動に関連する金融トランザクションを記述します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/experience-event/industry-verticals/experienceevent-balance-transfers.schema.json).
