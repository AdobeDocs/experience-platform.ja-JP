---
keywords: Experience Platform；ホーム；人気のあるトピック；イベントハブ；Azure イベントハブ；イベントハブ
solution: Experience Platform
title: フローサービス API を使用した Azure Event Hubs ソース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを Azure Event Hubs アカウントに接続する方法を説明します。
exl-id: a4d0662d-06e3-44f3-8cb7-4a829c44f4d9
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '719'
ht-degree: 8%

---


# [!DNL Flow Service] API を使用して [!DNL Azure Event Hubs] ソース接続を作成する

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Azure Event Hubs]（以下「[!DNL Event Hubs]」と呼びます）をExperience Platformに接続する手順について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

- [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
- [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL Event Hubs] を Platform に正しく接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL Event Hubs] アカウントと接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `sasKeyName` | 承認規則の名前。SAS キー名とも呼ばれます。 |
| `sasKey` | 生成された共有アクセス署名。 |
| `namespace` | アクセスする [!DNL Event Hubs] の名前空間。 [!DNL Event Hubs] 名前空間は一意のスコープコンテナを提供し、そこで 1 つ以上の [!DNL Event Hubs] を作成できます。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Event Hubs] 接続仕様 ID は次のとおりです。`bf9f5905-92b7-48bf-bf20-455bc6b60a4e`. |

これらの値の詳細については、[ この Event Hubs のドキュメント ](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ソース接続を作成する最初の手順は、[!DNL Event Hubs] ソースを認証し、ベース接続 ID を生成することです。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を識別できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL Event Hubs] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

**API 形式**

```http
POST /connections
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Azure Event Hubs connection",
        "description": "Connector for Azure Event Hubs",
        "auth": {
            "specName": "Azure EventHub authentication credentials",
            "params": {
                "sasKeyName": "{SAS_KEY_NAME}",
                "sasKey": "{SAS_KEY}",
                "namespace": "{NAMESPACE}"
            }
        },
        "connectionSpec": {
            "id": "bf9f5905-92b7-48bf-bf20-455bc6b60a4e",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.sasKeyName` | 承認規則の名前。SAS キー名とも呼ばれます。 |
| `auth.params.sasKey` | 生成された共有アクセス署名。 |
| `auth.params.namespace` | アクセスする [!DNL Event Hubs] の名前空間。 |
| `connectionSpec.id` | [!DNL Event Hubs] 接続仕様 ID は次のとおりです。`bf9f5905-92b7-48bf-bf20-455bc6b60a4e` |

**応答**

正常な応答は、新しく作成されたベース接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この接続 ID は、次の手順でソース接続を作成する際に必要です。

```json
{
    "id": "4cdbb15c-fb1e-46ee-8049-0f55b53378fe",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## ソース接続の作成

ソース接続は、データの取り込み元の外部ソースへの接続を作成し、管理します。 ソース接続は、データソース、データ形式、データフローの作成に必要なソース接続 ID などの情報で構成されます。 ソース接続インスタンスは、テナントと IMS 組織に固有です。

ソース接続を作成するには、[!DNL Flow Service] API の `/sourceConnections` エンドポイントにPOSTリクエストを実行します。

**API 形式**

```http
POST /sourceConnections
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
    -H 'authorization: Bearer {ACCESS_TOKEN}' \
    -H 'content-type: application/json' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_Org}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -d '{
        "name": "Azure Event Hubs source connection",
        "description": "A source connection for Azure Event Hubs",
        "baseConnectionId": "4cdbb15c-fb1e-46ee-8049-0f55b53378fe",
        "connectionSpec": {
            "id": "bf9f5905-92b7-48bf-bf20-455bc6b60a4e",
            "version": "1.0"
        },
        "data": {
            "format": "json"
        },
        "params": {
            "eventHubName": "{EVENTHUB_NAME}",
            "dataType": "raw",
            "reset": "latest",
            "consumerGroup": "{CONSUMER_GROUP}"
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前がわかりやすいことを確認します。 |
| `description` | ソース接続に関する詳細情報を含めるために指定できるオプションの値です。 |
| `baseConnectionId` | 前の手順で生成した [!DNL Event Hubs] ソースの接続 ID。 |
| `connectionSpec.id` | [!DNL Event Hubs] の固定接続仕様 ID。 この ID は、です。`bf9f5905-92b7-48bf-bf20-455bc6b60a4e`. |
| `data.format` | 取り込む [!DNL Event Hubs] データの形式。 現在サポートされているデータ形式は `json` のみです。 |
| `params.eventHubName` | [!DNL Event Hubs] ソースの名前。 |
| `params.dataType` | このパラメーターは、取り込まれるデータのタイプを定義します。 次のようなデータタイプがサポートされています。`raw` と `xdm`。 |
| `params.reset` | このパラメーターは、データの読み取り方法を定義します。 `latest` を使用して最新のデータから読み取りを開始し、`earliest` を使用してストリーム内の最初に使用可能なデータから読み取りを開始します。 このパラメーターはオプションで、指定しない場合はデフォルトで `earliest` になります。 |
| `params.consumerGroup` | [!DNL Event Hubs] に使用するパブリッシュまたはサブスクリプションのメカニズム。 このパラメーターはオプションで、指定しない場合はデフォルトで `$Default` になります。 詳しくは、この [[!DNL Event Hubs]  イベントコンシューマーのガイド ](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-features#event-consumers) を参照してください。 |

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Event Hubs] ソース接続を作成しました。 次のチュートリアルでこのソース接続 ID を使用して、 [!DNL Flow Service] API](../../collect/streaming.md) を使用してストリーミングデータフローを作成できます。[
