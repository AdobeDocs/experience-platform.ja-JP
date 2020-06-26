---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイムの顧客プロファイルAPI開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: d464a6b4abd843f5f8545bc3aa8000f379a86c6d
workflow-type: tm+mt
source-wordcount: '1919'
ht-degree: 2%

---


# エッジ投影設定と宛先エンドポイント

複数のチャネルにわたって顧客に対して、調整、一貫性、パーソナライズされたエクスペリエンスをリアルタイムで提供するためには、変更が発生した場合に適切なデータを容易に利用でき、継続的に更新する必要があります。 Adobe Experience Platformを使用すると、エッジと呼ばれるものを使用して、データにリアルタイムでアクセスできます。 エッジは、データを格納し、アプリケーションから容易にアクセスできるようにする、地理的に配置されたサーバです。 例えば、Adobe TargetやAdobe Campaignなどのアドビアプリケーションは、パーソナライズされた顧客体験をリアルタイムで提供するためにエッジを使用します。 データは投影によってエッジにルーティングされ、投影先はデータの送信先となるエッジを定義し、投影設定はエッジで利用可能にする特定の情報を定義します。 このガイドでは、リアルタイム顧客プロファイルAPIを使用して、宛先や設定などのエッジ予測を操作するための詳細な手順を説明します。

## はじめに

このガイドで使用されるAPIエンドポイントは、 [リアルタイム顧客プロファイルAPIの一部](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/real-time-customer-profile.yaml)です。 先に進む前に、 [はじめに](getting-started.md) 、関連ドキュメントへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、Experience PlatformAPIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報を確認してください。

>[!NOTE]
>ペイロード(POST、PUT、PATCH)を含む要求には、 `Content-Type` ヘッダーが必要です。 このドキュメントでは、複数 `Content-Type` が使用されています。 サンプル呼び出しのヘッダーには特に注意を払い、各リクエストで正しいヘッダーが使用されていることを確認して `Content-Type` ください。

## 投影先

投影は、データを送信する位置を指定することで、1つ以上のエッジにルーティングできます。 作成される各投影宛先には一意のIDが割り当てられ、このIDを使用して投影設定が作成されます。

### すべての宛先のリスト

エンドポイントにGETリクエストを行うことで、組織用に既に作成されているエッジの宛先をリストでき `/config/destinations` ます。

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

応答には、配列内の個々のオブジェクトとして表示される各宛先の詳細を持つ `projectionDestinations` 配列が含まれます。 投影法が設定されていない場合、 `projectionDestinations` 配列は空を返します。

>[!NOTE]
>この応答はスペースを節約し、2つの宛先しか表示しません。

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
| `_links.self.href` | トップレベルで、はGETリクエストを行う際に使用するパスと一致します。 個々の宛先オブジェクト内で、このパスをGETリクエストで使用して、特定の宛先の詳細を直接参照できます。 |
| `id` | 各宛先オブジェクト内で、には、その宛先に対する読み取り専用の、システム生成の一意のIDが `"id"` 表示されます。 このIDは、特定の宛先を参照する場合や、投影設定を作成する場合に使用されます。 |

