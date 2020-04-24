---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: カタログデータをフィルタするクエリパラメータ
topic: developer guide
translation-type: tm+mt
source-git-commit: 5699022d1f18773c81a0a36d4593393764cb771a

---


# カタログデータをフィルタするクエリパラメータ

Catalog Service APIを使用すると、応答データをリクエストパラメーターを使用してフィルタリングするクエリができます。 カタログのベストプラクティスの一部は、すべてのAPI呼び出しでフィルターを使用することです。これらの呼び出しは、APIの負荷を軽減し、全体的なパフォーマンスを改善するのに役立ちます。

このドキュメントでは、APIでカタログオブジェクトをフィルタリングする最も一般的な方法について説明します。 カタログAPIの操作方法について詳しくは、 [Catalog開発者ガイドを読みながら](getting-started.md) 、このドキュメントを参照することをお勧めします。 カタログサービスの一般情報について詳しくは、カタログの概要を参 [照してくださ](../home.md)い。

## 返されるオブジェクトの制限

クエリパ `limit` ラメーターは、応答で返されるオブジェクトの数を制限します。 カタログの回答は、設定された制限に従って自動的に課金されます。

* パラメータ `limit` ーが指定されていない場合、応答ペイロードあたりのオブジェクトの最大数は20です。
* データセットクエリの場合、 `observableSchema` クエリパラメーターを使用して要求さ `properties` れた場合、返されるデータセットの最大数は20です。
* その他のすべてのカタログクエリのグローバル制限は100オブジェクトです。
* 無効なパ `limit` ラメータ( `limit=0`を含む)を使用すると、適切な範囲を示す400レベルのエラー応答が発生します。
* ヘッダパラメータとして渡される制限またはクエリオフセットは、ヘッダとして渡される制限またはオフセットよりも優先されます。

**API形式**

```http
GET /{OBJECT_TYPE}?limit={LIMIT}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得するCatalogオブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{LIMIT}` | 返すオブジェクトの数を1 ～ 100の範囲で示す整数です。 |

**リクエスト**

次のリクエストでは、データセットのリストを取得し、応答を3つのオブジェクトに制限します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/catalog/dataSets?limit=3 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、リストセットのパラメータを返します。この値は、クエリパラメータで示される数に制限さ `limit` れます。

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

パラメータを使用して返されたオブジェクトの数をフィルタリングす `limit` る場合でも、返されるオブジェクト自体には、実際に必要な情報よりも多くの情報が含まれることがあります。 システムの負荷をさらに減らすには、必要なプロパティのみを含めるように応答をフィルタリングすることをお勧めします。

パラメータ `properties` ーフィルターは、指定されたプロパティのセットのみを返すようにオブジェクトに応答します。 このパラメーターは、1つ以上のプロパティを返すように設定できます。

