---
title: メディアレポートの詳細データタイプ
description: メディアレポートの詳細 Experience Data Model （XDM）データタイプについて説明します。
exl-id: e8bf20a9-9ac0-4339-8200-5d6d9328ce3b
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 2%

---

# [!UICONTROL  メディアレポートの詳細 ] データタイプ

>[!NOTE]
>
>Media Collection フィールドは、データをキャプチャし、さらに処理するために他のAdobe サービスに送信します。 メディアレポートフィールドは、Adobe サービスが送信したメディアコレクションフィールドを分析するためにユーザーが使用します。 このデータは、他の特定のユーザー指標と共に計算され、レポートされます。

[!UICONTROL  メディアレポートの詳細 ] は、メディア再生イベントに関する重要な詳細をキャプチャする標準の Experience Data Model （XDM）データタイプです。 [!UICONTROL  メディアレポート詳細 ] データタイプを使用すると、コンテンツ内の再生ヘッドの位置、一意のセッション識別子、セッションに関連するネストされた様々なプロパティなど、詳細をキャプチャできます。 このデータタイプは、再生エクスペリエンスの包括的な概要を提供し、再生セッション中のメディア消費パターンと関連イベントのトラッキングおよび分析を可能にします。

>[!NOTE]
>
>以下に説明するフィールドは、リクエストの作成に直接使用されません。 代わりに、Adobe Experience PlatformまたはAdobe Analyticsに送信されるフィールドのコレクションがリクエストデータから組み立てられ、指標がサーバーインフラストラクチャによって組み込まれるか処理されます。 Platform は様々なタイプのユーザーイベントを収集しますが、ユーザーに返されるレポートは、`media.sessionStart`、`media.adStart`、`media.sessionComplete` などの特定のイベントに焦点を当てています。 つまり、収集時に 12 種類のイベントを送信しても、レポートには、以下に示す 5 つのイベントに基づく分類のみが表示されます。

+++選択すると、[!UICONTROL  メディアレポートの詳細 ] データタイプの図が表示されます。
![ メディアレポートの詳細 [!UICONTROL  データタイプ ] 図 ](../images/data-types/media-reporting-details.png)
+++

| 表示名 | プロパティ | データタイプ | 説明 |
| --------------------- | --------------- | --------- | ----------- |
| [!UICONTROL Advertisingの詳細 ] | `advertisingDetails` | [[!UICONTROL advertisingDetails]](./advertising-details-reporting.md) | Advertisingの詳細は、エクスペリエンスイベント中の広告アクティビティに関連する特定の情報を参照します。 これには、広告メタデータ、ターゲティングの詳細およびパフォーマンス指標が含まれます。 |
| [!UICONTROL Advertising ポッドの詳細 ] | `advertisingPodDetails` | [[!UICONTROL advertisingPodDetails]](./advertising-pod-details-reporting.md) | Advertising ポッドの詳細には、エクスペリエンスイベント内の広告ポッドに関する情報が含まれます。 広告シーケンス、コンテンツおよびエンゲージメント指標に関するインサイトを提供します。 |
| [!UICONTROL  チャプターの詳細 ] | `chapterDetails` | [[!UICONTROL chapterDetails]](./chapter-details-reporting.md) | チャプター詳細は、コンテンツのチャプターまたはセグメント化された部分に関連するデータをキャプチャします。 チャプターマーカー、タイムラインおよび関連するメタデータに関する情報が提供されます。 |
| [!UICONTROL  状態のリスト ] | `states` | [[!UICONTROL playerStateData]](./player-state-data-reporting.md) | States プロパティは、エクスペリエンスイベント全体の様々な状態をキャプチャする配列です。 このプロパティは、再生、ユーザー操作またはコンテンツの変更に関する順次データを提供します。 |
| [!UICONTROL Qoe データの詳細 ] | `qoeDataDetails` | [[!UICONTROL qoeDataDetails]](./qoe-data-details-reporting.md) | QoE （エクスペリエンス品質）データの詳細は、パフォーマンス関連の指標とユーザーエクスペリエンスのデータを取り込みます。 品質、応答性、ユーザーとのインタラクションに関するインサイトを提供します。 |
| [!UICONTROL  セッションの詳細 ] | `sessionDetails` | [[!UICONTROL sessionDetails]](./session-details-reporting.md) | セッションの詳細には、エクスペリエンスイベントに関連する包括的な情報が含まれ、再生セッションに関連するユーザーインタラクション、期間およびコンテキストデータに関するインサイトが提供されます。 |
| [!UICONTROL  カスタムメタデータ ] | `customMetadata` | [[!UICONTROL customMetadataDetails]](./custom-metadata-details-reporting.md) | カスタムメタデータには、エクスペリエンスイベントに関連付けられたユーザー定義または追加のメタデータが含まれます。 このメタデータを使用すると、パーソナライズされたデータや特定のデータをイベントコンテキストに含めることができます。 |
| [!UICONTROL  再生ヘッド ] | `playhead` | 整数 | 再生ヘッドは、メディアコンテンツ内の現在の再生位置を表します。 ライブコンテンツの場合は、その日の現在の時間（0 &lt; =再生ヘッド &lt; 86400）を示します。 録画されたコンテンツの場合は、コンテンツのデュレーションの現在の秒数（0 &lt; =再生ヘッド &lt; コンテンツの長さ）が反映されます。 |

{style="table-layout:auto"}
