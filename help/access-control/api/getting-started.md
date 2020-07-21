---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: アクセス制御開発ガイド
topic: developer guide
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 1%

---


# [!DNL Access control] 開発ガイド

[!DNL Access control] の管理 [!DNL Experience Platform] には、 [AdobeAdmin Consoleを使用します](https://adminconsole.adobe.com)。 この機能は、Admin Consoleの製品プロファイルを利用して、ユーザーを権限およびサンドボックスにリンクします。 See the [access control overview](../home.md) for more information.

この開発者ガイドでは、にリクエストをフォーマットする方法について説明し [!DNL Access Control API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/access-control.yaml)、以下の操作について説明します。

- [権限とリソースタイプのリスト名](./permissions-and-resource-types.md)
- [現在のユーザーに対して有効な表示ポリシー](./effective-policies.md)

## はじめに

以下の節では、 [!DNL Schema Registry] APIを正しく呼び出すために知る必要がある追加情報について説明します。

### サンプルAPI呼び出しの読み取り

このガイドは、リクエストをフォーマットする方法を示すAPI呼び出しの例を提供します。 例えば、パス、必須のヘッダー、適切にフォーマットされた要求ペイロードなどです。 API応答で返されるサンプルJSONも提供されます。 サンプルAPI呼び出しのドキュメントで使用される規則について詳しくは、トラブルシューティングガイドのAPI呼び出し例 [を読む方法に関する節](../../landing/troubleshooting.md#how-do-i-format-an-api-request) を参照して [!DNL Experience Platform] ください。

### 必要なヘッダーの値の収集

APIを呼び出すには、まず [!DNL Platform] 認証チュートリアルを完了する必要があり [ます](../../tutorials/authentication.md)。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

- 認証： 無記名 `{ACCESS_TOKEN}`
- x-api-key: `{API_KEY}`
- x-gw-ims-org-id: `{IMS_ORG}`

内のすべてのリソース [!DNL Experience Platform] は、特定の仮想サンドボックスに分離されます。 APIへのすべてのリクエストには、操作が実行されるサンドボックスの名前を指定するヘッダーが必要で [!DNL Platform] す。

- x-sandbox-name: `{SANDBOX_NAME}`

>[!NOTE]
>
>のサンドボックスについて詳し [!DNL Platform]くは、 [Sandboxの概要ドキュメントを参照してください](../../sandboxes/home.md)。

ペイロード(POST、PUT、PATCH)を含むすべてのリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

## 次の手順

必要な資格情報を収集したら、残りの開発者ガイドを読み続けることができます。 各節では、エンドポイントに関する重要な情報を提供し、CRUD操作を実行するためのAPI呼び出しの例を示します。 各呼び出しには、一般的な **API形式**、必要なヘッダーと適切にフォーマットされたペイロードを示すサンプル **リクエスト** 、および正常な呼び出しのためのサンプル **応答** が含まれます。