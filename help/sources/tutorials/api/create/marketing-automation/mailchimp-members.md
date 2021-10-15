---
keywords: Experience Platform；ホーム；人気のあるトピック；ソース；コネクタ；ソースコネクタ；ソースコネクタ；ソース sdk;SDK;SDK
solution: Experience Platform
title: フローサービス API を使用した MailChimp メンバーのデータフローの作成
topic-legacy: tutorial
description: フローサービス API を使用してAdobe Experience Platformを MailChimp メンバーに接続する方法を説明します。
source-git-commit: c8d94af6185785a0e4bfce9889c04405ed223b1f
workflow-type: tm+mt
source-wordcount: '2500'
ht-degree: 7%

---

# フローサービス API を使用した [!DNL MailChimp Members] のデータフローの作成

次のチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL MailChimp Members] データを Platform に取り込むためのソース接続とデータフローを作成する手順を説明します。

## 前提条件

OAuth 2 更新コードを使用して [!DNL MailChimp] をAdobe Experience Platformに接続する前に、まず [!DNL MailChimp.] のアクセストークンを取得する必要があります。アクセストークンの検索方法について詳しくは、[[!DNL MailChimp] OAuth 2 のガイド ](https://mailchimp.com/developer/marketing/guides/access-user-data-oauth-2/) を参照してください。

## ベース接続を作成する {#base-connection}

[!DNL MailChimp] 認証資格情報を取得したら、データフローの作成プロセスを開始して、[!DNL MailChimp Members] データを Platform に取り込むことができます。 データフローを作成する最初の手順は、ベース接続を作成することです。

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

[!DNL MailChimp] は、基本認証と OAuth 2 更新コードの両方をサポートしています。次の例で、いずれかの認証タイプを使用して認証する方法に関するガイダンスを参照してください。

### 基本認証を使用して [!DNL MailChimp] ベース接続を作成する

基本的なPOSTを使用して [!DNL MailChimp] ベース接続を作成するには、`host`、`authorizationTestUrl`、`username` および `password` の資格情報を指定しながら、[!DNL Flow Service] API の `/connections` エンドポイントに対して認証リクエストを実行します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL MailChimp] のベース接続を作成します。

```shell
curl -X POST \
'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
      "name": "MailChimp base connection with basic authentication",
      "description": "MailChimp Members base connection with basic authentication",
      "connectionSpec": {
          "id": "2e8580db-6489-4726-96de-e33f5f60295f",
          "version": "1.0"
      },
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "host": "{HOST}",
              "authorizationTestUrl": "https://login.mailchimp.com/oauth2/metadata",
              "username": "{USERNAME}",
              "password": "{PASSWORD}"
          }
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ベース接続の名前。 ベース接続の名前がわかりやすいことを確認します。これを使用して、ベース接続の情報を検索できます。 |
| `description` | （オプション）ベース接続に関する詳細情報を提供するために含めることができるプロパティ。 |
| `connectionSpec.id` | ソースの接続仕様 ID。 この ID は、ソースが登録され、[!DNL Flow Service] API を通じて承認された後に取得できます。 |
| `auth.specName` | ソースを Platform に接続するために使用する認証タイプ。 |
| `auth.params.host` | [!DNL MailChimp] API への接続に使用するルート URL。 ルート URL の形式は `https://{DC}.api.mailchimp.com` です。`{DC}` は、お使いのアカウントに対応するデータセンターを表します。 |
| `auth.params.authorizationTestUrl` | （オプション）認証テスト URL は、ベース接続の作成時に資格情報を検証するために使用します。 指定しない場合、代わりに、ソース接続の作成手順で資格情報が自動的にチェックされます。 |
| `auth.params.username` | [!DNL MailChimp] アカウントに対応するユーザー名。 これは基本認証に必要です。 |
| `auth.params.password` | [!DNL MailChimp] アカウントに対応するパスワード。 これは基本認証に必要です。 |

**応答**

