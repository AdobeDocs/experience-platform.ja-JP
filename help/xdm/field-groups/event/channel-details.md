---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: チャネル詳細スキーマフィールドグループ
description: チャネル詳細スキーマフィールドグループについて説明します。
exl-id: b8ec2f57-6882-466e-9b22-61fb2178fb1e
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 20%

---

# [!UICONTROL &#x200B; チャネルの詳細 &#x200B;] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL &#x200B; チャネルの詳細 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス ](../../classes/experienceevent.md) の標準スキーマフィールドグループで、ID、チャネルタイプ、メディアタイプ、場所タイプなどのチャネル情報を記述するために使用されます。

![](../../images/field-groups/channel-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `channel` | [Experience チャネル ](../../data-types/experience-channel.md) | 商品の返品、保証登録、買い物かご/注文プロセスを説明するオブジェクトです。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-channel.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-channel.schema.json)
