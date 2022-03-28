---
keywords: Experience Platform；ホーム；人気の高いトピック；oracle;eloqua;oracleeloqua
solution: Experience Platform
title: フローサービス API を使用したOracleEloqua ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience PlatformをOracleEloqua に接続する方法を説明します。
source-git-commit: 423f0efc3c0d9fb38cb8d1b16b885e1eddc23e72
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 5%

---

# の作成 [!DNL Oracle Eloqua] を使用したベース接続 [!DNL Flow Service] API

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、のベース接続を作成する手順を説明します。 [!DNL Oracle Eloqua] の使用 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、 Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md):Platform を使用すると、様々なソースからデータを取り込みながら、 [!DNL Platform] サービス。
* [サンドボックス](../../../../../sandboxes/home.md):Platform は、単一をパーティション化する仮想サンドボックスを提供します [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立てます。

次の節では、に正常に接続するために必要な追加情報を示します。 [!DNL Oracle Eloqua] の使用 [!DNL Flow Service] API

### 必要な資格情報の収集

次のために [!DNL Flow Service] ～とつながる [!DNL Oracle Eloqua]を使用する場合、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `endpoint` | のエンドポイント [!DNL Oracle Eloqua]. |
| `username` | ユーザー名 [!DNL Oracle Eloqua] アカウント ユーザー名は、 `siteName + \\ + username`で、 `siteName` は、ログインに使用した会社名です [!DNL Oracle Eloqua] および `username` はユーザー名です。 例えば、ログインユーザー名は次のようになります。 `adobe\\emily`. |
| `password` | 次に対応するパスワード： [!DNL Oracle Eloqua] ユーザー名。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 の接続仕様 ID の値 [!DNL Oracle Eloqua] ソースは次のように固定されます。 `35d6c4d8-c9a9-11eb-b8bc-0242ac130003`. |

の認証資格情報の詳細 [!DNL Oracle Eloqua]を参照し、 [[!DNL Oracle Eloqua] 認証に関するガイド](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/Authentication_Basic.html).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、 [Platform API の概要](../../../../../landing/api-guide.md).

## ベース接続を作成する

ベース接続では、ソースと Platform の間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID など）が保持されます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を識別できます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Oracle Eloqua] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL Oracle Eloqua]:

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json'
  -d '{
      "name": "Oracle Eloqua Base Connection",
      "description": "Base Connection for Oracle Eloqua",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "endpoint": "{ENDPOINT}",
              "username": "{USERNAME}",
              "password": "{PASSWORD}"
          }
      },
      "connectionSpec": {
          "id": "35d6c4d8-c9a9-11eb-b8bc-0242ac130003",
          "version": "1.0"
      }
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `name` | お客様の [!DNL Oracle Eloqua] ベース接続。 わかりやすい名前を付けることをお勧めします。この値を使用して、ベース接続を検索できます。 |
| `description` | （オプション）ベース接続に関する補足情報を提供するために含めることができるプロパティ。 |
| `auth.specName` | 接続に使用する認証タイプ。 |
| `auth.params.endpoint` | のエンドポイント [!DNL Oracle Eloqua] サーバー。 |
| `auth.params.username` | の [!DNL Oracle Eloqua] アカウント |
| `auth.params.password` | ユーザーの [!DNL Oracle Eloqua] アカウント |
| `connectionSpec.id` | の接続仕様 ID の値 [!DNL Oracle Eloqua] ソースは次のように固定されます。 `35d6c4d8-c9a9-11eb-b8bc-0242ac130003`. |

**応答**

正常な応答は、新しく作成されたベース接続の詳細 ( 一意の識別子 (`id`) をクリックします。 この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Oracle Eloqua] を使用したベース接続 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [を使用したマーケティング自動化ソースの構造とコンテンツの調査 [!DNL Flow Service] API](../../explore/marketing-automation.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/marketing-automation.md)


