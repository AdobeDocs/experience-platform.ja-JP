---
title: 残高転送スキーマフィールドグループ
description: このドキュメントでは、「残高移動」スキーマ・フィールド・グループの概要を示します。
source-git-commit: 32d8798d426696d8fd4ace4c53a8bf9b4db26b61
workflow-type: tm+mt
source-wordcount: '119'
ht-degree: 6%

---

# [!UICONTROL 残高移動] スキーマフィールドグループ

[!UICONTROL 残高移動] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md). フィールドグループには、 `personalFinances.balanceTransfers` スキーマに対するオブジェクト。このスキーマは、勘定間の財務残高移動に関する詳細をキャプチャします。

![](../../images/field-groups/balance-transfers.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountFrom` | [[!UICONTROL 金融口座]](../../data-types/financial-account.md) | 残高の転送元の金融口座を表します。 |
| `accountTo` | [[!UICONTROL 金融口座]](../../data-types/financial-account.md) | 残高の転送先の金融口座を表します。 |
| `transaction` | [[!UICONTROL トランザクション]](../../data-types/transaction.md) | 残高移動に関連する金融トランザクションを記述します。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/experience-event/industry-verticals/experienceevent-balance-transfers.schema.json).
