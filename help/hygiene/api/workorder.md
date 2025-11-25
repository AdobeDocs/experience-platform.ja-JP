---
title: 削除作業指示のレコード
description: Data Hygiene API の/workorder エンドポイントを使用して、Adobe Experience Platformでレコード削除作業指示を管理する方法を説明します。 このガイドでは、割り当て量、処理タイムライン、API の使用状況について説明します。
role: Developer
exl-id: f6d9c21e-ca8a-4777-9e5f-f4b2314305bf
source-git-commit: f1f37439bd4d77faf1015741e604eee7188c58d7
workflow-type: tm+mt
source-wordcount: '2440'
ht-degree: 3%

---

# 削除作業指示のレコード {#work-order-endpoint}

Data Hygiene API の `/workorder` エンドポイントを使用して、Adobe Experience Platformでのレコード削除作業指示の作成、表示および管理をおこないます。 作業指示を使用すると、データセット間でデータの削除を制御、監視および追跡して、データ品質を維持し、組織のデータガバナンス標準をサポートできます。

>[!IMPORTANT]
>
>レコード削除作業指示は、データクレンジング、匿名データの削除またはデータの最小化を目的としています。 **GDPR などのプライバシー規制の下で、データ主体の権利リクエストに対してレコード削除作業指示を使用しないでください。** コンプライアンスのユースケースについては、[Adobe Experience Platform Privacy Service](../../privacy-service/home.md) を使用してください。

## はじめに

