---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイムの顧客プロファイルAPI開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: 95e002c60389ca7e4c1dcf32bbcf6f552cd55d95
workflow-type: tm+mt
source-wordcount: '1697'
ht-degree: 1%

---


# エンティティ(プロファイルアクセス)

Adobe Experience Platformを使用すると、RESTful APIまたはユーザーインターフェイスを使用して、リアルタイム顧客プロファイルデータにアクセスできます。 このガイドでは、APIを使用して、一般に「プロファイル」と呼ばれるエンティティにアクセスする方法について概説します。 プラットフォームUIを使用したプロファイルデータへのアクセスについて詳しくは、 [プロファイルユーザーガイドを参照してください](../ui/user-guide.md)。

## はじめに

このガイドで使用されるAPIエンドポイントは、リアルタイム顧客プロファイルAPIの一部です。 先に進む前に、 [Real-time Customer Customer API開発者ガイドを参照してください](getting-started.md)。

特に、プロファイル開発ガイドの「 [はじめに](getting-started.md#getting-started) 」の節には、関連トピックへのリンク、このドキュメントのサンプルAPI呼び出しを読むためのガイド、Experience Platform APIの呼び出しを成功させるために必要なヘッダーに関する重要な情報が含まれています。

## ID別のプロファイルデータへのアクセス

エンドポイントにGETリクエストを送信し、一連のクエリパラメーターとしてエンティティのIDを指定することで、プロファイルエンティティにアクセスでき `/access/entities` ます。 このIDは、ID値(`entityId`)とID名前空間(`entityIdNS`)で構成されます。

リクエストパスで指定されるクエリパラメーターで、アクセスするデータを指定します。 複数のパラメーターを含めることができ、アンパサンド(&amp;)で区切って指定できます。 有効なパラメーターの完全なリストは、付録の「 [クエリパラメーター](#query-parameters) 」セクションに記載されています。

**API形式**

```http
GET /access/entities?{QUERY_PARAMETERS}
```

**リクエスト**

次のリクエストは、IDを使用して顧客の電子メールと名前を取得します。

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
>関連するグラフが50個を超えるIDをリンクしている場合、このサービスはHTTPステータス422と「関連IDが多すぎます」というメッセージを返します。 このエラーが表示される場合は、検索を絞り込むためにクエリパラメーターを追加することを検討してください。

## IDのリストによるプロファイルデータへのアクセス

エンドポイントにPOSTリクエストを送信し、ペイロードにIDを提供することで、IDによって複数のプロファイルエンティティにアクセスでき `/access/entities` ます。 これらのIDは、ID値(`entityId`)とID名前空間(`entityIdNS`)で構成されます。

**API形式**

```http
POST /access/entities
```

**リクエスト**

次のリクエストは、IDのリストによって複数の顧客の名前と電子メールアドレスを取得します。

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
| `schema.name` | ***（必須）*** エンティティが属するXDMスキーマの名前。 |
| `fields` | 返すXDMフィールドを文字列の配列として指定します。 デフォルトでは、すべてのフィールドが返されます。 |
| `identities` | ***（必須）*** アクセスするエンティティのIDのリストを含む配列。 |
| `identities.entityId` | アクセスするエンティティのID。 |
| `identities.entityIdNS.code` | アクセスするエンティティIDの名前空間。 |
| `timeFilter.startTime` | 時間範囲フィルターの開始時間（含む）。 精度はミリ秒にする必要があります。 指定しなかった場合、デフォルトは使用可能な時間の開始です。 |
| `timeFilter.endTime` | 時間範囲フィルターの終了時間、除外。 精度はミリ秒にする必要があります。 指定しなかった場合、デフォルトは使用可能な時間の終わりです。 |
| `limit` | 返すレコード数。 返されるエクスペリエンスイベントの数にのみ適用されます。 デフォルト： 1,000。 |
| `orderby` | 取得したエクスペリエンスイベントをタイムスタンプ別に並べ替える順序で、デフォルト値 `(+/-)timestamp` と同様に記述され `+timestamp`ます。 |
| `withCA` | 参照用に計算済み属性を有効にする機能フラグ。 デフォルト： false。 |

**応答**&#x200B;成功応答は、要求本文に指定されたエンティティの要求されたフィールドを返します。

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

## ID別のプロファイルの時系列イベントへのアクセス

エンドポイントにGET要求を行うと、関連するプロファイルエンティティのIDで時系列イベントにアクセスでき `/access/entities` ます。 このIDは、ID値(`entityId`)とID名前空間(`entityIdNS`)で構成されます。

リクエストパスで指定されるクエリパラメーターで、アクセスするデータを指定します。 複数のパラメーターを含めることができ、アンパサンド(&amp;)で区切って指定できます。 有効なパラメーターの完全なリストは、付録の「 [クエリパラメーター](#query-parameters) 」セクションに記載されています。

**API形式**

```http
GET /access/entities?{QUERY_PARAMETERS}
```

**リクエスト**

次の要求では、IDでプロファイルエンティティを検索し、そのエンティティに関連付けられているすべての時系列イベントのプロパティ `endUserIDs`、 `web`および値 `channel` を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、リクエストクエリーのパラメーターで指定された時系列イベントおよび関連するフィールドのページ分割リストを返します。

>[!NOTE]
>リクエストで制限が1(`limit=1`)に指定されているので、以下の応答 `count` 内のエンティティは1で、1つのエンティティのみが返されます。

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

### 結果の後続のページにアクセスする

時系列のイベントを取得すると、結果がページ分割されます。 結果の後続のページがある場合、 `_page.next` プロパティにはIDが含まれます。 また、この `_links.next.href` プロパティは次のページを取得するための要求URIを提供します。 結果を取得するには、別のGETリクエストを `/access/entities` エンドポイントに対して行いますが、必ず、指定したURIの値 `/entities` に置き換える必要があります。

>[!NOTE]
>リクエストパスで誤って繰り返しを行わないよう `/entities/` にしてください。 このように見えるのは一度だけ `/access/entities?start=...`

**API形式**

```http
GET /access/{NEXT_URI}
```

| パラメーター | 説明 |
|---|---|
| `{NEXT_URI}` | から取得されるURI値 `_links.next.href`。 |

**リクエスト**

次のリクエストは、 `_links.next.href` URIをリクエストパスとして使用して、次のページの結果を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/access/entities?start=c8d11988-6b56-4571-a123-b6ce74236037&orderby=timestamp&schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、結果の次のページを返します。 この応答には、 `_page.next` およびの空の文字列値で示されるように、後続のページの結果はありません `_links.next.href`。

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

## ID別の複数のプロファイルの時系列イベントへのアクセス

エンドポイントにPOSTリクエストを行い、ペイロードでプロファイルIDを提供することで、複数の関連プロファイルから時系列イベントにアクセスでき `/access/entities` ます。 これらのIDは、それぞれID値(`entityId`)とID名前空間(`entityIdNS`)で構成されます。

**API形式**

```http
POST /access/entities
```

**リクエスト**

次のリクエストは、プロファイルIDのリストに関連付けられた時系列イベントのユーザーID、ローカル時間、および国コードを取得します。

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
| `schema.name` | **（必須）** 取得するエンティティのXDMスキーマ |
| `relatedSchema.name` | が `schema.name``_xdm.context.experienceevent` この値の場合は、時系列イベントが関連付けられるプロファイルエンティティのスキーマを指定する必要があります。 |
| `identities` | **（必須）** 関連する時系列イベントを取得するプロファイルの配列。 配列の各エントリは、次の2つの方法のいずれかで設定されます。 (1) XIDを提供するID値と名前空間、または2)から成る完全修飾IDを使用する。 |
| `fields` | 指定した一連のフィールドに返されたデータを抽出します。 取得したデータに含まれるスキーマフィールドをフィルタリングする場合に使用します。 例： personalEmail,person.name,person.gender |
| `mergePolicyId` | 返されるデータを制御するマージポリシーを指定します。 サービス呼び出しで指定されていない場合は、そのスキーマのデフォルトが使用されます。 デフォルトの結合ポリシーが設定されていない場合、デフォルトでは、プロファイルの結合とIDのステッチは行われません。 |
| `orderby` | 取得したエクスペリエンスイベントをタイムスタンプ別に並べ替える順序で、デフォルト値 `(+/-)timestamp` と同様に記述され `+timestamp`ます。 |
| `timeFilter.startTime` | 時系列オブジェクトをフィルターする開始時間を指定します（ミリ秒）。 |
| `timeFilter.endTime` | 時系列オブジェクトをフィルターする終了時間を指定します（ミリ秒）。 |
| `limit` | 返すオブジェクトの最大数を指定する数値。 デフォルト： 1000 |
| `withCA` | 参照用に計算済み属性を有効にする機能フラグ。 デフォルト： false |

**応答**

成功した応答は、リクエストで指定された複数のプロファイルに関連付けられた時系列イベントのページ分割リストを返します。

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

この例の応答では、最初にリストされたプロファイル(「GkouAW-yD9aoRCPhRYROJ-TetAFW」)がの値を提供します。つまり、このプロファイルには他のページも結果が存在します。 `_links.next.payload`その他の結果へのアクセス方法の詳細については、 [追加の結果へのアクセスに関する次の節を参照してください](#access-additional-results) 。

### その他の結果へのアクセス {#access-additional-results}

時系列のイベントを取得する場合、多くの結果が返される可能性があるので、結果は多くの場合ページ分割されます。 特定のプロファイルの結果の後続のページがある場合、そのプロファイルの `_links.next.payload` 値にはペイロードオブジェクトが含まれます。

リクエスト本文でこのペイロードを使用して、エンドポイントに対して追加のPOSTリクエストを実行し、そのプロファイルの時系列データの後続のページを取得でき `access/entities` ます。

## 複数のスキーマエンティティの時系列イベントへのアクセス

関係記述子を介して接続された複数のエンティティにアクセスできます。 次のAPI呼び出しの例では、2つのスキーマ間で既に関係が定義されていると仮定しています。 リレーションシップ記述子の詳細については、スキーマレジストリAPI開発者ガイド [記述子のサブガイド]](../../xdm/api/descriptors.md)を参照してください。

リクエストパスにクエリパラメーターを含めて、アクセスするデータを指定できます。 複数のパラメーターを含めることができ、アンパサンド(&amp;)で区切って指定できます。 有効なパラメーターの完全なリストは、付録の「 [クエリパラメーター](#query-parameters) 」セクションに記載されています。

**API形式**

```http
GET /access/entities?{QUERY_PARAMETERS}
```

**リクエスト**

次のリクエストは、異なるスキーマ間で情報にアクセスするために、以前に確立された関係記述子を含むエンティティを取得します。

```shell
curl -X GET \
  https://platform.adobe.io/data/core/ups/access/entities?relatedSchema.name=_xdm.context.profile&schema.name=_xdm.context.experienceevent&relatedEntityId=GkouAW-2Xkftzer3bBtHiW8GkaFL \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
```

**応答**

成功した応答は、複数のエンティティに関連付けられた時系列イベントのページ分割リストを返します。

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

### 結果の後続のページにアクセスする

時系列のイベントを取得すると、結果がページ分割されます。 結果の後続のページがある場合、 `_page.next` プロパティにはIDが含まれます。 さらに、この `_links.next.href` プロパティは、エンドポイントに追加のGET要求を行って後続のページを取得するための要求URIを提供し `access/entities` ます。

## 次の手順

このガイドに従うと、リアルタイム顧客プロファイルデータフィールド、プロファイルおよび時系列データに正常にアクセスできます。 プラットフォームに保存されている他のデータリソースにアクセスする方法については、「 [データアクセスの概要](../../data-access/home.md)」を参照してください。

## 付録 {#appendix}

次の節では、APIを使用したプロファイルデータへのアクセスに関する補足情報を説明します。

### クエリパラメーター {#query-parameters}

エンドポイントへのGET要求のパスには、次のパラメーターが使用され `/access/entities` ます。 アクセスするプロファイルエンティティを識別し、応答で返されるデータをフィルタリングします。 必須のパラメーターにはラベルが付けられますが、残りのパラメーターはオプションです。

| パラメーター | 説明 | 例 |
|---|---|---|
| `schema.name` | **（必須）** 取得するエンティティのXDMスキーマ | `schema.name=_xdm.context.experienceevent` |
| `relatedSchema.name` | が「_xdm.context.experienceevent」 `schema.name` の場合、この値では、時系列イベントが関連付けられているプロファイルエンティティのスキーマを指定する必要があります。 | `relatedSchema.name=_xdm.context.profile` |
| `entityId` | **（必須）** エンティティのID。 このパラメーターの値がXIDでない場合は、ID名前空間パラメーターも指定する必要があります(以下を参照 `entityIdNS` )。 | `entityId=janedoe@example.com` |
| `entityIdNS` | がXIDとして指定され `entityId` ない場合は、ID名前空間を指定する必要があります。 | `entityIdNE=email` |
| `relatedEntityId` | が「_xdm.context.experienceevent」の場合、この値 `schema.name` には、関連するプロファイルエンティティのID名前空間を指定する必要があります。 この値は、と同じ規則に従い `entityId`ます。 | `relatedEntityId=69935279872410346619186588147492736556` |
| `relatedEntityIdNS` | が&quot;_xdm.context.experienceevent&quot;の場合、この値 `schema.name` にはで指定したエンティティのID名前空間を指定する必要があり `relatedEntityId`ます。 | `relatedEntityIdNS=CRMID` |
| `fields` | 応答で返されるデータをフィルターします。 取得したデータに含めるスキーマフィールドの値を指定する場合に使用します。 複数のフィールドの場合、値をコンマで区切り、間にスペースを入れません | `fields=personalEmail,person.name,person.gender` |
| `mergePolicyId` | 返されるデータを制御するマージポリシーを指定します。 呼び出しで指定されていない場合は、そのスキーマのデフォルトが使用されます。 デフォルトの結合ポリシーが設定されていない場合、デフォルトでは、プロファイルの結合とIDのステッチは行われません。 | `mergePoilcyId=5aa6885fcf70a301dabdfa4a` |
| `orderBy` | 取得したエクスペリエンスイベントをタイムスタンプ別に並べ替える順序で、デフォルト値 `(+/-)timestamp` と同様に記述され `+timestamp`ます。 | `orderby=-timestamp` |
| `startTime` | 時系列オブジェクトをフィルターする開始時間を指定します（ミリ秒）。 | `startTime=1539838505` |
| `endTime` | 時系列オブジェクトをフィルターする終了時間を指定します（ミリ秒）。 | `endTime=1539838510` |
| `limit` | 返すオブジェクトの最大数を指定する数値。 デフォルト： 1000 | `limit=100` |
| `withCA` | 参照用に計算済み属性を有効にする機能フラグ。 デフォルト： false | `withCA=true` |