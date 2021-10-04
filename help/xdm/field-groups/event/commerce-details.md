---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: コマース詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「コマースの詳細」スキーマフィールドグループの概要を説明します。
exl-id: 36aba186-fadb-4abb-a94f-7e151ff3f744
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 4%

---

# [!UICONTROL コマース詳細ス] キーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[ フィールドグループ名の更新 ](../name-updates.md) のドキュメントを参照してください。

[!UICONTROL コマー] ス [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md)クラスの標準スキーマフィールドグループを説明します。製品情報（SKU、名前、数量）、標準買い物かご操作（注文、チェックアウト、放棄）などのコマースデータを記述します。

![](../../images/field-groups/commerce-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `commerce` | [Commerce](../../data-types/commerce.md) | 製品返品、保証登録、買い物かご/注文プロセスを説明するオブジェクト。 |
| `productListItems` | [ 製品リスト項目 ](../../data-types/product-list-item.md) の配列 | 顧客が選択した製品を表す項目のリスト。特定の時点での特定のオプションと価格（製品レコードとは異なる場合があります）が表示されます。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-commerce.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-commerce.schema.json)
