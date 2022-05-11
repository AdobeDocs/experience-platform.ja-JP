---
keywords: Experience Platform；ホーム；人気の高いトピック；データ準備；データ準備；ストリーミング；アップサート；ストリーミングアップサート
title: データ準備を使用して、プロファイル・サービスに部分的な行更新を送信
description: このドキュメントでは、Data Prep を使用して、プロファイル・サービスに部分的な行更新を送信する方法に関する情報を提供します。
source-git-commit: 26059af9f899b4ccf5701899c9ac8111d56488e8
workflow-type: tm+mt
source-wordcount: '1057'
ht-degree: 6%

---

# 行の一部更新を次に送信： [!DNL Profile Service] using [!DNL Data Prep]

でのアップサートのストリーミング [!DNL Data Prep] 部分行の更新を次の項目に送信できます。 [!DNL Profile Service] データを作成し、単一の API リクエストで新しい id リンクを確立することもできます。

アップサートをストリーミングすることで、データをに変換しながら、データの形式を保持できます。 [!DNL Profile Service] 取り込み中のPATCHリクエスト。 指定した入力に基づいて、 [!DNL Data Prep] を使用すると、単一の API ペイロードを送信し、データを [!DNL Profile Service] PATCHと [!DNL Identity Service] CREATE リクエスト。

このドキュメントでは、でアップサートをストリーミングする方法について説明します。 [!DNL Data Prep].

## はじめに

