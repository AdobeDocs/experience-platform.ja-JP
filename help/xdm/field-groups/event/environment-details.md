---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；環境；環境の詳細；
solution: Experience Platform
title: 環境詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「 ExperienceEvent環境の詳細」スキーマフィールドグループの概要を説明します。
exl-id: 1d25b98f-66ac-443f-9b1c-dfd20a168c59
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 4%

---


# [!UICONTROL 環境詳細ス] キーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL 環境の詳]  [[!DNL XDM ExperienceEvent] ](../../classes/individual-profile.md) 細：デバイスの詳細、ブラウザー情報、ローカル時間、その他の地理情報など、エクスペリエンスイベントに関連する環境の詳細に関する情報を取得するために使用される、クラスの標準スキーマフィールドグループです。

<img src="../../images/field-groups/environment-details.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `device` | [デバイス](../../data-types/device.md) | セッション間（通常はcookieで）で追跡可能な、識別されたデバイス、アプリケーション、またはデバイスのブラウザーインスタンスを表します。 |
| `environment` | [環境](../../data-types/environment.md) | イベント監視の状況コンテキストに関する情報を説明し、特にネットワークやソフトウェアのバージョンなどの一時的な情報を詳しく説明します。 |
| `placeContext` | [場所コンテキスト](../../data-types/place-context.md) | イベント監視に関連する一時的な状況を示します。 例としては、天気、現地時間、トラフィック、曜日、稼働日と休日、勤務時間など、ロケール固有の情報が含まれます。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/experience-event/experienceevent-environment-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/experience-event/experienceevent-environment-details.schema.json)
