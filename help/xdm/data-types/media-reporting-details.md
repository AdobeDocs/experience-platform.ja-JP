---
title: メディアレポートの詳細データタイプ
description: メディアレポートの詳細のエクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: fe239bee3c853d43c04200092f59537dfeb00c87
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 2%

---

# [!UICONTROL メディアレポートの詳細] データタイプ

>[!NOTE]
>
>メディアコレクションフィールドは、データをキャプチャして他のAdobe サービスに送信し、さらに処理します。 メディアレポートフィールドは、Adobe サービスが送信したメディアコレクションフィールドを分析するために使用します。 このデータは、他の特定のユーザー指標と共に、計算され、レポートされます。

[!UICONTROL メディアレポートの詳細] は、メディア再生イベントに関する重要な詳細をキャプチャする、標準のエクスペリエンスデータモデル (XDM) データタイプです。 以下を使用します。 [!UICONTROL メディアレポートの詳細] コンテンツ内の再生ヘッドの位置、一意のセッション識別子、セッションに関連する様々なネストされたプロパティなどの詳細を取り込むデータ型です。 このデータタイプは、再生セッション中のメディア消費パターンおよび関連するイベントの追跡と分析を可能にする、再生エクスペリエンスの包括的な概要を提供します。

>[!NOTE]
>
>以下に示すフィールドは、リクエストの作成に直接使用されるわけではありません。 代わりに、Adobe Experience PlatformまたはAdobe Analyticsに送信されるフィールドのコレクションがリクエストデータから組み立てられ、指標がサーバーインフラストラクチャに組み込まれたり、処理されたりします。 Platform が様々なタイプのユーザーイベントを収集しますが、返されるレポートは、次のような特定のイベントに焦点を当てます。 `media.sessionStart`, `media.adStart`、および `media.sessionComplete`. つまり、収集時に 12 種類のイベントを送信しますが、レポートには次の 5 つのイベントに基づく分類のみが表示されます。

+++選択すると、 [!UICONTROL メディアレポートの詳細] データタイプ。
![の図 [!UICONTROL メディアレポートの詳細] データタイプ。](../images/data-types/media-reporting-details.png)
+++

| 表示名 | プロパティ | データタイプ | 説明 |
| --------------------- | --------------- | --------- | ----------- |
| [!UICONTROL 広告の詳細] | `advertisingDetails` | [[!UICONTROL advertisingDetails]](./advertising-details-reporting.md) | 広告の詳細とは、エクスペリエンスイベント中の広告アクティビティに関する特定の情報を指します。 これには、広告メタデータ、ターゲティングの詳細、パフォーマンス指標が含まれます。 |
| [!UICONTROL 広告ポッドの詳細] | `advertisingPodDetails` | [[!UICONTROL advertisingPodDetails]](./advertising-pod-details-reporting.md) | 広告ポッドの詳細には、エクスペリエンスイベント内の広告ポッドに関する情報が含まれます。 広告シーケンス、コンテンツ、エンゲージメント指標に関するインサイトを提供します。 |
| [!UICONTROL チャプターの詳細] | `chapterDetails` | [[!UICONTROL chapterDetails]](./chapter-details-reporting.md) | チャプターの詳細は、コンテンツの章またはセグメント化された部分に関連するデータをキャプチャします。 チャプターマーカー、タイムラインおよび関連するメタデータに関する情報が提供されます。 |
| [!UICONTROL 州のリスト] | `states` | [[!UICONTROL playerStateData]](./player-state-data-reporting.md) | States プロパティは、エクスペリエンスイベント全体で様々な状態をキャプチャする配列です。 このプロパティは、再生、ユーザーアクション、コンテンツの変更に関する順次データを提供します。 |
| [!UICONTROL データの詳細の問い合わせ] | `qoeDataDetails` | [[!UICONTROL qoeDataDetails]](./qoe-data-details-reporting.md) | QoE(Quality of Experience) データの詳細では、パフォーマンス関連の指標とユーザーエクスペリエンスデータが取り込まれます。 品質、応答性、ユーザーインタラクションに関するインサイトを提供します。 |
| [!UICONTROL セッションの詳細] | `sessionDetails` | [[!UICONTROL sessionDetails]](./session-details-reporting.md) | セッションの詳細には、エクスペリエンスイベントに関連する包括的な情報が含まれ、ユーザーのインタラクション、時間、再生セッションに関連するコンテキストデータに関するインサイトが提供されます。 |
| [!UICONTROL カスタムメタデータ] | `customMetadata` | [[!UICONTROL customMetadataDetails]](./custom-metadata-details-reporting.md) | カスタムメタデータには、エクスペリエンスイベントに関連付けられたユーザー定義のメタデータまたは追加のメタデータが含まれます。 このメタデータを使用すると、パーソナライズされたデータや特定のデータをイベントコンテキストに含めることができます。 |
| [!UICONTROL 再生ヘッド] | `playhead` | 整数 | 再生ヘッドは、メディアコンテンツ内の現在の再生位置を表します。 ライブコンテンツの場合は、1 日の現在の秒 (0 &lt;=再生ヘッド &lt; 86400) を示します。 記録されたコンテンツの場合は、コンテンツの再生時間の現在の秒（0 &lt;=再生ヘッド &lt; コンテンツの長さ）を反映します。 |

{style="table-layout:auto"}
