---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリパラメーターを使用したカタログデータのフィルター
topic: developer guide
translation-type: tm+mt
source-git-commit: 73a492ba887ddfe651e0a29aac376d82a7a1dcc4
workflow-type: tm+mt
source-wordcount: '2033'
ht-degree: 1%

---


# クエリパラメーターを使用した [!DNL Catalog] データのフィルター

この [!DNL Catalog Service] APIを使用すると、リクエストクエリパラメーターを使用して応答データをフィルタリングできます。 のベストプラクティスの一部 [!DNL Catalog] は、すべてのAPI呼び出しでフィルターを使用することです。これらのメソッドを使用すると、APIの負荷が軽減され、全体的なパフォーマンスが向上します。

このドキュメントでは、APIで [!DNL Catalog] オブジェクトをフィルタリングする最も一般的な方法について説明します。 この [APIの使い方について詳しくは、](getting-started.md) カタログ開発者ガイドを読みながらこのドキュメントを参照することをお勧めします [!DNL Catalog] 。 詳しくは、 [!DNL Catalog Service]カタログの概要を参照してください [](../home.md)。

## 返されるオブジェクトの制限

応答で返されるオブジェクトの数は、 `limit` クエリパラメーターによって制限されます。 [!DNL Catalog] 次の制限に従って、応答は自動的に課金されます。

* パラメーターが指定されていない場合、応答ペイロードあたりのオブジェクトの最大数は20です。 `limit`
* データセットクエリの場合、 `observableSchema` クエリパラメーターを使用してリクエスト `properties` が行われると、返されるデータセットの最大数は20です。
* 他のすべてのCatalogクエリのグローバル制限は、100オブジェクトです。
* 無効な `limit``limit=0`パラメーター（含む）は、適切な範囲を示す400レベルのエラー応答になります。
* クエリパラメータとして渡される制限やオフセットは、ヘッダとして渡される制限やオフセットよりも優先されます。

**API形式**

```http
GET /{OBJECT_TYPE}?limit={LIMIT}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得する [!DNL Catalog] オブジェクトの型。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{LIMIT}` | 返すオブジェクトの数を示す1 ～ 100の整数です。 |

**リクエスト**

次のリクエストでは、データセットのリストを取得しながら、応答を3つのオブジェクトに制限します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/catalog/dataSets?limit=3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、クエリパラメーターで示される数に制限されたデータセットのリストを返し `limit` ます。

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

## 表示プロパティの制限

パラメーターを使用して返されたオブジェクトの数をフィルタリングする場合でも、返されるオブジェクト自体には、実際に必要とする以上の情報が含まれる場合があります。 `limit` システムの負荷をさらに軽減するには、応答をフィルタリングして、必要なプロパティのみを含めることをお勧めします。

パラメーター `properties` フィルターは、指定されたプロパティのセットのみを返すようにオブジェクトに応答します。 このパラメーターは、1つ以上のプロパティを返すように設定できます。

この `properties` パラメーターは最上位レベルのオブジェクトプロパティのみを受け入れます。つまり、次のサンプルオブジェクトでは、、、およ `name`びのフィルターを適用できますが、には適用できません `description``subItem``sampleKey`。

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

**API形式**

```http
GET /{OBJECT_TYPE}?properties={PROPERTY}
GET /{OBJECT_TYPE}?properties={PROPERTY_1},{PROPERTY_2},{PROPERTY_3}
GET /{OBJECT_TYPE}/{OBJECT_ID}?properties={PROPERTY_1},{PROPERTY_2},{PROPERTY_3}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得する [!DNL Catalog] オブジェクトの型。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{PROPERTY}` | 応答本文に含める属性の名前。 |
| `{OBJECT_ID}` | 取得する特定の [!DNL Catalog] オブジェクトの固有な識別子。 |

**リクエスト**

