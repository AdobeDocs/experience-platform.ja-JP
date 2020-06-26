---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイムの顧客プロファイルAPI開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: 93aae0e394e1ea9b6089d01c585a94871863818e
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---


# リアルタイム顧客プロファイルAPIの概要 {#getting-started}

リアルタイム顧客プロファイルAPIを使用すると、計算済み属性の設定、エンティティへのアクセス、プロファイルデータのエクスポート、不要なデータセットやバッチの削除など、プロファイルリソースに対する基本的なCRUD操作を実行できます。

開発者ガイドを使用するには、プロファイルデータの操作に関連する様々なAdobe Experience Platformサービスについて、十分な理解が必要です。 リアルタイム顧客プロファイルAPIの使用を開始する前に、次のサービスのドキュメントを確認してください。

* [リアルタイム顧客プロファイル](../home.md): 複数のソースからの集計データに基づいて、リアルタイムで統合された顧客プロファイルを提供します。
* [Adobe Experience PlatformIDサービス](../../identity-service/home.md): デバイスとシステム間でIDをブリッジ化することで、顧客と行動をより良く表示できます。
* [Adobe Experience Platformセグメントサービス](../../segmentation/home.md): リアルタイム顧客プロファイルデータからオーディエンスセグメントを作成できます。
* [Experience Data Model(XDM)](../../xdm/home.md): Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
* [サンドボックス](../../sandboxes/home.md): Experience Platformは、1つのPlatformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

以下の節では、プロファイルAPIエンドポイントの呼び出しを正常に行うために知っておく必要がある追加情報について説明します。

## サンプルAPI呼び出しの読み取り

リアルタイム顧客プロファイルAPIドキュメントには、リクエストを正しくフォーマットする方法を示すAPI呼び出しの例が記載されています。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例 [の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照してください。

## 必須ヘッダー

また、APIドキュメントでは、Platformエンドポイントの呼び出しを正常に行うために、 [認証のチュートリアル](../../tutorials/authentication.md) を完了している必要があります。 次に示すように、Experience PlatformAPI呼び出しに必要な各ヘッダーの値を認証チュートリアルで説明します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

Experience Platform内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 PlatformAPIへの要求には、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

Platform内のサンドボックスについて詳しくは、「 [Sandboxの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

リクエスト本文のペイロードを含むすべてのリクエスト（POST、PUT、PATCH呼び出しなど）には、 `Content-Type` ヘッダーが必要です。 各呼び出しに固有の受け入れられた値は、呼び出しパラメーターに提供されます。

## 次の手順

Real-time Customer CustomerプロファイルAPIを使用して呼び出しを開始するには、使用可能なエンドポイントガイドの1つを選択します。