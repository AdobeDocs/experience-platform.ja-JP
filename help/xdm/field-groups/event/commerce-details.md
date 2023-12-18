---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: コマース詳細スキーマフィールドグループ
description: コマース詳細スキーマフィールドグループについて説明します。
exl-id: 36aba186-fadb-4abb-a94f-7e151ff3f744
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 15%

---

# [!UICONTROL コマースの詳細] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL コマースの詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md)：製品情報（SKU、名前、数量）や標準的な買い物かご操作（注文、チェックアウト、放棄）などのコマースデータの説明に使用します。

![](../../images/field-groups/commerce-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `commerce` | [コマース](../../data-types/commerce.md) | 製品の返品、保証登録、買い物かご/注文プロセスを表すオブジェクト。 |
| `productListItems` | の配列 [製品リスト項目](../../data-types/product-list-item.md) | 顧客が選択した製品を表す項目のリストで、特定の時点での特定のオプションと価格（製品レコードと異なる場合があります）を含みます。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-commerce.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-commerce.schema.json)
