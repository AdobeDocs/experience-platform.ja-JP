---
title: QoE(Quality of Experience) データ詳細情報データタイプ
description: QoE(Quality of Experience) データ詳細情報データタイプエクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: 65f3dcf1cacfbc4e8a598244810d238bd88f64bd
workflow-type: tm+mt
source-wordcount: '427'
ht-degree: 5%

---

# QoE(Quality of Experience) データ詳細情報データタイプ

[!UICONTROL QoE データの詳細情報] は標準のエクスペリエンスデータモデル (XDM) データタイプで、メディア再生中のエクスペリエンスの質 (QoE) に関する詳細な指標を提供します。 以下を使用します。 [!UICONTROL QoE データの詳細情報] ビットレート情報、フレームレート、バッファリングイベント、ドロップフレームなどの詳細を取り込むためのデータタイプ。 このデータタイプを使用すると、再生品質を分析でき、ストリーミングのパフォーマンス、ユーザーエクスペリエンス、再生セッション中に発生する可能性のある問題に関するインサイトを得ることができます。

+++「QoE Data Details Information」データタイプを表示する場合に選択します。
![QoE(Quality of Experience) データ詳細情報データタイプを示す図です。](../images/data-types/qoe-data-details-information.png)
+++

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------------------------------|----------------------------|-----------|--------------------------------------------------------------------------------------------------|
| [!UICONTROL 平均ビットレートバケット] | `bitrateAverageBucket` | 文字列 | 100 kbps の間隔で事前定義されたグループに分類された平均ビットレート (kbps)。 |
| [!UICONTROL ビットレート] | `bitrate` | 整数 | ビットレート値 (kbps)。 |
| [!UICONTROL 平均ビットレート] | `bitrateAverage` | 数値 | 平均ビットレート（kbps、整数）。 ビットレート値の加重平均として計算されます。 |
| [!UICONTROL ビットレート変更の影響を受けたストリーム] | `hasBitrateChangeImpactedStreams` | ブール値 | 再生中にビットレートの変更がストリームに与えた影響かどうかを示します。 |
| [!UICONTROL ビットレート変更] | `bitrateChangeCount` | 整数 | 再生中にビットレートが変更された合計数。 |
| [!UICONTROL ドロップフレームの影響を受けたストリーム] | `hasDroppedFrameImpactedStreams` | ブール値 | 再生中にストリームがドロップフレームの影響を受けたかどうかを示します。 |
| [!UICONTROL ドロップフレーム] | `droppedFrames` | 整数 | 再生中にドロップしたフレームの総数。 |
| [!UICONTROL 開始前にドロップ] | `isDroppedBeforeStart` | ブール値 | 広告に関係なく、ユーザーがビデオを開始する前に終了したかどうかを示します。 |
| [!UICONTROL 1 秒あたりのフレーム数] | `framesPerSecond` | 整数 | 現在のストリームフレームレート（1 秒あたりのフレーム数）。 |
| [!UICONTROL 開始時間] | `timeToStart` | 整数 | ビデオの読み込みと開始の間の時間（秒）。 |
| [!UICONTROL バッファーの影響を受けたストリーム] | `hasBufferImpactedStreams` | ブール値 | 再生中にストリームがバッファリングの影響を受けたかどうかを示します。 |
| [!UICONTROL バッファーイベント] | `bufferCount` | 整数 | 再生中の様々なバッファー状態の数。 |
| [!UICONTROL 合計バッファー時間] | `bufferTime` | 整数 | 再生中にバッファリングに費やした合計時間（秒）。 |
| [!UICONTROL エラーの影響を受けたストリーム] | `hasErrorImpactedStreams` | ブール値 | 再生中にストリームでエラーが発生したかどうかを示します。 |
| [!UICONTROL エラー] | `errorCount` | 整数 | 再生中に発生したエラーの総数。 |
| [!UICONTROL 停止の影響を受けたストリーム] | `hasStallImpactedStreams` | ブール値 | ストリームの再生中に停止が発生したかどうかを示します。 |
| [!UICONTROL 停止イベント] | `stallCount` | 整数 | 再生中の停止イベントの数。 |
| [!UICONTROL 合計停止時間] | `stallTime` | 整数 | 再生中に再生が停止した合計時間（秒）。 |
| [!UICONTROL プレーヤー SDK のエラー ID] | `playerSdkErrors` | 文字列の配列 | 再生時にプレーヤー SDK によって生成される一意のエラー ID。 |
| [!UICONTROL 外部エラー ID] | `externalErrors` | 文字列の配列 | 外部ソースからの一意のエラー ID（CDN エラーなど）。 |
| [!UICONTROL メディア SDK のエラー ID] | `mediaSdkErrors` | 文字列の配列 | 再生時にメディア SDK によって生成される一意のエラー ID。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/components/datatypes/qoedatadetails.schema.json).