開始する前に、[&#x200B; 概要 &#x200B;](./overview.md) を参照して、必要なヘッダー、サンプル API 呼び出しの読み方および関連ドキュメントの場所を確認してください。

## 割り当て量と処理タイムライン {#quotas}

レコードの削除作業指示は、組織のライセンス使用権限によって決定され、1 日ごとおよび 1 か月ごとの識別子の送信制限の対象となります。 これらの制限は、UI ベースと API ベースのレコード削除リクエストの両方に適用されます。

>[!NOTE]
>
>1 日に最大 **1,000,000 個の識別子を送信できますが** その権限は残りの月別クォータで与えられている場合に限られます。 1 か月の上限が 100 万未満の場合、毎日の送信がその上限を超えることはできません。

### 製品別の月間送信使用権限 {#quota-limits}

次の表に、製品および資格レベル別の識別子の送信制限を示します。 各製品の月額上限は、固定の識別子上限またはライセンス取得済みデータボリュームに関連付けられた割合ベースのしきい値の、2 つの値のいずれか小さい方です。

| 製品 | 使用権限の説明 | 月間キャップ （いずれか小さい方） |
|----------|-------------------------|---------------------------------|
| Real-Time CDPまたはAdobe Journey Optimizer | プライバシーとセキュリティシールドまたは Healthcare Shield アドオンなし | 2,000,000 個の識別子（アドレス可能なオーディエンスの 5%） |
| Real-Time CDPまたはAdobe Journey Optimizer | Privacy and Security Shield または Healthcare Shield アドオンを使用 | 15,000,000 個の識別子（アドレス可能なオーディエンスの 10%） |
| Customer Journey Analytics | プライバシーとセキュリティシールドまたは Healthcare Shield アドオンなし | CJAの権利行あたり 2,000,000 の識別子または 1,000 の識別子 |
| Customer Journey Analytics | Privacy and Security Shield または Healthcare Shield アドオンを使用 | CJAの権利行あたり 15,000,000 個の識別子または 2,000 個の識別子 |

>[!NOTE]
>
>ほとんどの組織では、実際のアドレス可能なオーディエンスまたはCJA行の使用権限に基づいて、月間の上限が引き下げられます。

>[!NOTE]
>
>クォータは、毎月 1 日にリセットされます。 未使用の割り当ては引き継がれ **い**。

>[!NOTE]
>
>クォータの使用状況は、**送信された識別子** に対して組織でライセンスを取得した 1 か月の使用権限に基づきます。 クォータはシステム・ガードレールによって適用されませんが、監視および確認が可能です。\
>レコード削除作業指示能力は **共有サービス** です。 1 か月の上限には、Real-Time CDP、Adobe Journey Optimizer、Customer Journey Analyticsおよび該当する Shield アドオン全体で最高の使用権限が反映されます。

### 識別子の送信のタイムラインの処理 {#sla-processing-timelines}

送信後、レコードの削除作業指示は、使用権限レベルに基づいてキューに入れられ、処理されます。

| 製品と使用権限の説明 | キューの期間 | 最大処理時間（SLA） |
|------------------------------------|---------------------|-------------------------------|
| プライバシーとセキュリティシールドまたは Healthcare Shield アドオンなし | 最長 15 日間 | 30 日 |
| Privacy and Security Shield または Healthcare Shield アドオンを使用 | 通常 24 時間 | 15 日 |

組織でさらに上限が必要な場合は、Adobe担当者に連絡して使用権限のレビューを依頼してください。

>[!TIP]
>
>現在のクォータの使用状況または使用権限層を確認するには、[&#x200B; クォータのリファレンス ガイド &#x200B;](../api/quota.md) を参照してください。

## レコード削除作業指示のリスト {#list}

組織のデータハイジーン操作用の、ページ分割されたレコード削除作業指示のリストを取得します。 クエリパラメーターを使用して結果をフィルタリングします。 各作業指示レコードには、アクションタイプ（`identity-delete` など）、ステータス、関連するデータセットおよびユーザーの詳細、監査メタデータが含まれています。

**API 形式**

```http
GET /workorder
```

次の表では、レコード削除作業指示のリストに使用できる問合せパラメータを説明します。

| クエリパラメーター | 説明 |
| --------------- | ------------|
| `search` | フィールド（author、displayName、description、datasetName）間で、大文字と小文字を区別しない部分一致（ワイルドカード検索）。 また、正確な有効期限 ID にも一致します。 |
| `type` | 作業指示タイプ（`identity-delete` など）で結果をフィルタリングします。 |
| `status` | 作業指示ステータスのコンマ区切りリスト。 ステータス値は、大文字と小文字を区別します。<br> 列挙：`received`、`validated`、`submitted`、`ingested`、`completed`、`failed` |
| `author` | 作業指示（または元の作成者）を最後に更新した人物を検索します。 リテラルまたは SQL パターンを使用できます。 |
| `displayName` | 作業指示の表示名で大文字と小文字が区別されない一致。 |
| `description` | 作業指示の説明で大文字と小文字が区別されない一致。 |
| `workorderId` | 作業指示 ID の完全一致。 |
| `sandboxName` | リクエストで使用されているサンドボックス名と完全に一致するか、`*` を使用してすべてのサンドボックスを含めます。 |
| `fromDate` | この日付以降に作成された作業指示でフィルタリングします。 `toDate` が設定されている必要があります。 |
| `toDate` | この日付以前に作成された作業指示でフィルタリングします。 `fromDate` が設定されている必要があります。 |
| `filterDate` | この日付に作成、更新、または変更された作業指示ステータスのみを返します。 |
| `page` | 返すページインデックス （0 から始まります）。 |
| `limit` | 1 ページあたりの最大結果数（1 ～ 100、デフォルト：25）。 |
| `orderBy` | 結果の並べ替え順。 昇順/降順には `+` または `-` のプレフィックスを使用します。 例：`orderBy=-datasetName`。 |
| `properties` | 結果ごとに含める追加フィールドのコンマ区切りリスト。 （オプション） |


**リクエスト**

次のリクエストは、すべての完了済みレコード削除作業指示を取得します（ページあたり 2 つに制限されています）。

```shell
curl -X GET \
  "https://platform.adobe.io/data/core/hygiene/workorder?status=completed&limit=2" \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、レコード削除作業指示のページ分割されたリストが返されます。

```json
{
  "results": [
    {
      "workorderId": "DI-1729d091-b08b-47f4-923f-6a4af52c93ac",
      "orgId": "9C1F2AC143214567890ABCDE@AcmeOrg",
      "bundleId": "BN-4cfabf02-c22a-45ef-b21f-bd8c3d631f41",
      "action": "identity-delete",
      "createdAt": "2034-03-15T11:02:10.935Z",
      "updatedAt": "2034-03-15T11:10:10.938Z",
      "operationCount": 3,
      "targetServices": [
        "profile",
        "datalake",
        "identity"
      ],
      "status": "received",
      "createdBy": "a.stark@acme.com <a.stark@acme.com> BD8C3D631F41@acme.com",
      "datasetId": "a7b7c8f3a1b8457eaa5321ab",
      "datasetName": "Acme_Customer_Exports",
      "displayName": "Customer Identity Delete Request",
      "description": "Scheduled identity deletion for compliance"
    }
  ],
  "total": 1,
  "count": 1,
  "_links": {
    "next": {
      "href": "https://platform.adobe.io/workorder?page=1&limit=2",
      "templated": false
    },
    "page": {
      "href": "https://platform.adobe.io/workorder?limit={limit}&page={page}",
      "templated": true
    }
  }
}
```

次の表に、応答のプロパティを示します。

| プロパティ | 説明 |
| --- | --- |
| `results` | レコードの配列で、作業指示オブジェクトを削除します。 各オブジェクトには、以下のフィールドが含まれます。 |
| `workorderId` | レコード削除作業指示の一意の ID。 |
| `orgId` | 一意の組織 ID。 |
| `bundleId` | このレコード削除作業指示を含むバンドルの一意の ID。 バンドルを使用すると、複数の削除指示をダウンストリームサービスでグループ化して処理できます。 |
| `action` | 作業指示でリクエストされたアクションタイプ。 |
| `createdAt` | 作業指示が作成されたときのタイムスタンプ。 |
| `updatedAt` | 作業指示が最後に更新されたときのタイムスタンプ。 |
| `operationCount` | 作業指示に含まれる操作の数。 |
| `targetServices` | 作業指示のターゲットサービスのリスト。 |
| `status` | 作業指示の現在のステータス。 使用可能な値：`received`、`validated`、`submitted`、`ingested`、`completed`、`failed`。 |
| `createdBy` | 作業指示を作成したユーザーのメールアドレスおよび識別子。 |
| `datasetId` | 作業指示に関連付けられたデータセットの一意の ID。 リクエストがすべてのデータセットに適用される場合、このフィールドは「すべて」に設定されます。 |
| `datasetName` | 作業指示に関連付けられたデータセットの名前。 |
| `displayName` | 作業指示の人間が読み取れるラベル。 |
| `description` | 作業指示の目的の説明。 |
| `total` | クエリに一致するレコード削除作業指示の合計数。 |
| `count` | 現在のページのレコード削除作業指示の数。 |
| `_links` | ページネーションとナビゲーションリンク。 |
| `next` | 次のページ用の `href` （文字列）と `templated` （ブール値）を持つオブジェクト。 |
| `page` | ページナビゲーション用の `href` （文字列）および `templated` （ブール値）を持つオブジェクト。 |

{style="table-layout:auto"}

## レコード削除作業指示の作成 {#create}

単一のデータセットまたはすべてのデータセットから 1 つ以上の ID に関連付けられたレコードを削除するには、`/workorder` エンドポイントに対して POST リクエストを実行します。

作業指示は非同期で処理され、送信後に作業指示リストに表示されます。

>[!TIP]
>
>API を通じて送信された各レコード削除作業指示には、最大 100,000 個の ID **含めることができ** す。 効率を最大化するために、リクエストのできるだけ多くの ID を送信します。 単一 ID の作業指示など、少量の送信を避けます。

**API 形式**

```http
POST /workorder
```

>[!NOTE]
>
>削除できるのは、関連付けられた XDM スキーマがプライマリ ID または ID マップを定義するデータセットからのレコードのみです。

>[!NOTE]
>
>既にアクティブな有効期限があるデータセットに対してレコード削除作業指示を作成しようとすると、リクエストから HTTP 400 （無効なリクエスト）が返されます。アクティブな有効期限は、まだ完了していないスケジュール済みの削除です。

**リクエスト**

次のリクエストは、指定されたメールアドレスに関連付けられているすべてのレコードを、特定のデータセットから削除します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/hygiene/workorder \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'Content-Type: application/json' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "displayName": "Acme Loyalty - Customer Data Deletion",
        "description": "Delete all records associated with the specified email addresses from the Acme_Loyalty_2023 dataset.",
        "action": "delete_identity",
        "datasetId": "7eab61f3e5c34810a49a1ab3",
        "namespacesIdentities": [
          {
            "namespace": {
              "code": "email"
            },
            "IDs": [
              "alice.smith@acmecorp.com",
              "bob.jones@acmecorp.com",
              "charlie.brown@acmecorp.com"
            ]
          }
        ]
      }'
```

次の表では、レコード削除作業指示を作成するためのプロパティについて説明します。

| プロパティ | 説明 |
| --- | --- |
| `displayName` | このレコード削除作業指示の人間が読み取れるラベル。 |
| `description` | レコード削除作業指示の説明。 |
| `action` | レコード削除作業指示に対してリクエストされたアクション。 特定の ID に関連付けられているレコードを削除するには、`delete_identity` を使用します。 |
| `datasetId` | データセットの一意の ID。 特定のデータセットのデータセット ID を使用するか、`ALL` を使用してすべてのデータセットをターゲットにします。 データセットには、プライマリ ID または ID マップが必要です。 ID マップが存在する場合、`identityMap` という名前の最上位フィールドとして存在します。<br> データセット行の ID マップに多くの ID が含まれている場合がありますが、プライマリとしてマークできるのは 1 つだけであることに注意してください。 `"primary": true` がプライマリ ID と一致するように強制するには、`id` を含める必要があります。 |
| `namespacesIdentities` | オブジェクトの配列。各オブジェクトには、以下が含まれます。<br><ul><li> `namespace`:ID 名前空間を指定する `code` プロパティを持つオブジェクト（例：「email」）。</li><li> `IDs`：この名前空間で削除する ID 値の配列。</li></ul>ID 名前空間は、ID データに対するコンテキストを提供します。 Experience Platformが提供する標準の名前空間を使用するか、独自の名前空間を作成できます。 詳しくは、[ID 名前空間ドキュメント &#x200B;](../../identity-service/features/namespaces.md) および [ID サービス API 仕様 &#x200B;](https://developer.adobe.com/experience-platform-apis/references/identity-service/#operation/getIdNamespaces) を参照してください。 |

**応答**

応答が成功すると、新しいレコードの削除作業指示の詳細が返されます。

```json
{
  "workorderId": "DI-95c40d52-6229-44e8-881b-fc7f072de63d",
  "orgId": "8B1F2AC143214567890ABCDE@AcmeOrg",
  "bundleId": "BN-c61bec61-5ce8-498f-a538-fb84b094adc6",
  "action": "identity-delete",
  "createdAt": "2035-06-02T09:21:00.000Z",
  "updatedAt": "2035-06-02T09:21:05.000Z",
  "operationCount": 1,
  "targetServices": [
    "profile",
    "datalake",
    "identity"
  ],
  "status": "received",
  "createdBy": "c.lannister@acme.com <c.lannister@acme.com> 7EAB61F3E5C34810A49A1AB3@acme.com",
  "datasetId": "7eab61f3e5c34810a49a1ab3",
  "datasetName": "Acme_Loyalty_2023",
  "displayName": "Loyalty Identity Delete Request",
  "description": "Schedule deletion for Acme loyalty program dataset"
}
```

次の表に、応答のプロパティを示します。

| プロパティ | 説明 |
| --- | --- |
| `workorderId` | レコード削除作業指示の一意の ID。 この値を使用して、削除のステータスまたは詳細を検索します。 |
| `orgId` | 一意の組織 ID。 |
| `bundleId` | このレコード削除作業指示を含むバンドルの一意の ID。 バンドルを使用すると、複数の削除指示をダウンストリームサービスでグループ化して処理できます。 |
| `action` | レコード削除作業指示でリクエストされたアクションタイプ。 |
| `createdAt` | 作業指示が作成されたときのタイムスタンプ。 |
| `updatedAt` | 作業指示が最後に更新されたときのタイムスタンプ。 |
| `operationCount` | 作業指示に含まれる操作の数。 |
| `targetServices` | レコード削除作業指示のターゲットサービスのリスト。 |
| `status` | レコード削除作業指示の現在のステータス。 |
| `createdBy` | レコード削除作業指示を作成したユーザーのメールアドレスおよび識別子。 |
| `datasetId` | データセットの一意の ID。 すべてのデータセットに対するリクエストの場合、値は `ALL` に設定されます。 |
| `datasetName` | このレコード削除作業指示のデータセットの名前。 |
| `displayName` | レコード削除作業指示の人間が読み取れるラベル。 |
| `description` | レコード削除作業指示の説明。 |

{style="table-layout:auto"}

>[!NOTE]
>
>レコード削除作業指示のアクションプロパティは、現在、API 応答で `identity-delete` 定されています。 API が別の値（`delete_identity` など）を使用するように変更された場合、それに応じてこのドキュメントが更新されます。

## レコード削除リクエスト用に ID リストを JSON に変換

識別子を含む CSV、TSV、TXT ファイルからレコード削除作業指示を作成するには、変換スクリプトを使用して、`/workorder` エンドポイントに必要な JSON ペイロードを生成します。 この方法は、既存のデータファイルを使用する場合に特に便利です。 すぐに使用できるスクリプトと包括的な手順については、[csv-to-data-hygiene GitHub リポジトリ &#x200B;](https://github.com/perlmonger42/csv-to-data-hygiene) を参照してください。

### JSON ペイロードの生成

次の bash スクリプトの例は、Python または Ruby で変換スクリプトを実行する方法を示しています。

>[!BEGINTABS]

>[!TAB Python スクリプトの実行例 ]

```bash
#!/usr/bin/env bash

rm -rf ./output && mkdir output
for NAME in UTF8 CSV TSV TXT XYZ big; do
  ./csv-to-DI-payload.py sample/sample-$NAME.* \
      --verbose \
      --column 2 \
      --namespace email \
      --dataset-id 66f4161cc19b0f2aef3edf10 \
      --description 'a simple sample' \
      --output-dir output
  echo Checking output/sample-$NAME-*.json against expect/sample-$NAME-*.json
  diff <(cat output/sample-$NAME-*.json) <(cat expect/sample-$NAME-*.json) || echo Unexpected output in sample-$NAME-*.*
done
```

>[!TAB Ruby スクリプトの実行例 ]

```bash
#!/usr/bin/env bash

rm -rf ./output && mkdir output
for NAME in UTF8 CSV TSV TXT XYZ big; do
  ./csv-to-DI-payload.rb sample/sample-$NAME.* \
      --verbose \
      --column 2 \
      --namespace email \
      --dataset-id 66f4161cc19b0f2aef3edf10 \
      --description 'a simple sample' \
      --output-dir output
  echo Checking output/sample-$NAME-*.json against expect/sample-$NAME-*.json
  diff <(cat output/sample-$NAME-*.json) <(cat expect/sample-$NAME-*.json) || echo Unexpected output in sample-$NAME-*.*
done
```

>[!ENDTABS]

次の表に bash スクリプトのパラメーターを示します。

| パラメーター | 説明 |
| ---           | ---     |
| `verbose` | 詳細出力を有効にします。 |
| `column` | 削除する ID 値を含む列のインデックス（1 から始まる）またはヘッダー名。 指定しない場合は、デフォルトで最初の列に設定されます。 |
| `namespace` | ID 名前空間を指定する `code` プロパティを持つオブジェクト（例えば「email」）。 |
| `dataset-id` | 作業指示に関連付けられたデータセットの一意の ID。 リクエストがすべてのデータセットに適用される場合、このフィールドは `ALL` に設定されます。 |
| `description` | レコード削除作業指示の説明。 |
| `output-dir` | 出力 JSON ペイロードを書き込むディレクトリ。 |

{style="table-layout:auto"}

以下の例に、CSV、TSV、TXT ファイルから変換された成功した JSON ペイロードを示します。 指定した名前空間に関連付けられたレコードが含まれており、メールアドレスで識別されたレコードを削除するために使用されます。

```json
{
  "action": "delete_identity",
  "datasetId": "66f4161cc19b0f2aef3edf10",
  "displayName": "output/sample-big-001.json",
  "description": "a simple sample",
  "identities": [
    {
      "namespace": {
        "code": "email"
      },
      "id": "1"
    },
    {
      "namespace": {
        "code": "email"
      },
      "id": "2"
    }
  ]
}
```

次の表に、JSON ペイロードのプロパティを示します。

| プロパティ | 説明 |
| ---          | ---     |
| `action` | レコード削除作業指示に対してリクエストされたアクション。 変換スクリプトによって自動的に `delete_identity` に設定されます。 |
| `datasetId` | データセットの一意の ID。 |
| `displayName` | このレコード削除作業指示の人間が読み取れるラベル。 |
| `description` | レコード削除作業指示の説明。 |
| `identities` | オブジェクトの配列。各オブジェクトには、以下が含まれます。<br><ul><li> `namespace`: ID 名前空間を指定する `code` プロパティを持つオブジェクト（例えば「email」）。</li><li> `id`：この名前空間で削除する ID 値。</li></ul> |

{style="table-layout:auto"}

### 生成された JSON データを `/workorder` エンドポイントに送信します。

リクエストを送信するには、「レコードの削除作業指示の作成 [&#x200B; の節の手順に従 &#x200B;](#create) ます。 `-d` POST リクエストを `curl` API エンドポイントに送信する場合は、変換された JSON ペイロードをリクエスト本文（`/workorder`）として使用します。

## 特定のレコード削除作業指示の詳細の取得 {#lookup}

`/workorder/{WORKORDER_ID}` に対してGET リクエストを実行して、特定のレコード削除作業指示の情報を取得します。 応答には、アクションタイプ、ステータス、関連するデータセットおよびユーザー情報、監査メタデータが含まれます。

**API 形式**

```http
GET /workorder/{WORKORDER_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{WORK_ORDER_ID}` | 参照しているレコード削除作業指示の一意の ID。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X GET \
  https://platform.adobe.io/data/core/hygiene/workorder/DI-6fa98d52-7bd2-42a5-bf61-fb5c22ec9427 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、指定されたレコード削除作業指示の詳細が返されます。

```json
{
  "workorderId": "DI-6fa98d52-7bd2-42a5-bf61-fb5c22ec9427",
  "orgId": "3C7F2AC143214567890ABCDE@AcmeOrg",
  "bundleId": "BN-dbe3ffad-cb0b-401f-91ae-01c189f8e7b2",
  "action": "identity-delete",
  "createdAt": "2037-01-21T08:25:45.119Z",
  "updatedAt": "2037-01-21T08:30:45.233Z",
  "operationCount": 3,
  "targetServices": [
    "ajo",
    "profile",
    "datalake",
    "identity"
  ],
  "status": "received",
  "createdBy": "g.baratheon@acme.com <g.baratheon@acme.com> C189F8E7B2@acme.com",
  "datasetId": "d2f1c8a4b8f747d0ba3521e2",
  "datasetName": "Acme_Marketing_Events",
  "displayName": "Marketing Identity Delete Request",
  "description": "Scheduled identity deletion for marketing compliance"
}
```

次の表に、応答のプロパティを示します。

| プロパティ | 説明 |
| --- | --- |
| `workorderId` | レコード削除作業指示の一意の ID。 |
| `orgId` | 組織の一意の ID。 |
| `bundleId` | このレコード削除作業指示を含むバンドルの一意の ID。 バンドルを使用すると、複数の削除指示をダウンストリームサービスでグループ化して処理できます。 |
| `action` | レコード削除作業指示でリクエストされたアクションタイプ。 |
| `createdAt` | 作業指示が作成されたときのタイムスタンプ。 |
| `updatedAt` | 作業指示が最後に更新されたときのタイムスタンプ。 |
| `operationCount` | 作業指示に含まれる操作の数。 |
| `targetServices` | このレコード削除作業指示の影響を受けるターゲットサービスのリスト。 |
| `status` | レコード削除作業指示の現在のステータス。 |
| `createdBy` | レコード削除作業指示を作成したユーザーのメールアドレスおよび識別子。 |
| `datasetId` | 作業指示に関連付けられたデータセットの一意の ID。 |
| `datasetName` | 作業指示に関連付けられたデータセットの名前。 |
| `displayName` | レコード削除作業指示の人間が読み取れるラベル。 |
| `description` | レコード削除作業指示の説明。 |

## レコード削除作業指示の更新

`name` エンドポイントに対してPUT リクエストを実行して、レコード削除作業指示の `description` および `/workorder/{WORKORDER_ID}` を更新します。

**API 形式**

```http
PUT /workorder/{WORKORDER_ID}
```

次の表に、このリクエストのパラメーターを示します。

| パラメーター | 説明 |
| --- | --- |
| `{WORK_ORDER_ID}` | 更新するレコード削除作業指示の一意の ID。 |

{style="table-layout:auto"}

**リクエスト**

```shell
curl -X PUT \
  https://platform.adobe.io/data/core/hygiene/workorder/DI-893a6b1d-47c2-41e1-b3f1-2d7c2956aabb \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
        "name": "Updated Marketing Identity Delete Request",
        "description": "Updated deletion request for marketing data"
      }'
```

次の表に、更新可能なプロパティを示します。

| プロパティ | 説明 |
| --- | --- |
| `name` | レコード削除作業指示の更新された人間が読み取れるラベル。 |
| `description` | レコード削除作業指示の更新された説明。 |

{style="table-layout:auto"}

**応答**

応答が成功すると、更新された作業指示リクエストが返されます。

```json
{
  "workorderId": "DI-893a6b1d-47c2-41e1-b3f1-2d7c2956aabb",
  "orgId": "7D4E2AC143214567890ABCDE@AcmeOrg",
  "bundleId": "BN-12abcf45-32ea-45bc-9d1c-8e7b321cabc8",
  "action": "identity-delete",
  "createdAt": "2038-04-15T12:14:29.210Z",
  "updatedAt": "2038-04-15T12:30:29.442Z",
  "operationCount": 2,
  "targetServices": [
    "profile",
    "datalake"
  ],
  "status": "received",
  "createdBy": "b.tarth@acme.com <b.tarth@acme.com> 8E7B321CABC8@acme.com",
  "datasetId": "1a2b3c4d5e6f7890abcdef12",
  "datasetName": "Acme_Marketing_2024",
  "displayName": "Updated Marketing Identity Delete Request",
  "description": "Updated deletion request for marketing data",
  "productStatusDetails": [
        {
            "productName": "Data Management",
            "productStatus": "waiting",
            "createdAt": "2024-06-12T20:11:18.447747Z"
        },
        {
            "productName": "Identity Service",
            "productStatus": "success",
            "createdAt": "2024-06-12T20:36:09.020832Z"
        },
        {
            "productName": "Profile Service",
            "productStatus": "waiting",
            "createdAt": "2024-06-12T20:11:18.447747Z"
        },
        {
            "productName": "Journey Orchestrator",
            "productStatus": "success",
            "createdAt": "2024-06-12T20:12:19.843199Z"
        }
    ]
}
```

| プロパティ | 説明 |
| ---------------- | ----------------------------------------------------------------------------------------- |
| `workorderId` | レコード削除作業指示の一意の ID。 |
| `orgId` | 組織の一意の ID。 |
| `bundleId` | このレコード削除作業指示を含むバンドルの一意の ID。 バンドルを使用すると、複数の削除指示をダウンストリームサービスでグループ化して処理できます。 |
| `action` | レコード削除作業指示でリクエストされたアクションタイプ。 |
| `createdAt` | 作業指示が作成されたときのタイムスタンプ。 |
| `updatedAt` | 作業指示が最後に更新されたときのタイムスタンプ。 |
| `operationCount` | 作業指示に含まれる操作の数。 |
| `targetServices` | このレコード削除作業指示の影響を受けるターゲットサービスのリスト。 |
| `status` | レコード削除作業指示の現在のステータス。 使用可能な値：`received`、`validated`、`submitted`、`ingested`、`completed`、`failed`。 |
| `createdBy` | レコード削除作業指示を作成したユーザーのメールアドレスおよび識別子。 |
| `datasetId` | レコードの削除作業指示に関連付けられたデータセットの一意の ID。 |
| `datasetName` | レコードの削除作業指示に関連付けられたデータセットの名前。 |
| `displayName` | レコード削除作業指示の人間が読み取れるラベル。 |
| `description` | レコード削除作業指示の説明。 |
| `productStatusDetails` | リクエストのダウンストリームプロセスの現在のステータスをリストする配列。 各オブジェクトには、以下が含まれます。<ul><li>`productName`：ダウンストリームサービスの名前。</li><li>`productStatus`：ダウンストリームサービスの現在の処理ステータス。</li><li>`createdAt`：最新のステータスがサービスからポストされたときのタイムスタンプ。</li></ul>このプロパティは、作業指示がダウンストリームサービスに送信されて処理を開始した後に使用できます。 |

{style="table-layout:auto"}
