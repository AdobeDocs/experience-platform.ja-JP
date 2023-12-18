---
title: アプリ詳細スキーマフィールドグループ
description: 「アプリケーション詳細スキーマ」フィールドグループについて説明します。
exl-id: 5df99f9a-b36a-4c2b-a4a4-d3cf054f09b8
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '123'
ht-degree: 4%

---

# [!UICONTROL アプリの詳細] スキーマフィールドグループ

[!UICONTROL アプリの詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md). フィールドグループには、 `application` スキーマに対するオブジェクト。クラッシュ回数、機能の使用状況、起動回数、アップグレード回数など、アプリケーション関連の詳細を取り込みます。

![](../../images/field-groups/application-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `application` | [[!UICONTROL アプリケーション]](../../data-types/financial-account.md) | イベントに関連するアプリケーション情報をキャプチャします（アプリケーション名、アプリのバージョン、インストール、起動、クラッシュ、クロージャなど）。 イベントのターゲットとなるアプリケーション（プッシュ通知の送信先など）またはイベントを発信するアプリケーション（クリック、ログインなど）のどちらかです。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/experience-event/experienceevent-application.schema.json).
