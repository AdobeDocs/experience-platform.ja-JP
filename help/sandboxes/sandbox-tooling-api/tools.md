---
title: サンドボックスツールツール API エンドポイント
description: Sandbox Tooling API の/tools エンドポイントを使用すると、Adobe Experience Platformでジョブ JSON データを取得できます。
source-git-commit: e4e89c5250885bef177ba0d678629261a361a66d
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 3%

---

# ツールエンドポイント

サンドボックスツールを使用すると、様々なアーティファクトを選択して、パッケージに書き出すことができます。 パッケージは、1 つのオブジェクトまたは複数のオブジェクトで構成できます。 パッケージに含まれるオブジェクトは、同じサンドボックスからのものである必要があります。

The `/tools` サンドボックスツール API のエンドポイントを使用すると、ジョブの JSON データをリストし、取得できます。

## ジョブの詳細 {#details}

ジョブの JSON データを独立して取得するには、に対してGETリクエストを実行します。 `/tools` エンドポイントに追加し、ジョブの ID を指定します。

**API 形式**

```http
GET /tools/job/{JOB_ID}
```

| パラメーター | 説明 |
| --- | --- |
| {JOB_ID} | 検索するジョブの ID。 |

**リクエスト**

次のリクエストでは、 {JOB_ID}.

```shell
curl -X GET \
  https://platform.adobe.io/data/foundation/exim/tools/job/{JOB_ID} \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
```

**応答**

正常な応答は、クエリされたジョブ ID の詳細を返し、リアルタイムのステータス更新を次のように提供します。 `completedTasks` および `failedTasks` は、ジョブの進行に応じて更新されます。

```json
{
    "status": "OK",
    "type": "SUCCESS",
    "message": "Job with ID: 0fe588dc4af64f9f98396cb6b49afb6c found",
    "object": {
        "id": "0fe588dc4af64f9f98396cb6b49afb6c",
        "name": "acme",
        "description": "Acme Business Group",
        "visibility": "TENANT",
        "requestType": "IMPORT",
        "expiry": 0,
        "snapshotId": "dd0b89579d554d07a814a620a44f9e43",
        "createdTimestamp": 1696510894380,
        "modifiedTimestamp": 1696510894380,
        "type": "PARTIAL",
        "jobStatus": "SUCCESS",
        "jobType": "NEW",
        "counter": 0,
        "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg",
        "jobHash": "6n8iV3KS6OGb0YQIQBaGhoAKKNeATeROqzV8/zbkNM8=",
        "sourceSandbox": {
            "name": "acme-sandbox",
            "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg",
            "empty": false
        },
        "destinationSandbox": {
            "name": "acme-sandbox1",
            "imsOrgId": "5C1328435BF324E90A49402A@AdobeOrg",
            "empty": false
        },
        "completedTasks": [
            {
                "taskType": "POST",
                "taskStatus": "SUCCESS",
                "artifact": {
                    "id": "https://ns.adobe.com/cjmstage/mixins/68f038b712e54546f99035a20d6ead649ca9d5b135eb24de",
                    "type": "REGISTRY_MIXIN",
                    "found": false,
                    "count": 0,
                    "title": "drb",
                    "messages": [
                        {
                            "status": "CREATED",
                            "attempt": 1,
                            "message": "REGISTRY_MIXIN created. Source id=https://ns.adobe.com/cjmstage/mixins/68f038b712e54546f99035a20d6ead649ca9d5b135eb24de; Target id=https://ns.adobe.com/cjmstage/mixins/220968a10a69ced58180ff1ccbe4f03124e44d17a0f23383"
                        }
                    ],
                    "newId": "https://ns.adobe.com/cjmstage/mixins/220968a10a69ced58180ff1ccbe4f03124e44d17a0f23383"
                }
            },
            {
                "taskType": "POST",
                "taskStatus": "SUCCESS",
                "artifact": {
                    "id": "https://ns.adobe.com/cjmstage/schemas/3926d1ff658a191bcb511b7c4ec45afee1c146a5b152e500",
                    "type": "REGISTRY_SCHEMA",
                    "found": false,
                    "count": 0,
                    "title": "drbFormSubmissions",
                    "messages": [
                        {
                            "status": "CREATED",
                            "attempt": 1,
                            "message": "REGISTRY_SCHEMA created. Source id=https://ns.adobe.com/cjmstage/schemas/3926d1ff658a191bcb511b7c4ec45afee1c146a5b152e500; Target id=https://ns.adobe.com/cjmstage/schemas/5e32908b8db9b37a5f3e7b5851d6ffa9e3bf8487abaef3c5"
                        }
                    ],
                    "newId": "https://ns.adobe.com/cjmstage/schemas/5e32908b8db9b37a5f3e7b5851d6ffa9e3bf8487abaef3c5"
                }
            },
            {
                "taskType": "POST",
                "taskStatus": "SUCCESS",
                "artifact": {
                    "id": "651344d17c8c8d289d10a8e6",
                    "type": "CATALOG_DATASET",
                    "found": false,
                    "count": 0,
                    "title": "drbFormSubmissions",
                    "messages": [
                        {
                            "status": "CREATED",
                            "attempt": 1,
                            "message": "CATALOG_DATASET created. Source id=651344d17c8c8d289d10a8e6; Target id=651eb3af5901df289dcb4511"
                        }
                    ],
                    "newId": "651eb3af5901df289dcb4511"
                }
            }
        ],
        "importReplacementMap": {
            "651344d17c8c8d289d10a8e6": "651eb3af5901df289dcb4511",
            "5a3b530ee7c4b38e9b33a69d22bfb75a2c5020e5a7a61e51": "b6d8ae6376864e22ed8169a9dac3b2115d1c72b5c1d1bced",
            "https://ns.adobe.com/cjmstage/mixins/68f038b712e54546f99035a20d6ead649ca9d5b135eb24de": "https://ns.adobe.com/cjmstage/mixins/220968a10a69ced58180ff1ccbe4f03124e44d17a0f23383",
            "https://ns.adobe.com/cjmstage/schemas/3926d1ff658a191bcb511b7c4ec45afee1c146a5b152e500": "https://ns.adobe.com/cjmstage/schemas/5e32908b8db9b37a5f3e7b5851d6ffa9e3bf8487abaef3c5"
        },
        "sourceParentArtifactMap": {
            "651344d17c8c8d289d10a8e6": "745F37C35E4B776E0A49421B@AdobeOrg::ajo-object-copy::CATALOG_DATASET::6512ec5a5bcd5e289c33a594",
            "5a3b530ee7c4b38e9b33a69d22bfb75a2c5020e5a7a61e51": "745F37C35E4B776E0A49421B@AdobeOrg::ajo-object-copy::REGISTRY_DESCRIPTOR::9d20176f1eb3f09dac4070b4bd6c4f79e8debdc8a0535725",
            "https://ns.adobe.com/cjmstage/mixins/68f038b712e54546f99035a20d6ead649ca9d5b135eb24de": "745F37C35E4B776E0A49421B@AdobeOrg::ajo-object-copy::REGISTRY_MIXIN::https://ns.adobe.com/cjmstage/mixins/37d29b7590c59928583b4d9a189229261291e388b7aa1031",
            "https://ns.adobe.com/cjmstage/schemas/3926d1ff658a191bcb511b7c4ec45afee1c146a5b152e500": "745F37C35E4B776E0A49421B@AdobeOrg::ajo-object-copy::REGISTRY_SCHEMA::https://ns.adobe.com/cjmstage/schemas/926a7aa38b7cc93b87dd03c8f73e6e7537651407c30b66a0"
        }
    }
}
```
