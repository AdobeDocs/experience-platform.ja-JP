---
title: QoE(Quality of Experience) データ詳細収集データタイプ
description: QoE(Quality of Experience) データ詳細データ収集タイプエクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: e1c94635b691c68de6154d19e974bfdf2e250923
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 5%

---

# QoE(Quality of Experience) データ詳細の収集データタイプ

[!UICONTROL QoE データの詳細] コレクションは標準のエクスペリエンスデータモデル (XDM) データタイプで、メディア再生中のエクスペリエンスの質 (QoE) に関する詳細な指標を提供します。 以下を使用します。 [!UICONTROL QoE データの詳細] ビットレート情報、フレームレート、バッファリングイベント、ドロップフレームなどの詳細をキャプチャするためのコレクションデータ型です。 メディアコレクションフィールドは、データをキャプチャして他のAdobe サービスに送信し、さらに処理します。 このデータタイプを使用すると、再生品質を分析でき、ストリーミングのパフォーマンス、ユーザーエクスペリエンス、再生セッション中に発生する可能性のある問題に関するインサイトを得ることができます。

+++「QoE Data Details」データタイプを表示する場合に選択します。
![QoE(Quality of Experience) データ詳細収集データタイプを示す図です。](../images/data-types/qoe-data-details-collection.png)
+++

>[!NOTE]
>
>各ディスプレイ名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれます。 リンクされたページには、Adobe、実装値、ネットワークパラメーター、レポートおよび重要な考慮事項によって収集されるビデオ広告データの詳細が含まれます。

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------|-----------|-----------|---------------------------------------------------------------------------------------|
| [[!UICONTROL ビットレート]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#average-bitrate) | `bitrate` | 整数 | × | ビットレート値 (kbps)。 |
| [[!UICONTROL ドロップフレーム]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#dropped-frames) | `droppedFrames` | 整数 | × | 再生中にドロップしたフレームの総数。 |
| [[!UICONTROL 1 秒あたりのフレーム数]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#frames-per-second) | `framesPerSecond` | 整数 | × | 現在のストリームフレームレート（1 秒あたりのフレーム数）。 |
| [[!UICONTROL 開始時間]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/quality-parameters.html#time-to-start-1) | `timeToStart` | 整数 | × | ビデオの読み込みと開始の間の時間（秒）。 |

{style="table-layout:auto"}