次のリクエストは、データセットのリストを取得します。 パラメーターの下に指定されたプロパティ名のコンマ区切りリストは、応答で返されるプロパティを示します。 `properties` また、返されるデータセットの数を制限する `limit` パラメータも含まれます。 リクエストに `limit` パラメーターが含まれていない場合、応答には最大20個のオブジェクトが含まれます。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?limit=4&properties=name,schemaRef' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、要求されたプロパティのみが表示された [!DNL Catalog] オブジェクトのリストが返されます。

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

上記の回答に基づき、以下を推定できます。

* 要求されたプロパティがオブジェクトにない場合は、要求されたプロパティのみが表示されます。 (`Dataset1`)
* 要求されたプロパティがオブジェクトに含まれていない場合は、空のオブジェクトとして表示されます。 (`Dataset2`)
* データセットは、要求されたプロパティがそのプロパティを含み、値がない場合、そのプロパティを空のオブジェクトとして返すことがあります。 (`Dataset3`)
* それ以外の場合は、リクエストされたすべてのプロパティの完全な値がデータセットに表示されます。 (`Dataset4`)

## 応答リストのオフセット開始インデックス

クエリパラメーターは、0から始まる番号を使用して、指定した数だけ応答リストを前にオフセットします。 `start` 例えば、3番目にリストされ `start=2` たオブジェクトの開始に対する応答をオフセットします。

パラメーターとパラメーターが対になっていない場合、返されるオブジェクトの最大数は20です。 `start``limit`

**API形式**

```http
GET /{OBJECT_TYPE}?start={OFFSET}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得するCatalogオブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OFFSET}` | 応答をオフセットするオブジェクトの数を示す整数です。 |

**リクエスト**

次のリクエストは、データセットのリストを取得し、5番目のオブジェクト(`start=4`)にオフセットし、応答を2つの返されるデータセット(`limit=2`)に制限します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/catalog/dataSets?start=4&limit=2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

この応答には、2つの最上位レベルの項目(`limit=2`)が含まれるJSONオブジェクトが含まれ、各データセットとその詳細情報が含まれます（この例では詳細をまとめています）。 応答は4時間ずつ移動します(`start=4`)。つまり、表示されるデータセットは5番目と6番目です。

```json
{
    "Dataset5": {},
    "Dataset6": {}
}
```

## タグでフィルター

一部のCatalogオブジェクトでは、 `tags` 属性の使用がサポートされています。 タグを使用して、オブジェクトに情報を付加し、後でそのオブジェクトを取得できます。 使用するタグの選択方法と適用方法は、組織のプロセスに応じて異なります。

タグを使用する場合は、いくつかの制限が考慮されます。

* 現在タグをサポートしているCatalogオブジェクトは、データセット、バッチ、接続のみです。
* タグ名はIMS組織に固有です。
* アドビのプロセスでは、特定の動作に対してタグが使用される場合があります。 これらのタグの名前には、標準として「adobe」というプリフィックスが付けられます。 したがって、タグ名を宣言する際は、このような表記規則は避ける必要があります。
* 次のタグ名は、全体で使用するために予約されてい [!DNL Experience Platform]るので、貴社のタグ名として宣言することはできません。
   * `unifiedProfile`: このタグ名は、で取り込むデータセット用に予約されてい [!DNL Real-time Customer Profile](../../profile/home.md)ます。
   * `unifiedIdentity`: このタグ名は、で取り込むデータセット用に予約されてい [!DNL Identity Service](../../identity-service/home.md)ます。

次に、 `tags` プロパティを含むデータセットの例を示します。 このプロパティ内のタグは、キーと値のペアの形式をとります。各タグ値は、1つの文字列を含む配列として表示されます。

