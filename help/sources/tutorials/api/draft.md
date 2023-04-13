---
keywords: Experience Platform;ホーム;人気のトピック;Flow Service;
title: Flow Service API を使用したドラフトデータフロー
description: Flow Service API を使用してデータフローをドラフト状態に設定する方法を説明します。
badge: label="新機能" type="Positive"
source-git-commit: d093e34ae4b353d1ed6db922b6da66cf23f25c48
workflow-type: ht
source-wordcount: '591'
ht-degree: 100%

---

# Flow Service API を使用したドラフトデータフロー

フロー作成呼び出し時に `mode=draft` クエリパラメーターを指定することで、Flow Service API を使用する際にデータフローをドラフトとして保存します。ドラフトは、後で新しい情報を使用して更新し、準備が整ったら公開できます。このチュートリアルでは、Flow Service API を使用してデータフローをドラフト状態に設定する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### 前提条件

このチュートリアルでは、データフローを作成するために必要なアセットが既に生成されている必要があります。これには以下が含まれます。

* 認証済みのベース接続
* ソース接続
* ターゲット XDM スキーマ
* ターゲットデータセット
* ターゲット接続
* マッピング

これらの値がまだない場合は、[ソースのカタログの概要](../../home.md)からソースを選択します。次に、その指定したソースの指示に従って、データフローのドラフト設定に必要なアセットを生成します。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../landing/api-guide.md)のガイドを参照してください。

## データフローのドラフト状態への設定

以下の節では、データフローをドラフトとして設定し、データフローを更新し、データフローを公開して、最終的にデータフローを削除するプロセスの概要を説明します。

### データフローのドラフト設定

データフローをドラフトとして設定するには、`mode=draft` をクエリパラメーターとして追加したうえで、`/flows` エンドポイントに対して POST リクエストを行います。これにより、データフローを作成し、ドラフトとして保存できます。

**API 形式**

```http
POST /flows?mode=draft
```

| パラメーター | 説明 |
| --- | --- |
| `mode` | データフローの状態を決定するユーザー指定のクエリパラメーター。データフローをドラフトとして設定するには、`mode` を `draft` に設定します。 |

**リクエスト**

次のリクエストでは、ドラフトデータフローを作成しています。

```shell
  'https://platform-int.adobe.io/data/foundation/flowservice/flows?mode=draft' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "HTTP source dataflow for ACME data",
    "sourceConnectionIds": [
        "61c0c5f1-bfe5-40f7-8f8c-a4dc175ddac6"
    ],
    "targetConnectionIds": [
        "78f41c31-3652-4a5e-b264-74331226dcf3"
    ],
    "flowSpec": {
        "id": "c1a19761-d2c7-4702-b9fa-fe91f0613e81",
        "version": "1.0"
    }
  }'
```

**応答**

正常な応答は、データフローの `id` と対応する `etag` を返します。

```json
{
  "id": "c9751426-dff8-49b0-965f-50defcf4187b",
  "etag": "\"69057131-0000-0200-0000-640f48320000\""
}
```

### データフローの更新

ドラフトを更新するには、更新するデータフローの ID を指定したうえで、`/flows` エンドポイントに対して PATCH リクエストを行います。この手順では、更新するデータフローの `etag` に対応する `If-Match` ヘッダーパラメーターも指定する必要があります。

**API 形式**

```http
PATCH /flows/{FLOW_ID}
```

**リクエスト**

次のリクエストでは、ドラフトに設定したデータフローにマッピング変換を追加しています。

```shell
curl -X PATCH \
  'https://platform.adobe.io/data/foundation/flowservice/flows/c9751426-dff8-49b0-965f-50defcf4187b' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
  -H 'If-Match: "69057131-0000-0200-0000-640f48320000"' \
  -d '[
        {
          "op": "add",
          "path": "/transformations",
          "value": [
              {
                  "name": "Mapping",
                  "params": {
                      "mappingId": "44d42ed27c46499a80eb0c0705c38cbd",
                      "mappingVersion": 0
                  }
              }
          ]
      }
  ]'
```

**応答**

正常な応答は、フロー ID と `etag` を返します。変更を確認するには、フロー ID を指定したうえで `/flows` エンドポイントに対して GET リクエストを行います。

```json
{
  "id": "c9751426-dff8-49b0-965f-50defcf4187b",
  "etag": "\"69057131-0000-0200-0000-640f48320000\""
}
```

### データフローの公開

ドラフトを公開する準備が整ったら、公開するドラフトデータフローの ID と公開用のアクション操作を指定したうえで、`/flows` エンドポイントに対して POST リクエストを行います。

**API 形式**

```http
POST /flows/{FLOW_ID}/action?op=publish
```

| パラメーター | 説明 |
| --- | --- |
| `op` | クエリされたデータフローの状態を更新するアクション操作。ドラフトデータフローを公開するには、`op` を `publish` に設定します。 |

**リクエスト**

次のリクエストでは、ドラフトデータフローを公開しています。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows/c9751426-dff8-49b0-965f-50defcf4187b/action?op=publish' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、データフローの ID と対応する `etag` を返します。

```json
{
  "id": "c9751426-dff8-49b0-965f-50defcf4187b",
  "etag": "\"69057131-0000-0200-0000-640f48320000\""
}
```

### データフローの削除

データフローを削除するには、削除するデータフローの ID を指定したうえで、`/flows` エンドポイントに対して DELETE リクエストを行います。Flow Service API を使用してデータフローを削除する手順について詳しくは、[API でのデータフローの削除](./delete-dataflows.md)に関するガイドを参照してください。