---
keywords: Experience Platform;プロファイル;リアルタイム顧客プロファイル;トラブルシューティング;API
title: エンティティ（プロファイルアクセス） API エンドポイント
type: Documentation
description: Adobe Experience Platformでは、RESTful API またはユーザーインターフェイスを使用して、リアルタイム顧客プロファイルデータにアクセスできます。 このガイドでは、Profile API を使用してエンティティ（一般的には「プロファイル」と呼ばれます）にアクセスする方法について説明します。
role: Developer
exl-id: 06a1a920-4dc4-4468-ac15-bf4a6dc885d4
source-git-commit: 9f9823a23c488e63b8b938cb885f050849836e36
workflow-type: tm+mt
source-wordcount: '2181'
ht-degree: 36%

---

# エンティティエンドポイント （プロファイルアクセス）

Adobe Experience Platformでは、RESTful API またはユーザーインターフェイスを使用して、[!DNL Real-Time Customer Profile] データにアクセスできます。 このガイドでは、API を使用してエンティティ（より一般的には「プロファイル」として知られています）にアクセスする方法について説明します。[!DNL Platform] UI を使用したプロファイルへのアクセスについて詳しくは、[ プロファイルユーザーガイド ](../ui/user-guide.md) を参照してください。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Real-Time Customer Profile API]](https://www.adobe.com/go/profile-apis-en) の一部です。先に進む前に、[はじめる前に](getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## エンティティの取得 {#retrieve-entity}

プロファイルエンティティまたはその時系列データを取得するには、必要なクエリパラメーターと共に `/access/entities` エンドポイントにGETリクエストを行います。

>[!BEGINTABS]

>[!TAB  プロファイルエンティティ ]

**API 形式**

```http
GET /access/entities?{QUERY_PARAMETERS}
```

クエリパスに指定されたデータパラメーターで、アクセスするデータを指定します。複数のパラメーターを使用する場合は、アンパサンド（&amp;）で区切ります。

プロファイルエンティティにアクセスするには、次のクエリパラメーターを指定する **必要があります**。

- `schema.name`：エンティティの XDM スキーマの名前。 このユースケースでは、`schema.name=_xdm.context.profile` です。
- `entityId`：取得しようとしているエンティティの ID。
- `entityIdNS`：取得しようとしているエンティティの名前空間。 `entityId` が XID ではない場合、この値を指定する必要があ **ま**。

有効なリストの完全なパラメーターは、付録の「[クエリパラメータ](#query-parameters)」の節に記載されています。

**リクエスト**

次のリクエストでは、ID を使用して、顧客のメールアドレスと名前を取得します。

+++ ID を使用してエンティティを取得するリクエスト例

```shell
curl -X GET 'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.profile&entityId=janedoe@example.com&entityIdNS=email&fields=identities,person.name,workEmail' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答に成功すると、HTTP ステータス 200 とリクエストされたエンティティが返されます。

+++ リクエストされたエンティティを含む応答のサンプル

```json
{
    "BVrqzwVv7o2p3naHvnsWpqZXv3KJgA": {
        "entityId": "BVrqzwVv7o2p3naHvnsWpqZXv3KJgA",
        "sources": [
            "1000000000"
        ],
        "entity": {
            "identities": [
                {
                    "id": "89149270342662559642753730269986316601",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "janedoe@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "johnsmith@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316604",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "58832431024964181144308914570411162539",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316602",
                    "namespace": {
                        "code": "ecid"
                    },
                    "primary": true
                }
            ],
            "person": {
                "name": {
                    "firstName": "Jane",
                    "middleName": "F",
                    "lastName": "Doe"
                }
            },
            "workEmail": {
                "primary": true,
                "address": "janedoe@example.com",
                "label": "Jane Doe",
                "type": "work",
                "status": "active"
            }
        },
        "lastModifiedAt": "2018-08-28T20:57:24Z"
    }
}
```

+++

>[!NOTE]
>
>関連するグラフが 50 個を超える ID をリンクする場合、このサービスは HTTP ステータス 422（処理できないエンティティ）と「関連 ID が多すぎます」というメッセージを返します。このエラーが表示される場合は、検索を絞り込むためにクエリパラメータを追加することを検討してください。

>[!TAB  時系列イベント ]

**API 形式**

```http
GET /access/entities?{QUERY_PARAMETERS}
```

クエリパスに指定されたデータパラメーターで、アクセスするデータを指定します。複数のパラメーターを使用する場合は、アンパサンド（&amp;）で区切ります。

時系列イベントデータにアクセスするには、次のクエリパラメーターを指定する **必要があります**。

- `schema.name`：エンティティの XDM スキーマの名前。 このユースケースでは、この値は `schema.name=_xdm.context.experienceevent` です。
- `relatedSchema.name`：関連するスキーマの名前。 スキーマ名はエクスペリエンスイベントなので、この **必須** の値は `relatedSchema.name=_xdm.context.profile` にする必要があります。
- `relatedEntityId`：関連エンティティの ID。
- `relatedEntityIdNS`：関連エンティティの名前空間。 `relatedEntityId` が XID ではない場合、この値を指定する必要があ **ま**。

有効なリストの完全なパラメーターは、付録の「[クエリパラメータ](#query-parameters)」の節に記載されています。

**リクエスト**

次のリクエストでは、ID でプロファイルエンティティを検索し、エンティティに関連付けられているすべての時系列イベントの `endUserIDs`、`web`、`channel` プロパティの値を取得します。

+++ エンティティに関連付けられた時系列イベントを取得するサンプルリクエスト

```shell
curl -X GET 'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、リクエストクエリパラメーターで指定された時系列イベントと関連フィールドのページ分割リストと共に返されます。

>[!NOTE]
>
>リクエストで上限の 1 が指定（`limit=1`）されたので、以下の応答の `count` は 1 で、1 つのエンティティのみが返されます。

+++ リクエストされた時系列イベントデータを含む応答のサンプル

```json
{
    "_page": {
        "orderby": "timestamp",
        "start": "c8d11988-6b56-4571-a123-b6ce74236036",
        "count": 1,
        "next": "c8d11988-6b56-4571-a123-b6ce74236037"
    },
    "children": [
        {
            "relatedEntityId": "A29cgveD5y64e2RixjUXNzcm",
            "entityId": "c8d11988-6b56-4571-a123-b6ce74236036",
            "timestamp": 1531260476000,
            "entity": {
                "endUserIDs": {
                    "_experience": {
                        "ecid": {
                            "id": "89149270342662559642753730269986316900",
                            "namespace": {
                                "code": "ecid"
                            }
                        }
                    }
                },
                "channel": {
                    "_type": "web"
                },
                "web": {
                    "webPageDetails": {
                        "name": "Fernie Snow",
                        "pageViews": {
                            "value": 1
                        }
                    }
                }
            },
            "lastModifiedAt": "2018-08-21T06:49:02Z"
        }
    ],
    "_links": {
        "next": {
            "href": "/entities?start=c8d11988-6b56-4571-a123-b6ce74236037&orderby=timestamp&schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1"
        }
    }
}
```

+++

>[!TAB B2B アカウント ]

**API 形式**

```http
GET /access/entities?{QUERY_PARAMETERS}
```

クエリパスに指定されたデータパラメーターで、アクセスするデータを指定します。複数のパラメーターを使用する場合は、アンパサンド（&amp;）で区切ります。

B2B アカウントデータにアクセスするには、次のクエリパラメーターを指定する **必要があります**。

- `schema.name`：エンティティの XDM スキーマの名前。 このユースケースでは、この値は `schema.name=_xdm.context.account` です。
- `entityId`：取得しようとしているエンティティの ID。
- `entityIdNS`：取得しようとしているエンティティの名前空間。 `entityId` が XID ではない場合、この値を指定する必要があ **ま**。

有効なリストの完全なパラメーターは、付録の「[クエリパラメータ](#query-parameters)」の節に記載されています。

**リクエスト**

+++ B2B アカウントを取得するサンプルリクエスト

```shell
curl -X GET 'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.account&entityIdNs=b2b_account&entityId=2334262' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答に成功すると、HTTP ステータス 200 とリクエストされたエンティティが返されます。

+++ リクエストされたエンティティを含む応答のサンプル

```json
{
    "GuQ-AUFjgjaeIw": {
        "entityId": "GuQ-AUFjgjaeIw",
        "mergePolicy": {
            "id": "a6150f47-a94f-4c9d-bfa0-958a370020ee"
        },
        "sources": [
            "er_m_attr"
        ],
        "entity": {
            "_id": "id1",
            "extSourceSystemAudit": {
                "lastReferencedDate": "2024-03-09 12:21:43.0",
                "lastActivityDate": "2024-03-09 12:21:43.0",
                "lastUpdatedDate": "2024-03-09 12:21:43.0",
                "lastUpdatedBy": "{USER_ID}",
                "externalKey": {
                    "sourceID": "{SOURCE_ID}",
                    "sourceKey": "{SOURCE_KEY}",
                    "sourceInstanceID": "{SOURCE_INSTANCE_ID}",
                    "sourceType": "{SOURCE_TYPE}"
                },
                "lastViewedDate": "2024-03-09 12:21:43.0",
                "createdDate": "2024-03-09 12:21:43.0"
            },
            "accountID": "2334262",
            "identityMap": {
                "b2b_account": [
                    {
                        "id": "2334263"
                    },
                    {
                        "id": "2334262"
                    },
                    {
                        "id": "{SOURCE_ID}"
                    }
                ]
            },
            "isDeleted": false,
            "accountKey": {
                "sourceID": "2334262",
                "sourceKey": "2334262",
                "sourceInstanceID": "2334262",
                "sourceType": "Random"
            }
        }
    }
}
```

+++

>[!TAB B2B オポチュニティ ]

**API 形式**

```http
GET /access/entities?{QUERY_PARAMETERS}
```

クエリパスに指定されたデータパラメーターで、アクセスするデータを指定します。複数のパラメーターを使用する場合は、アンパサンド（&amp;）で区切ります。

B2B オポチュニティエンティティにアクセスするには、次のクエリパラメーターを指定する **必要があります**。

- `schema.name`：エンティティの XDM スキーマの名前。 このユースケースでは、`schema.name=_xdm.context.opportunity` です。
- `entityId`：取得しようとしているエンティティの ID。
- `entityIdNS`：取得しようとしているエンティティの名前空間。 `entityId` が XID ではない場合、この値を指定する必要があ **ま**。

有効なリストの完全なパラメーターは、付録の「[クエリパラメータ](#query-parameters)」の節に記載されています。

**リクエスト**

+++ B2B 商談エンティティを取得するリクエストのサンプル

```shell
curl -X GET 'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.opportunity&entityIdNs=b2b_opportunity&entityId=2334262' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答に成功すると、HTTP ステータス 200 とリクエストされたエンティティが返されます。

+++ リクエストされたエンティティを含む応答のサンプル

```json
{
  "Ggw_AUFjgjaeIw": {
        "entityId": "Ggw_AUFjgjaeIw",
        "mergePolicy": {
            "id": "162824be-07f5-4cd0-aa85-2ff3c8f6c775"
        },
        "sources": [
            "er_m_attr"
        ],
        "entity": {
            "_id": "id1",
            "extSourceSystemAudit": {
                "lastReferencedDate": "2024-03-09 12:21:43.0",
                "lastActivityDate": "2024-03-09 12:21:43.0",
                "lastUpdatedDate": "2024-03-09 12:21:43.0",
                "lastUpdatedBy": "{USER_ID}",
                "externalKey": {
                    "sourceID": "00394S0001xpG6xABE",
                    "sourceKey": "0043c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce",
                    "sourceInstanceID": "00DC0000000Q35nMAC",
                    "sourceType": "Salesforce"
                },
                "lastViewedDate": "2024-03-09 12:21:43.0",
                "createdDate": "2024-03-09 12:21:43.0"
            },
            "accountID": "2334262",
            "identityMap": {
                "b2b_opportunity": [
                    {
                        "id": "0043c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce"
                    },
                    {
                        "id": "2334263"
                    },
                    {
                        "id": "2334262"
                    }
                ]
            },
            "isDeleted": false,
            "opportunityKey": {
                "sourceID": "2334262",
                "sourceKey": "2334262",
                "sourceInstanceID": "2334262",
                "sourceType": "Random"
            },
            "accountKey": {
                "sourceID": "2334262",
                "sourceKey": "2334262",
                "sourceInstanceID": "2334262",
                "sourceType": "Random"
            }
        }
    }
}
```

+++

>[!ENDTABS]

## 複数のエンティティの取得 {#retrieve-entities}

`/access/entities` エンドポイントにPOSTリクエストを実行し、ペイロードに ID を指定することで、複数のプロファイルエンティティまたは時系列イベントを取得できます。

>[!BEGINTABS]

>[!TAB  プロファイルエンティティ ]

**API 形式**

```http
POST /access/entities
```

**リクエスト**

次のリクエストは、ID のリストから複数の顧客の名前とメールアドレスを取得します。

+++複数のエンティティを取得するサンプルリクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/ups/access/entities \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "schema":{
            "name":"_xdm.context.profile"
        },
        "fields":[
            "identities",
            "person.name",
            "workEmail"
        ],
        "identities":[
            {
                "entityId":"89149270342662559642753730269986316601",
                "entityIdNS":{
                    "code":"ECID"
                }
            },
            {
                "entityId":"89149270342662559642753730269986316900",
                "entityIdNS":{
                    "code":"ECID"
                }
            },
            {
                "entityId":"89149270342662559642753730269986316602",
                "entityIdNS":{
                    "code":"ECID"
                }
            }
        ],
        "timeFilter": {
            "startTime": 1539838505,
            "endTime": 1539838510
        },
        "limit": 10,
        "orderby": "-timestamp"
      }'
```

| プロパティ | タイプ | 説明 |
| -------- |----- | ----------- |
| `schema.name` | 文字列 | **（必須）** エンティティが属する XDM スキーマの名前。 |
| `fields` | 配列 | 返される XDM フィールド（文字列の配列）。デフォルトでは、すべてのフィールドが返されます。 |
| `identities` | 配列 | **（必須）** アクセスするエンティティの ID のリストを含む配列。 |
| `identities.entityId` | 文字列 | アクセスするエンティティの ID。 |
| `identities.entityIdNS.code` | 文字列 | アクセスするエンティティ ID の名前空間。 |
| `timeFilter.startTime` | 整数 | プロファイルエンティティをフィルタリングするための開始時間をミリ秒単位で指定します。 デフォルトでは、この値は、使用可能な時間の始めとして設定されます。 |
| `timeFilter.endTime` | 整数 | プロファイルエンティティのフィルタリングの終了時間をミリ秒単位で指定します。 デフォルトでは、この値は利用可能な時間の終わりとして設定されています。 |
| `limit` | 整数 | 返されるレコードの最大数。 デフォルトでは、この値は 1000 に設定されています。 |
| `orderby` | 文字列 | 取得したエクスペリエンスイベントをタイムスタンプ別に並べ替える順序です。デフォルトは `+timestamp` で、`(+/-)timestamp` として記述されます。 |

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、リクエスト本文で指定されたエンティティのリクエストフィールドと共に返されます。

+++ リクエストされたエンティティを含む応答のサンプル

```json
{
    "A29cgveD5y64ezlhxjUXNzcm": {
        "entityId": "A29cgveD5y64ezlhxjUXNzcm",
        "sources": [
            "1000000000"
        ],
        "entity": {
            "identities": [
                {
                    "id": "89149270342662559642753730269986316601",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "janedoe@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "05DD23564EC4607F0A490D44",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316603",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "janesmith@example.com",
                    "namespace": {
                        "code": "email"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316604",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316700",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316701",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "58832431024964181144308914570411162539",
                    "namespace": {
                        "code": "ecid"
                    }
                },
                {
                    "id": "89149270342662559642753730269986316602",
                    "namespace": {
                        "code": "ecid"
                    },
                    "primary": true
                }
            ],
            "person": {
                "name": {
                    "firstName": "Jane",
                    "middleName": "F",
                    "lastName": "Doe"
                }
            },
            "workEmail": {
                "primary": true,
                "address": "janedoe@example.com",
                "label": "Jane Doe",
                "type": "work",
                "status": "active"
            }
        },
        "lastModifiedAt": "2018-08-28T20:57:24Z"
    },
    "A29cgveD5y64e2RixjUXNzcm": {
        "entityId": "A29cgveD5y64e2RixjUXNzcm",
        "sources": [
            ""
        ],
        "entity": {},
        "lastModifiedAt": "1970-01-01T00:00:00Z"
    },
    "A29cgveD5y64ezphxjUXNzcm": {
        "entityId": "A29cgveD5y64ezphxjUXNzcm",
        "sources": [
            "1000000000"
        ],
        "entity": {
            "identities": [
                {
                    "id": "89149270342662559642753730269986316602",
                    "namespace": {
                        "code": "ecid"
                    },
                    "primary": true
                },
                {
                    "id": "janedoe@example.com",
                    "namespace": {
                        "code": "email"
                    }
                }
            ],
            "person": {
                "name": {
                    "firstName": "Jane",
                    "middleName": "F",
                    "lastName": "Doe"
                }
            },
            "workEmail": {
                "primary": true,
                "address": "janedoe@example.com",
                "label": "Jane Doe",
                "type": "work",
                "status": "active"
            }
        },
        "lastModifiedAt": "2018-08-27T23:25:52Z"
    }
}
```

+++

>[!TAB  時系列イベント ]

**API 形式**

```http
POST /access/entities
```

**リクエスト**

次のリクエストでは、プロファイル ID のリストに関連付けられた時系列イベントのユーザー ID、現地時間、国コードを取得します。

+++ 時系列データを取得するサンプルリクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/ups/access/entities \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "schema": {
        "name": "_xdm.context.experienceevent"
    },
    "relatedSchema": {
        "name": "_xdm.context.profile"
    },
    "identities": [
        {
            "relatedEntityId": "GkouAW-yD9aoRCPhRYROJ-TetAFW"
        }
        {
            "relatedEntityId": "GkouAW-2u-7iWt5vQ9u2wm40JOZY"
        }
    ],
    "fields": [
        "endUserIDs",
        "placeContext.localTime",
        "placeContext.geo.countryCode"
    ],
    
    "timeFilter": {
        "startTime": 11539838505
        "endTime": 1539838510
    },
    "limit": 10,
    "orderby": "-timestamp"
}'
```

| プロパティ | タイプ | 説明 |
| -------- | ---- | ----------- |
| `schema.name` | 文字列 | **（必須）** エンティティが属する XDM スキーマの名前。 |
| `relatedSchema.name` | 文字列 | `schema.name` が `_xdm.context.experienceevent` の場合、この値は、時系列イベントが関連するプロファイルエンティティのスキーマを指定する必要があります。 |
| `identities` | 配列 | **（必須）** 関連する時系列イベントを取得するプロファイルの配列リスト。 配列内の各エントリは、次の 2 つの方法のいずれかで設定されます。 <ol><li>ID 値および名前空間で構成される完全修飾 ID の使用</li><li>XID の指定</li></ol> |
| `fields` | 文字列 | 返される XDM フィールド（文字列の配列）。デフォルトでは、すべてのフィールドが返されます。 |
| `orderby` | 文字列 | 取得したエクスペリエンスイベントをタイムスタンプ別に並べ替える順序です。デフォルトは `+timestamp` で、`(+/-)timestamp` として記述されます。 |
| `timeFilter.startTime` | 整数 | 時系列オブジェクトをフィルタリングするための開始時間をミリ秒単位で指定します。 デフォルトでは、この値は、使用可能な時間の始めとして設定されます。 |
| `timeFilter.endTime` | 整数 | 時系列オブジェクトをフィルタリングするための終了時間をミリ秒単位で指定します。 デフォルトでは、この値は利用可能な時間の終わりとして設定されています。 |
| `limit` | 整数 | 返されるレコードの最大数。 デフォルトでは、この値は 1,000 に設定されています。 |

+++

**応答**

応答が成功すると、HTTP ステータス 200 が、リクエストで指定された複数のプロファイルに関連付けられた時系列イベントのページ分割リストと共に返されます。

+++ 時系列イベントを含む応答のサンプル

```json
{
    "GkouAW-yD9aoRCPhRYROJ-TetAFW": {
        "_page": {
            "orderby": "timestamp",
            "start": "ee0fa8eb-f09c-4d72-a432-fea7f189cfcd",
            "count": 10,
            "next": "40cb2fb3-78cd-49d3-806f-9bdb22748226"
        },
        "children": [
            {
                "relatedEntityId": "GkouAW-yD9aoRCPhRYROJ-TetAFW",
                "entityId": "ee0fa8eb-f09c-4d72-a432-fea7f189cfcd",
                "timestamp": 1537275882000,
                "entity": {
                    "endUserIDs": {
                        "_experience": {
                            "mcid": {
                                "id": "67971860962043911970658021809222795905",
                                "namespace": {
                                    "code": "ECID"
                                }
                            },
                            "aacustomid": {
                                "id": "50353446361742744826197433431642033796",
                                "namespace": {
                                    "code": "CRMID"
                                },
                                "primary": true
                            },
                            "acid": {
                                "id": "2de32e9a00003314-2fd9c00000000026",
                                "namespace": {
                                    "code": "AVID"
                                }
                            }
                        }
                    },
                    "placeContext": {
                        "localTime": "2018-09-18T13:04:42Z",
                        "geo": {
                            "countryCode": "MX"
                        }
                    }
                },
                "lastModifiedAt": "2018-10-24T17:35:01Z"
            },
            {
                "relatedEntityId": "GkouAW-yD9aoRCPhRYROJ-TetAFW",
                "entityId": "a9e137b4-1348-4878-8167-e308af523d8b",
                "timestamp": 1537275889000,
                "entity": {
                    "endUserIDs": {
                        "_experience": {
                            "mcid": {
                                "id": "67971860962043911970658021809222795905",
                                "namespace": {
                                    "code": "ECID"
                                }
                            },
                            "aacustomid": {
                                "id": "50353446361742744826197433431642033796",
                                "namespace": {
                                    "code": "CRMID"
                                },
                                "primary": true
                            },
                            "acid": {
                                "id": "2de32e9a00003314-2fd9c00000000026",
                                "namespace": {
                                    "code": "AVID"
                                }
                            }
                        }
                    },
                    "placeContext": {
                        "localTime": "2018-09-18T13:04:49Z",
                        "geo": {
                            "countryCode": "MX"
                        }
                    }
                },
                "lastModifiedAt": "2018-10-24T17:35:01Z"
            }
        ],
        "_links": {
            "next": {
                "href": "/entities",
                "payload": {
                    "schema": {
                        "name": "_xdm.context.experienceevent"
                    },
                    "relatedSchema": {
                        "name": "_xdm.context.profile"
                    },
                    "timeFilter": {
                        "startTime": 1537275882000
                    },
                    "fields": [
                        "endUserIDs",
                        "placeContext.localTime",
                        "placeContext.geo.countryCode"
                    ],
                    "identities": [
                        {
                            "relatedEntityId": "GkouAW-yD9aoRCPhRYROJ-TetAFW",
                            "start": "40cb2fb3-78cd-49d3-806f-9bdb22748226"
                        }
                    ],
                    "limit": 10
                }
            }
        }
    },
    "GkouAW-2u-7iWt5vQ9u2wm40JOZY": {
        "_page": {
            "orderby": "timestamp",
            "start": "2746d0db-fa64-4e29-b67e-324bec638816",
            "count": 9,
            "next": ""
        },
        "children": [
            {
                "relatedEntityId": "GkouAW-2u-7iWt5vQ9u2wm40JOZY",
                "entityId": "2746d0db-fa64-4e29-b67e-324bec638816",
                "timestamp": 1537559483000,
                "entity": {
                    "endUserIDs": {
                        "_experience": {
                            "mcid": {
                                "id": "76436745599328540420034822220063618863",
                                "namespace": {
                                    "code": "ECID"
                                }
                            },
                            "aacustomid": {
                                "id": "48593470048917738786405847327596263131",
                                "namespace": {
                                    "code": "CRMID"
                                },
                                "primary": true
                            },
                            "acid": {
                                "id": "2de32e9a80007451-03da600000000028",
                                "namespace": {
                                    "code": "AVID"
                                }
                            }
                        }
                    },
                    "placeContext": {
                        "localTime": "2018-09-21T19:51:23Z",
                        "geo": {
                            "countryCode": "US"
                        }
                    }
                },
                "lastModifiedAt": "2018-10-24T17:34:58Z"
            },
            {
                "relatedEntityId": "GkouAW-2u-7iWt5vQ9u2wm40JOZY",
                "entityId": "9bf337a1-3256-431e-a38c-5c0d42d121d1",
                "timestamp": 1537559486000,
                "entity": {
                    "endUserIDs": {
                        "_experience": {
                            "mcid": {
                                "id": "76436745599328540420034822220063618863",
                                "namespace": {
                                    "code": "ECID"
                                }
                            },
                            "aacustomid": {
                                "id": "48593470048917738786405847327596263131",
                                "namespace": {
                                    "code": "CRMID"
                                },
                                "primary": true
                            },
                            "acid": {
                                "id": "2de32e9a80007451-03da600000000028",
                                "namespace": {
                                    "code": "AVID"
                                }
                            }
                        }
                    },
                    "placeContext": {
                        "localTime": "2018-09-21T19:51:26Z",
                        "geo": {
                            "countryCode": "US"
                        }
                    }
                },
                "lastModifiedAt": "2018-10-24T17:34:58Z"
            }
        ],
        "_links": {
            "next": {
                "href": ""
            }
        }
    }
}`
```

+++

>[!NOTE]
>
>この応答の例では、最初にリストされたプロファイル（「GkouAW-yD9aoRCPhRYROJ-TetAFW」）が `_links.next.payload` の値を提供します。つまり、このプロファイルに対して結果のページが追加されます。
>
>これらの結果にアクセスするには、リストされたペイロードをリクエスト本文として、`/access/entities` エンドポイントに追加のPOSTリクエストを実行します。

>[!TAB B2B アカウント ]

**API 形式**

```http
POST /access/entities
```

**リクエスト**

次のリクエストは、リクエストされた B2B アカウントを取得します。

+++複数のエンティティを取得するサンプルリクエスト

```shell
curl -X POST https://platform.adobe.io/data/core/ups/access/entities \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "schema":{
            "name":"_xdm.context.account"
        },
        "identities": [
            {
                "entityId": "2334262",
                "entityIdNS": {
                    "code":"b2b_account"
                }
            },
            {
                "entityId": "2334263",
                "entityIdNS": {
                    "code":"b2b_account"
                }
            },
            {
                "entityId": "2334264",
                "entityIdNS": {
                    "code":"b2b_account"
                }
            }
        ]
    }'
```

| プロパティ | タイプ | 説明 |
| -------- |----- | ----------- |
| `schema.name` | 文字列 | **（必須）** エンティティが属する XDM スキーマの名前。 |
| `identities` | 配列 | **（必須）** アクセスするエンティティの ID のリストを含む配列。 |
| `identities.entityId` | 文字列 | アクセスするエンティティの ID。 |
| `identities.entityIdNS.code` | 文字列 | アクセスするエンティティ ID の名前空間。 |

+++

**応答**

応答に成功すると、HTTP ステータス 200 とリクエストされたエンティティが返されます。

+++ リクエストされたエンティティを含む応答のサンプル

```json
{
    "GuQ-AUFjgjeeIw": {
        "requestedIdentity": {
            "entityId": "2334263",
            "entityIdNS": {
                "code": "b2b_account"
            }
        },
        "entityId": "GuQ-AUFjgjeeIw",
        "mergePolicy": {
            "id": "a6150f47-a94f-4c9d-bfa0-958a370020ee"
        },
        "sources": [
            "er_m_attr"
        ],
        "entity": {
            "_id": "id1",
            "extSourceSystemAudit": {
                "lastReferencedDate": "2024-03-09 12:21:43.0",
                "lastActivityDate": "2024-03-09 12:21:43.0",
                "lastUpdatedDate": "2024-03-09 12:21:43.0",
                "lastUpdatedBy": "{USER_ID}",
                "externalKey": {
                    "sourceID": "00394S0001xpG6xABE",
                    "sourceKey": "0043c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce",
                    "sourceInstanceID": "00DC0000000Q35nMAC",
                    "sourceType": "Salesforce"
                },
                "lastViewedDate": "2024-03-09 12:21:43.0",
                "createdDate": "2024-03-09 12:21:43.0"
            },
            "accountID": "2334262",
            "identityMap": {
                "b2b_account": [
                    {
                        "id": "2334263"
                    },
                    {
                        "id": "2334262"
                    },
                    {
                        "id": "0043c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce"
                    }
                ]
            },
            "isDeleted": false,
            "accountKey": {
                "sourceID": "2334262",
                "sourceKey": "2334262",
                "sourceInstanceID": "2334262",
                "sourceType": "Random"
            }
        }
    },
    "GuQ-AUFjgjaeIw": {
        "requestedIdentity": {
            "entityId": "2334262",
            "entityIdNS": {
                "code": "b2b_account"
            }
        },
        "entityId": "GuQ-AUFjgjaeIw",
        "mergePolicy": {
            "id": "a6150f47-a94f-4c9d-bfa0-958a370020ee"
        },
        "sources": [
            "er_m_attr"
        ],
        "entity": {
            "_id": "id1",
            "extSourceSystemAudit": {
                "lastReferencedDate": "2024-03-09 12:21:43.0",
                "lastActivityDate": "2024-03-09 12:21:43.0",
                "lastUpdatedDate": "2024-03-09 12:21:43.0",
                "lastUpdatedBy": "{USER_ID}",
                "externalKey": {
                    "sourceID": "00394S0001xpG6xABE",
                    "sourceKey": "0043c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce",
                    "sourceInstanceID": "00DC0000000Q35nMAC",
                    "sourceType": "Salesforce"
                },
                "lastViewedDate": "2024-03-09 12:21:43.0",
                "createdDate": "2024-03-09 12:21:43.0"
            },
            "accountID": "2334262",
            "identityMap": {
                "b2b_account": [
                    {
                        "id": "2334263"
                    },
                    {
                        "id": "2334262"
                    },
                    {
                        "id": "0043c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce"
                    }
                ]
            },
            "isDeleted": false,
            "accountKey": {
                "sourceID": "2334262",
                "sourceKey": "2334262",
                "sourceInstanceID": "2334262",
                "sourceType": "Random"
            }
        }
    },
    "GuQ-AUFjgjmeIw": {
        "requestedIdentity": {
            "entityId": "2334265",
            "entityIdNS": {
                "code": "b2b_account"
            }
        },
        "entityId": "GuQ-AUFjgjmeIw",
        "mergePolicy": {
            "id": "a6150f47-a94f-4c9d-bfa0-958a370020ee"
        },
        "sources": [
            "er_m_attr"
        ],
        "entity": {
            "_id": "id1",
            "extSourceSystemAudit": {
                "lastReferencedDate": "2024-03-09 12:21:43.0",
                "lastActivityDate": "2024-03-09 12:21:43.0",
                "lastUpdatedDate": "2024-03-09 12:21:43.0",
                "lastUpdatedBy": "{USER_ID}",
                "externalKey": {
                    "sourceID": "00394S0001xpG6xABE",
                    "sourceKey": "0054c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce",
                    "sourceInstanceID": "00DC0000000Q35nMAC",
                    "sourceType": "Salesforce"
                },
                "lastViewedDate": "2024-03-09 12:21:43.0",
                "createdDate": "2024-03-09 12:21:43.0"
            },
            "accountID": "2334265",
            "identityMap": {
            "b2b_account": [
                {
                    "id": "0054c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce"
                },
                {
                    "id": "2334265"
                }
            ]
        },
        "isDeleted": false,
        "accountKey": {
            "sourceID": "2334265",
            "sourceKey": "2334265",
            "sourceInstanceID": "2334265",
            "sourceType": "Random"
        }
    }
}
```

+++

>[!TAB B2B オポチュニティ ]

**API 形式**

```http
POST /access/entities
```

**リクエスト**

次のリクエストは、リクエストされた B2B オポチュニティを取得します。

+++ 複数のエンティティを取得するリクエストのサンプル

```shell
curl -X POST https://platform.adobe.io/data/core/ups/access/entities \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "schema":{
            "name":"_xdm.context.opportunity"
        },
        "identities": [
            {
                "entityId": "2334262",
                "entityIdNS": {
                    "code":"b2b_opportunity"
                }
            },
            {
                "entityId": "2334263",
                "entityIdNS": {
                    "code":"b2b_opportunity"
                }
            },
            {
                "entityId": "2334264",
                "entityIdNS": {
                    "code":"b2b_opportunity"
                }
            },
            {
                "entityId": "2334265",
                "entityIdNS": {
                    "code":"b2b_opportunity"
                }
            }
        ]
    }'
