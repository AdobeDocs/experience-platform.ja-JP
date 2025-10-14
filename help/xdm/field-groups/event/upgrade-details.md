---
title: アップグレードの詳細スキーマフィールドグループ
description: アップグレードの詳細スキーマフィールドグループについて説明します。
exl-id: cd3f4cd9-ee0e-4bdf-a630-dd2c3c3cc8c7
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 3%

---

# [!UICONTROL &#x200B; アップグレードの詳細 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; アップグレードの詳細 &#x200B;] は、アップグレードマーケティングイベントに関する情報を取得するために使用される [[!DNL XDM ExperienceEvent]  クラス &#x200B;](../../classes/experienceevent.md) の標準スキーマフィールドグループです。トランザクションの詳細と、オファーが顧客に表示される様々な方法を含みます。

フィールドグループは、単一のオブジェクトタイプのフィールド `upgrades` を提供します。 このオブジェクトに含まれるプロパティについては、以下で説明します。

![&#x200B; アップグレードの詳細構造 &#x200B;](../../images/field-groups/upgrade-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `upgradeImpressions` | [&#x200B; インプレッション &#x200B;](../../data-types/impressions.md) の配列 | 顧客の記録されたインプレッション（アップグレードオファーを含むデジタルビューまたはエンゲージメント）をリストする配列です。 |
| `upgradeTransaction` | [&#x200B; トランザクション &#x200B;](../../data-types/transaction.md) | アップグレードの通貨トランザクションを表します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upgrade-details.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upgrade-details.schema.json)
