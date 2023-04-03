---
title: ストリーミングメディア用 Adobe Analytics Assurance での表示
description: このガイドでは、ストリーミングメディア用 Adobe AnalyticsをAdobe Experience Platform Assurance と共に使用する方法を説明します。
source-git-commit: 07dc01c11c79ac2dad05d89309cabb5715c0b63c
workflow-type: tm+mt
source-wordcount: '412'
ht-degree: 2%

---


# アシュランスのストリーミングメディア用 Adobe Analytics表示

ストリーミングメディア用 Adobe AnalyticsとAdobe Experience Platform Assurance の統合により、モバイルアプリで Media Analytics の実装を検証できるようになりました。 Media Analytics ビューには、次のようなメディアセッションで追跡されたものが表示されます。

- すべてのコンテンツコア、標準メタデータ、カスタムメタデータのプロパティと、セッション終了およびセッション完了イベントを含むセッション開始イベント。
- すべての広告プロパティが添付された広告ブレーク開始イベントおよび広告開始イベント、および両方のスキップイベントと完了イベント。
- チャプター開始には、すべてのプロパティが添付され、チャプタースキップイベントとチャプター完了イベントも追加されます。
- すべての再生変更イベント（再生、一時停止、バッファー、エラー、ビットレート変更）。
- すべてのプレーヤーステート変更トラッキングイベント（開始、終了）。

Analytics でデータが処理されると、後処理済みのステータスとデータ（メディア視聴時間、合計一時停止時間など）もイベントの詳細表示で使用できます。

## はじめに

続行する前に、次のサービスを使用していることを確認してください。

- この [Adobe Experience Platform Data Collection UI](https://experience.adobe.com/#/data-collection/)
- [Adobe Experience Platform Assurance](https://experience.adobe.com/assurance)

アプリケーションに Assurance をインストールする方法については、 [アシュランスの実装ガイド](../tutorials/implement-assurance.md).

## ストリーミングメディア用 Adobe Analyticsでアシュランスを使用

接続してAdobe Analytics用にアプリを設定したら、ストリーミングメディア分析用にアプリを設定する準備が整いました。 左側のパネルの下部で、を選択します。 **[!UICONTROL 設定]** Media Analytics イベントビューを追加するには、以下を実行します。 **保存** それは。

![設定](./images/adobe-analytics-streaming-media/configure.png)

追加したら、 **[!UICONTROL Media Analytics イベント]** 表示 **[!UICONTROL Adobe Analytics]** 」セクションを使用して、セッショントラッキングを検証します。

![選択](./images/adobe-analytics-streaming-media/select.png)

内 **[!UICONTROL Media Analytics イベント]** を表示する場合は、セッション ID(VSID) で検索およびフィルタリングして、特定のメディアセッションを表示できます。 追加のイベントの詳細を表示するには、特定のイベントを選択します。

![メディアイベント](./images/adobe-analytics-streaming-media/media-events.png)

API 呼び出しをより簡潔に表示するには、 **[!UICONTROL 再生ヘッドの更新イベントを非表示にする]** フィルター。

![再生ヘッドを非表示](./images/adobe-analytics-streaming-media/hide-playhead.png)

>[!INFO]
>
>後処理された Media Analytics データを表示するには、次の SDK バージョンが必要です。Android Media 2.1.2 およびiOS AEPMedia 3.0.1（またはそれ以降）

後処理されたデータを表示するには、セッション開始イベントを見つけ、「ステータス」列でセッションが完了したことを検証します。 完了した場合は、イベントをクリックして、イベントの詳細ビューでメディアセッションの概要を表示します。 詳しくは、下にスクロールして後処理した詳細を確認してください。

![後処理済みのビュー](./images/adobe-analytics-streaming-media/post-processed-view.png)
