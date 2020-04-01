---
keywords: Experience Platform;profile;real-time customer profile;troubleshooting;API
solution: Adobe Experience Platform
title: リアルタイム顧客プロファイルAPI開発ガイド
topic: guide
translation-type: tm+mt
source-git-commit: 95e002c60389ca7e4c1dcf32bbcf6f552cd55d95

---


# エンティティ(プロファイルアクセス)

Adobe Experience Platformを使用すると、RESTful APIまたはユーザーインターフェイスを使用して、リアルタイムの顧客プロファイルデータにアクセスできます。 このガイドでは、APIを使用してエンティティ(一般に「プロファイル」)にアクセスする方法について説明します。 プラットフォームUIを使用したプロファイルデータへのアクセスについて詳しくは、 [プロファイルユーザーガイドを参照してくださ](../ui/user-guide.md)い。

## はじめに

このガイドで使用されるAPIエンドポイントは、リアルタイム顧客プロファイルAPIの一部です。 先に進む前に、リアルタイム顧客 [プロファイルAPI開発ガイドを参照してください](getting-started.md)。

特に、プロファイル開発ガ [イドの](getting-started.md#getting-started) 「はじめに」の節には、関連トピックへのリンク、このドキュメントのサンプルAPI呼び出しの読み方のガイド、Experience Platform APIを正常に呼び出すために必要なヘッダーに関する重要な情報が含まれています。

## ID別のプロファイルデータへのアクセス

エンドポイントにGETリクエストを送信し、一連のプロファイルパラメーターとし `/access/entities` てエンティティのIDを指定することで、クエリエンティティにアクセスできます。 このIDは、ID値(`entityId`)とID名前空間(`entityIdNS`)です。

クエリパスに指定されたデータパラメーターで、アクセスするデータを指定します。 複数のパラメーターを含め、アンパサンド(&amp;)で区切ることができます。 有効なリストの完全なパラメータは、付録の [クエリ](#query-parameters) ・パラメータの節に記載されています。

**API形式**

```http
GET /access/entities?{QUERY_PARAMETERS}
```

**リクエスト**

次のリクエストは、顧客の電子メールと名前をIDを使用して取得します。

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
>関連するグラフが50個を超えるIDをリンクする場合、このサービスはHTTPステータス422と「関連IDが多すぎます」というメッセージを返します。 このエラーが表示される場合は、検索を絞り込むためにクエリパラメータを追加することを検討してください。

## IDのプロファイル別にリストデータにアクセス

エンドポイントにPOSTリクエストを送信し、ペイロードにIDを提供することで、ID `/access/entities` 別に複数のプロファイルエンティティにアクセスできます。 これらのIDは、ID値(`entityId`)とID名前空間(`entityIdNS`)で構成されます。

**API形式**

```http
POST /access/entities
```

**リクエスト**

次のリクエストは、複数の顧客の名前と電子メールアドレスをIDのリストで取得します。

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
| `schema.name` | ***（必須）*** 、エンティティが属するXDMスキーマの名前。 |
| `fields` | 返されるXDMフィールドを文字列の配列で指定します。 デフォルトでは、すべてのフィールドが返されます。 |
| `identities` | ***（必須）*** 、アクセスするエンティティのIDのリストを含む配列。 |
| `identities.entityId` | アクセスするエンティティのID。 |
| `identities.entityIdNS.code` | アクセスする名前空間IDのエンティティ。 |
| `timeFilter.startTime` | 開始範囲フィルターの時間（含む）。 精度はミリ秒にする必要があります。 指定しなかった場合、デフォルトは使用可能な時間の開始です。 |
| `timeFilter.endTime` | 時間範囲フィルターの終了時間（除外）。 精度はミリ秒にする必要があります。 指定しなかった場合、デフォルトは使用可能な時間の終わりです。 |
| `limit` | 返すレコードの数。 返されたエクスペリエンスのイベント数にのみ適用。 デフォルト：1,000。 |
| `orderby` | 取得したエクスペリエンスイベントをタイムスタンプ別に並べ替える順序で、デフォ `(+/-)timestamp` ルトの設定として記述されま `+timestamp`す。 |
| `withCA` | 参照の計算済み属性を有効にする機能フラグ。 デフォルト：false。 |

**応答**&#x200B;成功応答は、要求されたエンティティの要求されたフィールドを要求本文に指定して返します。

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

## IDによるプロファイルの時系列イベントへのアクセス

エンドポイントにGET要求を行うと、関連するプロファイルイベントのIDで時系列エンティティにアクセスで `/access/entities` きます。 このIDは、ID値(`entityId`)とID名前空間(`entityIdNS`)です。

クエリパスに指定されたデータパラメーターで、アクセスするデータを指定します。 複数のパラメーターを含め、アンパサンド(&amp;)で区切ることができます。 有効なリストの完全なパラメータは、付録の [クエリ](#query-parameters) ・パラメータの節に記載されています。

**API形式**

```http
GET /access/entities?{QUERY_PARAMETERS}
```

**リクエスト**

次の要求では、IDでプロファイルエンティティを検索し、プロパティ、およびエンティティに関連付けられ `endUserIDs`ているす `web`べての `channel` 時系列イベントの値を取得します。

```shell
curl -X GET \
  'https://platform.adobe.io/data/core/ups/access/entities?schema.name=_xdm.context.experienceevent&relatedSchema.name=_xdm.context.profile&relatedEntityId=89149270342662559642753730269986316900&relatedEntityIdNS=ECID&fields=endUserIDs,web,channel&startTime=1531260476000&endTime=1531260480000&limit=1' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、リクエストのリストーで指定された時系列イベントと関連フィールドのページ分割されたクエリを返します。

>[!NOTE]
>リクエストに1つの制限(`limit=1`)が指定されたので、以下の応 `count` 答内のエンティティは1で、1つのエンティティのみが返されます。

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

時系列のイベントを取得すると、結果がページ分割されます。 結果の後続のページがある場合、プロパティ `_page.next` にはIDが含まれます。 また、このプロパ `_links.next.href` ティは次のページを取得するための要求URIを提供します。 結果を取得するには、別のGETリクエストをエンドポイ `/access/entities` ントに対して行いますが、必ず指定したURIの値 `/entities` に置き換える必要があります。

>[!NOTE]
>リクエストパスで誤って繰り返さないよ `/entities/` うにしてください。 これは一度だけ `/access/entities?start=...`

**API形式**

```http
GET /access/{NEXT_URI}
```

| パラメーター | 説明 |
|---|---|
| `{NEXT_URI}` | 取得したURI値 `_links.next.href`。 |

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

成功した応答は、結果の次のページを返します。 この応答には、およびの空の文字列値で示される結果の後続ページはあり `_page.next` ません `_links.next.href`。

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

## ID別の複数のイベントの時系列プロファイルへのアクセス

エンドポイントにPOSTリクエストを送信し、ペイロードでプロファイルIDを提供することで、複数の関連する `/access/entities` プロファイルから時系列イベントにアクセスできます。 これらのIDは、それぞれID値(`entityId`)とID名前空間(`entityIdNS`)で構成されます。

**API形式**

```http
POST /access/entities
```

**リクエスト**

次のリクエストは、ユーザーIDのリストに関連付けられた時系列イベントのユーザーID、ローカル時間および国コードを取得します。

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
| `schema.name` | **(REQUIRED)** 取得するエンティティのXDMスキーマ |
| `relatedSchema.name` | がの場 `schema.name` 合、こ `_xdm.context.experienceevent` の値では、時系列プロファイルが関連するイベントエンティティのスキーマを指定する必要があります。 |
| `identities` | **（必須）関連する時系列** プロファイルを取得するイベントの配列。 配列内の各エントリは、次の2つの方法のいずれかで設定されます。1) XIDを提供するID値と名前空間、または2から成る完全修飾IDを使用する。 |
| `fields` | 指定したフィールドのセットに返されたデータを分離します。 取得したデータに含まれるスキーマフィールドをフィルタリングする場合に使用します。 例：personalEmail,person.name,person.gender |
| `mergePolicyId` | 返されるデータを制御するマージポリシーを指定します。 サービス呼び出しで指定されていない場合は、組織のデフォルトのスキーマが使用されます。 デフォルトの結合ポリシーが設定されていない場合、デフォルトでは、プロファイルの結合とIDのステッチは行われません。 |
| `orderby` | 取得したエクスペリエンスイベントをタイムスタンプ別に並べ替える順序で、デフォ `(+/-)timestamp` ルトの設定として記述されま `+timestamp`す。 |
| `timeFilter.startTime` | 時系列オブジェクトを開始するフィルター時間をミリ秒単位で指定します。 |
| `timeFilter.endTime` | 時系列オブジェクトをフィルタリングする終了時間をミリ秒単位で指定します。 |
| `limit` | 返すオブジェクトの最大数を指定する数値。 デフォルト：1000 |
| `withCA` | 参照の計算済み属性を有効にする機能フラグ。 デフォルト：false |

**応答**

成功した応答は、リクエストで指定された複数のリストに関連付けられた時系列イベントのページ分割プロファイルを返します。

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

この例の応答では、最初にリストされたプロファイル(「GkouAW-yD9aoRCPhRYROJ-TetAFW」)が値を提供します。つまり、このプロファイルの `_links.next.payload`結果には他のページが含まれます。 その他の結果へのアクセス方法につ [いて詳しくは](#access-additional-results) 、次のセクションを参照してください。

### その他の結果へのアクセス {#access-additional-results}

時系列のイベントを取得すると、多くの結果が返される場合があるので、結果はページ分割されることがよくあります。 特定のプロファイルの結果の後続のページがある場合、そのプロファイルの値 `_links.next.payload` にはペイロードオブジェクトが含まれます。

リクエスト本文でこのペイロードを使用して、追加のPOSTリクエストをエンドポイントに対して実行し、その `access/entities` プロファイルの時系列データの後続のページを取得できます。

## 複数のエンティティの時系列イベントにアクセスするスキーマ

関係記述子を介して接続された複数のエンティティにアクセスできます。 次のAPI呼び出しの例では、2つの関係が既に定義されていると仮定しています。スキーマ 関係記述子の詳細については、スキーマレジストリAPI開発者ガイド記述子 [サブガイド]](../../xdm/api/descriptors.md)を参照してください。

リクエストパスにクエリパラメーターを含めて、アクセスするデータを指定できます。 複数のパラメーターを含め、アンパサンド(&amp;)で区切ることができます。 有効なリストの完全なパラメータは、付録の [クエリ](#query-parameters) ・パラメータの節に記載されています。

**API形式**

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

成功した応答は、複数のエンティティに関連付けられた時系列リストのページ分割イベントを返します。

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

時系列のイベントを取得すると、結果がページ分割されます。 結果の後続のページがある場合、プロパティ `_page.next` にはIDが含まれます。 また、このプロパ `_links.next.href` ティは、エンドポイントに追加のGET要求を行うことで後続のページを取得するための要求URIを提供 `access/entities` します。

## 次の手順

このガイドに従うと、リアルタイム顧客プロファイルデータフィールド、プロファイル、時系列データに正常にアクセスできます。 プラットフォームに保存されている他のデータリソースにアクセスする方法については、「データアクセスの概要」 [を参照してくださ](../../data-access/home.md)い。

## 付録 {#appendix}

次の節では、APIを使用したプロファイルデータへのアクセスに関する補足情報を示します。

### クエリパラメータ {#query-parameters}

次のパラメーターは、エンドポイントへのGET要求のパスで使用さ `/access/entities` れます。 アクセスするプロファイルエンティティを識別し、応答で返されるデータをフィルタリングします。 必須パラメーターはラベル付けされますが、残りはオプションです。

| パラメーター | 説明 | 例 |
|---|---|---|
| `schema.name` | **(REQUIRED)** 取得するエンティティのXDMスキーマ | `schema.name=_xdm.context.experienceevent` |
| `relatedSchema.name` | が「 `schema.name` _xdm.context.experienceevent」の場合、この値は時系列イベントが関連するプロファイルエンティティのスキーマを指定する必要があります。 | `relatedSchema.name=_xdm.context.profile` |
| `entityId` | **（必須）** 、エンティティのID。 このパラメーターの値がXIDでない場合は、ID名前空間パラメーターも指定する必要があります(以下を参 `entityIdNS` 照)。 | `entityId=janedoe@example.com` |
| `entityIdNS` | がXIDと `entityId` して指定されていない場合、このフィールドでIDの指定が必要です。名前空間 | `entityIdNE=email` |
| `relatedEntityId` | が「_xdm. `schema.name` context.experienceevent」の場合、この値は関連するプロファイルエンティティのID名前空間を指定する必要があります。 この値は、と同じ規則に従います `entityId`。 | `relatedEntityId=69935279872410346619186588147492736556` |
| `relatedEntityIdNS` | が「_xdm. `schema.name` context.experienceevent」の場合、この値はで指定したエンティティのID名前空間を指定する必要がありま `relatedEntityId`す。 | `relatedEntityIdNS=CRMID` |
| `fields` | フィルターに応答で返されるデータ。 取得したデータに含めるスキーマフィールドの値を指定します。 複数のフィールドの場合、値はコンマで区切り、間にスペースは入れません | `fields=personalEmail,person.name,person.gender` |
| `mergePolicyId` | 返されるデータを制御するマージポリシーを指定します。 呼び出しで指定されていない場合は、組織のデフォルトのスキーマが使用されます。 デフォルトの結合ポリシーが設定されていない場合、デフォルトでは、プロファイルの結合とIDのステッチは行われません。 | `mergePoilcyId=5aa6885fcf70a301dabdfa4a` |
| `orderBy` | 取得したエクスペリエンスイベントをタイムスタンプ別に並べ替える順序で、デフォ `(+/-)timestamp` ルトの設定として記述されま `+timestamp`す。 | `orderby=-timestamp` |
| `startTime` | 時系列オブジェクトを開始するフィルター時間をミリ秒単位で指定します。 | `startTime=1539838505` |
| `endTime` | 時系列オブジェクトをフィルタリングする終了時間をミリ秒単位で指定します。 | `endTime=1539838510` |
| `limit` | 返すオブジェクトの最大数を指定する数値。 デフォルト：1000 | `limit=100` |
| `withCA` | 参照の計算済み属性を有効にする機能フラグ。 デフォルト：false | `withCA=true` |