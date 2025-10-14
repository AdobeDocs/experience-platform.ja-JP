---
title: Flow Service API を使用したMicrosoft Dynamics ベース接続の作成
description: Flow Service API を使用してExperience PlatformをMicrosoft Dynamics アカウントに接続する方法について説明します。
exl-id: 423c6047-f183-4d92-8d2f-cc8cc26647ef
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1338'
ht-degree: 14%

---

# [!DNL Flow Service] API を使用した [!DNL Microsoft Dynamics] のExperience Platformへの接続

このガイドでは、[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/) を使用して [!DNL Microsoft Dynamics] ソースをAdobe Experience Platformに接続する方法について説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [&#x200B; ソース &#x200B;](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../../../landing/api-guide.md) を参照してください。

次の節では、[!DNL Flow Service] API を使用してExperience Platformを Dynamics アカウントに正しく接続するために必要な追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Dynamics] に接続するには、次の接続プロパティの値を指定する必要があります。

>[!BEGINTABS]

>[!TAB  基本認証 ]

| 資格情報 | 説明 |
| --- | --- |
| `serviceUri` | [!DNL Dynamics] インスタンスのサービス URL。 |
| `username` | [!DNL Dynamics] ユーザーアカウントのユーザー名。 |
| `password` | [!DNL Dynamics] アカウントのパスワード。 |

>[!TAB  サービスプリンシパルとキーの認証 ]

| 資格情報 | 説明 |
| --- | --- |
| `servicePrincipalId` | [!DNL Dynamics] アカウントのクライアント ID。 この ID は、サービスプリンシパルおよびキーベースの認証を使用する場合に必要です。 |
| `servicePrincipalKey` | サービスプリンシパルの秘密鍵。 この資格情報は、サービスプリンシパルおよびキーベースの認証を使用する場合に必要です。 |

>[!ENDTABS]

基本について詳しくは、[&#x200B; このドキュメント  [!DNL Dynamics]  を参照してください &#x200B;](https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/authenticate-oauth)。

## ベース接続の作成

>[!TIP]
>
>作成した後は、[!DNL Dynamics] ベース接続の認証タイプを変更できません。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Dynamics] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```http
POST /connections
```

>[!BEGINTABS]

>[!TAB  基本認証 ]

基本認証を使用した [!DNL Dynamics] ベース接続を作成するには、[!DNL Flow Service] API に POST リクエストを実行し、その際に接続の `serviceUri`、`username`、`password` の値を指定します。

**リクエスト**

次のリクエストは、基本認証を使用して [!DNL Dynamics] ソースのベース接続を作成します。

