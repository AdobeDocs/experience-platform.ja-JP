---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: Web 詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、Web 詳細スキーマフィールドグループの概要を説明します。
exl-id: eb42606b-ade4-4d72-b601-c560009c98e8
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 4%

---

# [!UICONTROL Web 詳細スキ] ーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[ フィールドグループ名の更新 ](../name-updates.md) のドキュメントを参照してください。

[!UICONTROL Web 詳] 細は、クラス [[!DNL XDM ExperienceEvent] ](../../classes/experienceevent.md)の標準スキーマフィールドグループで、インタラクション、ページの詳細、リファラーなどの Web 詳細イベントに関する情報を説明するのに使用されます。

![](../../images/field-groups/web-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `web` | [Web 情報](../../data-types/web-information.md) | リンクのクリック数、Web ページの詳細、リファラー情報およびブラウザーの詳細について説明します。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-web.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-web.schema.json)
