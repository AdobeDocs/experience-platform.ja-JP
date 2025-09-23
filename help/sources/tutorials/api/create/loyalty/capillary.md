---
title: Flow Service API を使用した Capilary とExperience Platformの接続
description: API を使用してキャピラリーをExperience Platformに接続する方法を説明します。
badge: ベータ版
exl-id: 763792d0-d5dc-40ac-b86a-6a0d26463b71
source-git-commit: 91d6206c6ce387fde365fa72dc79ca79fc0e46fa
workflow-type: tm+mt
source-wordcount: '1150'
ht-degree: 9%

---

# [!DNL Capillary Streaming Events] API を使用した [!DNL Flow Service] のExperience Platformへの接続

>[!AVAILABILITY]
>
>[!DNL Capillary Streaming Events] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、ソースの概要の [ 利用条件 ](../../../../home.md#terms-and-conditions) を参照してください。

このガイドを読んで、[!DNL Capillary Streaming Events] と [[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/) を使用して [!DNL Capillary] アカウントからAdobe Experience Platformにデータをストリーミングする方法を学びます。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL Capillary Streaming Events]  概要 ](../../../../connectors/loyalty/capillary.md) を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法については、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) に関するガイドを参照してください。

>[!BEGINSHADEBOX]

## 開発者プロセスチェックリスト

1. スキーマレジストリを使用して、ターゲット **エクスペリエンスデータモデル（XDM）スキーマ** 作成または選択します。 この XDM スキーマを使用して、カタログサービスで **データセットを作成** します。
2. **ベース接続** を作成して、[!DNL Capillary] 資格情報を保存します。
3. **にバインドする** ソース接続 `baseConnectionId` を作成します。
4. **ターゲット接続** を作成して、データレイクにデータが確実に取り込まれるようにします。
5. データ準備を使用して、[!DNL Capillary] ソースフィールドを正しい XDM フィールドにマッピングするマッピングを作成します。
6. `sourceConnectionId`、`targetConnectionId`、`mappingID` を使用してデータフローを作成します
7. 単一のサンプルプロファイル/トランザクションイベントでテストし、データフローを検証します。

>[!ENDSHADEBOX]

## ベース接続の作成 {#base-connection}

ベース接続は、資格情報と接続の詳細を保持します。 [!DNL Capillary] のベース接続を作成するには、`/connections` API の [!DNL Flow Service] エンドポイントに対して POST リクエストを実行し、リクエスト本文に [!DNL Capillary] 資格情報を指定します。

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

ソース接続を作成するには、ベース接続 ID を指定したうえで、`/sourceConnections` エンドポイントに対して POST リクエストを行います。

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

リクエストが成功した場合は、HTTP ステータス 201 と、一意の ID （`id`）を含む新しく作成されたソース接続の詳細が返されます。

```json
{
  "id": "34ece231-294d-416c-ad2a-5a5dfb2bc69f",
  "etag": "\"d505125b-0000-0200-0000-637eb7790000\""
}
```

### スキーマ設定

>[!BEGINTABS]

>[!TAB  プロファイルの取得 ]

プロファイルには、ID 属性とロイヤルティ属性が含まれています。 [!DNL Capillary] プロファイルスキーマに基づいた例については、次のペイロードを参照してください。 このスキーマを設定して、XDM 個人プロファイルにマッピングできます。

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

>[!TAB  トランザクションの取得 ]

トランザクションは、コマースアクティビティをキャプチャします。 [!DNL Capillary] イベントスキーマに基づいた例については、次のペイロードを参照してください。 このスキーマを設定して、XDM エクスペリエンスイベントにマッピングできます。

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

<!--### Supported Events

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
* `customerUpdated`-->

### 履歴データの移行

ロイヤルティの履歴データと取引データをExperience Platformに取り込むことができます。 データを構造化された CSV ファイルとして [!DNL Capillary] から書き出し、[!DNL SFTP] を使用して安全に転送し、Experience Platform データセットに取り込むだけです。 最初の移行が完了すると、イベント駆動型コネクタを通じてデータがリアルタイムで最新の状態に保たれます。

### ターゲット XDM スキーマの作成 {#target-schema}

エクスペリエンスデータモデル（XDM）スキーマは、Experience Platform内でカスタマーエクスペリエンスのデータを整理および記述するための標準化された方法を提供します。 ソースデータをExperience Platformに取り込むには、まず取り込むデータの構造とタイプを定義するターゲット XDM スキーマを作成する必要があります。 このスキーマは、取り込んだデータが存在するExperience Platform データセットのブループリントとして機能します。

[Schema Registry API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/) に POST リクエストを行うことで、ターゲット XDM スキーマを作成することができます。 ターゲット XDM スキーマの作成方法に関する詳細な手順については、次のガイドを参照してください。

* [API を使用したスキーマの作成 ](../../../../../xdm/api/schemas.md)。
* [UI を使用したスキーマの作成 ](../../../../../xdm/tutorials/create-schema-ui.md)。

作成したら、後でターゲットデータセットとマッピングにターゲット XDM スキーマ `$id` が必要になります。

