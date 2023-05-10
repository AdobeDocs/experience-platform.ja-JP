---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: キャンペーンマーケティング詳細スキーマフィールドグループ
description: このドキュメントでは、「キャンペーンマーケティング詳細」スキーマフィールドグループの概要を説明します。
exl-id: be08b38b-68a0-4a74-9b8f-0344a0637395
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 17%

---

# [!UICONTROL キャンペーンマーケティングの詳細] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL キャンペーンマーケティングの詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md)：キャンペーングループ、名前、トラッキングコードなど、マーケティングキャンペーン情報の説明に使用されます。

![](../../images/field-groups/campaign-marketing-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `marketing` | [マーケティング](../../data-types/marketing.md) | マーケティングキャンペーン情報（キャンペーングループ、名前、トラッキングコードなど）を表すオブジェクト。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-marketing.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-marketing.schema.json)
