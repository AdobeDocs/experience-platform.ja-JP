---
title: QoE （エクスペリエンス品質）データの詳細収集データタイプ
description: QoE （エクスペリエンス品質）データの詳細データ収集タイプ エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: d99816d9-e207-434a-9a40-ee9ded46c4d2
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 6%

---

# QoE （エクスペリエンス品質）データの詳細収集データタイプ

[!UICONTROL QoE データの詳細 ] コレクションは、標準のエクスペリエンスデータモデル（XDM）データタイプで、メディア再生中のエクスペリエンスの品質（QoE）に関連する詳細な指標を提供します。 [!UICONTROL QoE データの詳細 ] 収集データ型を使用して、ビットレート情報、フレームレート、バッファリングイベント、ドロップフレームなどの詳細をキャプチャします。 メディアコレクションフィールドは、データをキャプチャし、さらに処理するために他のAdobe サービスに送信します。 このデータタイプを使用すると、再生の品質を分析できるので、ストリーミングのパフォーマンス、ユーザーエクスペリエンス、再生セッション中に発生する可能性のある問題に関するインサイトが得られます。

+++QoE データの詳細データの種類を表示する場合に選択します。
![QoE （エクスペリエンス品質）データの詳細収集データタイプの図。](../images/data-types/qoe-data-details-collection.png)
+++

>[!NOTE]
>
>各表示名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれています。 リンク先のページには、Adobeで収集された動画広告データの詳細、実装値、ネットワークパラメーター、レポート、重要な検討事項が含まれています。

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------|-----------|-----------|---------------------------------------------------------------------------------------|
| [[!UICONTROL  ビットレート ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#average-bitrate) | `bitrate` | 整数 | × | ビットレート値（kbps）。 |
| [[!UICONTROL  ドロップフレーム ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#dropped-frames) | `droppedFrames` | 整数 | × | 再生中にドロップしたフレームの合計数。 |
| [[!UICONTROL  フレーム/秒 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#frames-per-second) | `framesPerSecond` | 整数 | × | 現在のストリームフレームレート（1 秒あたりのフレーム数）。 |
| [[!UICONTROL  開始時間 ]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#time-to-start-1) | `timeToStart` | 整数 | × | ビデオの読み込みから開始までの時間（秒単位）。 |

{style="table-layout:auto"}