このパ `properties` ラメーターは、最上位レベルのオブジェクトプロパティのみを受け入れます。つまり、次のサンプルオブジェクトでは、、、およびに対して `name`フィルターを適用できますが、 `description`に対し `subItem`ては `sampleKey`適用できません。

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
| `{OBJECT_TYPE}` | 取得するCatalogオブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{PROPERTY}` | 応答本文に含める属性の名前。 |
| `{OBJECT_ID}` | 取得する特定のCatalogオブジェクトの一意の識別子。 |

**リクエスト**

次のリクエストは、データセットのリストを取得します。 パラメーターの下に指定されたプロパティ名のコンマ区切りリスト `properties` ーは、応答で返されるプロパティを示します。 また、返 `limit` されるデータセットの数を制限するパラメータも含まれます。 リクエストにパラメーターが含まれていな `limit` い場合、応答には最大20個のオブジェクトが含まれます。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?limit=4&properties=name,schemaRef' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常に応答すると、要求されたリストのみが表示されたCatalogオブジェクトのプロパティが返されます。

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

上記の回答に基づいて、次の事項を推論できる。

* 要求されたプロパティがオブジェクトにない場合は、そのオブジェクトに含まれている要求されたプロパティのみが表示されます。(`Dataset1`)
* 要求されたプロパティがオブジェクトに含まれていない場合は、空のオブジェクトとして表示されます。(`Dataset2`)
* データセットは、要求されたプロパティがそのプロパティを含むが値がない場合、そのプロパティを空のオブジェクトとして返すことがあります。(`Dataset3`)
* それ以外の場合は、リクエストされたすべてのプロパティの完全な値がデータセットに表示されます。(`Dataset4`)

## 応答リストのオフセット開始インデックス

クエリパ `start` ラメーターは、0から始まる番号を使用して、応答リストを指定された数だけ前方にオフセットします。 例えば、3番目にリ `start=2` ストされたオブジェクトの開始に対する応答をオフセットします。

パラメータ `start` ーとパラメーターがペアで `limit` ない場合、返されるオブジェクトの最大数は20です。

**API形式**

```http
GET /{OBJECT_TYPE}?start={OFFSET}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得するCatalogオブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{OFFSET}` | 応答をオフセットするオブジェクトの数を示す整数。 |

**リクエスト**

次のリクエストは、データセットのリストを取得し、5番目のオブジェクト(`start=4`)にオフセットし、応答を2つの戻りデータセット(`limit=2`)に制限します。

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/catalog/dataSets?start=4&limit=2 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答には、各データセットに対して1つずつ、2つの最上位項目(`limit=2`)とその詳細（この例で詳細をまとめたもの）を含むJSONオブジェクトが含まれます。 応答が4ずつシフトされます(`start=4`)。つまり、表示されるデータセットは時系列で5と6です。

```json
{
    "Dataset5": {},
    "Dataset6": {}
}
```

## タグでフィルター

一部のCatalogオブジェクトは、属性の使用をサポートし `tags` ています。 タグは、オブジェクトに情報を付加し、後でそのオブジェクトを取得するために使用できます。 使用するタグと適用方法の選択は、組織のプロセスによって異なります。

タグを使用する場合は、いくつかの制限事項を考慮する必要があります。

* 現在タグをサポートしているCatalogオブジェクトは、データセット、バッチ、接続のみです。
* タグ名はIMS組織に固有です。
* アドビのプロセスは、特定の動作にタグを使用する場合があります。 これらのタグの名前には、標準として「adobe」というプリフィックスが付けられます。 したがって、タグ名を宣言する際は、このような規則を避ける必要があります。
* 次のタグ名は、エクスペリエンスプラットフォーム全体で使用するために予約されているので、組織のタグ名として宣言することはできません。
   * `unifiedProfile`:このタグ名は、リアルタイムの顧客プロファイルが取り込むデータセ [ット用に予約されています](../../profile/home.md)。
   * `unifiedIdentity`:このタグ名は、 [Identity Serviceが取り込むデータセット用に予約されています](../../identity-service/home.md)。

次に、プロパティを含むデータセットの例を示し `tags` ます。 このプロパティ内のタグは、キーと値のペアの形式をとり、各タグ値は1つの文字列を含む配列として表示されます。

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

パラメーターの `tags` 値は、形式を使用して、キーと値のペアの形式を取りま `{TAG_NAME}:{TAG_VALUE}`す。 複数のキーと値のペアは、コンマ区切りの形式で提供できます。リスト 複数のタグを指定する場合、AND関係が想定されます。

このパラメーターでは、タグ値にワイルドカ`*`ード文字()を使用できます。 例えば、の検索文字列は、タグ値が「test」 `test*` で始まる任意のオブジェクトを返します。 ワイルドカードのみで構成される検索文字列を使用して、値に関係なく、特定のタグが含まれているかどうかに基づいてオブジェクトをフィルタリングできます。

```http
GET /{OBJECT_TYPE}?tags={TAG_NAME}:{TAG_VALUE}
GET /{OBJECT_TYPE}?tags={TAG_NAME_1}:{TAG_VALUE_1},{TAG_NAME_2}:{TAG_VALUE_2}
GET /{OBJECT_TYPE}?tags={TAG_NAME}:{TAG_VALUE}*
GET /{OBJECT_TYPE}?tags={TAG_NAME}:*
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得するCatalogオブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`dataSets`</li></ul> |
| `{TAG_NAME}` | フィルターに使用するタグの名前。 |
| `{TAG_VALUE}` | フィルターに使用するタグの値。 ワイルドカード文字(`*`)をサポートします。 |

**リクエスト**

次のリクエストは、データセットのリストを取得し、特定の値を持つ1つのタグと、2番目のタグが存在することでフィルタリングします。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?tags=sampleTag:123456,secondTag:* \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、値が「123456」、値が「123456」 `sampleTag` であるデータセットのリストを返し、その結果 `secondTag` は任意の値を返します。 制限も指定しない限り、応答には最大20個のオブジェクトが含まれます。

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

