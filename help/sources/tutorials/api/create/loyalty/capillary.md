---
title: Flow Service APIを使用してCapillaryをExperience Platformに接続する
description: APIを使用してCapillaryをExperience Platformに接続する方法について説明します。
badge: ベータ版
exl-id: 763792d0-d5dc-40ac-b86a-6a0d26463b71
source-git-commit: e4ee4accdb28dafda7e37625eb84062bb6e53644
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 9%

---

# [!DNL Capillary Streaming Events] APIを使用して[!DNL Flow Service]をExperience Platformに接続します

>[!AVAILABILITY]
>
>[!DNL Capillary Streaming Events] ソースはベータ版です。ベータ版のソースの使用について詳しくは、ソースの概要の[条件](../../../../home.md#terms-and-conditions)を参照してください。

このガイドでは、[!DNL Capillary Streaming Events]と[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/)を使用して、[!DNL Capillary] アカウントからAdobe Experience Platformにデータをストリーミングする方法について説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [&#x200B; ソース &#x200B;](../../../../home.md): Experience Platformを使用すると、様々なソースからデータを取り込むことができますが、Experience Platform サービスを使用して着信データを構造化、ラベル付け、強化することができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md): Experience Platformは、1つのExperience Platform インスタンスを個別のバーチャル環境に分割して、デジタルエクスペリエンスアプリケーションの開発と進化に役立つバーチャルサンドボックスを提供します。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL Capillary Streaming Events] 概要](../../../../connectors/loyalty/capillary.md)を参照してください。

### Experience Platform APIの使用

Experience Platform APIを正常に呼び出す方法について詳しくは、[Experience Platform APIの概要](../../../../../landing/api-guide.md)に関するガイドを参照してください。

>[!BEGINSHADEBOX]

## 開発者プロセスチェックリスト

1. スキーマレジストリを使用して、ターゲット **Experience Data Model （XDM） スキーマ**&#x200B;を作成または選択します。 このXDM スキーマを使用して、カタログサービスで&#x200B;**データセット**&#x200B;を作成します。
2. **ベース接続**&#x200B;を作成して、[!DNL Capillary]資格情報を保存します。
3. **にバインドする** ソース接続`baseConnectionId`を作成します。
4. データがデータレイクに格納されるように、**ターゲット接続**&#x200B;を作成します。
5. データ準備を使用して、[!DNL Capillary] ソースフィールドを正しいXDM フィールドにマッピングするマッピングを作成します。
6. `sourceConnectionId`、`targetConnectionId`、`mappingID`を使用してデータフローを作成します
7. 単一のサンプルプロファイル/トランザクションイベントでテストして、データフローを検証します。

>[!ENDSHADEBOX]

## ベース接続の作成 {#base-connection}

ベース接続には、資格情報と接続の詳細が保持されます。 [!DNL Capillary]のベース接続を作成するには、`/connections` APIの[!DNL Flow Service] エンドポイントに対してPOST リクエストを行い、リクエスト本文に[!DNL Capillary]資格情報を指定します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Capillary] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Capillary base connection",
    "description": "Base connection to authenticate the [!DNL Capillary] source.",
    "connectionSpec": {
      "id": "6360f136-5980-4111-8bdf-15d29eab3b5a",
      "version": "1.0"
    },
    "auth": {
      "specName": "OAuth generic-rest-connector",
      "params": {
        "clientId": "{CLIENT_ID}",
        "clientSecret": "{CLIENT_SECRET}",
        "accessToken": "{ACCESS_TOKEN}"
      }
    }
  }'
```

**応答**

```
A successful response returns the newly created base connection, including its unique connection identifier (id). This ID is required to explore your source's file structure and contents in the next step.

{
     "id": "70383d02-2777-4be7-a309-9dd6eea1b46d",
     "etag": "\"d64c8298-add4-4667-9a49-28195b2e2a84\""
}
```

### ソース接続の作成

ソース接続を作成するには、ベース接続IDを指定しながら、`/sourceConnections` エンドポイントにPOST リクエストを行います。

**API 形式**

```http
POST /flowservice/sourceConnections
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
      "name": "Capillary Streaming",
      "description": "Capillary Streaming",
      "baseConnectionId": "70383d02-2777-4be7-a309-9dd6eea1b46d",
      "connectionSpec": {
          "id": "6360f136-5980-4111-8bdf-15d29eab3b5a",
          "version": "1.0"
      }
    }'
