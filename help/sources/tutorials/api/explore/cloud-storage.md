---
keywords: Experience Platform；ホーム；人気の高いトピック；クラウドストレージ；クラウドストレージ
title: フローサービス API を使用したクラウドストレージフォルダーの調査
description: このチュートリアルでは、フローサービス API を使用して、サードパーティのクラウドストレージシステムを調べます。
exl-id: ba1a9bff-43a6-44fb-a4e7-e6a45b7eeebd
source-git-commit: 93061c84639ca1fdd3f7abb1bbd050eb6eebbdd6
workflow-type: tm+mt
source-wordcount: '663'
ht-degree: 20%

---

# を使用してクラウドストレージフォルダーを調べる [!DNL Flow Service] API

このチュートリアルでは、 [[!DNL Flow Service]](https://www.adobe.io/experience-platform-apis/references/flow-service/) API

>[!NOTE]
>
>クラウドストレージを参照するには、クラウドストレージソースの有効なベース接続 ID が既に存在している必要があります。 この ID がない場合、 [ソースの概要](../../../home.md#cloud-storage) を参照してください。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../landing/api-guide.md)のガイドを参照してください。

## クラウドストレージフォルダーを参照する

クラウドストレージフォルダーの構造に関する情報を取得するには、 [!DNL Flow Service] ソースのベース接続 ID を提供する際の API。

クラウドストレージを調査するためのGETリクエストを実行する場合、次の表に示すクエリパラメーターを含める必要があります。

| パラメーター | 説明 |
| --------- | ----------- |
| `objectType` | 参照するオブジェクトのタイプ。 この値は次のいずれかに設定します。 <ul><li>`folder`:特定のディレクトリの参照</li><li>`root`:ルートディレクトリを参照します。</li></ul> |
| `object` | このパラメーターは、特定のディレクトリを表示する場合にのみ必要です。 値は、参照するディレクトリのパスを表します。 |


**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=root
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=folder&object={PATH}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | クラウドストレージソースのベース接続 ID。 |
| `{PATH}` | ディレクトリのパス。 |

**リクエスト**

```shell
curl -X GET \
  'http://platform.adobe.io/data/foundation/flowservice/connections/dc3c0646-5e30-47be-a1ce-d162cb8f1f07/explore?objectType=folder&object=root' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、クエリされたディレクトリ内で見つかったファイルとフォルダーの配列を返します。 次の項目をメモします。 `path` プロパティを指定してファイルの構造を検査する必要があるので、アップロードするファイルのプロパティを設定します。

```json
[
    {
        "type": "file",
        "name": "account.csv",
        "path": "/test-connectors/testFolder-fileIngestion/account.csv",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "file",
        "name": "profileData.json",
        "path": "/test-connectors/testFolder-fileIngestion/profileData.json",
        "canPreview": true,
        "canFetchSchema": true
    },
    {
        "type": "file",
        "name": "sampleprofile--3.parquet",
        "path": "/test-connectors/testFolder-fileIngestion/sampleprofile--3.parquet",
        "canPreview": true,
        "canFetchSchema": true
    }
]
```

## Inspectファイルの構造

クラウドストレージからGETファイルの構造を調べるには、ファイルのパスと型をクエリパラメーターとして指定し、データリクエストを実行します。

ファイルのパスとタイプを指定してGETリクエストを実行することで、クラウドストレージソースからデータファイルの構造を調べることができます。 また、CSV、TSV、圧縮 JSON、区切り形式のファイルなど、様々なファイルタイプを検査する場合は、クエリパラメーターの一部としてファイルタイプを指定します。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&fileType={FILE_TYPE}&{QUERY_PARAMS}&preview=true
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&preview=true&fileType=delimited&columnDelimiter=\t
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&preview=true&fileType=delimited&compressionType=gzip;
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | クラウドストレージソースコネクタの接続 ID。 |
| `{FILE_PATH}` | 検査するファイルへのパス。 |
| `{FILE_TYPE}` | ファイルのタイプ。 次のファイルタイプがサポートされています。<ul><li>区切り</code>:区切り文字の値。 DSV ファイルはコンマで区切る必要があります。</li><li>JSON</code>:JavaScript のオブジェクト表記法。 JSON ファイルは XDM に準拠している必要があります</li><li>PARQUET</code>:Apache Parquet。 Parquet ファイルは XDM に準拠している必要があります。</li></ul> |
| `{QUERY_PARAMS}` | 結果のフィルタリングに使用できるオプションのクエリパラメーター。 詳しくは、 [クエリパラメーター](#query) を参照してください。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/{CONNECTION_ID}/explore?objectType=file&object=/aep-bootcamp/Adobe%20Pets%20Customer%2020190801%20EXP.json&fileType=json&preview=true' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、クエリされたファイルの構造（テーブル名とデータ型を含む）を返します。

```json
[
    {
        "name": "Id",
        "type": "String"
    },
    {
        "name": "FirstName",
        "type": "String"
    },
    {
        "name": "LastName",
        "type": "String"
    },
    {
        "name": "Email",
        "type": "String"
    },
    {
        "name": "Phone",
        "type": "String"
    }
]
```

## クエリパラメーターの使用 {#query}

この [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) では、様々なファイルタイプのプレビューと検査をおこなうためのクエリパラメーターの使用をサポートしています。

| パラメーター | 説明 |
| --------- | ----------- |
| `columnDelimiter` | CSV または TSV ファイルを検査する列区切り文字として指定した 1 文字の値。 パラメーターを指定しない場合、値はデフォルトでコンマになります `(,)`. |
| `compressionType` | 圧縮された区切りファイルまたは JSON ファイルをプレビューするための必須クエリパラメーター。 サポートされる圧縮ファイルは次のとおりです。 <ul><li>`bzip2`</li><li>`gzip`</li><li>`deflate`</li><li>`zipDeflate`</li><li>`tarGzip`</li><li>`tar`</li></ul> |

## 次の手順

このチュートリアルに従って、クラウドストレージシステムを調べ、に取り込むファイルのパスを見つけました [!DNL Platform]をクリックし、その構造を確認しました。 次のチュートリアルでこの情報を使用して、 [クラウドストレージからデータを収集し、Platform に取り込む](../collect/cloud-storage.md).
