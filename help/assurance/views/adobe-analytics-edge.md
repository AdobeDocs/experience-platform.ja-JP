---
title: Assurance の Analytics イベント 2.0
description: このガイドでは、Adobe Experience Platform Assurance でAdobe Analyticsと Analytics Edgeのビューを使用する方法について説明します。
badgeBeta: label="ベータ版" type="Informative"
exl-id: faaa2c1d-3471-4d86-9a25-03265b996e31
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '742'
ht-degree: 0%

---

# Assurance の Analytics イベント 2.0

Analytics Events 2.0 は、Adobe Analytics実装をデバッグおよび検証するユーザーに SDK イベントをより詳細に表示します。 ビューには、[Adobe Experience Platform Mobile SDK からAdobe Analyticsに送信されたイベントと ](https://developer.adobe.com/client-sdks/solution/adobe-analytics/)[Adobe Experience Platform Edge Network SDK](https://developer.adobe.com/client-sdks/edge/edge-network/) が表示されます。 また、このビューには詳細パネルも用意されており、イベントがデバイスから離脱した後にクライアント SDK とアップストリームサービスによってどのように処理されたかに関するコンテキストを提供します。

## はじめに

このビューを使用するには、次の手順を実行します。

1. [Adobe Experience Platform Assurance を設定します ](../tutorials/implement-assurance.md)。
2. [Assurance セッションを作成して接続します ](../tutorials/using-assurance.md)。
3. 左側のナビゲーション **ホーム** ビューメニューの Assurance UI で、「**Analytics Events 2.0 （Beta）**」を選択します。 このオプションが表示されない場合は、ウィンドウの左下にある **設定** を選択し、**Analytics Events 2.0 （Beta）** を追加して、**保存** を選択します。

## Analytics イベントビュー

**Adobe Analytics** モバイル拡張機能を使用している場合は、Analytics イベントビューを使用します。 このビューを使用すると、アクションの追跡、状態の追跡、ライフサイクルイベントなど、接続したクライアントから送信された Analytics イベントを簡単に表示できます。 テーブル内の Analytics イベントの 1 つを選択すると、イベントがどのように処理されたかの詳細が右側のパネルに表示されます。

![Analytics イベントビューの様々なコンポーネントを示す画像 ](./images/adobe-analytics-edge/analytics-events.png)

### Post処理ステータス

SDK がAdobe Analyticsでネットワークリクエストを送信すると、Adobe Analyticsが Assurance リクエストの後処理データを取得できたかどうかをステータスが示します。 リクエストがトリガーされた後、後処理ステータスが操作中は、Analytics イベントビューをアクティブにしておく必要があります。

後処理情報を取得するには、ログインしたユーザーが対応するレポートスイートにアクセスできる必要があります。

| ステータス | 説明 |
| :----- | :---------- |
| `Queued` | ネットワークリクエストは後処理情報を取得しています。 |
| `Processed` | ネットワークリクエストが成功し、後処理情報が受信されます。 |
| `Delayed` | 後処理情報を取得するための再試行リクエストの最大数を超えました。 |
| `Error` | エラーが原因で、ネットワーク要求が失敗しました。 エラーの詳細は、イベントの詳細表示に表示されます。 |
| `Unauthorized` | ユーザーには、Adobe Analytics レポートスイートへのアクセス権がありません。 |
| `Unavailable` | Adobe Analytics リクエストに対応する `AnalyticsResponse` イベントがありません。 |
| `No Debug Flag` | 現在のAdobe Analyticsまたは Assurance SDK バージョンでは、Analytics のデバッグ機能がサポートされていない可能性があります。 詳しくは、[ トラブルシューティングガイド ](../troubleshooting.md) を参照してください。 |
| `Expired` | `AnalyticsTrack` または `LifecycleStart` イベントが 24 時間より古い。 |

### イベントの詳細表示

Analytics のトラックイベントの場合、詳細ビューには次の部分が表示されます。

- 起点となる SDK Analytics リクエストイベント。
- リクエストからのメタデータとコンテキストデータ（レポートスイート ID、SDK 拡張機能バージョン、コンテキストデータなど）。
- リポジトリ、eVar および prop のマッピングを含む、Analytics イベントに関するPostで処理される情報。

### Analytics ビューの検証

検証ビューを使用すると、Analytics に関連する検証スクリプトの結果を簡単に表示できます。 バリデーターによって表示されるエラーには、修正する場所へのリンクが含まれている場合や、エラー状態のイベントが表示されている場合があります。

![Analytics ビューで「バリデーター」タブを示す画像 ](./images/adobe-analytics-edge/analytics-validation-view.png)

## Analytics Edge ビュー

**Analytics または** Edge Edge **モバイル拡張機能を使用している場合は、Edge Network Bridge** ビューを使用します。 このビューを有効にするには、右上の「Analytics Edge （Beta）」切り替えスイッチを選択して、現在のセッションでEdge Network を介して送信された Analytics イベントを表示します。 これには、ライフサイクル拡張機能、Edge リクエスト、Edge Bridge イベント（トラックアクションとトラックステートに基づく）によって発生したすべてのイベントが含まれます。

![Analytics ビューと Analytics Edge ビューの切り替えに使用される切り替えを示す画像 ](./images/adobe-analytics-edge/analytics-view-toggle.png)

Analytics Edge ビューには、クライアントがディスパッチした Analytics 関連のEdge リクエストとライフサイクルメソッドに関する情報が表示されます。 リストでイベントを選択すると、右側のパネルにクライアント SDK によって処理されたイベントと、デバイスを離れた後にアップストリームサービスによって処理されたイベントが表示されるので、呼び出しによって発生したイベントのチェーンを簡単に確認できます。

![Analytics Edge ビューの様々なコンポーネントを示す画像 ](./images/adobe-analytics-edge/edge-analytics-events.png)

### Analytics Edgeの検証

Analytics Edgeの検証ビューを使用すると、Analytics Edgeに関連する検証スクリプトの結果を簡単に表示できます。 バリデーターによって表示されるエラーには、修正する場所へのリンクが含まれている場合や、エラー状態のイベントが表示されている場合があります。

![Analytics Edge ビューの「バリデーター」タブを示す画像 ](./images/adobe-analytics-edge/edge-analytics-validation-view.png)
