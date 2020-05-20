---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: プライバシーサービス開発ガイド
description: RESTful APIを使用して、Adobe Experience Cloudアプリケーション全体でデータサブジェクトの個人データを管理します
topic: developer guide
translation-type: tm+mt
source-git-commit: a1161630c8edae107b784f32ee20af225f9f8c46
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 2%

---


# プライバシーサービス開発ガイド

Adobe Experience Platformプライバシーサービスは、RESTful APIとユーザーインターフェイスを提供します。このインターフェイスを使用すると、Adobe Experience Cloudアプリケーション全体でデータサブジェクト（顧客）の個人データを管理（アクセスおよび削除）できます。 また、プライバシーサービスは、Experience Cloudアプリケーションに関連するジョブのステータスと結果にアクセスできる、中央の監査とログのメカニズムも提供します。

このガイドは、プライバシーサービスAPIの使用方法をカバーしています。 UIの使用方法について詳しくは、「 [プライバシーサービスのUIの概要](../ui/overview.md)」を参照してください。 プライバシーサービスAPIの使用可能なすべてのエンドポイントの包括的なリストについては、 [APIリファレンスを参照してください](https://www.adobe.io/apis/experiencecloud/gdpr/api-reference.html)。

## はじめに

このガイドでは、次のエクスペリエンスプラットフォーム機能を理解している必要があります。

* [プライバシーサービス](../home.md): RESTful APIとユーザーインターフェイスを提供します。このインターフェイスを使用すると、Adobe Experience Cloudアプリケーション全体で、データサブジェクト（顧客）からのアクセス要求と削除要求を管理できます。

以降の節では、プライバシーサービスAPIの呼び出しを正しく行うために知っておく必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このチュートリアルでは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

* Content-Type: application/json

## 次の手順

使用するヘッダーがわかったら、Privacy Service APIの呼び出しを開始する準備が整いました。 プライバシー [ジョブに関するドキュメントでは](privacy-jobs.md) 、Privacy Service APIを使用して実行できる様々なAPI呼び出しについて詳しく説明します。 各サンプル呼び出しには、一般的なAPI形式、必要なヘッダーを表示するサンプルリクエスト、サンプルレスポンスが含まれます。