```

| プロパティ | タイプ | 説明 |
| -------- |----- | ----------- |
| `schema.name` | 文字列 | **（必須）** エンティティが属する XDM スキーマの名前。 |
| `identities` | 配列 | **（必須）** アクセスするエンティティの ID のリストを含む配列。 |
| `identities.entityId` | 文字列 | アクセスするエンティティの ID。 |
| `identities.entityIdNS.code` | 文字列 | アクセスするエンティティ ID の名前空間。 |

+++

**応答**

応答に成功すると、HTTP ステータス 200 とリクエストされたエンティティが返されます。

+++ リクエストされたエンティティを含む応答のサンプル

```json
{
    "Ggw_AUFjgjaeIw": {
        "requestedIdentity": {
            "entityId": "2334262",
            "entityIdNS": {
                "code": "b2b_opportunity"
            }
        },
        "entityId": "Ggw_AUFjgjaeIw",
        "mergePolicy": {
            "id": "162824be-07f5-4cd0-aa85-2ff3c8f6c775"
        },
        "sources": [
            "er_m_attr"
        ],
        "entity": {
            "_id": "id1",
            "extSourceSystemAudit": {
                "lastReferencedDate": "2024-03-09 12:21:43.0",
                "lastActivityDate": "2024-03-09 12:21:43.0",
                "lastUpdatedDate": "2024-03-09 12:21:43.0",
                "lastUpdatedBy": "{USER_ID}",
                "externalKey": {
                    "sourceID": "00394S0001xpG6xABE",
                    "sourceKey": "0043c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce",
                    "sourceInstanceID": "00DC0000000Q35nMAC",
                    "sourceType": "Salesforce"
                },
                "lastViewedDate": "2024-03-09 12:21:43.0",
                "createdDate": "2024-03-09 12:21:43.0"
            },
            "accountID": "2334262",
            "identityMap": {
                "b2b_opportunity": [
                    {
                        "id": "0043c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce"
                    },
                    {
                        "id": "2334263"
                    },
                    {
                        "id": "2334262"
                    }
                ]
            },
            "isDeleted": false,
            "opportunityKey": {
                "sourceID": "2334262",
                "sourceKey": "2334262",
                "sourceInstanceID": "2334262",
                "sourceType": "Random"
            },
            "accountKey": {
                "sourceID": "2334262",
                "sourceKey": "2334262",
                "sourceInstanceID": "2334262",
                "sourceType": "Random"
            }
        }
    },
    "Ggw_AUFjgjieIw": {
        "requestedIdentity": {
            "entityId": "2334264",
            "entityIdNS": {
                "code": "b2b_opportunity"
            }
        },
        "entityId": "Ggw_AUFjgjieIw",
        "mergePolicy": {
            "id": "162824be-07f5-4cd0-aa85-2ff3c8f6c775"
        },
        "sources": [
            "er_m_attr"
        ],
        "entity": {
            "_id": "id1",
            "extSourceSystemAudit": {
                "lastReferencedDate": "2024-03-09 12:21:43.0",
                "lastActivityDate": "2024-03-09 12:21:43.0",
                "lastUpdatedDate": "2024-03-09 12:21:43.0",
                "lastUpdatedBy": "{USER_ID}",
                "externalKey": {
                    "sourceID": "00394S0001xpG6xABE",
                    "sourceKey": "0041c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce",
                    "sourceInstanceID": "00DC0000000Q35nMAC",
                    "sourceType": "Salesforce"
                },
                "lastViewedDate": "2024-03-09 12:21:43.0",
                "createdDate": "2024-03-09 12:21:43.0"
            },
            "accountID": "2334264",
            "identityMap": {
                "b2b_opportunity": [
                    {
                        "id": "2334264"
                    },
                    {
                        "id": "0041c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce"
                    }
                ]
            },
            "isDeleted": false,
            "opportunityKey": {
                "sourceID": "2334262",
                "sourceKey": "2334262",
                "sourceInstanceID": "2334262",
                "sourceType": "Random"
            },
            "accountKey": {
                "sourceID": "2334264",
                "sourceKey": "2334264",
                "sourceInstanceID": "2334264",
                "sourceType": "Salesforce"
            }
        }
    },
    "Ggw_AUFjgjeeIw": {
        "requestedIdentity": {
            "entityId": "2334263",
            "entityIdNS": {
                "code": "b2b_opportunity"
            }
        },
        "entityId": "Ggw_AUFjgjeeIw",
        "mergePolicy": {
            "id": "162824be-07f5-4cd0-aa85-2ff3c8f6c775"
        },
        "sources": [
            "er_m_attr"
        ],
        "entity": {
            "_id": "id1",
            "extSourceSystemAudit": {
                "lastReferencedDate": "2024-03-09 12:21:43.0",
                "lastActivityDate": "2024-03-09 12:21:43.0",
                "lastUpdatedDate": "2024-03-09 12:21:43.0",
                "lastUpdatedBy": "{USER_ID}",
                "externalKey": {
                    "sourceID": "00394S0001xpG6xABE",
                    "sourceKey": "0043c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce",
                    "sourceInstanceID": "00DC0000000Q35nMAC",
                    "sourceType": "Salesforce"
                },
                "lastViewedDate": "2024-03-09 12:21:43.0",
                "createdDate": "2024-03-09 12:21:43.0"
            },
            "accountID": "2334262",
            "identityMap": {
                "b2b_opportunity": [
                    {
                        "id": "0043c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce"
                    },
                    {
                        "id": "2334263"
                    },
                    {
                        "id": "2334262"
                    }
                ]
            },
            "isDeleted": false,
            "opportunityKey": {
                "sourceID": "2334262",
                "sourceKey": "2334262",
                "sourceInstanceID": "2334262",
                "sourceType": "Random"
            },
            "accountKey": {
                "sourceID": "2334262",
                "sourceKey": "2334262",
                "sourceInstanceID": "2334262",
                "sourceType": "Random"
            }
        }
    },
    "Ggw_AUFjgjmeIw": {
        "requestedIdentity": {
            "entityId": "2334265",
            "entityIdNS": {
                "code": "b2b_opportunity"
            }
        },
        "entityId": "Ggw_AUFjgjmeIw",
        "mergePolicy": {
            "id": "162824be-07f5-4cd0-aa85-2ff3c8f6c775"
        },
        "sources": [
            "er_m_attr"
        ],
        "entity": {
            "_id": "id1",
            "extSourceSystemAudit": {
                "lastReferencedDate": "2024-03-09 12:21:43.0",
                "lastActivityDate": "2024-03-09 12:21:43.0",
                "lastUpdatedDate": "2024-03-09 12:21:43.0",
                "lastUpdatedBy": "{USER_ID}",
                "externalKey": {
                    "sourceID": "00394S0001xpG6xABE",
                    "sourceKey": "0054c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce",
                    "sourceInstanceID": "00DC0000000Q35nMAC",
                    "sourceType": "Salesforce"
                },
                "lastViewedDate": "2024-03-09 12:21:43.0",
                "createdDate": "2024-03-09 12:21:43.0"
            },
            "accountID": "2334265",
            "identityMap": {
                "b2b_opportunity": [
                    {
                        "id": "2334265"
                    },
                    {
                        "id": "0054c329201xpG6xAAE@00DC0000000Q35nWIN.Salesforce"
                    }
                ]
            },
            "isDeleted": false,
            "opportunityKey": {
                "sourceID": "2334262",
                "sourceKey": "2334262",
                "sourceInstanceID": "2334262",
                "sourceType": "Random"
            },
            "accountKey": {
                "sourceID": "2334265",
                "sourceKey": "2334265",
                "sourceInstanceID": "2334265",
                "sourceType": "Random"
            }
        }
    }
}
```

+++

>[!ENDTABS]

### 結果の後続ページへのアクセス

時系列イベントを取得すると、結果はページ付けされます。結果の後続のページがある場合、`_page.next` プロパティには ID が含まれます。また、`_links.next.href` プロパティは次のページを取得するためのリクエスト URI を提供します。結果を取得するには、`/access/entities` エンドポイントに対して別のGETリクエストを実行し、`/entities` を指定された URI の値に置き換えます。

>[!NOTE]
>
>リクエストパスで誤ってリク `/entities/` ストを繰り返さないようにします。 これは一度だけ `/access/entities?start=...` のように存在するべきです。

**API 形式**

```http
GET /access/{NEXT_URI}
```

| パラメーター | 説明 |
|---|---|
| `{NEXT_URI}` | `_links.next.href` から取得した URI 値。 |

**リクエスト**

次のリクエストでは、`_links.next.href` URI をリクエストパスとして使用して、次のページの結果を取得します。

+++ 結果の次のページにアクセスするためのサンプルリクエスト

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/access/entities?start=c8d11988-6b56-4571-a123-b6ce74236037&orderby=timestamp&schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答** 

正常な応答は、結果の次のページを返します。この応答には、`_page.next` および `_links.next.href` の空の文字列値で示される結果の後続ページはありません。

+++ エンティティの次のページを含む応答のサンプル

```json
{
    "_page": {
        "orderby": "timestamp",
        "start": "c8d11988-6b56-4571-a123-b6ce74236037",
        "count": 1,
        "next": ""
    },
    "children": [
        {
            "relatedEntityId": "A29cgveD5y64e2RixjUXNzcm",
            "entityId": "c8d11988-6b56-4571-a123-b6ce74236037",
            "timestamp": 1531260477000,
            "entity": {
                "endUserIDs": {
                    "_experience": {
                        "ecid": {
                            "id": "89149270342662559642753730269986316900",
                            "namespace": {
                                "code": "ecid"
                            }
                        }
                    }
                },
                "channel": {
                    "_type": "web"
                },
                "web": {
                    "webPageDetails": {
                        "name": "Fernie Snow",
                        "pageViews": {
                            "value": 1
                        }
                    }
                }
            },
            "lastModifiedAt": "2018-08-21T06:50:01Z"
        }
    ],
    "_links": {
        "next": {
            "href": ""
        }
    }
}
```

+++

## エンティティを削除 {#delete-entity}

プロファイルストアからエンティティを削除するには、必要なクエリパラメーターと共に `/access/entities` エンドポイントに対してDELETEリクエストを行います。

**API 形式**

```http
DELETE /access/entities?{QUERY_PARAMETERS}
```

クエリパスに指定されたデータパラメーターで、アクセスするデータを指定します。複数のパラメーターを使用する場合は、アンパサンド（&amp;）で区切ります。

エンティティを削除するには、次のクエリパラメーターを指定する **必要があります**。

- `schema.name`：エンティティの XDM スキーマの名前。 このユースケースでは、`schema.name=_xdm.context.profile` を **のみ** 使用できます。
- `entityId`：取得しようとしているエンティティの ID。
- `entityIdNS`：取得しようとしているエンティティの名前空間。 `entityId` が XID ではない場合、この値を指定する必要があ **ま**。
- `mergePolicyId`：エンティティの結合ポリシー ID。 結合ポリシーには、ID ステッチとキー値 XDM オブジェクト結合に関する情報が含まれています。 この値を指定しない場合、デフォルトの結合ポリシーが使用されます。

**リクエスト**

次のリクエストは、指定されたエンティティを削除します。

+++ エンティティを削除するリクエストのサンプル

```shell
curl -X DELETE 'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.profile&entityId=janedoe@example.com&entityIdNS=email' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

