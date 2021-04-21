---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM;ExperienceEvent；フィールド；スキーマ;スキーマ;スキーマデザイン；ミックスイン；環境;環境の詳細；
solution: Experience Platform
title: 環境の詳細Mixin
topic-legacy: overview
description: このドキュメントでは、ExperienceEvent環境の詳細ミックスインの概要を説明します。
exl-id: 1d25b98f-66ac-443f-9b1c-dfd20a168c59
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 3%

---

# [!UICONTROL 環境] の詳細

>[!NOTE]
>
>いくつかのミックスインの名前が変更されました。 詳しくは、[mixin name updates](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL 環境] の詳細は、デバイスの詳細、ブラウザーの情報、ローカル時間、その他の地理的な情報など、エクスペリエンスイベントに関連する環境の詳細に関する情報を取り込むために使用される、 [[!DNL XDM ExperienceEvent] ](../../classes/individual-profile.md) クラスの標準ミックスインです。

<img src="../../images/mixins/environment-details.png" width="500" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `device` | [デバイス](../../data-types/device.md) | セッション間で追跡可能な、識別されたデバイス、アプリケーション、またはデバイスのブラウザーインスタンスを表します。通常はcookieによって追跡されます。 |
| `environment` | [環境](../../data-types/environment.md) | イベント監視の状況コンテキストに関する情報、特にネットワークやソフトウェアのバージョンなどの一時的な情報の詳細を説明します。 |
| `placeContext` | [配置コンテキスト](../../data-types/place-context.md) | イベント観測に関連する過渡的な状況を説明します。 例としては、天気、ローカル時間、トラフィック、曜日、稼働日と休日、勤務時間など、ロケール固有の情報があります。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/experience-event/experienceevent-environment-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/experience-event/experienceevent-environment-details.schema.json)
