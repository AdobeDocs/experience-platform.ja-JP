---
title: カードアクションスキーマフィールドグループ
description: このドキュメントでは、「カードアクション」スキーマフィールドグループの概要を説明します。
exl-id: 49851544-9118-4b73-b1d1-4cf49b3f1dee
source-git-commit: f5df893260f0772ad54ccdb00d99ed8f328d35a9
workflow-type: tm+mt
source-wordcount: '193'
ht-degree: 9%

---

# [!UICONTROL カードアクション] スキーマフィールドグループ

[!UICONTROL カードアクション] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md). フィールドグループには、 `personalFinances.cardActions` スキーマへのフィールド。カードのタイプ、アクティベーションステータス、ロックステータスなど、カードの操作に関する詳細をキャプチャします。

![](../../images/field-groups/card-actions.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `cardActivated` | 整数 | カードが正常にアクティブ化された際にトラッキングします。 |
| `cardActivationStart` | 整数 | カードのアクティベーションプロセスが開始された際にトラッキングします。 |
| `cardCancelled` | 整数 | カードがキャンセルされた際にトラッキングします。 |
| `cardControlsLocked` | 整数 | カードのコントロールがロックされた際にトラッキングします。 |
| `cardControlsUnlocked` | 整数 | カードのコントロールがロック解除された際にトラッキングします。 |
| `cardID` | 文字列 | アクティブ化するカードの識別子。 この値は、カード番号とは異なる可能性があります。 |
| `cardLocked` | 整数 | カードがロックされた際にトラッキングします。 |
| `cardOrderNew` | 整数 | カードがリクエストされた際にトラッキングします。 |
| `cardOrderType` | 文字列 | カード注文イベントに関連付けられているカード注文のタイプ。 |
| `cardType` | 文字列 | カードのタイプ。 |
| `cardUnlocked` | 整数 | カードがロック解除された際にトラッキングします。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/experience-event/experienceevent-card-actions.schema.json).