+++

**応答**

応答が成功すると、HTTP ステータス 202 が、空の応答本文と共に返されます。

## 次の手順

このガイドに従うと、[!DNL Real-Time Customer Profile] のデータフィールド、プロファイルおよび時系列データに正常にアクセスできます。 [!DNL Platform] に保存されている他のデータリソースにアクセスする方法については、[ データアクセスの概要 ](../../data-access/home.md) を参照してください。

## 付録 {#appendix}

次の節では、API を使用した [!DNL Profile] データへのアクセスに関する補足情報を提供します。

### クエリパラメーター {#query-parameters}

次のパラメーターは、`/access/entities` エンドポイントに対する GET リクエストのパスで使用されます。アクセスするプロファイルエンティティを識別し、応答で返されるデータをフィルターします。必須パラメーターはラベル付けされますが、残りはオプションです。

| パラメーター | 説明 | 例 |
| --------- | ----------- | ------- |
| `schema.name` | **（必須）** エンティティの XDM スキーマの名前。 | `schema.name=_xdm.context.experienceevent` |
| `relatedSchema.name` | `schema.name` が `_xdm.context.experienceevent` の場合、この値は、時系列イベントが関連するプロファイルエンティティのスキーマを指定します **必須**。 | `relatedSchema.name=_xdm.context.profile` |
| `entityId` | **（必須）** エンティティの ID。 このパラメーターの値が XID でない場合、ID 名前空間パラメーター（`entityIdNS`）も指定する必要があります。 | `entityId=janedoe@example.com` |
| `entityIdNS` | XID として `entityId` を指定しない場合、このフィールドは ID 名前空間を指定します **必須**。 | `entityIdNS=email` |
| `relatedEntityId` | `schema.name` が `_xdm.context.experienceevent` の場合、この値は、関連するプロファイルエンティティの ID を指定します **必須**。 この値は、`entityId` と同じ規則に従います 。 | `relatedEntityId=69935279872410346619186588147492736556` |
| `relatedEntityIdNS` | `schema.name` が「_xdm.context.experienceevent」の場合、この値は `relatedEntityId` で指定したエンティティの ID 名前空間を指定する必要があります。 | `relatedEntityIdNS=CRMID` |
| `fields` | 応答で返されるデータをフィルターします。取得したデータに含めるスキーマフィールドの値を指定します。複数のフィールドの場合、値はコンマで区切り、スペースは使用しません。 | `fields=personalEmail,person.name,person.gender` |
| `mergePolicyId` | 返されるデータを制御する結合ポリシーを識別します。 呼び出しで指定されていない場合は、組織のデフォルトのスキーマが使用されます。デフォルトの結合ポリシーが設定されていない場合、デフォルトでは、プロファイル結合も ID ステッチも行われません。 | `mergePolicyId=5aa6885fcf70a301dabdfa4a` |
| `orderBy` | 取得したエンティティの並べ替え順（タイムスタンプ別）。 これは `(+/-)timestamp` として記述され、デフォルトは `+timestamp` です。 | `orderby=-timestamp` |
| `startTime` | エンティティをフィルター処理する開始時間をミリ秒単位で指定します。 | `startTime=1539838505` |
| `endTime` | エンティティのフィルタリングの終了時間をミリ秒単位で指定します。 | `endTime=1539838510` |
| `limit` | 返されるエンティティの最大数を指定します。 デフォルトでは、この値は 1000 に設定されています。 | `limit=100` |
| `property` | プロパティ値でフィルタリングします。 このクエリパラメーターでは、=、！のエバリュエーターがサポートされています。=、&lt;、&lt;=、>、>=。 これは、最大 3 つのプロパティをサポートするエクスペリエンスイベントでのみ使用できます。 | `property=webPageDetails.isHomepage=true&property=localTime<="2020-07-20"` |
