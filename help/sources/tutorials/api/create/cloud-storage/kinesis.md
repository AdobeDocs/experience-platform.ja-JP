---
keywords: Experience Platform；ホーム；人気の高いトピック；Kinesis;kinesis;Amazon Kinesis;amazon kinesis
solution: Experience Platform
title: フローサービス API を使用したAmazon Kinesisソース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience PlatformをAmazon Kinesisソースに接続する方法を説明します。
exl-id: 64da8894-12ac-45a0-b03e-fe9b6aa435d3
source-git-commit: 27e5c64f31b9a68252d262b531660811a0576177
workflow-type: tm+mt
source-wordcount: '730'
ht-degree: 8%

---

# の作成 [!DNL Amazon Kinesis] フローサービス API を使用したソース接続

このチュートリアルでは、接続の手順について説明します [!DNL Amazon Kinesis] （以下「」という。）[!DNL Kinesis]&quot;) をExperience Platform、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、 [!DNL Platform] サービス。
* [サンドボックス](../../../../../sandboxes/home.md)[!DNL Platform]：Experience は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

次の節では、接続を成功させるために知っておく必要がある追加情報を示します [!DNL Kinesis] を使用して Platform に [!DNL Flow Service] API

### 必要な資格情報の収集

次のために [!DNL Flow Service] を [!DNL Amazon Kinesis] アカウントの場合、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `accessKeyId` | アクセスキー ID は、 [!DNL Kinesis] アカウントを Platform に送信します。 |
| `secretKey` | 秘密鍵は、 [!DNL Kinesis] アカウントを Platform に送信します。 |
| `region` | の地域 [!DNL Kinesis] アカウント 詳しくは、 [許可リストへの IP アドレスの追加](../../../../ip-address-allow-list.md) 地域の詳細については、を参照してください。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 この [!DNL Kinesis] 接続仕様 ID: `86043421-563b-46ec-8e6c-e23184711bf6`. |

詳しくは、 [!DNL Kinesis] アクセスキーとその生成方法については、こちらを参照してください。 [[!DNL AWS] IAM ユーザーのアクセスキーの管理に関するガイド](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、 [Platform API の概要](../../../../../landing/api-guide.md).

## ベース接続を作成する

ソース接続を作成する最初の手順は、 [!DNL Kinesis] ソースおよびベース接続 ID を生成します。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を識別できます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Kinesis] 認証資格情報をリクエストパラメーターの一部として使用します。

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
| `auth.params.accessKeyId` | のアクセスキー ID [!DNL Kinesis] アカウント |
| `auth.params.secretKey` | の秘密アクセスキー [!DNL Kinesis] アカウント |
| `auth.params.region` | の地域 [!DNL Kinesis] アカウント |
| `connectionSpec.id` | この [!DNL Kinesis] 接続仕様 ID: `86043421-563b-46ec-8e6c-e23184711bf6` |

**応答**

正常な応答は、新しく作成されたベース接続の詳細 ( 一意の識別子 (`id`) をクリックします。 この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## ソース接続の作成 {#source}

ソース接続は、データの取り込み元となる外部ソースへの接続を作成および管理します。 ソース接続は、データソース、データ形式、データフローの作成に必要なソース接続 ID などの情報で構成されます。 ソース接続インスタンスは、テナントと IMS 組織に固有です。

ソース接続を作成するには、 `/sourceConnections` エンドポイント [!DNL Flow Service] API

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
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前がわかりやすい名前になっていることを確認します。 |
| `description` | ソース接続に関する詳細情報を含めるために指定できるオプションの値です。 |
| `baseConnectionId` | のベース接続 ID [!DNL Kinesis] 前の手順で生成されたソース。 |
| `connectionSpec.id` | の固定接続仕様 ID [!DNL Kinesis]. この ID は次のとおりです。 `86043421-563b-46ec-8e6c-e23184711bf6` |
| `data.format` | の形式 [!DNL Kinesis] 取り込むデータ。 現在、サポートされているデータ形式は次のみです。 `json`. |
| `params.stream` | レコードを取り込むデータストリームの名前。 |
| `params.dataType` | このパラメーターは、取り込まれるデータのタイプを定義します。 次のようなデータタイプがサポートされています。 `raw` および `xdm`. |
| `params.reset` | このパラメーターは、データの読み取り方法を定義します。 用途 `latest` 最新のデータから読み込みを開始するには、 `earliest` をクリックして、ストリーム内の最初の使用可能なデータから読み取りを開始します。 |

**応答**

正常な応答は、一意の識別子 (`id`) に含まれます。 この ID は、次のチュートリアルでデータフローを作成する際に必要です。

```json
{
    "id": "e96d6135-4b50-446e-922c-6dd66672b6b2",
    "etag": "\"66013508-0000-0200-0000-5f6e2ae70000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Kinesis] を使用したソース接続 [!DNL Flow Service] API 次のチュートリアルでは、このソース接続 ID を使用して、 [を使用してストリーミングデータフローを作成する [!DNL Flow Service] API](../../collect/streaming.md).
