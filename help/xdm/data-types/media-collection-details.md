---
title: メディアコレクション詳細データタイプ
description: メディアコレクションの詳細のエクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: fe239bee3c853d43c04200092f59537dfeb00c87
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 1%

---

# [!UICONTROL メディアコレクションの詳細] データタイプ

[!UICONTROL メディアコレクションの詳細] は、メディア再生イベントに関する重要な詳細をキャプチャする、標準のエクスペリエンスデータモデル (XDM) データタイプです。 以下を使用します。 [!UICONTROL メディアコレクションの詳細] コンテンツ内の再生ヘッドの位置、一意のセッション識別子、セッションに関連する様々なネストされたプロパティなどの詳細を取り込むデータ型です。 このデータタイプは、再生セッション中のメディア消費パターンおよび関連するイベントの追跡と分析を可能にする、再生エクスペリエンスの包括的な概要を提供します。

>[!NOTE]
>
>メディアコレクションフィールドは、データをキャプチャして他のAdobe サービスに送信し、さらに処理します。 メディアレポートフィールドは、Adobe サービスが送信したメディアコレクションフィールドを分析するために使用します。 このデータは、他の特定のユーザー指標と共に、計算され、レポートされます。

+++選択すると、 [!UICONTROL メディアコレクションの詳細] データタイプ。
![の図 [!UICONTROL メディアコレクションの詳細情報] データタイプ。](../images/data-types/media-collection-details.png)
+++

| 表示名 | プロパティ | 次に必要なイベント： | データタイプ | 説明 |
| ------------------------------------ | ----------------------- | ---------------------------------------------------------- | --------- | ----------- |
| [!UICONTROL 広告の詳細] | `advertisingDetails` | `adStart` | [[!UICONTROL advertisingDetails]  — コレクション](./advertising-details-collection.md) | 広告の詳細とは、エクスペリエンスイベント中の広告アクティビティに関する特定の情報を指します。 これには、広告メタデータ、ターゲティングの詳細、パフォーマンス指標が含まれます。 |
| [!UICONTROL 広告ポッドの詳細] | `advertisingPodDetails` | `adBreakStart` | [[!UICONTROL advertisingPodDetails]  — コレクション](./advertising-pod-details-collection.md) | 広告ポッドの詳細には、エクスペリエンスイベント内の広告ポッドに関する情報が含まれます。 広告シーケンス、コンテンツ、エンゲージメント指標に関するインサイトを提供します。 |
| [!UICONTROL チャプターの詳細] | `chapterDetails` | `chapterStart` | [[!UICONTROL chapterDetails]  — コレクション](./chapter-details-collection.md) | チャプターの詳細は、コンテンツの章またはセグメント化された部分に関連するデータをキャプチャします。 チャプターマーカー、タイムラインおよび関連するメタデータに関する情報が提供されます。 |
| [!UICONTROL エラーの詳細] | `errorDetails` | `error` | [[!UICONTROL errorDetails]  — コレクション](./error-details-collection.md) | エラーの詳細には、エクスペリエンスイベント中に発生したエラーに関する情報が含まれます。 これには、エラーコード、説明、タイムスタンプおよび関連するコンテキストデータが含まれます。 |
| [!UICONTROL List Of States End] | `statesEnd` | 使用場所 `statesUpdate` | [[!UICONTROL statesEnd]  — コレクション](./list-of-states-end-collection.md) | 状態 End は、エクスペリエンスイベントの終了時の状態をリストする配列を提供します。 これには、最終的な再生状態やコンテンツのステータスに関する詳細が含まれます。 |
| [!UICONTROL List Of States Start] | `statesStart` | 使用場所 `statesUpdate` | [[!UICONTROL statesStart]  — コレクション](./list-of-states-start-collection.md) | 状態 Start は、エクスペリエンスイベントの先頭の状態をリストする配列を提供します。 再生、ユーザーアクション、コンテンツの詳細に関するデータが含まれます。 |
| [!UICONTROL データの詳細の問い合わせ] | `qoeDataDetails` | すべてのオプション | [[!UICONTROL qoeDataDetails]  — コレクション](./qoe-data-details-collection.md) | QoE(Quality of Experience) データの詳細では、パフォーマンス関連の指標とユーザーエクスペリエンスデータが取り込まれます。 品質、応答性、ユーザーインタラクションに関するインサイトを提供します。 |
| [!UICONTROL セッションの詳細] | `sessionDetails` | `sessionStart` | [[!UICONTROL sessionDetails]  — コレクション](./session-details-collection.md) | セッションの詳細には、エクスペリエンスイベントに関連する包括的な情報が含まれ、ユーザーのインタラクション、時間、再生セッションに関連するコンテキストデータに関するインサイトが提供されます。 |
| [!UICONTROL カスタムメタデータ] | `customMetadata` | オプション： `sessionStart`, `adStart`, `sessionStart` | [[!UICONTROL customMetadataDetails]  — コレクション](./custom-metadata-details-collection.md) | カスタムメタデータには、エクスペリエンスイベントに関連付けられたユーザー定義のメタデータまたは追加のメタデータが含まれます。 このメタデータを使用すると、パーソナライズされたデータや特定のデータをイベントコンテキストに含めることができます。 |
| [!UICONTROL メディアセッション ID] | `sessionID` | すべてのイベント **例外** `sessionStart` およびダウンロードしたコンテンツ。 | 文字列 | メディアセッション ID は、個々の再生セッション中に、コンテンツストリームのインスタンスを一意に識別します。 これは、ユーザーまたはビューアに関連付けられた特定の再生エクスペリエンスを追跡および管理するための独特な識別子として機能します。<br><em>注意：<em>`sessionId` は、以下を除くすべてのイベントで送信されます。 `sessionStart` およびを含むすべてのダウンロードされたイベントに対して。 |
| [!UICONTROL 再生ヘッド] | `playhead` | すべてのイベント | 整数 | 再生ヘッドは、メディアコンテンツ内の現在の再生位置を表します。 ライブコンテンツの場合は、1 日の現在の秒 (0 &lt;=再生ヘッド &lt; 86400) を示します。 記録されたコンテンツの場合は、コンテンツの再生時間の現在の秒（0 &lt;=再生ヘッド &lt; コンテンツの長さ）を反映します。 |

{style="table-layout:auto"}
