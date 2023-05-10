---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: チャネル詳細スキーマフィールドグループ
description: このドキュメントでは、「チャネル詳細」スキーマフィールドグループの概要を説明します。
exl-id: b8ec2f57-6882-466e-9b22-61fb2178fb1e
source-git-commit: 60c0bd62b4effaa161c61ab304718ab8c20a06e1
workflow-type: tm+mt
source-wordcount: '143'
ht-degree: 18%

---

# [!UICONTROL チャネルの詳細] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL チャネルの詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md)：チャネル情報（ID、チャネルタイプ、メディアタイプ、場所タイプなど）の説明に使用されます。

![](../../images/field-groups/channel-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `channel` | [エクスペリエンスチャネル](../../data-types/experience-channel.md) | 製品の返品、保証登録、買い物かご/注文プロセスを表すオブジェクト。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-channel.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-channel.schema.json)
