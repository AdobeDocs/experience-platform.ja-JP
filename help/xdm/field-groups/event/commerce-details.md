---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: Commerce詳細スキーマフィールドグループ
description: Commerceの詳細スキーマフィールドグループについて説明します。
exl-id: 36aba186-fadb-4abb-a94f-7e151ff3f744
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '158'
ht-degree: 15%

---

# [!UICONTROL Commerceの詳細 &#x200B;] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL Commerceの詳細 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス &#x200B;](../../classes/experienceevent.md) の標準スキーマフィールドグループで、商品情報（SKU、名前、数量）や標準の買い物かご操作（注文、チェックアウト、放棄）などのコマースデータを表すために使用されます。

![](../../images/field-groups/commerce-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `commerce` | [Commerce](../../data-types/commerce.md) | 商品の返品、保証登録、買い物かご/注文プロセスを説明するオブジェクトです。 |
| `productListItems` | [&#x200B; 製品リスト項目 &#x200B;](../../data-types/product-list-item.md) の配列 | 顧客が選択した製品を表す項目のリスト。特定の時点での特定のオプションと価格設定（製品レコードとは異なる場合があります）。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-commerce.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-commerce.schema.json)
