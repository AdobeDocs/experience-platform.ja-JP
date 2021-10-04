---
keywords: Experience Platform；ホーム；人気のあるトピック；Kinesis;kinesis;Amazon Kinesis;amazon kinesis
solution: Experience Platform
title: フローサービス API を使用したAmazon Kinesisソース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して、Adobe Experience PlatformをAmazon Kinesisソースに接続する方法を説明します。
exl-id: 64da8894-12ac-45a0-b03e-fe9b6aa435d3
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 8%

---

# フローサービス API を使用して [!DNL Amazon Kinesis] ソース接続を作成する

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Amazon Kinesis]（以下「[!DNL Kinesis]」と呼びます）をExperience Platformに接続する手順について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)[!DNL Platform]：Experience は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL Kinesis] を Platform に正しく接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL Amazon Kinesis] アカウントと接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `accessKeyId` | アクセスキー ID は、Platform への [!DNL Kinesis] アカウントの認証に使用するアクセスキーペアの半分です。 |
| `secretKey` | 秘密アクセスキーは、[!DNL Kinesis] アカウントを Platform に認証するために使用するアクセスキーペアの残りの半分です。 |
| `region` | [!DNL Kinesis] アカウントの地域です。 地域の詳細については、[許可リストへの IP アドレスの追加 ](../../../../ip-address-allow-list.md) のガイドを参照してください。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Kinesis] 接続仕様 ID は次のとおりです。`86043421-563b-46ec-8e6c-e23184711bf6`. |

[!DNL Kinesis] アクセスキーとその生成方法の詳細については、この [[!DNL AWS] IAM ユーザーのアクセスキーの管理に関するガイド ](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ソース接続を作成する最初の手順は、[!DNL Kinesis] ソースを認証し、ベース接続 ID を生成することです。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を識別できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL Kinesis] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

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
        "name": "Amazon Kinesis connection",
        "description": "Connector for Amazon Kinesis",
        "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
        "auth": {
            "specName": "Aws Kinesis authentication credentials",
            "params": {
                "accessKeyId": "{ACCESS_KEY_ID}",
                "secretKey": "{SECRET_KEY}",
                "region": "{REGION}"
            }
        },
        "connectionSpec": {
            "id": "86043421-563b-46ec-8e6c-e23184711bf6",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.accessKeyId` | [!DNL Kinesis] アカウントのアクセスキー ID。 |
| `auth.params.secretKey` | [!DNL Kinesis] アカウントの秘密アクセスキー。 |
| `auth.params.region` | [!DNL Kinesis] アカウントの地域です。 |
| `connectionSpec.id` | [!DNL Kinesis] 接続仕様 ID:`86043421-563b-46ec-8e6c-e23184711bf6` |

**応答**

正常な応答は、新しく作成されたベース接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、次の手順でソース接続を作成する際に必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## ソース接続の作成 {#source}

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
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "AWS Kinesis source connection",
        "description": "A source connection for AWS Kinesis",
        "baseConnectionId": "4cb0c374-d3bb-4557-b139-5712880adc55",
        "connectionSpec": {
            "id": "86043421-563b-46ec-8e6c-e23184711bf6",
            "version": "1.0"
        },
        "data": {
            "format": "json"
        },
        "params": {
            "stream": "{STREAM}",
            "dataType": "raw",
            "reset": "latest"
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前がわかりやすいことを確認します。 |
| `description` | ソース接続に関する詳細情報を含めるために指定できるオプションの値です。 |
| `baseConnectionId` | 前の手順で生成した [!DNL Kinesis] ソースのベース接続 ID。 |
| `connectionSpec.id` | [!DNL Kinesis] の固定接続仕様 ID。 この ID は、です。`86043421-563b-46ec-8e6c-e23184711bf6` |
| `data.format` | 取り込む [!DNL Kinesis] データの形式。 現在サポートされているデータ形式は `json` のみです。 |
| `params.stream` | レコードの取り込み元のデータストリームの名前。 |
| `params.dataType` | このパラメーターは、取り込まれるデータのタイプを定義します。 次のようなデータタイプがサポートされています。`raw` と `xdm`。 |
| `params.reset` | このパラメーターは、データの読み取り方法を定義します。 `latest` を使用して最新のデータから読み取りを開始し、`earliest` を使用してストリーム内の最初に使用可能なデータから読み取りを開始します。 |

**応答**

正常な応答は、新しく作成されたソース接続の一意の識別子 (`id`) を返します。 この ID は、次のチュートリアルでデータフローを作成する際に必要になります。

```json
{
    "id": "e96d6135-4b50-446e-922c-6dd66672b6b2",
    "etag": "\"66013508-0000-0200-0000-5f6e2ae70000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Kinesis] ソース接続を作成しました。 次のチュートリアルでこのソース接続 ID を使用して、 [!DNL Flow Service] API](../../collect/streaming.md) を使用してストリーミングデータフローを作成できます。[
