---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: チャネル詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「チャネル詳細」スキーマフィールドグループの概要を説明します。
exl-id: b8ec2f57-6882-466e-9b22-61fb2178fb1e
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '146'
ht-degree: 4%

---

# [!UICONTROL チャネル詳細] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[ フィールドグループ名の更新 ](../name-updates.md) のドキュメントを参照してください。

[!UICONTROL チャネル] ID、チャネルタイプ、メディアタイプ、場所タイプなどのチャネル情報の説明に使用される、クラスの標準スキーマフィールドグループ [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md)。

![](../../images/field-groups/channel-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `channel` | [エクスペリエンスチャネル](../../data-types/experience-channel.md) | 製品返品、保証登録、買い物かご/注文プロセスを説明するオブジェクト。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-channel.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-channel.schema.json)
