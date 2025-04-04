---
title: 暗号化されたデータの取り込み
description: API を使用して、クラウドストレージバッチソースを通じて暗号化されたファイルを取り込む方法を説明します。
exl-id: 83a7a154-4f55-4bf0-bfef-594d5d50f460
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1816'
ht-degree: 63%

---

# 暗号化されたデータの取り込み

クラウドストレージバッチソースを使用して、暗号化されたデータファイルをAdobe Experience Platformに取り込むことができます。 暗号化されたデータの取り込みでは、非対称暗号化メカニズムを利用して、バッチデータを Experience Platform に安全に転送できます。 現在のところ、サポートされている非対称暗号化メカニズムは PGP と GPG です。

暗号化されたデータの取り込みプロセスは次のとおりです。

1. [Experience Platform API を使用して暗号化キーペアを作成](#create-encryption-key-pair)します。 暗号化キーペアは、秘密鍵と公開鍵で構成されます。 暗号化キーペアを作成したら、公開鍵を、それに対応する公開鍵 ID および有効期限と共にコピーまたはダウンロードできます。 このプロセス中に、秘密鍵は Experience Platform によってセキュアなコンテナに保存されます。**メモ：** 応答内の公開鍵は Base64 でエンコードされており、使用する前にデコードする必要があります。
2. 公開鍵を使用して、取り込むデータファイルを暗号化します。
3. 暗号化されたファイルをクラウドストレージに格納します。
4. 暗号化されたファイルの準備が整ったら、[クラウドストレージソースのソース接続とデータフローを作成](#create-a-dataflow-for-encrypted-data)します。フロー作成手順で、`encryption` パラメーターを指定し、公開鍵 ID を含める必要があります。
5. Experience Platform は、セキュアなコンテナから秘密鍵を取得して、取り込み時にデータを復号します。

>[!IMPORTANT]
>
>1 つの暗号化ファイルの最大サイズは 1 GB です。例えば、1 回のデータフロー実行で 2 GB 分のデータを取り込むことができますが、そのデータ内の個々のファイルは 1 GB を超えることはできません。

このドキュメントでは、データを暗号化するための暗号化キーペアを生成し、暗号化されたデータをクラウドストレージソースを使用して Experience Platform に取り込む手順について説明します。

## 基本を学ぶ {#get-started}

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
   * [クラウドストレージソース](../api/collect/cloud-storage.md)：クラウドストレージソースから Experience Platform にバッチデータを取り込むためのデータフローを作成します。
* [ サンドボックス ](../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../landing/api-guide.md) を参照してください。

### 暗号化されたファイルでサポートされるファイル拡張子 {#supported-file-extensions-for-encrypted-files}

暗号化ファイルでサポートされるファイル拡張子のリストを以下に示します。

* .csv
* .tsv
* .json
* .parquet
* .csv.gpg
* .tsv.gpg
* .json.gpg
* .parquet.gpg
* .csv.pgp
* .tsv.pgp
* .json.pgp
* .parquet.pgp
* .gpg
* .pgp

>[!NOTE]
>
>Adobe Experience Platform ソースでの暗号化されたファイル取り込みは、openPGP をサポートしており、PGP の特定の独自バージョンはサポートしていません。

## 暗号化キーペアの作成 {#create-encryption-key-pair}

>[!IMPORTANT]
>
>暗号化キーは、特定のサンドボックスに固有です。 したがって、暗号化されたデータを組織内の別のサンドボックスに取り込む場合は、新しい暗号化キーを作成する必要があります。

暗号化されたデータを Experience Platform に取り込む最初の手順は、[!DNL Connectors] API の `/encryption/keys` エンドポイントに対して POST リクエストを実行して暗号化キーペアを作成することです。

**API 形式**

```http
POST /data/foundation/connectors/encryption/keys
```

**リクエスト**

+++リクエスト例を表示

次のリクエストは、PGP 暗号化アルゴリズムを使用して暗号化キーペアを生成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/connectors/encryption/keys' \
  -H 'Authorization: Bearer {{ACCESS_TOKEN}}' \
  -H 'x-api-key: {{API_KEY}}' \
  -H 'x-gw-ims-org-id: {{ORG_ID}}' \
  -H 'x-sandbox-name: {{SANDBOX_NAME}}' \
  -H 'Content-Type: application/json' 
  -d '{
      "name": "acme-encryption",
      "encryptionAlgorithm": "PGP",
      "params": {
          "passPhrase": "{{PASSPHRASE}}"
      }
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `name` | 暗号化キーペアの名前。 |
| `encryptionAlgorithm` | 使用する暗号化アルゴリズムのタイプ。 サポートされているタイプは `PGP` と `GPG` です。 |
| `params.passPhrase` | パスフレーズは、暗号化キーにさらに保護レイヤーを追加します。 作成時に、Experience Platform は、公開鍵とは別のセキュアなコンテナにパスフレーズを保存します。 空白以外の文字列をパスフレーズとして指定する必要があります。 |

+++

**応答**

+++応答の例を表示

正常な応答では、Base64 でエンコードされた公開鍵、公開鍵 ID および鍵の有効期限が返されます。 有効期限は、キーの生成日から 180 日後に自動的に設定されます。 現在、有効期限は設定変更できません。

```json
{
    ​"publicKey": "{PUBLIC_KEY}",
    ​"publicKeyId": "{PUBLIC_KEY_ID}",
    ​"expiryTime": "1684843168"
}
```

| プロパティ | 説明 |
| --- | --- |
| `publicKey` | 公開鍵は、クラウドストレージ内のデータの暗号化に使用されます。このキーは、この手順で作成された秘密鍵に対応します。ただし、秘密鍵は直ちに Experience Platform に送信されます。 |
| `publicKeyId` | 公開鍵 ID は、データフローを作成し、暗号化されたクラウドストレージデータを Experience Platform に取り込むために使用されます。 |
| `expiryTime` | 有効期限は、暗号化キーペアの有効期限を定義します。この日付は、キーの生成日から 180 日後に自動的に設定され、UNIX タイムスタンプ形式で表示されます。 |

+++

### 暗号化キーの取得 {#retrieve-encryption-keys}

組織内のすべての暗号化キーを取得するには、`/encryption/keys` endpoit=nt に対してGET リクエストを実行します。

**API 形式**

```http
GET /data/foundation/connectors/encryption/keys
```

**リクエスト**

+++リクエスト例を表示

次のリクエストでは、組織内のすべての暗号化キーを取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/encryption/keys' \
  -H 'Authorization: Bearer {{ACCESS_TOKEN}}' \
  -H 'x-api-key: {{API_KEY}}' \
  -H 'x-gw-ims-org-id: {{ORG_ID}}' \
```

+++

**応答**

+++応答の例を表示

正常な応答では、暗号化アルゴリズム、名前、公開鍵、公開鍵 ID、鍵のタイプおよび鍵の対応する有効期限が返されます。

```json
{
    "encryptionAlgorithm": "{ENCRYPTION_ALGORITHM}",
    "name": "{NAME}",
    "publicKeyId": "{PUBLIC_KEY_ID}",
    "publicKey": "{PUBLIC_KEY}",
    "keyType": "{KEY_TYPE}",
    "expiryTime": "{EXPIRY_TIME}"
}
```

+++

### ID による暗号化キーの取得 {#retrieve-encryption-keys-by-id}

特定の暗号化キーセットを取得するには、エンドポイントに対してGET リク `/encryption/keys` ストを実行し、公開鍵 ID をヘッダーパラメーターとして指定します。

**API 形式**

```http
GET /data/foundation/connectors/encryption/keys/{PUBLIC_KEY_ID}
```

**リクエスト**

+++リクエスト例を表示

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/encryption/keys/{publicKeyId}' \
  -H 'Authorization: Bearer {{ACCESS_TOKEN}}' \
  -H 'x-api-key: {{API_KEY}}' \
  -H 'x-gw-ims-org-id: {{ORG_ID}}' \
```

+++

**応答**

+++応答の例を表示

正常な応答では、暗号化アルゴリズム、名前、公開鍵、公開鍵 ID、鍵のタイプおよび鍵の対応する有効期限が返されます。

```json
{
    "encryptionAlgorithm": "{ENCRYPTION_ALGORITHM}",
    "name": "{NAME}",
    "publicKeyId": "{PUBLIC_KEY_ID}",
    "publicKey": "{PUBLIC_KEY}",
    "keyType": "{KEY_TYPE}",
    "expiryTime": "{EXPIRY_TIME}"
}
```

+++

### 顧客管理キーペアの作成 {#create-customer-managed-key-pair}

オプションで、署名検証キーペアを作成して、暗号化されたデータに署名し、取り込むことができます。

この段階では、独自の秘密鍵および公開鍵の組み合わせを生成し、秘密鍵を使用して暗号化されたデータに署名する必要があります。次に、Experience Platformが署名を検証するために、Base64 で公開鍵をエンコードし、それをExperience Platformに共有する必要があります。

### 公開鍵を Experience Platform に共有

公開鍵を共有するには、`/customer-keys` エンドポイントに POST リクエストを行い、暗号化アルゴリズムと Base64 でエンコードされた公開鍵を提供します。

**API 形式**

```http
POST /data/foundation/connectors/encryption/customer-keys
```

**リクエスト**

+++リクエスト例を表示

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/connectors/encryption/customer-keys' \
  -H 'Authorization: Bearer {{ACCESS_TOKEN}}' \
  -H 'x-api-key: {{API_KEY}}' \
  -H 'x-gw-ims-org-id: {{ORG_ID}}' \
  -H 'x-sandbox-name: {{SANDBOX_NAME}}' \
  -H 'Content-Type: application/json' 
  -d '{
      "name": "acme-sign-verification-keys"
      "encryptionAlgorithm": {{ENCRYPTION_ALGORITHM}},       
      "publicKey": {{BASE_64_ENCODED_PUBLIC_KEY}},
      "params": {
          "passPhrase": {{PASS_PHRASE}}
      }
    }'
```

| パラメーター | 説明 |
| --- | --- |
| `encryptionAlgorithm` | 使用する暗号化アルゴリズムのタイプ。 サポートされているタイプは `PGP` と `GPG` です。 |
| `publicKey` | 暗号化された署名に使用する、顧客が管理する鍵に対応する公開鍵。この鍵は、Base64 でエンコードする必要があります。 |

+++

**応答**

+++応答の例を表示

```json
{    
  "publicKeyId": "e31ae895-7896-469a-8e06-eb9207ddf1c2" 
} 
```

| プロパティ | 説明 |
| --- | --- |
| `publicKeyId` | この公開鍵 ID は、顧客が管理する鍵を Experience Platform と共有する際に返されます。署名済みおよび暗号化されたデータのデータフローを作成する際に、この公開鍵 ID を署名検証鍵 ID として提供できます。 |

+++

### 顧客管理キーペアの取得

顧客管理キーを取得するには、`/customer-keys` エンドポイントに対してGET リクエストを実行します。

**API 形式**

```http
GET /data/foundation/connectors/encryption/customer-keys
```

**リクエスト**

+++リクエスト例を表示

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/encryption/customer-keys' \
  -H 'Authorization: Bearer {{ACCESS_TOKEN}}' \
  -H 'x-api-key: {{API_KEY}}' \
  -H 'x-gw-ims-org-id: {{ORG_ID}}' \
```

+++

**応答**

+++応答の例を表示

```json
[
    {
        "encryptionAlgorithm": "{ENCRYPTION_ALGORITHM}",
        "name": "{NAME}",
        "publicKeyId": "{PUBLIC_KEY_ID}",
        "publicKey": "{PUBLIC_KEY}",
        "keyType": "{KEY_TYPE}",
    }
]
```

+++

## [!DNL Flow Service] API を使用した Experience Platform へのクラウドストレージソースの接続 

暗号化キーペアを取得したら、手順を進めて、クラウドストレージソースのソース接続を作成し、暗号化されたデータをExperience Platformに取り込むことができます。

まず、Experience Platformに対してソースを認証するためのベース接続を作成する必要があります。 ベース接続を作成しソースを認証するには、使用するソースを以下のリストから選択します。

* [Amazon S3](../api/create/cloud-storage/s3.md)
* [[!DNL Apache HDFS]](../api/create/cloud-storage/hdfs.md)
* [Azure Blob](../api/create/cloud-storage/blob.md)
* [Azure Data Lake Storage Gen2](../api/create/cloud-storage/adls-gen2.md)
* [Azure File Storage](../api/create/cloud-storage/azure-file-storage.md)
* [Data Landing Zone](../api/create/cloud-storage/data-landing-zone.md)
* [FTP](../api/create/cloud-storage/ftp.md)
* [Google Cloud Storage](../api/create/cloud-storage/google.md)
* [Oracle Object Storage](../api/create/cloud-storage/oracle-object-storage.md)
* [SFTP](../api/create/cloud-storage/sftp.md)

ベース接続を作成したら、[クラウドストレージソースのソース接続の作成](../api/collect/cloud-storage.md)に関するチュートリアルで概要を説明している手順に従って、ソース接続、ターゲット接続、およびマッピングを作成する必要があります。

## 暗号化されたデータのデータフローの作成 {#create-a-dataflow-for-encrypted-data}

>[!NOTE]
>
>暗号化されたデータの取り込みのデータフローを作成するには、次のものが必要です。
>
>* [公開鍵 ID](#create-encryption-key-pair)
>* [ソース接続 ID](../api/collect/cloud-storage.md#source)
>* [ターゲット接続 ID](../api/collect/cloud-storage.md#target)
>* [マッピング ID](../api/collect/cloud-storage.md#mapping)

データフローを作成するには、[!DNL Flow Service] API の `/flows` エンドポイントに対して POST リクエストを実行します。暗号化されたデータを取り込むには、`transformations` プロパティに `encryption` セクションを追加し、その中に、前の手順で作成した `publicKeyId` を含める必要があります。

**API 形式**

```http
POST /flows
```

>[!BEGINTABS]

>[!TAB 暗号化されたデータ取り込みのデータフローの作成]

**リクエスト**

+++リクエスト例を表示

次のリクエストでは、クラウドストレージソースの暗号化されたデータを取り込むデータフローを作成しています。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows' \
  -H 'x-api-key: {{API_KEY}}' \
  -H 'x-gw-ims-org-id: {{ORG_ID}}' \
  -H 'x-sandbox-name: {{SANDBOX_NAME}}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "ACME Customer Data",
    "description": "ACME Customer Data (Encrypted)",
    "flowSpec": {
        "id": "9753525b-82c7-4dce-8a9b-5ccfce2b9876",
        "version": "1.0"
    },
    "sourceConnectionIds": [
        "655f7c1b-1977-49b3-a429-51379ecf0e15"
    ],
    "targetConnectionIds": [
        "de688225-d619-481c-ae3b-40c250fd7c79"
    ],
    "transformations": [
        {
            "name": "Mapping",
            "params": {
                "mappingId": "6b6e24213dbe4f57bd8207d21034ff03",
                "mappingVersion":"0"
            }
        },
        {
            "name": "Encryption",
            "params": {
                "publicKeyId":"311ef6f8-9bcd-48cf-a9e9-d12c45fb7a17"
            }
        }
    ],
    "scheduleParams": {
        "startTime": "1675793392",
        "frequency": "once"
    }
}'
```

| プロパティ | 説明 |
| --- | --- |
| `flowSpec.id` | クラウドストレージソースに対応するフロー固有の ID。 |
| `sourceConnectionIds` | ソース接続 ID。 この ID は、ソースからExperience Platformへのデータ転送を表します。 |
| `targetConnectionIds` | ターゲット接続 ID。 この ID は、Experience Platformに取り込まれたデータが格納される場所を表します。 |
| `transformations[x].params.mappingId` | マッピング ID。 |
| `transformations.name` | 暗号化されたファイルを取り込む場合、データフローの追加の変換パラメーターとして `Encryption` を指定する必要があります。 |
| `transformations[x].params.publicKeyId` | 作成した公開鍵 ID。 この ID は、クラウドストレージデータの暗号化に使用される暗号化キーペアの一方です。 |
| `scheduleParams.startTime` | エポック時間で表した、データフローの開始時間。 |
| `scheduleParams.frequency` | データフローがデータを収集する頻度。指定できる値は、`once`、`minute`、`hour`、`day`、`week` です。 |
| `scheduleParams.interval` | インターバルは 2 つの連続したフロー実行の間隔を指定します。インターバルの値はゼロ以外の整数にしてください。頻度が `once` に設定されている場合、間隔は必須ではありません。また、頻度は他の頻度の値に対して、`15` よりも大きいか、等しい必要があります。 |

+++

**応答**

+++応答の例を表示

正常な応答では、暗号化されたデータの新規作成されたデータフローの ID（`id`）が返されます。

```json
{
    "id": "dbc5c132-bc2a-4625-85c1-32bc2a262558",
    "etag": "\"8e000533-0000-0200-0000-5f3c40fd0000\""
}
```

+++

>[!TAB 暗号化された署名済みのデータを取り込むためのデータフローの作成]

**リクエスト**

+++リクエスト例を表示

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows' \
  -H 'x-api-key: {{API_KEY}}' \
  -H 'x-gw-ims-org-id: {{ORG_ID}}' \
  -H 'x-sandbox-name: {{SANDBOX_NAME}}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "ACME Customer Data (with Sign Verification)",
    "description": "ACME Customer Data (with Sign Verification)",
    "flowSpec": {
        "id": "9753525b-82c7-4dce-8a9b-5ccfce2b9876",
        "version": "1.0"
    },
    "sourceConnectionIds": [
        "655f7c1b-1977-49b3-a429-51379ecf0e15"
    ],
    "targetConnectionIds": [
        "de688225-d619-481c-ae3b-40c250fd7c79"
    ],
    "transformations": [
        {
            "name": "Mapping",
            "params": {
                "mappingId": "6b6e24213dbe4f57bd8207d21034ff03",
                "mappingVersion":"0"
            }
        },
        {
            "name": "Encryption",
            "params": {
                "publicKeyId":"311ef6f8-9bcd-48cf-a9e9-d12c45fb7a17",
                "signVerificationKeyId":"e31ae895-7896-469a-8e06-eb9207ddf1c2"
            }
        }
    ],
    "scheduleParams": {
        "startTime": "1675793392",
        "frequency": "once"
    }
}'
```

| プロパティ | 説明 |
| --- | --- |
| `params.signVerificationKeyId` | 署名検証鍵 ID は、Base64 でエンコードされた公開鍵を Experience Platform と共有した後に取得された公開鍵 ID と同じです。 |

+++

**応答**

+++応答の例を表示

正常な応答では、暗号化されたデータの新規作成されたデータフローの ID（`id`）が返されます。

```json
{
    "id": "dbc5c132-bc2a-4625-85c1-32bc2a262558",
    "etag": "\"8e000533-0000-0200-0000-5f3c40fd0000\""
}
```

+++

>[!ENDTABS]

### 暗号化キーの削除 {#delete-encryption-keys}

暗号化キーを削除するには、`/encryption/keys` エンドポイントに対してDELETE リクエストを実行し、公開鍵 ID をヘッダーパラメーターとして指定します。

**API 形式**

```http
DELETE /data/foundation/connectors/encryption/keys/{PUBLIC_KEY_ID}
```

**リクエスト**

+++リクエスト例を表示

```shell
curl -X DELETE \
  'https://platform.adobe.io/data/foundation/connectors/encryption/keys/{publicKeyId}' \
  -H 'Authorization: Bearer {{ACCESS_TOKEN}}' \
  -H 'x-api-key: {{API_KEY}}' \
  -H 'x-gw-ims-org-id: {{ORG_ID}}' \
```

+++

**応答** 

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

### 暗号化キーを検証 {#validate-encryption-keys}

暗号化キーを検証するには、`/encryption/keys/validate/` エンドポイントに対してGET リクエストを実行し、検証する公開鍵 ID をヘッダーパラメーターとして指定します。

```http
GET /data/foundation/connectors/encryption/keys/validate/{PUBLIC_KEY_ID}
```

**リクエスト**

+++リクエスト例を表示

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/encryption/keys/validate/{publicKeyId}' \
  -H 'Authorization: Bearer {{ACCESS_TOKEN}}' \
  -H 'x-api-key: {{API_KEY}}' \
  -H 'x-gw-ims-org-id: {{ORG_ID}}' \
```

+++

**応答**

応答が成功すると、ID が有効か無効かを示す確認が返されます。

>[!BEGINTABS]

>[!TAB  有効 ]

有効な公開鍵 ID は、公開鍵 ID と共に `Active` のステータスを返します。

```json
{
    "publicKeyId": "{PUBLIC_KEY_ID}",
    "status": "Active"
}
```

>[!TAB  無効 ]

無効な公開鍵 ID は、公開鍵 ID と共に `Expired` のステータスを返します。

```json
{
    "publicKeyId": "{PUBLIC_KEY_ID}",
    "status": "Expired"
}
```

>[!ENDTABS]


## 繰り返し取り込みの制限 {#restrictions-on-recurring-ingestion}

暗号化されたデータの取り込みは、ソースでの繰り返しフォルダーまたはマルチレベルフォルダーの取り込みをサポートしていません。 すべての暗号化ファイルは、1 つのフォルダーに格納する必要があります。 1 つのソースパスに複数のフォルダーがあるワイルドカードもサポートされていません。

次に、ソースパスが `/ACME-customers/*.csv.gpg` である、サポートされているフォルダー構造の例を示します。

このシナリオでは、太字のファイルがExperience Platformに取り込まれます。

* ACME-customers
   * **File1.csv.gpg**
   * File2.json.gpg
   * **File3.csv.gpg**
   * File4.json
   * **File5.csv.gpg**

次に、ソースパスが `/ACME-customers/*` である、サポートされていないフォルダー構造の例を示します。

このシナリオでは、フロー実行が失敗し、ソースからデータをコピーできないことを示すエラーメッセージを返します。

* ACME-customers
   * File1.csv.gpg
   * File2.json.gpg
   * Subfolder1
      * File3.csv.gpg
      * File4.json.gpg
      * File5.csv.gpg
* ACME-loyalty
   * File6.csv.gpg


## 次の手順

このチュートリアルでは、クラウドストレージデータの暗号化キーペアと、暗号化されたデータを [!DNL Flow Service API] を使用して取り込むデータフローを作成しました。データフローの完全性、エラーおよび指標に関するステータスの更新については、[API を使用したデータフローの監視 ](./monitor.md) に関するガイドを参照し  [!DNL Flow Service]  ください。