```

**応答**

応答が成功すると、HTTP ステータス 201が、新しく作成されたソース接続の詳細（一意の識別子（`id`）を含む）で返されます。

```json
{
  "id": "34ece231-294d-416c-ad2a-5a5dfb2bc69f",
  "etag": "\"d505125b-0000-0200-0000-637eb7790000\""
}
```

### スキーマ設定

>[!BEGINTABS]

>[!TAB  プロファイル取り込み]

プロファイルには、IDとロイヤルティ属性が含まれます。 [!DNL Capillary] プロファイルスキーマに基づく例の次のペイロードを表示します。 このスキーマを設定してXDM個人プロファイルにマッピングできます。

**リクエスト**

```json
{
  "identityMap": {
    "email": [
      {
        "authenticatedState": "ambiguous",
        "id": "john.doe@capillarytech.com",
        "primary": true
      }
    ]
  },
  "loyalty": {
    "tier": "gold",
    "points": 1250,
    "lifetimePoints": 122,
    "expiredPoints": 12,
    "pointsRedeemed": 500,
    "program": "loyalty program name",
    "status": "active"
  }
}
```

**応答**

```json
{
  "id": "8c19f1c3-4b91-47cd-8cb5-b152a93f7349",
  "status": "success",
  "message": "Profile record ingested successfully"
}
```

>[!TAB  トランザクション取得]

トランザクションはコマースアクティビティをキャプチャします。 [!DNL Capillary] イベントスキーマに基づく例の次のペイロードを表示します。 このスキーマを設定してXDM エクスペリエンスイベントにマッピングできます。

**リクエスト**

```json
{
  "_id": "T0001",
  "timestamp": "2025-07-14T12:00:00-06:00",
  "identityMap": {
    "email": [
      {
        "authenticatedState": "ambiguous",
        "id": "john@capillarytech.com",
        "primary": true
      }
    ]
  },
  "commerce": {
    "commerceScope": {
      "storeCode": "HSR"
    },
    "order": {
      "priceTotal": 90
    }
  },
  "productLineItems": [
    {
      "SKU": "sku_01",
      "quantity": 1,
      "priceTotal": 100,
      "name": "Kitkat",
      "discountAmount": 10
    }
  ]
}
```

**応答**

```json
{
  "id": "T0001",
  "status": "success",
  "message": "Transaction event ingested successfully"
}
```

>[!ENDTABS]

<!--
### Supported Events

The [!DNL Capillary] source supports the following events:

* `pointsIssued`
* `tierDowngraded`
* `tierUpgraded`
* `pointsExpiryChange`
* `pointsExpired`
* `transactionUpdated`
* `customerAdded`
* `tierDowngradeReminder`
* `promotionEarned`
* `pointsExpiryReminder`
* `pointsRedeemed`
* `transactionAdded`
* `tierRenewed`
* `customerUpdated`
-->

### 履歴データの移行

Experience Platformに過去のロイヤルティデータと取引データを取り込むことができます。 データを[!DNL Capillary]から構造化CSV ファイルとしてエクスポートし、[!DNL SFTP]を使用して安全に転送し、Experience Platform データセットに取り込むだけです。 初回移行後も、イベント駆動型コネクタを使用してデータをリアルタイムで最新の状態に保つことができます。

### ターゲット XDM スキーマの作成 {#target-schema}

Experience Data Model （XDM）スキーマは、Experience Platform内の顧客体験データを整理および記述するための標準化された方法を提供します。 ソースデータをExperience Platformに取り込むには、まず、取り込むデータの構造とタイプを定義するターゲット XDM スキーマを作成する必要があります。 このスキーマは、取り込んだデータが格納されるExperience Platform データセットの設計図として機能します。

ターゲット XDM スキーマは、[Schema Registry API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/)に対してPOST リクエストを実行することで作成できます。 ターゲット XDM スキーマの作成方法について詳しくは、次のガイドを参照してください。

* [API](../../../../../xdm/api/schemas.md)を使用してスキーマを作成します。
* [UI](../../../../../xdm/tutorials/create-schema-ui.md)を使用してスキーマを作成します。

作成したターゲット XDM スキーマ `$id`は、後でターゲットデータセットとマッピングに必要になります。

## ターゲットデータセットの作成 {#target-dataset}

データセットは、データのコレクションのための保存および管理構造体です。通常、列（スキーマ）と行（フィールド）を持つテーブルのように構造化されます。 Experience Platformに正常に取り込まれたデータは、データセットとしてデータレイク内に保存されます。 この手順では、新しいデータセットを作成するか、既存のデータセットを使用できます。

ペイロード内でターゲットスキーマのIDを指定しながら、[&#x200B; カタログサービス API](https://developer.adobe.com/experience-platform-apis/references/catalog/)にPOST リクエストを行うことで、ターゲットデータセットを作成できます。 ターゲットデータセットの作成方法について詳しくは、[APIを使用したデータセットの作成](../../../../../catalog/api/create-dataset.md)に関するガイドを参照してください。


## ターゲット接続の作成 {#target}

ターゲット接続は、取り込まれたデータが取り込まれる宛先への接続を表します。 ターゲット接続を作成するには、データレイクに関連付けられた固定接続仕様IDを指定する必要があります。 この接続仕様IDは`c604ff05-7f1a-43c0-8e18-33bf874cb11c`です。

**API 形式**

```http
POST /targetConnections
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Capillary Target Connection",
      "description": "Capillary Target Connection",
      "data": {
          "schema": {
              "id": "https://ns.adobe.com/{TENANT_ID}/schemas/52b59140414aa6a370ef5e21155fd7a686744b8739ecc168",
              "version": "application/vnd.adobe.xed-full+json;version=1"
          }
      },
      "params": {
          "dataSetId": "6889f4f89b982b2b90bc1207"
      },
      "connectionSpec": {
          "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
          "version": "1.0"
      }
    }'
