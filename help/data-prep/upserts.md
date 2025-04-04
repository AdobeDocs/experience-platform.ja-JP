---
keywords: Experience Platform；ホーム；人気のトピック；data prep;Data Prep；ストリーミング；アップサート；ストリーミングアップサート
title: データ準備を使用した、リアルタイム顧客プロファイルへの部分行の更新の送信
description: データ準備を使用して、リアルタイム顧客プロファイルに部分行の更新を送信する方法を説明します。
exl-id: f9f9e855-0f72-4555-a4c5-598818fc01c2
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1361'
ht-degree: 3%

---

# [!DNL Data Prep] を使用した [!DNL Real-Time Customer Profile] への部分行の更新の送信

>[!IMPORTANT]
>
>* DCS インレットを介したプロファイル更新のための、エクスペリエンスデータモデル（XDM）エンティティ更新メッセージ（JSON PATCH操作を含む）の取り込みは非推奨（廃止予定）になりました。 このガイドで説明されている手順に従ってください。
>
>* また、HTTP API ソースを使用して [ 生データを DCS インレットに取り込む ](../sources/tutorials/api/create/streaming/http.md#sending-messages-to-an-authenticated-streaming-connection) したり、プロファイルの更新のためにデータを XDM 準拠のメッセージに変換するために必要なデータマッピングを指定したりすることもできます。
>
>* アップサートをストリーミングで配列を使用する場合は、操作の明確な目的を定義するために、`upsert_array_append` または `upsert_array_replace` を明示的に使用する必要があります。 これらの関数がない場合、エラーが発生することがあります。

[!DNL Data Prep] でストリーミングアップサートを使用して、[!DNL Real-Time Customer Profile] データに部分行の更新を送信すると同時に、単一の API リクエストで新しい ID リンクを作成および確立します。

アップサートをストリーミングすると、データの形式を維持しながら、取り込み時にデータを [!DNL Real-Time Customer Profile] のPATCH リクエストに変換できます。 指定した入力に基づいて、1 つの API ペイロード [!DNL Data Prep] 送信し、そのデータをPATCHと [!DNL Identity Service] CREATE リクエストの両方 [!DNL Real-Time Customer Profile] 変換できます。

[!DNL Data Prep] は、ヘッダーパラメーターを使用して、挿入とアップサートを区別します。 アップサートを使用する行には、すべてヘッダーが必要です。 ID 記述子を使用したアップサートも、使用しないアップサートも可能です。 ID でアップサートを使用している場合は、[ID データセットの設定 ](#configure-the-identity-dataset) の節で説明されている設定手順に従う必要があります。 ID を指定せずにアップサートを使用している場合は、リクエストで ID 設定を指定する必要はありません。 詳しくは、[ID を使用しないアップサートのストリーミング ](#payload-without-identity-configuration) の節を参照してください。

>[!NOTE]
>
>アップサート機能を活用するには、データ取り込み時に XDM 互換の設定をオフにし、[Data Prep Mapper](./ui/mapping.md) を使用して受信ペイロードを再マッピングすることをお勧めします。

このドキュメントでは、[!DNL Data Prep] でのアップサートのストリーミング方法について説明します。

## はじめに

この概要では、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Data Prep]](./home.md):[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model （XDM）との間でデータのマッピング、変換、検証をおこなうことができます。
* [[!DNL Identity Service]](../identity-service/home.md)：デバイスやシステム間で ID を橋渡しすることで、個々の顧客とその行動をより確実に把握することができます。
* [リアルタイム顧客プロファイル](../profile/home.md)：複数のソースから集約されたデータに基づいて、統合された顧客プロファイルをリアルタイムに提供します。
* [ ソース ](../sources/home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。

## [!DNL Data Prep] でのストリーミングアップサートの使用 {#streaming-upserts-in-data-prep}

>[!NOTE]
>
>次のソースは、ストリーミングアップサートの使用をサポートしています。<ul><li>[[!DNL Amazon Kinesis]](../sources/connectors/cloud-storage/kinesis.md)</li><li>[[!DNL Azure Event Hubs]](../sources/connectors/cloud-storage/eventhub.md)</li><li>[[!DNL HTTP API]](../sources/connectors/streaming/http.md)</li></ul>

### ストリーミングアップサートの高レベルなワークフロー

[!DNL Data Prep] でのアップサートのストリーミングは、次のように動作します。

* 最初に、データセットを作成し、[!DNL Profile] 用できるようにしておく必要があります。 詳しくは、[ データセットの有効化  [!DNL Profile]](../catalog/datasets/enable-for-profile.md) に関するガイドを参照してください。
* 新しい ID をリンクする必要がある場合は、[!DNL Profile] しいデータセットとして **同じスキーマを持つ** 追加のデータセットを作成する必要もあります。
* データセットの準備が整ったら、受信リクエストを [!DNL Profile] のデータセットにマッピングするためのデータフローを作成する必要があります。
* 次に、必要なヘッダーを含めるように受信リクエストを更新する必要があります。 これらのヘッダーは、以下を定義します。
   * [!DNL Profile] で実行する必要があるデータ操作：`create`、`merge`、および `delete`。
   * [!DNL Identity Service] で実行されるオプションの ID 操作：`create`。

### ID データセットの設定 {#configure-the-identity-dataset}

新しい ID をリンクする必要がある場合は、追加のデータセットを作成して、受信ペイロードに渡す必要があります。 ID データセットを作成する場合は、次の要件が満たされていることを確認する必要があります。

* ID データセットには、関連付けられたスキーマが [!DNL Profile] データセットとして必要です。 スキーマの不一致により、システム動作に一貫性がなくなる場合があります。
* ただし、ID データセットが [!DNL Profile] データセットと異なることを確認する必要があります。 データセットが同じ場合、データは更新されず、上書きされます。
* 初期データセットは [!DNL Profile] に対して有効にする必要がありますが、ID データセットは [!DNL Profile] に対して **有効にしない** ください。 そうしないと、データは更新されずに上書きされてしまいます。 ただし、[!DNL Identity Service] に対しては、ID データセット **有効にする必要があります** です。

#### ID データセットに関連付けられたスキーマの必須フィールド {#identity-dataset-required-fileds}

スキーマに必須フィールドが含まれている場合、[!DNL Identity Service] が ID のみを受け取ることができるように、データセットの検証を抑制する必要があります。 `disabled` の値を `acp_validationContext` パラメーターに適用することで、検証を抑制できます。 以下の例を参照してください。

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
| `flowId` | データフローを識別する一意の ID。 このデータフロー ID は、[!DNL Amazon Kinesis]、[!DNL Azure Event Hubs]、[!DNL HTTP API] のいずれかで作成されたソース接続に対応している必要があります。 また、このデータフローには、ターゲットデータセットとして [!DNL Profile] 対応データセットが必要です。 **メモ**:[!DNL Profile] 対応のターゲットデータセットの ID も、`datasetId` パラメーターとして使用されます。 |
| `imsOrgId` | 組織に対応する ID。 |
| `datasetId` | データフローの [!DNL Profile] 対応ターゲットデータセットの ID。 **メモ**：これは、データフローで見つかった [!DNL Profile] 対応のターゲットデータセット ID と同じ ID です。 |
| `operations` | このパラメーターは、受信リクエストに基づ [!DNL Data Prep] て実行されるアクションの概要を示します。 |
| `operations.data` | [!DNL Real-Time Customer Profile] で実行する必要があるアクションを定義します。 |
| `operations.identity` | データに対して許可される操作を [!DNL Identity Service] で定義します。 |
| `operations.identityDatasetId` | （任意）新しい ID をリンクする必要がある場合にのみ必須の ID データセットの ID。 |

#### サポートされる操作

[!DNL Real-Time Customer Profile] では、次の操作がサポートされています。

| 運用 | 説明 |
| --- | --- | 
| `create` | デフォルトの操作。 これにより、[!DNL Real-Time Customer Profile] 用の XDM エンティティ作成メソッドが生成されます。 |
| `merge` | これにより、[!DNL Real-Time Customer Profile] の XDM エンティティ更新メソッドが生成されます。 |
| `delete` | これにより、[!DNL Real-Time Customer Profile] の XDM エンティティ削除メソッドが生成され、[!DNL Profile store] からデータが永続的に削除されます。 |

[!DNL Identity Service] では、次の操作がサポートされています。

| 運用 | 説明 |
| --- | --- |
| `create` | このパラメーターで許可されている操作のみ。 `create` が `operations.identity` の値として渡されると、[!DNL Data Prep] は [!DNL Identity Service] の XDM エンティティ作成リクエストを生成します。 ID が既に存在する場合、その ID は無視されます。 **注意：** `operations.identity` が `create` に設定されている場合は、`identityDatasetId` も指定する必要があります。 コンポーネントによって内部で生成された XDM エンティティ作成メッセージ [!DNL Data Prep]、このデータセット ID に対して生成されます。 |

### ID 設定のないペイロード {#payload-without-identity-configuration}

新しい ID をリンクする必要がない場合は、`identity` パラメーターと `identityDatasetId` パラメーターを操作で省略できます。 これにより、データは [!DNL Real-Time Customer Profile] にのみ送信され、[!DNL Identity Service] はスキップされます。 以下のペイロードの例を参照してください。

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

XDM を更新する場合は、スキーマが [!DNL Profile] に対して有効になっており、プライマリ ID が含まれている必要があります。 XDM スキーマのプライマリ ID は、次の 2 つの方法で指定できます。

* XDM スキーマのプライマリ ID として静的フィールドを指定します。
* ID フィールドの 1 つをプライマリ ID として、XDM スキーマの ID マップフィールドグループを使用して指定します。

### 静的フィールドを XDM スキーマのプライマリ ID フィールドとして指定します

次の例では、`state`、`homePhone.number` およびその他の属性が、それぞれ指定された値でアップサートされ、`sampleEmail@gmail.com` のプライマリ ID で [!DNL Profile] に入れられます。 次に、ストリーミング [!DNL Data Prep] コンポーネントによって XDM エンティティ更新メッセージが生成されます。 次に、XDM 更新メッセージ [!DNL Real-Time Customer Profile] プロファイルレコードをアップサートすることを確認します。

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

この例では、ヘッダーに、`identity` と `identityDatasetId` のプロパティを持つ `operations` 属性が含まれています。 これにより、データを [!DNL Real-Time Customer Profile] と結合したり、ID を [!DNL Identity Service] に渡したりすることもできます。

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

次に、[!DNL Data Prep] でアップサートをストリーミングする際に考慮すべき既知の制限事項のリストを示します。

* ストリーミングアップサート方法は、部分行の更新を [!DNL Real-Time Customer Profile] に送信する場合にのみ使用してください。 部分行の更新は、データレイクによって使用 **れ** せん）。
* ストリーミングアップサートのメソッドでは、ID の更新、置換、削除はサポートされていません。 存在しない場合は、新しい ID が作成されます。 したがって、`identity` 操作は常に create に設定する必要があります。 ID が既に存在する場合、操作は no-op です。
* ストリーミングアップサート方式では、現在、[Adobe Experience Platform Web SDK](/help/web-sdk/home.md) および [Adobe Experience Platform Mobile SDK](https://developer.adobe.com/client-sdks/documentation/) をサポートしていません。

## 次の手順

このドキュメントでは、アップサートをストリーミングして [!DNL Real-Time Customer Profile] データに部分行の更新を送信すると同時に、単一の API リクエストで ID を作成 [!DNL Data Prep] リンクする方法を説明しました。 その他の [!DNL Data Prep] 機能について詳しくは、[[!DNL Data Prep]  概要 ](./home.md) を参照してください。 [!DNL Data Prep] API 内でのマッピングセットの使用方法については、[[!DNL Data Prep]  開発者ガイド ](./api/overview.md) を参照してください。
