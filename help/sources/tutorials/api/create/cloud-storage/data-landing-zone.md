---
title: Flow Service API を使用したAdobe Experience Platformへのデータランディングゾーンの接続
description: Flow Service API を使用してAdobe Experience Platformを Data Landing Zone に接続する方法を説明します。
exl-id: bdb60ed3-7c63-4a69-975a-c6f1508f319e
source-git-commit: 16cc811a545414021b8686ae303d6112bcf6cebb
workflow-type: tm+mt
source-wordcount: '1417'
ht-degree: 13%

---

# Flow Service API を使用した [!DNL Data Landing Zone] のAdobe Experience Platformへの接続

>[!IMPORTANT]
>
>このページは、Experience Platformの [!DNL Data Landing Zone] *ソース* コネクタに固有のページです。 [!DNL Data Landing Zone] *宛先* コネクタへの接続について詳しくは、[[!DNL Data Landing Zone]  宛先ドキュメントページ ](/help/destinations/catalog/cloud-storage/data-landing-zone.md) を参照してください。

[!DNL Data Landing Zone] は、ファイルをAdobe Experience Platformに取り込むための、セキュリティで保護されたクラウドベースのファイルストレージ機能です。 データは、7 日後に [!DNL Data Landing Zone] から自動的に削除されます。

