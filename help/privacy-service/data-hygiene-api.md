---
title: データ衛生 API（アルファ）
description: Adobe Experience Platformで顧客の保存した個人データをプログラムで修正または削除する方法を説明します。
hide: true
hidefromtoc: true
source-git-commit: dd8978566730975f0bde36f3af490cd33362b3ba
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 18%

---

# データ衛生 API（アルファ）

>[!IMPORTANT]
>
>データ衛生 API は現在アルファ状態で、お客様の組織がまだアクセスできない可能性があります。 このドキュメントで説明する機能は変更される場合があります。

データの衛生管理 API を使用すると、Adobe Experience Platformに保存された顧客の個人データをプログラムで修正または削除できます。 Privacy ServiceAPI とは異なり、これらの操作は法的なプライバシー規制に関連付ける必要はなく、純粋にデータをクリーンで正確に保つために使用できます。

## はじめに

この節では、データ衛生 API を呼び出す前に知っておく必要がある中心概念の概要を説明します。

### 必須ヘッダーの値の収集

データ衛生 API への呼び出しをおこなうには、まず認証資格情報を収集する必要があります。 これらは、Privacy ServiceAPI へのアクセスに使用される資格情報と同じです。 Privacy ServiceAPI の [ 開始ガイド ](./api/getting-started.md) に従って、次に示すように、データ衛生 API に必要な各ヘッダーの値を生成します。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

### API 呼び出し例の読み取り

このドキュメントでは、API 呼び出しの例を示し、リクエストの形式を設定する方法を示します。 ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience PlatformAPI の入門ガイドの [API 呼び出し例 ](../landing/api-guide.md#sample-api) の読み方に関する節を参照してください。

## 削除ジョブの作成

削除ジョブを作成するには、ジョブリクエストをPOSTします。

**API 形式**

```http
POST /jobs
```

**リクエスト**

リクエストペイロードの構造は、Privacy ServiceAPI](./api/privacy-jobs.md#access-delete) の [ 削除リクエストの構造と同じです。 この配列には、削除するデータを持つユーザーを表すオブジェクトを持つ `users` 配列が含まれます。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/hygiene/jobs \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'Content-Type: application/json' \
  -d '{
        "companyContexts": [
          {
            "namespace": "imsOrgID",
            "value": "{IMS_ORG}"
          }
        ],
        "users": [
          {
            "key": "John Doe",
            "action": [
              "delete"
            ],
            "userIDs": [
              {
                "namespace": "email",
                "value": "johnd@example.com",
                "type": "standard",
              },
              {
                "namespace": "ECID",
                "value": "9cbefef1-dd44-4411-87db-2d387bf882bc",
                "type": "standard"
              }
            ]
          },
          {
            "key": "Jane Doe",
            "action": [
              "delete"
            ],
            "userIDs": [
              {
                "namespace": "Loyalty ID",
                "value": "30583967185734",
                "type": "custom"
              }
            ]
          }
        ]
      }'
```

| プロパティ | 説明 |
| --- | --- |
| `companyContexts` | 組織の認証情報を含む配列。次のプロパティを持つ 1 つのオブジェクトを含める必要があります。 <ul><li>`namespace`:に設定する必要がありま `imsOrgID`す。</li><li>`value`:お使いの IMS Org ID。これは `x-gw-ims-org-id` ヘッダーで指定された値と同じです。</li></ul> |
| `users` | 削除する情報を持つユーザーが少なくとも 1 人コレクションを含む配列。 各ユーザーオブジェクトには、次の情報が含まれます。 <ul><li>`key`：応答データ内の個別のジョブ ID を修飾するために使用されるユーザーの識別子。この値に対して一意で、容易に識別できる文字列を選択し、後で参照または参照できるようにすることをお勧めします。</li><li>`action`：ユーザーのデータに対して実行する必要のあるアクションをリストする配列。次の 1 つの文字列値を含む必要があります。`delete`.</li><li>`userIDs`：ユーザーの ID のコレクションです。1 人のユーザーが持つことのできる ID の数は 9 個に制限されます。各 ID には、次のプロパティが含まれます。 <ul><li>`namespace`:ID に関 [連](../identity-service/namespaces.md) 付けられた ID 名前空間。これは、Platform で認識される [ 標準の名前空間 ](./api/appendix.md#standard-namespaces) か、組織で定義されるカスタム名前空間にすることができます。 使用する名前空間のタイプは、`type` プロパティに反映する必要があります。</li><li>`value`:ID 値。</li><li>`type`:グローバルに認識される `standard` 名前空間を使用する場合、または組織で定 `custom` 義された名前空間を使用する場合は、をに設定する必要があります。</li></ul></li></ul> |

{style=&quot;table-layout:auto&quot;}

**応答**

正常な応答は、作成されたジョブの詳細を返します。

```json
{
  "requestId": "16318094870430026RX-334",
  "totalRecords": 2,
  "jobs": [
    {
      "jobId": "c9b5fd82-db14-4c27-8bec-64a06e1fbda4",
      "customer": {
        "user": {
          "key": "John Doe",
          "action": ["delete"],
          "userIDs": [
            {
              "namespace": "email",
              "value": "johnd@example.com",
              "type": "standard",
              "namespaceId": 6,
              "isDeletedClientSide": false
            },
            {
              "namespace": "ECID",
              "value": "9cbefef1-dd44-4411-87db-2d387bf882bc",
              "type": "standard",
              "namespaceId": 4,
              "isDeletedClientSide": false
            }
          ]
        }
      }
    },
    {
      "jobId": "8ddc8e73-cecc-4be3-ae44-cdba127f7c70",
      "customer": {
        "user": {
          "key": "Jane Doe",
          "action": ["delete"],
          "userIDs": [
            {
              "namespace": "Loyalty ID",
              "value": "30583967185734",
              "type": "custom",
              "isDeletedClientSide": false
            }
          ]
        }
      }
    }
  ]
}
```
