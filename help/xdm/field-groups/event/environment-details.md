---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM;ExperienceEvent；フィールド；スキーマ;スキーマ;スキーマデザイン；フィールドグループ；フィールドグループ；環境;環境の詳細；
solution: Experience Platform
title: 環境の詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「ExperienceEvent環境の詳細スキーマ」フィールドグループの概要を説明します。
exl-id: 1d25b98f-66ac-443f-9b1c-dfd20a168c59
translation-type: tm+mt
source-git-commit: d425dcd9caf8fccd0cb35e1bac73950a6042a0f8
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 2%

---


# [!UICONTROL 環境] 詳細スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL 環境]  [[!DNL XDM ExperienceEvent] ](../../classes/individual-profile.md) の詳細は、スキーマの詳細、ブラウザーの情報、ローカル時間、その他の地理的イベントなど、エクスペリエンスに関連する環境の詳細に関する情報を取り込むために使用される、クラスの標準フィールドグループです。

<img src="../../images/field-groups/environment-details.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `device` | [デバイス](../../data-types/device.md) | セッション間で追跡可能な、識別されたデバイス、アプリケーション、またはデバイスのブラウザーインスタンスを表します。通常はcookieによって追跡されます。 |
| `environment` | [環境](../../data-types/environment.md) | イベント監視の状況コンテキストに関する情報、特にネットワークやソフトウェアのバージョンなどの一時的な情報の詳細を説明します。 |
| `placeContext` | [配置コンテキスト](../../data-types/place-context.md) | イベント観測に関連する過渡的な状況を説明します。 例としては、天気、ローカル時間、トラフィック、曜日、稼働日と休日、勤務時間など、ロケール固有の情報があります。 |

フィールドグループの詳細については、public XDM repositoryを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/experience-event/experienceevent-environment-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/experience-event/experienceevent-environment-details.schema.json)
