---
title: Flow Service API を使用した Chatlio のSource接続とデータフローの作成
description: Flow Service API を使用してAdobe Experience Platformを Chatlio に接続する方法を説明します。
badge: ベータ版
exl-id: 867b8096-0841-4462-9888-e60c97c2115e
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1396'
ht-degree: 49%

---

# Flow Service API を使用した [!DNL Chatlio] のソース接続とデータフローの作成

>[!NOTE]
>
>[!DNL Chatlio] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../../../home.md#terms-and-conditions) を参照してください。

以下のチュートリアルでは、ソース接続とデータフローを作成し、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用してイベントデータ [[!DNL Chatlio]](https://chatlio.com/)Adobe Experience Platformに取り込む手順を詳しく説明します。

## はじめに {#getting-started}

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## [!DNL Flow Service] API を使用した [!DNL Chatlio] のExperience Platformへの接続 {#connect-platform-to-flow-api}

次に、ソース接続とデータフローを作成して [!DNL Chatlio] イベントデータをExperience Platformに取り込むために必要な手順の概要を説明します。

### ソース接続の作成 {#source-connection}

[!DNL Flow Service] API に POST リクエストを実行し、その際にソースの接続仕様 ID、名前や説明などの詳細、データの形式を指定することで、ソース接続を作成します。

**API 形式**

```https
POST /sourceConnections
```

**リクエスト**

次のリクエストは、[!DNL Chatlio] のソース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Streaming Source Connection for Chatlio.",
      "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
      "description": "Streaming Source Connection for Chatlio.",
      "connectionSpec": {
          "id": "073127a3-26e3-496c-9d94-9f48fb93fba8",
          "version": "1.0"
      },
      "data": {
          "format": "json"
      }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前はわかりやすいものにしてください。 |
| `description` | 含めることでソース接続に関する詳細情報を提供できるオプションの値です。 |
| `connectionSpec.id` | ソースに対応する接続仕様の ID。 |
| `data.format` | 取り込む [!DNL Chatlio] データの形式。現在、サポートされているデータ形式は `json` のみです。 |

**応答**

リクエストが成功した場合は、新たに作成されたソース接続の一意の ID（`id`）が返されます。この ID は、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "7689a40d-43eb-4f74-a3f1-092a55884f6c",
    "etag": "\"01013ed0-0000-0200-0000-63f314d00000\""
}
```

### ターゲット XDM スキーマの作成 {#target-schema}

ソースデータをExperience Platformで使用するには、必要に応じてターゲットスキーマを作成してソースデータを構造化する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれるExperience Platform データセットが作成されます。

[Schema Registry API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/) に POST リクエストを実行することで、ターゲット XDM スキーマを作成できます。

ターゲット XDM スキーマの作成手順について詳しくは、 [API を使用したスキーマの作成](https://experienceleague.adobe.com/docs/experience-platform/xdm/api/schemas.html?lang=ja#create)に関するチュートリアルを参照してください。

### ターゲットデータセットの作成 {#target-dataset}

[Catalog Service API](https://developer.adobe.com/experience-platform-apis/references/catalog/) に POST リクエストを実行し、その際にペイロード内でターゲットスキーマの ID を指定することで、ターゲットデータセットを作成できます。

ターゲットデータセットの作成手順について詳しくは、 [API を使用したデータセットの作成](https://experienceleague.adobe.com/docs/experience-platform/catalog/api/create-dataset.html?lang=ja)に関するチュートリアルを参照してください。

### ターゲット接続の作成 {#target-connection}

ターゲット接続は、取り込まれたデータが保存される宛先への接続を表します。 ターゲット接続を作成するには、データレイクに対応する固定接続仕様 ID を指定する必要があります。 この ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。

これで、一意の識別子、ターゲットスキーマ、ターゲットデータセット、およびデータレイクに対する接続仕様 ID が得られました。 これらの識別子を使用すると、受信ソースデータを格納するデータセットを指定する [!DNL Flow Service] API を使用して、ターゲット接続を作成することができます。

**API 形式**

```https
POST /targetConnections
```

**リクエスト**

次のリクエストは、[!DNL Chatlio] のターゲット接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Streaming Target Connection for a Chatlio.",
      "description": "Streaming Target Connection for a Chatlio.",
      "connectionSpec": {
          "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
          "version": "1.0"
      },
      "data": {
          "format": "json",
          "schema": {
              "id": "https://ns.adobe.com/extconndev/schemas/49cecec83dd1a8da1aef4a96c67c06654e8c337a0a3b4262",
              "version": "application/vnd.adobe.xed-full+json;version=1"
          }
      },
      "params": {
          "dataSetId": "63ef7df781f14a1bd02a7e49"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | ターゲット接続の名前。ターゲット接続の情報を検索に使用できるように、ターゲット接続はわかりやすい名前にしてください。 |
| `description` | ターゲット接続に関する詳細を提供するために含めることができるオプションの値です。 |
| `connectionSpec.id` | データレイクに対応する接続仕様 ID。 この修正済み ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。 |
| `data.format` | 取り込む [!DNL Chatlio] データの形式。 |
| `params.dataSetId` | 前の手順で取得したターゲットデータセット ID。 |

**応答**

リクエストが成功した場合は、新しいターゲット接続の一意の ID（`id`）が返されます。この ID は、後の手順で必要になります。

```json
{
    "id": "f7be8ab6-e5ea-4405-83b9-200cdfb2a9e5",
    "etag": "\"7f0072bc-0000-0200-0000-63f314a50000\""
}
```

### マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。これを実現するには、リクエストペイロード内で定義されたデータマッピングを使用して、[[!DNL Data Prep] API](https://www.adobe.io/experience-platform-apis/references/data-prep/) に対して POST リクエストを実行します。

**API 形式**

```http
POST /conversion/mappingSets
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/conversion/mappingSets' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "outputSchema": {
          "schemaRef": {
              "id": "https://ns.adobe.com/{TENANT_ID}/schemas/945546112b746524bfd9f1264b26c2b7d8e7f5b7fadb953a",
              "contentType": "application/vnd.adobe.xed-full+json;version=1"
          }
      },
      "mappings": [
        {
            "destinationXdmPath": "_extconndev.slackchannel_ID",
            "sourceAttribute": "slackChannelId",
            "identity": false,
            "version": 0
        },
        {
            "destinationXdmPath": "_extconndev.slackchannel_name",
            "sourceAttribute": "slackChannelName",
            "identity": false,
            "version": 0
        },
        {
            "destinationXdmPath": "_extconndev.UUID",
            "sourceAttribute": "visitor.UUID",
            "identity": false,
            "version": 0
        },
        {
            "destinationXdmPath": "_extconndev.chatlio_email",
            "sourceAttribute": "visitor.email",
            "identity": false,
            "version": 0
        },
        {
            "destinationXdmPath": "_extconndev.message",
            "sourceAttribute": "message",
            "identity": false,
            "version": 0
        },
        {
            "destinationXdmPath": "_extconndev.channel_ID",
            "sourceAttribute": "channelId",
            "identity": false,
            "version": 0
        }
    ]
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `outputSchema.schemaRef.id` | 以前の手順で生成された[ターゲット XDM スキーマ](#target-schema)の ID。 |
| `mappings.sourceType` | マッピングされるソース属性タイプ。 |
| `mappings.source` | 宛先 XDM パスにマッピングする必要があるソース属性。 |
| `mappings.destination` | ソース属性がマッピングされている宛先 XDM パス。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成されたマッピングの詳細が返されます。この値は、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "4b7188aad69c44529a5e674ab5d3568b",
    "version": 0,
    "createdDate": 1676875099546,
    "modifiedDate": 1676875099546,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}"
}
```

### フローの作成 {#flow}

[!DNL Chatlio] からExperience Platformにデータを取り込むための最後の手順は、データフローを作成することです。 現時点で、次の必要な値の準備ができています。

* [ソース接続 ID](#source-connection)
* [ターゲット接続 ID](#target-connection)
* [マッピング ID](#mapping)

データフローは、ソースからデータをスケジュールおよび収集する役割を果たします。ペイロードに前述の値を提供しながら POST リクエストを実行することで、データフローを作成することができます。

**API 形式**

```http
POST /flows
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Streaming Dataflow for Chatlio",
      "description": "Streaming Dataflow for Chatlio",
      "flowSpec": {
          "id": "e77fde5a-22a8-11ed-861d-0242ac120002",
          "version": "1.0"
      },
      "sourceConnectionIds": [
          "7689a40d-43eb-4f74-a3f1-092a55884f6c"
      ],
      "targetConnectionIds": [
          "f7be8ab6-e5ea-4405-83b9-200cdfb2a9e5"
      ],
      "transformations": [
      {
        "name": "Mapping",
        "params": {
          "mappingId": "4b7188aad69c44529a5e674ab5d3568b",
          "mappingVersion": 0
        }
      }
    ]
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | データフローの名前。データフローの情報を検索する際に使用できるので、データフローはわかりやすい名前にしてください。 |
| `description` | データフローの詳細を提供するために含めることができるオプションの値です。 |
| `flowSpec.id` | データフローの作成に必要なフロー仕様 ID。この修正済み ID は `e77fde5a-22a8-11ed-861d-0242ac120002` です。 |
| `flowSpec.version` | フロー仕様 ID の対応するバージョン。この値のデフォルトは `1.0` です。 |
| `sourceConnectionIds` | 以前の手順で生成された[ソース接続 ID](#source-connection)。 |
| `targetConnectionIds` | 以前の手順で生成された[ターゲット接続 ID](#target-connection)。 |
| `transformations` | このプロパティには、データに適用する必要がある様々な変換が含まれています。このプロパティは、XDM に準拠していないデータをExperience Platformに取り込む場合に必要です。 |
| `transformations.name` | 変換に割り当てられた名前。 |
| `transformations.params.mappingId` | 以前の手順で生成された[マッピング ID](#mapping)。 |
| `transformations.params.mappingVersion` | マッピング ID の対応するバージョン。この値のデフォルトは `0` です。 |

**応答**

正常な応答は、新しく作成したデータフローの ID（`id`）を返します。この ID を使用して、データフローを監視、更新または削除できます。

```json
{
    "id": "d947e6a9-ea53-42c4-985c-c9379265491f",
    "etag": "\"9b01c840-0000-0200-0000-63f3163e0000\""
}
```

### ストリーミングエンドポイント URL の取得 {#get-streaming-endpoint}

データフローを作成したので、ストリーミングエンドポイント URL を取得できるようになりました。 このエンドポイント URL を使用して、ソースを Webhook に登録し、ソースがExperience Platformと通信できるようにします。

ストリーミングエンドポイント URL を取得するには、`/flows` エンドポイントに対してGET リクエストを実行し、データフローの ID を指定します。

**API 形式**

```http
GET /flows/{FLOW_ID}
```

**リクエスト**

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/flows/4982698b-e6b3-48c2-8dcf-040e20121fd2' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、`inletUrl` とマークされたエンドポイント URL を含む、データフローに関する情報が返されます。 必要な値を取得するには、[Webhook の設定 ](../../../ui/create/marketing-automation/chatlio-webhook.md#get-streaming-endpoint-url) ページを参照してください。

```json
{
    "items": [
        {
            "id": "d947e6a9-ea53-42c4-985c-c9379265491f",
            "createdAt": 1676875325841,
            "updatedAt": 1676875331938,
            "createdBy": "acme@AdobeID",
            "updatedBy": "acme@AdobeID",
            "createdClient": "{CREATED_CLIENT}",
            "updatedClient": "{UPDATED_CLIENT}",
            "sandboxId": "{SANDBOX_ID}",
            "sandboxName": "{SANDBOX_NAME}",
            "imsOrgId": "{ORG_ID}",
            "name": "Streaming Dataflow for Chatlio",
            "description": "Streaming Dataflow for Chatlio",
            "flowSpec": {
                "id": "e77fde5a-22a8-11ed-861d-0242ac120002",
                "version": "1.0"
            },
            "state": "enabled",
            "version": "\"9b012541-0000-0200-0000-63f316430000\"",
            "etag": "\"9b012541-0000-0200-0000-63f316430000\"",
            "sourceConnectionIds": [
                "7689a40d-43eb-4f74-a3f1-092a55884f6c"
            ],
            "targetConnectionIds": [
                "f7be8ab6-e5ea-4405-83b9-200cdfb2a9e5"
            ],
            "inheritedAttributes": {
                "properties": {
                    "isSourceFlow": true
                },
                "sourceConnections": [
                    {
                        "id": "7689a40d-43eb-4f74-a3f1-092a55884f6c",
                        "connectionSpec": {
                            "id": "073127a3-26e3-496c-9d94-9f48fb93fba8",
                            "version": "1.0"
                        }
                    }
                ],
                "targetConnections": [
                    {
                        "id": "f7be8ab6-e5ea-4405-83b9-200cdfb2a9e5",
                        "connectionSpec": {
                            "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
                            "version": "1.0"
                        }
                    }
                ]
            },
            "options": {
                "inletUrl": "https://dcs.adobedc.net/collection/e71b6a6cd7270388674f8ab68ee438da58ba4434dea63cc547ee21547275923c"
            },
            "transformations": [
                {
                    "name": "Mapping",
                    "params": {
                        "mappingId": "4b7188aad69c44529a5e674ab5d3568b",
                        "mappingVersion": 0
                    }
                }
            ],
            "runs": "/runs?property=flowId==d947e6a9-ea53-42c4-985c-c9379265491f",
            "providerRefId": "bfac26f5-9256-438c-b1a0-e07a7dc675f6",
            "lastOperation": {
                "started": 0,
                "updated": 0,
                "operation": "enable"
            }
        }
    ]
}
```

## 付録 {#appendix}

次の節では、データフローの監視、更新、削除を行う手順について説明します。

### データフローの監視 {#monitor-dataflow}

データフローが作成されると、それを通して取り込まれるデータを監視し、フローの実行状況、完了状況、エラーなどの情報を確認することができます。完全な API の例については、[API を使用したソースデータフローのモニタリング ](https://experienceleague.adobe.com/docs/experience-platform/sources/api-tutorials/monitor.html?lang=ja) に関するガイドを参照してください。

### データフローの更新 {#update-dataflow}

データフローの ID を指定しながら、API の `/flows` エンドポイントに対してPATCH リクエストを実行することで、名前や説明、実行スケジュールおよび関連するマッピングセットなど、データフローの詳細 [!DNL Flow Service] 更新します。 PATCH リクエストを行う場合は、データフローの一意の `etag` を `If-Match` ヘッダーで指定する必要があります。 完全な API の例については、[API を使用したソースデータフローの更新 ](https://experienceleague.adobe.com/docs/experience-platform/sources/api-tutorials/update-dataflows.html?lang=ja) に関するガイドを参照してください

### アカウントを更新 {#update-account}

ベース接続 ID をクエリパラメーターとして指定して [!DNL Flow Service] API に対してPATCH リクエストを実行することで、ソースアカウントの名前、説明、資格情報を更新します。 PATCH リクエストを行う場合は、ソースアカウントの一意の `etag` を `If-Match` ヘッダーで指定する必要があります。 完全な API の例については、[API を使用したソースアカウントの更新 ](https://experienceleague.adobe.com/docs/experience-platform/sources/api-tutorials/update.html?lang=ja) に関するガイドを参照してください。

### データフローの削除 {#delete-dataflow}

クエリパラメーターの一部として削除するデータフローの ID を指定したうえで [!DNL Flow Service] API に対してDELETE リクエストを実行することで、データフローを削除します。 完全な API の例については、[API を使用したデータフローの削除 ](https://experienceleague.adobe.com/docs/experience-platform/sources/api-tutorials/delete-dataflows.html?lang=ja) に関するガイドを参照してください。

### アカウントを削除 {#delete-account}

[!DNL Flow Service] API にDELETE リクエストを実行し、その際に削除するアカウントのベース接続 ID を指定することで、アカウントを削除します。 完全な API の例については、[API を使用したソースアカウントの削除 ](https://experienceleague.adobe.com/docs/experience-platform/sources/api-tutorials/delete.html?lang=ja) に関するガイドを参照してください。