カタログAPIの一部のエンドポイントには、幅のあるクエリを許可するクエリパラメーターがあり、ほとんどの場合は日付が使用されます。

**API形式**

```http
GET /batches?createdAfter={TIMESTAMP_1}&createdBefore={TIMESTAMP_2}
```

| パラメーター | 説明 |
| --- | --- |
| `{TIMESTAMP }` | UNIXエポック時間のdatetime整数。 |

**リクエスト**

次のリクエストは、2019年4月にリストされたバッチの作成日を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/batches?createdAfter=1554076800000&createdBefore=1556668799000' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答には、指定したリスト範囲内に含まれるCatalogオブジェクトのデータが含まれます。 制限も指定しない限り、応答には最大20個のオブジェクトが含まれます。

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

クエリパ `orderBy` ラメーターを使用すると、指定したプロパティ値に基づいて応答データを並べ替え（並べ替え）できます。 このパラメーターには、「方向」(昇順または降順`asc` の場合)、コ `desc` ロン()、結果を並べ替えるプロパティ`:`の順に並べ替える必要があります。 方向を指定しない場合、デフォルトの方向は昇順です。

複数の並べ替えプロパティは、コンマで区切ったリストで指定できます。 最初の並べ替えプロパティで、そのプロパティと同じ値を含む複数のオブジェクトが生成される場合、2番目の並べ替えプロパティを使用して、一致するオブジェクトをさらに並べ替えます。

例えば、次のクエリを考えます。 `orderBy=name,desc:created`. 結果は、最初の並べ替えプロパティに基づいて昇順で並べ替えられま `name`す。 複数のレコードが同じプロパティを共有してい `name` る場合、一致するレコードは2番目の並べ替えプロパティで並べ替えられま `created`す。 返されたレコードが同じレコードを共有しな `name`い場合、プ `created` ロパティは並べ替えを考慮しません。


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

次の要求は、プロパティで並べ替えられたリストセットのデータセットを取得 `name` します。 同じデータセットを共有するデ `name`ータセットがある場合、そのデータセットはプロパティによって降順 `updated` で並べ替えられます。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?orderBy=name,desc:updated' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答には、リストに従って並べ替えられたCatalogオブジェクトのパラメータが含ま `orderBy` れます。 制限も指定しない限り、応答には最大20個のオブジェクトが含まれます。

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

Catalogは、プロパティでフィルタリングする2つの方法を提供します。この方法については、以下の節で詳しく説明します。

