---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイム顧客プロファイルAPI開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: bb7aad4de681316cc9f9fd1d9310695bd220adb1

---


# エッジの宛先と投影

複数のチャネルにわたって顧客の体験をリアルタイムで調整し、一貫性を保ち、パーソナライズするためには、変更が発生した場合に適切なデータを容易に利用でき、継続的に更新する必要があります。 Adobe Experience Platformを使用すると、エッジと呼ばれるものを使用して、このリアルタイムでデータにアクセスできます。 エッジとは、データを格納し、アプリケーションから容易にアクセスできるようにする、地理的に配置されたサーバです。 例えば、アドビのターゲットやAdobe Campaignなどのアドビアプリケーションは、エッジを使用して、パーソナライズされた顧客エクスペリエンスをリアルタイムで提供します。 データは投影によってエッジにルーティングされ、データの送信先となるエッジを定義する投影先と、エッジ上で利用可能にする特定の情報を定義する投影設定とがあります。 このガイドでは、リアルタイム顧客プロファイルAPIを使用して、宛先や設定などのエッジ投影を操作する手順を詳しく説明します。

## はじめに

このガイドで使用されるAPIエンドポイントは、リアルタイム顧客プロファイルAPIの一部です。 続行する前に、リアルタイムのお客様 [プロファイル開発ガイドを参照してください](getting-started.md)。 特に、プロファイル開発ガ [イドの](getting-started.md#getting-started) 「はじめに」の節には、関連トピックへのリンク、このドキュメントのサンプルAPI呼び出しの読み方のガイド、Experience Platform APIを正常に呼び出すために必要なヘッダーに関する重要な情報が含まれています。

## 投影先

投影は、データを送信する場所を指定することで、1つ以上のエッジにルーティングできます。 作成される各投影先には一意のIDが割り当てられ、このIDを使用して投影設定が作成されます。

### リスト

エンドポイントにGETリクエストを行うことで、組織用に既に作成されているエッジの宛先をリストで `/config/destinations` きます。

**API形式**

```http
GET /config/destinations
```

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/destinations \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答には、各宛先の `projectionDestinations` 詳細が配列内の個々のオブジェクトとして表示される配列が含まれます。 投影が設定されていない場合、配列は空 `projectionDestinations` を返します。

>[!NOTE]
>この応答はスペースを節約し、2つの宛先のみを表示します。

```json
{
    "_links": {
        "self": {
            "href": "/data/core/ups/config/destinations",
            "templated": false
        }
    },
    "_embedded": {
        "projectionDestinations": [
            {
                "_links": {
                    "self": {
                        "href": "/data/core/ups/config/destinations/cef0e2ef-5249-4e25-be90-94f5797a2841",
                        "templated": false
                    }
                },
                "id": "cef0e2ef-5249-4e25-be90-94f5797a2841",
                "type": "EDGE",
                "ttl": 3600,
                "dataCenters": [
                    "VA5"
                ],
                "version": 1
            },
            {
                "_links": {
                    "self": {
                        "href": "/data/core/ups/config/destinations/7d26263f-a5df-43ce-b088-7f70e187f549",
                        "templated": false
                    }
                },
                "id": "7d26263f-a5df-43ce-b088-7f70e187f549",
                "type": "EDGE",
                "ttl": 3600,
                "dataCenters": [
                    "OR1"
                ],
                "version": 1
            }
        ]
    }
}
```

| プロパティ | 説明 |
|---|---|
| `_links.self.href` | 最上位レベルで、はGETリクエストの作成に使用されるパスと一致します。 個々の宛先オブジェクト内で、このパスをGETリクエストで使用して、特定の宛先の詳細を直接参照できます。 |
| `id` | 各宛先オブジェクト内で、は、 `"id"` 宛先の読み取り専用の、システム生成の一意のIDを表示します。 このIDは、特定の宛先を参照する場合や、投影設定を作成する場合に使用されます。 |

個々の宛先の属性について詳しくは、次の宛先の作成の節を [参照してく](#create-a-destination) ださい。

### Create a destination {#create-a-destination}

必要な宛先がまだ存在しない場合は、エンドポイントにPOSTリクエストを作成して、新しい投影先を作成でき `/config/destinations` ます。

**API形式**

```http
POST /config/destinations
```

**リクエスト**

次のリクエストは、新しいエッジの宛先を作成します。

>[!NOTE]
>宛先を作成するPOSTリクエストには、次に示すように、特定の `Content-Type` ヘッダーが必要です。 正しくないヘッダーを使 `Content-Type` 用すると、HTTPステータス415（サポートされていないメディアタイプ）エラーが発生します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/destinations \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/vnd.adobe.platform.projectionDestination+json; version=1' \
  -d '{
        "type": "EDGE",
        "dataCenters": [ 
          "OR1" 
        ],
        "ttl": 3600,
        "replicationPolicy": REACTIVE
      }'
```

| プロパティ | 説明 |
|---|---|
| `type` **(必須)** | 作成する宛先のタイプ。 受け入れられる唯一の値「EDGE」は、エッジの宛先を作成します。 |
| `dataCenters` **(必須)** | 投影をルーティングするリストのエッジを示す文字列配列。 次の値を1つ以上含むことができます。&quot;OR1&quot; — 米国西部、&quot;VA5&quot; — 米国東部、&quot;NLD1&quot; - EMEA。 |
| `ttl` **(必須)** | 投影の有効期限を指定します。 許容値の範囲：600 ～ 604800。 デフォルト値：3600。 |
| `replicationPolicy` **(必須)** | ハブからエッジへのデータ複製の動作を定義します。  サポートされる値：プロアクティブ、リアクティブ。 デフォルト値：反応。 |

**応答**

正常な応答は、読み取り専用の、システム生成の一意のID(`id`)を含む、新しく作成されたエッジ宛先の詳細を返します。

```json
{
    "self": {
        "href": "/data/core/ups/config/destinations/cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4",
        "templated": false
    },
    "id": "cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4",
    "type": "EDGE",
    "dataCenters": [
       "OR1"
    ],
    "ttl": 3600,
    "version": 1
}
```

| プロパティ | 説明 |
|---|---|
| `self.href` | このパスは、宛先を直接参照(GET)するために使用され、宛先の更新(PUT)または削除(DELETE)にも使用できます。 |
| `id` | 宛先の読み取り専用の、システム生成の一意のID。 このIDは、宛先を直接参照するため、およびプロジェクション設定を作成する際に使用されます。 |
| `version` | この読み取り専用の値は、宛先の現在のバージョンを表示します。 宛先が更新されると、バージョン番号が自動的に増分されます。 |

### 表示先

投影先の一意のIDがわかっている場合は、その詳細を表示に対して参照要求を実行できます。 これは、エンドポイントにGETリクエストを行い、リクエ `/config/destinations` ストパスに宛先のIDを含めることで行われます。

**API形式**

```http
GET /config/destinations/{DESTINATION_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DESTINATION_ID}` | 表示する投影先の一意のID。 |

**リクエスト**

次のリクエストは、リクエストパスで指定されたIDの宛先を表示するための参照(GET)を実行します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/destinations/9d66c06e-c745-480c-b64c-1d5234d25f4b \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答オブジェクトは、投影先の詳細を表示します。 属性 `id` は、要求で指定された投影先のIDと一致する必要があります。

```json
{
    "self": {
        "href": "/data/core/ups/config/destinations/cef0e2ef-5249-4e25-be90-94f5797a2841",
        "templated": false
    },
    "id": "cef0e2ef-5249-4e25-be90-94f5797a2841",
    "type": "EDGE",
    "ttl": 3600,
    "dataCenters": [
        "OR1"
    ],
    "version": 1
}
```

### 宛先の更新

既存の宛先は、エンドポイントにPUTリクエストを行い、更新対象の `/config/destinations` 宛先のIDをリクエストパスに含めることで更新できます。 この操作は基本的に宛 _先の書き換え_ なので、要求の本文には、新しい宛先を作成する際に指定するのと同じ属性を指定する必要があります。

>[!CAUTION]
>更新要求に対するAPI応答は直ちに行われますが、プロジェクションに対する変更は非同期で適用されます。 つまり、宛先の定義を更新するタイミングと適用するタイミングに時間差があります。

**API形式**

```
PUT /config/destinations/{DESTINATION_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DESTINATION_ID}` | 更新する投影先の一意のID。 |

**リクエスト**

次のリクエストは、2つ目の場所(`dataCenters`)を含むように既存の宛先を更新します。

>[!IMPORTANT]
>PUTリクエストには、次に示すよう `Content-Type` に特定のヘッダーが必要です。 正しくないヘッダーを使 `Content-Type` 用すると、HTTPステータス415（サポートされていないメディアタイプ）エラーが発生します。

```shell
curl -X PUT \
  https://platform.adobe.io/data/core/ups/config/destinations/8b90ce19-e7dd-403a-ae24-69683a6674e7 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/vnd.adobe.platform.projectionDestination+json' \
  -d '{
        "type": "EDGE",
        "dataCenters": [ 
          "OR1",
          "VA5" 
        ],
        "replicationPolicy": REACTIVE,
        "currentVersion": 1
      }'
```

| プロパティ | 説明 |
|---|---|
| `currentVersion` | 既存の宛先の現在のバージョン。 宛先の参照リクエ `version` ストを実行する際の属性の値。 |

**応答**

応答には、宛先のIDや新しい宛先を含む、更新された宛先の詳細が `version` 含まれます。

```json
{
    "self": {
        "href": "/data/core/ups/config/destinations/8b90ce19-e7dd-403a-ae24-69683a6674e7",
        "templated": false
    },
    "id": "8b90ce19-e7dd-403a-ae24-69683a6674e7",
    "type": "EDGE",
    "ttl": 8000,
    "dataCenters": [
        "OR1",
        "VA5"
    ],
    "version": 2
}
```

### 宛先の削除

組織で投影先が不要になった場合は、エンドポイントにDELETEリクエストを送信し、削除する宛先のIDをリクエストパスに含めることで、投影先を削除できます。 `/config/destinations`

>[!CAUTION]
>削除リクエストに対するAPIの応答は直ちに行われますが、エッジ上のデータに対する実際の変更は非同期で行われます。 つまり、プロファイルデータは（投影先で指定された）すべてのエッジから削除されますが、 `dataCenters` 処理の完了には時間がかかります。

**API形式**

```
DELETE /config/destinations/{DESTINATION_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DESTINATION_ID}` | 削除する投影先の一意のID。 |


**リクエスト**

```shell
curl -X DELETE \
  https://platform.adobe.io/data/core/ups/config/destinations/8b90ce19-e7dd-403a-ae24-69683a6674e7 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

削除要求は、HTTPステータス204（コンテンツなし）と空の応答本文を返します。 削除が成功したことを確認するには、IDで宛先の参照リクエストを実行します。 参照は、HTTPステータス404（見つかりません）を返す必要があります。

## 投影設定

投影設定は、各エッジで使用可能なデータに関する情報を提供します。 完全なエクスペリエンスデータモデル(XDM)スキーマを端に投影する代わりに、投影はスキーマから特定のデータ（フィールド）のみを提供します。 組織は、各XDMスキーマに複数の投影設定を定義できます。

### リストすべての投影設定

エンドポイントにGETリクエストを行うことで、組織に対して作成されたすべての投影設定をリストで `/config/projections` きます。 また、リクエストパスにオプションのパラメーターを追加して、特定のスキーマの投影設定にアクセスしたり、個々の投影をその名前で参照したりすることもできます。

**API形式**

```http
GET /config/projections
GET /config/projections?schemaName={SCHEMA_NAME}
GET /config/projections?schemaName={SCHEMA_NAME}&name={PROJECTION_NAME}
```

| パラメーター | 説明 |
|---|---|
| `{SCHEMA_NAME}` | アクセスするスキーマ設定に関連付けられているプロジェクションクラスの名前。 |
| `{PROJECTION_NAME}` | アクセスするプロジェクション設定の名前。 |

>[!NOTE]
>`schemaName` は、パラメーターを使用す `name` る場合に必要です。プロジェクション設定名は、パラメータークラスのコンテキストでのみ一意なスキーマ名です。

**リクエスト**

次のリクエストでは、Experience Data ModelスキーマクラスXDM Individualプロファイルに関連付けられているすべてのプロジェクション設定をリストします。 XDMとそのプラットフォーム内での役割について詳しくは、 [XDMシステム概要を読んでください](../../xdm/home.md)。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/projections?schemaName=_xdm.context.profile \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、ルートリスト内の投影設定(配列内に含ま `_embedded` れる)を返し `projectionConfigs` ます。 組織の投影設定が行われていない場合、アレイ `projectionConfigs` は空になります。

```json
{
    "_links": {
        "self": {
            "href": "/data/core/ups/config/projections",
            "templated": false
        }
    },
    "_embedded": {
        "projectionConfigs": [
            {
                "_links": {
                    "destination": {
                        "href": "/data/core/ups/config/destinations/a689248a-5d2b-44af-bb70-c8f17f97011b",
                        "templated": false
                    },
                    "self": {
                        "href": "/data/core/ups/config/projections/99aed0bc-c183-4997-ada7-7843642e08f6",
                        "templated": false
                    }
                },
                "_embedded": {
                    "destination": {
                        "self": {
                            "href": "/data/core/ups/config/destinations/a689248a-5d2b-44af-bb70-c8f17f97011b",
                            "templated": false
                        },
                        "id": "a689248a-5d2b-44af-bb70-c8f17f97011b",
                        "type": "EDGE",
                        "ttl": 1000,
                        "dataCenters": [
                            "OR1"
                        ],
                        "version": 1
                    }
                },
                "selector": "strategy",
                "version": 2,
                "id": "99aed0bc-c183-4997-ada7-7843642e08f6",
                "schemaName": "_xdm.context.profile",
                "name": "adcloud_rlsa",
                "destinationId": "a689248a-5d2b-44af-bb70-c8f17f97011b"
            },
        ]
    }
}
```

### 投影設定の作成

エッジで使用可能にするXDMフィールドを指定する新しい投影設定を作成(POST)できます。

**API形式**

```http
POST /config/projections?schemaName={SCHEMA_NAME}
```

| パラメーター | 説明 |
|---|---|
| `{SCHEMA_NAME}` | アクセスするスキーマ設定に関連付けられているプロジェクションクラスの名前。 |

**リクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/projections?schemaName=_xdm.context.profile \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "selector": "emails,person(firstName)",
        "name": "my_test_projection",
        "destinationId": "cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4"
      }'