```json
{
    "5be1f2ecc73c1714ceba66e2": {
        "imsOrg": "{IMS_ORG}",
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

**API形式**

パラメーターの値は、 `tags` 形式を使用して、キーと値のペアの形式を取り `{TAG_NAME}:{TAG_VALUE}`ます。 複数のキーと値のペアは、カンマ区切りのリストの形式で指定できます。 複数のタグを指定する場合、AND関係が想定されます。

このパラメーターでは、タグ値にワイルドカード文字(`*`)を使用できます。 例えば、の検索文字列は、タグ値が「test」で始まる任意のオブジェクトを `test*` 返します。 ワイルドカードのみから成る検索文字列は、値に関係なく、特定のタグが含まれているかどうかに基づいてオブジェクトをフィルタリングする場合に使用できます。

```http
GET /{OBJECT_TYPE}?tags={TAG_NAME}:{TAG_VALUE}
GET /{OBJECT_TYPE}?tags={TAG_NAME_1}:{TAG_VALUE_1},{TAG_NAME_2}:{TAG_VALUE_2}
GET /{OBJECT_TYPE}?tags={TAG_NAME}:{TAG_VALUE}*
GET /{OBJECT_TYPE}?tags={TAG_NAME}:*
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得する [!DNL Catalog] オブジェクトの型。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`dataSets`</li></ul> |
| `{TAG_NAME}` | フィルターに使用するタグの名前。 |
| `{TAG_VALUE}` | フィルターに使用するタグの値。 ワイルドカード文字(`*`)をサポートします。 |

**リクエスト**

次のリクエストは、データセットのリストを取得し、特定の値を持つ1つのタグと、2つ目のタグが存在することでフィルタリングします。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?tags=sampleTag:123456,secondTag:* \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、「123456」と「AND」の値を含むデータセット `sampleTag` のリスト `secondTag` を返します。 制限も指定しない限り、応答には最大20個のオブジェクトが含まれます。

