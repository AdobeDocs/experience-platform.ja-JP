---
keywords: Experience Platform;プロファイル;リアルタイム顧客プロファイル;トラブルシューティング;API
title: エンティティ（プロファイルアクセス） API エンドポイント
type: Documentation
description: Adobe Experience Platformでは、RESTful API またはユーザーインターフェイスを使用して、リアルタイム顧客プロファイルデータにアクセスできます。 このガイドでは、Profile API を使用してエンティティ（一般的には「プロファイル」と呼ばれます）にアクセスする方法について説明します。
role: Developer
exl-id: 06a1a920-4dc4-4468-ac15-bf4a6dc885d4
source-git-commit: 17bd3494c2d9b2a05ca86903297ebec85c9350f2
workflow-type: tm+mt
source-wordcount: '2290'
ht-degree: 28%

---

# エンティティエンドポイント （プロファイルアクセス）

>[!IMPORTANT]
>
>Real-Time CDP Ultimateがある場合は **これらのエンドポイントを** のみ）使用できます。
>
>Real-Time CDP Primeがある場合、引き続き、パーソナライゼーションのユースケースにエクスペリエンスイベントを取り込んで使用し、Experience Platform UI 内でイベントを表示できますが、API を使用してプログラムによってエクスペリエンスイベントを検索することはできま **ん**。
>
>Real-Time CDP Ultimateがあり、現在プログラムによってイベントを検索し **い** 場合は、Adobe カスタマーケアに連絡してこの機能を有効にしてもらってください。

Adobe Experience Platformでは、RESTful API またはユーザーインターフェイスを使用して、[!DNL Real-Time Customer Profile] データにアクセスできます。 このガイドでは、API を使用してエンティティ（より一般的には「プロファイル」として知られています）にアクセスする方法について説明します。[!DNL Experience Platform] UI を使用したプロファイルへのアクセスについて詳しくは、[ プロファイルユーザーガイド ](../ui/user-guide.md) を参照してください。

## はじめに

