---
keywords: エクスペリエンス Platform、home、人気のある話題。ファイル転送プロトコル。ファイル転送プロトコル
solution: Experience Platform
title: フローサービス API を使用した FTP ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して Adobe エクスペリエンスプラットフォームを FTP (ファイル転送プロトコル) サーバーに接続する方法について説明します。
exl-id: a7bef346-b357-49bc-ac54-ac8b42adac50
source-git-commit: 13bd1254dfe89004465174a7532b4f6aaef54c09
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 13%

---

# API を使用した FTP ベース接続の作成 [!DNL Flow Service]

>[!NOTE]
>
>FTP コネクタはベータ版です。 機能とドキュメントは変更される場合があります。[ ](../../../../home.md#terms-and-conditions) ベータ版のコネクタの使用について詳しくは、ソースの概要を参照してください。

ベース接続は、ソースと Adobe エクスペリエンスプラットフォームとの間の認証された接続を表します。

このチュートリアルでは、 [!DNL FTP] API を使用して、ファイル転送プロトコル用の基本的な接続を作成する手順について説明し [[!DNL Flow Service]  ](https://www.adobe.io/experience-platform-apis/references/flow-service/) ます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース ](../../../../home.md) : [!DNL Experience Platform] 多種多様なソースからのデータの ingested を可能にするとともに、サービスを使用した受信データを構造化、ラベル付け、拡張するための機能を提供し [!DNL Platform] ます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の各セクションでは、 [!DNL FTP] API を使用してサーバーに接続するために必要な追加情報を記載して [!DNL Flow Service] います。

### 必要な資格情報の収集

に接続するには [!DNL Flow Service] [!DNL FTP] 、次の接続プロパティの値を指定する必要があります。

| Chap | 説明 |
| ---------- | ----------- |
| `host` | サーバーに関連付けられた名前または IP アドレスを指定し [!DNL FTP] ます。 |
| `username` | サーバーへのアクセスを許可するユーザー名 [!DNL FTP] です。 |
| `password` | サーバーのパスワードを指定 [!DNL FTP] します。 |
| `connectionSpec.id` | コネクション仕様は、ベースおよびソース接続の作成に関連付けられた認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID [!DNL FTP] は、次のとおりです `fb2e94c9-c031-467d-8103-6bd6e0a432f2` 。 |

### プラットフォーム Api の使用

プラットフォーム Api の呼び出しを適切に行う方法については、Platform Api の概要を参照してください [ ](../../../../../landing/api-guide.md) 。

## ベース接続の作成

ベース接続を行うと、ソースとプラットフォームの間の情報が保持されます。ソースの認証の資格情報、接続の現在の状態、および一意の基本接続 ID が含まれています。 ベース接続 ID を使用して、ソース内でファイルを検索してナビゲートし、データの種類とフォーマットに関する情報も含めて、取り込む特定のアイテムを指定することができます。

ベース接続 ID を作成するには、そのエンドポイントに POST 要求を行います。この場合は、 `/connections` [!DNL FTP] 要求パラメーターの一部として認証資格情報を指定します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次の要求によって、次のような基本的な接続が作成され [!DNL FTP] ます。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d  '{
        "name": "FTP connector with password",
        "description": "FTP connector password",
        "auth": {
            "specName": "Basic Authentication for FTP",
            "params": {
                "host": "{HOST}",
                "userName": "{USERNAME}",
                "password": "{PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "fb2e94c9-c031-467d-8103-6bd6e0a432f2",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.host` | FTP サーバーのホスト名。 |
| `auth.params.username` | FTP サーバーに関連付けられたユーザー名です。 |
| `auth.params.password` | FTP サーバーに関連付けられたパスワードを指定します。 |
| `connectionSpec.id` | FTP サーバーの接続条件 ID。 `fb2e94c9-c031-467d-8103-6bd6e0a432f2` |

**応答**

応答が成功した場合は、新しく作成された接続の一意の識別子 () が返され `id` ます。 この ID は、次のチュートリアルで FTP サーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

## 次の手順

このチュートリアルでは、API を使用して FTP 接続を作成 [!DNL Flow Service] し、接続の一意の ID 値を取得しました。 この接続 ID を使用し [ て、フローサービス API を使用してクラウドストレージを探すことができ ](../../explore/cloud-storage.md) ます。
