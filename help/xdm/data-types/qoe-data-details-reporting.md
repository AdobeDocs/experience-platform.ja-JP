---
title: QoE （エクスペリエンス品質）データの詳細レポートのデータタイプ
description: QoE （エクスペリエンス品質）データの詳細レポートデータタイプエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 608baa9b-12ca-466c-a962-1401abc0344e
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 4%

---

# QoE （Quality of Experience）データの詳細レポートのデータタイプ

[!UICONTROL QoE データの詳細 ] レポートは、標準のエクスペリエンスデータモデル（XDM）データタイプで、メディア再生中のエクスペリエンスの品質（QoE）に関連する詳細な指標を提供します。 [!UICONTROL QoE データの詳細 ] レポート データ タイプを使用して、ビットレート情報、フレーム レート、バッファリング イベント、ドロップフレームなどの詳細をキャプチャします。 メディアレポートフィールドは、Adobe サービスが送信したメディアコレクションフィールドを分析するためにユーザーが使用します。 このデータは、他の特定のユーザー指標と共に計算され、レポートされます。 このデータタイプを使用すると、再生の品質を分析できるので、ストリーミングのパフォーマンス、ユーザーエクスペリエンス、再生セッション中に発生する可能性のある問題に関するインサイトが得られます。

+++QoE データの詳細レポートのデータの種類を表示する場合に選択します。
![QoE （エクスペリエンス品質）データの詳細レポートデータタイプの図。](../images/data-types/qoe-data-details-reporting.png)
+++

>[!NOTE]
>
>各表示名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれています。 リンク先のページには、Adobeで収集された動画広告データの詳細、実装値、ネットワークパラメーター、レポート、重要な検討事項が含まれています。

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------|-----------|---------------------------------------------------------------------------------------------------|
| [[!UICONTROL  平均ビットレート ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#average-bitrate-1) | `bitrateAverage` | 数値 | 平均ビットレート（kbps、整数）。 ビットレート値の加重平均として計算されます。 |
| [[!UICONTROL  平均ビットレートバケット ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#average-bitrate) | `bitrateAverageBucket` | 文字列 | 100 kbps の間隔で事前定義済みのバケットに分類された平均ビットレート（kbps）。 |
| [[!UICONTROL  ビットレート ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#average-bitrate) | `bitrate` | 整数 | ビットレート値（kbps）。 |
| [[!UICONTROL  ビットレート変更の影響を受けたストリーム ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#bitrate-change-impacted-streams) | `hasBitrateChangeImpactedStreams` | ブール値 | 再生中にビットレート変更の影響を受けたストリームがあるかどうかを示します。 |
| [[!UICONTROL  ビットレートの変更 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#bitrate-changes) | `bitrateChangeCount` | 整数 | 再生中のビットレート変更の合計数。 |
| [[!UICONTROL  バッファーイベント ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#buffer-events) | `bufferCount` | 整数 | 再生中の様々なバッファー状態の数。 |
| [[!UICONTROL  バッファーの影響を受けたストリーム ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#buffer-impacted-streams) | `hasBufferImpactedStreams` | ブール値 | 再生中にストリームがバッファリングの影響を受けたかどうかを示します。 |
| [[!UICONTROL  ドロップフレームの影響を受けたストリーム ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#dropped-frame-impacted-streams) | `hasDroppedFrameImpactedStreams` | ブール値 | 再生中にドロップフレームによってストリームが影響を受けたかどうかを示します。 |
| [[!UICONTROL  ドロップフレーム ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#dropped-frames-1) | `droppedFrames` | 整数 | 再生中にドロップしたフレームの合計数。 |
| [[!UICONTROL  開始前にドロップ ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#drops-before-start) | `isDroppedBeforeStart` | ブール値 | 広告に関係なく、ユーザーが開始前にビデオを終了するかどうかを示します。 |
| [[!UICONTROL  エラーの影響を受けたストリーム ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#error-impacted-streams) | `hasErrorImpactedStreams` | ブール値 | 再生中にストリームでエラーが発生したかどうかを示します。 |
| [[!UICONTROL  エラー ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#errors-%2F-error-events) | `errorCount` | 整数 | 再生中に発生したエラーの合計数。 |
| [[!UICONTROL  外部エラー ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#external-error-ids) | `externalErrors` | 文字列の配列 | 外部ソースからの一意のエラー ID （CDN エラーなど）。 |
| [[!UICONTROL  フレーム/秒 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#frames-per-second) | `framesPerSecond` | 整数 | 現在のストリームフレームレート（1 秒あたりのフレーム数）。 |
| [[!UICONTROL Media SDK エラー ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#media-sdk-error-ids) | `mediaSdkErrors` | 文字列の配列 | 再生中に Media SDK によって生成される一意のエラー ID。 |
| [[!UICONTROL  プレーヤー SDK エラー ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#player-sdk-error-ids) | `playerSdkErrors` | 文字列の配列 | 再生中にプレーヤー SDK によって生成される一意のエラー ID。 |
| [[!UICONTROL  停止イベント ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#stalling-events) | `stallCount` | 整数 | 再生中の停止イベントの数。 |
| [[!UICONTROL  停止の影響を受けたストリーム ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#stalling-impacted-streams) | `hasStallImpactedStreams` | ブール値 | 再生中にストリームが停止したかどうかを示します。 |
| [[!UICONTROL  開始時間 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#time-to-start-1) | `timeToStart` | 整数 | ビデオの読み込みから開始までの時間（秒単位）。 |
| [[!UICONTROL  合計バッファー時間 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#total-buffer-duration-1) | `bufferTime` | 整数 | 再生中にバッファリングに費やした合計時間（秒単位）。 |
| [[!UICONTROL  合計停止時間 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#total-stalling-duration) | `stallTime` | 整数 | 再生中に再生が停止した合計時間（秒単位）。 |

{style="table-layout:auto"}
