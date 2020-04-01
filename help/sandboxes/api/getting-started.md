---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Sandbox API開発者ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: b4741cdfd065bbaed7f2feeafe8619191e4b8f6c

---


# Sandbox API開発者ガイド

Adobe Experience Platformのサンドボックスは、独立した開発環境を提供し、実稼働環境に影響を与えることなく、機能のテスト、実験の実行、カスタム設定を行うことができます。

この開発者ガイドでは、Sandbox APIを使用してExperience Platformのサンドボックスを管理する手順を説明し、様々な操作を実行するためのサンプルAPI呼び出しを含んでいます。

## Sandbox APIの使い始めに

IMS組織のサンドボックスを管理するには、Sandbox管理権限が必要です。 アクセス権限のないユーザーは、現在のユーザーのアクティブなサンドボッ [クスのリスト表示にエンドポイントのみを使用できま](./list-active-sandboxes.md)す。 Experience PlatformのSandbox権限を割り当てる方法につ [いて詳しくは、](../../access-control/home.md) アクセス制御の概要を参照してください。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 これには、パス、必須ヘッダー、適切にフォーマットされたリクエストペイロードが含まれます。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、エクスペリエンスプラットフォームのトラブルシューテ [ィングガイドのAPI呼び出し例の読み方に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照してください。

### 必要なヘッダーの値の収集

このガイドでは、プラットフォームAPIを正しく呼び出す [ために](../../tutorials/authentication.md) 、認証チュートリアルを完了している必要があります。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

* 認証：無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

認証ヘッダーに加えて、すべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード（POST、PUTおよびPATCH）を含むすべてのリクエストには、次の追加のヘッダーが必要です。

* コンテンツタイプ：application/json

## 次の手順

これで、必要な資格情報を収集したので、残りの開発者ガイドを読み続けることができます。 各節では、エンドポイントに関する重要な情報を提供し、CRUD操作を実行するためのAPI呼び出しの例を示します。 各呼び出しには、一般的な **API形式**、必要なヘッダーと適切な形式のペイロードを示す **サンプルリクエスト、および正常な呼び** 出しのサンプル応答が含まれます **** 。