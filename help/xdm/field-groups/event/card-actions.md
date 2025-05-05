---
title: カードアクションスキーマフィールドグループ
description: カードアクションのスキーマフィールドグループについて説明します。
exl-id: 49851544-9118-4b73-b1d1-4cf49b3f1dee
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 24%

---

# [!UICONTROL &#x200B; カードアクション &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; カードアクション &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループです。 フィールドグループは、スキーマに単一の `personalFinances.cardActions` フィールドを提供します。このスキーマは、カードタイプ、アクティベーションステータス、ロックステータスなど、カードアクションに関する詳細を収集します。

![](../../images/field-groups/card-actions.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `cardActivated` | 整数 | カードが正常にアクティブ化された際にトラッキングします。 |
| `cardActivationStart` | 整数 | カードの有効化プロセスが開始された際にトラッキングします。 |
| `cardCancelled` | 整数 | カードがキャンセルされた際にトラッキングします。 |
| `cardControlsLocked` | 整数 | カードのコントロールがロックされた際にトラッキングします。 |
| `cardControlsUnlocked` | 整数 | カードのコントロールがロック解除された際にトラッキングします。 |
| `cardID` | 文字列 | 有効化するカードの識別子。 この値は、カード番号とは異なる可能性があります。 |
| `cardLocked` | 整数 | カードがロックされた際にトラッキングします。 |
| `cardOrderNew` | 整数 | カードがリクエストされた際にトラッキングします。 |
| `cardOrderType` | 文字列 | カード注文イベントに関連付けられたカード注文のタイプ。 |
| `cardType` | 文字列 | カードのタイプ。 |
| `cardUnlocked` | 整数 | カードがロック解除された際にトラッキングします。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[ 公開 XDM リポジトリ ](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/experience-event/experienceevent-card-actions.schema.json) を参照してください。
