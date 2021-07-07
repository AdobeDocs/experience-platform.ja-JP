---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: コマース詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「コマース詳細」スキーマフィールドグループの概要を説明します。
source-git-commit: afe748d443aad7b6da5b348cd569c9e806e4419b
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 4%

---


# [!UICONTROL Commerce Details] schema field group

>[!NOTE]
>
>The names of several schema field groups have changed. 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL Commerce Details] is a standard schema field group for the [[!DNL XDM ExperienceEvent] class](../../classes/experienceevent.md), used to describe commerce data such as product information (SKU, name, quantity), and standard cart operations (order, checkout, abandon).

![](../../images/field-groups/commerce-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `commerce` | [コマース](../../data-types/commerce.md) | An object that describes product returns, warranty registration, and shopping cart/order processes. |
| `productListItems` | Array of [Product list items](../../data-types/product-list-item.md) | 顧客が選択した製品を表す項目のリスト。特定の時点での特定のオプションと価格（製品レコードと異なる場合があります）が表示されます。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [Populated example](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-commerce.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-commerce.schema.json)