## ターゲットデータセットの作成 {#target-dataset}

データセットは、データのコレクションのためのストレージと管理の構成体で、通常は、列（スキーマ）と行（フィールド）を持つテーブルのように構造化されます。 Experience Platformに正常に取り込まれたデータは、データセットとしてデータレイク内に保存されます。 この手順では、新しいデータセットを作成するか、既存のデータセットを使用します。

ペイロードにターゲットスキーマの ID を指定しながら [Catalog Service API](https://developer.adobe.com/experience-platform-apis/references/catalog/) に対して POST リクエストを実行することで、ターゲットデータセットを作成できます。 ターゲットデータセットの作成手順について詳しくは、[API を使用したデータセットの作成 ](../../../../../catalog/api/create-dataset.md) に関するガイドを参照してください。


## ターゲット接続の作成 {#target}

ターゲット接続は、取り込まれたデータが取り込まれる宛先への接続を表します。 ターゲット接続を作成するには、データレイクに関連付けられた固定接続仕様 ID を指定する必要があります。 この接続仕様 ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。

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

次に、ターゲットデータセットが準拠するターゲットスキーマにソースデータをマッピングします。 マッピングを作成するには、`mappingSets`API[[!DNL Data Prep]  の ](https://developer.adobe.com/experience-platform-apis/references/data-prep/) エンドポイントに対して POST リクエストを実行します。 ターゲット XDM スキーマ ID と、作成するマッピングセットの詳細を含めます。

次のように、キャピラリフィールドを対応する XDM スキーマフィールドにマッピングします。

| ソーススキーマ | ターゲットスキーマ |
|------------------------------|-------------------------------|
| `identityMap.email.id` | `xdm:identityMap.email[0].id` |
| `loyalty.points` | `xdm:loyalty.points` |
| `loyalty.tier` | `xdm:loyalty.tier` |
| `commerce.order.priceTotal` | `xdm:commerce.order.priceTotal` |
| `productLineItems.SKU` | `xdm:productListItems.SKU` |

>[!TIP]
>
>データをマッピングする準備が整ったら、[ イベントとプロファイルのマッピング ](../../../../images/tutorials/create/capillary/mappings.zip) をダウンロードして、[!DNL Capillary] および [ ファイルをデータ準備にインポート ](../../../../../data-prep/ui/mapping.md#import-mapping) できます。

### データフローの作成 {#flow}

ソース接続、マッピング、ターゲット接続を作成したら、データフローを設定して、[!DNL Capillary] からExperience Platformにデータを移動できます。

一般的なデータフローを次に示します。

* **プロファイルデータフロー**:[!DNL Capillary] プロファイルデータを XDM 個人プロファイルデータセットに取り込みます。
* **トランザクションデータフロー**：トランザクションデータ [!DNL Capillary]XDM ExperienceEvent データセットに取り込みます。

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
>`startTime` は UNIX エポック秒単位で表示されます。

**応答**

正常な応答は、対応するデータフロー ID を含むデータフローを返します。

```json
{
  "id": "92f11b8c-0a9f-45a9-8239-60b4e8430a88",
  "status": "enabled",
  "message": "Dataflow created successfully"
}
```

## エラー処理

このコネクタには、次のシナリオに対応する堅牢なエラー処理が含まれています。

* **認証エラー**：認証が失敗すると、Adobe資格情報を自動更新します。
* **レート制限エラー**:API レート制限に達した場合に、指数バックオフを使用して再試行を実装します。
* **ネットワークエラー**：失敗したネットワークリクエストをログに記録して再試行します。
* **データ検証エラー**：手動でのレビューおよび解決のために、無効なペイロードをログに記録します。

すべてのエラーは、トラブルシューティングとデバッグを容易にするために、エラータイプ、タイムスタンプ、リクエストペイロード、Adobe API 応答などの詳細と共にログに記録されます。

## 接続のテスト

接続をテストするための手順を以下に示します。

* `/connections/{BASE_CONNECTION_ID}` にGET リクエストを送信し、ベース接続 ID を入力して、ベース接続が存在することを確認します。 この手順では、ベース接続のステータスが `active` に設定されていることを確認することもできます。
* `/flowservice/sourceConnections/{SOURCE_CONNECTION_ID}` にGET リクエストを送信し、ソース接続 ID を指定して、ソース接続を検証します。
* ストリーミングエンドポイント URL を使用して、サンプルプロファイルペイロードを送信します（プロファイル取り込み JSON を使用）。
* Experience Platform UI のデータセットに移動し、データセットに対してクエリを実行してレコードを確認します。
* データ準備ログを使用して、エラーを調べます。
* サポートチケットを開く必要がある場合は、次の点を確認します。
   * リクエストペイロード
   * 応答本文
   * リクエスト ID
   * タイムスタンプ
   * リソース ID。

## 付録

その他の操作については、次のドキュメントを参照してください

* [データフローの監視](../../../../../dataflows/ui/monitor-sources.md)
* [ データフローの更新 ](../../../ui/update-dataflows.md)
* [ データフローの削除 ](../../../ui/delete.md)
* [ ソースアカウントを更新 ](../../../ui/update.md)
