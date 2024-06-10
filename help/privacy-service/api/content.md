---
title: コンテンツ API エンドポイント
description: Privacy ServiceAPI を使用してアクセスデータを取得する方法について説明します。
role: Developer
badgePrivateBeta: label="プライベートベータ版" type="Informative"
exl-id: b3b7ea0f-957d-4e51-bf92-121e9ae795f5
source-git-commit: e3a453ad166fe244b82bd1f90e669579fcf09d17
workflow-type: tm+mt
source-wordcount: '696'
ht-degree: 6%

---

# コンテンツエンドポイント

>[!IMPORTANT]
>
>この `/content` エンドポイントは現在ベータ版です。お客様の組織はまだアクセスできない可能性があります。 機能とドキュメントは変更される場合があります。

の使用 `/content` 安全に取得するエンドポイント *アクセス情報* （プライバシー主体がアクセスを正当にリクエストできる情報）を顧客に提供します。 への応答で提供されたダウンロード URL `/jobs/{JOB_ID}` GETリクエストは、Adobe サービスエンドポイントを指します。 その後、に対してGETリクエストを実行できます。 `/jobs/:JOB_ID/content` をクリックして、顧客データを JSON 形式で返します。 このアクセス方法は、セキュリティを強化するために、認証とアクセス制御の複数の層を実装する。

このガイドを使用する前に、 [はじめる前に](./getting-started.md) 以下の API 呼び出しの例で示している必須の認証ヘッダーについて説明します。

>[!TIP]
>
>必要なアクセス情報のジョブ ID が不明な場合は、を呼び出します。 `/jobs` エンドポイントを設定し、追加のクエリパラメーターを使用して結果をフィルタリングします。 使用可能なクエリパラメーターの完全なリストは、 [プライバシージョブエンドポイントガイド](./privacy-jobs.md).

## プライバシージョブ情報の取得

特定のジョブに関する情報（現在の処理ステータスなど）を取得するには、そのジョブのを含めます `jobId` へのGETリクエストのパス内 `/jobs` エンドポイント。

**API 形式**

```http
GET /jobs/{JOB_ID}
```

**リクエスト**

次のリクエストは、リクエストパスで `jobId` が指定されたジョブの詳細を取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/privacy/jobs/dbe3a6a6-f8e6-11ee-a365-8d1d6df81cc5 \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}'
```

**応答** 

正常な応答は、指定されたジョブの詳細を返します。

>[!NOTE]
>
>プライバシージョブには、 `complete` を含めるためのステータス `downloadUrl`.

```json
{
    "jobId":"dbe3a6a6-f8e6-11ee-a365-8d1d6df81cc5",
    "requestId":"17129380910360540RX-753",
    "userKey":"1234",
    "action":"access",
    "status":"complete",
    "submittedBy":"jsnow@adobe.com",
    "createdDate":"04/12/2024 04:08 PM GMT",
    "lastModifiedDate":"04/12/2024 04:08 PM GMT",
    "userIds":[{
        "namespace":"ECID",
        "value":"1234",
        "type":"standard",
        "namespaceId":4,
        "isDeletedClientSide":false
        }],
    "productResponses":[{
        "product":"Identity",
        "retryCount":0,
        "processedDate":"04/12/2024 04:08 PM GMT",
        "productStatusResponse":{"status":"submitted"
        }}],
    "downloadUrl":"https://platform.adobe.io/data/core/privacy/jobs/dbe3a6a6-f8e6-11ee-a365-8d1d6df81cc5/content",
    "regulation":"gdpr"
}
```

| プロパティ | 説明 |
|----------------------|---------------------------------------------------------------------------------------------------------------|
| `jobId` | プライバシージョブの一意の ID。 |
| `requestId` | Privacy Serviceに対して行われた特定のリクエストの一意の ID。 |
| `userKey` | `userKey` が `key` プライバシーリクエストを送信した際に指定した値。 この `key` 値は、データ主体に対して、意味のある識別子を提供する機会です。 これは、通常、そのデータ主体を追跡するためにシステムが作成した一意の ID です。 ヒント：すべてのアクティブなプライバシージョブをリストし、比較できます `key` を各ジョブに追加します。 |
| `action` | リクエストされたアクションのタイプ。 指定できる値は次のとおりです `access` および `delete`. |
| `status` | プライバシージョブの現在のステータス。 |
| `submittedBy` | プライバシージョブを送信した人物のメールアドレス。 |
| `createdDate` | プライバシージョブが作成された日時。 |
| `lastModifiedDate` | プライバシージョブが最後に変更された日時。 |
| `userIds` | ユーザー識別子と関連情報を含む配列。 |
| `userIds.namespace` | ユーザー識別子に使用する名前空間。 |
| `userIds.value` | ユーザー識別子の実際の値。 |
| `userIds.type` | 識別子のタイプ（例： `standard` または `custom`）に設定します。 |
| `userIds.namespaceId` | ユーザー ID の分類および管理に使用される名前空間の識別子。 |
| `userIds.isDeletedClientSide` | 識別子がクライアント側で削除されたかどうかを示すブール値。 |
| `productResponses` | プライバシージョブに関連する様々な製品やサービスからの応答を含む配列。 |
| `productResponses.product` | データ主体に関する情報の取得に使用された製品またはサービスの名前。 |
| `productResponses.retryCount` | リクエストが再試行された回数。 |
| `productResponses.processedDate` | 商品の応答が処理された日時。 |
| `productResponses.productStatusResponse` | 製品応答のステータスを含むオブジェクト。 |
| `productResponses.productStatusResponse.status` | 製品応答のステータス。 |
| `downloadURL` | この属性はエンドポイントを提供します。このエンドポイントは、ジョブが完了した後 60 日間呼び出すことができます。 ジョブのステータスは、 `complete` および `action` 次でなければなりません： `access`. それ以外の場合、このフィールドは存在しません。 |
| `regulation` | プライバシーリクエストが処理される規制フレームワーク。例： `gdpr`, `ccpa`, `lgpd_bra`, `pdpa_tha`など。 |

{style="table-layout:auto"}

## 顧客アクセス情報の取得 {#retrieve-access-data}

GET主体のクエリに応じて生成された「アクセス情報」を取得するには、 `/jobs/{JOB_ID}/content` エンドポイント。 応答は、データ主体のデータを保持する各製品のサブフォルダーを含むフォルダーを含む zip ファイル（*.zip）です。

>[!TIP]
>
>このリクエストを行うには、特定のジョブ ID が必要です。 特定のジョブ ID を取得する必要がある場合、まず、以下に対してGETリクエストを実行します。 `/jobs` エンドポイントを設定し、追加のクエリパラメーターを使用して結果をフィルタリングします。 許可されているクエリパラメーターを含め、詳しくは [プライバシージョブエンドポイントガイド](./privacy-jobs.md).

**API 形式**

```http
GET /jobs/{JOB_ID}/content
```

**リクエスト**

次のリクエストは、リクエストで指定されたジョブ ID の「アクセス情報」を返します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/privacy/jobs/32d429b1-f7f4-11ee-a365-574bcf5a525d/content \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \ 
  -H 'Accept: application/json`
```

**応答**

応答は zip ファイル（*.zip）です。 情報は通常、JSON 形式で返されますが、保証はできません。 抽出されたデータは、任意の形式で返すことができます。

