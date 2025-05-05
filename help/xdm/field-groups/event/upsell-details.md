---
title: アップセルの詳細スキーマフィールドグループ
description: アップセルの詳細スキーマフィールドグループについて説明します。
exl-id: 6b69805d-03bc-489b-945a-03e61b99842e
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 3%

---

# [!UICONTROL &#x200B; アップセルの詳細 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; アップセルの詳細 &#x200B;] は、アップセルマーケティングイベントに関する情報を取得するために使用される [[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループです。トランザクションの詳細と、オファーが顧客に表示される様々な方法を含みます。

フィールドグループは、単一のオブジェクトタイプのフィールド `upsells` を提供します。 このオブジェクトに含まれるプロパティについては、以下で説明します。

![ アップセルの詳細構造 ](../../images/field-groups/upsell-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `upsellImpressions` | [ インプレッション ](../../data-types/impressions.md) の配列 | 顧客の記録されたインプレッション（アップセルオファーのデジタルビューまたはエンゲージメント）をリストする配列です。 |
| `upsellTransaction` | [ トランザクション ](../../data-types/transaction.md) | アップセルの通貨トランザクションを表します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upsell-details.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upsell-details.schema.json)
