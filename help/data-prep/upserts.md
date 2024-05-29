---
keywords: Experience Platform；ホーム；人気のトピック；data prep;Data Prep；ストリーミング；アップサート；ストリーミングアップサート
title: データ準備を使用した、リアルタイム顧客プロファイルへの部分行の更新の送信
description: データ準備を使用して、リアルタイム顧客プロファイルに部分行の更新を送信する方法を説明します。
exl-id: f9f9e855-0f72-4555-a4c5-598818fc01c2
source-git-commit: e52eb90b64ae9142e714a46017cfd14156c78f8b
workflow-type: tm+mt
source-wordcount: '1241'
ht-degree: 5%

---

# 部分行の更新の送信先 [!DNL Real-Time Customer Profile] 使用 [!DNL Data Prep]

>[!WARNING]
>
>DCS インレットを介したプロファイル更新のエクスペリエンスデータモデル（XDM）エンティティ更新メッセージ（JSON PATCH操作を含む）への取り込みは非推奨（廃止予定）になりました。 または、次のことができます。 [生データを DCS インレットに取り込みます。](../sources/tutorials/api/create/streaming/http.md#sending-messages-to-an-authenticated-streaming-connection) を使用して、プロファイルの更新のためにデータを XDM 準拠のメッセージに変換するために必要なデータマッピングを指定します。

アップサートのストリーミング [!DNL Data Prep] 部分行の更新をに送信できます [!DNL Real-Time Customer Profile] 単一の API リクエストで新しい ID リンクを作成および確立する際にも、データを使用します。

アップサートをストリーミングすると、データをに変換する間、データの形式を保持できます [!DNL Real-Time Customer Profile] 取り込み中のPATCHリクエスト。 指定した入力に基づいて、 [!DNL Data Prep] 単一の API ペイロードを送信し、データを両方に変換できるようにします [!DNL Real-Time Customer Profile] PATCHと [!DNL Identity Service] リクエストを作成します。

>[!NOTE]
>
>アップサート機能を活用するには、データ取り込み時に XDM 互換の設定をオフにし、を使用して受信ペイロードを再マッピングすることをお勧めします [データ準備マッパー](./ui/mapping.md).

このドキュメントでは、でのアップサートのストリーミング方法について説明します [!DNL Data Prep].

## はじめに

この概要では、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Data Prep]](./home.md): [!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model （XDM）との間でデータのマッピング、変換、検証をおこなうことができます。
* [[!DNL Identity Service]](../identity-service/home.md)：デバイスやシステム間で ID を橋渡しすることで、個々の顧客とその行動をより確実に把握することができます。
* [リアルタイム顧客プロファイル](../profile/home.md)：複数のソースから集約されたデータに基づいて、統合された顧客プロファイルをリアルタイムに提供します。
* [ソース](../sources/home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。

## でのストリーミングアップサートの使用 [!DNL Data Prep] {#streaming-upserts-in-data-prep}

>[!NOTE]
>
>次のソースは、ストリーミングアップサートの使用をサポートしています。<ul><li>[[!DNL Amazon Kinesis]](../sources/connectors/cloud-storage/kinesis.md)</li><li>[[!DNL Azure Event Hubs]](../sources/connectors/cloud-storage/eventhub.md)</li><li>[[!DNL HTTP API]](../sources/connectors/streaming/http.md)</li></ul>

### ストリーミングアップサートの高レベルなワークフロー

アップサートのストリーミング [!DNL Data Prep] は次のように動作します。

* 最初に、のデータセットを作成して有効にする必要があります [!DNL Profile] 消費。 のガイドを参照してください [データセットの有効化 [!DNL Profile]](../catalog/datasets/enable-for-profile.md) を参照してください。
* 新しい ID をリンクする必要がある場合は、追加のデータセットも作成する必要があります **同じスキーマを使用** as your [!DNL Profile] データセット。
* データセットの準備が整ったら、受信リクエストをにマッピングするためのデータフローを作成する必要があります。 [!DNL Profile] データセット；
* 次に、必要なヘッダーを含めるように受信リクエストを更新する必要があります。 これらのヘッダーは、以下を定義します。
   * で実行する必要があるデータ操作 [!DNL Profile]: `create`, `merge`、および `delete`.
   * で実行するオプションの ID 操作 [!DNL Identity Service]: `create`.

### ID データセットの設定

新しい ID をリンクする必要がある場合は、追加のデータセットを作成して、受信ペイロードに渡す必要があります。 ID データセットを作成する場合は、次の要件が満たされていることを確認する必要があります。

* ID データセットには、関連するスキーマがである必要があります。 [!DNL Profile] データセット。 スキーマの不一致により、システム動作に一貫性がなくなる場合があります。
* ただし、ID データセットがと異なることを確認する必要があります [!DNL Profile] データセット。 データセットが同じ場合、データは更新されず、上書きされます。
* 一方、初期データセットはを有効にする必要があります [!DNL Profile]、id データセット **有効にしないでください** （用） [!DNL Profile]. そうしないと、データは更新されずに上書きされてしまいます。 ただし、ID データセット **有効にする必要があります** （用） [!DNL Identity Service].

#### ID データセットに関連付けられたスキーマの必須フィールド {#identity-dataset-required-fileds}

スキーマに必須フィールドが含まれている場合、有効にするには、データセットの検証を抑制する必要があります [!DNL Identity Service] ID のみを受信する場合。 検証を抑制するには、 `disabled` の値をに変更します。 `acp_validationContext` パラメーター。 以下の例を参照してください。

```shell
curl -X POST 'https://platform.adobe.io/data/foundation/catalog/dataSets/62257bef7a75461948ebcaaa' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "tags": {
        "acp_validationContext": [
            "disabled"
        ],
        "unifiedProfile": [
            "enabled:false"
        ],
        "unifiedIdentity": [
            "enabled:true"
        ]
    }
}'
```

>[!TIP]
>
>ID データセットに関連付けられたスキーマに必須フィールドがない場合は、追加の設定を行う必要はありません。

## 受信ペイロード構造

以下に、新しい ID リンクを確立する受信ペイロード構造の例を示します。

### ID 設定を持つペイロード

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
| `flowId` | データフローを識別する一意の ID。 このデータフロー ID は、で作成されたソース接続に対応している必要があります [!DNL Amazon Kinesis], [!DNL Azure Event Hubs]、または [!DNL HTTP API]. このデータフローには、も含まれている必要があります [!DNL Profile]が有効なデータセットをターゲットデータセットとして選択しました。 **注意**：の ID [!DNL Profile]を有効にしたターゲットデータセットは、としても使用されます `datasetId` パラメーター。 |
| `imsOrgId` | 組織に対応する ID。 |
| `datasetId` | の ID [!DNL Profile]がデータフローの有効なターゲットデータセットを有効化しました。 **注意**：これは [!DNL Profile]がデータフロー内に有効なターゲットデータセット ID を見つけました。 |
| `operations` | このパラメーターでは、のアクションの概要を説明します [!DNL Data Prep] 受信リクエストに基づいておこなわれます。 |
| `operations.data` | で実行する必要があるアクションを定義します。 [!DNL Real-Time Customer Profile]. |
| `operations.identity` | によって、データに許可された操作を定義します [!DNL Identity Service]. |
| `operations.identityDatasetId` | （任意）新しい ID をリンクする必要がある場合にのみ必須の ID データセットの ID。 |

#### サポートされる操作

次の操作は、でサポートされています。 [!DNL Real-Time Customer Profile]:

| 運用 | 説明 |
| --- | --- | 
| `create` | デフォルトの操作。 これにより、の XDM エンティティ作成メソッドが生成されます [!DNL Real-Time Customer Profile]. |
| `merge` | これにより、の XDM エンティティ更新メソッドが生成されます [!DNL Real-Time Customer Profile]. |
| `delete` | これにより、の XDM エンティティ削除メソッドが生成されます [!DNL Real-Time Customer Profile] を削除し、 [!DNL Profile store]. |

次の操作は、でサポートされています。 [!DNL Identity Service]:

| 運用 | 説明 |
| --- | --- |
| `create` | このパラメーターで許可されている操作のみ。 次の場合 `create` は、の値として渡されます。 `operations.identity`、次に [!DNL Data Prep] 次の XDM エンティティ作成リクエストを生成 [!DNL Identity Service]. ID が既に存在する場合、その ID は無視されます。 **注意：** 次の場合 `operations.identity` はに設定されています。 `create`を選択し、続いて `identityDatasetId` も指定する必要があります。 によって内部で生成された XDM エンティティ作成メッセージ [!DNL Data Prep] このデータセット id に対してコンポーネントが生成されます。 |

### ID 設定のないペイロード

新しい ID をリンクする必要がない場合は、を省略できます `identity` および `identityDatasetId` 操作のパラメーター。 これにより、データは次の場所にのみ送信されます。 [!DNL Real-Time Customer Profile] をスキップします [!DNL Identity Service]. 以下のペイロードの例を参照してください。

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

## プライマリ ID の動的な受け渡し

XDM を更新する場合、スキーマでを有効にする必要があります [!DNL Profile] とはプライマリ ID を含みます。 XDM スキーマのプライマリ ID は、次の 2 つの方法で指定できます。

* XDM スキーマのプライマリ ID として静的フィールドを指定します。
* ID フィールドの 1 つをプライマリ ID として、XDM スキーマの ID マップフィールドグループを使用して指定します。

### 静的フィールドを XDM スキーマのプライマリ ID フィールドとして指定します

以下の例では、 `state`, `homePhone.number` およびその他の属性は、それぞれの指定された値でにアップサートされます。 [!DNL Profile] ～という主体性を持つ `sampleEmail@gmail.com`. 次に、ストリーミングによって XDM エンティティ更新メッセージが生成されます [!DNL Data Prep] コンポーネント。 [!DNL Real-Time Customer Profile] 次に、XDM 更新メッセージがプロファイルレコードをアップサートすることを確認します。

>[!NOTE]
>
>この例では、ID に対して操作が定義されていないので、ID はリンクされません。

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

### ID フィールドの 1 つをプライマリ ID として、XDM スキーマの ID マップフィールドグループを使用して指定します

この例では、ヘッダーにが含まれています `operations` を持つ属性 `identity` および `identityDatasetId` プロパティ。 これにより、データをに結合できます [!DNL Real-Time Customer Profile] また、に ID を渡す場合にも使用できます [!DNL Identity Service].

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

## 既知の制限と主な考慮事項

次に、アップサートをストリーミングする際に考慮すべき既知の制限事項のリストを示します [!DNL Data Prep]:

* ストリーミングアップサート方法は、行の部分更新をに送信する場合にのみ使用してください。 [!DNL Real-Time Customer Profile]. 部分行の更新は次のとおりです **ではない** データレイクで使用されます。
* ストリーミングアップサートのメソッドでは、ID の更新、置換、削除はサポートされていません。 存在しない場合は、新しい ID が作成されます。 したがって、 `identity` 操作は常に create に設定する必要があります。 ID が既に存在する場合、操作は no-op です。
* ストリーミングアップサート方法は現在サポートされていません [Adobe Experience Platform Web SDK](/help/web-sdk/home.md) および [Adobe Experience Platform Mobile SDK](https://developer.adobe.com/client-sdks/documentation/).

## 次の手順

このドキュメントでは、でアップサートをストリーミングする方法を説明しました [!DNL Data Prep] に部分行の更新を送信するには [!DNL Real-Time Customer Profile] データは、単一の API リクエストで ID を作成およびリンクすることもできます。 その他の詳細情報 [!DNL Data Prep] 機能です。を読んでください。 [[!DNL Data Prep] の概要](./home.md). 内でのマッピングセットの使用方法を説明します [!DNL Data Prep] API。を読み取ってください。 [[!DNL Data Prep] 開発者ガイド](./api/overview.md).
