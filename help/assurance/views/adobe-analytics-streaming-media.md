---
title: Assurance のストリーミングメディア用 Adobe Analytics表示
description: このガイドでは、Adobe Experience Platform Assurance とストリーミングメディア用 Adobe Analyticsの使用方法について説明します。
exl-id: 9a9c2c64-e9ed-4d58-b936-d802f1c3b7d3
source-git-commit: 05a7b73da610a30119b4719ae6b6d85f93cdc2ae
workflow-type: tm+mt
source-wordcount: '412'
ht-degree: 0%

---

# Assurance のストリーミングメディア用 Adobe Analytics ビュー

ストリーミングメディア用 Adobe AnalyticsとAdobe Experience Platform Assurance の統合により、モバイルアプリで Media Analytics 実装を検証できるようになりました。 Media Analytics ビューには、メディアセッションで追跡された内容が次のように表示されます。

- すべてのコンテンツコア、標準メタデータ、カスタムメタデータプロパティおよびセッション終了イベントとセッション完了イベントを含むセッション開始イベント。
- すべての広告プロパティが添付された広告ブレーク開始イベントと広告開始イベント、および両方のスキップイベントと完了イベント。
- すべてのプロパティが添付されたチャプター開始イベント、およびチャプタースキップ イベントとチャプター完了イベント。
- すべての再生変更イベント（再生、一時停止、バッファー、エラー、ビットレート変更）。
- すべてのプレーヤーステート変更トラッキングイベント （開始、終了）。

Analytics でデータが処理されると、処理後のステータスとデータ（メディア視聴時間や合計一時停止時間など）をイベントの詳細ビューでも使用できるようになります。

## はじめに

続行する前に、次のサービスを利用していることを確認してください。

- [Adobe Experience Platform Data Collection UI](https://experience.adobe.com/#/data-collection/)
- [Adobe Experience Platform アシュランス ](https://experience.adobe.com/assurance)

アプリケーションに Assurance をインストールする方法については、[Assurance 実装ガイド ](../tutorials/implement-assurance.md) を参照してください。

## ストリーミングメディア用 Adobe Analyticsでの Assurance の使用

接続してアプリを Streaming Media 用に設定したら、Adobe Analytics用に設定する準備が整います。 左側のパネルの下部にある **[!UICONTROL 設定]** を選択して、Media Analytics イベント表示を追加し **保存** します。

![設定](./images/adobe-analytics-streaming-media/configure.png)

追加したら、「**[!UICONTROL Adobe Analytics**[!UICONTROL 」セクションの ]**Media Analytics イベント]** ビューを選択して、セッショントラッキングを検証します。

![ 選択 ](./images/adobe-analytics-streaming-media/select.png)

**[!UICONTROL Media Analytics イベント]** ビューでは、セッション ID （VSID）で検索およびフィルタリングして、特定のメディアセッションを表示できます。 追加のイベントの詳細を表示するには、特定のイベントを選択します。

![ メディアイベント ](./images/adobe-analytics-streaming-media/media-events.png)

API 呼び出しをより簡単に確認するには、**[!UICONTROL 再生ヘッド更新イベントを非表示にする]** フィルターを選択して再生ヘッド更新イベントを非表示にすることもできます。

![ 再生ヘッドを非表示 ](./images/adobe-analytics-streaming-media/hide-playhead.png)

>[!INFO]
>
>処理後の Media Analytics データを表示するには、SDK バージョン（Android Media 2.1.2 およびiOS AEPMedia 3.0.1 以降）を使用する必要があります

後処理されたデータを表示するには、セッション開始イベントを見つけ、セッションが完了したことをステータス列で検証します。 完了したら、イベントをクリックして、イベントの詳細表示でメディアセッションの概要を表示します。 詳細については、下にスクロールして、後処理の詳細を見つけます。

![Postで処理されたビュー ](./images/adobe-analytics-streaming-media/post-processed-view.png)
