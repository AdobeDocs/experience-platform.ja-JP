---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: プライバシーサービス開発ガイド
description: RESTful APIを使用して、Adobe Experience Cloudアプリケーション全体でデータサブジェクトの個人データを管理します
topic: developer guide
translation-type: tm+mt
source-git-commit: a1161630c8edae107b784f32ee20af225f9f8c46

---


# プライバシーサービス開発ガイド

Adobe Experience Platform Privacy Serviceは、Adobe Experience Cloudアプリケーション全体でデータの件名（顧客）の個人データを管理（アクセスおよび削除）できるRESTful APIとユーザーインターフェイスを提供します。 また、プライバシーサービスは、Experience Cloudアプリケーションに関連するジョブのステータスと結果にアクセスできる、中央の監査とログのメカニズムも提供します。

このガイドでは、プライバシーサービスAPIの使用方法を説明します。 UIの使用方法について詳しくは、プライバシーサービスのUIの概 [要を参照してください](../ui/overview.md)。 プライバシーサービスAPIで使用可能なすべてのエンドポイントの包括的なリストについては、 [APIリファレンスを参照してくださ](https://www.adobe.io/apis/experiencecloud/gdpr/api-reference.html)い。

## はじめに

このガイドでは、次のエクスペリエンスプラットフォームの機能を理解する必要があります。

* [プライバシーサービス](../home.md):Adobe Experience Cloudアプリケーション全体でデータサブジェクト（顧客）からのアクセスおよび削除のリクエストを管理できるRESTful APIおよびユーザーインターフェイスを提供します。

次の節では、プライバシーサービスAPIを正しく呼び出すために知っておく必要がある追加情報を示します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を示します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md) （英語のみ）を参照してください。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず認証チュートリアルを完了する必要 [があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次のヘッダーが追加で必要です。

* コンテンツタイプ：application/json

## 次の手順

これで、使用するヘッダーがわかりましたので、プライバシーサービスAPIを呼び出す準備が整いました。 プライバシー [ドキュメント](privacy-jobs.md) （Privacy Service APIを使用して実行できる様々なAPI呼び出し）に関する詳細な説明を行います。 各サンプル呼び出しには、一般的なAPI形式、必要なヘッダーを示すサンプルリクエスト、およびサンプルの応答が含まれます。
