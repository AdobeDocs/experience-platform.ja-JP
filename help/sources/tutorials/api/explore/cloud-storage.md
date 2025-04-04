---
keywords: Experience Platform；ホーム；人気のトピック；クラウドストレージ；クラウドストレージ
title: Flow Service API を使用したクラウドストレージフォルダーの探索
description: このチュートリアルでは、Flow Service API を使用して、サードパーティのクラウドストレージシステムを調べます。
exl-id: ba1a9bff-43a6-44fb-a4e7-e6a45b7eeebd
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '695'
ht-degree: 15%

---

# [!DNL Flow Service] API を使用したクラウドストレージフォルダーの探索

このチュートリアルでは、[[!DNL Flow Service]](https://www.adobe.io/experience-platform-apis/references/flow-service/) API を使用してクラウドストレージの構造と内容を調査およびプレビューする手順を説明します。

>[!NOTE]
>
>クラウドストレージを探索するには、クラウドストレージソースの有効なベース接続 ID が必要です。 この ID がない場合は、ベース接続を作成できるクラウドストレージソースのリストについて、[ ソースの概要 ](../../../home.md#cloud-storage) を参照してください。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../landing/api-guide.md) を参照してください。

## クラウドストレージフォルダーの参照

ソースのベース接続 ID を指定したうえで [!DNL Flow Service] API に対してGET リクエストを行うことで、クラウドストレージフォルダーの構造に関する情報を取得できます。

クラウドストレージを調べるためにGET リクエストを実行する場合、次の表にリストされているクエリパラメーターを含める必要があります。

| パラメーター | 説明 |
| --------- | ----------- |
| `objectType` | 参照するオブジェクトのタイプ。 この値を次のいずれかに設定します。 <ul><li>`folder`：特定のディレクトリを参照します</li><li>`root`: ルートディレクトリを参照します。</li></ul> |
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

応答が成功すると、クエリされたディレクトリ内にあるファイルとフォルダーの配列が返されます。 アップロードするファイルの `path` プロパティをメモします。構造を検査するには次の手順でファイルを指定する必要があります。

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

## ファイルの構造を検査する

クラウドストレージからデータファイルの構造を調べるには、GET リクエストを実行し、その際にファイルのパスとタイプをクエリパラメーターとして指定します。

クラウドストレージソースのデータファイルの構造を調べるには、ファイルのパスとタイプを指定したうえでGET リクエストを実行します。 また、クエリパラメーターの一部としてファイルタイプを指定することで、CSV、TSV、圧縮 JSON や区切り文字付きファイルなど、様々なファイルタイプを検査することもできます。

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&fileType={FILE_TYPE}&{QUERY_PARAMS}&preview=true
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&preview=true&fileType=delimited&columnDelimiter=\t
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=file&object={FILE_PATH}&preview=true&fileType=delimited&compressionType=gzip;
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=FILE&object={FILE_PATH}&preview=true&fileType=delimited&encoding=ISO-8859-1;
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | クラウドストレージソースコネクタの接続 ID。 |
| `{FILE_PATH}` | 検査するファイルへのパス。 |
| `{FILE_TYPE}` | ファイルのタイプ。 次のファイルタイプがサポートされています。<ul><li><code> 区切り</code>：区切り文字区切りの値。 DSV ファイルはコンマで区切る必要があります。</li><li><code>JSON</code>:JavaScript オブジェクトの表記。 JSON ファイルは XDM に準拠している必要があります</li><li><code> 寄木細工</code>: Apache Parquet。 Parquet ファイルは XDM に準拠している必要があります。</li></ul> |
| `{QUERY_PARAMS}` | 結果のフィルタリングに使用できるオプションのクエリパラメーター。 詳しくは、[ クエリパラメーター ](#query) の節を参照してください。 |

**リクエスト**

```shell
curl -X GET \
    'http://platform.adobe.io/data/foundation/flowservice/connections/{BASE_CONNECTION_ID}/explore?objectType=file&object=/aep-bootcamp/Adobe%20Pets%20Customer%2020190801%20EXP.json&fileType=json&preview=true' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、テーブル名とデータタイプを含む、クエリされたファイルの構造が返されます。

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

[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) では、クエリパラメーターを使用して、様々なファイルタイプのプレビューと検査を行うことができます。

| パラメーター | 説明 |
| --------- | ----------- |
| `columnDelimiter` | CSV または TSV ファイルを調べるために列区切り文字として指定した単一の文字の値。 パラメーターを指定しない場合、デフォルト値はコンマ `(,)` になります。 |
| `compressionType` | 圧縮された区切り文字付きまたは JSON ファイルをプレビューするための必須のクエリパラメーター。 サポートされている圧縮ファイルは次のとおりです。 <ul><li>`bzip2`</li><li>`gzip`</li><li>`deflate`</li><li>`zipDeflate`</li><li>`tarGzip`</li><li>`tar`</li></ul> |
| `encoding` | プレビューのレンダリング時に使用するエンコーディングタイプを定義します。 サポートされているエンコーディングタイプは `UTF-8` と `ISO-8859-1` です。 **メモ**:`encoding` パラメーターは、区切り形式の CSV ファイルを取り込む場合にのみ使用できます。 他のファイルタイプは、デフォルトのエンコーディング `UTF-8` で取り込まれます。 |

## 次の手順

このチュートリアルでは、クラウドストレージシステムを調べ、[!DNL Experience Platform] に取り込むファイルのパスを見つけ、その構造を確認しました。 次のチュートリアルではこの情報を使用して [ クラウドストレージからデータを収集し、Experience Platformに取り込む ](../collect/cloud-storage.md) ことができます。
