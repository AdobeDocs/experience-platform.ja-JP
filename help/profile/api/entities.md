---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、トラブルシューティング、API
title: エンティティ（プロファイルアクセス）API エンドポイント
topic-legacy: guide
type: Documentation
description: Adobe Experience Platform を使用すると、RESTful API またはユーザーインターフェイスを使用して、リアルタイムの顧客プロファイルデータにアクセスできます。このガイドでは、プロファイル API を使用してエンティティ（より一般的には「プロファイル」と呼ばれます）にアクセスする方法について説明します。
exl-id: 06a1a920-4dc4-4468-ac15-bf4a6dc885d4
source-git-commit: 4c544170636040b8ab58780022a4c357cfa447de
workflow-type: tm+mt
source-wordcount: '1732'
ht-degree: 90%

---

# エンティティエンドポイント（プロファイルアクセス）

Adobe Experience Platformでは、RESTful API またはユーザーインターフェイスを使用して [!DNL Real-time Customer Profile] データにアクセスできます。 このガイドでは、API を使用してエンティティ（より一般的には「プロファイル」として知られています）にアクセスする方法について説明します。[!DNL Platform] UI を使用したプロファイルへのアクセスについて詳しくは、『[ プロファイルユーザーガイド ](../ui/user-guide.md)』を参照してください。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Real-time Customer Profile API]](https://www.adobe.com/go/profile-apis-jp) の一部です。先に進む前に、[はじめる前に](getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## ID によるプロファイルデータへのアクセス

[!DNL Profile] エンティティにアクセスするには、`/access/entities` エンドポイントにGETリクエストを送信し、一連のクエリパラメーターとしてエンティティの ID を指定します。 この ID は、ID 値（`entityId`）と ID 名前空間（`entityIdNS`）です。

クエリパスに指定されたデータパラメーターで、アクセスするデータを指定します。複数のパラメーターを含め、アンパサンド（&amp;）で区切ることができます。有効なリストの完全なパラメーターは、付録の「[クエリパラメータ](#query-parameters)」の節に記載されています。

**API 形式**

```http
GET /access/entities?{QUERY_PARAMETERS}
```

**リクエスト**

次のリクエストでは、顧客の電子メールと名前を ID を使用して取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.profile&entityId=janedoe@example.com&entityIdNS=email&fields=identities,person.name,workEmail' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

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

>[!NOTE]
>
>関連するグラフが 50 個を超える ID をリンクする場合、このサービスは HTTP ステータス 422（処理できないエンティティ）と「関連 ID が多すぎます」というメッセージを返します。このエラーが表示される場合は、検索を絞り込むためにクエリパラメータを追加することを検討してください。

## ID リストによるプロファイルデータへのアクセス

`/access/entities` エンドポイントに対して POST リクエストを送信し、ペイロードに ID を提供することで、ID によって複数のプロファイルエンティティにアクセスできます。これらの ID は、ID 値（`entityId`）と ID 名前空間（`entityIdNS`）で構成されます。

**API 形式**

```http
POST /access/entities
```

**リクエスト**

次のリクエストでは、複数の顧客の名前と電子メールアドレスを ID のリストで取得します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/access/entities \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
        "orderby": "-timestamp",
        "withCA": true
      }'
```

| プロパティ | 説明 |
|---|---|
| `schema.name` | ***（必須）*** エンティティが属する XDM スキーマの名前。 |
| `fields` | 返される XDM フィールド（文字列の配列）。デフォルトでは、すべてのフィールドが返されます。 |
| `identities` | ***（必須）*** アクセスするエンティティの ID のリストを含む配列。 |
| `identities.entityId` | アクセスするエンティティの ID。 |
| `identities.entityIdNS.code` | アクセスするエンティティ ID の名前空間。 |
| `timeFilter.startTime` | 範囲フィルターの開始時間（含む）。精度はミリ秒にする必要があります。指定しなかった場合、デフォルトは使用可能な時間の始まりです。 |
| `timeFilter.endTime` | 範囲フィルターの終了時間（除外）。精度はミリ秒にする必要があります。指定しなかった場合、デフォルトは使用可能な時間の終わりです。 |
| `limit` | 返すレコードの数。返されたエクスペリエンスのイベント数にのみ適用。デフォルトは 1,000 です。 |
| `orderby` | 取得したエクスペリエンスイベントをタイムスタンプ別に並べ替える順序です。デフォルトは `+timestamp` で、`(+/-)timestamp` として記述されます。 |
| `withCA` | 参照の計算済み属性を有効にする機能フラグ。デフォルトは false です。 |

**応答**&#x200B;正常な応答は、リクエスト本文で指定されたエンティティのリクエストされたフィールドを返します。

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

## ID によるプロファイルの時系列イベントへのアクセス

`/access/entities` エンドポイントに対して GET リクエストを実行すると、関連するプロファイルイベントの ID で時系列エンティティにアクセスできます。この ID は、ID 値（`entityId`）と ID 名前空間（`entityIdNS`）です。

クエリパスに指定されたデータパラメーターで、アクセスするデータを指定します。複数のパラメーターを含め、アンパサンド（&amp;）で区切ることができます。有効なリストの完全なパラメーターは、付録の「[クエリパラメータ](#query-parameters)」の節に記載されています。

**API 形式**

```http
GET /access/entities?{QUERY_PARAMETERS}
```

**リクエスト**

次のリクエストでは、ID でプロファイルエンティティを検索し、エンティティに関連付けられているすべての時系列イベントの `endUserIDs`、`web`、`channel` プロパティの値を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、リクエストクエリパラメーターで指定された時系列イベントと関連フィールドのページ付けされたリストを返します。

>[!NOTE]
>
>リクエストで上限の 1 が指定（`limit=1`）されたので、以下の応答の `count` は 1 で、1 つのエンティティのみが返されます。

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

### 結果の後続ページへのアクセス

時系列イベントを取得すると、結果はページ付けされます。結果の後続のページがある場合、`_page.next` プロパティには ID が含まれます。また、`_links.next.href` プロパティは次のページを取得するためのリクエスト URI を提供します。結果を取得するには、別の GET リクエストを `/access/entities` エンドポイントに対しておこないますが、必ず `/entities` を指定した URI の値に置き換える必要があります。

>[!NOTE]
>
>リクエストパスで誤って `/entities/` を繰り返さないようにしてください。これは一度だけ `/access/entities?start=...` のように存在するべきです。

**API 形式**

```http
GET /access/{NEXT_URI}
```

| パラメーター | 説明 |
|---|---|
| `{NEXT_URI}` | `_links.next.href` から取得した URI 値。 |

**リクエスト**

次のリクエストでは、`_links.next.href` URI をリクエストパスとして使用して、次のページの結果を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/access/entities?start=c8d11988-6b56-4571-a123-b6ce74236037&orderby=timestamp&schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、結果の次のページを返します。この応答には、`_page.next` および `_links.next.href` の空の文字列値で示される結果の後続ページはありません。

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

## ID による複数プロファイルの時系列イベントへのアクセス

`/access/entities` エンドポイントに対して POST リクエストを実行し、ペイロードでプロファイル ID を提供することで、複数の関連するプロファイルから時系列イベントにアクセスできます。これらの ID は、それぞれ ID 値（`entityId`）と ID 名前空間（`entityIdNS`）で構成されます。

**API 形式**

```http
POST /access/entities
```

**リクエスト**

次のリクエストでは、プロファイル ID のリストに関連付けられた時系列イベントのユーザー ID、現地時間、国コードを取得します。

```shell
curl -X POST \
  https://platform.adobe.io/data/core/ups/access/entities \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
    "limit": 10
}'
```

| プロパティ | 説明 |
|---|---|
| `schema.name` | **（必須）** 取得するエンティティの XDM スキーマ。 |
| `relatedSchema.name` | `schema.name` が `_xdm.context.experienceevent` の場合、この値は、時系列イベントが関連するプロファイルエンティティのスキーマを指定する必要があります。 |
| `identities` | **（必須）** 関連する時系列イベントを取得するプロファイルの配列リスト。配列内の各エントリは、1)ID 値と名前空間で構成される完全修飾 ID を使用する、または 2) XID を提供するという 2 つの方法のいずれかで設定されます。 |
| `fields` | 指定したフィールドのセットに返されたデータを分離します。取得したデータに含まれるスキーマフィールドをフィルターする場合に使用します。例：personalEmail,person.name,person.gender |
| `mergePolicyId` | 返されるデータを制御する結合ポリシーを指定します。サービス呼び出しで指定されていない場合は、組織のデフォルトのスキーマが使用されます。デフォルトの結合ポリシーが設定されていない場合、デフォルトでは、プロファイルの結合と ID の結合はおこなわれません。 |
| `orderby` | 取得したエクスペリエンスイベントをタイムスタンプ別に並べ替える順序です。デフォルトは `+timestamp` で、`(+/-)timestamp` として記述されます。 |
| `timeFilter.startTime` | 時系列オブジェクトのフィルターを開始する時間をミリ秒単位で指定します。 |
| `timeFilter.endTime` | 時系列オブジェクトのフィルターを終了する時間をミリ秒単位で指定します。 |
| `limit` | 返すオブジェクトの最大数を指定する数値。デフォルトは 1000 です。 |
| `withCA` | 参照の計算済み属性を有効にする機能フラグ。デフォルトは false です。 |

**応答** 

正常な応答は、リクエストクエリパラメータで指定された時系列イベントと関連フィールドのページ付けされたリストを返します。

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

この例の応答では、最初にリストされたプロファイル（「GkouAW-yD9aoRCPhRYROJ-TetAFW」）が `_links.next.payload` の値を提供します。つまり、このプロファイルの結果には他のページが含まれます。その他の結果へのアクセス方法について詳しくは、次の「[その他の結果へのアクセス](#access-additional-results)」の節を参照してください。

### その他の結果へのアクセス {#access-additional-results}

時系列のイベントを取得すると、多くの結果が返される場合があるので、結果はページ付けされることがよくあります。特定のプロファイルの結果の後続のページがある場合、そのプロファイルの `_links.next.payload` 値にはペイロードオブジェクトが含まれます。

リクエスト本文でこのペイロードを使用して、追加の POST リクエストを `access/entities` エンドポイントに対して実行し、そのプロファイルの時系列データの後続のページを取得できます。

## 複数スキーマエンティティの時系列イベントへのアクセス

関係記述子を介して接続された複数のエンティティにアクセスできます。次の API 呼び出しの例では、2 つのスキーマ間の関係が既に定義されていると仮定しています。関係記述子の詳細については、『[!DNL Schema Registry] API 開発者ガイド [ 記述子エンドポイントガイド ](../../xdm/api/descriptors.md)』を参照してください。

リクエストパスにクエリパラメーターを含めて、アクセスするデータを指定できます。複数のパラメーターを含め、アンパサンド（&amp;）で区切ることができます。有効なリストの完全なパラメーターは、付録の「[クエリパラメータ](#query-parameters)」の節に記載されています。

**API 形式**

```http
GET /access/entities?{QUERY_PARAMETERS}
```

**リクエスト**

次のリクエストは、異なるエンティティ間の情報にアクセスするために、以前に確立された関係記述子を含むスキーマを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/access/entities?relatedSchema.name=_xdm.context.profile&schema.name=_xdm.context.experienceevent&relatedEntityId=GkouAW-2Xkftzer3bBtHiW8GkaFL \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答** 

正常な応答は、複数のエンティティに関連付けられた時系列イベントのページ付けされたリストを返します。

```json
{
    "_page": {
        "orderby": "timestamp",
        "start": "cb10369f-a47b-4e65-afb4-06e1ad78a648",
        "count": 1,
        "next": ""
    },
    "children": [
        {
            "relatedEntityId": "GkouAW-2Xkftzer3bBtHiW8GkaFL",
            "entityId": "cb10369f-a47b-4e65-afb4-06e1ad78a648",
            "timestamp": 1564614939000,
            "entity": {
                "environment": {
                    "browserDetails": {}
                },
                "identityMap": {
                    "CRMId": [
                        {
                            "id": "78520026455138218785449796480922109723",
                            "primary": true
                        }
                    ]
                },

                "commerce": {
                    "productViews": {
                        "value": 1
                    }
                },
                "productListItems": [
                    {
                        "name": "Red shoe",
                        "quantity": 85,
                        "storesAvailableIn": [
                            "da6dced5-9574-4dda-89b5-9dc106903f80",
                            "981bb433-2ee5-4db0-a19a-449ec9dbf39f"
                        ],
                        "SKU": "8f998279-797b-4da2-9e60-88bf73a9f15a",
                        "priceTotal": 934.8
                    }
                ],
                "_id": "cb10369f-a47b-4e65-afb4-06e1ad78a648",
                "commerce": {
                    "order": {}
                },
                "placeContext": {
                    "geo": {
                        "_schema": {}
                    }
                },
                "device": {},
                "timestamp": "2019-07-31T23:15:39Z",
                "_experience": {
                    "profile": {
                        "identityNamespaces": {
                            "/productListItems[*]/SKU": {
                                "namespace": {
                                    "code": "ECID"
                                }
                            }
                        }
                    }
                }
            },
            "lastModifiedAt": "2019-10-10T00:14:19Z"
        }
    ],
    "_links": {
        "next": {
            "href": ""
        }
    }
}
```

### 結果の後続ページへのアクセス

時系列イベントを取得すると、結果はページ付けされます。結果の後続のページがある場合、`_page.next` プロパティには ID が含まれます。また、`_links.next.href` プロパティは、`access/entities` エンドポイントに対して追加の GET リクエストを実行することで後続のページを取得するためのリクエスト URI を提供します。

## 次の手順

このガイドに従うことで、[!DNL Real-time Customer Profile] データフィールド、プロファイル、時系列データに正常にアクセスできました。 [!DNL Platform] に格納されている他のデータリソースにアクセスする方法については、「[ データアクセスの概要 ](../../data-access/home.md)」を参照してください。

## 付録 {#appendix}

次の節では、API を使用した [!DNL Profile] データへのアクセスに関する補足情報を示します。

### クエリパラメーター {#query-parameters}

次のパラメーターは、`/access/entities` エンドポイントに対する GET リクエストのパスで使用されます。アクセスするプロファイルエンティティを識別し、応答で返されるデータをフィルターします。必須パラメーターはラベル付けされますが、残りはオプションです。

| パラメーター | 説明 | 例 |
|---|---|---|
| `schema.name` | **（必須）** 取得するエンティティの XDM スキーマ。 | `schema.name=_xdm.context.experienceevent` |
| `relatedSchema.name` | `schema.name` が「_xdm.context.experienceevent」の場合、この値は時系列イベントが関連するプロファイルエンティティのスキーマを指定する必要があります。 | `relatedSchema.name=_xdm.context.profile` |
| `entityId` | **（必須）** エンティティの ID。このパラメーターの値が XID でない場合は、ID 名前空間パラメーターも指定する必要があります（以下の `entityIdNS` 参照）。 | `entityId=janedoe@example.com` |
| `entityIdNS` | `entityId` が XID として指定されていない場合、このフィールドで ID 名前空間の指定が必要です。 | `entityIdNE=email` |
| `relatedEntityId` | `schema.name` が「_xdm.context.experienceevent」の場合、この値は関連するプロファイルエンティティの ID 名前空間を指定する必要があります。この値は、`entityId` と同じ規則に従います 。 | `relatedEntityId=69935279872410346619186588147492736556` |
| `relatedEntityIdNS` | `schema.name` が「_xdm.context.experienceevent」の場合、この値は `relatedEntityId` で指定したエンティティの ID 名前空間を指定する必要があります。 | `relatedEntityIdNS=CRMID` |
| `fields` | 応答で返されるデータをフィルターします。取得したデータに含めるスキーマフィールドの値を指定します。複数のフィールドの場合、値はコンマで区切り、間にスペースは入れません | `fields=personalEmail,person.name,person.gender` |
| `mergePolicyId` | 返されるデータを制御する結合ポリシーを指定します。呼び出しで指定されていない場合は、組織のデフォルトのスキーマが使用されます。デフォルトの結合ポリシーが設定されていない場合、デフォルトでは、プロファイルの結合と ID の結合はおこなわれません。 | `mergePoilcyId=5aa6885fcf70a301dabdfa4a` |
| `orderBy` | 取得したエクスペリエンスイベントをタイムスタンプ別に並べ替える順序です。デフォルトは `+timestamp` で、`(+/-)timestamp` として記述されます。 | `orderby=-timestamp` |
| `startTime` | 時系列オブジェクトのフィルターを開始する時間をミリ秒単位で指定します。 | `startTime=1539838505` |
| `endTime` | 時系列オブジェクトのフィルターを終了する時間をミリ秒単位で指定します。 | `endTime=1539838510` |
| `limit` | 返すオブジェクトの最大数を指定する数値。デフォルトは 1000 です。 | `limit=100` |
| `property` | プロパティ値でフィルターします。 次の評価演算子をサポートします。=、!=、&lt;、&lt;=、>、>=。 エクスペリエンスイベントでのみ使用でき、最大 3 つのプロパティがサポートされます。 | `property=webPageDetails.isHomepage=true&property=localTime<="2020-07-20"` |
| `withCA` | 参照の計算済み属性を有効にする機能フラグ。デフォルトは false です。 | `withCA=true` |