正常な応答は、新しく作成されたベース接続を返します。この中には一意の接続識別子 (`id`) も含まれます。 この ID は、次の手順でソースのファイル構造とコンテンツを調べるために必要です。

```json
{
    "id": "4cea039f-f1cc-4fa5-9136-db8dd4c7fbfa",
    "etag": "\"4000cff7-0000-0200-0000-6154bad60000\""
}
```

### OAuth 2 更新コードを使用して [!DNL MailChimp] ベース接続を作成する

OAuth 2 更新コードを使用して [!DNL MailChimp] ベースPOSTを作成するには、`host`、`authorizationTestUrl` および `accessToken` の資格情報を指定しながら、`/connections` エンドポイントに接続リクエストを実行します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL MailChimp] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
      "name": "MailChimp base connection with OAuth 2 refresh code",
      "description": "MailChimp Members base connection with OAuth 2 refresh code",
      "connectionSpec": {
          "id": "2e8580db-6489-4726-96de-e33f5f60295f",
          "version": "1.0"
      },
      "auth": {
          "specName": "oAuth2RefreshCode",
          "params": {
              "host": "{HOST}",
              "authorizationTestUrl": "https://login.mailchimp.com/oauth2/metadata",
              "accessToken": "{ACCESS_TOKEN}"
          }
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ベース接続の名前。 ベース接続の名前がわかりやすいことを確認します。これを使用して、ベース接続の情報を検索できます。 |
| `description` | （オプション）ベース接続に関する詳細情報を提供するために含めることができるプロパティ。 |
| `connectionSpec.id` | ソースの接続仕様 ID。 この ID は、[!DNL Flow Service] API を使用してソースを登録した後に取得できます。 |
| `auth.specName` | Platform へのソースの認証に使用する認証タイプ。 |
| `auth.params.host` | [!DNL MailChimp] API への接続に使用するルート URL。 ルート URL の形式は `https://{DC}.api.mailchimp.com` です。`{DC}` は、お使いのアカウントに対応するデータセンターを表します。 |
| `auth.params.authorizationTestUrl` | （オプション）認証テスト URL は、ベース接続の作成時に資格情報を検証するために使用します。 指定しない場合、代わりに、ソース接続の作成手順で資格情報が自動的にチェックされます。 |
| `auth.params.accessToken` | ソースの認証に使用される、対応するアクセストークン。 これは、OAuth ベースの認証に必要です。 |

**応答**

正常な応答は、新しく作成されたベース接続を返します。この中には一意の接続識別子 (`id`) も含まれます。 この ID は、次の手順でソースのファイル構造とコンテンツを調べるために必要です。

```json
{
    "id": "4cea039f-f1cc-4fa5-9136-db8dd4c7fbfa",
    "etag": "\"4000cff7-0000-0200-0000-6154bad60000\""
}
```

## ソースの参照 {#explore}

前の手順で生成したベース接続 ID を使用して、GETリクエストを実行することで、ファイルとディレクトリを調べることができます。

>[!TIP]
>
>`{SOURCE_PARAMS}` に対して受け入れられる書式型を取得するには、base64 で `list_id` 文字列全体をエンコードする必要があります。 例えば、base64 でエンコードされた `"list_id": "10c097ca71"` は `eyJsaXN0SWQiOiIxMGMwOTdjYTcxIn0=` と同じです。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=rest&object={OBJECT}&fileType={FILE_TYPE}&preview={PREVIEW}&sourceParams={SOURCE_PARAMS}
```

ソースのファイル構造とコンテンツを調べるためにGETリクエストを実行する場合は、次の表に示すクエリーパラメーターを含める必要があります。

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | 前の手順で生成したベース接続 ID。 |
| `{OBJECT_TYPE}` | 調査するオブジェクトのタイプ。 REST ソースの場合、この値はデフォルトで `rest` に設定されます。 |
| `{OBJECT}` | 調査するオブジェクト。 |
| `{FILE_TYPE}` | このパラメーターは、特定のディレクトリを表示する場合にのみ必要です。 この値は、調査するディレクトリのパスを表します。 |
| `{PREVIEW}` | 接続のコンテンツがプレビューをサポートするかどうかを定義する boolean 値です。 |
| `{SOURCE_PARAMS}` | `list_id` の base64 エンコードされた文字列。 |

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/05c595e5-edc3-45c8-90bb-fcf556b57c4b/explore?objectType=rest&object=json&fileType=json&preview=true&sourceParams=eyJsaXN0SWQiOiIxMGMwOTdjYTcxIn0=' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、問い合わせられたファイルの構造を返します。

```json
{ 
"data": [
    {
        "list_id": "10c097ca71",
        "_links": [
            {
                "rel": "self",
                "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members",
                "method": "GET",
                "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/CollectionResponse.json",
                "schema": "https://us6.api.mailchimp.com/schema/3.0/Paths/Lists/Members/Collection.json"
            },
            {
                "rel": "parent",
                "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71",
                "method": "GET",
                "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/Response.json"
            },
            {
                "rel": "create",
                "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members",
                "method": "POST",
                "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/Response.json",
                "schema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/POST.json"
            }
        ],
        "members": [
            {
                "id": "cff65fb4c5f5828666ad846443720efd",
                "email_address": "kendallt2134@gmail.com",
                "unique_email_id": "72c758cbf1",
                "contact_id": "874a0d6e9ddb89d8b4a31e416ead2d6f",
                "full_name": "Kendall Roy",
                "web_id": 547094062,
                "email_type": "html",
                "status": "subscribed",
                "consents_to_one_to_one_messaging": true,
                "merge_fields": {
                    "FNAME": "Kendall",
                    "LNAME": "Roy",
                    "ADDRESS": {
                        "country": "US"
                    }
                },
                "stats": {
                    "avg_open_rate": 0,
                    "avg_click_rate": 0
                },
                "ip_opt": "103.43.112.97",
                "timestamp_opt": "2021-06-01T15:31:36+00:00",
                "member_rating": 2,
                "last_changed": "2021-06-01T15:31:36+00:00",
                "vip": false,
                "location": {
                    "latitude": 0,
                    "longitude": 0,
                    "gmtoff": 0,
                    "dstoff": 0
                },
                "source": "Admin Add",
                "tags_count": 0,
                "list_id": "10c097ca71",
                "_links": [
                        {
                            "rel": "self",
                            "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members/cff65fb4c5f5828666ad846443720efd",
                            "method": "GET",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/Response.json"
                        },
                        {
                            "rel": "parent",
                            "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members",
                            "method": "GET",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/CollectionResponse.json",
                            "schema": "https://us6.api.mailchimp.com/schema/3.0/Paths/Lists/Members/Collection.json"
                        },
                        {
                            "rel": "update",
                            "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members/cff65fb4c5f5828666ad846443720efd",
                            "method": "PATCH",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/Response.json",
                            "schema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/PATCH.json"
                        },
                        {
                            "rel": "upsert",
                            "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members/cff65fb4c5f5828666ad846443720efd",
                            "method": "PUT",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/Response.json",
                            "schema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/PUT.json"
                        },
                        {
                            "rel": "delete",
                            "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members/cff65fb4c5f5828666ad846443720efd",
                            "method": "DELETE"
                        },
                        {
                            "rel": "activity",
                            "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members/cff65fb4c5f5828666ad846443720efd/activity",
                            "method": "GET",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/Activity/Response.json"
                        },
                        {
                            "rel": "goals",
                            "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members/cff65fb4c5f5828666ad846443720efd/goals",
                            "method": "GET",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/Goals/Response.json"
                        },
                        {
                            "rel": "notes",
                            "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members/cff65fb4c5f5828666ad846443720efd/notes",
                            "method": "GET",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/Notes/CollectionResponse.json"
                        },
                        {
                            "rel": "events",
                            "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members/cff65fb4c5f5828666ad846443720efd/events",
                            "method": "POST",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/Events/POST.json"
                        },
                        {
                            "rel": "delete_permanent",
                            "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members/cff65fb4c5f5828666ad846443720efd/actions/delete-permanent",
                            "method": "POST"
                        }
                    ]
                }
            ]  
        }
    ]
}
```

## ソース接続の作成 {#source-connection}

[!DNL Flow Service] API にPOSTリクエストを実行して、ソース接続を作成できます。 ソース接続は、接続 ID、ソースデータファイルのパス、接続仕様 ID で構成されます。

ソース接続を作成するには、データ形式属性の列挙値も定義する必要があります。

ファイルベースのソースには、次の列挙値を使用します。

| データフォーマット | 列挙値 |
| ----------- | ---------- |
| 区切り | `delimited` |
| JSON | `json` |
| Parquet | `parquet` |

すべてのテーブルベースのソースで、値を `tabular` に設定します。

**API 形式**

```https
POST /sourceConnections
```

**リクエスト**

次のリクエストは、 [!DNL MailChimp] のソース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
      "name": "MailChimp source connection to ingest listId",
      "description": "MailChimp Members source connection to ingest listId",
      "baseConnectionId": "4cea039f-f1cc-4fa5-9136-db8dd4c7fbfa",
      "connectionSpec": {
          "id": "2e8580db-6489-4726-96de-e33f5f60295f",
          "version": "1.0"
      },
      "data": {
          "format": "json",
      },
      "params": {
          "listId": "10c097ca71"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前がわかりやすいことを確認します。 |
| `description` | （オプション）ソース接続に関する詳細情報を提供するために含めることができるプロパティ。 |
| `baseConnectionId` | [!DNL MailChimp] のベース接続 ID。 この ID は、前の手順で生成されました。 |
| `connectionSpec.id` | ソースに対応する接続仕様 ID。 |
| `data.format` | 取り込む [!DNL MailChimp] データの形式。 |
| `params.listId` | [!DNL MailChimp] リスト ID は、オーディエンス ID とも呼ばれ、他の統合にオーディエンスデータを転送できます。 |

**応答**

正常な応答は、新しく作成されたソース接続の一意の識別子 (`id`) を返します。 この ID は、後の手順でデータフローを作成する際に必要です。

```json
{
    "id": "a51e4cf6-65ef-45f4-b4bf-4f03da5f01cc",
    "etag": "\"6b02b65d-0000-0200-0000-6154bfbe0000\""
}
```

## ターゲット XDM スキーマの作成 {#target-schema}

ソースデータを Platform で使用するには、必要に応じてソースデータを構造化するために、ターゲットスキーマを作成する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれる Platform データセットを作成します。

[ スキーマレジストリ API](https://www.adobe.io/experience-platform-apis/references/schema-registry/) に対してPOSTリクエストを実行すると、ターゲット XDM スキーマを作成できます。

ターゲット XDM スキーマの作成方法に関する詳細な手順については、[API を使用したスキーマの作成 ](../../../../../xdm/api/schemas.md) に関するチュートリアルを参照してください。

### ターゲットデータセットの作成 {#target-dataset}

[ カタログサービス API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) に対してPOSTリクエストを実行し、ペイロード内のターゲットスキーマの ID を指定することで、ターゲットデータセットを作成できます。

ターゲットデータセットの作成方法について詳しくは、[API を使用したデータセットの作成 ](../../../../../catalog/api/create-dataset.md) に関するチュートリアルを参照してください。

## ターゲット接続の作成 {#target-connection}

ターゲット接続は、取得されたデータの宛先への接続を表します。 ターゲット接続を作成するには、[!DNL Data Lake] に対応する固定接続仕様 ID を指定する必要があります。 この ID は次のとおりです。`c604ff05-7f1a-43c0-8e18-33bf874cb11c`.

これで、ターゲットスキーマとターゲットデータセット、接続仕様 ID が一意の識別子で [!DNL Data Lake] になりました。 これらの識別子を使用して、[!DNL Flow Service] API を使用してターゲット接続を作成し、受信ソースデータを格納するデータセットを指定できます。

**API 形式**

```https
POST /targetConnections
```

**リクエスト**

次のリクエストは、 [!DNL MailChimp] のターゲット接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
      "name": "MailChimp target connection",
      "description": "MailChimp Members target connection",
      "connectionSpec": {
          "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
          "version": "1.0"
      },
      "data": {
          "format": "parquet_xdm",
          "schema": {
              "id": "https://ns.adobe.com/{TENANT_ID}/schemas/570630b91eb9d5cf5db0436756abb110d02912917a67da2d",
              "version": "application/vnd.adobe.xed-full+json;version=1"
          }
      },
      "params": {
          "dataSetId": "6155e3a9bd13651949515f14"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | ターゲット接続の名前。 ターゲット接続の名前がわかりやすい名前であることを確認します。これは、ターゲット接続に関する情報を検索する場合に使用できます。 |
| `description` | （オプション）ターゲット接続に関する詳細情報を提供するために含めることができるプロパティ。 |
| `connectionSpec.id` | [!DNL Data Lake] に対応する接続仕様 ID。 この固定 ID は次のとおりです。`c604ff05-7f1a-43c0-8e18-33bf874cb11c`. |
| `data.format` | Platform に取り込む [!DNL MailChimp] データの形式。 |
| `params.dataSetId` | 前の手順で取得したターゲットデータセット ID。 |


**応答**

正常な応答は、新しいターゲット接続の一意の識別子 (`id`) を返します。 この ID は後の手順で必要になります。

```json
{
    "id": "8db5fb4a-6ce8-4370-afc0-1765e39535a5",
    "etag": "\"960093ce-0000-0200-0000-6154da3e0000\""
}
```

## マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。 これは、リクエストペイロード内で定義されたデータマッピングを使用して、[[!DNL Data Prep] API](https://www.adobe.io/experience-platform-apis/references/data-prep/) に対してPOSTリクエストを実行することで実現されます。

**API 形式**

```http
POST /conversion/mappingSets
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/conversion/mappingSets' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
      "version": 0,
      "xdmSchema": "_{TENANT_ID}.schemas.570630b91eb9d5cf5db0436756abb110d02912917a67da2d",
      "xdmVersion": "1.0",
      "mappings": [
      {
        "destinationXdmPath": "person.name.firstName",
        "sourceAttribute": "merge_fields.FNAME",
        "identity": false,
        "version": 0
      },
      {
        "destinationXdmPath": "person.name.lastName",
        "sourceAttribute": "merge_fields.LNAME",
        "identity": false,
        "version": 0
      }
    ]
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `xdmSchema` | 前の手順で生成された [ ターゲット XDM スキーマ ](#target-schema) の ID。 |
| `mappings.destinationXdmPath` | ソース属性のマッピング先の XDM パス。 |
| `mappings.sourceAttribute` | 宛先 XDM パスにマッピングする必要があるソース属性。 |
| `mappings.identity` | マッピングセットに [!DNL Identity Service] のマークを付けるかどうかを指定する boolean 値。 |

**応答**

正常な応答は、新しく作成されたマッピングの詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この値は、後の手順でデータフローを作成する際に必要です。

```json
{
    "id": "5a365b23962d4653b9d9be25832ee5b4",
    "version": 0,
    "createdDate": 1597784069368,
    "modifiedDate": 1597784069368,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}"
}
```

## フローの作成 {#flow}

[!DNL MailChimp] データを Platform に取り込む最後の手順は、データフローを作成することです。 現時点では、次の必須値が用意されています。

* [ソース接続 ID](#source-connection)
* [ターゲット接続 ID](#target-connection)
* [マッピング ID](#mapping)

データフローは、ソースからデータをスケジュールおよび収集する役割を果たします。 データフローを作成するには、前述の値をPOST内に指定しながらペイロードリクエストを実行します。

取り込みをスケジュールするには、まず開始時間の値を秒単位のエポック時間に設定する必要があります。 次に、頻度の値を次の 5 つのオプションのいずれかに設定する必要があります。`once`、`minute`、`hour`、`day`、または `week`。 interval 値は、2 つの連続した取り込みから 1 回限りの取り込みを作成するまでの間隔を指定し、間隔を設定する必要はありません。 その他のすべての周波数の間隔値は、`15` 以上に設定する必要があります。


**API 形式**

```http
POST /flows
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
      "name": "MailChimp Members dataflow",
      "description": "MailChimp Members dataflow",
      "flowSpec": {
          "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
          "version": "1.0"
      },
      "sourceConnectionIds": [
          "a51e4cf6-65ef-45f4-b4bf-4f03da5f01cc"
      ],
      "targetConnectionIds": [
          "8db5fb4a-6ce8-4370-afc0-1765e39535a5"
      ],
      "transformations": [
          {
              "name": "Mapping",
              "params": {
                  "mappingId": "5a365b23962d4653b9d9be25832ee5b4",
                  "mappingVersion": 0
              }
          }
      ],
      "scheduleParams": {
          "startTime": "1632809759",
          "frequency": "minute",
          "interval": 15
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | データフローの名前。 データフローの情報を調べるのに使用できるので、データフローの名前が説明的であることを確認します。 |
| `description` | （オプション）データフローの詳細を提供するために含めることができるプロパティ。 |
| `flowSpec.id` | データフローの作成に必要なフロー仕様 ID。 この固定 ID は次のとおりです。`6499120c-0b15-42dc-936e-847ea3c24d72`. |
| `flowSpec.version` | フロー仕様 ID の対応するバージョン。 この値のデフォルトは `1.0` です。 |
| `sourceConnectionIds` | 前の手順で生成された [ ソース接続 ID](#source-connection)。 |
| `targetConnectionIds` | 前の手順で生成された [ ターゲット接続 ID](#target-connection)。 |
| `transformations` | このプロパティには、データに適用する必要がある様々な変換が含まれています。 このプロパティは、XDM に準拠していないデータを Platform に取り込む場合に必要です。 |
| `transformations.name` | 変換に割り当てる名前。 |
| `transformations.params.mappingId` | 前の手順で生成された [ マッピング ID](#mapping)。 |
| `transformations.params.mappingVersion` | マッピング ID の対応するバージョン。 この値のデフォルトは `0` です。 |
| `scheduleParams.startTime` | データの最初の取り込みが開始されるまでの指定された開始時間。 |
| `scheduleParams.frequency` | データフローがデータを収集する頻度。 指定できる値は次のとおりです。`once`、`minute`、`hour`、`day`、または `week`。 |
| `scheduleParams.interval` | この間隔は、2 つの連続したフロー実行の間隔を指定します。 間隔の値はゼロ以外の整数にする必要があります。 頻度が `once` に設定されている場合は、間隔は不要で、他の頻度値の場合は `15` 以上にする必要があります。 |

**応答**

リクエストが成功した場合は、新しく作成したデータフローの ID(`id`) が返されます。 この ID を使用して、データフローを監視、更新または削除できます。

```json
{
    "id": "209812ad-7bef-430c-b5b2-a648aae72094",
    "etag": "\"2e01f11d-0000-0200-0000-615649660000\""
}
```

## データフローの監視

データフローを作成したら、そのデータフローを通じて取り込まれるデータを監視して、フロー実行、完了ステータス、エラーに関する情報を確認できます。

**API 形式**

```http
GET /runs?property=flowId=={FLOW_ID}
```

**リクエスト**

次のリクエストは、既存のデータフローの仕様を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/runs?property=flowId==993f908f-3342-4d9c-9f3c-5aa9a189ca1a' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、フロー実行に関する詳細を返します。フロー実行の作成日、ソース接続、ターゲット接続、およびフロー実行の一意の識別子 (`id`) に関する情報が含まれます。

```json
{
    "items": [
        {
            "id": "209812ad-7bef-430c-b5b2-a648aae72094",
            "createdAt": 1633044829955,
            "updatedAt": 1633044838006,
            "createdBy": "{CREATED_BY}",
            "updatedBy": "{UPDATED_BY}",
            "createdClient": "{CREATED_CLIENT}",
            "updatedClient": "{UPDATED_CLIENT}",
            "sandboxId": "{SANDBOX_ID}",
            "sandboxName": "{SANDBOX_NAME}",
            "imsOrgId": "{IMS_ORG}",
            "name": "MailChimp Members dataflow",
            "description": "MailChimp Members dataflow",
            "flowSpec": {
                "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
                "version": "1.0"
            },
            "state": "enabled",
            "version": "\"2e01f11d-0000-0200-0000-615649660000\"",
            "etag": "\"2e01f11d-0000-0200-0000-615649660000\"",
            "sourceConnectionIds": [
                "e70d2773-711f-43ee-b956-9a1a5da03dd8"
            ],
            "targetConnectionIds": [
                "43e141f6-6385-4d80-a4e4-c0fb59abbd43"
            ],
            "inheritedAttributes": {
                "sourceConnections": [
                    {
                        "id": "e70d2773-711f-43ee-b956-9a1a5da03dd8",
                        "connectionSpec": {
                            "id": "2e8580db-6489-4726-96de-e33f5f60295f",
                            "version": "1.0"
                        },
                        "baseConnection": {
                            "id": "05c595e5-edc3-45c8-90bb-fcf556b57c4b",
                            "connectionSpec": {
                                "id": "2e8580db-6489-4726-96de-e33f5f60295f",
                                "version": "1.0"
                            }
                        }
                    }
                ],
                "targetConnections": [
                    {
                        "id": "43e141f6-6385-4d80-a4e4-c0fb59abbd43",
                        "connectionSpec": {
                            "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
                            "version": "1.0"
                        }
                    }
                ]
            },
            "scheduleParams": {
                "startTime": "1633044818",
                "frequency": "minute",
                "interval": 15
            },
            "transformations": [
                {
                    "name": "Mapping",
                    "params": {
                        "mappingId": "5a365b23962d4653b9d9be25832ee5b4",
                        "mappingVersion": 0
                    }
                }
            ],
            "runs": "/flows/209812ad-7bef-430c-b5b2-a648aae72094/runs",
            "lastOperation": {
                "started": 1633044829988,
                "updated": 0,
                "operation": "create"
            }
        }
    ]
}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `items` | 特定のフロー実行に関連付けられたメタデータの単一のペイロードが含まれます。 |
| `id` | データフローに対応する ID が表示されます。 |
| `state` | データフローの現在の状態を表示します。 |
| `inheritedAttributes` | フローを定義する属性が含まれます。例えば、対応するベース、ソース、ターゲット接続の ID などです。 |
| `scheduleParams` | 開始時間（エポック時間）、頻度、間隔など、データフローの取り込みスケジュールに関する情報が含まれます。 |
| `transformations` | データフローに適用する変換プロパティに関する情報が含まれます。 |
| `runs` | フローの対応する実行 ID を表示します。 この ID を使用して、特定のフロー実行を監視できます。 |

## データフローの更新

データフローの実行スケジュール、名前および説明を更新するには、使用するフロー ID、バージョン、新しいスケジュールを指定しながら、[!DNL Flow Service] API に対してPATCHリクエストを実行します。

>[!IMPORTANT]
>
>`If-Match` ヘッダーは、PATCHリクエストを行う際に必要です。 このヘッダーの値は、更新する接続の一意のバージョンです。

**API 形式**

```http
PATCH /flows/{FLOW_ID}
```

**リクエスト**

次のリクエストは、フロー実行スケジュールと、データフローの名前および説明を更新します。

```shell
curl -X PATCH \
  'https://platform.adobe.io/data/foundation/flowservice/flows/209812ad-7bef-430c-b5b2-a648aae72094' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -H 'If-Match: "2e01f11d-0000-0200-0000-615649660000"' \
  -d '[
          {
              "op": "replace",
              "path": "/scheduleParams/frequency",
              "value": "day"
          },
          {
              "op": "replace",
              "path": "/name",
              "value": "MailChimp Members Dataflow 2.0"
          },
          {
              "op": "replace",
              "path": "/description",
              "value": "MailChimp Members Dataflow Updated"
          }
      ]'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `op` | データフローの更新に必要なアクションの定義に使用する操作呼び出し。 操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するパラメーターのパス。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

リクエストが成功した場合は、フロー ID と更新されたタグが返されます。 フロー ID を提供しながら、[!DNL Flow Service] API にGETリクエストを送信することで、更新を確認できます。

```json
{
    "id": "209812ad-7bef-430c-b5b2-a648aae72094",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

## データフローの削除

既存のフロー ID を使用すると、[!DNL Flow Service] API に対してDELETEリクエストを実行して、データフローを削除できます。

**API 形式**

```http
DELETE /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 削除するデータフローの一意の `id` 値。 |

**リクエスト**

```shell
curl -X DELETE \
  'https://platform.adobe.io/data/foundation/flowservice/flows/209812ad-7bef-430c-b5b2-a648aae72094' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、空白の本文とともに HTTP ステータス 204（コンテンツなし）を返します。データフローに対して検索 (GET) リクエストを試行して、削除を確認できます。 API は、データフローが削除されたことを示す HTTP 404（見つかりません）エラーを返します。

## 接続を更新する

接続の名前、説明、および資格情報を更新するには、[!DNL Flow Service] API に対してPATCHリクエストを実行し、基本接続 ID、バージョン、使用する新しい情報を指定します。

>[!IMPORTANT]
>
>`If-Match` ヘッダーは、PATCHリクエストを行う際に必要です。 このヘッダーの値は、更新する接続の一意のバージョンです。

**API 形式**

```http
PATCH /connections/{BASE_CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | 更新する接続の一意の `id` 値。 |

**リクエスト**

次のリクエストでは、接続を更新するための新しい名前と説明、および新しい資格情報のセットを提供します。

```shell
curl -X PATCH \
  'https://platform.adobe.io/data/foundation/flowservice/connections/4cea039f-f1cc-4fa5-9136-db8dd4c7fbfa' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -H 'If-Match: 4000cff7-0000-0200-0000-6154bad60000' \
  -d '[
      {
          "op": "replace",
          "path": "/auth/params",
          "value": {
              "username": "mailchimp-member-activity-user",
              "password": "{NEW_PASSWORD}"
          }
      },
      {
          "op": "replace",
          "path": "/name",
          "value": "MailChimp Members Connection 2.0"
      },
      {
          "op": "add",
          "path": "/description",
          "value": "Updated MailChimp Members Connection"
      }
  ]'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `op` | 接続の更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するパラメーターのパス。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

正常な応答は、ベース接続 ID と更新された etag を返します。 接続 ID を提供しながら [!DNL Flow Service] API にGETリクエストを送信することで、更新を確認できます。

```json
{
    "id": "4cea039f-f1cc-4fa5-9136-db8dd4c7fbfa",
    "etag": "\"3600e378-0000-0200-0000-5f40212f0000\""
}
```

## 接続を削除する

既存のベース接続 ID がある場合は、[!DNL Flow Service] API に対してDELETEリクエストを実行します。

**API 形式**

```http
DELETE /connections/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | 削除するベース接続の一意の `id` 値。 |

**リクエスト**

```shell
curl -X DELETE \
  'https://platform.adobe.io/data/foundation/flowservice/connections/4cea039f-f1cc-4fa5-9136-db8dd4c7fbfa' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、空白の本文とともに HTTP ステータス 204（コンテンツなし）を返します。

接続に対して検索 (GET) リクエストを試行すると、削除を確認できます。