このチュートリアルでは、[!DNL Data Landing Zone]API[[!DNL Flow Service]  を使用して ](https://www.adobe.io/experience-platform-apis/references/flow-service/) ソース接続を作成する手順を説明します。 このチュートリアルでは、[!DNL Data Landing Zone] ールの取得方法、資格情報の表示と更新方法に関する手順も説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

また、このチュートリアルでは [Experience Platform API の概要 ](../../../../../landing/api-guide.md) を読んで、Experience Platform API への認証方法と、ドキュメントに記載されている呼び出し例を解釈する方法についても確認する必要があります。

次の節では、[!DNL Data Landing Zone] API を使用して [!DNL Flow Service] ソース接続を正常に作成するために必要な追加情報を示しています。

## 使用可能なランディングゾーンの取得

>[!IMPORTANT]
>
>**[!UICONTROL Manage Sources]** API を使用して [!DNL Data Landing Zone] を取得するには、`type=user_drop_zone` のアクセス制御権限が必要です。 詳しくは、[ アクセス制御の概要 ](../../../../../access-control/home.md) を参照するか、製品管理者に問い合わせて、必要な権限を取得してください。

API を使用して [!DNL Data Landing Zone] にアクセスする最初の手順は、`/landingzone` API の [!DNL Connectors] エンドポイントに対してGET リクエストを実行し、その際にリクエストヘッダーの一部として `type=user_drop_zone` を指定することです。

**API 形式**

```http
GET /data/foundation/connectors/landingzone?type=user_drop_zone
```

| ヘッダー | 説明 |
| --- | --- |
| `user_drop_zone` | `user_drop_zone` タイプを使用すると、ランディングゾーンコンテナを、使用可能な他のタイプのコンテナと区別できます。 |

**リクエスト**

次のリクエストでは、既存のランディングゾーンが取得されます。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/landingzone?type=user_drop_zone' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' 
```

**応答**

プロバイダーに応じて、リクエストが成功すると、次の値が返されます。

>[!BEGINTABS]

>[!TAB Azure での応答 ]

```json
{
    "containerName": "dlz-user-container",
    "containerTTL": "7"
}
```

| プロパティ | 説明 |
| --- | --- |
| `containerName` | 取得したランディングゾーンの名前。 |
| `containerTTL` | ランディングゾーン内のデータに適用される有効期限（日数）。 特定のランディングゾーン内のはすべて、7 日後に削除されます。 |


>[!TAB AWSに対する回答 ]

```json
{
  "dlzPath": {
    "bucketName": "dlz-prod-sandboxName",
    "dlzFolder": "dlz-adf-connectors"
  },
  "dataTTL": {
    "timeUnit": "days",
    "timeQuantity": 7
  },
  "dlzProvider": "Amazon S3"
}
```

>[!ENDTABS]


## 資格情報 [!DNL Data Landing Zone] 取得

[!DNL Data Landing Zone] の資格情報を取得するには、`/credentials` API の [!DNL Connectors] エンドポイントに対してGET リクエストを実行します。

**API 形式**

```http
GET /data/foundation/connectors/landingzone/credentials?type=user_drop_zone
```

**リクエスト**

次のリクエスト例では、既存のランディングゾーンの資格情報を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=user_drop_zone' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

**応答**

プロバイダーに応じて、リクエストが成功すると、次の値が返されます。

>[!BEGINTABS]

>[!TAB Azure での応答 ]

```json
{
    "containerName": "dlz-user-container",
    "SASToken": "sv=2020-04-08&si=dlz-ed86a61d-201f-4b50-b10f-a1bf173066fd&sr=c&sp=racwdlm&sig=4yTba8voU3L0wlcLAv9mZLdZ7NlMahbfYYPTMkQ6ZGU%3D",
    "storageAccountName": "dlblobstore99hh25i3dflek",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-user-container?sv=2020-04-08&si=dlz-ed86a61d-201f-4b50-b10f-a1bf173066fd&sr=c&sp=racwdlm&sig=4yTba8voU3L0wlcLAv9mZLdZ7NlMahbfYYPTMkQ6ZGU%3D",
    "expiryDate": "2024-01-06"
}
```

| プロパティ | 説明 |
| --- | --- |
| `containerName` | [!DNL Data Landing Zone] の名前。 |
| `SASToken` | [!DNL Data Landing Zone] ーザーの共有アクセス署名トークン。 この文字列には、リクエストの認証に必要なすべての情報が含まれます。 |
| `storageAccountName` | ストレージアカウントの名前。 |
| `SASUri` | [!DNL Data Landing Zone] ーザーの共有アクセス署名 URI。 この文字列は、認証対象の [!DNL Data Landing Zone] への URI とそれに対応する SAS トークンの組み合わせです。 |
| `expiryDate` | SAS トークンの有効期限が切れる日付。 [!DNL Data Landing Zone] へのデータのアップロードにアプリケーションで引き続き使用するには、有効期限の前にトークンを更新する必要があります。 指定された有効期限の前にトークンを手動で更新しない場合、GET資格情報の呼び出しが実行されると、自動的に更新され、新しいトークンが提供されます。 |

>[!TAB AWSに対する回答 ]

```json
{
  "credentials": {
    "clientId": "example-client-id",
    "awsAccessKeyId": "example-access-key-id",
    "awsSecretAccessKey": "example-secret-access-key",
    "awsSessionToken": "example-session-token"
  },
  "dlzPath": {
    "bucketName": "dlz-prod-sandboxName",
    "dlzFolder": "user_drop_zone"
  },
  "dlzProvider": "Amazon S3",
  "expiryTime": 1735689599
}
```

| プロパティ | 説明 |
| --- | --- |
| `credentials.clientId` | AWSの [!DNL Data Landing Zone] のクライアント ID。 |
| `credentials.awsAccessKeyId` | AWSの [!DNL Data Landing Zone] ーザーのアクセスキー ID。 |
| `credentials.awsSecretAccessKey` | AWSでの [!DNL Data Landing Zone] の秘密アクセスキー。 |
| `credentials.awsSessionToken` | AWS セッショントークン。 |
| `dlzPath.bucketName` | AWS バケットの名前。 |
| `dlzPath.dlzFolder` | アクセスする [!DNL Data Landing Zone] フォルダー。 |
| `dlzProvider` | 使用している [!DNL Data Landing Zone] プロバイダー。 Amazonの場合は、[!DNL Amazon S3] になります。 |
| `expiryTime` | Unix 時間の有効期限。 |

>[!ENDTABS]

### API を使用した必須フィールドの取得

トークンを生成したら、以下のリクエスト例を使用して、プログラムで必須フィールドを取得できます。

>[!BEGINTABS]

>[!TAB Python]

```py
import requests
 
# API endpoint
url = "https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=user_drop_zone"
 
headers = {
    "Authorization": "{TOKEN}",
    "Content-Type": "application/json",
    "x-gw-ims-org-id": "{ORG_ID}",
    "x-api-key": "{API_KEY}"
}
 
# Send GET request to the API
response = requests.get(url, headers=headers)
 
# Check if the request was successful
if response.status_code == 200:
    # Parse the response as JSON (if applicable)
    data = response.json()
 
    # Print or work with the fetched data 
    print(" Sas Token:", data['SASToken'])
    print(" Container Name:",  data['containerName'])
    print("\n")
 
else:
    # Print an error message if the request failed
    print(f"Failed to fetch data. Status code: {response.status_code}")
    print(f"Response: {response.text}")
```

>[!TAB Java]


```java
package org.example;
 
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
 
import org.apache.http.HttpResponse;
import org.apache.http.client.ClientProtocolException;
import org.apache.http.client.methods.HttpGet;
import org.apache.http.impl.client.DefaultHttpClient;
 
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
 
public class Main {
    public static void main(String[] args) {
 
        ObjectMapper objectMapper = new ObjectMapper();
 
        try {
 
            DefaultHttpClient httpClient = new DefaultHttpClient();
            HttpGet getRequest = new HttpGet(
                "https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=user_drop_zone");
            getRequest.addHeader("accept", "application/json");
            getRequest.addHeader("Authorization","<TOKEN>");
            getRequest.addHeader("Content-Type", "application/json");
            getRequest.addHeader("x-gw-ims-org-id", "<ORG_ID>");
            getRequest.addHeader("x-api-key", "<API_KEY>");
 
            HttpResponse response = httpClient.execute(getRequest);
 
            if (response.getStatusLine().getStatusCode() != 200) {
                throw new RuntimeException("Failed : HTTP error code : "
                    + response.getStatusLine().getStatusCode());
            }
 
            final JsonNode jsonResponse = objectMapper.readTree(response.getEntity().getContent());
 
            System.out.println("\nOutput from API Response .... \n");
            System.out.printf("ContainerName: %s%n", jsonResponse.at("/containerName").textValue());
            System.out.printf("SASToken: %s%n", jsonResponse.at("/SASToken").textValue());
 
            httpClient.getConnectionManager().shutdown();
 
        } catch (ClientProtocolException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

>[!ENDTABS]


## 資格情報 [!DNL Data Landing Zone] 更新

`SASToken` API の `/credentials` エンドポイントに対して POST リクエストを実行することで、[!DNL Connectors] を更新できます。

**API 形式**

```http
POST /data/foundation/connectors/landingzone/credentials?type=user_drop_zone&action=refresh
```

| ヘッダー | 説明 |
| --- | --- |
| `user_drop_zone` | `user_drop_zone` タイプを使用すると、ランディングゾーンコンテナを、使用可能な他のタイプのコンテナと区別できます。 |
| `refresh` | `refresh` のアクションを使用すると、ランディングゾーンの資格情報をリセットし、新しい `SASToken` を自動的に生成できます。 |

**リクエスト**

次のリクエストは、ランディングゾーン資格情報を更新します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/connectors/landingzone/credentials?type=user_drop_zone&action=refresh' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

**応答**

次の応答は、`SASToken` および `SASUri` の更新された値を返します。

```json
{
    "containerName": "dlz-user-container",
    "SASToken": "sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D",
    "storageAccountName": "dlblobstore99hh25i3dflek",
    "SASUri": "https://dlblobstore99hh25i3dflek.blob.core.windows.net/dlz-user-container?sv=2020-04-08&si=dlz-9c4d03b8-a6ff-41be-9dcf-20123e717e99&sr=c&sp=racwdlm&sig=JbRMoDmFHQU4OWOpgrKdbZ1d%2BkvslO35%2FXTqBO%2FgbRA%3D",
    "expiryDate": "2024-01-06"
}
```

## ランディングゾーンのファイル構造と内容の探索

`connectionSpecs` API の [!DNL Flow Service] エンドポイントに対してGET リクエストを実行することで、ランディングゾーンのファイル構造と内容を調べることができます。

**API 形式**

```http
GET /connectionSpecs/{CONNECTION_SPEC_ID}/explore?objectType=root
```

| パラメーター | 説明 |
| --- | --- |
| `{CONNECTION_SPEC_ID}` | [!DNL Data Landing Zone] に対応する接続仕様 ID。 この修正済み ID は `26f526f2-58f4-4712-961d-e41bf1ccc0e8` です。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connectionSpecs/26f526f2-58f4-4712-961d-e41bf1ccc0e8/explore?objectType=root' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、クエリされたディレクトリ内にあるファイルとフォルダーの配列が返されます。 構造を検査するために次の手順でアップロードするファイルを指定する必要があるので、アップロードするファイルの `path` プロパティをメモします。

```json
[
    {
        "type": "file",
        "name": "account.csv",
        "path": "dlz-user-container/account.csv",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "file",
        "name": "data8.csv",
        "path": "dlz-user-container/data8.csv",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "folder",
        "name": "userdata1",
        "path": "dlz-user-container/userdata1/",
        "canPreview": false,
        "canFetchSchema": false
    }
]
```

## ランディングゾーンファイルの構造と内容をプレビュー

ランディングゾーン内のファイルの構造を調べるには、GET リクエストを実行し、その際にファイルのパスとタイプをクエリパラメーターとして指定します。

**API 形式**

```http
GET /connectionSpecs/{CONNECTION_SPEC_ID}/explore?objectType=file&object={OBJECT}&fileType={FILE_TYPE}&preview={PREVIEW}
```

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `{CONNECTION_SPEC_ID}` | [!DNL Data Landing Zone] に対応する接続仕様 ID。 この修正済み ID は `26f526f2-58f4-4712-961d-e41bf1ccc0e8` です。 |  |
| `{OBJECT_TYPE}` | アクセスするオブジェクトのタイプ。 | `file` |
| `{OBJECT}` | アクセスするオブジェクトのパスと名前。 | `dlz-user-container/data8.csv` |
| `{FILE_TYPE}` | ファイルのタイプ。 | <ul><li>`delimited`</li><li>`json`</li><li>`parquet`</li></ul> |
| `{PREVIEW}` | ファイルのプレビューがサポートされているかどうかを定義するブール値。 | </ul><li>`true`</li><li>`false`</li></ul> |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connectionSpecs/26f526f2-58f4-4712-961d-e41bf1ccc0e8/explore?objectType=file&object=dlz-user-container/data8.csv&fileType=delimited&preview=true' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、ファイル名とデータタイプを含む、クエリされたファイルの構造が返されます。

```json
{
    "format": "flat",
    "schema": {
        "columns": [
            {
                "name": "Id",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "FirstName",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "LastName",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "Email",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "Phone",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            }
        ]
    },
    "data": [
        {
            "Email": "rsmith@abc.com",
            "FirstName": "Richard",
            "Phone": "111111111",
            "Id": "12345",
            "LastName": "Smith"
        },
        {
            "Email": "morgan@bac.com",
            "FirstName": "Morgan",
            "Phone": "22222222222",
            "Id": "67890",
            "LastName": "Hart"
        }
    ]
}
```

### `determineProperties` を使用して、[!DNL Data Landing Zone] のファイルプロパティ情報を自動検出します

`determineProperties` パラメーターを使用すると、ソースの内容と構造を調べるためにGETを呼び出す際に、[!DNL Data Landing Zone] ージのファイルコンテンツのプロパティ情報を自動検出できます。

#### `determineProperties` の使用例

次の表は、`determineProperties` クエリパラメーターを使用する際や、ファイルに関する情報を手動で指定する際に発生する可能性がある様々なシナリオの概要を説明しています。

| `determineProperties` | `queryParams` | 応答 |
| --- | --- | --- |
| True | なし | `determineProperties` がクエリパラメーターとして指定されている場合、ファイルプロパティの検出が行われ、応答は、ファイルタイプ、圧縮タイプ、列の区切り文字に関する情報を含んだ新しい `properties` キーを返します。 |
| なし | True | ファイルタイプ、圧縮タイプ、列区切り文字の値が `queryParams` の一部として手動で指定された場合、これらの値がスキーマの生成に使用され、同じプロパティが応答の一部として返されます。 |
| True | True | 両方のオプションが同時に実行された場合は、エラーが返されます。 |
| なし | なし | 2 つのオプションのどちらも指定されていない場合、応答のプロパティを取得する方法がないため、エラーが返されます。 |

**API 形式**

```http
GET /connectionSpecs/{CONNECTION_SPEC_ID}/explore?objectType=file&object={OBJECT}&fileType={FILE_TYPE}&preview={PREVIEW}&determineProperties=true
```

| パラメーター | 説明 | 例 |
| --- | --- | --- |
| `determineProperties` | このクエリパラメーターを使用すると、[!DNL Flow Service] API でファイルタイプ、圧縮タイプ、列の区切り文字に関する情報など、ファイルのプロパティに関する情報を検出できます。 | `true` |

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs/26f526f2-58f4-4712-961d-e41bf1ccc0e8/explore?objectType=file&object=dlz-user-container/garageWeek/file1&preview=true&determineProperties=true' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、クエリされたファイルの構造（ファイル名とデータタイプを含む）と、`properties`、`fileType`、`compressionType` に関する情報を含む `columnDelimiter` キーが返されます。

+++自分をクリック

```json
{
    "properties": {
        "fileType": "delimited",
        "compressionType": "tarGzip",
        "columnDelimiter": "~"
    },
    "format": "flat",
    "schema": {
        "columns": [
            {
                "name": "id",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "firstName",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "lastName",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "email",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            },
            {
                "name": "birthday",
                "type": "string",
                "xdm": {
                    "type": "string"
                }
            }
        ]
    },
    "data": [
        {
            "birthday": "1313-0505-19731973",
            "firstName": "Yvonne",
            "lastName": "Thilda",
            "id": "100",
            "email": "Yvonne.Thilda@yopmail.com"
        },
        {
            "birthday": "1515-1212-19731973",
            "firstName": "Mary",
            "lastName": "Pillsbury",
            "id": "101",
            "email": "Mary.Pillsbury@yopmail.com"
        },
        {
            "birthday": "0505-1010-19751975",
            "firstName": "Corene",
            "lastName": "Joeann",
            "id": "102",
            "email": "Corene.Joeann@yopmail.com"
        },
        {
            "birthday": "2727-0303-19901990",
            "firstName": "Dari",
            "lastName": "Greenwald",
            "id": "103",
            "email": "Dari.Greenwald@yopmail.com"
        },
        {
            "birthday": "1717-0404-19651965",
            "firstName": "Lucy",
            "lastName": "Magdalen",
            "id": "199",
            "email": "Lucy.Magdalen@yopmail.com"
        }
    ]
}
```

+++

| プロパティ | 説明 |
| --- | --- |
| `properties.fileType` | クエリされたファイルの対応するファイルタイプ。 サポートされているファイルタイプは、`delimited`、`json`、`parquet` です。 |
| `properties.compressionType` | クエリされたファイルに使用される、対応する圧縮タイプ。 サポートされている圧縮タイプは次の通りです。 <ul><li>`bzip2`</li><li>`gzip`</li><li>`zipDeflate`</li><li>`tarGzip`</li><li>`tar`</li></ul> |
| `properties.columnDelimiter` | クエリされたファイルに使用される、対応する列区切り文字。 あらゆる単一の文字の値を、列の区切り文字として使用できます。デフォルト値はコンマ `(,)` です。 |


## ソース接続の作成

ソース接続は、データの取り込み元となる外部ソースへの接続を作成および管理します。ソース接続は、データソース、データ形式、データフローの作成に必要なソース接続 ID などの情報で構成されます。 ソース接続インスタンスは、テナントと組織に固有です。

ソース接続を作成するには、[!DNL Flow Service] API の `/sourceConnections` エンドポイントに POST リクエストを実行します。


**API 形式**

```http
POST /sourceConnections
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Data Landing Zone source connection",
        "data": {
            "format": "delimited"
        },
        "params": {
            "path": "dlz-user-container/data8.csv"
        },
        "connectionSpec": {
            "id": "26f526f2-58f4-4712-961d-e41bf1ccc0e8",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | [!DNL Data Landing Zone] ソース接続の名前。 |
| `data.format` | Experience Platformに取り込むデータの形式。 |
| `params.path` | Experience Platformに取り込むファイルへのパス。 |
| `connectionSpec.id` | [!DNL Data Landing Zone] に対応する接続仕様 ID。 この修正済み ID は `26f526f2-58f4-4712-961d-e41bf1ccc0e8` です。 |

**応答**

リクエストが成功した場合は、新しく作成されたソース接続の一意の ID（`id`）が返されます。この ID は、次のチュートリアルでデータフローを作成する際に必要です。

```json
{
    "id": "f5b46949-8c8d-4613-80cc-52c9c039e8b9",
    "etag": "\"1400d460-0000-0200-0000-613be3520000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Data Landing Zone] 資格情報を取得し、そのファイル構造を調べて、Experience Platformに取り込むファイルを見つけ、Experience Platformへのデータの取り込みを開始するソース接続を作成しました。 次のチュートリアルに進むことができます。ここでは、[API を使用してクラウドストレージデータをExperience Platformに取り込むためのデータフローを作成  [!DNL Flow Service]  する方法を説明し ](../../collect/cloud-storage.md) す。
