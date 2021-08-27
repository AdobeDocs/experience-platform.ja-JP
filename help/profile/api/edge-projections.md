---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API
title: エッジ投影APIエンドポイント
topic-legacy: guide
type: Documentation
description: Adobe Experience Platformでは、変更が発生した際に適切なデータを容易に利用でき、継続的に更新することで、複数のチャネルにわたって顧客に合わせて調整され、一貫性のある、パーソナライズされたエクスペリエンスをリアルタイムで提供できます。 これは、データを格納し、アプリケーションから容易にアクセスできるようにする、地理的に配置されたサーバーであるエッジを使用しておこなわれます。
exl-id: ce429164-8e87-412d-9a9d-e0d4738c7815
source-git-commit: 4c544170636040b8ab58780022a4c357cfa447de
workflow-type: tm+mt
source-wordcount: '1959'
ht-degree: 85%

---

# エッジ投影設定と宛先エンドポイント

複数のチャネルにわたって調整され、パーソナライズされた、一貫性のあるエクスペリエンスをリアルタイムで顧客に提供するには、適切なデータを容易に利用できるようにするとともに、変更が発生した際にデータを継続的に更新する必要があります。Adobe Experience Platform を使用すると、エッジと呼ばれるものを使用して、データへのこのリアルタイムアクセスを可能にします。エッジとは、データを格納し、アプリケーションから容易にアクセスできるようにする、地理的に配置されたサーバーです。たとえば、Adobe Target や Adobe Campaign などの Adobe アプリケーションは、エッジを使用して、パーソナライズされた顧客体験をリアルタイムで提供します。データは投影によってエッジにルーティングされ、データの送信先となるエッジを定義する投影先と、エッジ上で利用可能にする特定の情報を定義する投影設定があります。このガイドでは、[!DNL Real-time Customer Profile] APIを使用して、宛先や設定などのエッジ投影を操作する手順を詳しく説明します。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Real-time Customer Profile API]](https://www.adobe.com/go/profile-apis-en) の一部です。先に進む前に、[はじめる前に](getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

>[!NOTE]
>
>ペイロード(POST、PUT、PATCH)を含むリクエストには、`Content-Type`ヘッダーが必要です。 このドキュメントでは、複数の`Content-Type`が使用されています。 サンプル呼び出しのヘッダーに特に注意して、各リクエストで正しい`Content-Type`を使用していることを確認してください。

## 投影先

投影は、データを送信する場所を指定することで、1 つ以上のエッジにルーティングできます。作成される各投影先には一意の ID が割り当てられ、この ID を使用して投影設定が作成されます。

### すべての宛先のリスト

`/config/destinations`エンドポイントに GET リクエストをおこなうことで、組織用に既に作成されているエッジの宛先をリストできます。

**API 形式**

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

応答には、各宛先の詳細が配列内の個々のオブジェクトとして表示される`projectionDestinations`配列が含まれます。投影が設定されていない場合、`projectionDestinations`配列は空を返します。

>[!NOTE]
>
>この応答はスペースを節約し、2 つの宛先のみを表示します。

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
| `_links.self.href` | 最上位レベルでは、GET リクエストの作成に使用されるパスと一致します。個々の宛先オブジェクト内で、このパスを GET リクエストで使用して、特定の宛先の詳細を直接参照できます。 |
| `id` | 各宛先オブジェクト内では、`"id"`は宛先の読み取り専用の、システム生成の一意の ID を表示します。この ID は、特定の宛先を参照する場合や、投影設定を作成する場合に使用されます。 |

個々の宛先の属性について詳しくは、次の「[宛先の作成](#create-a-destination)」の節を 参照してください。

### 宛先の作成 {#create-a-destination}

必要な宛先がまだ存在しない場合は、`/config/destinations`エンドポイントに POST リクエストを作成して、新しい投影先を作成できます。

**API 形式**

```http
POST /config/destinations
```

**リクエスト**

次のリクエストは、新しいエッジの宛先を作成します。

>[!NOTE]
>
>宛先を作成する POST リクエストには、次に示すように、特定の`Content-Type`ヘッダーが必要です。正しくない`Content-Type`ヘッダーを使用すると、HTTP ステータス 415（サポートされていないメディアタイプ）エラーが発生します。

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
| `dataCenters` **(必須)** | 投影の方向をリストする文字列配列。 次の値を 1 つ以上含むことができます。「OR1」- 米国西部、「VA5」- 米国東部、「NLD1」- EMEA。 |
| `ttl` **(必須)** | 投影の有効期限を指定します。 許容値の範囲：600～604800。デフォルト値：3600。 |
| `replicationPolicy` **(必須)** | ハブからエッジへのデータレプリケーションの動作を定義します。  サポートされる値：PROACTIVE、REACTIVE。デフォルト値：反応性。 |

**応答** 

正常な応答は、読み取り専用の、システム生成の一意の ID（`id`）を含む、新しく作成されたエッジ宛先の詳細を返します。

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
| `self.href` | このパスは、宛先を直接参照（GET）するために使用され、宛先の更新（PUT）または削除（DELETE）にも使用できます。 |
| `id` | 宛先の読み取り専用の、システム生成の一意の ID。この ID は、宛先を直接参照するため、および投影設定を作成する際に使用されます。 |
| `version` | この読み取り専用の値は、宛先の現在のバージョンを表示します。宛先が更新されると、バージョン番号が自動的に増分されます。 |

### 宛先の表示

投影先の一意の ID がわかっている場合は、ルックアップリクエストを実行してその詳細を表示できます。これは、`/config/destinations`エンドポイントに GET リクエストを行い、リクエストパスに宛先の ID を含めることでおこなわれます。

**API 形式**

```http
GET /config/destinations/{DESTINATION_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DESTINATION_ID}` | 表示する投影先の一意の ID。 |

**リクエスト**

次のリクエストは、ルックアップ（GET）を実行して、リクエストパスで指定された ID の宛先を表示します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/destinations/9d66c06e-c745-480c-b64c-1d5234d25f4b \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答オブジェクトは、投影先の詳細を表示します。`id`属性は、リクエストで指定された投影先の ID と一致する必要があります。

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

既存の宛先は、`/config/destinations`エンドポイントに PUT リクエストをおこない、更新対象の宛先の ID をリクエストパスに含めることで更新できます。この操作は基本的に宛先の書き換えなので、リクエストの本文には、新しい宛先を作成する際に指定するのと同じ属性を指定する必要があります。

>[!CAUTION]
>
>更新リクエストに対する API 応答は直ちに行われますが、投影に対する変更は非同期で適用されます。つまり、宛先の定義を更新するタイミングと適用するタイミングに時間差があります。

**API 形式**

```http
PUT /config/destinations/{DESTINATION_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DESTINATION_ID}` | 更新する投影先の一意の ID。 |

**リクエスト**

次のリクエストは、2 つ目の場所（`dataCenters`）を含むように既存の宛先を更新します。

>[!IMPORTANT]
>
>PUT リクエストには、次に示すように特定の`Content-Type`ヘッダーが必要です。正しくない`Content-Type`ヘッダーを使用すると、HTTP ステータス 415（サポートされていないメディアタイプ）エラーが発生します。

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
| `currentVersion` | 既存の宛先の現在のバージョン。宛先のルックアップリクエストを実行する際の`version`属性の値。 |

**応答** 

応答には、宛先の ID や新しい`version`を含む、更新された宛先の詳細が含まれます。

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

組織で投影先が不要になった場合は、`/config/destinations`エンドポイントに DELETE リクエストを送信し、削除する宛先の ID をリクエストパスに含めることで、投影先を削除できます。 

>[!CAUTION]
>
>削除リクエストに対する API の応答は直ちにおこなわれますが、エッジ上のデータに対する実際の変更は非同期で実施されます。つまり、プロファイルデータはすべてのエッジ（投影先で指定された`dataCenters`）から削除されますが、処理の完了には時間がかかります。

**API 形式**

```http
DELETE /config/destinations/{DESTINATION_ID}
```

| パラメーター | 説明 |
|---|---|
| `{DESTINATION_ID}` | 削除する投影先の一意の ID。 |


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

削除リクエストは、HTTP ステータス 204（コンテンツなし）と空の応答本文を返します。削除が成功したことを確認するには、ID で宛先のルックアップリクエストを実行します。ルックアップは、HTTP ステータス 404（見つかりません）を返す必要があります。

## 投影設定

投影設定は、各エッジで使用可能なデータに関する情報を提供します。完全な[!DNL Experience Data Model](XDM)スキーマをエッジに投影するのではなく、投影はスキーマから特定のデータ（フィールド）のみを提供します。 組織は、各 XDM スキーマに複数の投影設定を定義できます。

### すべての投影設定のリスト

`/config/projections`エンドポイントに GET リクエストをおこなうことで、組織に対して作成されたすべての投影設定をリストできます。また、リクエストパスにオプションのパラメーターを追加して、特定のスキーマの投影設定にアクセスしたり、個々の投影をその名前でルックアップしたりすることもできます。

**API 形式**

```http
GET /config/projections
GET /config/projections?schemaName={SCHEMA_NAME}
GET /config/projections?schemaName={SCHEMA_NAME}&name={PROJECTION_NAME}
```

| パラメーター | 説明 |
|---|---|
| `{SCHEMA_NAME}` | アクセスする投影設定に関連付けられているスキーマクラスの名前。 |
| `{PROJECTION_NAME}` | アクセスする投影設定の名前。 |

>[!NOTE]
>
>`schemaName`投影設定名は、スキーマクラスのコンテキストでのみ一意であるため、`name`パラメーターを使用する場合は必須です。

**リクエスト**

次のリクエストは、[!DNL Experience Data Model]スキーマクラス[!DNL XDM Individual Profile]に関連付けられているすべての投影設定をリストします。 XDMと[!DNL Platform]内での役割の詳細については、まず「[XDMシステムの概要](../../xdm/home.md)」をお読みください。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/config/projections?schemaName=_xdm.context.profile \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

成功した応答は、ルート`_embedded`属性内の投影設定（`projectionConfigs`配列内に含まれる）を返します。組織の投影設定がおこなわれていない場合、`projectionConfigs`配列は空になります。

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

エッジで使用可能にする XDM フィールドを指定する新しい投影設定を作成（POST）できます。

**API 形式**

```http
POST /config/projections?schemaName={SCHEMA_NAME}
```

| パラメーター | 説明 |
|---|---|
| `{SCHEMA_NAME}` | アクセスする投影設定に関連付けられているスキーマクラスの名前。 |

**リクエスト**

>[!NOTE]
>
>設定を作成するPOSTリクエストには、次に示すように、特定の`Content-Type`ヘッダーが必要です。 正しくない`Content-Type`ヘッダーを使用すると、HTTP ステータス 415（サポートされていないメディアタイプ）エラーが発生します。

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
| `selector` | エッジに複製されるスキーマ内のプロパティのリストを含む文字列。セレクターの操作に関するベストプラクティスは、このドキュメントの「[セレクター](#selectors)」の節で参照できます。 |
| `name` | 新しい投影設定のわかりやすい名前。 |
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

## セレクター {#selectors}

セレクターは、XDM フィールド名のリストをコンマで区切ったものです。投影設定では、セレクターは投影に含めるプロパティを指定します。`selector`パラメーター値の形式は、XPath 構文に大まかに基づいています。次に、サポートされている構文の概要を示します。その他の参照例も示します。

### サポートされる構文

* 複数のフィールドを選択するには、コンマを使用します。スペースは使用しないでください。
* ドット表記を使用して、ネストされたフィールドを選択します。
   * たとえば、`foo`という名前のフィールド内にネストされている`field`フィールドを選択するには、セ レクター`foo.field`を使用します。
* サブフィールドを含むフィールドを含めると、すべてのサブフィールドもデフォルトで投影されます。ただし、丸括弧`"( )"`を使用して返されるサブフィールドをフィルタリングすることはできます。
   * たとえば、`addresses(type,city.country)`は、各`addresses`配列要素の住所の種類と住所の市区町村が存在する国のみを返します。
   * 上記の例は`addresses.type,addresses.city.country`と同じです。

>[!NOTE]
>
>サブフィールドの参照には、ドット表記と括弧の両方がサポートされています。ただし、ドット表記を使用する方がより簡潔で、フィールド階層のより良い説明を提供するため、この方法をお勧めします。

* セレクター内の各フィールドは、応答のルートを基準に指定します。
   * データがリソースのコレクションの場合、投影にはリソースの配列が含まれます。
   * データが単一のリソースの場合、投影にはそのリソースを基準とするフィールドが含まれます。
   * 選択したフィールドが配列（または配列の一部）の場合、投影には配列内のすべての要素の選択した部分が含まれます。

### セレクターパラメーターの例

次の例は、サンプルの`selector`パラメーターと、その後に表す構造化された値を示しています。

**person.lastName**

リクエストされたリソース内の`person`オブジェクトの`lastName`サブフィールドを返します。

```json
{
  "person": {
    "lastName": "Smith"
  }
}
```

**アドレス**

`addresses`配列内のすべての要素を返します。各要素内のすべてのフィールドが含まれますが、他のフィールドは含まれません。

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

`addresses`配列内の`person.lastName`フィールドとすべての要素を返します。

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
>
>ネストされたフィールドが返されるたびに、それを含む親オブジェクトが投影に含まれます。親フィールドも明示的に選択されていない限り、他の子フィールドは含まれません。

**addresses(type,city)**

`addresses`配列内の各要素の`type`および`city`フィールドの値のみを返します。各`addresses`要素に含まれるその他のサブフィールドはすべて除外されます。

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

このガイドでは、`selector`パラメーターを適切にフォーマットする方法など、投影と宛先を設定する手順を示しました。 組織のニーズに合った新しい投影先および設定を作成できるようになりました。
