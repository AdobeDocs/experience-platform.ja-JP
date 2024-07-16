---
title: メディアコレクションの詳細データタイプ
description: メディアコレクションの詳細 Experience Data Model （XDM）データタイプについて説明します。
exl-id: 1faf60f7-6afb-4ce2-b50d-967776a57715
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 1%

---

# [!UICONTROL  メディアコレクションの詳細 ] データタイプ

[!UICONTROL  メディアコレクションの詳細 ] は、メディア再生イベントに関する重要な詳細をキャプチャする標準の Experience Data Model （XDM）データタイプです。 [!UICONTROL  メディアコレクションの詳細 ] データタイプを使用すると、コンテンツ内の再生ヘッドの位置、一意のセッション識別子、セッションに関連するネストされた様々なプロパティなど、詳細をキャプチャできます。 このデータタイプは、再生エクスペリエンスの包括的な概要を提供し、再生セッション中のメディア消費パターンと関連イベントのトラッキングおよび分析を可能にします。

>[!NOTE]
>
>Media Collection フィールドは、データをキャプチャし、さらに処理するために他のAdobe サービスに送信します。 メディアレポートフィールドは、Adobe サービスが送信したメディアコレクションフィールドを分析するためにユーザーが使用します。 このデータは、他の特定のユーザー指標と共に計算され、レポートされます。

+++選択すると、[!UICONTROL  メディアコレクションの詳細 ] データタイプの図が表示されます。
![ メディアコレクションの詳細情報 [!UICONTROL  データタイプ ] 図 ](../images/data-types/media-collection-details.png)
+++

| 表示名 | プロパティ | 必要なイベント | データタイプ | 説明 |
| ------------------------------------ | ----------------------- | ---------------------------------------------------------- | --------- | ----------- |
| [!UICONTROL Advertisingの詳細 ] | `advertisingDetails` | `adStart` | [[!UICONTROL advertisingDetails] - コレクション ](./advertising-details-collection.md) | Advertisingの詳細は、エクスペリエンスイベント中の広告アクティビティに関連する特定の情報を参照します。 これには、広告メタデータ、ターゲティングの詳細およびパフォーマンス指標が含まれます。 |
| [!UICONTROL Advertising ポッドの詳細 ] | `advertisingPodDetails` | `adBreakStart` | [[!UICONTROL advertisingPodDetails] - コレクション ](./advertising-pod-details-collection.md) | Advertising ポッドの詳細には、エクスペリエンスイベント内の広告ポッドに関する情報が含まれます。 広告シーケンス、コンテンツおよびエンゲージメント指標に関するインサイトを提供します。 |
| [!UICONTROL  チャプターの詳細 ] | `chapterDetails` | `chapterStart` | [[!UICONTROL chapterDetails] - コレクション ](./chapter-details-collection.md) | チャプター詳細は、コンテンツのチャプターまたはセグメント化された部分に関連するデータをキャプチャします。 チャプターマーカー、タイムラインおよび関連するメタデータに関する情報が提供されます。 |
| [!UICONTROL  エラーの詳細 ] | `errorDetails` | `error` | [[!UICONTROL errorDetails] - コレクション ](./error-details-collection.md) | エラーの詳細には、エクスペリエンスイベント中に発生したエラーに関する情報が含まれます。 これには、エラーコード、説明、タイムスタンプおよび関連するコンテキストデータが含まれます。 |
| [!UICONTROL  状態のリスト終了 ] | `statesEnd` | `statesUpdate` で使用 | [[!UICONTROL statesEnd] - コレクション ](./list-of-states-end-collection.md) | States End エクスペリエンスイベントの終了時に状態をリストする配列を提供します。 最終的な再生状態やコンテンツのステータスに関する詳細が含まれています。 |
| [!UICONTROL  状態のリストが開始 ] | `statesStart` | `statesUpdate` で使用 | [[!UICONTROL statesStart] - コレクション ](./list-of-states-start-collection.md) | 状態 Start は、エクスペリエンスイベントの開始時に状態をリストする配列を提供します。 再生、ユーザーのアクションまたはコンテンツの詳細に関連するデータが含まれます。 |
| [!UICONTROL Qoe データの詳細 ] | `qoeDataDetails` | すべてのオプション | [[!UICONTROL qoeDataDetails] - コレクション ](./qoe-data-details-collection.md) | QoE （エクスペリエンス品質）データの詳細は、パフォーマンス関連の指標とユーザーエクスペリエンスのデータを取り込みます。 品質、応答性、ユーザーとのインタラクションに関するインサイトを提供します。 |
| [!UICONTROL  セッションの詳細 ] | `sessionDetails` | `sessionStart` | [[!UICONTROL sessionDetails] - コレクション ](./session-details-collection.md) | セッションの詳細には、エクスペリエンスイベントに関連する包括的な情報が含まれ、再生セッションに関連するユーザーインタラクション、期間およびコンテキストデータに関するインサイトが提供されます。 |
| [!UICONTROL  カスタムメタデータ ] | `customMetadata` | `sessionStart`、`adStart`、`sessionStart` の場合はオプション | [[!UICONTROL customMetadataDetails] - コレクション ](./custom-metadata-details-collection.md) | カスタムメタデータには、エクスペリエンスイベントに関連付けられたユーザー定義または追加のメタデータが含まれます。 このメタデータを使用すると、パーソナライズされたデータや特定のデータをイベントコンテキストに含めることができます。 |
| [!UICONTROL  メディアセッション ID] | `sessionID` | すべてのイベント **除く**`sessionStart` とダウンロードされたコンテンツ。 | 文字列 | メディアセッション ID は、個々の再生セッション中に、コンテンツストリームのインスタンスを一意に識別します。 ユーザーまたはビューアに関連付けられた特定の再生エクスペリエンスをトラッキングおよび管理するための独特の識別子として機能します。<br><em> メモ：<em>`sessionId` は、`sessionStart` を除くすべてのイベントで送信されます。ただし、ダウンロードされたすべてのイベントでは送信されません。 |
| [!UICONTROL  再生ヘッド ] | `playhead` | すべてのイベント | 整数 | 再生ヘッドは、メディアコンテンツ内の現在の再生位置を表します。 ライブコンテンツの場合は、その日の現在の時間（0 &lt; =再生ヘッド &lt; 86400）を示します。 録画されたコンテンツの場合は、コンテンツのデュレーションの現在の秒数（0 &lt; =再生ヘッド &lt; コンテンツの長さ）が反映されます。 |

{style="table-layout:auto"}