* [シンプルフィルター](#using-simple-filters):特定のプロパティが特定の値と一致するかどうかでフィルタリングします。
* [プロパティパラメーターの使用](#using-the-property-parameter):条件付き式を使用して、プロパティが存在するかどうか、またはプロパティの値が別の指定した値や正規式と一致、近似、比較するかどうかを基にフィルタリングします。

### 単純なフィルター {#using-simple-filters}

単純なフィルターを使用すると、特定のプロパティ値に基づいて応答をフィルタリングできます。 単純なフィルターは、の形式をとりま `{PROPERTY_NAME}={VALUE}`す。

例えば、クエリは、プ `name=exampleName` ロパティに「exampleName」とい `name` う値が含まれているオブジェクトのみを返します。 これに対し、クエリは `name=!exampleName` 「exampleName」でな `name` いプロパテ **ィを持つ** オブジェクトのみを返します。

また、単純なフィルターは、単一のプロパティに対して複数の値をクエリする機能をサポートしています。 複数の値が指定された場合、応答は指定されたリスト内の値のいず **れかに** 、プロパティが一致するオブジェクトを返します。 複数値のクエリを反転するには、リストに文字をプリフィックスし、指定したリストにプロパティ値が含まれていな `!` い **(例えば、**`name=!exampleName,anotherName`)オブジェクトのみを返します。

**API形式**

```http
GET /{OBJECT_TYPE}?{PROPERTY_NAME}={VALUE}
GET /{OBJECT_TYPE}?{PROPERTY_NAME}=!{VALUE}
GET /{OBJECT_TYPE}?{PROPERTY_NAME}={VALUE_1},{VALUE_2},{VALUE_3}
GET /{OBJECT_TYPE}?{PROPERTY_NAME}=!{VALUE_1},{VALUE_2},{VALUE_3}
```

| パラメーター | 説明 |
| --- | --- |
| `{OBJECT_TYPE}` | 取得するCatalogオブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{PROPERTY_NAME}` | 値のフィルターに使用するプロパティの名前です。 |
| `{VALUE}` | 含める（または除外する）結果を決定するプロパティ値(クエリに応じて)。 |

**リクエスト**

次のリクエストは、データセットのリストを取得し、「exampleName」または「anotherName」の値を持つプロパティを持つデ `name` ータセットのみを含めるようにフィルタリングします。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets?name=exampleName,anotherName' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答には、「exampleName」または「anotherName」のデータセットを除く、デ `name` ータセットのリストが含まれます。 制限も指定しない限り、応答には最大20個のオブジェクトが含まれます。

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

### パラメータの `property` 使用 {#using-the-property-parameter}

この `property` クエリパラメータは、単純なパラメータよりも柔軟にプロパティベースのフィルタリングを行えるフィルターです。 プロパティに特定の値が含まれるかどうかに基づいてフィルタリングする以外に、他の比較演算子(「より大きい」( `property` )や「より小さい」(`>``<`)など)を使用して、プロパティ値でフィルタリングする正規式を使用できます。 また、値に関係なく、プロパティが存在するかどうかでフィルタリングすることもできます。

このパ `property` ラメーターは、最上位レベルのオブジェクトプロパティのみを受け入れます。つまり、次のサンプルオブジェクトでは、、、およびのプロパティでフィルタリングできますが、 `name`のプロパティではフ `description`ィルタリ `subItem`ングできませ `sampleKey`ん。

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
| `{OBJECT_TYPE}` | 取得するCatalogオブジェクトのタイプ。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> |
| `{CONDITION}` | 条件付き式。クエリの対象となるプロパティと、その値の評価方法を示します。 以下に例を示します。 |

このパラメーターの値は、 `property` 複数の異なる種類の条件付き式をサポートします。 次の表に、サポートされる式の基本構文を示します。

| シンボル | 説明 | 例 |
| --- | --- | --- |
| (None) | プロパティ名を演算子なしで指定すると、値に関係なく、プロパティが存在するオブジェクトのみが返されます。 | `property=name` |
| ! | パラメータの値に「`!`」を接頭辞として付けると、プ `property` ロパティが存在しないオブジェクトのみが返 **されます** 。 | `property=!name` |
| ~ | チルダ(`~`)記号の後に指定された正規式に一致するプロパティ値（文字列）を持つオブジェクトのみを返します。 | `property=name~^example` |
| == | プロパティ値が記号(`==`)の後に指定した文字列と完全に一致する重複のみを返します。 | `property=name==exampleName` |
| != | 不等号()の後に指定された **文字列** とプロパティ値が一致しないオブジェクトのみを返し`!=`ます。 | `property=name!=exampleName` |
| &lt; | プロパティ値が指定した値より小さい（ただし、等しくない）オブジェクトのみを返します。 | `property=version<1.0.0` |
| &lt;= | プロパティ値が指定した値（またはそれと等しい）より小さいオブジェクトのみを返します。 | `property=version<=1.0.0` |
| > | 指定した値より大きい（ただし、等しくない）プロパティ値を持つオブジェクトのみを返します。 | `property=version>1.0.0` |
| >= | プロパティ値が指定した値（またはそれと等しい）より大きいオブジェクトのみを返します。 | `property=version>=1.0.0` |

>[!NOTE] このプ `name` ロパティでは、検索文字列全体また `*`はその一部として、ワイルドカードの使用がサポートされています。 ワイルドカードは、検索文字列が値「test」と一致する `te*st` ように、空の文字に一致します。 アスタリスクは2倍にすることでエスケープさ`**`れる()。 検索文字列の重複アスタリスクは、リテラル文字列として1つのアスタリスクを表します。

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

成功した応答には、バージョン番号が1.0.3を超えるデータセットのリストが含まれます。制限も指定しない限り、応答には最大20個のオブジェクトが含まれます。

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

## 複数のフィルター

アンパサンド(`&`)を使用すると、複数のフィルターを1つのリクエストで結合できます。 リクエストに追加の条件が追加されると、AND関係が想定されます。

**API形式**

```http
GET /{OBJECT_TYPE}?{FILTER_1}={VALUE}&{FILTER_2}={VALUE}&{FILTER_3}={VALUE}
```
