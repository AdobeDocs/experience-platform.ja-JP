---
title: 暗号化されたデータ取り込み
description: Adobe Experience Platformでは、クラウドストレージバッチソースを使用して、暗号化されたファイルを取り込むことができます。
hide: true
hidefromtoc: true
source-git-commit: 0457784b8aa97d55882b794077aecdbd2a9a612a
workflow-type: tm+mt
source-wordcount: '914'
ht-degree: 21%

---

# 暗号化されたデータ取り込み

Adobe Experience Platformでは、クラウドストレージバッチソースを使用して、暗号化されたファイルを取り込むことができます。 暗号化されたデータ取り込みを使用すると、非対称の暗号化メカニズムを利用して、バッチデータを安全にExperience Platformに転送できます。 現在、サポートされている非対称暗号化メカニズムは PGP と GPG です。

暗号化されたデータ取り込みプロセスは次のとおりです。

1. [Experience PlatformAPI を使用した暗号化キーペアの作成](#create-encryption-key-pair). 暗号化キーのペアは、秘密鍵と公開鍵で構成されます。 作成した公開鍵は、対応する公開鍵 ID と有効期限と共に、コピーまたはダウンロードできます。 このプロセスの間、秘密鍵はセキュリティで保護された Vault にExperience Platformによって保存されます。
2. 公開鍵を使用して、取り込むデータファイルを暗号化します。
3. 暗号化されたファイルをクラウドストレージに配置します。
4. 暗号化されたファイルの準備が整ったら、 [クラウドストレージソース用のソース接続とデータフローの作成](#create-a-dataflow-for-encrypted-data). フロー作成手順で、 `encryption` パラメーターを使用し、公開鍵 ID を含めます。
5. Experience Platformは、セキュリティで保護された Vault から秘密鍵を取得し、取得時にデータを復号化します。

このドキュメントでは、データを暗号化するための暗号化キーのペアを生成し、クラウドストレージソースを使用して暗号化されたデータをExperience Platformに取り込む手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
   * [クラウドストレージソース](../api/collect/cloud-storage.md):データフローを作成して、クラウドストレージソースからExperience Platformにバッチデータを取り込みます。
* [サンドボックス](../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../landing/api-guide.md)のガイドを参照してください。

## 暗号化キーペアを作成 {#create-encryption-key-pair}

暗号化されたデータをExperience Platformに取り込む最初の手順は、に対してPOSTリクエストを実行して、暗号化キーのペアを作成することです `/encryption/keys` エンドポイント [!DNL Connectors] API

**API 形式**

```http
POST /data/foundation/connectors/encryption/keys
```

**リクエスト**

次のリクエストは、PGP 暗号化アルゴリズムを使用して暗号化キーペアを生成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/connectors/encryption/keys' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' 
  -d '{
      "encryptionAlgorithm": "PGP",
      "params": {
          "passPhrase": "{PASSPHRASE}"
      }
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `encryptionAlgorithm` | 使用する暗号化アルゴリズムのタイプ。 サポートされている暗号化の種類は次のとおりです `PGP` および `GPG`. |
| `params.passPhrase` | このパスフレーズは、暗号化キーに対する保護の追加レイヤーを提供します。 作成時に、Experience Platformはパスフレーズを公開鍵とは別の安全な Vault に保存します。 パスフレーズは、空白以外の文字列を指定する必要があります。 |

**応答**

正常な応答は、公開鍵、公開鍵 ID、および鍵の有効期限を返します。 有効期限は、キーの生成日から 180 日後に自動的に設定されます。 有効期限は現在設定できません。

```json
{
    ​"publicKey": "{PUBLIC_KEY}",
    ​"publicKeyId": "{PUBLIC_KEY_ID}",
    ​"expiryTime": "1684843168"
}
```

## を使用して、クラウドストレージソースをExperience Platformに接続します。 [!DNL Flow Service] API

暗号化キーペアを取得したら、続行して、クラウドストレージソース用のソース接続を作成し、暗号化されたデータを Platform に取り込むことができます。

まず、Platform に対してソースを認証するためのベース接続を作成する必要があります。 ベース接続を作成し、ソースを認証するには、以下のリストから、使用するソースを選択します。

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

ベース接続を作成したら、次の手順に従う必要があります。 [クラウドストレージソースのソース接続の作成](../api/collect/cloud-storage.md) ソース接続、ターゲット接続、およびマッピングを作成するために使用します。

## 暗号化されたデータのデータフローの作成 {#create-a-dataflow-for-encrypted-data}

>[!NOTE]
>
>暗号化されたデータ取り込みのデータフローを作成するには、次が必要です。
>* [公開鍵 ID](#create-encryption-key-pair)
>* [ソース接続 ID](../api/collect/cloud-storage.md#source)
>* [ターゲット接続 ID](../api/collect/cloud-storage.md#target)
>* [マッピング ID](../api/collect/cloud-storage.md#mapping)


データフローを作成するには、 `/flows` エンドポイント [!DNL Flow Service] API 暗号化されたデータを取り込むには、 `encryption` セクションから `transformations` プロパティと `publicKeyId` これは、前の手順で作成されたものです。

**API 形式**

```http
POST /flows
```

**リクエスト**

次のリクエストは、データフローを作成して、クラウドストレージソースの暗号化されたデータを取り込みます。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "ACME Customer Data",
      "description: "ACME encrypted data ingestion",
      "flowSpec": {
          "id": "9753525b-82c7-4dce-8a9b-5ccfce2b9876",
          "version": "1.0"
      },
      "sourceConnectionIds": [
          "26b53912-1005-49f0-b539-12100559f0e2"
      ],
      "targetConnectionIds": [
        "f7eb08fa-5f04-4e45-ab08-fa5f046e45ee"
      ],
      "transformations": [
          {
              "name": "Mapping",
              "params": {
                  "mappingId": "bf5286a9c1ad4266baca76ba3adc9366",
                  "mappingVersion": 0
              }
          },
          {
              "name": "Encryption",
              "params": {
                  "publicKeyId": 512e686e-543e-4354-bcba-e1403ddcc532
          }
  }
      ],
      "scheduleParams": {
          "startTime": "1597784298",
          "frequency": "once"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `flowSpec.id` | クラウドストレージソースに対応するフロー仕様 ID。 |
| `sourceConnectionIds` | ソース接続 ID。 この ID は、ソースから Platform へのデータ転送を表します。 |
| `targetConnectionIds` | ターゲット接続 ID。 この ID は、Platform に引き継がれたデータが到着する場所を表します。 |
| `transformations[x].params.mappingId` | マッピング ID。 |
| `transformations.name` | 暗号化されたファイルを取り込む場合、 `Encryption` をデータフローの追加の変換パラメーターとして使用する場合にのみ使用します。 |
| `transformations[x].params.publicKeyId` | 作成した公開鍵 ID。 この ID は、クラウドストレージデータの暗号化に使用する暗号化キーペアの半分です。 |
| `scheduleParams.startTime` | エポック時間で表した、データフローの開始時間。 |
| `scheduleParams.frequency` | データフローがデータを収集する頻度。指定できる値は、`once`、`minute`、`hour`、`day`、`week` です。 |
| `scheduleParams.interval` | インターバルは 2 つの連続したフロー実行の間隔を指定します。インターバルの値はゼロ以外の整数にしてください。頻度が `once` に設定されている場合、間隔は必須ではありません。また、頻度は他の頻度の値に対して、`15` よりも大きいか、等しい必要があります。 |

**応答**

成功すると、ID(`id`) に含まれます。

```json
{
    "id": "dbc5c132-bc2a-4625-85c1-32bc2a262558",
    "etag": "\"8e000533-0000-0200-0000-5f3c40fd0000\""
}
```

## 次の手順

このチュートリアルでは、クラウドストレージデータの暗号化キーペアを作成し、 [!DNL Flow Service API]. データフローの完全性、エラー、指標に関するステータスの更新については、 [を使用したデータフローの監視 [!DNL Flow Service] API](./monitor.md).