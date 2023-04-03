---
title: Adobe Analytics Assurance での表示
description: このガイドでは、Adobe AnalyticsをAdobe Experience Platform Assurance と共に使用する方法を説明します。
source-git-commit: 07dc01c11c79ac2dad05d89309cabb5715c0b63c
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 3%

---


# アシュランスのAdobe Analytics表示

Adobe AnalyticsとのAdobe Experience Platform Assurance 統合により、Adobe Analytics実装のデバッグおよび検証に役立つ SDK イベントをより豊富に表示できます。 ビューに、 [Adobe Experience Platform SDK](https://developer.adobe.com/client-sdks/documentation/adobe-analytics/). また、このビューには、各レポートスイートの処理ルールの適用後にイベントがどのように処理されたかに関する情報を提供する「応答」の詳細も含まれています。

![](./images/adobe-analytics/overview.png)

## はじめに

続行する前に、次のサービスを使用していることを確認してください。

- この [Adobe Experience Platform Data Collection UI](https://experience.adobe.com/#/data-collection/)
- [Adobe Experience Platform Assurance](https://experience.adobe.com/assurance)

アプリケーションに Assurance をインストールする方法については、 [アシュランスの実装ガイド](../tutorials/implement-assurance.md).

## 後処理ステータス

SDK がAdobe Analyticsでネットワークリクエストをおこなうと、アシュランスがAdobe Analyticsリクエストの後処理情報を取得できたかどうかをステータスが示します。

後処理情報を取得するには、ログインユーザーが対応するレポートスイートにアクセスできる必要があります。

| ステータス | 説明 |
| :----- | :---------- |
| `Queued` | ネットワークリクエストが後処理情報を取得しています。 |
| `Processed` | ネットワークリクエストが成功し、後処理情報が受信されました。 |
| `Delayed` | 後処理情報を取得するためのリクエストの再試行の最大数を超えました。 |
| `Error` | エラーが発生したため、ネットワークリクエストが失敗しました。 エラーに関する詳細は、イベントの詳細ビューに表示されます。 |
| `Unauthorized` | ユーザーはAdobe Analyticsレポートスイートにアクセスできません。 |
| `Unavailable` | Adobe Analyticsリクエストには、対応する `AnalyticsResponse` イベント。 |
| `No Debug Flag` | 現在のAdobe Analyticsまたは Assurance SDK バージョンは、Analytics デバッグ機能をサポートしていない可能性があります。 詳しくは、 [トラブルシューティングガイド](../troubleshooting.md). |
| `Expired` | この `AnalyticsTrack` または `LifecycleStart` イベントが 24 時間より古い。 |

## イベントの詳細ビュー

Analytics 追跡イベントの場合、詳細ビューには次の貴重な部分が含まれます。

- 元の SDK Analytics リクエストイベントです。
- レポートスイート ID、SDK 拡張バージョン、OOTB コンテキストデータなど、リクエストからの OOTB メタデータおよびコンテキストデータ。
- Analytics イベントに関する後処理された情報です。このイベントには、Revar、Evar、Prop などのマッピングが含まれます。