+++選択するとリクエストの例が表示されます

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Dynamics connection",
      "description": "Dynamics connection using basic auth",
      "auth": {
          "specName": "Basic Authentication for Dynamics-Online",
          "params": {
              "serviceUri": "{SERVICE_URI}",
              "username": "{USERNAME}",
              "password": "{PASSWORD}"
          }
      },
      "connectionSpec": {
          "id": "38ad80fe-8b06-4938-94f4-d4ee80266b07",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.serviceUri` | [!DNL Dynamics] インスタンスに関連付けられたサービス URI。 |
| `auth.params.username` | [!DNL Dynamics] アカウントに関連付けられたユーザー名。 |
| `auth.params.password` | [!DNL Dynamics] アカウントに関連付けられたパスワード。 |
| `connectionSpec.id` | [!DNL Dynamics] 接続仕様 ID：`38ad80fe-8b06-4938-94f4-d4ee80266b07` |

+++

**応答**

リクエストが成功した場合は、一意の ID （`id`）を含む、新しく作成されたベース接続が返されます。

+++選択すると応答の例が表示されます

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"9e0052a2-0000-0200-0000-5e35tb330000\""
}
```

+++

>[!TAB  サービスプリンシパルキーベースの認証 ]

サービスプリンシパルキーベースの認証を使用して [!DNL Dynamics] ベースの接続を作成するには、[!DNL Flow Service] API に POST リクエストを実行し、その際に接続の `serviceUri`、`servicePrincipalId`、`servicePrincipalKey` の値を指定します。

**リクエスト**

次のリクエストは、基本サービスプリンシパルキーベースの認証を使用して、[!DNL Dynamics] ソースのベース接続を作成します。

+++選択するとリクエストの例が表示されます

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Dynamics connection",
      "description": "Dynamics connection using key-based authentication",
      "auth": {
          "specName": "Service Principal Key Based Authentication",
          "params": {
              "serviceUri": "{SERVICE_URI}",
              "servicePrincipalId": "{SERVICE_PRINCIPAL_ID}",
              "servicePrincipalKey": "{SERVICE_PRINCIPAL_KEY}"
          }
      },
      "connectionSpec": {
          "id": "38ad80fe-8b06-4938-94f4-d4ee80266b07",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.serviceUri` | [!DNL Dynamics] インスタンスに関連付けられたサービス URI。 |
| `auth.params.servicePrincipalId` | [!DNL Dynamics] アカウントのクライアント ID。 この ID は、サービスプリンシパルおよびキーベースの認証を使用する場合に必要です。 |
| `auth.params.servicePrincipalKey` | サービスプリンシパルの秘密鍵。 この資格情報は、サービスプリンシパルおよびキーベースの認証を使用する場合に必要です。 |
| `connectionSpec.id` | [!DNL Dynamics] 接続仕様 ID：`38ad80fe-8b06-4938-94f4-d4ee80266b07` |

+++

**応答**

リクエストが成功した場合は、一意の ID （`id`）を含む、新しく作成された接続が応答として返されます。

+++選択すると応答の例が表示されます

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"9e0052a2-0000-0200-0000-5e35tb330000\""
}
```

+++

>[!ENDTABS]

## データテーブルの探索

[!DNL Dynamics] データテーブルを調べるには、`/connections/{BASE_CONNECTION_ID}/explore` エンドポイントに対してGET リクエストを実行し、クエリパラメーターの一部としてベース接続 ID を指定します。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
```

| クエリパラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | ベース接続の ID。 この ID を使用して、ソースの内容と構造を調べます。 |

**リクエスト**

次のリクエストは、ベース接続 ID が `dd668808-25da-493f-8782-f3433b976d1e` の [!DNL Dynamics] ソースについて、使用可能なテーブルとビューのリストを取得します。

+++選択するとリクエストの例が表示されます

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/dd668808-25da-493f-8782-f3433b976d1e/explore?objectType=root' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

応答が成功すると、ルートレベルの [!DNL Dynamics] テーブルとビューのディレクトリが返されます。

+++選択すると応答の例が表示されます

```json
[
    {
        "type": "table",
        "name": "systemuserlicenses",
        "path": "systemuserlicenses",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "table",
        "name": "Process Dependency",
        "path": "workflowdependency",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "view",
        "name": "accountView1",
        "path": "accountView1",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "view",
        "name": "Inactive_ACC_custom",
        "path": "Inactive_ACC_custom",
        "canPreview": true,
        "canFetchSchema": true
    }
]
```

+++

### プライマリキーを使用したデータ調査の最適化

>[!NOTE]
>
>最適化にプライマリキーアプローチを使用する場合にのみ、参照以外の属性を使用できます。

クエリをクエリパラメーターの一部として指定することで、検索クエリ `primaryKey` 最適化できます。 `primaryKey` をクエリパラメーターとして含める場合は、[!DNL Dynamics] テーブルのプライマリキーを指定する必要があります。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?preview=true&object={OBJECT}&objectType={OBJECT_TYPE}&previewCount=10&primaryKey={PRIMARY_KEY}
```

| クエリパラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | ベース接続の ID。 この ID を使用して、ソースの内容と構造を調べます。 |
| `preview` | データのプレビューを有効にするブール値。 |
| `{OBJECT}` | 参照する [!DNL Dynamics] オブジェクト。 |
| `{OBJECT_TYPE}` | オブジェクトのタイプ。 |
| `previewCount` | 返されるプレビューを特定の数のレコードに制限する制限。 |
| `{PRIMARY_KEY}` | プレビュー用に取得するテーブルの主キー。 |

**リクエスト**

+++選択するとリクエストの例が表示されます

```shell
curl -X GET \
  'https://platform-stage.adobe.io/data/foundation/flowservice/connections/dd668808-25da-493f-8782-f3433b976d1e/explore?preview=true&object=lead&objectType=table&previewCount=10&primaryKey=leadid' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

## テーブルの構造を検査する

特定のテーブルの構造を調べるには、にGET リクエストを実行し、`/connections/{BASE_CONNECTION_ID}/explore` の特定のテーブルへのパスをクエリパラメーターとして指定します。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?object={TABLE_PATH}&objectType=table
```

| クエリーパラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | ベース接続の ID。 この ID を使用して、ソースの内容と構造を調べます。 |
| `{TABLE_PATH}` | 参照する特定のテーブルへのパス。 |

**リクエスト**

次のリクエストでは、パス `workflowdependency` を含む [!DNL Dynamics] テーブルの構造と内容を取得します。

+++選択するとリクエストの例が表示されます

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/dd668808-25da-493f-8782-f3433b976d1e/explore?object=workflowdependency&objectType=table' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

応答が成功すると、パス `workflowdependency` のコンテンツが返されます。

+++選択すると応答の例が表示されます

```json
{
    "format": "flat",
    "schema": {
        "columns": [
            {
                "name": "first_name",
                "type": "string",
                "meta": {
                    "originalType": "String"
                }
            },
            {
                "name": "last_name",
                "type": "string",
                "meta": {
                    "originalType": "String"
                }
            },
            {
                "name": "email",
                "type": "string",
                "meta": {
                    "originalType": "String"
                }
            }
        ]
    }
}
```

+++

## ビューの構造の検査

ま [!DNL Dynamics]、ビューとは、表示する列、各列の幅、レコードのリストの並べ替えのデフォルトのシステム、リストに表示するレコードを制限するために適用されるデフォルトのフィルターを指します。

ビューの構造を調べるには、にGET リクエストを実行し、`/connections/{BASE_CONNECTION_ID}/explore` クエリパラメーターでビューパスを指定します。 さらに、`view` のように `objectType` を指定する必要があります。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?object={VIEW_PATH}&objectType=view
```

| クエリーパラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | ベース接続の ID。 この ID を使用して、ソースの内容と構造を調べます。 |
| `{VIEW_PATH}` | 検査するビューへのパス。 |

**リクエスト**

次のリクエストは、`accountView1` を取得します。

+++選択するとリクエストの例が表示されます

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/dd668808-25da-493f-8782-f3433b976d1e/explore?object=accountView1&objectType=view' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

応答が成功すると、`accountView1` の構造が返されます。

+++選択すると応答の例が表示されます

```json
{
    "format": "flat",
    "schema": {
        "columns": [
            {
                "name": "name",
                "type": "string",
                "meta": {
                    "originalType": "string"
                },
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "fetchxml",
                "type": "string",
                "meta": {
                    "originalType": "string"
                },
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "querytype",
                "type": "integer",
                "meta": {
                    "originalType": "int"
                },
                "xdm": {
                    "type": "integer",
                    "minimum": -2147483648,
                    "maximum": 2147483647
                }
            },
            {
                "name": "userqueryid",
                "type": "string",
                "meta": {
                    "originalType": "guid"
                },
                "xdm": {
                    "type": "string"
                }
            }
        ]
    }
}
```

+++

## エンティティタイプビューをプレビュー

ビューのコンテンツをプレビューするには、`/connections/{BASE_CONNECTION_ID}/explore` に対してGET リクエストを実行し、ビューパスと `preview=true` をクエリパラメーターに含めます。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?object={VIEW_PATH}&preview=true&objectType=view
```

| クエリーパラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | ベース接続の ID。 この ID を使用して、ソースの内容と構造を調べます。 |
| `{VIEW_PATH}` | 検査するビューへのパス。 |


**リクエスト**

次のリクエストは、`accountView1` のコンテンツをプレビューします。

+++選択するとリクエストの例が表示されます

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections/dd668808-25da-493f-8782-f3433b976d1e/explore?object=accountView1&preview=true&objectType=view' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

応答が成功すると、`accountView1` のコンテンツが返されます。

+++選択すると応答の例が表示されます

```json
{
    "format": "flat",
    "schema": {
        "columns": [
            {
                "name": "emailaddress1",
                "type": "string",
                "meta": {
                    "originalType": "string"
                },
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "contactid",
                "type": "string",
                "meta": {
                    "originalType": "guid"
                },
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "fullname",
                "type": "string",
                "meta": {
                    "originalType": "string"
                },
                "xdm": {
                    "type": "string"
                }
            }
        ]
    },
    "data": [
        {
            "contactid": "396e19de-0852-ec11-8c62-00224808a1df",
            "fullname": "Tim Barr",
            "emailaddress1": "barrtim@googlemedia.com"
        }
    ]
}
```

+++

## ビューを取り込むソース接続を作成

ソース接続を作成してビューを取り込むには、`/sourceConnections` エンドポイントに対して POST リクエストを実行し、テーブル名を指定して、リクエスト本文で `view` のように指定 `entityType` ます。

**API 形式**

```http
POST /sourceConnections
```

**リクエスト**

次のリクエストでは、[!DNL Dynamics] ソース接続を作成し、ビューを取り込んでいます。

+++選択するとリクエストの例が表示されます

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Dynamics Source Connection",
      "description": "Dynamics Source Connection",
      "baseConnectionId": "dd668808-25da-493f-8782-f3433b976d1e",
      "data": {
          "format": "tabular",
          "schema": null,
          "properties": null
      },
      "params": {
          "tableName": "Contacts with name TIM",
          "entityType": "view"
      },
      "connectionSpec": {
          "id": "38ad80fe-8b06-4938-94f4-d4ee80266b07",
          "version": "1.0"
      }
  }'
```

+++

**応答**

正常な応答は、新しく生成されたソース接続 ID と、対応する etag を返します。

+++選択すると応答の例が表示されます

```json
{
    "id": "e566bab3-1b58-428c-b751-86b8cc79a3b4",
    "etag": "\"82009592-0000-0200-0000-678121030000\""
}
```

+++

### プライマリキーを使用したデータフローの最適化

また、リクエスト本文のパラメーターの一部としてプライマリキーを指定することで、[!DNL Dynamics] データフローを最適化することもできます。

**API 形式**

```http
POST /sourceConnections
```

**リクエスト**

次のリクエストは、プライマリキーを `contactid` として指定して、[!DNL Dynamics] ソース接続を作成します。

+++選択するとリクエストの例が表示されます

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Dynamics Source Connection",
      "description": "Dynamics Source Connection",
      "baseConnectionId": "dd668808-25da-493f-8782-f3433b976d1e",
      "data": {
          "format": "tabular"
      },
      "params": {
          "tableName": "contact",
          "primaryKey": "contactid"
      },
      "connectionSpec": {
          "id": "38ad80fe-8b06-4938-94f4-d4ee80266b07",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `baseConnectionId` | ベース接続の ID。 |
| `data.format` | データの形式。 |
| `params.tableName` | [!DNL Dynamics] のテーブル名。 |
| `params.primaryKey` | クエリを最適化するテーブルのプライマリキー。 |
| `connectionSpec.id` | [!DNL Dynamics] ソースに対応する接続仕様 ID。 |

+++

**応答**

正常な応答は、新しく生成されたソース接続 ID と、対応する etag を返します。

+++選択すると応答の例が表示されます

```json
{
    "id": "e566bab3-1b58-428c-b751-86b8cc79a3b4",
    "etag": "\"82009592-0000-0200-0000-678121030000\""
}
```

+++


## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Microsoft Dynamics] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [&#x200B; [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [&#x200B; [!DNL Flow Service] API を使用した、CRM データをExperience Platformに取り込むデータフローの作成](../../collect/crm.md)