```json
{
    "5b67f4dd9f6e710000ea9da4": {
            "version": "1.0.2",
            "imsOrg": "{IMS_ORG}",
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
            "imsOrg": "{IMS_ORG}",
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

## 日付範囲でフィルター

APIの一部のエンドポイントには、幅のあるクエリを許可するクエリパラメーターがあります。ほとんどの場合、日付が使用されます。 [!DNL Catalog]

**API形式**

```http
GET /batches?createdAfter={TIMESTAMP_1}&createdBefore={TIMESTAMP_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{TIMESTAMP }` | UNIXエポック時間のdatetime整数。 |

**リクエスト**

次のリクエストは、2019年4月の月に作成されたバッチのリストを取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/batches?createdAfter=1554076800000&createdBefore=1556668799000' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答には、指定した日付範囲に該当する [!DNL Catalog] オブジェクトのリストが含まれます。 制限も指定しない限り、応答には最大20個のオブジェクトが含まれます。

```json
{
    "5b67f4dd9f6e710000ea9da4": {
            "version": "1.0.2",
            "imsOrg": "{IMS_ORG}",
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
            "imsOrg": "{IMS_ORG}",
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

## プロパティで並べ替え

クエリ `orderBy` パラメーターを使用すると、指定したプロパティ値に基づいて応答データを並べ替え（並べ替え）できます。 このパラメーターには、「方向」(昇順または降順`asc` の場合) `desc` と、その後にコロン(`:`)が続き、結果を並べ替えるプロパティが必要です。 方向を指定しない場合、デフォルトの方向は昇順です。

複数の並べ替えプロパティは、カンマで区切ったリストで指定できます。 最初の並べ替えプロパティで、そのプロパティに対して同じ値を持つ複数のオブジェクトが生成される場合は、2番目の並べ替えプロパティを使用して、一致するオブジェクトをさらに並べ替えます。

例えば、次のクエリを考えてみましょう。 `orderBy=name,desc:created`. 結果は、最初の並べ替えプロパティに基づいて昇順で並べ替えられ `name`ます。 複数のレコードが同じ `name` プロパティを共有する場合、一致するレコードは、2番目のsortingプロパティ()で並べ替えられ `created`ます。 返されたレコードが同じレコードを持たない場合 `name`、この `created` プロパティは並べ替えの要素になりません。


**API形式**

```http
GET /{OBJECT_TYPE}?orderBy=asc:{PROPERTY_NAME}
GET /{OBJECT_TYPE}?orderBy=desc:{PROPERTY_NAME}
GET /{OBJECT_TYPE}?orderBy={PROPERTY_NAME_1},desc:{PROPERTY_NAME_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得するCatalogオブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{PROPERTY_NAME}` | 結果の並べ替えに使用するプロパティの名前。 |

**リクエスト**

次の要求は、プロパティで並べ替えられたデータセットのリストを取得し `name` ます。 データセットが同じものを共有している場合 `name`、それらのデータセットは、その `updated` プロパティに基づいて降順に並べ替えられます。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?orderBy=name,desc:updated' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答には、パラメーターに従って並べ替えられた [!DNL Catalog] オブジェクトのリストが含まれ `orderBy` ます。 制限も指定しない限り、応答には最大20個のオブジェクトが含まれます。

```json
{
    "5b67f4dd9f6e710000ea9da4": {
            "version": "1.0.2",
            "imsOrg": "{IMS_ORG}",
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
            "imsOrg": "{IMS_ORG}",
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
            "imsOrg": "{IMS_ORG}",
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

## プロパティでフィルタ

[!DNL Catalog] プロパティでフィルタリングする方法は2つあり、以下の節で詳しく説明します。

* [単純なフィルターの使用](#using-simple-filters): 特定のプロパティが特定の値と一致するかどうかでフィルタします。
* [プロパティパラメーターの使用](#using-the-property-parameter): 条件付き式を使用して、プロパティが存在するかどうか、またはプロパティの値が別の指定した値や正規式と一致、近似、比較するかどうかを基にフィルタリングします。

### 単純なフィルターの使用 {#using-simple-filters}

単純なフィルターを使用すると、特定のプロパティ値に基づいて応答をフィルタリングできます。 単純なフィルターは、の形式をとり `{PROPERTY_NAME}={VALUE}`ます。

例えば、このクエリは、 `name=exampleName``name` プロパティに「exampleName」という値が含まれているオブジェクトのみを返します。 一方、このクエリは、 `name=!exampleName` プロパティが「exampleName `name` 」で **** ないオブジェクトのみを返します。

また、単純なフィルターでは、単一のプロパティに対して複数の値をクエリする機能もサポートされています。 複数の値が指定された場合、指定されたリスト内の値のい **ずれかに一致するプロパティを持つオブジェクトが返されます** 。 リストに `!` 文字をプリフィックスすると、指定したクエリにプロパティ値がな **いオブジェクト(例えば**`name=!exampleName,anotherName`)のみが返され、複数値のリストを反転できます。

**API形式**

```http
GET /{OBJECT_TYPE}?{PROPERTY_NAME}={VALUE}
GET /{OBJECT_TYPE}?{PROPERTY_NAME}=!{VALUE}
GET /{OBJECT_TYPE}?{PROPERTY_NAME}={VALUE_1},{VALUE_2},{VALUE_3}
GET /{OBJECT_TYPE}?{PROPERTY_NAME}=!{VALUE_1},{VALUE_2},{VALUE_3}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得する [!DNL Catalog] オブジェクトの型。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{PROPERTY_NAME}` | 値のフィルターに使用するプロパティの名前です。 |
| `{VALUE}` | 含める（または除外する）結果をクエリに応じて決定するプロパティ値です。 |

**リクエスト**

次のリクエストは、データセットのリストを取得します。データセットは、プ `name` ロパティの値が「exampleName」または「anotherName」であるデータセットのみが含まれるようにフィルタリングされます。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?name=exampleName,anotherName' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答には、「exampleName」または「anotherName」のデータセットを除くデータセットのリストが含ま `name` れます。 制限も指定しない限り、応答には最大20個のオブジェクトが含まれます。

```json
{
    "5b67f4dd9f6e710000ea9da4": {
            "version": "1.0.2",
            "imsOrg": "{IMS_ORG}",
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
            "imsOrg": "{IMS_ORG}",
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

### パラメーターの使用 `property` {#using-the-property-parameter}

この `property` クエリパラメーターは、単純なフィルターーよりも柔軟に、プロパティベースのフィルタリングを行えます。 プロパティに特定の値が含まれるかどうかに基づいてフィルタリングするだけでなく、この `property` 式では、他の比較演算子(「より大きい」(`>`)や「より小さい」(`<`)など)を使用したり、プロパティ値でフィルタリングしたりできます。 また、値に関係なく、プロパティが存在するかどうかでフィルタリングすることもできます。

この `property` パラメーターは最上位レベルのオブジェクトプロパティのみを受け入れます。つまり、次のサンプルオブジェクトでは、、、およ `name`びのプロパティでフィルタリングできますが、のプロパティではフィルタリングできま `description`せん `subItem``sampleKey`。

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

**API形式**

```http
GET /{OBJECT_TYPE}?property={CONDITION}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得する [!DNL Catalog] オブジェクトの型。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{CONDITION}` | クエリするプロパティとその値の評価方法を示す条件付き式。 以下に例を示します。 |

この `property` パラメーターの値は、複数の異なる種類の条件式をサポートします。 次の表に、サポートされる式の基本的な構文を示します。

| シンボル | 説明 | 例 |
| --- | --- | --- |
| (None) | プロパティ名に演算子を指定しないと、値に関係なく、プロパティが存在するオブジェクトのみが返されます。 | `property=name` |
| ! | パラメーターの値に「`!`」をプリフィックスすると、プロパティが `property` 存在しないオブジェクトのみが返され **ます** 。 | `property=!name` |
| ~ | チルダ(`~`)記号の後に指定した正規式とプロパティ値（文字列）が一致するオブジェクトのみを返します。 | `property=name~^example` |
| == | 重複と等しい記号(`==`)の後に指定した文字列と完全に一致するプロパティ値を持つオブジェクトのみを返します。 | `property=name==exampleName` |
| != | 等しくない記号( ****`!=`)の後に指定された文字列とプロパティ値が一致しないオブジェクトのみを返します。 | `property=name!=exampleName` |
| &lt; | プロパティ値が指定した金額より小さい（ただし、等しくない）オブジェクトのみを返します。 | `property=version<1.0.0` |
| &lt;= | プロパティ値が指定した金額より小さい（または等しい）オブジェクトのみを返します。 | `property=version<=1.0.0` |
| > | プロパティ値が指定した金額より大きい（ただし、等しくない）オブジェクトのみを返します。 | `property=version>1.0.0` |
| >= | プロパティ値が指定した金額より大きい（または等しい）オブジェクトのみを返します。 | `property=version>=1.0.0` |

>[!NOTE]
>
>この `name` プロパティでは、検索文字列全体またはワイルドカードの一部 `*`として、ワイルドカードの使用がサポートされています。 ワイルドカードは、検索文字列が値「test」と一致するよう `te*st` に、空の文字に一致します。 アスタリスクは2倍にすることでエスケープされる(`**`)。 検索文字列内の重複アスタリスクは、リテラル文字列としての1つのアスタリスクを表します。

**リクエスト**

次の要求は、1.0.3より大きいバージョン番号を持つデータセットを返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/catalog/dataSets?property=version>1.0.3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答には、バージョン番号が1.0.3を超えるデータセットのリストが含まれます。制限を指定しない限り、応答には最大20個のオブジェクトが含まれます。

```json
{
    "5b67f4dd9f6e710000ea9da4": {
            "version": "1.1.2",
            "imsOrg": "{IMS_ORG}",
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
            "imsOrg": "{IMS_ORG}",
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
            "imsOrg": "{IMS_ORG}",
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

## 複数のフィルターの結合

アンパサンド(`&`)を使用すると、複数のフィルターを1つのリクエストで結合できます。 リクエストに追加の条件が追加されると、AND関係が想定されます。

**API形式**

```http
GET /{OBJECT_TYPE}?{FILTER_1}={VALUE}&{FILTER_2}={VALUE}&{FILTER_3}={VALUE}
```
