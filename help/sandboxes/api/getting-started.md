---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Sandbox API開発ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: b4741cdfd065bbaed7f2feeafe8619191e4b8f6c
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 0%

---


# Sandbox API開発ガイド

Adobe Experience Platformのサンドボックスは、実稼働環境に影響を与えることなく、機能のテスト、実験の実行、カスタム設定を行える、独立した開発環境を提供します。

この開発者ガイドでは、Sandbox APIを使用してExperience Platformのサンドボックスを管理する手順を説明し、様々な操作を実行するためのサンプルAPI呼び出しを含めます。

## Sandbox APIの使い始めに

IMS組織のサンドボックスを管理するには、Sandboxの管理権限が必要です。 アクセス権限を持たないユーザーは、エンドポイントを使用して、現在のユーザーのアクティブなサンドボックスの [リストを表示することのみができます](./list-active-sandboxes.md)。 Experience PlatformのSandbox権限を割り当てる方法について詳しくは、 [アクセス制御の概要](../../access-control/home.md) （英語）を参照してください。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される表記について詳しくは、Experience PlatformトラブルシューティングガイドのAPI呼び出し例の読み [方に関する節を参照してください](../../landing/troubleshooting.md#how-do-i-format-an-api-request) 。

### 必要なヘッダーの値の収集

このガイドでは、プラットフォームAPIの呼び出しを正常に行うために、 [認証のチュートリアル](../../tutorials/authentication.md) を完了している必要があります。 次に示すように、認証チュートリアルで、すべてのExperience Platform API呼び出しに必要な各ヘッダーの値を指定します。

* 認証： 無記名 `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

認証ヘッダーに加えて、すべての要求には、操作が実行されるサンドボックスの名前を指定するヘッダーが必要です。

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード（POST、PUT、およびPATCH）を含むすべてのリクエストには、次の追加ヘッダーが必要です。

* Content-Type: application/json

## 次の手順

必要な資格情報を収集したら、残りの開発者ガイドを読み続けることができます。 各節では、エンドポイントに関する重要な情報を提供し、CRUD操作を実行するためのAPI呼び出しの例を示します。 各呼び出しには、一般的な **API形式**、必要なヘッダーと適切にフォーマットされたペイロードを示すサンプル **リクエスト** 、および正常な呼び出しのためのサンプル **応答** が含まれます。