```

### マッピングの作成 {#mapping}

次に、ソースデータを、ターゲットデータセットが準拠するターゲットスキーマにマッピングします。 マッピングを作成するには、`mappingSets`API[[!DNL Data Prep] の](https://developer.adobe.com/experience-platform-apis/references/data-prep/) エンドポイントにPOST リクエストを行います。 ターゲット XDM スキーマ IDと、作成するマッピングセットの詳細を含めます。

次のように、Capillary フィールドを対応するXDM スキーマフィールドにマッピングします。

| ソーススキーマ | ターゲットスキーマ |
|------------------------------|-------------------------------|
| `identityMap.email.id` | `xdm:identityMap.email[0].id` |
| `loyalty.points` | `xdm:loyalty.points` |
| `loyalty.tier` | `xdm:loyalty.tier` |
| `commerce.order.priceTotal` | `xdm:commerce.order.priceTotal` |
| `productLineItems.SKU` | `xdm:productListItems.SKU` |

>[!TIP]
>
>データをマッピングする準備ができたら、[および](../../../../images/tutorials/create/capillary/mappings.zip) データ準備[!DNL Capillary]にファイルをインポートするための[&#x200B; イベントおよびプロファイル マッピング &#x200B;](../../../../../data-prep/ui/mapping.md#import-mapping)をダウンロードできます。

### データフローの作成 {#flow}

ソース接続、マッピング、およびターゲット接続を作成した後、データフローを設定して、[!DNL Capillary]からExperience Platformにデータを移動できます。

一般的なデータフローには、次のようなものがあります。

* **プロファイルデータフロー**: [!DNL Capillary] プロファイルデータをXDM個人プロファイルデータセットに取り込みます。
* **トランザクションデータフロー**: [!DNL Capillary]個のトランザクションデータをXDM ExperienceEvent データセットに取り込みます。

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "Capillary dataflow",
    "description": "Capillary → Experience Platform dataflow",
    "flowSpec": {
      "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
      "version": "1.0"
    },
    "sourceConnectionIds": "{SOURCE_CONNECTION_ID}",
    "targetConnectionIds": "{TARGET_CONNECTION_ID}",
    "transformations": [
      {
        "name": "Mapping",
        "params": {
          "mappingId": "{MAPPING_ID}",
          "mappingVersion": "0"
        }
      }
    ],
    "scheduleParams": {
      "startTime": "1625040887",
      "frequency": "minute",
      "interval": 15
    }
  }'
```

>[!NOTE]
>
>`startTime`はUNIX エポック秒です。

**応答**

応答が成功すると、対応するデータフローIDを含むデータフローが返されます。

```json
{
  "id": "92f11b8c-0a9f-45a9-8239-60b4e8430a88",
  "status": "enabled",
  "message": "Dataflow created successfully"
}
```

## エラー処理

コネクタには、次のシナリオに対する堅牢なエラー処理が含まれています。

* **認証エラー**：認証が失敗すると、Adobe資格情報が自動的に更新されます。
* **レート制限エラー**: API レート制限に達した場合に、指数関数的なバックオフで再試行を実装します。
* **ネットワークエラー**：失敗したネットワーク要求を記録し、再試行します。
* **データ検証エラー**：手動でのレビューと解決のために無効なペイロードをログに記録します。

トラブルシューティングとデバッグを容易にするために、エラータイプ、タイムスタンプ、リクエストペイロード、Adobe API レスポンスなどの詳細を使用してすべてのエラーを記録します。

## 接続をテストする

接続をテストする手順については、次の手順に従ってください。

* GET リクエストを`/connections/{BASE_CONNECTION_ID}`に送信し、ベース接続が存在することを確認するためにベース接続IDを指定します。 この手順では、ベース接続のステータスが`active`に設定されていることを確認することもできます。
* `/flowservice/sourceConnections/{SOURCE_CONNECTION_ID}`にGET リクエストを行い、ソース接続を検証するためにソース接続IDを指定します。
* ストリーミングエンドポイント URLを使用して、サンプルプロファイルペイロードを送信します（プロファイル取り込みJSONを使用）。
* Experience Platform UIでデータセットに移動し、データセットに対してクエリを実行して、レコードを確認します。
* データ準備ログを使用して、エラーを検査します。
* サポートチケットを発行する必要がある場合は、以下を確認してください。
   * リクエストペイロード
   * 応答本文
   * Request-id
   * タイムスタンプ
   * リソース ID。

## 付録

その他の操作について詳しくは、次のドキュメントを参照してください

* [データフローの監視](../../../../../dataflows/ui/monitor-sources.md)
* [&#x200B; データフローの更新](../../../ui/update-dataflows.md)
* [&#x200B; データフローを削除](../../../ui/delete.md)
* [&#x200B; ソースアカウントを更新](../../../ui/update.md)
