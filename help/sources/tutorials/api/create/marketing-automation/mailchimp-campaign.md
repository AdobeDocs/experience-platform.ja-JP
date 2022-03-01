---
keywords: Experience Platform；ホーム；人気の高いトピック；ソース；コネクタ；ソースコネクタ；ソース sdk;SDK;SDK
solution: Experience Platform
title: フローサービス API を使用して、Mailchimp キャンペーンのデータフローを作成します
topic-legacy: tutorial
description: フローサービス API を使用してAdobe Experience Platformを MailChimp Campaign に接続する方法を説明します。
exl-id: fd4821c7-6fe1-4cad-8e13-3549dbe0ce98
source-git-commit: fd851dea5623522e4706c6beb8bd086d466773b5
workflow-type: tm+mt
source-wordcount: '2319'
ht-degree: 7%

---

# のデータフローの作成 [!DNL Mailchimp Campaign] フローサービス API の使用

次のチュートリアルでは、ソース接続とデータフローを作成して、 [!DNL Mailchimp Campaign] を使用して Platform にデータを送信する [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## 前提条件

接続する前に [!DNL Mailchimp] OAuth 2 の更新コードを使用してAdobe Experience Platformにアクセスするには、まず [!DNL MailChimp.] 詳しくは、 [[!DNL Mailchimp] OAuth 2 ガイド](https://mailchimp.com/developer/marketing/guides/access-user-data-oauth-2/) を参照してください。

## ベース接続を作成する {#base-connection}

一度 [!DNL Mailchimp] 認証資格情報を使用する場合は、データフローの作成プロセスを開始して、 [!DNL Mailchimp Campaign] データを Platform に送信します。 データフローを作成する最初の手順は、ベース接続を作成することです。

ベース接続では、ソースと Platform の間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID など）が保持されます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を識別できます。

[!DNL Mailchimp] は、基本認証と OAuth 2 更新コードの両方をサポートしています。 どちらの認証タイプを使用して認証する方法については、次の例を参照してください。

### の作成 [!DNL Mailchimp] 基本認証を使用したベース接続

を作成するには、以下を実行します。 [!DNL Mailchimp] 基本認証を使用したベース接続、 `/connections` エンドポイント [!DNL Flow Service] の資格情報を指定する際の API `host`, `authorizationTestUrl`, `username`、および `password`.

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL Mailchimp]:

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
      "name": "Mailchimp base connection with basic authentication",
      "description": "Mailchimp Campaign base connection with basic authentication",
      "connectionSpec": {
          "id": "c8ce8c8c-37fb-4162-9fbf-c2f181e04a7a",
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
| `name` | ベース接続の名前。 ベース接続の情報を検索する際に使用できるので、ベース接続の名前がわかりやすい名前になっていることを確認します。 |
| `description` | （オプション）ベース接続に関する詳細情報を提供するために含めることができるプロパティ。 |
| `connectionSpec.id` | ソースの接続仕様 ID。 この ID は、ソースが登録され、で承認された後に、 [!DNL Flow Service] API |
| `auth.specName` | ソースを Platform に接続するために使用する認証タイプ。 |
| `auth.params.host` | 接続に使用するルート URL [!DNL Mailchimp] API ルート URL の形式は、 `https://{DC}.api.mailchimp.com`で、 `{DC}` は、お使いのアカウントに対応するデータセンターを表します。 |
| `auth.params.authorizationTestUrl` | （オプション）認証テスト URL は、ベース接続の作成時に資格情報を検証するために使用されます。 指定しない場合、代わりに、ソース接続の作成手順で資格情報が自動的に確認されます。 |
| `auth.params.username` | に対応するユーザー名 [!DNL Mailchimp] アカウント これは、基本認証に必要です。 |
| `auth.params.password` | ユーザーの [!DNL Mailchimp] アカウント これは、基本認証に必要です。 |

**応答**

正常な応答は、新しく作成されたベース接続を返します。この中には、一意の接続識別子 (`id`) をクリックします。 この ID は、次の手順でソースのファイル構造とコンテンツを調べるために必要です。

```json
{
    "id": "9601747c-6874-4c02-bb00-5732a8c43086",
    "etag": "\"3702dabc-0000-0200-0000-615b5b5a0000\""
}
```

### の作成 [!DNL Mailchimp] OAuth 2 更新コードを使用したベース接続

を作成するには、以下を実行します。 [!DNL Mailchimp] OAuth 2 更新コードを使用したベース接続。 `/connections` が `host`, `authorizationTestUrl`、および `accessToken`.

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、 [!DNL Mailchimp]:

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
      "name": "Mailchimp base connection with OAuth 2 refresh code",
      "description": "Mailchimp Campaign base connection with OAuth 2 refresh code",
      "connectionSpec": {
          "id": "c8ce8c8c-37fb-4162-9fbf-c2f181e04a7a",
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
| `name` | ベース接続の名前。 ベース接続の情報を検索する際に使用できるので、ベース接続の名前がわかりやすい名前になっていることを確認します。 |
| `description` | （オプション）ベース接続に関する詳細情報を提供するために含めることができるプロパティ。 |
| `connectionSpec.id` | ソースの接続仕様 ID。 この ID は、 [!DNL Flow Service] API |
| `auth.specName` | Platform へのソースの認証に使用する認証タイプ。 |
| `auth.params.host` | 接続に使用するルート URL [!DNL Mailchimp] API ルート URL の形式は、 `https://{DC}.api.mailchimp.com`で、 `{DC}` は、お使いのアカウントに対応するデータセンターを表します。 |
| `auth.params.authorizationTestUrl` | （オプション）認証テスト URL は、ベース接続の作成時に資格情報を検証するために使用されます。 指定しない場合、代わりに、ソース接続の作成手順で資格情報が自動的に確認されます。 |
| `auth.params.accessToken` | ソースの認証に使用された、対応するアクセストークン。 これは、OAuth ベースの認証に必要です。 |

**応答**

正常な応答は、新しく作成されたベース接続を返します。この中には、一意の接続識別子 (`id`) をクリックします。 この ID は、次の手順でソースのファイル構造とコンテンツを調べるために必要です。

```json
{
    "id": "9601747c-6874-4c02-bb00-5732a8c43086",
    "etag": "\"3702dabc-0000-0200-0000-615b5b5a0000\""
}
```

## ソースを参照 {#explore}

前の手順で生成したベース接続 ID を使用して、GETリクエストを実行することで、ファイルとディレクトリを調べることができます。 ソースのファイル構造とコンテンツを調べるためのGETリクエストを実行する場合は、次の表に示すクエリーパラメーターを含める必要があります。

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | 前の手順で生成したベース接続 ID。 |
| `{OBJECT_TYPE}` | 参照するオブジェクトのタイプ。 REST ソースの場合、この値はデフォルトでになります。 `rest`. |
| `{OBJECT}` | 参照するオブジェクト。 |
| `{FILE_TYPE}` | このパラメーターは、特定のディレクトリを表示する場合にのみ必要です。 その値は、参照するディレクトリのパスを表します。 |
| `{PREVIEW}` | 接続のコンテンツがプレビューをサポートするかどうかを定義する boolean 値です。 |
| `{SOURCE_PARAMS}` | の base64 エンコードされた文字列 `campaign_id`. |

>[!TIP]
>
>の受け入れ可能な format-type を取得するには `{SOURCE_PARAMS}`を使用する場合は、 `campaignId` 文字列を base64 に格納します。 例： `{"campaignId": "c66a200cda"}` base64 でエンコードされた値は、次の値と等しくなります。 `eyJjYW1wYWlnbklkIjoiYzY2YTIwMGNkYSJ9`.

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=rest&objectType={OBJECT_TYPE}&fileType={FILE_TYPE}&preview={PREVIEW}&sourceParams={SOURCE_PARAMS}
```

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/05c595e5-edc3-45c8-90bb-fcf556b57c4b/explore?objectType=rest&object=json&fileType=json&preview=true&sourceParams=eyJjYW1wYWlnbklkIjoiYzY2YTIwMGNkYSJ9' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、クエリされたファイルの構造を返します。

```json
{
    "data": [
        {
            "emails": [
                {
                    "campaign_id": "c66a200cda",
                    "list_id": "10c097ca71",
                    "list_is_active": true,
                    "email_id": "cff65fb4c5f5828666ad846443720efd",
                    "email_address": "kendall2134@gmail.com",
                    "_links": [
                        {
                            "rel": "parent",
                            "href": "https://us6.api.mailchimp.com/3.0/reports/c66a200cda/email-activity",
                            "method": "GET",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Reports/EmailActivity/CollectionResponse.json"
                        },
                        {
                            "rel": "self",
                            "href": "https://us6.api.mailchimp.com/3.0/reports/c66a200cda/email-activity/cff65fb4c5f5828666ad846443720efd",
                            "method": "GET",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Reports/EmailActivity/Response.json"
                        },
                        {
                            "rel": "member",
                            "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members/cff65fb4c5f5828666ad846443720efd",
                            "method": "GET",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/Response.json"
                        }
                    ]
                },
                {
                    "campaign_id": "c66a200cda",
                    "list_id": "10c097ca71",
                    "list_is_active": true,
                    "email_id": "a16b82774b211afaf60902d1afd8abc5",
                    "email_address": "logan9935890967@gmail.com",
                    "_links": [
                        {
                            "rel": "parent",
                            "href": "https://us6.api.mailchimp.com/3.0/reports/c66a200cda/email-activity",
                            "method": "GET",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Reports/EmailActivity/CollectionResponse.json"
                        },
                        {
                            "rel": "self",
                            "href": "https://us6.api.mailchimp.com/3.0/reports/c66a200cda/email-activity/a16b82774b211afaf60902d1afd8abc5",
                            "method": "GET",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Reports/EmailActivity/Response.json"
                        },
                        {
                            "rel": "member",
                            "href": "https://us6.api.mailchimp.com/3.0/lists/10c097ca71/members/a16b82774b211afaf60902d1afd8abc5",
                            "method": "GET",
                            "targetSchema": "https://us6.api.mailchimp.com/schema/3.0/Definitions/Lists/Members/Response.json"
                        }
                    ]
                },
            ]
        }
    ]    
}
```

## ソース接続の作成 {#source-connection}

ソース接続を作成するには、 [!DNL Flow Service] API ソース接続は、接続 ID、ソースデータファイルへのパス、接続仕様 ID で構成されます。

ソース接続を作成するには、 data format 属性の enum 値も定義する必要があります。

ファイルベースのソースには、次の enum 値を使用します。

| データフォーマット | 列挙値 |
| ----------- | ---------- |
| 区切り | `delimited` |
| JSON | `json` |
| Parquet | `parquet` |

すべてのテーブルベースのソースで、値をに設定します。 `tabular`.

**API 形式**

```https
POST /sourceConnections
```

**リクエスト**

次のリクエストは、 [!DNL Mailchimp]:

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -d '{
      "name": "MailChimp source connection to ingest campaign ID",
      "description": "MailChimp Campaign source connection to ingest campaign ID",
      "baseConnectionId": "4cea039f-f1cc-4fa5-9136-db8dd4c7fbfa",
      "connectionSpec": {
          "id": "c8ce8c8c-37fb-4162-9fbf-c2f181e04a7a",
          "version": "1.0"
      },
      "data": {
          "format": "json"
      },
      "params": {
          "campaignId": "c66a200cda"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前がわかりやすい名前になっていることを確認します。 |
| `description` | ソース接続に関する詳細情報を提供するために含めることができるオプションの値です。 |
| `baseConnectionId` | のベース接続 ID [!DNL Mailchimp]. この ID は、前の手順で生成されました。 |
| `connectionSpec.id` | ソースに対応する接続仕様 ID。 |
| `data.format` | の形式 [!DNL Mailchimp] 取り込むデータ。 |
| `params.campaignId` | この [!DNL Mailchimp] キャンペーン ID は特定の [!DNL Mailchimp] キャンペーンを開き、リストやオーディエンスに e メールを送信できます。 |

**応答**

正常な応答は、一意の識別子 (`id`) に含まれます。 この ID は、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "d6557bf1-7347-415f-964c-9316bd4cbf56",
    "etag": "\"e205c206-0000-0200-0000-615b5c070000\""
}
```

## ターゲット XDM スキーマの作成 {#target-schema}

ソースデータを Platform で使用するには、必要に応じてターゲットスキーマを作成し、ソースデータを構造化する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれる Platform データセットを作成します。

ターゲット XDM スキーマは、 [スキーマレジストリ API](https://www.adobe.io/experience-platform-apis/references/schema-registry/).

ターゲット XDM スキーマの作成方法に関する詳細な手順については、 [API を使用したスキーマの作成](../../../../../xdm/api/schemas.md).

### ターゲットデータセットの作成 {#target-dataset}

ターゲットデータセットは、 [カタログサービス API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml)：ペイロード内にターゲットスキーマの ID を指定します。

ターゲットデータセットの作成手順について詳しくは、 [API を使用したデータセットの作成](../../../../../catalog/api/create-dataset.md).

## ターゲット接続の作成 {#target-connection}

ターゲット接続は、取り込まれたデータが格納される宛先への接続を表します。 ターゲット接続を作成するには、 [!DNL Data Lake]. この ID は次のとおりです。 `c604ff05-7f1a-43c0-8e18-33bf874cb11c`.

これで、ターゲットスキーマとターゲットデータセットに一意の識別子、および [!DNL Data Lake]. これらの識別子を使用すると、 [!DNL Flow Service] 受信ソースデータを格納するデータセットを指定する API。

**API 形式**

```https
POST /targetConnections
```

**リクエスト**

次のリクエストは、 [!DNL Mailchimp]:

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
      "description": "MailChimp Campaign target connection",
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
| `name` | ターゲット接続の名前。 ターゲット接続の名前がわかりやすい名前であることを確認します。これを使用して、ターゲット接続に関する情報を検索できます。 |
| `description` | ターゲット接続に関する詳細を提供するために含めることができるオプションの値です。 |
| `connectionSpec.id` | に対応する接続仕様 ID [!DNL Data Lake]. この修正済み ID は次のとおりです。 `c604ff05-7f1a-43c0-8e18-33bf874cb11c`. |
| `data.format` | の形式 [!DNL Mailchimp] Platform に取り込むデータです。 |
| `params.dataSetId` | 前の手順で取得したターゲットデータセット ID。 |


**応答**

正常な応答は、新しいターゲット接続の一意の識別子 (`id`) をクリックします。 この ID は、後の手順で必要になります。

```json
{
    "id": "9463fe9c-027d-4347-a423-894fcd105647",
    "etag": "\"b902e822-0000-0200-0000-615b5c370000\""
}
```

>[!IMPORTANT]
>
>データ準備関数は、現在、 [!DNL Mailchimp Campaign].

<!--
## Create a mapping {#mapping}

In order for the source data to be ingested into a target dataset, it must first be mapped to the target schema that the target dataset adheres to. This is achieved by performing a POST request to Conversion Service with data mappings defined within the request payload.

**API format**

```http
POST /conversion/mappingSets
```

**Request**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/conversion/mappingSets' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
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

| Property | Description |
| --- | --- |
| `xdmSchema` | The ID of the [target XDM schema](#target-schema) generated in an earlier step. |
| `mappings.destinationXdmPath` | The destination XDM path where the source attribute is being mapped to. |
| `mappings.sourceAttribute` | The source attribute that needs to be mapped to a destination XDM path. |
| `mappings.identity` | A boolean value that designates whether the mapping set will be marked for [!DNL Identity Service]. |

**Response**

A successful response returns details of the newly created mapping including its unique identifier (`id`). This value is required in a later step to create a dataflow.

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

--->

## フローの作成 {#flow}

～に向けた最後の一歩 [!DNL Mailchimp] データを Platform に送信する場合、データフローを作成します。 現時点では、次の必要な値が準備されています。

* [ソース接続 ID](#source-connection)
* [ターゲット接続 ID](#target-connection)

データフローは、ソースからデータをスケジュールおよび収集する役割を果たします。 データフローを作成するには、前述の値をPOST内に指定しながらペイロードリクエストを実行します。

取り込みをスケジュールするには、まず開始時刻の値をエポック時間（秒）に設定する必要があります。 次に、頻度の値を次の 5 つのオプションのいずれかに設定する必要があります。 `once`, `minute`, `hour`, `day`または `week`. 間隔の値は、2 つの連続した取り込みから 1 回の取り込みを作成するまでの間隔を指定します (`once`) に間隔を設定する必要はありません。 その他のすべての頻度では、間隔の値を次の値以上に設定する必要があります `15`.


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
      "name": "MailChimp Campaign dataflow",
      "description": "MailChimp Campaign dataflow",
      "flowSpec": {
          "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
          "version": "1.0"
      },
      "sourceConnectionIds": [
          "d6557bf1-7347-415f-964c-9316bd4cbf56"
      ],
      "targetConnectionIds": [
          "9463fe9c-027d-4347-a423-894fcd105647"
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
| `name` | データフローの名前。 データフローの情報を検索する際に使用できるので、データフローの名前が説明的であることを確認します。 |
| `description` | （オプション）データフローの詳細を提供するために含めることができるプロパティ。 |
| `flowSpec.id` | データフローの作成に必要なフロー仕様 ID。 この修正済み ID は次のとおりです。 `6499120c-0b15-42dc-936e-847ea3c24d72`. |
| `flowSpec.version` | フロー仕様 ID の対応するバージョン。 この値のデフォルトはです。 `1.0`. |
| `sourceConnectionIds` | この [ソース接続 ID](#source-connection) が以前の手順で生成された場合に発生します。 |
| `targetConnectionIds` | この [ターゲット接続 ID](#target-connection) が以前の手順で生成された場合に発生します。 |
| `scheduleParams.startTime` | データの最初の取り込みが開始されるまでの指定された開始時間。 |
| `scheduleParams.frequency` | データフローがデータを収集する頻度。 指定できる値は次のとおりです。 `once`, `minute`, `hour`, `day`または `week`. |
| `scheduleParams.interval` | 間隔は、2 つの連続したフロー実行の間隔を示します。 間隔の値は、ゼロ以外の整数である必要があります。 頻度が `once` およびは次よりも大きいか等しい必要があります `15` を使用します。 |

**応答**

成功すると、ID(`id`) を含める必要があります。 この ID を使用して、データフローを監視、更新または削除できます。

```json
{
    "id": "be2d5249-eeaf-4a74-bdbd-b7bf62f7b2da",
    "etag": "\"7e010621-0000-0200-0000-615b5c9b0000\""
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

正常な応答は、フロー実行に関する詳細 ( 作成日、ソース接続、ターゲット接続に関する情報、フロー実行の一意の識別子 (`id`) をクリックします。

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
            "name": "MailChimp Campaign dataflow",
            "description": "MailChimp Campaign dataflow",
            "flowSpec": {
                "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
                "version": "1.0"
            },
            "state": "enabled",
            "version": "\"7e01322c-0000-0200-0000-615b5d520000\"",
            "etag": "\"7e01322c-0000-0200-0000-615b5d520000\"",
            "sourceConnectionIds": [
                "d6557bf1-7347-415f-964c-9316bd4cbf56"
            ],
            "targetConnectionIds": [
                "9463fe9c-027d-4347-a423-894fcd105647"
            ],
            "inheritedAttributes": {
                "sourceConnections": [
                    {
                        "id": "d6557bf1-7347-415f-964c-9316bd4cbf56",
                        "connectionSpec": {
                            "id": "c8ce8c8c-37fb-4162-9fbf-c2f181e04a7a",
                            "version": "1.0"
                        },
                        "baseConnection": {
                            "id": "9601747c-6874-4c02-bb00-5732a8c43086",
                            "connectionSpec": {
                                "id": "c8ce8c8c-37fb-4162-9fbf-c2f181e04a7a",
                                "version": "1.0"
                            }
                        }
                    }
                ],
                "targetConnections": [
                    {
                        "id": "9463fe9c-027d-4347-a423-894fcd105647",
                        "connectionSpec": {
                            "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
                            "version": "1.0"
                        }
                    }
                ]
            },
            "scheduleParams": {
                "startTime": "1633377385",
                "frequency": "minute",
                "interval": 15
            },
            "runs": "/flows/be2d5249-eeaf-4a74-bdbd-b7bf62f7b2da/runs",
            "lastOperation": {
                "started": 1633377421476,
                "updated": 0,
                "operation": "create"
            },
            "lastRunDetails": {
                "id": "84f95788-3e83-4ce0-8e45-c0a89117c6f1",
                "state": "failed",
                "startedAtUTC": 1633377445979,
                "completedAtUTC": 1633377487082
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
| `scheduleParams` | データフローの取り込みスケジュールに関する情報が含まれます。例えば、開始時間（エポック時間）、頻度、間隔などです。 |
| `transformations` | データフローに適用される変換プロパティに関する情報が含まれます。 |
| `runs` | フローの対応する実行 ID を表示します。 この ID を使用して、特定のフロー実行を監視できます。 |

## データフローを更新する

データフローの実行スケジュール、名前、説明を更新するには、に対してPATCHリクエストを実行します。 [!DNL Flow Service] 使用するフロー ID、バージョンおよび新しいスケジュールを指定する際の API。

>[!IMPORTANT]
>
>この `If-Match` ヘッダーは、ヘッダーリクエストをおこなう際にPATCHする必要があります。 このヘッダーの値は、更新する接続の一意のバージョンです。

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
              "value": "MailChimp Campaign Dataflow 2.0"
          },
          {
              "op": "replace",
              "path": "/description",
              "value": "MailChimp Campaign Dataflow Updated"
          }
      ]'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。 操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するパラメーターのパス。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

リクエストが成功した場合は、フロー ID と更新された etag が返されます。 更新を検証するには、 [!DNL Flow Service] フロー ID を指定する際の API。

```json
{
    "id": "209812ad-7bef-430c-b5b2-a648aae72094",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

## データフローを削除

既存のフロー ID を使用すると、 [!DNL Flow Service] API

**API 形式**

```http
DELETE /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 一意の `id` の値を指定します。 |

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

正常な応答は、空白の本文とともに HTTP ステータス 204（コンテンツなし）を返します。データフローに対して検索 (GET) リクエストを試行することで、削除を確認できます。 API は、データフローが削除されたことを示す HTTP 404（見つかりません）エラーを返します。

## 接続を更新

接続の名前、説明、および資格情報を更新するには、に対してPATCHリクエストを実行します。 [!DNL Flow Service] ベース接続 ID、バージョンおよび使用する新しい情報を提供する際の API。

>[!IMPORTANT]
>
>この `If-Match` ヘッダーは、ヘッダーリクエストをおこなう際にPATCHする必要があります。 このヘッダーの値は、更新する接続の一意のバージョンです。

**API 形式**

```http
PATCH /connections/{BASE_CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | 一意の `id` 更新する接続の値。 |

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
          "value": "MailChimp Campaign Connection 2.0"
      },
      {
          "op": "add",
          "path": "/description",
          "value": "Updated MailChimp Campaign Connection"
      }
  ]'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `op` | 接続の更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するパラメーターのパス。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

正常な応答は、ベース接続 ID と更新された etag を返します。 更新を検証するには、 [!DNL Flow Service] 接続 ID を指定する際の API。

```json
{
    "id": "4cea039f-f1cc-4fa5-9136-db8dd4c7fbfa",
    "etag": "\"3600e378-0000-0200-0000-5f40212f0000\""
}
```

## 接続を削除

既存のベース接続 ID がある場合は、 [!DNL Flow Service] API

**API 形式**

```http
DELETE /connections/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | 一意の `id` 削除するベース接続の値。 |

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

接続に対して検索 (GET) リクエストを試みて、削除を確認できます。
