---
keywords: Experience Platform；ホーム；人気のトピック；フィルター；フィルター；データのフィルター；データのフィルター；日付範囲
solution: Experience Platform
title: クエリパラメータを使用したカタログデータのフィルタリング
description: カタログサービス API を使用すると、応答データをリクエストパラメーターを使用してフィルタリングするクエリができます。カタログについてのベストプラクティスの一部は、すべての API 呼び出しでフィルターを使用することです。これらの呼び出しは、API の負荷を軽減し、全体的なパフォーマンスを改善するのに役立ちます。
exl-id: 0cdb5a7e-527b-46be-9ad8-5337c8dc72b7
source-git-commit: 74867f56ee13430cbfd9083a916b7167a9a24c01
workflow-type: tm+mt
source-wordcount: '2121'
ht-degree: 85%

---

# フィルター [!DNL Catalog] クエリーパラメーターを使用したデータ

この [!DNL Catalog Service] API を使用すると、応答データをリクエストクエリパラメーターを使用してフィルタリングできます。 のベストプラクティスの一部 [!DNL Catalog] は、すべての API 呼び出しでフィルターを使用します。API の負荷を軽減し、全体的なパフォーマンスを向上させるのに役立ちます。

このドキュメントでは、最も一般的なフィルタリング方法の概要を説明します [!DNL Catalog] オブジェクトを含める必要があります。 このドキュメントを参照しながら、 [カタログ開発者ガイド](getting-started.md) を使用して [!DNL Catalog] API 詳しくは、 [!DNL Catalog Service]を参照し、 [[!DNL Catalog] 概要](../home.md).

## 返されるオブジェクトの制限

`limit` クエリパラメーターは、応答で返されるオブジェクトの数を制限します。[!DNL Catalog] 応答は、設定された制限に従って自動的に課金されます。

* `limit` パラメーターが指定されていない場合、応答ペイロードあたりのオブジェクトの最大数は 20 です。
* データセットクエリの場合、`properties` クエリパラメーターを使用して `observableSchema` がリクエストされた場合、返されるデータセットの最大数は 20 です。
* その他のすべてのカタログクエリのグローバル制限は 100 オブジェクトです。
* 無効な `limit` パラメーター（`limit=0` を含む）を使用すると、適切な範囲を示す 400 レベルのエラー応答が発生します。
* クエリパラメータとして渡される制限またはオフセットは、ヘッダーとして渡される制限またはオフセットよりも優先されます

**API 形式**

```http
GET /{OBJECT_TYPE}?limit={LIMIT}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | のタイプ [!DNL Catalog] 取得するオブジェクト。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{LIMIT}` | 返すオブジェクトの数を 1 ～ 100 の範囲で示す整数。 |

**リクエスト**

次のリクエストでは、データセットのリストを取得し、応答を 3 つのオブジェクトに制限します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/catalog/dataSets?limit=3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、データセットのパラメーターを返します。この値は、`limit` クエリパラメーターで示される数に制限されます。

```json
{
    "5ba9452f7de80400007fc52a": {
        "name": "Sample Dataset 1",
        "description": "Description of dataset.",
        "files": "@/dataSets/5ba9452f7de80400007fc52a/views/5ba9452f7de80400007fc52b/files"
    },
    "5bb276b03a14440000971552": {
        "name": "Sample Dataset 2",
        "description": "Description of dataset.",
        "files": "@/dataSets/5bb276b03a14440000971552/views/5bb276b01250b012f9acc75b/files"
    },
    "5bceaa4c26c115000039b24b": {
        "name": "Sample Dataset 3"
    }
}
```

## 表示するプロパティの制限

`limit` パラメーターを使用して返されたオブジェクトの数をフィルタリングする場合でも、返されるオブジェクト自体には、実際に必要な情報よりも多くの情報が含まれることがあります。システムの負荷をさらに減らすには、ベストプラクティスは必要なプロパティのみを含めるように応答をフィルタリングすることです。