このガイドで使用する API エンドポイントは、[[!DNL Real-Time Customer Profile API]](https://www.adobe.com/go/profile-apis-en) の一部です。先に進む前に、[はじめる前に](getting-started.md)のガイドを参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の [!DNL Experience Platform] API の呼び出しを成功させるのに必要なヘッダーに関する重要な情報を確認してください。

## エンティティの解決

アーキテクチャのアップグレードの一環として、Adobeでは、最新のデータに基づく決定論的な ID マッチングを使用して、アカウントとオポチュニティのエンティティ解決を導入しています。 エンティティ解決ジョブは、B2B 属性を持つマルチエンティティオーディエンスを評価する前に、バッチセグメント化中に毎日実行されます。

この機能強化により、Experience Platformでは同じエンティティを表す複数のレコードを識別して統合できるので、データの一貫性が向上し、より正確なオーディエンスのセグメント化が可能になります。

以前は、アカウントとオポチュニティは、ID グラフベースの解決に依存しており、過去のすべての取り込み履歴を含め、ID を接続していました。 新しいエンティティ解決アプローチでは、ID は最新のデータのみに基づいてリンクされます。

- アカウントとオポチュニティは、時間の優先順位に基づくマージによって解決されます。
   - アカウント：`b2b_account` 名前空間を使用する ID
   - オポチュニティ：`b2b_opportunity` 名前空間を使用する ID。
- その他のエンティティはすべて単に統合され、プライマリ ID の重複のみが、時間の優先順位に基づくマージとマージされます。

>[!NOTE]
>
>エンティティの解決でサポートされているのは `b2b_account` と `b2b_opportunity` だけです。 他の名前空間の ID は、エンティティの解決には使用されません。 カスタム名前空間を使用している場合、アカウントとオポチュニティを見つけることができません。

### エンティティの解決はどのように機能しますか？

- **以前**：データユニバーサル番号付けシステム（DUNS）番号が追加 ID として使用され、アカウントの DUNS 番号が CRM などのソースシステムで更新された場合、アカウント ID は古い DUNS 番号と新しい DUNS 番号の両方にリンクされます。
- **後**:DUNS 番号が追加 ID として使用され、アカウントの DUNS 番号が CRM などのソースシステムで更新された場合、アカウント ID は新しい DUNS 番号にのみリンクされ、アカウントの現在の状態がより正確に反映されます。

このアップデートの結果、[!DNL Profile Access] API は、エンティティ解決ジョブサイクルが完了した後の、最新の結合プロファイルビューを反映するようになりました。 さらに、一貫性のあるデータにより、セグメント化、アクティブ化、分析などのユースケースの精度と一貫性が向上します。

## エンティティの取得 {#retrieve-entity}

>[!IMPORTANT]
>
>API を介したルックアップリクエストで、B2B エンティティ **アカウントと人物の関係、商談と人物の関係、キャンペーン、キャンペーンメンバー、マーケティングリスト、マーケティングリストメンバー** がサポートされなくなりました。
>
>これらのエンティティのサポートは非推奨（廃止予定）になりました。 これらのエンティティへのアクセスに依存する統合またはワークフローが既にある場合は、引き続き機能できるようにするために、サポートされているエンティティタイプを使用するように統合またはワークフローを更新してください。

プロファイルエンティティを取得するには、必要なクエリパラメーターと共に `/access/entities` エンドポイントに対してGET リクエストを行います。

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

さらに、次のクエリパラメーターの使用を *強く* 推奨します。

- `mergePolicyId`: データのフィルタリングに使用する結合ポリシーの ID。 結合ポリシーが指定されていない場合、組織のデフォルトの結合ポリシーが使用されます。

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

さらに、次のクエリパラメーターの使用を *強く* 推奨します。

- `mergePolicyId`: データのフィルタリングに使用する結合ポリシーの ID。 結合ポリシーが指定されていない場合、組織のデフォルトの結合ポリシーが使用されます。

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

さらに、次のクエリパラメーターの使用を *強く* 推奨します。

- `mergePolicyId`: データのフィルタリングに使用する結合ポリシーの ID。 結合ポリシーが指定されていない場合、組織のデフォルトの結合ポリシーが使用されます。

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

`/access/entities` エンドポイントに対して POST リクエストを実行し、ペイロードに ID を指定することで、複数のプロファイルエンティティを取得できます。

>[!BEGINTABS]

>[!TAB  プロファイルエンティティ ]

**API 形式**

```http
POST /access/entities
```

**リクエスト**

次のリクエストは、ID のリストから複数の顧客の名前とメールアドレスを取得します。

+++複数のエンティティを取得するリクエストのサンプル

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

>[!TAB B2B アカウント ]

**API 形式**

```http
POST /access/entities
```

**リクエスト**

次のリクエストは、リクエストされた B2B アカウントを取得します。

+++複数のエンティティを取得するリクエストのサンプル

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

時系列イベントを取得すると、結果はページ付けされます。結果の後続のページがある場合、`_page.next` プロパティには ID が含まれます。また、`_links.next.href` プロパティは次のページを取得するためのリクエスト URI を提供します。結果を取得するには、`/access/entities` エンドポイントに対して別のGET リクエストを実行し、`/entities` を指定された URI の値に置き換えます。

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

>[!IMPORTANT]
>
>エンティティを削除エンドポイントは、2025 年 10 月末までに非推奨（廃止予定）になります。 レコードの削除操作を実行する場合は、代わりに [ データライフサイクルレコード削除 API ワークフロー ](/help/hygiene/api/workorder.md) または [ データライフサイクルレコード削除 UI ワークフロー ](/help/hygiene/ui/record-delete.md) を使用できます。
>
>さらに、次の B2B エンティティに対する削除リクエストは、既に非推奨（廃止予定）になっています。
>
>- アカウント
>- アカウントと人物の関係
>- Opportunity
>- 商談と担当者の関係
>- Campaign
>- キャンペーンメンバー
>- マーケティングリスト
>- マーケティングリストメンバー

プロファイルストアからエンティティを削除するには、必要なクエリパラメーターと共に `/access/entities` エンドポイントに対してDELETE リクエストを行います。

**API 形式**

```http
DELETE /access/entities?{QUERY_PARAMETERS}
```

クエリパスに指定されたデータパラメーターで、アクセスするデータを指定します。複数のパラメーターを使用する場合は、アンパサンド（&amp;）で区切ります。

エンティティを削除するには、次のクエリパラメーターを指定する **必要があります**。

- `schema.name`：エンティティの XDM スキーマの名前。 このユースケースでは、**を** のみ `schema.name=_xdm.context.profile` 使用できます。
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

このガイドに従うと、[!DNL Real-Time Customer Profile] のデータフィールド、プロファイルおよび時系列データに正常にアクセスできます。 [!DNL Experience Platform] に保存されている他のデータリソースにアクセスする方法については、[ データアクセスの概要 ](../../data-access/home.md) を参照してください。

## 付録 {#appendix}

次の節では、API を使用した [!DNL Profile] データへのアクセスに関する補足情報を提供します。

### クエリパラメーター {#query-parameters}

次のパラメーターは、`/access/entities` エンドポイントに対する GET リクエストのパスで使用されます。アクセスするプロファイルエンティティを識別し、応答で返されるデータをフィルターします。必須パラメーターはラベル付けされますが、残りはオプションです。

| パラメーター | 説明 | 例 |
| --------- | ----------- | ------- |
| `schema.name` | **（必須）** エンティティの XDM スキーマの名前。 | `schema.name=_xdm.context.profile` |
| `relatedSchema.name` | `schema.name` が `_xdm.context.experienceevent` の場合、この値は、時系列イベントが関連するプロファイルエンティティのスキーマを指定します **必須**。 | `relatedSchema.name=_xdm.context.profile` |
| `entityId` | **（必須）** エンティティの ID。 このパラメーターの値が XID でない場合、ID 名前空間パラメーター（`entityIdNS`）も指定する必要があります。 | `entityId=janedoe@example.com` |
| `entityIdNS` | XID として `entityId` を指定しない場合、このフィールドは ID 名前空間を指定します **必須**。 | `entityIdNS=email` |
| `relatedEntityId` | `schema.name` が `_xdm.context.experienceevent` の場合、この値は、関連するプロファイルエンティティの ID を指定します **必須**。 この値は、`entityId` と同じ規則に従います 。 | `relatedEntityId=69935279872410346619186588147492736556` |
| `relatedEntityIdNS` | `schema.name` が「_xdm.context.experienceevent」の場合、この値は `relatedEntityId` で指定したエンティティの ID 名前空間を指定する必要があります。 | `relatedEntityIdNS=CRMID` |
| `fields` | 応答で返されるデータをフィルターします。取得したデータに含めるスキーマフィールドの値を指定します。複数のフィールドの場合、値はコンマで区切り、スペースは使用しません。 | `fields=personalEmail,person.name,person.gender` |
| `mergePolicyId` | *推奨* 返されるデータを制御する結合ポリシーを識別します。 呼び出しで指定されていない場合は、組織のデフォルトのスキーマが使用されます。リクエストしているスキーマにデフォルトの結合ポリシーが定義されていない場合、API は HTTP 422 エラーステータスコードを返します。 | `mergePolicyId=5aa6885fcf70a301dabdfa4a` |
| `orderBy` | 取得したエンティティの並べ替え順（タイムスタンプ別）。 これは `(+/-)timestamp` として記述され、デフォルトは `+timestamp` です。 | `orderby=-timestamp` |
| `startTime` | エンティティをフィルター処理する開始時間をミリ秒単位で指定します。 | `startTime=1539838505` |
| `endTime` | エンティティのフィルタリングの終了時間をミリ秒単位で指定します。 | `endTime=1539838510` |
| `limit` | 返されるエンティティの最大数を指定します。 デフォルトでは、この値は 1000 に設定されています。 | `limit=100` |
| `property` | プロパティ値でフィルタリングします。 このクエリパラメーターでは、=、！のエバリュエーターがサポートされています。=、&lt;、&lt;=、>、>=。 これは、最大 3 つのプロパティをサポートするエクスペリエンスイベントでのみ使用できます。 | `property=webPageDetails.isHomepage=true&property=localTime<="2020-07-20"` |
