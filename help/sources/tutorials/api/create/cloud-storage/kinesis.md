---
title: フローサービス API を使用したAmazon Kinesisソース接続の作成
description: フローサービス API を使用してAdobe Experience PlatformをAmazon Kinesisソースに接続する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: 64da8894-12ac-45a0-b03e-fe9b6aa435d3
source-git-commit: 9a8139c26b5bb5ff937a51986967b57db58aab6c
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# の作成 [!DNL Amazon Kinesis] フローサービス API を使用したソース接続

>[!IMPORTANT]
>
>この [!DNL Amazon Kinesis] ソースは、Real-time Customer Data Platform Ultimate を購入したユーザーがソースカタログで利用できます。

このチュートリアルでは、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Amazon Kinesis]（以下「[!DNL Kinesis]」）を Experience Platform に接続する手順を詳しく説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)[!DNL Platform]：Experience を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)[!DNL Platform]：Experience には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、[!DNL Flow Service] API を使用して [!DNL Kinesis] を Platform に正しく接続するために必要な追加情報を示します。

### 必要な認証情報の収集

次のために [!DNL Flow Service] を [!DNL Amazon Kinesis] アカウントの場合、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `accessKeyId` | アクセスキー ID は、 [!DNL Kinesis] アカウントを Platform に送信します。 |
| `secretKey` | 秘密鍵は、 [!DNL Kinesis] アカウントを Platform に送信します。 |
| `region` | の地域 [!DNL Kinesis] アカウント 詳しくは、 [許可リストへの IP アドレスの追加](../../../../ip-address-allow-list.md) 地域の詳細については、を参照してください。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Kinesis] 接続仕様 ID は `86043421-563b-46ec-8e6c-e23184711bf6` です。 |

詳しくは、 [!DNL Kinesis] アクセスキーとその生成方法については、こちらを参照してください。 [[!DNL AWS] IAM ユーザーのアクセスキーの管理に関するガイド](https://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_credentials_access-keys.html).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ソース接続を作成する最初の手順は、[!DNL Kinesis] ソースを認証し、ベース接続 ID を生成することです。ベース接続 ID を使用すると、ソース内を移動してファイルを探索し、データのタイプや形式に関する情報など、取り込みたい特定の項目を識別できます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際に [!DNL Kinesis] 認証資格情報をリクエストパラメーターの一部として指定します。

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
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `connectionSpec.id` | [!DNL Kinesis] 接続仕様 ID：`86043421-563b-46ec-8e6c-e23184711bf6` |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## ソース接続の作成 {#source}

ソース接続は、データの取り込み元となる外部ソースへの接続を作成および管理します。ソース接続は、データソース、データ形式、データフローの作成に必要なソース接続 ID などの情報で構成されます。 ソース接続インスタンスは、テナントと組織に固有です。

ソース接続を作成するには、[!DNL Flow Service] API の `/sourceConnections` エンドポイントに POST リクエストを実行します。

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
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前はわかりやすいものにしてください。 |
| `description` | 指定するとソース接続に関する詳細情報を含めることができるオプションの値。 |
| `baseConnectionId` | 前の手順で生成された [!DNL Kinesis] ソースのベース接続 ID。 |
| `connectionSpec.id` | [!DNL Kinesis] の固定接続仕様 ID。この ID は `86043421-563b-46ec-8e6c-e23184711bf6` です。 |
| `data.format` | 取り込む [!DNL Kinesis] データの形式。現在、サポートされているデータ形式は `json` のみです。 |
| `params.stream` | レコードを取り込むデータストリームの名前。 |
| `params.dataType` | このパラメーターは、取り込まれるデータのタイプを定義します。`raw` および `xdm` を含むデータタイプがサポートされています。 |
| `params.reset` | このパラメーターは、データの読み取り方法を定義します。 用途 `latest` 最新のデータから読み込みを開始するには、 `earliest` をクリックして、ストリーム内の最初の使用可能なデータから読み取りを開始します。 |

**応答**

リクエストが成功した場合は、新しく作成されたソース接続の一意の ID（`id`）が返されます。この ID は、次のチュートリアルでデータフローを作成する際に必要です。

```json
{
    "id": "e96d6135-4b50-446e-922c-6dd66672b6b2",
    "etag": "\"66013508-0000-0200-0000-5f6e2ae70000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Kinesis] ソース接続を作成しました。次のチュートリアルでは、このソース接続 ID を使用して、[ [!DNL Flow Service] API を使用したストリーミングデータフローの作成](../../collect/streaming.md)を行います。