個々の宛先の属性の詳細については、次の宛先の [作成に関する節を参照してください](#create-a-destination) 。

### Create a destination {#create-a-destination}

必要な宛先がまだ存在しない場合は、エンドポイントにPOSTリクエストを作成して、新しい投影先を作成でき `/config/destinations` ます。

**API形式**

```http
POST /config/destinations
```

**リクエスト**

次のリクエストは、新しいエッジ宛先を作成します。

>[!NOTE]
>宛先を作成するPOSTリクエストには、次に示すように、特定の `Content-Type` ヘッダーが必要です。 正しくない `Content-Type` ヘッダーを使用すると、HTTPステータス415（サポートされていないメディアタイプ）エラーが発生します。

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
| `type` **(必須)** | 作成する宛先のタイプ。 唯一許容される値「EDGE」は、エッジの宛先を作成します。 |
| `dataCenters` **(必須)** | 投影の方向を向くエッジをリストする文字列配列。 次の値を1つ以上含むことができます。 「OR1」 — 米国西部、「VA5」 — 米国東部、「NLD1」 - EMEA。 |
| `ttl` **(必須)** | 投影の有効期限を指定します。 受け入れられる値の範囲： 600 ～ 604800。 デフォルト値： 3600。 |
| `replicationPolicy` **(必須)** | ハブからエッジへのデータ複製の動作を定義します。  サポートされる値： 積極的で反応的。 デフォルト値： 反応性。 |

**応答**

正常な応答は、新しく作成されたエッジ宛先の詳細(読み取り専用の、システム生成の一意のID(`id`)を含む)を返します。

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
| `self.href` | このパスは、宛先を直接参照(GET)するために使用され、宛先を更新(PUT)または削除(DELETE)するために使用することもできます。 |
| `id` | 読み取り専用で、システムによって生成された、宛先の一意のID。 このIDは、宛先を直接参照する目的で使用され、投影設定を作成する際に使用されます。 |
| `version` | この読み取り専用の値は、宛先の現在のバージョンを表示します。 リンク先が更新されると、バージョン番号が自動的に増分されます。 |

### 表示先

投影先の一意のIDがわかっている場合は、その詳細を表示するための参照リクエストを実行できます。 これは、エンドポイントにGETリクエストを行い、リクエストパスに宛先のIDを含めることで行わ `/config/destinations` れます。

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

応答オブジェクトは、投影先の詳細を示します。 この `id` 属性は、要求で指定された投影先のIDと一致する必要があります。

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

エンドポイントにPUT要求を行い、更新対象の宛先のIDを要求パスに含めることで、既存の宛先を更新することがで `/config/destinations` きる。 この操作は基本的に宛先を _書き換えるので_ 、新しい宛先を作成する場合と同じ属性を要求の本文に指定する必要があります。

>[!CAUTION]
>更新要求に対するAPIの応答は直ちに行われますが、投影に対する変更は非同期で適用されます。 つまり、宛先の定義を更新するタイミングと、更新するタイミングとの間に時間差があります。

**API形式**

```
PUT /config/destinations/{DESTINATION_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DESTINATION_ID}` | 更新する投影先の一意のID。 |

**リクエスト**

次の要求は、2つ目の場所(`dataCenters`)を含むように既存の宛先を更新します。

>[!IMPORTANT]
>PUTリクエストには、次に示すように、特定の `Content-Type` ヘッダーが必要です。 正しくない `Content-Type` ヘッダーを使用すると、HTTPステータス415（サポートされていないメディアタイプ）エラーが発生します。

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
| `currentVersion` | 既存の宛先の現在のバージョン。 宛先に対する参照リクエストを実行する際の `version` 属性の値。 |

**応答**

応答には、宛先のIDや宛先の新しい情報など、更新された詳細 `version` が含まれます。

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

組織で投影先が不要になった場合は、エンドポイントにDELETEリクエストを行い、削除する宛先のIDをリクエストパスに含めることで、投影先を削除でき `/config/destinations` ます。

>[!CAUTION]
>削除リクエストに対するAPIの応答は直ちに行われますが、エッジ上のデータに対する実際の変更は非同期で行われます。 つまり、プロファイルデータはすべてのエッジから削除されます(投影先で `dataCenters` 指定)が、処理の完了には時間がかかります。

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

削除要求は、HTTPステータス204（コンテンツなし）と空の応答本文を返します。 宛先に対してIDによる参照リクエストを実行することで、削除が成功したことを確認できます。 このルックアップは、HTTPステータス404（エラー）を返す必要があります。

## 投影設定

投影設定は、各エッジで使用可能なデータに関する情報を提供します。 完全なExperience Data Model(XDM)スキーマを端に投影する代わりに、投影はスキーマから特定のデータ（フィールド）のみを提供します。 組織では、各XDMスキーマに対して複数の投影設定を定義できます。

### すべての投影設定のリスト

エンドポイントにGETリクエストを作成して、組織用に作成されたすべての投影設定をリストでき `/config/projections` ます。 また、リクエストパスにオプションのパラメーターを追加して、特定のスキーマの投影設定にアクセスしたり、個々の投影をその名前で参照したりすることもできます。

**API形式**

```http
GET /config/projections
GET /config/projections?schemaName={SCHEMA_NAME}
GET /config/projections?schemaName={SCHEMA_NAME}&name={PROJECTION_NAME}
```

| パラメーター | 説明 |
|---|---|
| `{SCHEMA_NAME}` | アクセスするプロジェクション設定に関連付けられているスキーマクラスの名前。 |
| `{PROJECTION_NAME}` | アクセスする投影設定の名前。 |

>[!NOTE]
>`schemaName` は、 `name` パラメーターを使用する場合に必須です。投影設定名は、スキーマクラスのコンテキストでのみ一意です。

**リクエスト**

次のリクエストでは、Experience Data ModelスキーマクラスXDM Individualプロファイルに関連付けられているすべての投影設定をリストします。 XDMとPlatform内でのその役割についての詳細は、 [XDMシステムの概要を読んでください](../../xdm/home.md)。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/projections?schemaName=_xdm.context.profile \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、ルート `_embedded` 属性内の投影設定のリスト( `projectionConfigs` 配列内に含まれる)を返します。 組織で投影設定が行われていない場合、 `projectionConfigs` アレイは空になります。

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
| `{SCHEMA_NAME}` | アクセスするプロジェクション設定に関連付けられているスキーマクラスの名前。 |

**リクエスト**

>[!NOTE]
>設定を作成するPOSTリクエストには、以下に示すように、特定の `Content-Type` ヘッダーが必要です。 正しくない `Content-Type` ヘッダーを使用すると、HTTPステータス415（サポートされていないメディアタイプ）エラーが発生します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/config/projections?schemaName=_xdm.context.profile \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/vnd.adobe.platform.projectionConfig+json; version=1' \
  -d '{
        "selector": "emails,person(firstName)",
        "name": "my_test_projection",
        "destinationId": "cc5a3bd1-f2b9-4965-b9bd-4e7416a02cd4"
      }'
```

| プロパティ | 説明 |
|---|---|
| `selector` | エッジに複製されるスキーマ内のプロパティのリストを含む文字列。 セレクターの操作に関するベストプラクティスは、このドキュメントの「 [セレクター](#selectors) 」セクションにあります。 |
| `name` | 新しい投影設定を説明する名前。 |
| `destinationId` | データの投影先となるエッジの宛先の識別子。 |

**応答**

正常な応答は、新しく作成された投影設定の詳細を返します。

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

セレクターは、XDMフィールド名をカンマで区切ったリストです。 投影設定では、セレクタは投影に含めるプロパティを指定します。 パラメーター値の形式は、XPath構文に大まかに基づいています。 `selector` サポートされている構文を以下にまとめます。その他の例も参照用に示します。

### サポートされる構文

* 複数のフィールドを選択する場合は、コンマを使用します。 スペースは使用しないでください。
* ネストされたフィールドを選択するには、ドット表記を使用します。
   * 例えば、という名前のフィールド内にネストされ `field` ている、という名前のフィールドを選択するに `foo`は、セレクターを使用し `foo.field`ます。
* サブフィールドを含むフィールドを含めると、すべてのサブフィールドもデフォルトで投影されます。 ただし、丸括弧を使用して返されるサブフィールドをフィルターでき `"( )"`ます。
   * 例えば、は、各 `addresses(type,city.country)``addresses` 配列要素に対して、住所の種類と住所の市区町村が存在する国のみを返します。
   * 上記の例はと同じで `addresses.type,addresses.city.country`す。

>[!NOTE]
>サブフィールドの参照には、ドット表記と括弧の両方がサポートされています。 ただし、ドット表記の方が簡潔で、フィールド階層の説明がしやすくなるので、ドット表記を使用することをお勧めします。

* セレクター内の各フィールドは、応答のルートを基準に指定されます。
   * データがリソースのコレクションである場合、投影にはリソースの配列が含まれます。
   * データが単一のリソースの場合、投影にはそのリソースに関連するフィールドが含まれます。
   * 選択したフィールドが配列の（またはその一部である）場合、投影には、配列内のすべての要素の選択した部分が含まれます。

### セレクターパラメーターの例

次の例は、サンプル `selector` パラメーターと、その後に表される構造化値を示しています。

**person.lastName**

要求されたリソース内の `lastName``person` オブジェクトのサブフィールドを返します。

```json
{
  "person": {
    "lastName": "Smith"
  }
}
```

**アドレス**

配列内のすべての要素を返します。各要素内のすべてのフィールドは含まれますが、他のフィールドは含まれません。 `addresses`

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

配列内の `person.lastName` フィールドとすべての要素を返し `addresses` ます。

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

住所配列のすべての要素に対して市区町村フィールドのみを返します。

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
>ネストされたフィールドが返される場合は常に、それを含む親オブジェクトが投影に含まれます。 親フィールドには、明示的に選択されていない限り、他の子フィールドは含まれません。

**addresses(type,city)**

配列内の各要素の `type` および `city` フィールドの値のみを返し `addresses` ます。 各 `addresses` 要素に含まれる他のサブフィールドはすべて除外されます。

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

このガイドでは、パラメータを適切にフォーマットする方法など、投影と宛先を設定するための手順を説明しました `selector` 。 組織のニーズに合わせて新しい投影先や設定を作成できるようになりました。