`properties`パラメーターフィルターは、指定されたプロパティのセットのみを返すようにオブジェクトに応答します。このパラメーターは、1 つ以上のプロパティを返すように設定できます。

`properties` パラメーターは、最上位レベルのオブジェクトプロパティのみを受け入れます。つまり、次のオブジェクト例では、`name`、`description`、`subItem` に対してフィルターを適用できますが、`sampleKey` に対してはできません。

```json
{
  "5ba9452f7de80400007fc52a": {
      "name": "Sample Dataset",
      "description": "Sample dataset containing important data",
      "subitem": {
          "sampleKey": "sampleValue"
      }
  }
}
```

**API 形式**

```http
GET /{OBJECT_TYPE}?properties={PROPERTY}
GET /{OBJECT_TYPE}?properties={PROPERTY_1},{PROPERTY_2},{PROPERTY_3}
GET /{OBJECT_TYPE}/{OBJECT_ID}?properties={PROPERTY_1},{PROPERTY_2},{PROPERTY_3}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | のタイプ [!DNL Catalog] 取得するオブジェクト。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{PROPERTY}` | 応答本文に含める属性の名前。 |
| `{OBJECT_ID}` | 特定の [!DNL Catalog] オブジェクトを取得中です。 |

**リクエスト**

次のリクエストは、データセットのリストを取得します。`properties` パラメーターの下に指定されたプロパティ名のコンマ区切りリストは、応答で返されるプロパティを示します。また、返されるデータセットの数を制限する `limit` パラメーターも含まれます。リクエストに `limit` パラメーターが含まれていない場合、応答には最大 20 個のオブジェクトが含まれます。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?limit=4&properties=name,schemaRef' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、 [!DNL Catalog] 要求されたプロパティのみを持つオブジェクトが表示されます。

```json
{
    "Dataset1": {
        "name": "Dataset 1",
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/bc82c518380478b59a95c63e0f843121",
            "contentType": "application/vnd.adobe.xed+json;version=1"
        }
    },
    "Dataset2": {},
    "Dataset3": {
        "name": {},
    },
    "Dataset4": {
        "name": "Dataset 4",
        "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/142afb78d8b368a5ba97a6cc8fc7e033",
            "contentType": "application/vnd.adobe.xed+json;version=1"
        }
    }
}
```

上記の回答に基づいて、以下を推論できます。

* リクエストされたプロパティがオブジェクトにない場合は、そのオブジェクトに含まれているリクエストされたプロパティのみが表示されます（`Dataset1`）。
* リクエストされたプロパティが 1 つもオブジェクトに含まれていない場合は、空のオブジェクトとして表示されます（`Dataset2`）。
* データセットは、リクエストされたプロパティがそのプロパティを含むが値がない場合、そのプロパティを空のオブジェクトとして返すことがあります（`Dataset3`）。
* それ以外の場合は、リクエストされたすべてのプロパティの完全な値がデータセットに表示されます（`Dataset4`）。

