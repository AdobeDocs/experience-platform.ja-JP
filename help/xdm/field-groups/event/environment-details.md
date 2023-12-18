---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；環境；環境の詳細；
solution: Experience Platform
title: 環境詳細スキーマフィールドグループ
description: 「 ExperienceEvent 環境の詳細」スキーマフィールドグループについて説明します。
exl-id: 1d25b98f-66ac-443f-9b1c-dfd20a168c59
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 13%

---


# [!UICONTROL 環境の詳細] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL 環境の詳細] は、 [[!DNL XDM ExperienceEvent] クラス](../../classes/experienceevent.md) デバイスの詳細、ブラウザー情報、ローカル時間、その他の地理的情報など、エクスペリエンスイベントに関する環境の詳細に関する情報をキャプチャするために使用します。

<img src="../../images/field-groups/environment-details.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `device` | [デバイス](../../data-types/device.md) | セッションをまたいで（通常は cookie で）追跡可能な、識別されたデバイス、アプリケーションまたはデバイスブラウザーインスタンスを表します。 |
| `environment` | [環境](../../data-types/environment.md) | イベント観測の状況に関する情報、特にネットワークやソフトウェアのバージョンなどの一時的な情報の詳細を説明します。 |
| `placeContext` | [場所のコンテキスト](../../data-types/place-context.md) | イベント観測に関連する一時的な状況を示します。 例としては、天気、現地時間、交通量、曜日、就業日と休日、就業時間など、ロケール固有の情報が含まれます。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-environment-details.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-environment-details.schema.json)