```

| プロパティ | 説明 |
|---|---|
| `selector` | エッジに複製されるリスト内のプロパティのスキーマを含む文字列。 セレクターの操作に関するベストプラクティスは、このセク [ターの](#selectors) 「ドキュメント」セクションです。 |
| `name` | 新しい投影設定を説明する名前。 |
| `destinationId` | データの投影先のエッジの識別子。 |

**応答**

成功した応答は、新しく作成された投影設定の詳細を返します。

```json
{
    "_links": {
        "destination": {
            "href": "/data/core/ups/config/destinations/cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4",
            "templated": false
        },
        "self": {
            "href": "/data/core/ups/config/projections/87fcd0bc-c183-4997-daf9-7843642g95a1",
            "templated": false
        }
    },
    "_embedded": {
        "destination": {
            "self": {
                "href": "/data/core/ups/config/destinations/cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4",
                "templated": false
            },
            "id": "cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4",
            "type": "EDGE",
            "ttl": 1000,
            "dataCenters": [
                "OR1"
            ],
            "version": 1
        }
    },
    "selector": "emails,person(firstName)",
    "version": 2,
    "id": "87fcd0bc-c183-4997-daf9-7843642g95a1",
    "schemaName": "_xdm.context.profile",
    "name": "my_test_projection",
    "destinationId": "cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4"
}
```

## Selectors {#selectors}

セレクターは、XDMフィールド名のリストをコンマで区切ったものです。 投影設定では、セレクタは投影に含めるプロパティを指定する。 パラメーター値の形 `selector` 式は、XPath構文に大まかに基づいています。 次に、サポートされている構文の概要を示します。その他の参照例も示します。

### サポートされる構文

* 複数のフィールドを選択するには、コンマを使用します。 スペースは使用しないでください。
* ドット表記を使用して、階層化されたフィールドを選択します。
   * 例えば、という名前のフィールド内にネストさ `field` れているフィールドを選択するには、セ `foo`レクターを使用しま `foo.field`す。
* サブフィールドを含むフィールドを含めると、すべてのサブフィールドもデフォルトで投影されます。 ただし、丸括弧を使用して返されるサブフィールドをフィルタリングすることはできま `"( )"`す。
   * 例えば、は、 `addresses(type,city.country)` 各配列要素の住所の種類と住所の市区町村が存在する国のみを返 `addresses` します。
   * 上記の例はと同じです `addresses.type,addresses.city.country`。

>[!NOTE]
>サブフィールドの参照には、ドット表記と括弧の両方がサポートされています。 ただし、ドット表記を使用する方がより簡潔で、フィールド階層の説明がより良いので、この方法をお勧めします。

* セレクター内の各フィールドは、応答のルートを基準に指定します。
   * データがリソースのコレクションの場合、プロジェクションにはリソースの配列が含まれます。
   * データが単一のリソースの場合、投影にはそのリソースを基準とするフィールドが含まれます。
   * 選択したフィールドが配列（または配列の一部）の場合、投影には配列内のすべての要素の選択した部分が含まれます。

### セレクターパラメーターの例

次の例は、サンプルのパラメ `selector` ーターと、その後に表す構造化値を示しています。

**person.lastName**

要求されたリ `lastName` ソース内のオブジェク `person` トのサブフィールドを返します。

```json
{
  "person": {
    "lastName": "Smith"
  }
}
```

**アドレス**

配列内のすべての要素を返します。各 `addresses` 要素内のすべてのフィールドは含まれますが、他のフィールドは含まれません。

```json
{
  "addresses": [
    {
      "type": "home",
      "street1": "100 Great Mall Parkway",
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    },
    {
      "type": "work",
      "street1": "1 Main Street",
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    }
  ]
}
```

**person.lastName,addresses**

配列内のフ `person.lastName` ィールドとすべての要素を返 `addresses` します。

```json
{
  "person": {
    "lastName": "Smith"
  },
  "addresses": [
    {
      "type": "home",
      "street1": "100 Great Mall Parkway",
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    },
    {
      "type": "work",
      "street1": "1 Main Street",
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    }
  ]
}
```

**addresses.city**

住所配列内のすべての要素の市区町村フィールドのみを返します。

```json
{
  "addresses": [
    {
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    },
    {
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    }
  ]
}
```

>[!NOTE]
>ネストされたフィールドが返されるたびに、投影には含まれる親オブジェクトが含まれます。 親フィールドも明示的に選択されていない限り、他の子フィールドは含まれません。

**addresses(type,city)**

配列内の各要素のフィー `type` ルドと `city` フィールドの値のみを返し `addresses` ます。 各要素に含まれるその他のサブフィールドはすべ `addresses` て除外されます。

```json
{
  "addresses": [
    {
      "type": "home",
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    },
    {
      "type": "work",
      "city": {
        "name": "San Jose",
        "country": "United States"
      }
    }
  ]
}
```

## 次の手順

このガイドでは、パラメータを適切にフォーマットする方法など、エッジ投影と宛先を設定する手順を示し `selector` ました。 組織のニーズに合わせて新しいエッジの宛先や投影を作成できるようになりました。 プロファイルAPIを通じて利用できる追加のアクションを見つけるには、 [Real-time CustomerプロファイルAPI開発者ガイドを参照してください](getting-started.md)。