---
title: QoE(Quality of Experience) データ詳細レポートのデータタイプ
description: QoE(Quality of Experience) データの詳細のレポートのデータタイプエクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: a604dc8b541784ace8aedef42030e5bd8b646c28
workflow-type: tm+mt
source-wordcount: '499'
ht-degree: 4%

---

# QoE(Quality of Experience) データ詳細レポートのデータタイプ

[!UICONTROL QoE データの詳細] レポートは、標準のエクスペリエンスデータモデル (XDM) データタイプで、メディア再生中のエクスペリエンスの質 (QoE) に関する詳細な指標を提供します。 以下を使用します。 [!UICONTROL QoE データの詳細] ビットレート情報、フレームレート、バッファリングイベント、ドロップフレームなどの詳細を取り込むためのレポートデータ型。 メディアレポートフィールドは、Adobe サービスが送信したメディアコレクションフィールドを分析するために使用します。 このデータは、他の特定のユーザー指標と共に、計算され、レポートされます。 このデータタイプを使用すると、再生品質を分析でき、ストリーミングのパフォーマンス、ユーザーエクスペリエンス、再生セッション中に発生する可能性のある問題に関するインサイトを得ることができます。

+++QoE Data Details Reporting データタイプを表示する場合に選択します。
![QoE(Quality of Experience) データ詳細レポートのデータタイプを示す図です。](../images/data-types/qoe-data-details-reporting.png)
+++

>[!NOTE]
>
>各ディスプレイ名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれます。 リンクされたページには、Adobe、実装値、ネットワークパラメーター、レポートおよび重要な考慮事項によって収集されるビデオ広告データの詳細が含まれます。

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------|-----------|---------------------------------------------------------------------------------------------------|
| [[!UICONTROL 平均ビットレート]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#average-bitrate-1) | `bitrateAverage` | 数値 | 平均ビットレート（kbps、整数）。 ビットレート値の加重平均として計算されます。 |
| [[!UICONTROL 平均ビットレートバケット]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#average-bitrate) | `bitrateAverageBucket` | 文字列 | 100 kbps の間隔で事前定義されたグループに分類された平均ビットレート (kbps)。 |
| [[!UICONTROL ビットレート]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#average-bitrate) | `bitrate` | 整数 | ビットレート値 (kbps)。 |
| [[!UICONTROL ビットレート変更の影響を受けたストリーム]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#bitrate-change-impacted-streams) | `hasBitrateChangeImpactedStreams` | ブール値 | 再生中にビットレートの変更がストリームに与えた影響かどうかを示します。 |
| [[!UICONTROL ビットレート変更]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#bitrate-changes) | `bitrateChangeCount` | 整数 | 再生中にビットレートが変更された合計数。 |
| [[!UICONTROL バッファーイベント]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#buffer-events) | `bufferCount` | 整数 | 再生中の様々なバッファー状態の数。 |
| [[!UICONTROL バッファーの影響を受けたストリーム]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#buffer-impacted-streams) | `hasBufferImpactedStreams` | ブール値 | 再生中にストリームがバッファリングの影響を受けたかどうかを示します。 |
| [[!UICONTROL ドロップフレームの影響を受けたストリーム]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#dropped-frame-impacted-streams) | `hasDroppedFrameImpactedStreams` | ブール値 | 再生中にストリームがドロップフレームの影響を受けたかどうかを示します。 |
| [[!UICONTROL ドロップフレーム]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#dropped-frames-1) | `droppedFrames` | 整数 | 再生中にドロップしたフレームの総数。 |
| [[!UICONTROL 開始前にドロップ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#drops-before-start) | `isDroppedBeforeStart` | ブール値 | 広告に関係なく、ユーザーがビデオを開始する前に終了したかどうかを示します。 |
| [[!UICONTROL エラーの影響を受けたストリーム]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#error-impacted-streams) | `hasErrorImpactedStreams` | ブール値 | 再生中にストリームでエラーが発生したかどうかを示します。 |
| [[!UICONTROL エラー]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#errors-%2F-error-events) | `errorCount` | 整数 | 再生中に発生したエラーの総数。 |
| [[!UICONTROL 外部エラー ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#external-error-ids) | `externalErrors` | 文字列の配列 | 外部ソースからの一意のエラー ID（CDN エラーなど）。 |
| [[!UICONTROL 1 秒あたりのフレーム数]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#frames-per-second) | `framesPerSecond` | 整数 | 現在のストリームフレームレート（1 秒あたりのフレーム数）。 |
| [[!UICONTROL メディア SDK のエラー ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#media-sdk-error-ids) | `mediaSdkErrors` | 文字列の配列 | 再生時にメディア SDK によって生成される一意のエラー ID。 |
| [[!UICONTROL プレーヤー SDK のエラー ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#player-sdk-error-ids) | `playerSdkErrors` | 文字列の配列 | 再生時にプレーヤー SDK によって生成される一意のエラー ID。 |
| [[!UICONTROL 停止イベント]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#stalling-events) | `stallCount` | 整数 | 再生中の停止イベントの数。 |
| [[!UICONTROL 停止の影響を受けたストリーム]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#stalling-impacted-streams) | `hasStallImpactedStreams` | ブール値 | ストリームの再生中に停止が発生したかどうかを示します。 |
| [[!UICONTROL 開始時間]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#time-to-start-1) | `timeToStart` | 整数 | ビデオの読み込みと開始の間の時間（秒）。 |
| [[!UICONTROL 合計バッファー時間]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#total-buffer-duration-1) | `bufferTime` | 整数 | 再生中にバッファリングに費やした合計時間（秒）。 |
| [[!UICONTROL 合計停止時間]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#total-stalling-duration) | `stallTime` | 整数 | 再生中に再生が停止した合計時間（秒）。 |

{style="table-layout:auto"}
