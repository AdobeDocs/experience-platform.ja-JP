---
keywords: Experience Platform；ホーム；人気の高いトピック；oracle;eloqua;oracleeloqua
solution: Experience Platform
title: フローサービス API を使用したOracleEloqua ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience PlatformをOracleEloqua に接続する方法を説明します。
exl-id: 866e408f-6e0b-4e81-9ad8-9d74c485c89a
source-git-commit: 93061c84639ca1fdd3f7abb1bbd050eb6eebbdd6
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 58%

---

# の作成 [!DNL Oracle Eloqua] を使用したベース接続 [!DNL Flow Service] API

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Oracle Eloqua] のベース接続を作成する手順を説明します。

## はじめに

このガイドは、Adobe Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Platform を使用すると、様々なソースからデータを取り込みながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)[!DNL Platform]： には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用して [!DNL Oracle Eloqua] に正常に接続するために必要な追加情報を示しています。

### 必要な認証情報の収集

[!DNL Flow Service] を [!DNL Oracle Eloqua] に接続するには、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| --- | --- |
| `endpoint` | のエンドポイント [!DNL Oracle Eloqua]. |
| `username` | ユーザー名 [!DNL Oracle Eloqua] アカウント ユーザー名は、 `siteName + \\ + username`で、 `siteName` は、ログインに使用した会社名です [!DNL Oracle Eloqua] および `username` はユーザー名です。 例えば、ログインユーザー名は次のようになります。 `adobe\\emily`. |
| `password` | 次に対応するパスワード： [!DNL Oracle Eloqua] ユーザー名。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。の接続仕様 ID の値 [!DNL Oracle Eloqua] ソースは次のように固定されます。 `35d6c4d8-c9a9-11eb-b8bc-0242ac130003`. |

の認証資格情報の詳細 [!DNL Oracle Eloqua]を参照し、 [[!DNL Oracle Eloqua] 認証に関するガイド](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/Authentication_Basic.html).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Oracle Eloqua] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Oracle Eloqua] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `name` | [!DNL Oracle Eloqua] ベース接続名。わかりやすい名前を付けることをお勧めします。この値を使用して、ベース接続を検索できます。 |
| `description` | （オプション）ベース接続に関する補足情報を提供するために含めることができるプロパティ。 |
| `auth.specName` | 接続に使用する認証タイプ。 |
| `auth.params.endpoint` | のエンドポイント [!DNL Oracle Eloqua] サーバー。 |
| `auth.params.username` | の [!DNL Oracle Eloqua] アカウント |
| `auth.params.password` | [!DNL Oracle Eloqua] アカウントに対応するパスワード。 |
| `connectionSpec.id` | の接続仕様 ID の値 [!DNL Oracle Eloqua] ソースは次のように固定されます。 `35d6c4d8-c9a9-11eb-b8bc-0242ac130003`. |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Oracle Eloqua] を使用したベース接続 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [を使用してデータテーブルの構造と内容を調べる [!DNL Flow Service] API](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/marketing-automation.md)
