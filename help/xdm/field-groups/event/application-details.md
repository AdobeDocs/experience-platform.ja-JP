---
title: アプリケーション詳細スキーマフィールドグループ
description: アプリケーション詳細スキーマフィールドグループについて説明します。
exl-id: 5df99f9a-b36a-4c2b-a4a4-d3cf054f09b8
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '123'
ht-degree: 3%

---

# [!UICONTROL &#x200B; アプリケーションの詳細 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; アプリケーションの詳細 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラス &#x200B;](../../classes/experienceevent.md) の標準スキーマフィールドグループです。 フィールドグループは、スキーマに単一の `application` オブジェクトを提供します。このオブジェクトは、クラッシュ回数、機能の使用状況、起動回数、アップグレード回数など、アプリケーション関連の詳細を取得します。

![](../../images/field-groups/application-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `application` | [[!UICONTROL &#x200B; 適用 &#x200B;]](../../data-types/financial-account.md) | アプリケーション名、アプリのバージョン、インストール、起動、クラッシュ、終了など、イベントに関連するアプリケーション情報をキャプチャします。 イベントの対象となるアプリケーション（送信中のプッシュ通知の宛先など）か、イベントの発生元となるアプリケーション（クリックやログインなど）のいずれかです。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[&#x200B; 公開 XDM リポジトリ &#x200B;](https://github.com/adobe/xdm/blob/master/docs/reference/fieldgroups/experience-event/experienceevent-application.schema.json) を参照してください。
