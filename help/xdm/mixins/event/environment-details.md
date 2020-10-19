---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;ExperienceEvent;fields;schemas;Schemas;Schema design;mixin;mixin;environment;environment details;
solution: Experience Platform
title: ExperienceEvent環境の詳細ミックスイン
topic: overview
description: このドキュメントでは、ExperienceEvent環境の詳細ミックスインの概要を説明します。
translation-type: tm+mt
source-git-commit: f5bddb39c16eb25e85297f56e331d3aa51510eb9
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 3%

---


# [!UICONTROL ExperienceEvent環境の詳細] Mixin

[!UICONTROL ExperienceEvent環境の詳細] は、デバイスの詳細、ブラウザーの情報、ローカル時間、その他の地理的な情報など、エクスペリエンスイベントに関連する環境の詳細に関する情報を取得するために使用される、 [[!DNL XDM ExperienceEvent] ](../../classes/individual-profile.md) クラスの標準ミックスインです。

<img src="../../images/mixins/environment-details.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `device` | [デバイス](../../data-types/device.md) | セッション間で追跡可能な、識別されたデバイス、アプリケーション、またはデバイスのブラウザーインスタンスを表します。通常はcookieによって追跡されます。 |
| `environment` | [環境](../../data-types/environment.md) | イベント監視の状況コンテキストに関する情報、特にネットワークやソフトウェアのバージョンなどの一時的な情報の詳細を説明します。 |
| `placeContext` | [配置コンテキスト](../../data-types/place-context.md) | イベント観測に関連する過渡的な状況を説明します。 例としては、天気、ローカル時間、トラフィック、曜日、稼働日と休日、勤務時間など、ロケール固有の情報があります。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/experience-event/experienceevent-environment-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/experience-event/experienceevent-environment-details.schema.json)
