---
title: 残高移動スキーマフィールドグループ
description: 残高移動スキーマフィールドグループについて説明します。
exl-id: be0d2ed6-6547-432a-af2f-409c33e268d4
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 3%

---

# [!UICONTROL &#x200B; 残高移動 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; 残高移動 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス &#x200B;](../../classes/experienceevent.md) の標準スキーマフィールドグループです。 フィールドグループは、スキーマに単一の `personalFinances.balanceTransfers` オブジェクトを提供します。このスキーマは、口座間の金融残高移動に関する詳細をキャプチャします。

![](../../images/field-groups/balance-transfers.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `accountFrom` | [[!UICONTROL &#x200B; 金融口座 &#x200B;]](../../data-types/financial-account.md) | 残高の振替元となる金融口座を表します。 |
| `accountTo` | [[!UICONTROL &#x200B; 金融口座 &#x200B;]](../../data-types/financial-account.md) | 残高の移動先の金融口座を表します。 |
| `transaction` | [[!UICONTROL &#x200B; トランザクション &#x200B;]](../../data-types/transaction.md) | 残高移動に関連する財務トランザクションを表します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[&#x200B; 公開 XDM リポジトリ &#x200B;](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/experience-event/industry-verticals/experienceevent-balance-transfers.schema.json) を参照してください。
