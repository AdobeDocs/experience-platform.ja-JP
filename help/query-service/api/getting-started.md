---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発ガイド
topic: query templates
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '391'
ht-degree: 1%

---


# クエリサービス開発ガイド

この開発者ガイドでは、Adobe Experience PlatformクエリサービスAPIの様々な操作を実行する手順を説明します。

## はじめに

このガイドでは、クエリサービスの使用に関連する様々なAdobe Experience Platformサービスについて、十分な理解を得ている必要があります。

- [クエリサービス](../home.md): データセットをクエリし、結果のクエリをExperience Platformの新しいデータセットとして取得する機能を提供します。
- [Experience Data Model(XDM)System](../../xdm/home.md): Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
- [サンドボックス](../../sandboxes/home.md): Experience Platformは、1つのPlatformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、APIを使用してクエリサービスを正しく使用するために知っておく必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 このドキュメントでサンプルAPI呼び出しに使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照してください。

### 必要なヘッダーの値の収集

Experience PlatformAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、PlatformAPIのすべての呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

- 認証: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 PlatformAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>Experience Platformでのサンドボックスの操作について詳しくは、 [サンドボックスの概要ドキュメントを参照してください](../../sandboxes/home.md)。

## サンプルAPI呼び出し

これで、使用するヘッダーが分かったので、クエリサービスAPIの呼び出しを開始する準備が整いました。 以下のドキュメントでは、クエリサービスAPIを使用して実行できる様々なAPI呼び出しについて説明します。 各サンプル呼び出しには、一般的なAPI形式、必要なヘッダーを表示するサンプルリクエスト、サンプルレスポンスが含まれます。

- [クエリ](queries.md)
- [接続パラメータ](connection-parameters.md)
- [予定クエリ](scheduled-queries.md)
- [スケジュールされたクエリに対して実行](runs-scheduled-queries.md)
- [クエリテンプレート](query-templates.md)

## 次の手順

これで、クエリサービスAPIを使用した呼び出しの実行方法を学んだので、独自の非インタラクティブクエリを作成できます。 クエリの作成方法の詳細については、『 [SQLリファレンスガイド](../sql/overview.md)』を参照してください。