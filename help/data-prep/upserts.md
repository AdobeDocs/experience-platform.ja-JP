---
keywords: Experience Platform；ホーム；人気のトピック；データ準備；データ準備；ストリーミング；アップサート；ストリーミングアップサート
title: データ準備を使用して、行の一部をリアルタイムの顧客プロファイルに送信します
description: データ準備を使用してリアルタイム顧客プロファイルに行の部分的な更新を送信する方法について説明します。
exl-id: f9f9e855-0f72-4555-a4c5-598818fc01c2
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1363'
ht-degree: 3%

---

# [!DNL Real-Time Customer Profile]を使用して行の部分的な更新を[!DNL Data Prep]に送信します

>[!IMPORTANT]
>
>* DCS インレットを介したプロファイル更新のExperience Data Model （XDM） Entity Update メッセージ（JSON PATCH操作を含む）の取り込みは非推奨になりました。 代替案として、このガイドで説明した手順に従ってください。
>
>* また、HTTP API ソースを使用して[生データをDCS インレット &#x200B;](../sources/tutorials/api/create/streaming/http.md#sending-messages-to-an-authenticated-streaming-connection)に取り込み、必要なデータマッピングを指定して、データをXDM準拠のメッセージに変換してプロファイルを更新することもできます。
>
>* ストリーミングアップサートで配列を使用する場合、操作の明確な意図を定義するために`upsert_array_append`または`upsert_array_replace`を明示的に使用する必要があります。 これらの関数が見つからない場合は、エラーが発生する可能性があります。

[!DNL Data Prep]でのストリーミングアップサートを使用して、部分的な行の更新を[!DNL Real-Time Customer Profile] データに送信すると同時に、単一のAPI リクエストで新しいID リンクを作成および確立します。

アップサートをストリーミングすることで、取り込み中にデータを[!DNL Real-Time Customer Profile]件のPATCH リクエストに変換する際に、データのフォーマットを保持できます。 入力に基づいて、[!DNL Data Prep]では、1つのAPI ペイロードを送信し、データを[!DNL Real-Time Customer Profile]個のPATCHと[!DNL Identity Service]個のCREATE リクエストの両方に変換できます。

[!DNL Data Prep]は、挿入とアップサートを区別するためにヘッダーパラメーターを使用します。 アップサートを使用するすべての行にはヘッダーが必要です。 ID記述子の有無にかかわらずアップサートを使用できます。 IDを持つアップサートを使用する場合は、[ID データセットの設定](#configure-the-identity-dataset)の節で説明されている設定手順に従う必要があります。 IDなしでアップサートを使用している場合は、リクエストでID設定を指定する必要はありません。 詳しくは、[IDを持たないアップサートのストリーミング &#x200B;](#payload-without-identity-configuration)に関する節を参照してください。

>[!NOTE]
>
>アップサート機能を活用するには、データ取り込み中にXDM互換の設定をオフにし、[&#x200B; データ準備マッパー](./ui/mapping.md)を使用して受信ペイロードを再マッピングすることをお勧めします。

このドキュメントでは、[!DNL Data Prep]でアップサートをストリーミングする方法について説明します。

## はじめに

この概要では、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Data Prep]](./home.md): [!DNL Data Prep]を使用すると、データ エンジニアはExperience Data Model （XDM）との間でデータをマッピング、変換、検証できます。
* [[!DNL Identity Service]](../identity-service/home.md): デバイスとシステム間でIDを橋渡しすることで、個々の顧客とその行動をより詳細に把握します。
* [リアルタイム顧客プロファイル](../profile/home.md)：複数のソースから集約されたデータに基づいて、統合された顧客プロファイルをリアルタイムに提供します。
* [&#x200B; ソース &#x200B;](../sources/home.md): Experience Platformを使用すると、様々なソースからデータを取り込むことができますが、Experience Platform サービスを使用して着信データを構造化、ラベル付け、強化することができます。

## [!DNL Data Prep]でのストリーミングアップサートの使用 {#streaming-upserts-in-data-prep}

>[!NOTE]
>
>次のソースは、ストリーミングアップサートの使用をサポートしています。<ul><li>[[!DNL Amazon Kinesis]](../sources/connectors/cloud-storage/kinesis.md)</li><li>[[!DNL Azure Event Hubs]](../sources/connectors/cloud-storage/eventhub.md)</li><li>[[!DNL HTTP API]](../sources/connectors/streaming/http.md)</li></ul>

### ストリーミングアップサートの上位レベルのワークフロー

[!DNL Data Prep]のストリーミングアップサートは次のように機能します。

* 最初に、[!DNL Profile]の使用に対してデータセットを作成し、有効にする必要があります。 詳しくは、[のデータセットを有効にする [!DNL Profile]](../catalog/datasets/enable-for-profile.md)に関するガイドを参照してください。
* 新しいIDをリンクする必要がある場合は、**データセットと同じスキーマ**&#x200B;を使用して、追加のデータセット [!DNL Profile]も作成する必要があります。
* データセットを準備したら、受信リクエストを[!DNL Profile] データセットにマッピングするためのデータフローを作成する必要があります。
* 次に、必要なヘッダーを含めるように、受信リクエストを更新する必要があります。 これらのヘッダーは次のように定義します。
   * [!DNL Profile]で実行する必要があるデータ操作：`create`、`merge`、および`delete`。
   * [!DNL Identity Service]で実行するオプション ID操作：`create`。

### ID データセットの設定 {#configure-the-identity-dataset}

新しいIDをリンクする必要がある場合は、追加のデータセットを作成して受信ペイロードに渡す必要があります。 ID データセットを作成する場合は、次の要件が満たされていることを確認する必要があります。

* ID データセットには、[!DNL Profile] データセットとして関連付けられたスキーマが必要です。 スキーマの不一致は、システム動作の一貫性を損なう可能性があります。
* ただし、ID データセットが[!DNL Profile] データセットと異なることを確認する必要があります。 データセットが同じ場合、データは更新されずに上書きされます。
* [!DNL Profile]に対して初期データセットを有効にする必要がありますが、**に対してID データセット**&#x200B;を有効にすることはできません[!DNL Profile]。 そうでない場合は、データも更新されずに上書きされます。 ただし、**に対してID データセット**&#x200B;を有効にする必要があります[!DNL Identity Service]。

#### ID データセットに関連付けられたスキーマの必須フィールド {#identity-dataset-required-fileds}

スキーマに必須フィールドが含まれている場合、[!DNL Identity Service]がIDのみを受信できるようにするには、データセットの検証を抑制する必要があります。 `disabled`値を`acp_validationContext` パラメーターに適用することで、検証を抑制できます。 次の例を参照してください。

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
>ID データセットに関連付けられたスキーマに必須フィールドがない場合、追加の設定を行う必要はありません。

## 受信ペイロード構造

次に、新しいID リンクを確立する受信ペイロード構造の例を示します。

### ID設定を使用したペイロード

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
| `flowId` | データフローを識別するための一意のID。 このデータフローIDは、[!DNL Amazon Kinesis]、[!DNL Azure Event Hubs]または[!DNL HTTP API]で作成されたソース接続に対応する必要があります。 このデータフローには、ターゲットデータセットとして[!DNL Profile]が有効なデータセットも必要です。 **注**: [!DNL Profile]が有効なターゲット データセットのIDは、`datasetId` パラメーターとしても使用されます。 |
| `imsOrgId` | 組織に対応するID。 |
| `datasetId` | データフローの[!DNL Profile]対応ターゲットデータセットのID。 **メモ**：これは、データフローに見つかった[!DNL Profile]対応のターゲットデータセット IDと同じIDです。 |
| `operations` | このパラメーターは、受信リクエストに基づいて[!DNL Data Prep]が実行するアクションの概要を示します。 |
| `operations.data` | [!DNL Real-Time Customer Profile]で実行する必要があるアクションを定義します。 |
| `operations.identity` | データに対して許可される操作を[!DNL Identity Service]で定義します。 |
| `operations.identityDatasetId` | （オプション）新しいIDをリンクする必要がある場合にのみ必要なID データセットのID。 |

#### サポートされる操作

次の操作は[!DNL Real-Time Customer Profile]によってサポートされています：

| 運用 | 説明 |
| --- | --- |
| `create` | デフォルトの操作。 これにより、[!DNL Real-Time Customer Profile]のXDM エンティティ作成メソッドが生成されます。 |
| `merge` | これにより、[!DNL Real-Time Customer Profile]のXDM エンティティ更新メソッドが生成されます。 |
| `delete` | これにより、[!DNL Real-Time Customer Profile]のXDM エンティティ削除方法が生成され、[!DNL Profile store]からデータが完全に削除されます。 |

次の操作は[!DNL Identity Service]によってサポートされています：

| 運用 | 説明 |
| --- | --- |
| `create` | このパラメーターに対して許可されている操作は1つだけです。 `create`が`operations.identity`の値として渡された場合、[!DNL Data Prep]は[!DNL Identity Service]に対するXDM エンティティ作成リクエストを生成します。 IDが既に存在する場合、そのIDは無視されます。 **メモ：** `operations.identity`が`create`に設定されている場合は、`identityDatasetId`も指定する必要があります。 [!DNL Data Prep] コンポーネントによって内部で生成されたXDM エンティティ作成メッセージは、このデータセット IDに対して生成されます。 |

### ID設定のないペイロード {#payload-without-identity-configuration}

新しいIDをリンクする必要がない場合は、操作で`identity`と`identityDatasetId`のパラメーターを省略できます。 これにより、データは[!DNL Real-Time Customer Profile]にのみ送信され、[!DNL Identity Service]はスキップされます。 例については、以下のペイロードを参照してください。

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

## プライマリ IDを動的に渡す

XDMを更新するには、スキーマを[!DNL Profile]に対して有効にし、プライマリ IDを含める必要があります。 XDM スキーマのプライマリ IDは、次の2つの方法で指定できます。

* XDM スキーマのプライマリ IDとして静的フィールドを指定します。
* XDM スキーマのID マップフィールドグループを使用して、いずれかのID フィールドをプライマリ IDとして指定します。

### 静的フィールドをXDM スキーマのプライマリ ID フィールドとして指定します

次の例では、`state`、`homePhone.number`およびその他の属性が、それぞれ指定された値で[!DNL Profile]に挿入され、プライマリ IDが`sampleEmail@gmail.com`になります。 次に、ストリーミング [!DNL Data Prep] コンポーネントによってXDM エンティティの更新メッセージが生成されます。 次に、[!DNL Real-Time Customer Profile]は、プロファイルレコードをアップサートするためのXDM更新メッセージを確認します。

>[!NOTE]
>
>この例では、IDに対して定義された操作がないので、IDはリンクされません。

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

### XDM スキーマのID マップフィールドグループを使用して、いずれかのID フィールドをプライマリ IDとして指定します

この例では、ヘッダーに`operations`および`identity` プロパティを持つ`identityDatasetId`属性が含まれています。 これにより、データを[!DNL Real-Time Customer Profile]と結合したり、IDを[!DNL Identity Service]に渡したりできます。

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

## 既知の制限事項と主な考慮事項

次に、[!DNL Data Prep]を使用してアップサートをストリーミングする際に考慮すべき既知の制限事項のリストを示します。

* ストリーミングアップサートメソッドは、部分的な行の更新を[!DNL Real-Time Customer Profile]に送信する場合にのみ使用する必要があります。 一部の行の更新は、データレイクで使用される&#x200B;**not**&#x200B;です。
* ストリーミングアップサートメソッドでは、IDの更新、置換、削除はサポートされていません。 存在しない場合は、新しいIDが作成されます。 したがって、`identity`操作は常に作成するように設定する必要があります。 IDが既に存在する場合、操作はno-opです。
* 現在、ストリーミングアップサートメソッドは、[Adobe Experience Platform Web SDK](/help/collection/js/js-overview.md)または[Adobe Experience Platform Mobile SDK](https://developer.adobe.com/client-sdks/documentation/)をサポートしていません。

## 次の手順

このドキュメントを読むことで、1つのAPI リクエストでIDを作成およびリンクしながら、[!DNL Data Prep]でアップサートをストリーミングして[!DNL Real-Time Customer Profile] データに部分的な行の更新を送信する方法を理解できるようになりました。 その他[!DNL Data Prep]機能について詳しくは、[[!DNL Data Prep] 概要](./home.md)を参照してください。 [!DNL Data Prep] API内でマッピングセットを使用する方法については、[[!DNL Data Prep] 開発者ガイド &#x200B;](./api/overview.md)を参照してください。
