---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: アクセス制御開発ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: df85ea955b7a308e6be1e2149fcdfb4224facc53
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 1%

---


# アクセス制御開発ガイド

Access control for Experience Platform is administered through the [Adobe Admin Console](https://adminconsole.adobe.com). この機能は、管理コンソールの製品プロファイルを利用して、ユーザーを権限およびサンドボックスにリンクします。 See the [access control overview](../home.md) for more information.

この開発者ガイドは、 [アクセス制御APIに対するリクエストの書式を設定する方法に関する情報を提供し](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/access-control.yaml)、以下の操作についてカバーしています。

- [権限とリソースタイプのリスト名](./permissions-and-resource-types.md)
- [現在のユーザーに対して有効な表示ポリシー](./effective-policies.md)

## はじめに

以下の節では、スキーマレジストリAPIを正しく呼び出すために知っておく必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

プラットフォームAPIを呼び出すには、まず [認証チュートリアルを完了する必要があります](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

エクスペリエンスプラットフォームのすべてのリソースは、特定の仮想サンドボックスに分離されています。 プラットフォームAPIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE] プラットフォームのサンドボックスについて詳しくは、「 [サンドボックスの概要に関するドキュメント](../../sandboxes/home.md)」を参照してください。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

## 次の手順

必要な資格情報を収集したら、残りの開発者ガイドを読み続けることができます。 各節では、エンドポイントに関する重要な情報を提供し、CRUD操作を実行するためのAPI呼び出しの例を示します。 各呼び出しには、一般的な **API形式**、必要なヘッダーと適切にフォーマットされたペイロードを示すサンプル **リクエスト** 、および正常な呼び出しのためのサンプル **応答** が含まれます。