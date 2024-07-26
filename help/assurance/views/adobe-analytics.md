---
title: Assurance のAdobe Analytics表示
description: このガイドでは、Adobe Experience Platform Assurance とAdobe Analyticsの使用方法について説明します。
exl-id: e5cc72b0-d6d6-430b-9321-4835c1f77581
source-git-commit: 515f58175a8ccba03581ce4d7faf23fdfed3571e
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 1%

---

# Assurance のAdobe Analytics ビュー

>[!IMPORTANT]
>
>Analytics イベントビューは、**Analytics Events 2.0 （Beta） プラグイン** に統合されました。  これは、今後、アシュランスから削除される予定です。 Assurance セッションでの Analytics のデバッグには、**Analytics Events 2.0 （Beta）プラグイン** を使用することをお勧めします。

Adobe Experience Platform Assurance とAdobe Analyticsの統合により、SDK イベントに関する豊富なビューが、Adobe Analytics実装のデバッグと検証を行うユーザーに提供されます。 ビューには、[Adobe Experience Platform SDK](https://developer.adobe.com/client-sdks/documentation/adobe-analytics/) からAdobe Analyticsに送信されたライフサイクルイベントとアクション/ステートイベントが表示されるようになりました。 また、このビューには、各レポートスイートの処理ルールを適用した後のイベントの処理方法に関する情報を提供する「応答」の詳細も含まれています。

![](./images/adobe-analytics/overview.png)

## はじめに

続行する前に、次のサービスを利用していることを確認してください。

- [Adobe Experience Platform Data Collection UI](https://experience.adobe.com/#/data-collection/)
- [Adobe Experience Platform アシュランス ](https://experience.adobe.com/assurance)

アプリケーションに Assurance をインストールする方法については、[Assurance 実装ガイド ](../tutorials/implement-assurance.md) を参照してください。

## 後処理ステータス

SDK がAdobe Analyticsでネットワークリクエストを送信すると、Adobe Analyticsが Assurance リクエストの後処理データを取得できたかどうかをステータスが示します。

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

## イベントの詳細表示

Analytics のトラックイベントの場合、詳細ビューには次のような有益な情報が含まれます。

- 起点となる SDK Analytics リクエストイベント。
- リクエストからの OOTB メタおよびコンテキストデータ （レポートスイート ID、SDK 拡張機能バージョン、OOTB コンテキストデータなど）。
- リポジトリ、eVar、prop などのマッピングを含む、Analytics イベントに関する後処理情報。
