---
title: アップグレードの詳細スキーマフィールドグループ
description: アップグレードの詳細スキーマフィールドグループについて説明します。
exl-id: cd3f4cd9-ee0e-4bdf-a630-dd2c3c3cc8c7
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '126'
ht-degree: 3%

---

# [!UICONTROL  アップグレードの詳細 ] スキーマフィールドグループ

[!UICONTROL  アップグレードの詳細 ] は、アップグレードマーケティングイベントに関する情報を取得するために使用される [[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループです。トランザクションの詳細と、オファーが顧客に表示される様々な方法を含みます。

フィールドグループは、単一のオブジェクトタイプのフィールド `upgrades` を提供します。 このオブジェクトに含まれるプロパティについては、以下で説明します。

![ アップグレードの詳細構造 ](../../images/field-groups/upgrade-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `upgradeImpressions` | [ インプレッション ](../../data-types/impressions.md) の配列 | 顧客の記録されたインプレッション（アップグレードオファーを含むデジタルビューまたはエンゲージメント）をリストする配列です。 |
| `upgradeTransaction` | [ トランザクション ](../../data-types/transaction.md) | アップグレードの通貨トランザクションを表します。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upgrade-details.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/industry-verticals/experienceevent-upgrade-details.schema.json)
