---
title: メディア詳細情報データタイプ
description: メディア詳細情報エクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: 65f3dcf1cacfbc4e8a598244810d238bd88f64bd
workflow-type: tm+mt
source-wordcount: '506'
ht-degree: 2%

---

# [!UICONTROL メディアの詳細情報] データタイプ

[!UICONTROL メディアの詳細情報] は、メディア再生イベントに関する重要な詳細をキャプチャする、標準のエクスペリエンスデータモデル (XDM) データタイプです。 以下を使用します。 [!UICONTROL メディアの詳細情報] コンテンツ内の再生ヘッドの位置、一意のセッション識別子、セッションに関連する様々なネストされたプロパティなどの詳細を取り込むデータ型です。 このデータタイプは、再生エクスペリエンスの包括的な概要を提供し、再生セッション中のメディア消費パターンおよび関連するイベントの追跡と分析を可能にします。

+++選択すると、 [!UICONTROL メディアの詳細情報] データタイプ。
![の図 [!UICONTROL メディアの詳細情報] データタイプ。](../images/data-types/media-details-information.png)
+++

| 表示名 | プロパティ | データタイプ | 説明 |
| --------------------- | --------------- | --------- | ----------- |
| [!UICONTROL 再生ヘッド] | `playhead` | 整数 | 再生ヘッドは、メディアコンテンツ内の現在の再生位置を表します。 ライブコンテンツの場合は、1 日の現在の秒 (0 &lt;=再生ヘッド &lt; 86400) を示します。 記録されたコンテンツの場合は、コンテンツの再生時間の現在の秒（0 &lt;=再生ヘッド &lt; コンテンツの長さ）を反映します。 |
| [!UICONTROL メディアセッション ID] | `sessionID` | 文字列 | メディアセッション ID は、個々の再生セッション中に、コンテンツストリームのインスタンスを一意に識別します。 これは、ユーザーまたはビューアに関連付けられた特定の再生エクスペリエンスを追跡および管理するための独特な識別子として機能します。 |
| [!UICONTROL セッションの詳細] | `sessionDetails` | [[!UICONTROL sessionDetails]](./session-details-information.md) | セッションの詳細には、エクスペリエンスイベントに関連する包括的な情報が含まれ、ユーザーのインタラクション、時間、再生セッションに関連するコンテキストデータに関するインサイトが提供されます。 |
| [!UICONTROL 広告の詳細] | `advertisingDetails` | [[!UICONTROL advertisingDetails]](./advertising-details-information.md) | 広告の詳細とは、エクスペリエンスイベント中の広告アクティビティに関する特定の情報を指します。 これには、広告メタデータ、ターゲティングの詳細、パフォーマンス指標が含まれます。 |
| [!UICONTROL 広告ポッドの詳細] | `advertisingPodDetails` | [[!UICONTROL advertisingPodDetails]](./advertising-pod-details-information.md) | 広告ポッドの詳細には、エクスペリエンスイベント内の広告ポッドに関する情報が含まれます。 広告シーケンス、コンテンツ、エンゲージメント指標に関するインサイトを提供します。 |
| [!UICONTROL チャプターの詳細] | `chapterDetails` | [[!UICONTROL chapterDetails]](./chapter-details-information.md) | チャプターの詳細は、コンテンツの章またはセグメント化された部分に関連するデータをキャプチャします。 チャプターマーカー、タイムラインおよび関連するメタデータに関する情報が提供されます。 |
| [!UICONTROL エラーの詳細] | `errorDetails` | [[!UICONTROL errorDetails]](./error-details-information.md) | エラーの詳細には、エクスペリエンスイベント中に発生したエラーに関する情報が含まれます。 これには、エラーコード、説明、タイムスタンプおよび関連するコンテキストデータが含まれます。 |
| [!UICONTROL データの詳細の問い合わせ] | `qoeDataDetails` | [[!UICONTROL qoeDataDetails]](./qoe-data-details-information.md) | QoE(Quality of Experience) データの詳細では、パフォーマンス関連の指標とユーザーエクスペリエンスデータが取り込まれます。 品質、応答性、ユーザーインタラクションに関するインサイトを提供します。 |
| [!UICONTROL List Of States Start] | `statesStart` | [[!UICONTROL playerStateData]](./player-state-data-information.md) | 状態 Start は、エクスペリエンスイベントの先頭の状態をリストする配列を提供します。 再生、ユーザーアクション、コンテンツの詳細に関するデータが含まれます。 |
| [!UICONTROL List Of States End] | `statesEnd` | [[!UICONTROL playerStateData]](./player-state-data-information.md) | 状態 End は、エクスペリエンスイベントの終了時の状態をリストする配列を提供します。 これには、最終的な再生状態やコンテンツのステータスに関する詳細が含まれます。 |
| [!UICONTROL 州のリスト] | `states` | [[!UICONTROL playerStateData]](./player-state-data-information.md) | States プロパティは、エクスペリエンスイベント全体で様々な状態をキャプチャする配列です。 このプロパティは、再生、ユーザーアクション、コンテンツの変更に関する順次データを提供します。 |
| [!UICONTROL カスタムメタデータ] | `customMetadata` | [[!UICONTROL customMetadataDetails]](./custom-metadata-details-information.md) | カスタムメタデータには、エクスペリエンスイベントに関連付けられたユーザー定義のメタデータまたは追加のメタデータが含まれます。 このメタデータを使用すると、パーソナライズされたデータや特定のデータをイベントコンテキストに含めることができます。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/components/datatypes/mediadetails.schema.json)
