---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリサービス開発者ガイド
topic: query templates
translation-type: tm+mt
source-git-commit: 91104399e50bce03fed7c9196e6e83fc48a54d1c

---


# クエリサービス開発者ガイド

この開発者ガイドでは、Adobe Experience Platform Platform Service APIで様々な操作を実行する手順をクエリします。

## はじめに

このガイドでは、クエリサービスの使用に関連する様々なAdobe Experience Platformサービスに関する十分な理解が必要です。

- [クエリサービス](../home.md):Experience Platformでデータセットをクエリし、結果のクエリを新しいデータセットとして取得する機能を提供します。
- [Experience Data Model(XDM)System](../../xdm/home.md):エクスペリエンスプラットフォームが顧客エクスペリエンスデータを整理するための標準化されたフレームワーク。
- [サンドボックス](../../sandboxes/home.md):Experience Platformは、デジタルエクスペリエンスアプリケーションの開発と発展を支援するために、単一のプラットフォームインスタンスを別々の仮想環境に分割する仮想サンドボックスを提供します。

以下の節では、APIを使用してクエリサービスを正しく使用するために知っておく必要がある追加情報を示します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 このドキュメントでサンプルAPI呼び出しに使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し [例を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照してください。

### 必要なヘッダーの値の収集

Experience Platform APIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルでは、すべてのプラットフォームAPI呼び出しで必要な各ヘッダーの値を指定します。

- 認証: `Bearer {ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されます。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] エクスペリエンスプラットフォームでのサンドボックスの操作について詳しくは、サンドボックスの概要ドキュメ [ントを参照してくださ](../../sandboxes/home.md)い。

## サンプルAPI呼び出し

これで、使用するヘッダーを理解できたので、クエリサービスAPIの呼び出しを開始できます。 以下のドキュメントは、クエリサービスAPIを使用して行える様々なAPI呼び出しの手順を示します。 各サンプル呼び出しには、一般的なAPI形式、必要なヘッダーを示すサンプルリクエスト、およびサンプルの応答が含まれます。

- [クエリ](queries.md)
- [接続パラメータ](connection-parameters.md)
- [予定クエリ](scheduled-queries.md)
- [スケジュールされたクエリ](runs-scheduled-queries.md)
- [クエリテンプレート](query-templates.md)

## 次の手順

これで、クエリサービスAPIを使用した呼び出しの作成方法を学習し、独自の非インタラクティブクエリを作成できます。 クエリの作成方法の詳細については、 [SQLリファレンスガイドを参照してください](../sql/overview.md)。