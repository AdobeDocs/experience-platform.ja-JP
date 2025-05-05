---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: Campaign マーケティング詳細スキーマフィールドグループ
description: Campaign マーケティング詳細スキーマフィールドグループについて説明します。
exl-id: be08b38b-68a0-4a74-9b8f-0344a0637395
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 19%

---

# [!UICONTROL &#x200B; キャンペーンマーケティング詳細 &#x200B;] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL &#x200B; キャンペーンマーケティング詳細 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループで、キャンペーングループ、名前、トラッキングコードなどのマーケティングキャンペーン情報を記述するために使用されます。

![](../../images/field-groups/campaign-marketing-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `marketing` | [ マーケティング ](../../data-types/marketing.md) | キャンペーングループ、名前、トラッキングコードなど、マーケティングキャンペーン情報を説明するオブジェクト。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-marketing.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-marketing.schema.json)
