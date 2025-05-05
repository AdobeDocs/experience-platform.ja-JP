---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM;ExperienceEvent；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；環境；環境の詳細；
solution: Experience Platform
title: 環境詳細スキーマフィールドグループ
description: ExperienceEvent 環境詳細スキーマフィールドグループについて説明します。
exl-id: 1d25b98f-66ac-443f-9b1c-dfd20a168c59
source-git-commit: 1d1224b263b55b290d2cac9c07dfd1b852c4cef5
workflow-type: tm+mt
source-wordcount: '191'
ht-degree: 13%

---


# [!UICONTROL &#x200B; 環境の詳細 &#x200B;] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL &#x200B; 環境の詳細 &#x200B;] は、[[!DNL XDM ExperienceEvent]  クラスの標準スキーマフィールドグループで ](../../classes/experienceevent.md) デバイスの詳細、ブラウザー情報、ローカル時間、その他の地理的情報など、エクスペリエンスイベントに関連する環境の詳細に関する情報をキャプチャするために使用されます。

![](../../images/field-groups/environment-details.png){width=500}

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `device` | [ デバイス ](../../data-types/device.md) | （通常は cookie で） セッションをまたいでトラッキング可能な、識別されたデバイス、アプリケーションまたはデバイスブラウザーインスタンスを表します。 |
| `environment` | [環境](../../data-types/environment.md) | イベント観測の状況コンテキストに関する情報を表します。特に、ネットワークやソフトウェアバージョンなどの一時的な情報を説明します。 |
| `placeContext` | [ 場所の背景 ](../../data-types/place-context.md) | イベントの観測に関連する一時的な状況を表します。 例えば、ロケール固有の情報（天気、現地時間、交通量、曜日、勤務日と休日、勤務時間など）が含まれます。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-environment-details.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-environment-details.schema.json)