この概要では、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Data Prep]](./home.md): [!DNL Data Prep] では、データエンジニアが Experience Data Model(XDM) との間で、データのマッピング、変換、検証をおこなうことができます。
* [[!DNL Identity Service]](../identity-service/home.md):デバイスやシステム間で ID を結び付けることで、個々の顧客とその行動をより良く把握できます。
* [リアルタイム顧客プロファイル](../profile/home.md)：複数のソースからの集計データに基づいて、統合された顧客プロファイルをリアルタイムで提供します。
* [ソース](../sources/home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。

## でのストリーミングアップサートの使用 [!DNL Data Prep] {#streaming-upserts-in-data-prep}

>[!NOTE]
>
>次のソースは、ストリーミングアップサートの使用をサポートしています。<ul><li>[[!DNL Amazon Kinesis]](../sources/connectors/cloud-storage/kinesis.md)</li><li>[[!DNL Azure Event Hubs]](../sources/connectors/cloud-storage/eventhub.md)</li><li>[[!DNL HTTP API]](../sources/connectors/streaming/http.md)</li></ul>

### ストリーミングアップサートの高レベルワークフロー

でのアップサートのストリーミング [!DNL Data Prep] は次のように機能します。

* 最初に、のデータセットを作成して有効にする必要があります。 [!DNL Profile] 消費。 詳しくは、 [のデータセットの有効化 [!DNL Profile]](../catalog/datasets/enable-for-profile.md) 詳細は、
* 新しい ID をリンクする必要がある場合は、追加のデータセットも作成する必要があります **同じスキーマで** の [!DNL Profile] データセット；
* データセットを準備したら、データフローを作成して、受信リクエストを [!DNL Profile] データセット；
* 次に、受信リクエストを更新して、必要なヘッダーを含める必要があります。 次のヘッダーで定義されます。
   * を使用して実行する必要があるデータ操作 [!DNL Profile]: `create`, `merge`、および `delete`;
   * 実行するオプションの ID 操作 [!DNL Identity Service]: `create`.

### ID データセットの設定

新しい ID をリンクする必要がある場合は、受信ペイロードで追加のデータセットを作成して渡す必要があります。 ID データセットを作成する場合は、次の要件が満たされていることを確認する必要があります。

* ID データセットには、 [!DNL Profile] データセット。 スキーマが一致しないと、システムの動作に一貫性がなくなる場合があります。
* ただし、ID データセットが [!DNL Profile] データセット。 データセットが同じ場合、データは更新されずに上書きされます。
* 最初のデータセットを有効にする必要があるのは、 [!DNL Profile]、ID データセット **次の値を指定しない** 有効にする [!DNL Profile]. そうしないと、データが更新されずに上書きされます。

#### ID データセットに関連付けられたスキーマ内の必須フィールド {#identity-dataset-required-fileds}

スキーマに必須フィールドが含まれている場合、を有効にするには、データセットの検証を抑制する必要があります [!DNL Identity Service] :id のみを受け取ります。 検証を無効にするには、 `disabled` 値を `acp_validationContext` パラメーター。 次の例を参照してください。

```shell
curl -X POST 'https://platform.adobe.io/data/foundation/catalog/dataSets/62257bef7a75461948ebcaaa' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "tags":{
        "acp_validationContext": ["disabled"]
        }
}'
```

>[!TIP]
>
>ID データセットに関連付けられているスキーマに必須フィールドがない場合は、追加の設定をおこなう必要はありません。

## 受信ペイロード構造

次の例は、新しい ID リンクを確立する受信ペイロード構造の例です。

### ID 設定を含むペイロード

```shell
{
  "header": {
    "flowId": "923e2ac3-3869-46ec-9e6f-7012c4e23f69",
    "imsOrgId": "{ORG_ID}",
    "datasetId": "621fc19ab33d941949af16c8",
    "operations": {
        "data": "create" (default)/"merge"/"delete",
        "identity": "create",
        "identityDatasetId": "621fc19ab33d941949af16d9"
    }
  }
... //The raw data attributes are included here as the key/value pairs of the "body" property.
}
```

| パラメーター | 説明 |
| --- | --- |
| `flowId` | データフローを識別する一意の ID。 このデータフロー ID は、 [!DNL Amazon Kinesis], [!DNL Azure Event Hubs]または [!DNL HTTP API]. また、このデータフローには、 [!DNL Profile]-enabled データセットをターゲットデータセットとして。 **注意**:の ID [!DNL Profile]有効なターゲットデータセットも `datasetId` パラメーター。 |
| `imsOrgId` | 組織に対応する ID。 |
| `datasetId` | の ID [!DNL Profile]-enabled データフローのターゲットデータセット。 **注意**:これは、 [!DNL Profile]データフロー内で有効なターゲットデータセット ID が見つかりました。 |
| `operations` | このパラメーターでは、 [!DNL Data Prep] は、受信リクエストに基づいて実行されます。 |
| `operations.data` | で実行する必要があるアクションを定義します。 [!DNL Profile Service]. |
| `operations.identity` | データに対して許可する操作を次のように定義します。 [!DNL Identity Service]. |
| `operations.identityDatasetId` | （オプション）新しい ID をリンクする必要がある場合にのみ必要な ID データセットの ID。 |

#### サポートされる操作

次の操作は、 [!DNL Profile Service]:

| 運用 | 説明 |
| --- | --- | 
| `create` | デフォルトの操作。 これにより、の XDM エンティティ作成メソッドが生成されます。 [!DNL Profile Service]. |
| `merge` | これにより、の XDM エンティティ更新メソッドが生成されます。 [!DNL Profile Service]. |
| `delete` | これにより、の XDM エンティティ削除メソッドが生成されます。 [!DNL Profile Service] また、 [!DNL Profile Store]. |

次の操作は、 [!DNL Identity Service]:

| 運用 | 説明 |
| --- | --- |
| `create` | このパラメーターに対して許可される唯一の操作です。 If `create` は `operations.identity`を、 [!DNL Data Prep] 次の XDM エンティティ作成リクエストを生成します： [!DNL Identity Service]. ID が既に存在する場合、ID は無視されます。 **注意：** If `operations.identity` が `create`、 `identityDatasetId` また、を指定する必要があります。 XDM エンティティは、 [!DNL Data Prep] このデータセット id に対してコンポーネントが生成されます。 |

### ID 設定のないペイロード

新しい ID をリンクする必要がない場合は、 `identity` および `identityDatasetId` パラメーターを使用します。 これにより、にのみデータが送信されます。 [!DNL Profile Service] そしてスキップ [!DNL Identity Service]. 以下の例のペイロードを参照してください。

```shell
{
  "header": {
    "flowId": "923e2ac3-3869-46ec-9e6f-7012c4e23f69",
    "imsOrgId": "{ORG_ID}",
    "datasetId": "621fc19ab33d941949af16c8",
    "operations": {
        "data": "create"/"merge"/"delete",
    }
  }
... //The raw data attributes are included here as the key/value pairs of the "body" property.
}
```

## プライマリ ID の動的な引き渡し

XDM の更新の場合、スキーマを有効にする必要があります。 [!DNL Profile] にはプライマリ id が含まれています。 XDM スキーマのプライマリ ID は、次の 2 つの方法で指定できます。

* 静的フィールドを XDM スキーマのプライマリ ID として指定する。
* XDM スキーマの ID マップフィールドグループを使用して、いずれかの ID フィールドをプライマリ ID として指定します。

### 静的フィールドを XDM スキーマのプライマリ ID フィールドとして指定する

次の例では、 `state`, `homePhone.number` その他の属性は、それぞれの指定された値で [!DNL Profile] ～の主なアイデンティティを持つ `sampleEmail@gmail.com`. 次に、XDM エンティティ更新メッセージがストリーミングによって生成されます [!DNL Data Prep] コンポーネント。 [!DNL Profile Service] 次に、XDM 更新メッセージがプロファイルレコードをアップサートすることを確認します。

>[!NOTE]
>
>この例では、ID に対して操作が定義されていないので、ID は相互にリンクされません。

```shell
curl -X POST 'https://dcs.adobedc.net/collection/9aba816d350a69c4abbd283eb5818ec3583275ffce4880ffc482be5a9d810c4b' \
  -H 'Content-Type: application/json' \
  -H 'x-adobe-flow-id: d5262d48-0f47-4949-be6d-795f06933527' \
  -d '{
    "header": {
        "flowId" : "d5262d48-0f47-4949-be6d-795f06933527",
        "imsOrgId": "{ORG_ID}",
        "datasetId": "62259f817f62d71947929a7b",
        "operations": {
         "data": "create"
     }
    },
    {
        "body": {
        "homeAddress": {
            "country": "US",
            "state": "GA",
            "region": "va7"
        },
        "homePhone": {
            "number": "123.456.799"
        },
        "identityMap": {
            "Email": [{
                "id": "sampleEmail@gmail.com",
                "primary": true
            }]
        },
      "personalEmail": {
            "address": "sampleEmail@gmail.com",
            "primary": true
       },
      "personID": "346576345",
      "_id": "346576345",
      "timestamp": "2021-05-05T17:51:45.1880+02",
      "workEmail": "sampleWorkEmail@gmail.com"
  }
}'
```

### XDM スキーマの ID マップフィールドグループを使用して、いずれかの ID フィールドをプライマリ ID として指定します

この例では、ヘッダーに `operations` 属性 `identity` および `identityDatasetId` プロパティ。 これにより、データを [!DNL Profile Service] また、に渡される ID [!DNL Identity Service].

```shell
curl -X POST 'https://dcs.adobedc.net/collection/9aba816d350a69c4abbd283eb5818ec3583275ffce4880ffc482be5a9d810c4b' \
  -H 'Content-Type: application/json' \
  -H 'x-adobe-flow-id: d5262d48-0f47-4949-be6d-795f06933527' \
  -d '{
    "header": {
        "flowId" : "d5262d48-0f47-4949-be6d-795f06933527",
        "imsOrgId": "{ORG_ID}",
        "datasetId": "62259f817f62d71947929a7b",
        "operations": {          
            "data": "merge",
            "identity": "create",
            "identityDatasetId": "6254a93b851ecd194b64af9e"
      }
    },
    {        
       "body": {
        "homeAddress": {
            "country": "US",
            "state": "GA",
            "region": "va7"
        },
        "homePhone": {
            "number": "123.456.799"
        },
        "identityMap": {
            "Email": [{
                "id": "sampleEmail@gmail.com",
                "primary": true
            }]
        },
      "personalEmail": {
            "address": "sampleEmail@gmail.com",
            "primary": true
       },
      "personID": "346576345",
      "_id": "346576345",
      "timestamp": "2021-05-05T17:51:45.1880+02",
      "workEmail": "sampleWorkEmail@gmail.com"
  }
 }'
```

## 次の手順

このドキュメントを読むと、でアップサートをストリーミングする方法を理解できます。 [!DNL Data Prep] 部分的な行の更新を [!DNL Profile Service] データを作成し、単一の API リクエストに id を作成してリンクすることもできます。 その他の情報 [!DNL Data Prep] 機能、お読みください [[!DNL Data Prep] 概要](./home.md). マッピングセットを [!DNL Data Prep] API( [[!DNL Data Prep] 開発者ガイド](./api/overview.md).
