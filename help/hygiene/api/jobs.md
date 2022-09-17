---
title: Data Hygiene API を使用して消費者レコードを削除
description: Adobe Experience Platform で顧客の保存した個人データをプログラムで修正または削除する方法を説明します。
hide: true
hidefromtoc: true
exl-id: d80a4be3-e072-4bb4-a56d-b34a20f88c78
source-git-commit: c0d51d33d1e9d49d43f732925f2a794b5afea03b
workflow-type: tm+mt
source-wordcount: '505'
ht-degree: 100%

---

# Data Hygiene API を使用して消費者レコードを削除

>[!IMPORTANT]
>
>Data Hygiene API は現在ベータ版です。このドキュメントで説明されている機能は変更されることがあります。

Data Hygiene API を使用すると、Adobe Experience Platform に保存されている顧客の個人データをプログラムで修正または削除できます。

API には、[Privacy Service API](../../privacy-service/api/overview.md) と同じルートパスを通してアクセスすることができます：`https://platform.adobe.io/data/core/privacy/`

## はじめに

この節では、データ衛生 API を呼び出す前に知っておく必要がある基本概念の概要を説明します。

### 必須ヘッダーの値の収集

データ衛生 API を呼び出すには、まず認証資格情報を収集する必要があります。これらは、Privacy Service API へのアクセスに使用される資格情報と同じです。以下に示すように、[API の概要](./overview.md#getting-started)を参照して、Data Hygiene API に必要な各ヘッダーの値を生成します。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、次のような追加ヘッダーが必要です。

* `Content-Type: application/json`

### API 呼び出し例の読み取り

このドキュメントでは、リクエストの形式を示すために、API 呼び出しの例を提供しています。サンプル API 呼び出しのドキュメントで使用されている規則については、Experience Platform API の開始ガイドの[API 呼び出し例の読み方](../../landing/api-guide.md#sample-api)に関する節を参照してください。

## 削除ジョブの作成

POST リクエストをおこなうことで、削除ジョブを作成できます。

**API 形式**

```http
POST /jobs
```

**リクエスト**

リクエストペイロードは、](../../privacy-service/api/privacy-jobs.md#access-delete)Privacy Service API の削除リクエスト[と同様の構造になっています。これには、データが削除されるユーザーをオブジェクトが表す `users` 配列が含まれています。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/privacy/jobs \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -d '{
        "companyContexts": [
          {
            "namespace": "imsOrgID",
            "value": "{ORG_ID}"
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
| `companyContexts` | 組織の認証情報を含む配列。次のプロパティのオブジェクトを 1 つ含める必要があります。 <ul><li>`namespace`：`imsOrgID` に設定する必要があります。</li><li>`value`：IMS 組織 ID。これは、`x-gw-ims-org-id` ヘッダーで提供される値と同じです。</li></ul> |
| `users` | 情報を削除する 1 人以上のユーザーのコレクションを含む配列。各ユーザーオブジェクトには、次の情報が含まれます。 <ul><li>`key`：応答データ内の個別のジョブ ID を修飾するために使用されるユーザーの識別子。後で参照または検索できるように、この値には一意で簡単に識別できる文字列を選択することをお勧めします。</li><li>`action`：ユーザーのデータに対して実行する必要のあるアクションをリストする配列。単一の文字列値「`delete`」を含む必要があります。</li><li>`userIDs`：ユーザーの ID のコレクションです。1 人のユーザーが持つことのできる ID の数は 9 個に制限されます。各 ID には、次のプロパティが含まれます。 <ul><li>`namespace`：ID に関連付けられた [ID 名前空間](../../identity-service/namespaces.md)。これは、Platform で認識される[標準の名前空間](../../privacy-service/api/appendix.md#standard-namespaces)にすることも、組織で定義されるカスタム名前空間にすることもできます。使用する名前空間のタイプは、`type` プロパティに反映する必要があります。</li><li>`value`：ID 値。</li><li>`type`：グローバルに認識された名前空間を使用している場合は `standard`、組織で定義されている名前空間を使用している場合は `custom` に設定する必要があります。</li></ul></li></ul> |

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