>[!NOTE]
>
>内 `schemaRef` プロパティのバージョン番号は、各データセットの最新のマイナーバージョンを示します。 詳しくは、XDM API ガイドの[スキーマのバージョン管理](../../xdm/api/getting-started.md#versioning)の節を参照してください。

## 応答リストのオフセット開始インデックス

`start` クエリパラメーターは、0 から始まる番号付けを使用して、応答リストを指定された数だけ前方にオフセットします。例えば、`start=2` は、リストされた 3 番目のオブジェクトで開始するように応答をオフセットします。

`start` パラメーターと `limit` パラメーターがペアでない場合、返されるオブジェクトの最大数は 20 です。

**API 形式**

```http
GET /{OBJECT_TYPE}?start={OFFSET}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得するカタログオブジェクトのタイプ。有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OFFSET}` | 応答をオフセットするオブジェクトの数を示す整数。 |

**リクエスト**

次のリクエストは、データセットのリストを取得し、5 番目のオブジェクト（`start=4`）にオフセットし、応答を 2 つの戻りデータセット（`limit=2`）に制限します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/catalog/dataSets?start=4&limit=2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答には、各データセットに対して 1 つずつ、2 つの最上位項目（`limit=2`）とその詳細（この例で詳細をまとめたもの）を含む JSON オブジェクトが含まれます。応答が 4 ずつシフトされます（`start=4`）。つまり、表示されるデータセットは時系列で 5 と 6 です。

```json
{
    "Dataset5": {},
    "Dataset6": {}
}
```

## タグによるフィルター

一部のカタログオブジェクトは、`tags` 属性の使用をサポートしています。タグは、オブジェクトに情報を付加し、後でそのオブジェクトを取得するために使用できます。使用するタグと適用方法の選択は、組織のプロセスによって異なります。

タグを使用する場合は、いくつかの制限が考慮されます。

* 現在タグをサポートしているカタログオブジェクトは、データセット、バッチ、接続のみです。
* タグ名は IMS 組織に固有です。
* アドビのプロセスは、特定の動作にタグを活用する場合があります。これらのタグの名前には、標準として「adobe」というプリフィックスが付けられます。したがって、タグ名を宣言する際は、このような規則を避ける必要があります。
* 次のタグ名は、で使用するために予約されています [!DNL Experience Platform]のタグ名として宣言することはできません。
   * `unifiedProfile`:このタグ名は、が取り込むデータセット用に予約されています。 [[!DNL Real-Time Customer Profile]](../../profile/home.md).
   * `unifiedIdentity`:このタグ名は、が取り込むデータセット用に予約されています。 [[!DNL Identity Service]](../../identity-service/home.md).

次に、`tags` プロパティを含むデータセットの例を示します。このプロパティ内のタグは、キーと値のペアの形式をとり、各タグ値は 1 つの文字列を含む配列として表示されます。

```json
{
    "5be1f2ecc73c1714ceba66e2": {
        "imsOrg": "{ORG_ID}",
        "tags": {
            "sampleTag": [
                "123456"
            ],
            "secondTag": [
                "sample_tag_value"
            ]
        },
        "name": "Sample Dataset",
        "description": "Same dataset containing sample data.",
        "dule": {
            "identity": [
                "I1"
            ]
        },
        "statsCache": {},
        "state": "DRAFT",
        "lastBatchId": "ca12b29612bf4052872edad59573703c",
        "lastBatchStatus": "success",
        "lastSuccessfulBatch": "ca12b29612bf4052872edad59573703c",
        "namespace": "{NAMESPACE}",
        "createdUser": "{CREATED_USER}",
        "createdClient": "{CREATED_CLIENT}",
        "updatedUser": "{UPDATED_USER}",
        "version": "1.0.0",
        "created": 1541534444286,
        "updated": 1541534444286
    }
}
```

**API 形式**

`tags` パラメーターの値は、`{TAG_NAME}:{TAG_VALUE}` 形式を使用して、キーと値のペアの形式を取ります。複数のキーと値のペアは、コンマ区切りのリスト形式で提供できます。複数のタグを指定する場合、AND 関係が想定されます。

このパラメーターでは、タグ値にワイルドカード文字（`*`）を使用できます。例えば、`test*` の検索文字列は、タグ値が「test」で始まる任意のオブジェクトを返します。ワイルドカードのみで構成される検索文字列を使用すると、値に関係なく、特定のタグが含まれているかどうかに基づいてオブジェクトをフィルタリングできます。

```http
GET /{OBJECT_TYPE}?tags={TAG_NAME}:{TAG_VALUE}
GET /{OBJECT_TYPE}?tags={TAG_NAME_1}:{TAG_VALUE_1},{TAG_NAME_2}:{TAG_VALUE_2}
GET /{OBJECT_TYPE}?tags={TAG_NAME}:{TAG_VALUE}*
GET /{OBJECT_TYPE}?tags={TAG_NAME}:*
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | のタイプ [!DNL Catalog] 取得するオブジェクト。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`dataSets`</li></ul> |
| `{TAG_NAME}` | フィルターに使用するタグの名前。 |
| `{TAG_VALUE}` | フィルターに使用するタグの値。ワイルドカード文字（`*`）をサポートします。 |

**リクエスト**

次のリクエストは、データセットのリストを取得し、特定の値を持つ 1 つのタグと、2 番目のタグが存在することでフィルタリングします。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?tags=sampleTag:123456,secondTag:* \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、値が「123456」である `sampleTag`、および、任意の値を持つ `secondTag` を含むデータセットのリストを返します。制限も指定しない限り、応答には最大 20 個のオブジェクトが含まれます。

```json
{
    "5b67f4dd9f6e710000ea9da4": {
            "version": "1.0.2",
            "imsOrg": "{ORG_ID}",
            "name": "Example Dataset 1",
            "created": 1533539550237,
            "updated": 1533539552416,
            "createdClient": "{API_KEY}",
            "createdUser": "{USER_ID}",
            "updatedUser": "{USER_ID}",
            "tags": {
                "sampleTag": [
                    "123456"
                ],
                "secondTag": [
                    "Example tag value"
                ]
            },
            "dule": {},
            "statsCache": {}
    },
    "5b1e3c867e6d2600003d5b49": {
            "version": "1.0.0",
            "imsOrg": "{ORG_ID}",
            "name": "Example Dataset 2",
            "created": 1533539550237,
            "updated": 1533539552416,
            "createdClient": "{API_KEY}",
            "createdUser": "{USER_ID}",
            "updatedUser": "{USER_ID}",
            "tags": {
                "sampleTag": [
                    "123456"
                ],
                "secondTag": [
                    "A different tag value"
                ],
                "anotherTag": [
                    "2.0"
                ]
            },
            "dule": {},
            "statsCache": {}
    }
}
```

## 日付範囲によるフィルター

の一部のエンドポイント [!DNL Catalog] API には、幅のあるクエリを許可するクエリパラメーターがあり、ほとんどの場合は日付が使用されます。

**API 形式**

```http
GET /batches?createdAfter={TIMESTAMP_1}&createdBefore={TIMESTAMP_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{TIMESTAMP }` | UNIX エポック時間の datetime 整数。 |

**リクエスト**

次のリクエストは、2019 年 4 月に作成されたバッチのリストを取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/batches?createdAfter=1554076800000&createdBefore=1556668799000' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、 [!DNL Catalog] 指定した日付範囲に該当するオブジェクト。 制限も指定しない限り、応答には最大 20 個のオブジェクトが含まれます。

```json
{
    "5b67f4dd9f6e710000ea9da4": {
            "version": "1.0.2",
            "imsOrg": "{ORG_ID}",
            "name": "Example Dataset 1",
            "created": 1554930967705,
            "updated": 1554931119718,
            "createdClient": "{API_KEY}",
            "createdUser": "{USER_ID}",
            "updatedUser": "{USER_ID}",
            "dule": {},
            "statsCache": {}
    },
    "5b1e3c867e6d2600003d5b49": {
            "version": "1.0.0",
            "imsOrg": "{ORG_ID}",
            "name": "Example Dataset 2",
            "created": 1554974386247,
            "updated": 1554974386268,
            "createdClient": "{API_KEY}",
            "createdUser": "{USER_ID}",
            "updatedUser": "{USER_ID}",
            "dule": {},
            "statsCache": {}
    }
}
```

## プロパティによる並べ替え

`orderBy` クエリパラメーターを使用すると、指定したプロパティ値に基づいて応答データを並べ替え（順序付け）できます。このパラメーターには、「方向」(昇順：`asc`、降順：`desc`)、コロン（`:`）、結果を並べ替えるプロパティの順に並べ替える必要があります。方向を指定しない場合、デフォルトの方向は昇順です。

複数の並べ替えプロパティは、コンマで区切ったリストで指定できます。最初の並べ替えプロパティで、そのプロパティが同じ値を含む複数のオブジェクトが生成される場合、2 番目の並べ替えプロパティを使用して、一致するオブジェクトをさらに並べ替えます。

例えば、クエリ「`orderBy=name,desc:created`」について考えます。結果は、最初の並べ替えプロパティ（`name`）に基づいて昇順で並べ替えられます。複数のレコードが同じ `name` プロパティを共有している場合、一致するレコードは 2 番目の並べ替えプロパティ（`created`）で並べ替えられます。返されたレコードが同じ `name` を共有しない場合、`created` プロパティは並べ替えで考慮されません。


**API 形式**

```http
GET /{OBJECT_TYPE}?orderBy=asc:{PROPERTY_NAME}
GET /{OBJECT_TYPE}?orderBy=desc:{PROPERTY_NAME}
GET /{OBJECT_TYPE}?orderBy={PROPERTY_NAME_1},desc:{PROPERTY_NAME_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得するカタログオブジェクトのタイプ。有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{PROPERTY_NAME}` | 結果の並べ替えに使用するプロパティの名前。 |

**リクエスト**

次のリクエストは、`name` プロパティで並べ替えられたリストセットのデータセットを取得します。同じ `name` を共有するデータセットがある場合、そのデータセットは `updated` プロパティによって降順で並べ替えられます。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?orderBy=name,desc:updated' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、 [!DNL Catalog] 次に従って並べ替えられるオブジェクト `orderBy` パラメーター。 制限も指定しない限り、応答には最大 20 個のオブジェクトが含まれます。

```json
{
    "5b67f4dd9f6e710000ea9da4": {
            "version": "1.0.2",
            "imsOrg": "{ORG_ID}",
            "name": "0405",
            "created": 1554930967705,
            "updated": 1554931119718,
            "createdClient": "{API_KEY}",
            "createdUser": "{USER_ID}",
            "updatedUser": "{USER_ID}",
            "dule": {},
            "statsCache": {}
    },
    "5b1e3c867e6d2600003d5b49": {
            "version": "1.0.3",
            "imsOrg": "{ORG_ID}",
            "name": "AAM Dataset",
            "created": 1554974386247,
            "updated": 1554974386268,
            "createdClient": "{API_KEY}",
            "createdUser": "{USER_ID}",
            "updatedUser": "{USER_ID}",
            "dule": {},
            "statsCache": {}
    },
    "5cd3a129ec106214b722a939": {
            "version": "1.0.2",
            "imsOrg": "{ORG_ID}",
            "name": "AAM Dataset",
            "created": 1554028394852,
            "updated": 1554130582960,
            "createdClient": "{API_KEY}",
            "createdUser": "{USER_ID}",
            "updatedUser": "{USER_ID}",
            "dule": {},
            "statsCache": {}
    }
}
```

## プロパティによるフィルター

[!DNL Catalog] には、プロパティでフィルタリングする 2 つの方法があります。この方法については、以降の節で詳しく説明します。

* [単純なフィルターの使用](#using-simple-filters)：特定のプロパティが特定の値と一致するかどうかでフィルタリングします。
* [プロパティパラメーターの使用](#using-the-property-parameter)：条件付き式を使用して、プロパティが存在するかどうか、またはプロパティの値が別の指定した値や正規式と一致、近似、比較するかどうかを基にフィルタリングします。

### 単純なフィルターの使用 {#using-simple-filters}

単純なフィルターを使用すると、特定のプロパティ値に基づいて応答をフィルタリングできます。単純なフィルターは、`{PROPERTY_NAME}={VALUE}` の形式をとります。

例えば、クエリ「`name=exampleName`」は、`name` プロパティに「exampleName」という値が含まれているオブジェクトのみを返します。これに対し、クエリ「`name=!exampleName`」は、`name` プロパティが「exampleName」で&#x200B;**ない**&#x200B;オブジェクトのみを返します。

また、単純なフィルターは、単一のプロパティに対して複数の値をクエリする機能をサポートしています。複数の値が指定された場合、応答は指定されたリスト内の値の&#x200B;**いずれかに**&#x200B;プロパティが一致するオブジェクトを返します。複数値のクエリを反転するには、リストの冒頭に `!` 文字を追加し、指定したリストにプロパティ値が含まれて&#x200B;**いない**（例えば、`name=!exampleName,anotherName`）オブジェクトのみを返します。

**API 形式**

```http
GET /{OBJECT_TYPE}?{PROPERTY_NAME}={VALUE}
GET /{OBJECT_TYPE}?{PROPERTY_NAME}=!{VALUE}
GET /{OBJECT_TYPE}?{PROPERTY_NAME}={VALUE_1},{VALUE_2},{VALUE_3}
GET /{OBJECT_TYPE}?{PROPERTY_NAME}=!{VALUE_1},{VALUE_2},{VALUE_3}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | のタイプ [!DNL Catalog] 取得するオブジェクト。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{PROPERTY_NAME}` | 値のフィルターに使用するプロパティの名前です。 |
| `{VALUE}` | クエリに応じて含めるまたは除外する結果を決定するプロパティ値。 |

**リクエスト**

次のリクエストは、データセットのリストを取得し、`name` プロパティの値が「exampleName」または「anotherName」であるデータセットのみを含めるようにフィルタリングします。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?name=exampleName,anotherName' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、`name` が「exampleName」または「anotherName」のデータセットを除く、データセットのリストを含みます。制限も指定しない限り、応答には最大 20 個のオブジェクトが含まれます。

```json
{
    "5b67f4dd9f6e710000ea9da4": {
            "version": "1.0.2",
            "imsOrg": "{ORG_ID}",
            "name": "exampleName",
            "created": 1554930967705,
            "updated": 1554931119718,
            "createdClient": "{API_KEY}",
            "createdUser": "{USER_ID}",
            "updatedUser": "{USER_ID}",
            "dule": {},
            "statsCache": {}
    },
    "5b1e3c867e6d2600003d5b49": {
            "version": "1.0.3",
            "imsOrg": "{ORG_ID}",
            "name": "anotherName",
            "created": 1554974386247,
            "updated": 1554974386268,
            "createdClient": "{API_KEY}",
            "createdUser": "{USER_ID}",
            "updatedUser": "{USER_ID}",
            "dule": {},
            "statsCache": {}
    }
}
```

### `property` パラメータの使用  {#using-the-property-parameter}

`property` クエリパラメーターは、単純なパラメーターよりも柔軟にプロパティベースのフィルタリングをおこなえます。プロパティに特定の値が含まれるかどうかに基づいてフィルタリングする以外に、`property` パラメーターは他の比較演算子（「より大きい」（`>`）や「より小さい」（`<`）など）を正規式としてプロパティ値によるフィルタリングに使用できます。また、値に関係なく、プロパティが存在するかどうかでフィルタリングすることもできます。

`property` パラメーターは、最上位レベルのオブジェクトプロパティのみを受け入れます。つまり、次のオブジェクト例では、`name`、`description`、`subItem` プロパティでフィルタリングできますが、`sampleKey` プロパティではフィルタリングできません。

```json
{
  "5ba9452f7de80400007fc52a": {
      "name": "Sample Dataset",
      "description": "Sample dataset containing important data",
      "subitem": {
          "sampleKey": "sampleValue"
      }
  }
}
```

**API 形式**

```http
GET /{OBJECT_TYPE}?property={CONDITION}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | のタイプ [!DNL Catalog] 取得するオブジェクト。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{CONDITION}` | クエリするプロパティ、およびその値の評価方法を示す条件式。以下に例を示します。 |

`property` パラメーターの値は、複数の異なる種類の条件付き式をサポートします。次の表に、サポートされる式の基本構文を示します。

| シンボル | 説明 | 例 |
| --- | --- | --- |
| なし | プロパティ名を演算子なしで指定すると、値に関係なく、プロパティが存在するオブジェクトのみが返されます。 | `property=name` |
| ! | `property` パラメーターの値に「`!`」を接頭辞として付けると、プロパティが存在&#x200B;**しない**&#x200B;オブジェクトのみが返されます。 | `property=!name` |
| ~ | チルダ（`~`）記号の後に指定された正規式に一致するプロパティ値（文字列）を持つオブジェクトのみを返します。 | `property=name~^example` |
| == | プロパティ値が`==`記号の後に指定した文字列と完全に一致する重複のみを返します。 | `property=name==exampleName` |
| ! = | プロパティ値が不等号（`!=`）の後に指定された文字列と一致&#x200B;**しない**&#x200B;オブジェクトのみを返します。 | `property=name!=exampleName` |
| &lt; | プロパティ値が指定した値未満のオブジェクトのみを返します。 | `property=version<1.0.0` |
| &lt;= | プロパティ値が指定した値以下のオブジェクトのみを返します。 | `property=version<=1.0.0` |
| > | プロパティ値が指定した値より大きい（ただし、等しくない）オブジェクトのみを返します。 | `property=version>1.0.0` |
| >= | プロパティ値が指定した値以上のオブジェクトのみを返します。 | `property=version>=1.0.0` |

>[!NOTE]
>
> `name` プロパティでは、検索文字列全体またはその一部として、ワイルドカード `*` の使用がサポートされています。ワイルドカードは空の文字に一致するので、検索文字列 `te*st` は「test」に一致します。アスタリスクは繰り返す（`**`）ことでエスケープされます。検索文字列内のダブルアスタリスクは、リテラル文字列として 1 つのアスタリスクを表します。

**リクエスト**

次のリクエストは、1.0.3 より大きいバージョン番号を持つデータセットを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/catalog/dataSets?property=version>1.0.3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、バージョン番号が 1.0.3 よりも大きいデータセットのリストを含みます。制限が指定されていない限り、応答には最大 20 個のオブジェクトが含まれます。

```json
{
    "5b67f4dd9f6e710000ea9da4": {
            "version": "1.1.2",
            "imsOrg": "{ORG_ID}",
            "name": "sampleDataset",
            "created": 1554930967705,
            "updated": 1554931119718,
            "createdClient": "{API_KEY}",
            "createdUser": "{USER_ID}",
            "updatedUser": "{USER_ID}",
            "dule": {},
            "statsCache": {}
    },
    "5b1e3c867e6d2600003d5b49": {
            "version": "1.0.6",
            "imsOrg": "{ORG_ID}",
            "name": "exampleDataset",
            "created": 1554974386247,
            "updated": 1554974386268,
            "createdClient": "{API_KEY}",
            "createdUser": "{USER_ID}",
            "updatedUser": "{USER_ID}",
            "dule": {},
            "statsCache": {}
    },
    "5cd3a129ec106214b722a939": {
            "version": "1.0.4",
            "imsOrg": "{ORG_ID}",
            "name": "anotherDataset",
            "created": 1554028394852,
            "updated": 1554130582960,
            "createdClient": "{API_KEY}",
            "createdUser": "{USER_ID}",
            "updatedUser": "{USER_ID}",
            "dule": {},
            "statsCache": {}
    }
}
```

## 複数のフィルターの組み合わせ

アンパサンド（`&`）を使用すると、複数のフィルターを 1 つのリクエストで結合できます。リクエストに追加の条件が追加されると、AND 関係が想定されます。

**API 形式**

```http
GET /{OBJECT_TYPE}?{FILTER_1}={VALUE}&{FILTER_2}={VALUE}&{FILTER_3}={VALUE}
```
