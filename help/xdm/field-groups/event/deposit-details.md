---
title: Deposit Details Schema Field Group
description: This document provides an overview of the Deposit Details schema field group.
source-git-commit: 32d8798d426696d8fd4ace4c53a8bf9b4db26b61
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 8%

---

# 

[[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md)`personalFinances.deposits`

![](../../images/field-groups/deposit-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `account` | [](../../data-types/financial-account.md) | Describes the financial account associated with the deposit. |
| `transaction` | [[!UICONTROL トランザクション]](../../data-types/transaction.md) | Describes the financial transaction associated with the deposit. |
| `mobileDeposit` | [!UICONTROL ブール値] | Indicates whether the deposit was done through a mobile platform. |

{style=&quot;table-layout:auto&quot;}

[](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/experience-event/industry-verticals/experienceevent-deposit-details.schema.json)
