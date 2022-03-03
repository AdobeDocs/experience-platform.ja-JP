---
title: Card Actions Schema Field Group
description: This document provides an overview of the Card Actions schema field group.
source-git-commit: eaea904ddda6b7ffee6f52cd4af897c2a8885714
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 9%

---

# 

[[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md)`personalFinances.cardActions`

![](../../images/field-groups/card-actions.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `cardActivated` | 整数 | Tracks when the card has been successfully activated. |
| `cardActivationStart` | 整数 | Tracks when the card activation process has been started. |
| `cardCancelled` | 整数 | Tracks when a card has been cancelled. |
| `cardControlsLocked` | 整数 | Tracks when a card&#39;s controls have been locked. |
| `cardControlsUnlocked` | 整数 | Tracks when a card&#39;s controls have been unlocked. |
| `cardID` | 文字列 | The identifier for the card being activated. This value might be different from the card number. |
| `cardLocked` | 整数 | Tracks when a card has been locked. |
| `cardOrderNew` | 整数 | Tracks when a card has been requested. |
| `cardOrderType` | 文字列 | The type of card order associated with a card order event. |
| `cardType` | 文字列 | The type of card. |
| `cardUnlocked` | 整数 | Tracks when a card has been unlocked. |

{style=&quot;table-layout:auto&quot;}

[](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/experience-event/experienceevent-card-actions.schema.json)
