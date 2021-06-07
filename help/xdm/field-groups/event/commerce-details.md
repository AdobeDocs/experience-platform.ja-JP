---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: コマース詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「コマース詳細」スキーマフィールドグループの概要を説明します。
source-git-commit: b22dce52563d5f3bbd1796c11d7c7b2a49fa6d5f
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 4%

---


# [!UICONTROL コマース詳細] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL コマー] スクラスの標準スキーマフィールドグループ [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md)。商品情報（SKU、名前、数量）や買い物かごの標準操作（注文、チェックアウト、放棄）などのコマースデータを説明するのに使用されます。

![](../../images/field-groups/commerce-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `commerce` | [コマース](../../data-types/commerce.md) | 製品返品、保証登録、買い物かご/注文プロセスを説明するオブジェクト。 |
| `productListItems` | [製品リスト項目](../../data-types/product-list-item.md)の配列 | 顧客が選択した製品を表す項目のリスト。特定の時点での特定のオプションと価格（製品レコードと異なる場合があります）が表示されます。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/experience-event/experienceevent-commerce.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/experience-event/experienceevent-commerce.schema.json)
