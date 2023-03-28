---
keywords: Experience Platform；ホーム；人気の高いトピック；フローサービス；
title: フローサービス API を使用したドラフトデータフロー
description: フローサービス API を使用して、データフローをドラフト状態に設定する方法を説明します。
badge: label="新機能" type="ポジティブ"
source-git-commit: d093e34ae4b353d1ed6db922b6da66cf23f25c48
workflow-type: tm+mt
source-wordcount: '591'
ht-degree: 18%

---

# フローサービス API を使用したドラフトデータフロー

フローサービス API を使用する場合は、 `mode=draft` フロー作成呼び出し中のクエリパラメーター。 ドラフトは、後で新しい情報で更新し、準備が整ったら公開できます。 このチュートリアルでは、フローサービス API を使用してデータフローをドラフト状態に設定する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### 前提条件

このチュートリアルでは、データフローを作成するために必要なアセットを既に生成している必要があります。 これには以下が含まれます。

* 認証済みのベース接続
* ソース接続
* ターゲット XDM スキーマ
* ターゲットデータセット
* ターゲット接続
* マッピング

これらの値がまだない場合は、 [ソースのカタログの概要](../../home.md). 次に、指定されたソースの指示に従って、データフローのドラフティングに必要なアセットを生成します。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../landing/api-guide.md)のガイドを参照してください。

## データフローをドラフト状態に設定

次の節では、データフローをドラフトとして設定し、データフローを更新し、データフローを公開し、最終的にデータフローを削除するプロセスについて説明します。

### データフローのドラフト作成

データフローをドラフトとして設定するには、にPOSTリクエストを実行します。 `/flows` 追加中のエンドポイント `mode=draft` をクエリパラメーターとして設定します。 これにより、データフローを作成してドラフトとして保存できます。

**API 形式**

```http
POST /flows?mode=draft
```

| パラメーター | 説明 |
| --- | --- |
| `mode` | データフローの状態を決定する、ユーザーが指定したクエリパラメーター。 データフローをドラフトとして設定するには、 `mode` から `draft`. |

**リクエスト**

次のリクエストは、ドラフトデータフローを作成します。

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

成功すると、 `id` そして対応する `etag` データフローの

```json
{
  "id": "c9751426-dff8-49b0-965f-50defcf4187b",
  "etag": "\"69057131-0000-0200-0000-640f48320000\""
}
```

### データフローの更新

下書きを更新するには、 `/flows` エンドポイントを使用して、更新するデータフローの ID を指定します。 この手順の間、 `If-Match` ヘッダーパラメーター。 `etag` 更新するデータフローの情報です。

**API 形式**

```http
PATCH /flows/{FLOW_ID}
```

**リクエスト**

次のリクエストでは、ドラフトのデータフローにマッピング変換を追加します。

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

リクエストが成功した場合は、フロー ID と `etag`. 変更を検証するには、 `/flows` エンドポイントを使用してフロー ID を指定します。

```json
{
  "id": "c9751426-dff8-49b0-965f-50defcf4187b",
  "etag": "\"69057131-0000-0200-0000-640f48320000\""
}
```

### データフローの公開

下書きを公開する準備が整ったら、にPOSTリクエストを送信します。 `/flows` エンドポイントを使用して、公開するドラフトデータフローの ID と、公開用のアクション操作を指定します。

**API 形式**

```http
POST /flows/{FLOW_ID}/action?op=publish
```

| パラメーター | 説明 |
| --- | --- |
| `op` | クエリされたデータフローの状態を更新するアクション操作。 ドラフトデータフローを公開するには、 `op` から `publish`. |

**リクエスト**

次のリクエストは、ドラフトデータフローをパブリッシュします。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows/c9751426-dff8-49b0-965f-50defcf4187b/action?op=publish' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、ID と、 `etag` データフローの

```json
{
  "id": "c9751426-dff8-49b0-965f-50defcf4187b",
  "etag": "\"69057131-0000-0200-0000-640f48320000\""
}
```

### データフローの削除

データフローを削除するには、にDELETEリクエストを実行します。 `/flows` エンドポイントを使用して、削除するデータフローの ID を指定します。 フローサービス API を使用してデータフローを削除する手順の詳細については、 [API でのデータフローの削除](./delete-dataflows.md).