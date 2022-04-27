---
keywords: Experience Platform；ホーム；人気の高いトピック；OneTrust
solution: Experience Platform
title: （ベータ版）フローサービス API を使用して、OneTrust 統合ソースのデータフローを作成する
description: Learn how to connect Adobe Experience Platform to OneTrust Integration using the Flow Service API.
exl-id: e224efe0-4756-4b8a-b446-a3e1066f2050
source-git-commit: fc719a4ec90c5150f129deec45da87df703ec4b5
workflow-type: tm+mt
source-wordcount: '2313'
ht-degree: 75%

---

# （ベータ版） [!DNL OneTrust Integration] を使用したソース [!DNL Flow Service] API

>[!NOTE]
>
>この [!DNL OneTrust Integration] ソースはベータ版です。 機能とドキュメントは変更される場合があります。 ベータ版のラベル付きソースの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions).

次のチュートリアルでは、ソース接続とデータフローを作成し、履歴データとスケジュールされた同意データの両方をから取り込む手順について説明します。 [[!DNL OneTrust Integration]](https://my.onetrust.com/s/contactsupport?language=en_US) を使用してAdobe Experience Platformに [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## 前提条件

>[!IMPORTANT]
>
>この [!DNL OneTrust Integration] ソースコネクタとドキュメントは [!DNL OneTrust Integration] チーム。 お問い合わせや更新のご依頼については、 [[!DNL OneTrust] チーム](https://my.onetrust.com/s/contactsupport?language=en_US) 直接

Before you can connect [!DNL OneTrust Integration] to Platform, you must first retrieve your access token. アクセストークンの検索方法について詳しくは、 [[!DNL OneTrust Integration] OAuth 2 ガイド](https://developer.onetrust.com/docs/api-docs-v3/b3A6MjI4OTUyOTc-generate-access-token).

システム間の更新トークンはではサポートされていないので、有効期限が切れた後、アクセストークンは自動的に更新されません。 [!DNL OneTrust]. したがって、アクセストークンの有効期限が切れる前に、接続でアクセストークンを必ず更新しておく必要があります。 アクセストークンの設定可能な最大有効期間は 1 年です。 アクセストークンの更新について詳しくは、 [[!DNL OneTrust] OAuth 2.0 クライアント資格情報の管理に関するドキュメント](https://developer.onetrust.com/docs/documentation/ZG9jOjIyODk1MTUw-managing-o-auth-2-0-client-credentials).

## 接続 [!DNL OneTrust Integration] を使用して Platform に [!DNL Flow Service] API

>[!NOTE]
>
>この [!DNL OneTrust Integration] API の仕様は、データ取り込み用にAdobeと共有されています。

次のチュートリアルでは、 [!DNL OneTrust Integration] ソース接続と、 [!DNL OneTrust Integration] を使用して Platform にデータを送信する [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

### ベース接続の作成 {#base-connection}

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

To create a base connection ID, make a POST request to the `/connections` endpoint while providing your [!DNL OneTrust Integration] authentication credentials as part of the request body.

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL OneTrust Integration] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "ONETRUST base connection",
      "description": "ONETRUST base connection to authenticate to Platform",
      "connectionSpec": {
          "id": "cf16d886-c627-4872-9936-fb08d6cba8cc",
          "version": "1.0"
      },
      "auth": {
          "specName": "OAuth2 Refresh Code",
          "params": {
              "host": "{HOST}",
              "accessToken": "{ACCESS_TOKEN_OT}"
          }
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ベース接続の名前。ベース接続の情報を検索する際に使用できるので、ベース接続の名前はわかりやすいものにしてください。 |
| `description` | ベース接続に関する詳細情報を提供するために含めることができるオプションの値です。 |
| `connectionSpec.id` | ソースの接続仕様 ID。この ID は、ソースが登録および承認された後に、[!DNL Flow Service] API から取得することができます。 |
| `auth.specName` | Platform へのソースの認証に使用する認証タイプ。 |
| `auth.params.` | Contains the credentials required to authenticate your source. API に接続するためのルートホストとアクセストークン。 |
| `auth.params.host` | The environment from which the [!DNL OneTrust Integration] data needs to be pulled from. |
| `auth.params.accessToken` | に対応するアクセストークン [!DNL OneTrust Integration] アカウント |

**応答**

リクエストが成功した場合は、一意の接続識別子（`id`）を含む、新しく作成されたベース接続が返されます。この ID は、次の手順でソースのファイル構造と内容を調べるために必要です。

```json
{
     "id": "622124ca-6d18-47f7-999c-66f599955309",
     "etag": "\"2e026443-0000-0200-0000-621f1af80000\""
}
```

### ソースを参照 {#explore}

前の手順で生成したベース接続 ID を使用することで、GET リクエストを実行してファイルとディレクトリを調べることができます。

次の呼び出しを使用して、に取り込むファイルのパスを見つけます。 [!DNL Platform]:

**API 形式**

```http
GET /connections/{BASE_CONNECTION_ID}/explore?objectType=rest&object={OBJECT}&fileType={FILE_TYPE}&preview={PREVIEW}
```

ソースのファイル構造とコンテンツを調べるために GET リクエストを実行する場合、次の表に示すクエリのパラメーターを含める必要があります。


| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | 前の手順で生成したベース接続 ID。 |
| `objectType=rest` | 参照するオブジェクトのタイプ。 現在、この値は常にに設定されています。 `rest`. |
| `{OBJECT}` | このパラメーターは、特定のディレクトリを表示する場合にのみ必要です。 値は、参照するディレクトリのパスを表します。 |
| `fileType=json` | The file type of the file you want to bring to Platform. 現在、 `json` は、サポートされている唯一のファイルタイプです。 |
| `{PREVIEW}` | 接続のコンテンツがプレビューをサポートするかどうかを定義するブール値です。 |

**リクエスト**

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connections/622124ca-6d18-47f7-999c-66f599955309/explore?objectType=rest&object=json&fileType=json&preview=true' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答が成功すると、クエリされたファイルの構造を返します。

>[!NOTE]
>
>以下の JSON 応答ペイロードは、簡潔にするために非表示になっています。 「このアイコンをクリックして、応答ペイロードを表示」を選択します。

+++クリック

```json
{
    "format": "hierarchical",
    "schema": {
        "type": "object",
        "properties": {
            "number": {
                "type": "integer",
                "minimum": -9007199254740992,
                "maximum": 9007199254740991
            },
            "size": {
                "type": "integer",
                "minimum": -9007199254740992,
                "maximum": 9007199254740991
            },
            "numberOfElements": {
                "type": "integer",
                "minimum": -9007199254740992,
                "maximum": 9007199254740991
            },
            "last": {
                "type": "boolean"
            },
            "pageable": {
                "type": "object",
                "properties": {
                    "paged": {
                        "type": "boolean"
                    },
                    "pageNumber": {
                        "type": "integer",
                        "minimum": -9007199254740992,
                        "maximum": 9007199254740991
                    },
                    "offset": {
                        "type": "integer",
                        "minimum": -9007199254740992,
                        "maximum": 9007199254740991
                    },
                    "tokenId": {
                        "type": "integer",
                        "minimum": -9007199254740992,
                        "maximum": 9007199254740991
                    },
                    "limit": {
                        "type": "integer",
                        "minimum": -9007199254740992,
                        "maximum": 9007199254740991
                    },
                    "pageSize": {
                        "type": "integer",
                        "minimum": -9007199254740992,
                        "maximum": 9007199254740991
                    },
                    "unpaged": {
                        "type": "boolean"
                    },
                    "sort": {
                        "type": "object",
                        "properties": {
                            "unsorted": {
                                "type": "boolean"
                            },
                            "sorted": {
                                "type": "boolean"
                            },
                            "empty": {
                                "type": "boolean"
                            }
                        }
                    }
                }
            },
            "sort": {
                "type": "object",
                "properties": {
                    "unsorted": {
                        "type": "boolean"
                    },
                    "sorted": {
                        "type": "boolean"
                    },
                    "empty": {
                        "type": "boolean"
                    }
                }
            },
            "content": {
                "type": "object",
                "properties": {
                    "LastUpdatedDate": {
                        "type": "string"
                    },
                    "Identifier": {
                        "type": "string"
                    },
                    "Language": {
                        "type": "string"
                    },
                    "TestDataSubject": {
                        "type": "boolean"
                    },
                    "CreatedDate": {
                        "type": "string"
                    },
                    "DataElements": {
                        "type": "array",
                        "items": {
                            "type": "object",
                            "properties": {}
                        }
                    },
                    "Id": {
                        "type": "string"
                    },
                    "Purposes": {
                        "type": "array",
                        "items": {
                            "type": "object",
                            "properties": {
                                "Status": {
                                    "type": "string"
                                },
                                "LastTransactionDate": {
                                    "type": "string"
                                },
                                "CustomPreferences": {
                                    "type": "array",
                                    "items": {
                                        "type": "object",
                                        "properties": {}
                                    }
                                },
                                "LastUpdatedDate": {},
                                "ExpiryDate": {},
                                "Topics": {
                                    "type": "array",
                                    "items": {
                                        "type": "object",
                                        "properties": {
                                            "IsConsented": {
                                                "type": "boolean"
                                            },
                                            "Id": {
                                                "type": "string"
                                            },
                                            "Name": {
                                                "type": "string"
                                            }
                                        }
                                    }
                                },
                                "TotalTransactionCount": {
                                    "type": "integer",
                                    "minimum": -9007199254740992,
                                    "maximum": 9007199254740991
                                },
                                "LastReceiptId": {},
                                "ConsentDate": {
                                    "type": "string"
                                },
                                "LastInteractionDate": {},
                                "Name": {
                                    "type": "string"
                                },
                                "FirstTransactionDate": {
                                    "type": "string"
                                },
                                "LastTransactionCollectionPointId": {
                                    "type": "string"
                                },
                                "LastTransactionCollectionPointVersion": {
                                    "type": "integer",
                                    "minimum": -9007199254740992,
                                    "maximum": 9007199254740991
                                },
                                "Version": {
                                    "type": "integer",
                                    "minimum": -9007199254740992,
                                    "maximum": 9007199254740991
                                },
                                "attributes": {
                                    "type": "object",
                                    "properties": {}
                                },
                                "Id": {
                                    "type": "string"
                                },
                                "PurposeNote": {},
                                "WithdrawalDate": {
                                    "type": "string"
                                }
                            }
                        }
                    }
                }
            },
            "first": {
                "type": "boolean"
            },
            "empty": {
                "type": "boolean"
            }
        }
    },
    "data": [
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "de1ab4d2-6ccf-42bd-b363-2d8cac60c88c",
                "Language": "en-us",
                "Identifier": "gkumar@onetrust.com",
                "LastUpdatedDate": "2019-06-05T12:02:07Z",
                "CreatedDate": "2018-05-02T03:14:28Z",
                "Purposes": [
                    {
                        "Id": "9edf57bb-0449-4c15-98e4-8641522e5ff4",
                        "Name": "Purpose_UAT",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-05-02T03:14:27Z",
                        "LastTransactionDate": "2019-05-29T11:08:26Z",
                        "WithdrawalDate": "2019-05-29T11:05:30Z",
                        "ConsentDate": "2018-05-02T03:14:27Z",
                        "TotalTransactionCount": 5,
                        "Topics": [
                            {
                                "Id": "d6e3d675-3d6f-4f4e-a157-bd93829ee632",
                                "Name": "Topic_UAT",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "735c85c8-c69c-44bc-8bad-ec0e806090bd",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "814c073a-95f1-4fc9-8263-1ec62225c5e9",
                        "Name": "Multi Pur_UAT",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-05-02T03:17:33Z",
                        "LastTransactionDate": "2018-05-02T03:19:49Z",
                        "ConsentDate": "2018-05-02T03:17:33Z",
                        "TotalTransactionCount": 4,
                        "LastTransactionCollectionPointId": "9a5b7375-bc13-47e8-8a58-1ca7d9c06c8e",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "4d52dcc4-82bf-44bf-ac49-854eba6ff8f3",
                        "Name": "Eloqua_UAT",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-05-02T03:26:36Z",
                        "LastTransactionDate": "2019-03-07T03:23:25Z",
                        "ConsentDate": "2018-05-02T03:26:36Z",
                        "TotalTransactionCount": 3,
                        "Topics": [
                            {
                                "Id": "690ad782-6280-4ea4-b62a-1514c39fc838",
                                "Name": "Eloqua_topic",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "f8886377-e4b1-45e4-b1c0-d7dacb744db8",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "723300e3-96cb-4da4-abdd-4913c05be215",
                        "Name": "Purpose 1",
                        "Version": 2,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-02-27T06:29:48Z",
                        "LastTransactionDate": "2019-02-28T12:00:45Z",
                        "ConsentDate": "2019-02-27T06:29:48Z",
                        "ExpiryDate": "2019-02-28T12:00:45Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "3dbfb978-10f9-44a9-9669-d699674edd9d",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "NO_CONSENT",
                        "FirstTransactionDate": "2019-03-07T03:21:58Z",
                        "LastTransactionDate": "2019-05-29T03:56:37Z",
                        "TotalTransactionCount": 6,
                        "LastTransactionCollectionPointId": "cea42a12-5de4-421a-b429-092e8d523948",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "a36f98d0-c662-4f61-a2a1-8bdab69e3c3a",
                        "Name": "Pur 1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-07T03:23:25Z",
                        "LastTransactionDate": "2019-03-07T03:23:27Z",
                        "ConsentDate": "2019-03-07T03:23:25Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "f8886377-e4b1-45e4-b1c0-d7dacb744db8",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "41ebdf32-068b-4239-89ac-42e20262c5a9",
                        "Name": "Send Notifications about Changes to Preferences",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-07T03:24:11Z",
                        "LastTransactionDate": "2019-03-07T03:27:29Z",
                        "ConsentDate": "2019-03-07T03:24:11Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "c29a99a9-de4d-4367-973b-95b0217d0640",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "759d8b24-e4c0-4107-86e2-3a408db13e6b",
                        "Name": "GJ Purpose 07",
                        "Version": 2,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2019-03-07T03:27:29Z",
                        "LastTransactionDate": "2019-05-29T11:09:03Z",
                        "WithdrawalDate": "2019-05-29T11:09:03Z",
                        "ConsentDate": "2019-03-07T03:27:29Z",
                        "TotalTransactionCount": 18,
                        "LastTransactionCollectionPointId": "cea42a12-5de4-421a-b429-092e8d523948",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "a57ee9da-b494-49e2-b562-6dfa31f190df",
                        "Name": "kbpurpose2",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2019-03-28T03:23:44Z",
                        "LastTransactionDate": "2019-03-28T03:24:29Z",
                        "WithdrawalDate": "2019-03-28T03:24:29Z",
                        "ConsentDate": "2019-03-28T03:23:44Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "c29a99a9-de4d-4367-973b-95b0217d0640",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "a1ccd810-94a0-4b58-946b-6705eae15c5b",
                        "Name": "Purpose01 v2",
                        "Version": 2,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-05-29T04:05:42Z",
                        "LastTransactionDate": "2019-06-05T12:02:06Z",
                        "WithdrawalDate": "2019-05-29T11:09:12Z",
                        "ConsentDate": "2019-05-29T04:05:42Z",
                        "ExpiryDate": "2019-06-05T12:02:06Z",
                        "TotalTransactionCount": 8,
                        "Topics": [
                            {
                                "Id": "af7bf604-2058-4089-985c-c84083e3e3e3",
                                "Name": "New_Topic",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "735c85c8-c69c-44bc-8bad-ec0e806090bd",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "a313353a-e13b-4554-be08-22cf4a78a31c",
                        "Name": "Purpose_1804 v2",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-05-29T04:05:42Z",
                        "LastTransactionDate": "2019-05-29T11:09:30Z",
                        "WithdrawalDate": "2019-05-29T11:05:30Z",
                        "ConsentDate": "2019-05-29T04:05:42Z",
                        "TotalTransactionCount": 4,
                        "CustomPreferences": [
                            {
                                "Id": "4935d35a-7216-480d-9da9-1aef0e078b89",
                                "Name": "Custom_S",
                                "Options": [
                                    {
                                        "Id": "8acc2f9f-37f8-4ace-a513-fae6b7dd0aa9",
                                        "Name": "Option2",
                                        "IsConsented": true
                                    }
                                ]
                            }
                        ],
                        "LastTransactionCollectionPointId": "735c85c8-c69c-44bc-8bad-ec0e806090bd",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "e2c7dbd5-9110-4407-baf4-1bb153f2adfb",
                "Identifier": "kburt@onetrust.com",
                "LastUpdatedDate": "2018-05-21T20:50:47Z",
                "CreatedDate": "2018-05-21T20:50:47Z",
                "Purposes": [
                    {
                        "Id": "9edf57bb-0449-4c15-98e4-8641522e5ff4",
                        "Name": "Purpose_UAT",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2018-05-21T20:50:47Z",
                        "LastTransactionDate": "2018-05-21T21:43:36Z",
                        "WithdrawalDate": "2018-05-21T21:43:36Z",
                        "ConsentDate": "2018-05-21T00:00:00Z",
                        "TotalTransactionCount": 3,
                        "LastTransactionCollectionPointId": "83cf0858-031e-47ac-800d-c34e8bcaa99b",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2018-05-21T21:38:54Z",
                        "LastTransactionDate": "2018-05-21T21:43:37Z",
                        "WithdrawalDate": "2018-05-21T21:43:37Z",
                        "ConsentDate": "2018-05-21T21:38:54Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "83cf0858-031e-47ac-800d-c34e8bcaa99b",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "a705e32e-2b00-40d8-a1f9-a6be831b8e37",
                        "Name": "kbpurpose3",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2018-05-21T21:39:23Z",
                        "LastTransactionDate": "2018-05-21T21:43:37Z",
                        "WithdrawalDate": "2018-05-21T21:43:37Z",
                        "ConsentDate": "2018-05-21T21:39:23Z",
                        "TotalTransactionCount": 3,
                        "LastTransactionCollectionPointId": "83cf0858-031e-47ac-800d-c34e8bcaa99b",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "a93143f5-9a33-4e24-9021-bd6ff7ad817c",
                        "Name": "kbpurpose4",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2018-05-21T21:39:23Z",
                        "LastTransactionDate": "2018-05-21T21:43:37Z",
                        "WithdrawalDate": "2018-05-21T21:43:37Z",
                        "ConsentDate": "2018-05-21T21:39:23Z",
                        "TotalTransactionCount": 3,
                        "LastTransactionCollectionPointId": "83cf0858-031e-47ac-800d-c34e8bcaa99b",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "814c073a-95f1-4fc9-8263-1ec62225c5e9",
                        "Name": "Multi Pur_UAT",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2018-05-21T21:41:36Z",
                        "LastTransactionDate": "2018-05-21T21:43:36Z",
                        "WithdrawalDate": "2018-05-21T21:43:36Z",
                        "ConsentDate": "2018-05-21T21:41:36Z",
                        "TotalTransactionCount": 3,
                        "LastTransactionCollectionPointId": "83cf0858-031e-47ac-800d-c34e8bcaa99b",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "60c60a52-02ee-479c-a045-75dea16552e5",
                "Language": "en-US",
                "Identifier": "consentot@gmail.com",
                "LastUpdatedDate": "2021-04-23T08:26:22Z",
                "CreatedDate": "2018-08-16T06:02:20Z",
                "DataElements": [
                    {
                        "Name": "Address",
                        "Value": "Add",
                        "Linked": false
                    },
                    {
                        "Name": "City",
                        "Value": "City",
                        "Linked": false
                    }
                ],
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "LastReceiptId": "4b30b903-01d7-4f74-b100-91832e1ff1aa",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2018-08-16T06:02:19Z",
                        "LastTransactionDate": "2021-04-23T08:26:20Z",
                        "WithdrawalDate": "2021-04-23T08:26:20Z",
                        "ConsentDate": "2018-08-16T06:02:19Z",
                        "TotalTransactionCount": 7,
                        "LastTransactionCollectionPointId": "00000000-0000-0000-0000-000000000000",
                        "LastTransactionCollectionPointVersion": 1,
                        "LastUpdatedDate": "2021-04-23T08:26:20Z",
                        "LastInteractionDate": "2021-04-23T08:26:20Z"
                    },
                    {
                        "Id": "2c7d3663-e1c5-4d82-bce4-a777e83cfda0",
                        "Name": "Purpose_123 v2",
                        "Version": 2,
                        "Status": "NO_CONSENT",
                        "FirstTransactionDate": "2018-11-09T04:51:41Z",
                        "LastTransactionDate": "2018-11-09T04:51:41Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "ff05cf0f-31ce-412c-9beb-cb1b7313f5c8",
                        "LastTransactionCollectionPointVersion": 3
                    },
                    {
                        "Id": "a1ccd810-94a0-4b58-946b-6705eae15c5b",
                        "Name": "Purpose01 v2",
                        "Version": 2,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2019-04-18T03:29:33Z",
                        "LastTransactionDate": "2019-04-18T03:30:00Z",
                        "WithdrawalDate": "2019-04-18T03:30:00Z",
                        "ConsentDate": "2019-04-18T03:29:33Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "735c85c8-c69c-44bc-8bad-ec0e806090bd",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "a313353a-e13b-4554-be08-22cf4a78a31c",
                        "Name": "Purpose_1804 v2",
                        "Version": 2,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2019-04-18T03:29:33Z",
                        "LastTransactionDate": "2019-04-18T03:53:57Z",
                        "WithdrawalDate": "2019-04-18T03:53:57Z",
                        "ConsentDate": "2019-04-18T03:29:33Z",
                        "TotalTransactionCount": 5,
                        "LastTransactionCollectionPointId": "b9ed1377-c66d-40d3-a36c-aabc4a5db45b",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "07c5b97a-5b53-4cd1-8f15-a979950ce0af",
                        "Name": "Purpose-1 v2",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-18T03:31:30Z",
                        "LastTransactionDate": "2019-04-18T03:32:21Z",
                        "ConsentDate": "2019-04-18T03:31:30Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "b9ed1377-c66d-40d3-a36c-aabc4a5db45b",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "d5541c9c-5827-4030-9d77-3458e26e455d",
                        "Name": "Purpose_3107",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-07-31T04:15:53Z",
                        "LastTransactionDate": "2019-07-31T04:15:53Z",
                        "ConsentDate": "2019-07-31T04:15:53Z",
                        "TotalTransactionCount": 1,
                        "CustomPreferences": [
                            {
                                "Id": "50e57d7a-3f1c-48d2-a358-fb8de7b71ea8",
                                "Name": "Custom_checkboxes",
                                "Options": [
                                    {
                                        "Id": "c1801683-cb43-472c-8581-4779b33dce0c",
                                        "Name": "Option1",
                                        "IsConsented": true
                                    }
                                ]
                            }
                        ],
                        "LastTransactionCollectionPointId": "1adf523e-d2f7-41c2-a4d9-1bc26502099b",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "c8fdae1a-9247-4a00-b042-4de69f3980d2",
                        "Name": "5.0Purp",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-11-25T06:12:20Z",
                        "LastTransactionDate": "2019-11-25T06:12:20Z",
                        "ConsentDate": "2019-11-25T06:12:20Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "56120ac3-fa07-44bc-b724-c33b532f63d7",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "1d04d38f-758e-4898-b884-bb6e818eb9cb",
                        "LastReceiptId": "b8cd0f83-f2ec-47b4-b14a-2c55c9c810b5",
                        "Name": "Purpose_0903",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2021-03-09T08:36:05Z",
                        "LastTransactionDate": "2021-03-09T08:36:28Z",
                        "ConsentDate": "2021-03-09T08:36:28Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "686a3b21-ca3a-41f5-861b-082b2d20cef3",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "LastReceiptId": "00aadfac-7d81-406d-8bb4-5acb6033ece2",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2021-03-09T08:37:24Z",
                        "LastTransactionDate": "2021-03-10T12:00:57Z",
                        "ConsentDate": "2021-03-09T08:37:24Z",
                        "ExpiryDate": "2021-03-10T12:00:57Z",
                        "TotalTransactionCount": 2,
                        "Topics": [
                            {
                                "Id": "1877891d-5f40-412d-8da0-16d1bf0f499b",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "4577b750-3c0a-4ead-b5b3-1fc03403cde7",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "41ebdf32-068b-4239-89ac-42e20262c5a9",
                        "LastReceiptId": "00d13ba7-2488-4129-9d50-9e3c06ba0139",
                        "Name": "Send Notifications about Changes to Preferences",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2021-03-09T08:37:37Z",
                        "LastTransactionDate": "2021-03-09T08:37:42Z",
                        "WithdrawalDate": "2021-03-09T08:37:37Z",
                        "ConsentDate": "2021-03-09T08:37:42Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "4577b750-3c0a-4ead-b5b3-1fc03403cde7",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "f33fd6f6-61c5-4ba1-9b79-805a5cdb82dd",
                "Identifier": "ot1shwetha@gmail.com",
                "LastUpdatedDate": "2019-04-05T11:36:19Z",
                "CreatedDate": "2018-08-16T06:14:10Z",
                "Purposes": [
                    {
                        "Id": "48c7374d-5d35-4268-b378-2f6d9d414822",
                        "Name": "nrfix",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-08-16T06:14:09Z",
                        "LastTransactionDate": "2018-08-16T06:14:09Z",
                        "ConsentDate": "2018-08-16T06:14:09Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "a3617404-b820-4552-8cc0-aeaac1e13f18",
                                "Name": "kbtopic1",
                                "IsConsented": true
                            },
                            {
                                "Id": "74cff719-9739-4e75-865b-a0ec787c1f08",
                                "Name": "kbtopic2",
                                "IsConsented": true
                            },
                            {
                                "Id": "dc9095ea-9ac7-4b6c-a8a4-2d97bf3f5417",
                                "Name": "GJ topic 1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "4061933d-7b58-4098-93c5-0d05fa177bf0",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "814c073a-95f1-4fc9-8263-1ec62225c5e9",
                        "Name": "Multi Pur_UAT",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-08-16T06:16:34Z",
                        "LastTransactionDate": "2018-08-16T06:16:34Z",
                        "ConsentDate": "2018-08-16T06:16:34Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "f81b2b28-47d1-4807-a139-dd94080c95a8",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "a57ee9da-b494-49e2-b562-6dfa31f190df",
                        "Name": "kbpurpose2",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2018-08-16T06:16:34Z",
                        "LastTransactionDate": "2018-08-16T06:17:05Z",
                        "WithdrawalDate": "2018-08-16T06:17:05Z",
                        "ConsentDate": "2018-08-16T06:16:34Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "f81b2b28-47d1-4807-a139-dd94080c95a8",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-08-16T06:20:43Z",
                        "LastTransactionDate": "2019-04-05T11:36:21Z",
                        "ConsentDate": "2018-08-16T06:20:43Z",
                        "TotalTransactionCount": 3,
                        "Topics": [
                            {
                                "Id": "38add7c6-ef9c-4b6d-8f3c-147c70124484",
                                "Name": "kbtopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "ac3bea9e-c509-4442-9cd5-569150abf4fb",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "a705e32e-2b00-40d8-a1f9-a6be831b8e37",
                        "Name": "kbpurpose3",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-08-16T06:20:43Z",
                        "LastTransactionDate": "2018-08-16T06:21:08Z",
                        "ConsentDate": "2018-08-16T06:20:43Z",
                        "ExpiryDate": "2018-08-16T06:21:08Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "5f3d59df-8966-46d1-9465-36eeb6424d56",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "a93143f5-9a33-4e24-9021-bd6ff7ad817c",
                        "Name": "kbpurpose4",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-08-16T06:20:43Z",
                        "LastTransactionDate": "2018-08-16T06:21:08Z",
                        "ConsentDate": "2018-08-16T06:20:43Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "5f3d59df-8966-46d1-9465-36eeb6424d56",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "2c7d3663-e1c5-4d82-bce4-a777e83cfda0",
                        "Name": "Purpose_123 v2",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T04:51:50Z",
                        "LastTransactionDate": "2018-11-09T04:51:50Z",
                        "ConsentDate": "2018-11-09T04:51:50Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "df17e795-6c63-48d8-8301-42ea30728fad",
                                "Name": "topic123",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "ff05cf0f-31ce-412c-9beb-cb1b7313f5c8",
                        "LastTransactionCollectionPointVersion": 2
                    },
                    {
                        "Id": "7a08dc31-8e5a-4d92-ab93-5af6528af046",
                        "Name": "govi47p",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2019-03-28T03:14:19Z",
                        "LastTransactionDate": "2019-03-28T03:16:44Z",
                        "WithdrawalDate": "2019-03-28T03:16:44Z",
                        "ConsentDate": "2019-03-28T03:14:19Z",
                        "TotalTransactionCount": 4,
                        "LastTransactionCollectionPointId": "50883b50-50c2-4539-8b25-6eba2631d97e",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "2881f1cd-6c60-41e7-8b8c-0899a612bfae",
                "Identifier": "DS1@gmail.com",
                "LastUpdatedDate": "2018-08-16T06:19:32Z",
                "CreatedDate": "2018-08-16T06:19:32Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2018-08-16T06:19:32Z",
                        "LastTransactionDate": "2018-08-16T06:19:32Z",
                        "ConsentDate": "2018-08-16T06:19:32Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "fb397993-4bc0-403e-b3be-d962089ea821",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "052b48cf-61d6-42d6-9b4a-901fa9019e5f",
                "Identifier": "DS2@gmail.com",
                "LastUpdatedDate": "2018-08-16T06:19:33Z",
                "CreatedDate": "2018-08-16T06:19:33Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2018-08-16T06:19:33Z",
                        "LastTransactionDate": "2018-08-16T06:19:33Z",
                        "ConsentDate": "2018-08-16T06:19:33Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "fb397993-4bc0-403e-b3be-d962089ea821",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "163c94c1-6774-4a10-be43-ac25a250ec77",
                "Identifier": "DS3@gmail.com",
                "LastUpdatedDate": "2018-08-16T06:19:33Z",
                "CreatedDate": "2018-08-16T06:19:33Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2018-08-16T06:19:33Z",
                        "LastTransactionDate": "2018-08-16T06:19:33Z",
                        "ConsentDate": "2018-08-16T06:19:33Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "fb397993-4bc0-403e-b3be-d962089ea821",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "22bfd53e-1325-4e90-b118-b94b534e7b16",
                "Identifier": "DS4@gmail.com",
                "LastUpdatedDate": "2018-08-16T06:19:34Z",
                "CreatedDate": "2018-08-16T06:19:34Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2018-08-16T06:19:34Z",
                        "LastTransactionDate": "2018-08-16T06:19:34Z",
                        "ConsentDate": "2018-08-16T06:19:34Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "fb397993-4bc0-403e-b3be-d962089ea821",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "1fa817a0-caf1-43da-ae89-6e2249876c63",
                "Identifier": "DS5@gmail.com",
                "LastUpdatedDate": "2018-08-16T06:19:35Z",
                "CreatedDate": "2018-08-16T06:19:35Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2018-08-16T06:19:35Z",
                        "LastTransactionDate": "2018-08-16T06:19:35Z",
                        "ConsentDate": "2018-08-16T06:19:35Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "fb397993-4bc0-403e-b3be-d962089ea821",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "260270ad-c056-43c6-a5ef-0e0a4962107d",
                "Identifier": "DS6@gmail.com",
                "LastUpdatedDate": "2018-08-16T06:19:35Z",
                "CreatedDate": "2018-08-16T06:19:35Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2018-08-16T06:19:35Z",
                        "LastTransactionDate": "2018-08-16T06:19:35Z",
                        "ConsentDate": "2018-08-16T06:19:35Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "fb397993-4bc0-403e-b3be-d962089ea821",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "a28ef470-70d9-4c0d-b84f-678563a24795",
                "Identifier": "DS7@gmail.com",
                "LastUpdatedDate": "2018-08-16T06:19:36Z",
                "CreatedDate": "2018-08-16T06:19:36Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2018-08-16T06:19:36Z",
                        "LastTransactionDate": "2018-08-16T06:19:36Z",
                        "ConsentDate": "2018-08-16T06:19:36Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "fb397993-4bc0-403e-b3be-d962089ea821",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "1940412f-b5f6-4799-b7a7-2e7cbcc25371",
                "Identifier": "DS8@gmail.com",
                "LastUpdatedDate": "2018-08-16T06:19:36Z",
                "CreatedDate": "2018-08-16T06:19:36Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2018-08-16T06:19:36Z",
                        "LastTransactionDate": "2018-08-16T06:19:36Z",
                        "ConsentDate": "2018-08-16T06:19:36Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "fb397993-4bc0-403e-b3be-d962089ea821",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "ae6bdfdc-8468-4574-affa-e5ae3f2033cd",
                "Identifier": "DS9@gmail.com",
                "LastUpdatedDate": "2018-08-16T06:19:37Z",
                "CreatedDate": "2018-08-16T06:19:37Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2018-08-16T06:19:37Z",
                        "LastTransactionDate": "2018-08-16T06:19:37Z",
                        "ConsentDate": "2018-08-16T06:19:37Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "fb397993-4bc0-403e-b3be-d962089ea821",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "3c049614-e7a2-452f-8895-5611ba1a9726",
                "Identifier": "Gjupdatebulk",
                "LastUpdatedDate": "2018-08-16T06:54:54Z",
                "CreatedDate": "2018-08-16T06:54:05Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2018-08-16T06:54:05Z",
                        "LastTransactionDate": "2018-08-16T06:54:54Z",
                        "WithdrawalDate": "2018-08-16T06:54:54Z",
                        "ConsentDate": "2018-08-16T06:54:05Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "d2dce55a-6a1f-4d44-82bd-931fdce43652",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "4cf01b2c-943f-41e8-b4b8-15c5a3e2fafc",
                "Identifier": "41b1dfb9-b64d-4a5e-8b06-3b1c3506a384",
                "LastUpdatedDate": "2019-09-22T12:00:24Z",
                "CreatedDate": "2018-09-27T03:12:45Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-09-27T03:12:45Z",
                        "LastTransactionDate": "2019-09-22T12:00:24Z",
                        "ConsentDate": "2018-09-27T03:12:45Z",
                        "ExpiryDate": "2019-09-22T12:00:24Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "a3eaebb2-fffa-4d97-a23a-19d4cd8f0cb4",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "3a9ca675-da64-48c4-a8e4-a63afbf16774",
                "Identifier": "pikachu@otprivacy.com",
                "LastUpdatedDate": "2018-10-12T18:03:48Z",
                "CreatedDate": "2018-10-12T18:03:48Z",
                "Purposes": [
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-10-12T18:03:47Z",
                        "LastTransactionDate": "2018-10-12T18:03:47Z",
                        "ConsentDate": "2018-10-12T18:03:47Z",
                        "ExpiryDate": "2018-10-12T18:03:47Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "1877891d-5f40-412d-8da0-16d1bf0f499b",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "4f6c04e0-94eb-4cd9-a4cb-7cb0e84f7f74",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "7c123112-6e37-40a3-8f7f-dbe6a7f7d17d",
                "Identifier": "squirtle@otprivacy.com",
                "LastUpdatedDate": "2018-10-12T18:05:02Z",
                "CreatedDate": "2018-10-12T18:05:02Z",
                "Purposes": [
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2018-10-12T18:05:02Z",
                        "LastTransactionDate": "2018-10-12T18:05:02Z",
                        "ConsentDate": "2018-10-12T18:05:02Z",
                        "ExpiryDate": "2018-10-13T18:05:02Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "1877891d-5f40-412d-8da0-16d1bf0f499b",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "f67ea136-377b-47dd-bccf-a08ed40ffaeb",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "23bde54b-ee7c-46c8-ac7c-93ee858dc269",
                "Identifier": "charmander@otprivacy.com",
                "LastUpdatedDate": "2018-10-12T18:05:50Z",
                "CreatedDate": "2018-10-12T18:05:50Z",
                "Purposes": [
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-10-12T18:05:50Z",
                        "LastTransactionDate": "2018-10-12T18:05:50Z",
                        "ConsentDate": "2018-10-12T18:05:50Z",
                        "ExpiryDate": "2018-10-12T18:05:50Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "1877891d-5f40-412d-8da0-16d1bf0f499b",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "4f6c04e0-94eb-4cd9-a4cb-7cb0e84f7f74",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "67b42d36-47ab-44a9-b5a3-1280fdf998ca",
                "Identifier": "bulbasaur@otprivacy.com",
                "LastUpdatedDate": "2018-10-12T18:06:24Z",
                "CreatedDate": "2018-10-12T18:06:24Z",
                "Purposes": [
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2018-10-12T18:06:23Z",
                        "LastTransactionDate": "2018-10-12T18:06:23Z",
                        "ConsentDate": "2018-10-12T18:06:23Z",
                        "ExpiryDate": "2018-10-13T18:06:23Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "1877891d-5f40-412d-8da0-16d1bf0f499b",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "f67ea136-377b-47dd-bccf-a08ed40ffaeb",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "aa3a8e69-1ff2-4241-a514-19fa4552830e",
                "Identifier": "neemaot18+uat09@gmail.com",
                "LastUpdatedDate": "2018-11-09T04:46:42Z",
                "CreatedDate": "2018-11-09T04:46:42Z",
                "Purposes": [
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2018-11-09T04:46:41Z",
                        "LastTransactionDate": "2018-11-09T04:46:41Z",
                        "ConsentDate": "2018-11-09T04:46:41Z",
                        "ExpiryDate": "2018-11-10T04:46:41Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "1877891d-5f40-412d-8da0-16d1bf0f499b",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "f67ea136-377b-47dd-bccf-a08ed40ffaeb",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "ee0d727f-5aea-4002-b28f-b1cf003c7504",
                "Identifier": "arjunhcot@gmail.com",
                "LastUpdatedDate": "2018-11-09T04:53:42Z",
                "CreatedDate": "2018-11-09T04:48:19Z",
                "Purposes": [
                    {
                        "Id": "7b70ca4e-8224-45b3-9924-1b5214b56ab7",
                        "Name": "test12",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T04:48:18Z",
                        "LastTransactionDate": "2018-11-09T04:48:18Z",
                        "ConsentDate": "2018-11-09T04:48:18Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "e27052e2-15cc-47a6-9241-900bbfbfda20",
                                "Name": "!!!tp",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "05b58f5f-5042-465e-859c-9f2c8c24a11e",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2018-11-09T04:50:29Z",
                        "LastTransactionDate": "2018-11-09T04:53:41Z",
                        "WithdrawalDate": "2018-11-09T04:53:41Z",
                        "ConsentDate": "2018-11-09T04:50:29Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "1c632058-c1bd-45e7-8313-edb895dd6458",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "d4c72c01-efbb-4a5f-9f0f-02f5aca08188",
                "Identifier": "58d0df0d-d3a2-4f05-bd59-b0bcc10566a5",
                "LastUpdatedDate": "2019-11-04T12:00:53Z",
                "CreatedDate": "2018-11-09T04:49:18Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-11-09T04:49:17Z",
                        "LastTransactionDate": "2019-11-04T12:00:53Z",
                        "ConsentDate": "2018-11-09T04:49:17Z",
                        "ExpiryDate": "2019-11-04T12:00:53Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "b336290f-94d2-4915-b5d6-a47e8fc8c183",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "9c472568-5f94-4a80-9164-80f0fabf7db0",
                "Identifier": "demouserot321@gmail.com",
                "LastUpdatedDate": "2018-11-09T11:20:27Z",
                "CreatedDate": "2018-11-09T04:51:59Z",
                "Purposes": [
                    {
                        "Id": "2c7d3663-e1c5-4d82-bce4-a777e83cfda0",
                        "Name": "Purpose_123 v2",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T04:51:59Z",
                        "LastTransactionDate": "2018-11-09T04:51:59Z",
                        "ConsentDate": "2018-11-09T04:51:59Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "df17e795-6c63-48d8-8301-42ea30728fad",
                                "Name": "topic123",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "ff05cf0f-31ce-412c-9beb-cb1b7313f5c8",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "e36ff8aa-dbf8-4996-a7f1-cc4b966de362",
                        "Name": "sdk1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T04:59:54Z",
                        "LastTransactionDate": "2018-11-09T11:20:26Z",
                        "ConsentDate": "2018-11-09T04:59:54Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "620f54ce-394f-4c6a-9e5e-8a51d7760a7f",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "917941cb-5dd4-4108-a9c5-0fa831a9a529",
                        "Name": "sdk2",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T04:59:54Z",
                        "LastTransactionDate": "2018-11-09T11:20:26Z",
                        "ConsentDate": "2018-11-09T04:59:54Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "620f54ce-394f-4c6a-9e5e-8a51d7760a7f",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "e1bb45d1-525b-43e0-861c-8986a938eb0d",
                "Identifier": "neemaot18+doiuat@gmail.com",
                "LastUpdatedDate": "2018-11-09T05:01:55Z",
                "CreatedDate": "2018-11-09T04:57:30Z",
                "Purposes": [
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2018-11-09T04:57:30Z",
                        "LastTransactionDate": "2018-11-09T05:01:55Z",
                        "WithdrawalDate": "2018-11-09T05:01:55Z",
                        "ConsentDate": "2018-11-09T04:57:30Z",
                        "TotalTransactionCount": 3,
                        "LastTransactionCollectionPointId": "b66aeb51-0959-493b-89d5-446692ebffbc",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "f2c6208b-69cf-4cb4-9214-5c22d88aa35e",
                "Identifier": "neemaot18+notgiven@gmail.com",
                "LastUpdatedDate": "2018-11-09T05:02:57Z",
                "CreatedDate": "2018-11-09T05:02:57Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T05:02:57Z",
                        "LastTransactionDate": "2018-11-09T05:02:57Z",
                        "ConsentDate": "2018-11-09T05:02:57Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "38add7c6-ef9c-4b6d-8f3c-147c70124484",
                                "Name": "kbtopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "7c128fb9-9d78-44c0-9ea0-f2fe0566991c",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-11-09T05:02:57Z",
                        "LastTransactionDate": "2018-11-09T05:02:57Z",
                        "ConsentDate": "2018-11-09T05:02:57Z",
                        "ExpiryDate": "2018-11-09T05:02:57Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "1877891d-5f40-412d-8da0-16d1bf0f499b",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "7c128fb9-9d78-44c0-9ea0-f2fe0566991c",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "ddb932f7-c2ba-4219-a5e2-1ea7dfdf5945",
                "Identifier": "neemaot18+newNG@gmail.com",
                "LastUpdatedDate": "2018-11-09T05:03:13Z",
                "CreatedDate": "2018-11-09T05:03:13Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T05:03:12Z",
                        "LastTransactionDate": "2018-11-09T05:03:12Z",
                        "ConsentDate": "2018-11-09T05:03:12Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "7c128fb9-9d78-44c0-9ea0-f2fe0566991c",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "0027099c-b1f6-4d3e-8041-0c72aaf57158",
                "Identifier": "neemaot18+new@gmail.com",
                "LastUpdatedDate": "2018-11-09T05:04:12Z",
                "CreatedDate": "2018-11-09T05:04:12Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T05:04:12Z",
                        "LastTransactionDate": "2018-11-09T05:04:12Z",
                        "ConsentDate": "2018-11-09T05:04:12Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "7c128fb9-9d78-44c0-9ea0-f2fe0566991c",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "NO_CONSENT",
                        "FirstTransactionDate": "2018-11-09T05:04:12Z",
                        "LastTransactionDate": "2018-11-09T05:04:12Z",
                        "ExpiryDate": "2018-11-10T05:04:12Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "7c128fb9-9d78-44c0-9ea0-f2fe0566991c",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "6cfa965c-0006-4da5-b92d-704e7919015f",
                "Identifier": "test@onetrust.com",
                "LastUpdatedDate": "2018-11-09T05:20:34Z",
                "CreatedDate": "2018-11-09T05:13:21Z",
                "Purposes": [
                    {
                        "Id": "e36ff8aa-dbf8-4996-a7f1-cc4b966de362",
                        "Name": "sdk1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T05:13:20Z",
                        "LastTransactionDate": "2018-11-09T05:19:42Z",
                        "ConsentDate": "2018-11-09T05:13:20Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "952a2511-90dd-46cd-acad-cc24836ba8a6",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "917941cb-5dd4-4108-a9c5-0fa831a9a529",
                        "Name": "sdk2",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T05:13:20Z",
                        "LastTransactionDate": "2018-11-09T05:19:42Z",
                        "ConsentDate": "2018-11-09T05:13:20Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "952a2511-90dd-46cd-acad-cc24836ba8a6",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T05:20:33Z",
                        "LastTransactionDate": "2018-11-09T05:20:33Z",
                        "ConsentDate": "2018-11-09T05:20:33Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "ff05cf0f-31ce-412c-9beb-cb1b7313f5c8",
                        "LastTransactionCollectionPointVersion": 4
                    },
                    {
                        "Id": "2c7d3663-e1c5-4d82-bce4-a777e83cfda0",
                        "Name": "Purpose_123 v2",
                        "Version": 4,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T05:20:33Z",
                        "LastTransactionDate": "2018-11-09T05:20:33Z",
                        "ConsentDate": "2018-11-09T05:20:33Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "df17e795-6c63-48d8-8301-42ea30728fad",
                                "Name": "topic123",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "ff05cf0f-31ce-412c-9beb-cb1b7313f5c8",
                        "LastTransactionCollectionPointVersion": 4
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "6023bc50-5199-4283-884d-78b879b330bc",
                "Identifier": "Test",
                "LastUpdatedDate": "2018-11-09T05:16:50Z",
                "CreatedDate": "2018-11-09T05:16:50Z",
                "Purposes": [
                    {
                        "Id": "7b70ca4e-8224-45b3-9924-1b5214b56ab7",
                        "Name": "test12",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T05:16:50Z",
                        "LastTransactionDate": "2018-11-09T05:16:50Z",
                        "ConsentDate": "2018-11-09T05:16:50Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "e27052e2-15cc-47a6-9241-900bbfbfda20",
                                "Name": "!!!tp",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "05b58f5f-5042-465e-859c-9f2c8c24a11e",
                        "LastTransactionCollectionPointVersion": 2
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "597e45c2-cc62-438a-8741-3aa66b7580a1",
                "Identifier": "test@oonetrust.com",
                "LastUpdatedDate": "2018-11-09T05:18:26Z",
                "CreatedDate": "2018-11-09T05:18:26Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T05:18:25Z",
                        "LastTransactionDate": "2018-11-09T05:18:25Z",
                        "ConsentDate": "2018-11-09T05:18:25Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "38add7c6-ef9c-4b6d-8f3c-147c70124484",
                                "Name": "kbtopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "7c128fb9-9d78-44c0-9ea0-f2fe0566991c",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-11-09T05:18:25Z",
                        "LastTransactionDate": "2018-11-09T05:18:25Z",
                        "ConsentDate": "2018-11-09T05:18:25Z",
                        "ExpiryDate": "2018-11-09T05:18:25Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "1877891d-5f40-412d-8da0-16d1bf0f499b",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "7c128fb9-9d78-44c0-9ea0-f2fe0566991c",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "444c4b81-1efe-4b64-a897-bc856dab2789",
                "Identifier": "neemaot18+testhostedsdk@gmail.com",
                "LastUpdatedDate": "2018-11-09T05:21:27Z",
                "CreatedDate": "2018-11-09T05:21:27Z",
                "Purposes": [
                    {
                        "Id": "7b70ca4e-8224-45b3-9924-1b5214b56ab7",
                        "Name": "test12",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T05:21:26Z",
                        "LastTransactionDate": "2018-11-09T05:21:26Z",
                        "ConsentDate": "2018-11-09T05:21:26Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "05b58f5f-5042-465e-859c-9f2c8c24a11e",
                        "LastTransactionCollectionPointVersion": 2
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "ffe055e9-5a52-4852-94f5-5b75d975b133",
                "Identifier": "new@gmail.com",
                "LastUpdatedDate": "2018-11-09T05:25:33Z",
                "CreatedDate": "2018-11-09T05:25:32Z",
                "Purposes": [
                    {
                        "Id": "7b70ca4e-8224-45b3-9924-1b5214b56ab7",
                        "Name": "test12",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T05:25:31Z",
                        "LastTransactionDate": "2018-11-09T05:25:32Z",
                        "ConsentDate": "2018-11-09T05:25:31Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "05b58f5f-5042-465e-859c-9f2c8c24a11e",
                        "LastTransactionCollectionPointVersion": 2
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "f9ddeb61-c2d2-450b-b80b-3211300a0a3a",
                "Identifier": "03c4cb82-2a15-41b4-b57e-a2342b69b23d",
                "LastUpdatedDate": "2019-11-04T12:00:53Z",
                "CreatedDate": "2018-11-09T06:35:42Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-11-09T06:35:41Z",
                        "LastTransactionDate": "2019-11-04T12:00:53Z",
                        "ConsentDate": "2018-11-09T06:35:41Z",
                        "ExpiryDate": "2019-11-04T12:00:53Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "9acaccde-989f-4c86-8e74-fc6177c4463d",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "eeefd995-c4c4-4768-914e-eb6041b50158",
                "Identifier": "ae0c6df1-242a-48a2-888d-bb901a5e507b",
                "LastUpdatedDate": "2019-11-04T12:00:53Z",
                "CreatedDate": "2018-11-09T06:51:36Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-11-09T06:51:36Z",
                        "LastTransactionDate": "2019-11-04T12:00:53Z",
                        "ConsentDate": "2018-11-09T06:51:36Z",
                        "ExpiryDate": "2019-11-04T12:00:53Z",
                        "TotalTransactionCount": 3,
                        "LastTransactionCollectionPointId": "1cdecdf6-e93a-493f-b118-e5b9b5c4b099",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "e7012470-65c3-4d24-abca-372b467b4ca3",
                "Identifier": "1c7abd21-5791-4c50-8ccc-9c266c9955c3",
                "LastUpdatedDate": "2019-11-04T12:00:53Z",
                "CreatedDate": "2018-11-09T06:53:36Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-11-09T06:53:35Z",
                        "LastTransactionDate": "2019-11-04T12:00:53Z",
                        "ConsentDate": "2018-11-09T06:53:35Z",
                        "ExpiryDate": "2019-11-04T12:00:53Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "1cdecdf6-e93a-493f-b118-e5b9b5c4b099",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "6674ff48-5898-4258-984f-95d75b88ec54",
                "Identifier": "062399f4-3b87-4353-9529-00aefa19f0b7",
                "LastUpdatedDate": "2019-11-04T12:00:54Z",
                "CreatedDate": "2018-11-09T06:55:49Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-11-09T06:55:49Z",
                        "LastTransactionDate": "2019-11-04T12:00:53Z",
                        "ConsentDate": "2018-11-09T06:55:49Z",
                        "ExpiryDate": "2019-11-04T12:00:53Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "1cdecdf6-e93a-493f-b118-e5b9b5c4b099",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "ac3f17ab-7f84-4a09-89b0-6a00891685ca",
                "Identifier": "99d6a1d7-bde7-4cf7-a4a4-1aa4a2b46f31",
                "LastUpdatedDate": "2019-11-04T12:00:54Z",
                "CreatedDate": "2018-11-09T07:09:46Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-11-09T07:09:45Z",
                        "LastTransactionDate": "2019-11-04T12:00:53Z",
                        "ConsentDate": "2018-11-09T07:09:45Z",
                        "ExpiryDate": "2019-11-04T12:00:53Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "1cdecdf6-e93a-493f-b118-e5b9b5c4b099",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "56d46e64-b06c-491e-ab47-07fc04562bfe",
                "Identifier": "757e1a97-ee75-495d-a3ce-1ed606989ea1",
                "LastUpdatedDate": "2019-11-04T12:00:54Z",
                "CreatedDate": "2018-11-09T07:12:09Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-11-09T07:12:08Z",
                        "LastTransactionDate": "2019-11-04T12:00:53Z",
                        "ConsentDate": "2018-11-09T07:12:08Z",
                        "ExpiryDate": "2019-11-04T12:00:53Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "1cdecdf6-e93a-493f-b118-e5b9b5c4b099",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "76291a34-1da3-4f03-adb7-652f0d937cc5",
                "Identifier": "a67d6ede-666a-4984-94f3-565efe6b7b10",
                "LastUpdatedDate": "2019-11-04T12:00:54Z",
                "CreatedDate": "2018-11-09T07:12:37Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-11-09T07:12:37Z",
                        "LastTransactionDate": "2019-11-04T12:00:53Z",
                        "ConsentDate": "2018-11-09T07:12:37Z",
                        "ExpiryDate": "2019-11-04T12:00:53Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "1cdecdf6-e93a-493f-b118-e5b9b5c4b099",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "40b90404-d9b3-4f5e-ba99-bd711f87d788",
                "Identifier": "neemaot18+test5678@gmail.com",
                "LastUpdatedDate": "2018-11-09T09:14:55Z",
                "CreatedDate": "2018-11-09T09:14:55Z",
                "Purposes": [
                    {
                        "Id": "e36ff8aa-dbf8-4996-a7f1-cc4b966de362",
                        "Name": "sdk1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T09:14:55Z",
                        "LastTransactionDate": "2018-11-09T09:14:55Z",
                        "ConsentDate": "2018-11-09T09:14:55Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "01f93325-3a98-49e9-b187-8fc61ec6e72e",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "917941cb-5dd4-4108-a9c5-0fa831a9a529",
                        "Name": "sdk2",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T09:14:55Z",
                        "LastTransactionDate": "2018-11-09T09:14:55Z",
                        "ConsentDate": "2018-11-09T09:14:55Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "01f93325-3a98-49e9-b187-8fc61ec6e72e",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "ce8bb24b-7dcb-4670-a8b4-1ed293df0b23",
                "Identifier": "heyuserone@gmail.com",
                "LastUpdatedDate": "2019-07-31T04:04:38Z",
                "CreatedDate": "2018-11-09T11:27:03Z",
                "Purposes": [
                    {
                        "Id": "e36ff8aa-dbf8-4996-a7f1-cc4b966de362",
                        "Name": "sdk1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2018-11-09T11:27:02Z",
                        "LastTransactionDate": "2018-11-09T11:27:02Z",
                        "ConsentDate": "2018-11-09T11:27:02Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "620f54ce-394f-4c6a-9e5e-8a51d7760a7f",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "3d68b3c4-cd9f-4ec2-8be1-5379542127f9",
                        "Name": "Purpose_1007 v1",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-07-31T04:04:38Z",
                        "LastTransactionDate": "2019-07-31T04:04:38Z",
                        "ConsentDate": "2019-07-31T04:04:38Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "5b6ce10f-852f-4e7b-a664-9ccd3267d394",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "d82e5cea-6765-4625-8fff-109b4c0a1cd1",
                "Identifier": "df52f785-d452-4845-bed4-b48b2a2dfd06",
                "LastUpdatedDate": "2019-12-05T12:00:43Z",
                "CreatedDate": "2018-12-10T03:00:19Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-12-10T03:00:17Z",
                        "LastTransactionDate": "2019-12-05T12:00:42Z",
                        "ConsentDate": "2018-12-10T03:00:17Z",
                        "ExpiryDate": "2019-12-05T12:00:42Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "23e3bd22-6902-4e88-b505-9438d2ff57f8",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "88b0d9df-45ca-47a9-b770-9d6516ae5c9b",
                "Identifier": "9fff549d-1224-4b1e-b441-d861d3363f60",
                "LastUpdatedDate": "2019-12-05T12:00:43Z",
                "CreatedDate": "2018-12-10T03:02:30Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-12-10T03:02:28Z",
                        "LastTransactionDate": "2019-12-05T12:00:42Z",
                        "ConsentDate": "2018-12-10T03:02:28Z",
                        "ExpiryDate": "2019-12-05T12:00:42Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "23e3bd22-6902-4e88-b505-9438d2ff57f8",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "c69f42cf-f99e-4b6b-aa69-e53ebdf6e467",
                "Identifier": "b7b8d234-7b52-41f7-9965-d2225cdab872",
                "LastUpdatedDate": "2019-12-05T12:00:43Z",
                "CreatedDate": "2018-12-10T03:08:33Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-12-10T03:08:32Z",
                        "LastTransactionDate": "2019-12-05T12:00:42Z",
                        "ConsentDate": "2018-12-10T03:08:32Z",
                        "ExpiryDate": "2019-12-05T12:00:42Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "23e3bd22-6902-4e88-b505-9438d2ff57f8",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "2e0678eb-072f-4424-87ab-b75dba1c88f9",
                "Identifier": "c647b7b1-58b5-4e39-8e8a-3ff0911d0dd9",
                "LastUpdatedDate": "2019-12-05T12:00:43Z",
                "CreatedDate": "2018-12-10T03:14:36Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-12-10T03:14:36Z",
                        "LastTransactionDate": "2019-12-05T12:00:42Z",
                        "ConsentDate": "2018-12-10T03:14:36Z",
                        "ExpiryDate": "2019-12-05T12:00:42Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "6a17a06e-9133-4f9a-9e6a-0727ca844da5",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "5f2e7816-69c3-428d-9e60-d6dc6fe007ea",
                "Identifier": "d8d3d654-6a90-4526-9ed0-da3161be1926",
                "LastUpdatedDate": "2019-12-05T12:00:43Z",
                "CreatedDate": "2018-12-10T03:14:44Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-12-10T03:14:43Z",
                        "LastTransactionDate": "2019-12-05T12:00:42Z",
                        "ConsentDate": "2018-12-10T03:14:43Z",
                        "ExpiryDate": "2019-12-05T12:00:42Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "6a17a06e-9133-4f9a-9e6a-0727ca844da5",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "1a72c1b5-4d28-409e-900c-a0b792dbf409",
                "Identifier": "ec3b4f5f-1bab-47d7-8259-6cc97e4e09e8",
                "LastUpdatedDate": "2019-12-05T12:00:43Z",
                "CreatedDate": "2018-12-10T03:14:59Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-12-10T03:14:59Z",
                        "LastTransactionDate": "2019-12-05T12:00:42Z",
                        "ConsentDate": "2018-12-10T03:14:59Z",
                        "ExpiryDate": "2019-12-05T12:00:42Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "b336290f-94d2-4915-b5d6-a47e8fc8c183",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "4e382461-bc33-4dbf-aadd-b0549a9b6af7",
                "Identifier": "8b5cce28-df38-44e4-882e-59d8cff0c22c",
                "LastUpdatedDate": "2019-12-05T12:00:43Z",
                "CreatedDate": "2018-12-10T03:17:07Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2018-12-10T03:17:07Z",
                        "LastTransactionDate": "2019-12-05T12:00:42Z",
                        "ConsentDate": "2018-12-10T03:17:07Z",
                        "ExpiryDate": "2019-12-05T12:00:42Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "6a17a06e-9133-4f9a-9e6a-0727ca844da5",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "725a2c16-63cd-4b05-8625-8dbc3b252a2f",
                "Identifier": "ssayyed0401@onetrust.com",
                "LastUpdatedDate": "2019-01-04T06:49:01Z",
                "CreatedDate": "2019-01-04T06:41:01Z",
                "Purposes": [
                    {
                        "Id": "a705e32e-2b00-40d8-a1f9-a6be831b8e37",
                        "Name": "kbpurpose3",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-01-04T06:41:00Z",
                        "LastTransactionDate": "2019-01-04T06:49:01Z",
                        "ConsentDate": "2019-01-04T06:41:00Z",
                        "ExpiryDate": "2019-01-04T06:49:01Z",
                        "TotalTransactionCount": 4,
                        "Topics": [
                            {
                                "Id": "a81822bc-0bfb-4eec-bdf4-e26aefd67ea7",
                                "Name": "kbtopic3",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "6b034294-a267-41cb-b96f-95a762573ead",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "4cb65836-ef0a-4e91-bf31-ba35ba558312",
                "Identifier": "ssayyed0402@onetrust.com",
                "LastUpdatedDate": "2019-01-04T06:51:39Z",
                "CreatedDate": "2019-01-04T06:51:39Z",
                "Purposes": [
                    {
                        "Id": "a705e32e-2b00-40d8-a1f9-a6be831b8e37",
                        "Name": "kbpurpose3",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-01-04T06:51:38Z",
                        "LastTransactionDate": "2019-01-04T06:51:38Z",
                        "ConsentDate": "2019-01-04T06:51:38Z",
                        "ExpiryDate": "2019-01-04T06:51:38Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "a81822bc-0bfb-4eec-bdf4-e26aefd67ea7",
                                "Name": "kbtopic3",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "6b034294-a267-41cb-b96f-95a762573ead",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "fad7aafa-d8b9-4178-b88f-ed3ef6cdc188",
                "Identifier": "f42fc88e-f503-4237-981f-ef9cf2281671",
                "LastUpdatedDate": "2019-12-30T12:00:29Z",
                "CreatedDate": "2019-01-04T07:24:40Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-01-04T07:24:39Z",
                        "LastTransactionDate": "2019-12-30T12:00:28Z",
                        "ConsentDate": "2019-01-04T07:24:39Z",
                        "ExpiryDate": "2019-12-30T12:00:28Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "b336290f-94d2-4915-b5d6-a47e8fc8c183",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "df251ca0-65c7-4fc5-be4f-12e4b6846fc3",
                "Identifier": "7db58a52-ac09-4f25-9f54-be112131479f",
                "LastUpdatedDate": "2019-12-30T12:00:29Z",
                "CreatedDate": "2019-01-04T07:26:04Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-01-04T07:26:04Z",
                        "LastTransactionDate": "2019-12-30T12:00:28Z",
                        "ConsentDate": "2019-01-04T07:26:04Z",
                        "ExpiryDate": "2019-12-30T12:00:28Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "b336290f-94d2-4915-b5d6-a47e8fc8c183",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "aca3e560-cb1e-4466-ad03-674367deb23c",
                "Identifier": "a20a6672-0d71-47d6-bb01-1181487ad1ea",
                "LastUpdatedDate": "2019-12-30T12:00:29Z",
                "CreatedDate": "2019-01-04T07:30:50Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-01-04T07:30:49Z",
                        "LastTransactionDate": "2019-12-30T12:00:28Z",
                        "ConsentDate": "2019-01-04T07:30:49Z",
                        "ExpiryDate": "2019-12-30T12:00:28Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "b336290f-94d2-4915-b5d6-a47e8fc8c183",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "e50c5cb4-3c15-488d-b19e-41ffb4401228",
                "Identifier": "govaswuat@onet.com",
                "LastUpdatedDate": "2019-01-04T12:57:25Z",
                "CreatedDate": "2019-01-04T12:57:25Z",
                "Purposes": [
                    {
                        "Id": "a705e32e-2b00-40d8-a1f9-a6be831b8e37",
                        "Name": "kbpurpose3",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-01-04T12:57:24Z",
                        "LastTransactionDate": "2019-01-04T12:57:24Z",
                        "ConsentDate": "2019-01-04T12:57:24Z",
                        "ExpiryDate": "2019-01-04T12:57:24Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "a81822bc-0bfb-4eec-bdf4-e26aefd67ea7",
                                "Name": "kbtopic3",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "a466a07b-118b-4a98-869b-49f224eac8e8",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "d1e3586e-9161-4a3b-9912-961956de65f8",
                "Identifier": "dcbae393-2b72-430b-ae63-58f43736893e",
                "LastUpdatedDate": "2020-01-20T12:00:25Z",
                "CreatedDate": "2019-01-25T03:40:25Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-01-25T03:40:19Z",
                        "LastTransactionDate": "2020-01-20T12:00:23Z",
                        "ConsentDate": "2019-01-25T03:40:19Z",
                        "ExpiryDate": "2020-01-20T12:00:23Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "a7664ab2-d1a5-462c-a933-2bcf31b3143d",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "de3d90fa-75b8-4d12-afd1-d552faee65b5",
                "Identifier": "slazar@onetrust.com",
                "LastUpdatedDate": "2019-02-07T17:48:46Z",
                "CreatedDate": "2019-02-07T17:44:41Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-02-07T17:44:45Z",
                        "LastTransactionDate": "2019-02-07T17:48:54Z",
                        "ConsentDate": "2019-02-07T17:44:45Z",
                        "TotalTransactionCount": 2,
                        "Topics": [
                            {
                                "Id": "38add7c6-ef9c-4b6d-8f3c-147c70124484",
                                "Name": "kbtopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "d28ba3e3-0a9f-48e5-9dbb-28eb256b7bdb",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "18b12ec2-3cf3-456d-a1d1-745a88d2393f",
                "Identifier": "0a30b5f4-01dd-45fb-9c04-fc4ac75a09a3",
                "LastUpdatedDate": "2020-02-03T12:00:30Z",
                "CreatedDate": "2019-02-08T02:54:15Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-02-08T02:54:19Z",
                        "LastTransactionDate": "2020-02-03T12:00:28Z",
                        "ConsentDate": "2019-02-08T02:54:19Z",
                        "ExpiryDate": "2020-02-03T12:00:28Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "e8332de9-f921-457e-b485-c4d72d663fd4",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "35f7827d-97ad-4ec4-826b-f2d0f33e6458",
                "Identifier": "3189c513-f54a-49f7-a03d-60dac19beb05",
                "LastUpdatedDate": "2019-02-14T03:29:01Z",
                "CreatedDate": "2019-02-14T03:28:27Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-02-14T03:28:20Z",
                        "LastTransactionDate": "2019-02-14T03:28:44Z",
                        "ConsentDate": "2019-02-14T03:28:20Z",
                        "ExpiryDate": "2020-02-09T03:28:44Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "d1f82242-e3dc-4918-9cb0-b1c4d1a57d9e",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "15cc286e-3c6f-4c11-8cb6-99b973acedee",
                "Identifier": "e9668c20-cab2-41aa-a532-c58bf0d9043f",
                "LastUpdatedDate": "2019-02-14T03:30:46Z",
                "CreatedDate": "2019-02-14T03:30:46Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-02-14T03:31:00Z",
                        "LastTransactionDate": "2019-02-14T03:31:00Z",
                        "ConsentDate": "2019-02-14T03:31:00Z",
                        "ExpiryDate": "2020-02-09T03:31:00Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "d1f82242-e3dc-4918-9cb0-b1c4d1a57d9e",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "655a95ef-75d6-44c3-a6da-23174d47d144",
                "Identifier": "c6d07198-08b7-419b-9416-e296bb1efd35",
                "LastUpdatedDate": "2019-02-19T03:04:15Z",
                "CreatedDate": "2019-02-19T03:04:15Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-02-19T03:04:13Z",
                        "LastTransactionDate": "2019-02-19T03:04:13Z",
                        "ConsentDate": "2019-02-19T03:04:13Z",
                        "ExpiryDate": "2020-02-14T03:04:13Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "d1f82242-e3dc-4918-9cb0-b1c4d1a57d9e",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "cdcd2f02-5ac6-452b-a1a0-65216d74e585",
                "Identifier": "8e740289-ab90-49ad-86e6-fa7b60daacd3",
                "LastUpdatedDate": "2019-02-19T03:06:21Z",
                "CreatedDate": "2019-02-19T03:06:21Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-02-19T03:06:21Z",
                        "LastTransactionDate": "2019-02-19T03:06:21Z",
                        "ConsentDate": "2019-02-19T03:06:21Z",
                        "ExpiryDate": "2020-02-14T03:06:21Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "d1f82242-e3dc-4918-9cb0-b1c4d1a57d9e",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "219c8fa5-c9c4-4231-9422-d81e573d2ae1",
                "Language": "en-us",
                "Identifier": "consentot1@gmail.com",
                "LastUpdatedDate": "2020-07-29T11:38:04Z",
                "CreatedDate": "2019-02-27T06:08:18Z",
                "Purposes": [
                    {
                        "Id": "723300e3-96cb-4da4-abdd-4913c05be215",
                        "Name": "Purpose 1",
                        "Version": 2,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2019-02-27T06:08:18Z",
                        "LastTransactionDate": "2019-02-27T06:17:04Z",
                        "WithdrawalDate": "2019-02-27T06:17:04Z",
                        "ConsentDate": "2019-02-27T06:08:18Z",
                        "TotalTransactionCount": 3,
                        "LastTransactionCollectionPointId": "3110507d-4516-4c2e-b6b4-50161c5aaacc",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "41ebdf32-068b-4239-89ac-42e20262c5a9",
                        "LastReceiptId": "e9667aae-67d9-4383-b343-27681d5f71f9",
                        "Name": "Send Notifications about Changes to Preferences",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-02-27T06:10:08Z",
                        "LastTransactionDate": "2020-07-29T11:38:03Z",
                        "ConsentDate": "2019-02-27T06:10:08Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "894247c4-fc5b-4b45-a7ce-628178c79587",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "759d8b24-e4c0-4107-86e2-3a408db13e6b",
                        "Name": "GJ Purpose 07",
                        "Version": 2,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2019-03-28T03:15:48Z",
                        "LastTransactionDate": "2019-03-28T03:15:48Z",
                        "ConsentDate": "2019-03-28T03:15:48Z",
                        "ExpiryDate": "2019-03-30T03:15:48Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "afcd5703-1247-4f5b-b0da-8d75e2986165",
                                "Name": "Eloqua_topic",
                                "IsConsented": true
                            },
                            {
                                "Id": "1a774b37-9733-4594-8656-fc5b799001ad",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "CustomPreferences": [
                            {
                                "Id": "6a4d4c03-0b4e-445c-8033-4f84210d91f7",
                                "Name": "Gj Preference ",
                                "Options": [
                                    {
                                        "Id": "a40b233c-2c5c-496c-a100-4f46a4104810",
                                        "Name": "Option 2",
                                        "IsConsented": true
                                    },
                                    {
                                        "Id": "cda3a2af-d6f4-4af4-9d5c-325e3d121c80",
                                        "Name": "Option 1",
                                        "IsConsented": true
                                    }
                                ]
                            }
                        ],
                        "LastTransactionCollectionPointId": "87ba854e-713b-4b88-8dee-3c537830c275",
                        "LastTransactionCollectionPointVersion": 3
                    },
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "NO_CONSENT",
                        "FirstTransactionDate": "2019-03-28T03:15:48Z",
                        "LastTransactionDate": "2019-03-28T03:15:48Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "87ba854e-713b-4b88-8dee-3c537830c275",
                        "LastTransactionCollectionPointVersion": 3
                    },
                    {
                        "Id": "7a08dc31-8e5a-4d92-ab93-5af6528af046",
                        "Name": "govi47p",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-05T11:40:18Z",
                        "LastTransactionDate": "2019-04-05T11:40:18Z",
                        "ConsentDate": "2019-04-05T11:40:18Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "3cedc4f1-40ae-4ffc-b55b-f75c252f0b4f",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "a313353a-e13b-4554-be08-22cf4a78a31c",
                        "Name": "Purpose_1804 v2",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-18T03:45:30Z",
                        "LastTransactionDate": "2019-05-08T03:00:08Z",
                        "ConsentDate": "2019-04-18T03:45:30Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "8e79794e-639f-4170-b695-44a5be165d07",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "c8fdae1a-9247-4a00-b042-4de69f3980d2",
                        "LastReceiptId": "e9667aae-67d9-4383-b343-27681d5f71f9",
                        "Name": "5.0Purp",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2020-07-29T11:38:03Z",
                        "LastTransactionDate": "2020-07-29T11:38:03Z",
                        "ConsentDate": "2020-07-29T11:38:03Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "e161b920-4b38-4adf-8ec0-a146aef96793",
                                "Name": "5.0Topic1",
                                "IsConsented": true
                            },
                            {
                                "Id": "ecac9d4d-f193-4696-bf64-714e91b0f583",
                                "Name": "5.0Topic2",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "894247c4-fc5b-4b45-a7ce-628178c79587",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "6b056ad0-a1f5-4af3-ab65-eb749455a6de",
                        "LastReceiptId": "e9667aae-67d9-4383-b343-27681d5f71f9",
                        "Name": "Analytics",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2020-07-29T11:38:03Z",
                        "LastTransactionDate": "2020-07-29T11:38:03Z",
                        "ConsentDate": "2020-07-29T11:38:03Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "894247c4-fc5b-4b45-a7ce-628178c79587",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "f7f1ce4a-919e-47b2-8445-80a4404b4499",
                "Identifier": "Ds1",
                "LastUpdatedDate": "2019-03-07T03:36:11Z",
                "CreatedDate": "2019-02-27T14:03:35Z",
                "Purposes": [
                    {
                        "Id": "723300e3-96cb-4da4-abdd-4913c05be215",
                        "Name": "Purpose 1",
                        "Version": 2,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2019-02-27T14:03:35Z",
                        "LastTransactionDate": "2019-02-27T14:03:35Z",
                        "ConsentDate": "2019-02-27T14:03:35Z",
                        "ExpiryDate": "2019-02-28T14:03:35Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "3110507d-4516-4c2e-b6b4-50161c5aaacc",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "759d8b24-e4c0-4107-86e2-3a408db13e6b",
                        "Name": "GJ Purpose 07",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2019-03-07T03:35:27Z",
                        "LastTransactionDate": "2019-03-07T03:36:01Z",
                        "WithdrawalDate": "2019-03-07T03:36:01Z",
                        "ConsentDate": "2019-03-07T03:35:27Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "f8886377-e4b1-45e4-b1c0-d7dacb744db8",
                        "LastTransactionCollectionPointVersion": 3
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "888f56db-77e2-431b-a4ea-453106852567",
                "Language": "en-us",
                "Identifier": "cmaddala@onetrust.com",
                "LastUpdatedDate": "2020-06-28T00:00:35Z",
                "CreatedDate": "2019-03-05T19:50:19Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-05T19:50:19Z",
                        "LastTransactionDate": "2019-03-05T19:53:57Z",
                        "ConsentDate": "2019-03-05T19:50:19Z",
                        "TotalTransactionCount": 2,
                        "Topics": [
                            {
                                "Id": "38add7c6-ef9c-4b6d-8f3c-147c70124484",
                                "Name": "kbtopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "d28ba3e3-0a9f-48e5-9dbb-28eb256b7bdb",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-03-05T19:53:57Z",
                        "LastTransactionDate": "2020-06-28T00:00:33Z",
                        "ConsentDate": "2019-03-05T19:53:57Z",
                        "ExpiryDate": "2020-06-28T00:00:33Z",
                        "TotalTransactionCount": 3,
                        "Topics": [
                            {
                                "Id": "1877891d-5f40-412d-8da0-16d1bf0f499b",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "0b842f56-c02b-4747-9946-bc92ac67b7d6",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "be160f0a-f61e-406c-8dcd-acb3118ab3bd",
                "Language": "en-us",
                "Identifier": "dkumar@onetrust.com",
                "LastUpdatedDate": "2020-08-20T00:01:07Z",
                "CreatedDate": "2019-03-05T20:41:28Z",
                "Purposes": [
                    {
                        "Id": "723300e3-96cb-4da4-abdd-4913c05be215",
                        "Name": "Purpose 1",
                        "Version": 2,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-03-05T20:41:11Z",
                        "LastTransactionDate": "2019-03-07T00:02:10Z",
                        "ConsentDate": "2019-03-05T20:41:11Z",
                        "ExpiryDate": "2019-03-07T00:02:10Z",
                        "TotalTransactionCount": 3,
                        "LastTransactionCollectionPointId": "3110507d-4516-4c2e-b6b4-50161c5aaacc",
                        "LastTransactionCollectionPointVersion": 2
                    },
                    {
                        "Id": "c8fdae1a-9247-4a00-b042-4de69f3980d2",
                        "LastReceiptId": "3dae1679-1702-4aa8-9e39-86e97cf0f018",
                        "Name": "5.0Purp",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2020-05-14T13:49:05Z",
                        "LastTransactionDate": "2020-08-17T23:48:00Z",
                        "WithdrawalDate": "2020-06-05T09:56:15Z",
                        "ConsentDate": "2020-08-17T23:48:00Z",
                        "TotalTransactionCount": 9,
                        "Topics": [
                            {
                                "Id": "e161b920-4b38-4adf-8ec0-a146aef96793",
                                "Name": "5.0Topic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "f4d737b0-4b77-4063-aae9-2d34ccf1bca2",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "759d8b24-e4c0-4107-86e2-3a408db13e6b",
                        "LastReceiptId": "d756230f-c8d9-4889-8fda-9f32e50c1a2d",
                        "Name": "GJ Purpose 07",
                        "Version": 2,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2020-05-14T13:49:05Z",
                        "LastTransactionDate": "2020-08-20T00:00:53Z",
                        "ConsentDate": "2020-08-17T23:48:00Z",
                        "ExpiryDate": "2020-08-20T00:00:53Z",
                        "TotalTransactionCount": 4,
                        "Topics": [
                            {
                                "Id": "afcd5703-1247-4f5b-b0da-8d75e2986165",
                                "Name": "Eloqua_topic",
                                "IsConsented": true
                            },
                            {
                                "Id": "1a774b37-9733-4594-8656-fc5b799001ad",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "f4d737b0-4b77-4063-aae9-2d34ccf1bca2",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "3ecdc14c-04e2-4dd4-83f1-42f8c19fb93d",
                        "Name": "Test purp 06",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2020-05-14T19:30:35Z",
                        "LastTransactionDate": "2020-05-14T19:30:35Z",
                        "ConsentDate": "2020-05-14T19:30:35Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "867d9cdb-2e98-4eb2-a5ac-56daa2736b1f",
                                "Name": "       Topic_1804",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "8b5157e4-c1ee-452c-9b5e-b692c26632f4",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "22e8673d-4d21-43c2-a370-2bf1eddf6bd4",
                "Identifier": "b36bb8ca-355e-483d-81d6-5267a75a40e5",
                "LastUpdatedDate": "2019-03-07T03:22:28Z",
                "CreatedDate": "2019-03-07T03:22:28Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-07T03:22:11Z",
                        "LastTransactionDate": "2019-03-07T03:22:11Z",
                        "ConsentDate": "2019-03-07T03:22:11Z",
                        "ExpiryDate": "2020-03-01T03:22:11Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "e8332de9-f921-457e-b485-c4d72d663fd4",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "91412611-6630-4c1d-8a20-fa2bb35d878e",
                "Identifier": "f1cb7796-9a43-488c-bd9b-216ef7442763",
                "LastUpdatedDate": "2019-03-07T03:22:45Z",
                "CreatedDate": "2019-03-07T03:22:45Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-07T03:22:56Z",
                        "LastTransactionDate": "2019-03-07T03:22:56Z",
                        "ConsentDate": "2019-03-07T03:22:56Z",
                        "ExpiryDate": "2020-03-01T03:22:56Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "53cae93a-6432-46bd-baa2-6c4b1b2f440a",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "72276d80-519c-4694-84aa-25d2ccdc5c59",
                "Identifier": "DS2",
                "LastUpdatedDate": "2019-03-07T03:36:23Z",
                "CreatedDate": "2019-03-07T03:35:28Z",
                "Purposes": [
                    {
                        "Id": "759d8b24-e4c0-4107-86e2-3a408db13e6b",
                        "Name": "GJ Purpose 07",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2019-03-07T03:35:28Z",
                        "LastTransactionDate": "2019-03-07T03:36:12Z",
                        "WithdrawalDate": "2019-03-07T03:36:12Z",
                        "ConsentDate": "2019-03-07T03:35:28Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "f8886377-e4b1-45e4-b1c0-d7dacb744db8",
                        "LastTransactionCollectionPointVersion": 3
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "eea09632-e4e0-46bb-ae6e-31b802af5fd0",
                "Identifier": "gkumar@onertrust.com",
                "LastUpdatedDate": "2019-03-07T03:50:38Z",
                "CreatedDate": "2019-03-07T03:50:38Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-07T03:50:39Z",
                        "LastTransactionDate": "2019-03-07T03:50:39Z",
                        "ConsentDate": "2019-03-07T03:50:39Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "f8886377-e4b1-45e4-b1c0-d7dacb744db8",
                        "LastTransactionCollectionPointVersion": 4
                    },
                    {
                        "Id": "a36f98d0-c662-4f61-a2a1-8bdab69e3c3a",
                        "Name": "Pur 1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-07T03:50:39Z",
                        "LastTransactionDate": "2019-03-07T03:50:39Z",
                        "ConsentDate": "2019-03-07T03:50:39Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "f89c2b49-17e2-4bd3-bc07-5224811c9c52",
                                "Name": "kbtopic2",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "f8886377-e4b1-45e4-b1c0-d7dacb744db8",
                        "LastTransactionCollectionPointVersion": 4
                    },
                    {
                        "Id": "4d52dcc4-82bf-44bf-ac49-854eba6ff8f3",
                        "Name": "Eloqua_UAT",
                        "Version": 1,
                        "Status": "NO_CONSENT",
                        "FirstTransactionDate": "2019-03-07T03:50:39Z",
                        "LastTransactionDate": "2019-03-07T03:50:39Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "f8886377-e4b1-45e4-b1c0-d7dacb744db8",
                        "LastTransactionCollectionPointVersion": 4
                    },
                    {
                        "Id": "759d8b24-e4c0-4107-86e2-3a408db13e6b",
                        "Name": "GJ Purpose 07",
                        "Version": 1,
                        "Status": "NO_CONSENT",
                        "FirstTransactionDate": "2019-03-07T03:50:39Z",
                        "LastTransactionDate": "2019-03-07T03:50:39Z",
                        "ExpiryDate": "2019-03-09T03:50:39Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "f8886377-e4b1-45e4-b1c0-d7dacb744db8",
                        "LastTransactionCollectionPointVersion": 4
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "589ef1bb-0b9f-4004-81d5-63ccbe578c89",
                "Identifier": "mbarron@onetrust.com",
                "LastUpdatedDate": "2019-03-11T22:45:25Z",
                "CreatedDate": "2019-03-11T22:42:44Z",
                "Purposes": [
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2019-03-11T22:42:45Z",
                        "LastTransactionDate": "2019-03-11T22:45:24Z",
                        "WithdrawalDate": "2019-03-11T22:45:24Z",
                        "ConsentDate": "2019-03-11T22:42:45Z",
                        "TotalTransactionCount": 3,
                        "LastTransactionCollectionPointId": "d28ba3e3-0a9f-48e5-9dbb-28eb256b7bdb",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-11T22:42:45Z",
                        "LastTransactionDate": "2019-03-11T22:43:12Z",
                        "ConsentDate": "2019-03-11T22:42:45Z",
                        "TotalTransactionCount": 2,
                        "Topics": [
                            {
                                "Id": "38add7c6-ef9c-4b6d-8f3c-147c70124484",
                                "Name": "kbtopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "d28ba3e3-0a9f-48e5-9dbb-28eb256b7bdb",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "8c52c62b-dd35-43f3-a0da-4bcef1b137e1",
                "Identifier": "416a373f-f861-47ae-b288-d7ad49c82ccd",
                "LastUpdatedDate": "2019-03-12T02:52:59Z",
                "CreatedDate": "2019-03-12T02:52:59Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-12T02:52:51Z",
                        "LastTransactionDate": "2019-03-12T02:52:51Z",
                        "ConsentDate": "2019-03-12T02:52:51Z",
                        "ExpiryDate": "2020-03-06T02:52:51Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "53cae93a-6432-46bd-baa2-6c4b1b2f440a",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "5555c2bf-ba69-4425-ba80-326e9825fc87",
                "Identifier": "13d6ae0e-daad-4390-be21-0b00bf2d47df",
                "LastUpdatedDate": "2019-03-12T02:53:14Z",
                "CreatedDate": "2019-03-12T02:53:14Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-12T02:53:29Z",
                        "LastTransactionDate": "2019-03-12T02:53:29Z",
                        "ConsentDate": "2019-03-12T02:53:29Z",
                        "ExpiryDate": "2020-03-06T02:53:29Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "53cae93a-6432-46bd-baa2-6c4b1b2f440a",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "311ad5ce-12fc-4354-8629-6f61bd1d842a",
                "Identifier": "fac6a378-32cf-4d60-9991-b556e8e3fdda",
                "LastUpdatedDate": "2019-03-12T09:57:25Z",
                "CreatedDate": "2019-03-12T09:57:25Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-12T09:57:16Z",
                        "LastTransactionDate": "2019-03-12T09:57:16Z",
                        "ConsentDate": "2019-03-12T09:57:16Z",
                        "ExpiryDate": "2020-03-06T09:57:16Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "53cae93a-6432-46bd-baa2-6c4b1b2f440a",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "fc29bc8a-93a9-4240-bd12-453e259980fa",
                "Identifier": "21e064a2-74be-4ddb-83f6-0c0dc39a72ac",
                "LastUpdatedDate": "2019-03-12T09:57:43Z",
                "CreatedDate": "2019-03-12T09:57:43Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-12T09:57:21Z",
                        "LastTransactionDate": "2019-03-12T09:57:21Z",
                        "ConsentDate": "2019-03-12T09:57:21Z",
                        "ExpiryDate": "2020-03-06T09:57:21Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "53cae93a-6432-46bd-baa2-6c4b1b2f440a",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "7402d64a-62ee-4a9c-bff8-86847ff5aa6f",
                "Identifier": "rfr",
                "LastUpdatedDate": "2019-03-28T03:11:07Z",
                "CreatedDate": "2019-03-28T03:11:07Z",
                "Purposes": [
                    {
                        "Id": "4d52dcc4-82bf-44bf-ac49-854eba6ff8f3",
                        "Name": "Eloqua_UAT",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-28T03:11:06Z",
                        "LastTransactionDate": "2019-03-28T03:11:06Z",
                        "ConsentDate": "2019-03-28T03:11:06Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "690ad782-6280-4ea4-b62a-1514c39fc838",
                                "Name": "Eloqua_topic",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "f8886377-e4b1-45e4-b1c0-d7dacb744db8",
                        "LastTransactionCollectionPointVersion": 4
                    },
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "NO_CONSENT",
                        "FirstTransactionDate": "2019-03-28T03:11:06Z",
                        "LastTransactionDate": "2019-03-28T03:11:06Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "f8886377-e4b1-45e4-b1c0-d7dacb744db8",
                        "LastTransactionCollectionPointVersion": 4
                    },
                    {
                        "Id": "a36f98d0-c662-4f61-a2a1-8bdab69e3c3a",
                        "Name": "Pur 1",
                        "Version": 1,
                        "Status": "NO_CONSENT",
                        "FirstTransactionDate": "2019-03-28T03:11:06Z",
                        "LastTransactionDate": "2019-03-28T03:11:06Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "f8886377-e4b1-45e4-b1c0-d7dacb744db8",
                        "LastTransactionCollectionPointVersion": 4
                    },
                    {
                        "Id": "759d8b24-e4c0-4107-86e2-3a408db13e6b",
                        "Name": "GJ Purpose 07",
                        "Version": 1,
                        "Status": "NO_CONSENT",
                        "FirstTransactionDate": "2019-03-28T03:11:06Z",
                        "LastTransactionDate": "2019-03-28T03:11:06Z",
                        "ExpiryDate": "2019-03-30T03:11:06Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "f8886377-e4b1-45e4-b1c0-d7dacb744db8",
                        "LastTransactionCollectionPointVersion": 4
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "68c0db4d-ed92-4244-8cde-2375dcc8cb17",
                "Identifier": "govi47p",
                "LastUpdatedDate": "2019-03-28T03:12:07Z",
                "CreatedDate": "2019-03-28T03:12:07Z",
                "Purposes": [
                    {
                        "Id": "7a08dc31-8e5a-4d92-ab93-5af6528af046",
                        "Name": "govi47p",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-28T03:12:01Z",
                        "LastTransactionDate": "2019-03-28T03:12:01Z",
                        "ConsentDate": "2019-03-28T03:12:01Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "3cedc4f1-40ae-4ffc-b55b-f75c252f0b4f",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "a10aacc9-fa28-4b32-93ff-6f685cea9bd5",
                "Identifier": "097cecdc-a9ff-4cf8-a55b-f2a30b98687c",
                "LastUpdatedDate": "2019-03-28T03:13:20Z",
                "CreatedDate": "2019-03-28T03:13:20Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-28T03:13:15Z",
                        "LastTransactionDate": "2019-03-28T03:13:15Z",
                        "ConsentDate": "2019-03-28T03:13:15Z",
                        "ExpiryDate": "2020-03-22T03:13:15Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "67a31c06-67db-429b-829f-9a5010971fe0",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "d32aaa76-8eb8-4983-a55e-a5ba5c47b842",
                "Identifier": "Gjexample@otprivacy.com",
                "LastUpdatedDate": "2019-03-28T03:21:43Z",
                "CreatedDate": "2019-03-28T03:21:43Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2019-03-28T03:21:29Z",
                        "LastTransactionDate": "2019-03-28T03:21:29Z",
                        "ConsentDate": "2019-03-28T03:21:29Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "38add7c6-ef9c-4b6d-8f3c-147c70124484",
                                "Name": "kbtopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "87ba854e-713b-4b88-8dee-3c537830c275",
                        "LastTransactionCollectionPointVersion": 3
                    },
                    {
                        "Id": "759d8b24-e4c0-4107-86e2-3a408db13e6b",
                        "Name": "GJ Purpose 07",
                        "Version": 2,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2019-03-28T03:21:29Z",
                        "LastTransactionDate": "2019-03-28T03:21:29Z",
                        "ConsentDate": "2019-03-28T03:21:29Z",
                        "ExpiryDate": "2019-03-30T03:21:29Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "afcd5703-1247-4f5b-b0da-8d75e2986165",
                                "Name": "Eloqua_topic",
                                "IsConsented": true
                            },
                            {
                                "Id": "1a774b37-9733-4594-8656-fc5b799001ad",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "CustomPreferences": [
                            {
                                "Id": "6a4d4c03-0b4e-445c-8033-4f84210d91f7",
                                "Name": "Gj Preference ",
                                "Options": [
                                    {
                                        "Id": "cda3a2af-d6f4-4af4-9d5c-325e3d121c80",
                                        "Name": "Option 1",
                                        "IsConsented": true
                                    },
                                    {
                                        "Id": "a40b233c-2c5c-496c-a100-4f46a4104810",
                                        "Name": "Option 2",
                                        "IsConsented": true
                                    }
                                ]
                            }
                        ],
                        "LastTransactionCollectionPointId": "87ba854e-713b-4b88-8dee-3c537830c275",
                        "LastTransactionCollectionPointVersion": 3
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "c005a5e1-2ac3-4520-95f8-3ee97d07b7f0",
                "Identifier": "govi477@gmail.com",
                "LastUpdatedDate": "2019-03-28T03:22:02Z",
                "CreatedDate": "2019-03-28T03:22:02Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-28T03:22:12Z",
                        "LastTransactionDate": "2019-03-28T03:22:12Z",
                        "ConsentDate": "2019-03-28T03:22:12Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "38add7c6-ef9c-4b6d-8f3c-147c70124484",
                                "Name": "kbtopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "ac3bea9e-c509-4442-9cd5-569150abf4fb",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "fc44ac74-f059-4c37-bd9b-68fe92f63aec",
                "Identifier": "blnandini6@gmail.com",
                "LastUpdatedDate": "2019-05-10T08:20:16Z",
                "CreatedDate": "2019-03-28T03:23:38Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-28T03:23:48Z",
                        "LastTransactionDate": "2019-03-28T03:23:48Z",
                        "ConsentDate": "2019-03-28T03:23:48Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "38add7c6-ef9c-4b6d-8f3c-147c70124484",
                                "Name": "kbtopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "ac3bea9e-c509-4442-9cd5-569150abf4fb",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "7a08dc31-8e5a-4d92-ab93-5af6528af046",
                        "Name": "govi47p",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-28T04:19:33Z",
                        "LastTransactionDate": "2019-03-28T04:20:27Z",
                        "ConsentDate": "2019-03-28T04:19:33Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "3cedc4f1-40ae-4ffc-b55b-f75c252f0b4f",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "17b34a38-4fff-42e6-9653-d96b58514855",
                        "Name": "purpose-11",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-05-10T08:20:58Z",
                        "LastTransactionDate": "2019-05-10T08:20:58Z",
                        "ConsentDate": "2019-05-10T08:20:58Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "94550faa-4dec-4882-b800-4a468bd76114",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "d7176d34-a31b-4414-81e1-4ecee81c7f41",
                "Identifier": "govi4757@gmail.com",
                "LastUpdatedDate": "2019-03-28T03:24:03Z",
                "CreatedDate": "2019-03-28T03:24:03Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-28T03:23:49Z",
                        "LastTransactionDate": "2019-03-28T03:23:49Z",
                        "ConsentDate": "2019-03-28T03:23:49Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "38add7c6-ef9c-4b6d-8f3c-147c70124484",
                                "Name": "kbtopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "ac3bea9e-c509-4442-9cd5-569150abf4fb",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "f5865a24-63ed-467d-a87a-ba9211553b7b",
                "Identifier": "blnandini62803@gmail.com",
                "LastUpdatedDate": "2019-03-28T04:02:32Z",
                "CreatedDate": "2019-03-28T04:02:32Z",
                "Purposes": [
                    {
                        "Id": "7a08dc31-8e5a-4d92-ab93-5af6528af046",
                        "Name": "govi47p",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-28T04:02:17Z",
                        "LastTransactionDate": "2019-03-28T04:02:17Z",
                        "ConsentDate": "2019-03-28T04:02:17Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "3cedc4f1-40ae-4ffc-b55b-f75c252f0b4f",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "7b63dccf-276d-4fb3-bc66-1ea9d01335b9",
                "Identifier": "Nand2803@otprivacy.com",
                "LastUpdatedDate": "2019-03-28T04:14:55Z",
                "CreatedDate": "2019-03-28T04:14:55Z",
                "DataElements": [
                    {
                        "Name": "FirstName",
                        "Value": "Nandini",
                        "Linked": false
                    },
                    {
                        "Name": "LastName",
                        "Value": "B L",
                        "Linked": false
                    }
                ],
                "Purposes": [
                    {
                        "Id": "7a08dc31-8e5a-4d92-ab93-5af6528af046",
                        "Name": "govi47p",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-28T04:15:03Z",
                        "LastTransactionDate": "2019-03-28T04:15:03Z",
                        "ConsentDate": "2019-03-28T04:15:03Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "3cedc4f1-40ae-4ffc-b55b-f75c252f0b4f",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "b4966cb2-7d2c-4a09-81a1-4ea7a7aed94a",
                "Identifier": "c75fdacb-5137-4982-bf4a-8fef1a9f44a7",
                "LastUpdatedDate": "2019-03-29T06:55:56Z",
                "CreatedDate": "2019-03-29T06:55:56Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-29T06:55:49Z",
                        "LastTransactionDate": "2019-03-29T06:55:49Z",
                        "ConsentDate": "2019-03-29T06:55:49Z",
                        "ExpiryDate": "2020-03-23T06:55:49Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "67a31c06-67db-429b-829f-9a5010971fe0",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "aaf3e43b-3bb2-4c09-b671-8aec4dfb8300",
                "Identifier": "6846071c-b60e-4ffc-86f8-f4cdeebb2c0f",
                "LastUpdatedDate": "2019-03-29T07:29:06Z",
                "CreatedDate": "2019-03-29T07:29:06Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-03-29T07:28:51Z",
                        "LastTransactionDate": "2019-03-29T07:28:51Z",
                        "ConsentDate": "2019-03-29T07:28:51Z",
                        "ExpiryDate": "2020-03-23T07:28:51Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "67a31c06-67db-429b-829f-9a5010971fe0",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "fbfd5454-8b3f-4bc7-8e8b-6cea81807078",
                "Identifier": "c3343b8b-b42b-472a-8a1c-2aafbf3206a6",
                "LastUpdatedDate": "2019-04-01T03:03:11Z",
                "CreatedDate": "2019-04-01T03:03:11Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-01T03:03:11Z",
                        "LastTransactionDate": "2019-04-01T03:03:11Z",
                        "ConsentDate": "2019-04-01T03:03:11Z",
                        "ExpiryDate": "2020-03-26T03:03:11Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "67a31c06-67db-429b-829f-9a5010971fe0",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "9e375667-0d78-4fdd-a2ee-9d693e9c59ee",
                "Identifier": "8003e82d-c888-49a0-bf23-ced67d0dcd2b",
                "LastUpdatedDate": "2019-04-01T03:07:35Z",
                "CreatedDate": "2019-04-01T03:07:35Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-01T03:07:28Z",
                        "LastTransactionDate": "2019-04-01T03:07:28Z",
                        "ConsentDate": "2019-04-01T03:07:28Z",
                        "ExpiryDate": "2020-03-26T03:07:28Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "67a31c06-67db-429b-829f-9a5010971fe0",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "cb4c2dc5-724e-474f-8818-ba1bd4f3efb5",
                "Language": "en-us",
                "Identifier": "graj@onetrust.com",
                "LastUpdatedDate": "2019-07-10T03:39:05Z",
                "CreatedDate": "2019-04-05T11:30:54Z",
                "Purposes": [
                    {
                        "Id": "4a5e6278-17f1-4283-beee-29f81cde2bd0",
                        "Name": "kbpurpose1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-05T11:30:46Z",
                        "LastTransactionDate": "2019-04-05T12:07:45Z",
                        "WithdrawalDate": "2019-04-05T12:06:57Z",
                        "ConsentDate": "2019-04-05T11:30:46Z",
                        "TotalTransactionCount": 14,
                        "LastTransactionCollectionPointId": "59da7125-74ef-4e96-97e2-676bc295c804",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-04-05T11:45:11Z",
                        "LastTransactionDate": "2019-04-06T12:00:11Z",
                        "WithdrawalDate": "2019-04-05T11:54:56Z",
                        "ConsentDate": "2019-04-05T11:45:11Z",
                        "ExpiryDate": "2019-04-06T12:00:11Z",
                        "TotalTransactionCount": 10,
                        "Topics": [
                            {
                                "Id": "1877891d-5f40-412d-8da0-16d1bf0f499b",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "90cba08f-9cc9-4d51-b5f4-7b097bdd8739",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "759d8b24-e4c0-4107-86e2-3a408db13e6b",
                        "Name": "GJ Purpose 07",
                        "Version": 2,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-04-05T11:45:11Z",
                        "LastTransactionDate": "2019-04-07T12:00:20Z",
                        "WithdrawalDate": "2019-04-05T11:54:56Z",
                        "ConsentDate": "2019-04-05T11:45:11Z",
                        "ExpiryDate": "2019-04-07T12:00:20Z",
                        "TotalTransactionCount": 11,
                        "Topics": [
                            {
                                "Id": "afcd5703-1247-4f5b-b0da-8d75e2986165",
                                "Name": "Eloqua_topic",
                                "IsConsented": true
                            },
                            {
                                "Id": "1a774b37-9733-4594-8656-fc5b799001ad",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "90cba08f-9cc9-4d51-b5f4-7b097bdd8739",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "7a08dc31-8e5a-4d92-ab93-5af6528af046",
                        "Name": "govi47p",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-05T11:45:11Z",
                        "LastTransactionDate": "2019-07-10T03:39:03Z",
                        "WithdrawalDate": "2019-05-08T06:06:07Z",
                        "ConsentDate": "2019-04-05T11:45:11Z",
                        "TotalTransactionCount": 25,
                        "LastTransactionCollectionPointId": "a157e758-2b88-4762-a012-da3211f9e489",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "a57ee9da-b494-49e2-b562-6dfa31f190df",
                        "Name": "kbpurpose2",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-04-05T12:00:30Z",
                        "LastTransactionDate": "2019-04-13T00:00:35Z",
                        "WithdrawalDate": "2019-04-05T12:06:57Z",
                        "ConsentDate": "2019-04-05T12:00:30Z",
                        "ExpiryDate": "2019-04-13T00:00:35Z",
                        "TotalTransactionCount": 6,
                        "Topics": [
                            {
                                "Id": "b0c0bf82-0d9b-4bda-8afb-25d514d090d0",
                                "Name": "kbtopic2",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "59da7125-74ef-4e96-97e2-676bc295c804",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "eb4c4ffe-72af-48bc-a3fe-d09af261cea1",
                        "Name": "Purpose12",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-05-29T03:54:27Z",
                        "LastTransactionDate": "2019-05-29T03:54:27Z",
                        "ConsentDate": "2019-05-29T03:54:27Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "94550faa-4dec-4882-b800-4a468bd76114",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "41ebdf32-068b-4239-89ac-42e20262c5a9",
                        "Name": "Send Notifications about Changes to Preferences",
                        "Version": 1,
                        "Status": "OPT_OUT",
                        "FirstTransactionDate": "2019-05-29T04:03:42Z",
                        "LastTransactionDate": "2019-05-29T04:03:42Z",
                        "ConsentDate": "2019-05-29T04:03:42Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "379e7adb-c178-4bc9-a9f1-fd9f73a3c4d6",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "7b70ca4e-8224-45b3-9924-1b5214b56ab7",
                        "Name": "test12",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-05-29T04:14:51Z",
                        "LastTransactionDate": "2019-05-29T04:14:51Z",
                        "ConsentDate": "2019-05-29T04:14:51Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "3f23b668-9c70-49aa-bad7-43ca218ecf5e",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "4d52dcc4-82bf-44bf-ac49-854eba6ff8f3",
                        "Name": "Eloqua_UAT",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-05-29T10:36:41Z",
                        "LastTransactionDate": "2019-05-29T10:36:41Z",
                        "ConsentDate": "2019-05-29T10:36:41Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "379e7adb-c178-4bc9-a9f1-fd9f73a3c4d6",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "48c7374d-5d35-4268-b378-2f6d9d414822",
                        "Name": "nrfix",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2019-05-29T10:35:53Z",
                        "LastTransactionDate": "2019-05-29T10:36:06Z",
                        "WithdrawalDate": "2019-05-29T10:36:06Z",
                        "ConsentDate": "2019-05-29T10:35:53Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "79c6c6ef-fba4-43f5-bc63-23f51e7f51ef",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "b612ec41-e461-449a-8404-42231d6b3771",
                "Identifier": "ahc1@onetrust.com",
                "LastUpdatedDate": "2019-04-05T11:44:37Z",
                "CreatedDate": "2019-04-05T11:44:37Z",
                "Purposes": [
                    {
                        "Id": "7a08dc31-8e5a-4d92-ab93-5af6528af046",
                        "Name": "govi47p",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-05T11:44:39Z",
                        "LastTransactionDate": "2019-04-05T11:44:39Z",
                        "ConsentDate": "2019-04-05T11:44:39Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "3cedc4f1-40ae-4ffc-b55b-f75c252f0b4f",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "821e45a0-04cd-47b6-bdfd-2f1f80744491",
                "Identifier": "consentot1+bvg@gmail.com",
                "LastUpdatedDate": "2019-04-05T11:51:28Z",
                "CreatedDate": "2019-04-05T11:51:28Z",
                "Purposes": [
                    {
                        "Id": "07c5b97a-5b53-4cd1-8f15-a979950ce0af",
                        "Name": "Purpose-1",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-05T11:51:30Z",
                        "LastTransactionDate": "2019-04-05T11:51:30Z",
                        "ConsentDate": "2019-04-05T11:51:30Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "9609c884-9016-4eb4-b524-2b5f7826b546",
                                "Name": "Topic_UAT",
                                "IsConsented": true
                            }
                        ],
                        "CustomPreferences": [
                            {
                                "Id": "803ea1a4-7179-4855-903a-b14cb87ee61d",
                                "Name": "custom-1",
                                "Options": [
                                    {
                                        "Id": "cfe18bf7-d889-4938-8a11-89ec4ee03577",
                                        "Name": "Option3",
                                        "IsConsented": true
                                    },
                                    {
                                        "Id": "a23f1424-4b23-46ce-ada1-edb3cdb81470",
                                        "Name": "Option2",
                                        "IsConsented": true
                                    }
                                ]
                            }
                        ],
                        "LastTransactionCollectionPointId": "4a96f54f-5854-4dd4-974f-e4b9326941d7",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "af7141f4-d0ae-4bd5-aec5-f48e671749e7",
                        "Name": "purpose-2",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-05T11:51:30Z",
                        "LastTransactionDate": "2019-04-05T11:51:30Z",
                        "ConsentDate": "2019-04-05T11:51:30Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "689bde48-658f-402d-a19c-b786ffc54d6d",
                                "Name": "topic123",
                                "IsConsented": true
                            }
                        ],
                        "CustomPreferences": [
                            {
                                "Id": "18190085-078a-4f5f-a575-3999735e5abf",
                                "Name": "custom-2",
                                "Options": [
                                    {
                                        "Id": "01bc14e5-c2bf-480a-b5f1-25a5b81c8037",
                                        "Name": "Option2",
                                        "IsConsented": true
                                    },
                                    {
                                        "Id": "2a036869-c289-4b8a-a75f-a496beb6066d",
                                        "Name": "Option3",
                                        "IsConsented": true
                                    }
                                ]
                            }
                        ],
                        "LastTransactionCollectionPointId": "4a96f54f-5854-4dd4-974f-e4b9326941d7",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "5d3aaceb-3c22-45ed-acfa-a075272c1b54",
                "Identifier": "example@otprivacy.com",
                "LastUpdatedDate": "2019-04-18T03:41:16Z",
                "CreatedDate": "2019-04-05T11:51:57Z",
                "Purposes": [
                    {
                        "Id": "07c5b97a-5b53-4cd1-8f15-a979950ce0af",
                        "Name": "Purpose-1",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2019-04-05T11:51:49Z",
                        "LastTransactionDate": "2019-04-05T11:53:50Z",
                        "WithdrawalDate": "2019-04-05T11:53:50Z",
                        "ConsentDate": "2019-04-05T11:51:49Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "4a96f54f-5854-4dd4-974f-e4b9326941d7",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "af7141f4-d0ae-4bd5-aec5-f48e671749e7",
                        "Name": "purpose-2",
                        "Version": 1,
                        "Status": "WITHDRAWN",
                        "FirstTransactionDate": "2019-04-05T11:51:49Z",
                        "LastTransactionDate": "2019-04-05T11:55:12Z",
                        "WithdrawalDate": "2019-04-05T11:55:12Z",
                        "ConsentDate": "2019-04-05T11:51:49Z",
                        "TotalTransactionCount": 2,
                        "LastTransactionCollectionPointId": "4a96f54f-5854-4dd4-974f-e4b9326941d7",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "a313353a-e13b-4554-be08-22cf4a78a31c",
                        "Name": "Purpose_1804 v2",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-18T03:41:19Z",
                        "LastTransactionDate": "2019-04-18T03:41:19Z",
                        "ConsentDate": "2019-04-18T03:41:19Z",
                        "TotalTransactionCount": 1,
                        "CustomPreferences": [
                            {
                                "Id": "4935d35a-7216-480d-9da9-1aef0e078b89",
                                "Name": "Custom_S",
                                "Options": [
                                    {
                                        "Id": "65655511-d4b1-46ac-a47a-f4dfbb2cffa6",
                                        "Name": "Option1",
                                        "IsConsented": true
                                    },
                                    {
                                        "Id": "8acc2f9f-37f8-4ace-a513-fae6b7dd0aa9",
                                        "Name": "Option2",
                                        "IsConsented": true
                                    }
                                ]
                            }
                        ],
                        "LastTransactionCollectionPointId": "b9ed1377-c66d-40d3-a36c-aabc4a5db45b",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "f0ad0a63-8c03-402f-8db6-a92d71c3dc63",
                "Identifier": "consentot1+hd@gmail.com",
                "LastUpdatedDate": "2019-04-05T12:00:52Z",
                "CreatedDate": "2019-04-05T12:00:52Z",
                "Purposes": [
                    {
                        "Id": "07c5b97a-5b53-4cd1-8f15-a979950ce0af",
                        "Name": "Purpose-1 v2",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-05T12:00:45Z",
                        "LastTransactionDate": "2019-04-05T12:00:45Z",
                        "ConsentDate": "2019-04-05T12:00:45Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "9609c884-9016-4eb4-b524-2b5f7826b546",
                                "Name": "Topic_UAT",
                                "IsConsented": true
                            }
                        ],
                        "CustomPreferences": [
                            {
                                "Id": "803ea1a4-7179-4855-903a-b14cb87ee61d",
                                "Name": "custom-1",
                                "Options": [
                                    {
                                        "Id": "cfe18bf7-d889-4938-8a11-89ec4ee03577",
                                        "Name": "Option3",
                                        "IsConsented": true
                                    },
                                    {
                                        "Id": "a23f1424-4b23-46ce-ada1-edb3cdb81470",
                                        "Name": "Option2",
                                        "IsConsented": true
                                    }
                                ]
                            }
                        ],
                        "LastTransactionCollectionPointId": "4a96f54f-5854-4dd4-974f-e4b9326941d7",
                        "LastTransactionCollectionPointVersion": 3
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "b9d79f27-08f4-4451-8bda-43dac5384665",
                "Identifier": "consentot1+doi@gmail.com",
                "LastUpdatedDate": "2019-04-05T12:03:56Z",
                "CreatedDate": "2019-04-05T12:03:40Z",
                "Purposes": [
                    {
                        "Id": "07c5b97a-5b53-4cd1-8f15-a979950ce0af",
                        "Name": "Purpose-1 v2",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-05T12:03:42Z",
                        "LastTransactionDate": "2019-04-05T12:03:55Z",
                        "ConsentDate": "2019-04-05T12:03:42Z",
                        "TotalTransactionCount": 2,
                        "Topics": [
                            {
                                "Id": "9609c884-9016-4eb4-b524-2b5f7826b546",
                                "Name": "Topic_UAT",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "4a96f54f-5854-4dd4-974f-e4b9326941d7",
                        "LastTransactionCollectionPointVersion": 4
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "17c5874a-3698-4985-92a5-a37268de7d81",
                "Identifier": "consentot1+ack@gmail.com",
                "LastUpdatedDate": "2019-04-05T12:06:31Z",
                "CreatedDate": "2019-04-05T12:05:48Z",
                "Purposes": [
                    {
                        "Id": "07c5b97a-5b53-4cd1-8f15-a979950ce0af",
                        "Name": "Purpose-1 v2",
                        "Version": 2,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-05T12:05:41Z",
                        "LastTransactionDate": "2019-04-05T12:06:31Z",
                        "ConsentDate": "2019-04-05T12:05:41Z",
                        "TotalTransactionCount": 2,
                        "Topics": [
                            {
                                "Id": "9609c884-9016-4eb4-b524-2b5f7826b546",
                                "Name": "Topic_UAT",
                                "IsConsented": true
                            }
                        ],
                        "CustomPreferences": [
                            {
                                "Id": "803ea1a4-7179-4855-903a-b14cb87ee61d",
                                "Name": "custom-1",
                                "Options": [
                                    {
                                        "Id": "a23f1424-4b23-46ce-ada1-edb3cdb81470",
                                        "Name": "Option2",
                                        "IsConsented": true
                                    }
                                ]
                            }
                        ],
                        "LastTransactionCollectionPointId": "75f1842a-b334-4c9a-b36c-6c95201eecc4",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "fe174def-a0b1-470f-a759-f29bc4af1cc6",
                "Identifier": "consentot1+b@gmail.com",
                "LastUpdatedDate": "2019-04-05T12:10:11Z",
                "CreatedDate": "2019-04-05T12:09:07Z",
                "Purposes": [
                    {
                        "Id": "07c5b97a-5b53-4cd1-8f15-a979950ce0af",
                        "Name": "Purpose-1 v2",
                        "Version": 2,
                        "Status": "NO_CONSENT",
                        "FirstTransactionDate": "2019-04-05T12:09:07Z",
                        "LastTransactionDate": "2019-04-05T12:10:11Z",
                        "WithdrawalDate": "2019-04-05T12:09:31Z",
                        "ConsentDate": "2019-04-05T12:09:07Z",
                        "TotalTransactionCount": 3,
                        "LastTransactionCollectionPointId": "4a96f54f-5854-4dd4-974f-e4b9326941d7",
                        "LastTransactionCollectionPointVersion": 4
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "58ed4dc9-7bbd-47ba-a11c-837a777b843f",
                "Identifier": "consentot1+b1@gmail.com",
                "LastUpdatedDate": "2019-04-05T12:11:11Z",
                "CreatedDate": "2019-04-05T12:11:11Z",
                "Purposes": [
                    {
                        "Id": "07c5b97a-5b53-4cd1-8f15-a979950ce0af",
                        "Name": "Purpose-1 v2",
                        "Version": 2,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2019-04-05T12:11:11Z",
                        "LastTransactionDate": "2019-04-05T12:11:11Z",
                        "ConsentDate": "2019-04-05T12:11:11Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "4a96f54f-5854-4dd4-974f-e4b9326941d7",
                        "LastTransactionCollectionPointVersion": 5
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "a77f5fbf-7aa1-46e0-9618-3d138d3e1619",
                "Identifier": "demouser1@gmail.com",
                "LastUpdatedDate": "2019-04-10T09:28:10Z",
                "CreatedDate": "2019-04-10T09:28:10Z",
                "Purposes": [
                    {
                        "Id": "07c5b97a-5b53-4cd1-8f15-a979950ce0af",
                        "Name": "Purpose-1 v2",
                        "Version": 2,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2019-04-10T09:28:00Z",
                        "LastTransactionDate": "2019-04-10T09:28:00Z",
                        "ConsentDate": "2019-04-10T09:28:00Z",
                        "TotalTransactionCount": 1,
                        "Topics": [
                            {
                                "Id": "9609c884-9016-4eb4-b524-2b5f7826b546",
                                "Name": "Topic_UAT",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "4a96f54f-5854-4dd4-974f-e4b9326941d7",
                        "LastTransactionCollectionPointVersion": 5
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "30039d13-9852-4e64-816a-443a63937e01",
                "Identifier": "pooja2007k@gmail.com",
                "LastUpdatedDate": "2019-04-18T03:21:56Z",
                "CreatedDate": "2019-04-18T03:21:56Z",
                "Purposes": [
                    {
                        "Id": "07c5b97a-5b53-4cd1-8f15-a979950ce0af",
                        "Name": "Purpose-1 v2",
                        "Version": 2,
                        "Status": "PENDING",
                        "FirstTransactionDate": "2019-04-18T03:22:21Z",
                        "LastTransactionDate": "2019-04-18T03:22:21Z",
                        "ConsentDate": "2019-04-18T03:22:21Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "4a96f54f-5854-4dd4-974f-e4b9326941d7",
                        "LastTransactionCollectionPointVersion": 5
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "ef07e08c-f0ca-4821-8fa7-1bf3a25e1338",
                "Identifier": "ot1shwetdfgha@gmail.com",
                "LastUpdatedDate": "2019-04-20T12:00:34Z",
                "CreatedDate": "2019-04-18T03:23:38Z",
                "Purposes": [
                    {
                        "Id": "f5733675-308a-41b0-9be5-aca3f2a4db44",
                        "Name": "1012kpurpose1 - V1 Name",
                        "Version": 1,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-04-18T03:24:02Z",
                        "LastTransactionDate": "2019-04-19T12:00:06Z",
                        "ConsentDate": "2019-04-18T03:24:02Z",
                        "ExpiryDate": "2019-04-19T12:00:06Z",
                        "TotalTransactionCount": 2,
                        "Topics": [
                            {
                                "Id": "1877891d-5f40-412d-8da0-16d1bf0f499b",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "7599b59b-4bbf-4d24-8e7e-85dce33e564f",
                        "LastTransactionCollectionPointVersion": 1
                    },
                    {
                        "Id": "759d8b24-e4c0-4107-86e2-3a408db13e6b",
                        "Name": "GJ Purpose 07",
                        "Version": 2,
                        "Status": "EXPIRED",
                        "FirstTransactionDate": "2019-04-18T03:24:02Z",
                        "LastTransactionDate": "2019-04-20T12:00:34Z",
                        "ConsentDate": "2019-04-18T03:24:02Z",
                        "ExpiryDate": "2019-04-20T12:00:34Z",
                        "TotalTransactionCount": 2,
                        "Topics": [
                            {
                                "Id": "1a774b37-9733-4594-8656-fc5b799001ad",
                                "Name": "1012ktopic1",
                                "IsConsented": true
                            },
                            {
                                "Id": "afcd5703-1247-4f5b-b0da-8d75e2986165",
                                "Name": "Eloqua_topic",
                                "IsConsented": true
                            }
                        ],
                        "LastTransactionCollectionPointId": "7599b59b-4bbf-4d24-8e7e-85dce33e564f",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        },
        {
            "number": 0,
            "size": 100,
            "numberOfElements": 100,
            "last": false,
            "pageable": {
                "limit": 100,
                "offset": 0,
                "sort": {
                    "sorted": true,
                    "unsorted": false,
                    "empty": false
                },
                "tokenId": 100,
                "pageSize": 100,
                "pageNumber": 0,
                "unpaged": false,
                "paged": true
            },
            "sort": {
                "sorted": true,
                "unsorted": false,
                "empty": false
            },
            "content": {
                "Id": "441c9c64-9069-4d09-b7ba-8dba06ef4dcd",
                "Identifier": "f666589e-b737-4678-a8d2-ecb59cb7b457",
                "LastUpdatedDate": "2019-04-18T03:24:28Z",
                "CreatedDate": "2019-04-18T03:24:28Z",
                "Purposes": [
                    {
                        "Id": "4f266f1a-c21a-4743-99c8-775c5edcd96c",
                        "Name": "Setting and Reading Cookies",
                        "Version": 1,
                        "Status": "ACTIVE",
                        "FirstTransactionDate": "2019-04-18T03:23:54Z",
                        "LastTransactionDate": "2019-04-18T03:23:54Z",
                        "ConsentDate": "2019-04-18T03:23:54Z",
                        "ExpiryDate": "2020-04-12T03:23:54Z",
                        "TotalTransactionCount": 1,
                        "LastTransactionCollectionPointId": "5042751b-b8f5-49d6-b824-7fa984f6d32f",
                        "LastTransactionCollectionPointVersion": 1
                    }
                ],
                "TestDataSubject": false
            },
            "first": true,
            "empty": false
        }
    ]
}
```

+++

### ソース接続の作成 {#source-connection}

[!DNL Flow Service] API に対して POST リクエストを実行することで、ソース接続を作成することができます。ソース接続は、接続 ID、ソースデータファイルへのパス、接続仕様 ID から構成されます。

**API 形式**

```https
POST /sourceConnections
```

**リクエスト**

次のリクエストは、[!DNL OneTrust Integration] のソース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "ONETRUST Source Connection",
      "description": "ONETRUST Source Connection",
      "baseConnectionId": "622124ca-6d18-47f7-999c-66f599955309",
      "connectionSpec": {
          "id": "cf16d886-c627-4872-9936-fb08d6cba8cc",
          "version": "1.0"
      },
      "data": {
          "format": "json"
      },
      "params": {}
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前はわかりやすいものにしてください。 |
| `description` | 含めることでソース接続に関する詳細情報を提供できるオプションの値です。 |
| `baseConnectionId` | [!DNL OneTrust Integration] のベース接続 ID。この ID は、前の手順で生成されました。 |
| `connectionSpec.id` | ソースに対応する接続仕様の ID。 |
| `data.format` | 取り込む [!DNL OneTrust Integration] データの形式。現在、サポートされているデータ形式は `json` のみです。 |

**応答**

リクエストが成功した場合は、新たに作成されたソース接続の一意の ID（`id`）が返されます。この ID は、後の手順でデータフローを作成する際に必要になります。

```json
{
     "id": "eb5833d3-230d-4700-80cc-bda396e7af8a",
     "etag": "\"da04c07f-0000-0200-0000-621f1afc0000\""
}
```

### ターゲット XDM スキーマの作成 {#target-schema}

ソースデータを Platform で使用するには、必要に応じてターゲットスキーマを作成してソースデータを構造化する必要があります。 次に、ターゲットスキーマを使用して、ソースデータが含まれる Platform データセットを作成します。

[Schema Registry API](https://developer.adobe.com/experience-platform-apis/references/schema-registry/) に POST リクエストを実行することで、ターゲット XDM スキーマを作成できます。

ターゲット XDM スキーマの作成手順について詳しくは、 [API を使用したスキーマの作成](../../../../../xdm/api/schemas.md)に関するチュートリアルを参照してください。

### ターゲットデータセットの作成 {#target-dataset}

[Catalog Service API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/catalog.yaml) に POST リクエストを実行し、その際にペイロード内でターゲットスキーマの ID を指定することで、ターゲットデータセットを作成できます。

ターゲットデータセットの作成手順について詳しくは、 [API を使用したデータセットの作成](../../../../../catalog/api/create-dataset.md)に関するチュートリアルを参照してください。

### ターゲット接続の作成 {#target-connection}

ターゲット接続は、取り込んだデータの保存先への接続を表します。 ターゲット接続を作成するには、[!DNL Data Lake] に対応する固定接続仕様 ID を指定する必要があります。 この ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。

これで、一意の識別子、ターゲットスキーマ、ターゲットデータセット、および [!DNL Data Lake] の接続仕様 ID が用意できました。これらの識別子を使用することで、[!DNL Flow Service] API を使用してターゲット接続を作成し、受信ソースデータを格納するデータセットを指定できます。

**API 形式**

```https
POST /targetConnections
```

**リクエスト**

次のリクエストは、[!DNL OneTrust Integration] のターゲット接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/targetConnections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "ONETRUST Target Connection",
      "description": "ONETRUST Target Connection",
      "connectionSpec": {
          "id": "c604ff05-7f1a-43c0-8e18-33bf874cb11c",
          "version": "1.0"
      },
      "data": {
          "format": "json"
      },
      "params": {
          "dataSetId": "61f6ca3f33978c19486bb463"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | ターゲット接続の名前。ターゲット接続の情報を検索に使用できるように、ターゲット接続はわかりやすい名前にしてください。 |
| `description` | ターゲット接続に関する詳細を提供するために含めることができるオプションの値です。 |
| `connectionSpec.id` | [!DNL Data Lake]に対応する接続仕様 ID。この修正済み ID は `c604ff05-7f1a-43c0-8e18-33bf874cb11c` です。 |
| `data.format` | Platform に取り込む [!DNL OneTrust Integration] データの形式。 |
| `params.dataSetId` | 前の手順で取得したターゲットデータセット ID。 |


**応答**

リクエストが成功した場合は、新しいターゲット接続の一意の ID（`id`）が返されます。この ID は、後の手順で必要になります。

```json
{
     "id": "495f761f-310a-4a7b-ae78-5b1152d74b38",
     "etag": "\"410a7b0c-0000-0200-0000-621f1afd0000\""
}
```

### マッピングの作成 {#mapping}

ソースデータをターゲットデータセットに取り込むには、まず、ターゲットデータセットが準拠するターゲットスキーマにマッピングする必要があります。これは、 [[!DNL Data Prep] API](https://www.adobe.io/experience-platform-apis/references/data-prep/) リクエストペイロード内で定義されたデータマッピングを使用して、

**API 形式**

```http
POST /conversion/mappingSets
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/conversion/mappingSets' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "version": 0,
      "xdmSchema": "https://ns.adobe.com/{TENANT_ID}/schemas/cfc8cee182e546c1fb35071185524b465e06bf1acb74f30d",
      "xdmVersion": "1.0",
      "id": null,
      "mappings": [{
              "sourceType": "ATTRIBUTE",
              "source": "content.Identifier",
              "destination": "_id",
              "name": "id",
              "description": "Identifier field"
          },
          {
              "sourceType": "ATTRIBUTE",
              "source": "content.Identifier",
              "destination": "_exchangesandboxbravo.Identifier"
          },
          {
              "sourceType": "ATTRIBUTE",
              "source": "content.Language",
              "destination": "_exchangesandboxbravo.Language",
              "description": "Language field"
          },
          {
              "sourceType": "ATTRIBUTE",
              "source": "content.CreatedDate",
              "destination": "_exchangesandboxbravo.CreatedDate",
              "description": "Created Date field"
          },
          {
              "sourceType": "ATTRIBUTE",
              "source": "content.LastUpdatedDate",
              "destination": "_exchangesandboxbravo.LastUpdatedDate",
              "description": "Created Date field"
          },
          {
              "sourceType": "ATTRIBUTE",
              "source": "content.DataElements",
              "destination": "_exchangesandboxbravo.DataElements"
          },
          {
              "sourceType": "ATTRIBUTE",
              "source": "content.Purposes",
              "destination": "_exchangesandboxbravo.Purposes"
          }

      ]
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `xdmSchema` | 以前の手順で生成された[ターゲット XDM スキーマ](#target-schema)の ID。 |
| `mappings.destinationXdmPath` | ソース属性がマッピングされている宛先 XDM パス。 |
| `mappings.sourceAttribute` | 宛先 XDM パスにマッピングする必要があるソース属性。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成されたマッピングの詳細が返されます。この値は、後の手順でデータフローを作成する際に必要になります。

```json
{
    "id": "a87f130e82f04d5188da01f087805c4b",
    "version": 0,
    "createdDate": 1646205694395,
    "modifiedDate": 1646205694395,
    "createdBy": "{CREATED_BY}",
    "modifiedBy": "{MODIFIED_BY}"
}
```

### フローの作成 {#flow}

データを取り込むための最後の手順 [!DNL OneTrust Integration] を Platform に送信する場合、データフローを作成します。 現時点で、次の必要な値の準備ができています。

* [ソース接続 ID](#source-connection)
* [ターゲット接続 ID](#target-connection)
* [マッピング ID](#mapping)

データフローは、ソースからデータをスケジュールおよび収集する役割を果たします。ペイロードに前述の値を提供しながら POST リクエストを実行することで、データフローを作成することができます。

取り込みをスケジュールするには、まず開始時刻の値をエポック時間（秒）に設定する必要があります。次に、頻度の値を次の 5 つのオプションのいずれかに設定する必要があります。`once`、`minute`、`hour`、`day` または `week`。間隔の値は、2 つの連続した取り込み間隔を指定します。ただし、1 回限りの取り込みを作成する場合は、間隔を設定する必要はありません。 それ以外の頻度では、間隔の値を `15` 以上に設定する必要があります。

**API 形式**

```http
POST /flows
```

**リクエスト**

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/flows' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "ONETRUST dataflow",
      "description": "ONETRUST dataflow",
      "flowSpec": {
          "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
          "version": "1.0"
      },
      "sourceConnectionIds": [
          "eb5833d3-230d-4700-80cc-bda396e7af8a"
      ],
      "targetConnectionIds": [
          "495f761f-310a-4a7b-ae78-5b1152d74b38"
      ],
      "transformations": [
          {
              "name": "Mapping",
              "params": {
                  "mappingId": "a87f130e82f04d5188da01f087805c4b",
                  "mappingVersion": 0
              }
          }
      ],
      "scheduleParams": {
          "startTime": "1625040887",
          "frequency": "minute",
          "interval": 15
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | データフローの名前。データフローの情報を検索する際に使用できるので、データフローはわかりやすい名前にしてください。 |
| `description` | データフローの詳細を指定するために含めることができるオプションの値です。 |
| `flowSpec.id` | データフローの作成に必要なフロー仕様 ID。この修正済み ID は `6499120c-0b15-42dc-936e-847ea3c24d72` です。 |
| `flowSpec.version` | フロー仕様 ID の対応するバージョン。この値のデフォルトは `1.0` です。 |
| `sourceConnectionIds` | 以前の手順で生成された[ソース接続 ID](#source-connection)。 |
| `targetConnectionIds` | 以前の手順で生成された[ターゲット接続 ID](#target-connection)。 |
| `transformations` | このプロパティには、データに適用する必要がある様々な変換が含まれています。このプロパティは、XDM に準拠していないデータを Platform に取り込む場合に必要です。 |
| `transformations.name` | 変換に割り当てられた名前。 |
| `transformations.params.mappingId` | 以前の手順で生成された[マッピング ID](#mapping)。 |
| `transformations.params.mappingVersion` | マッピング ID の対応するバージョン。この値のデフォルトは `0` です。 |
| `scheduleParams.startTime` | このプロパティには、データフローの取り込みスケジュールに関する情報が含まれています。 |
| `scheduleParams.frequency` | データフローがデータを収集する頻度。指定できる値は、`once`、`minute`、`hour`、`day`、`week` です。 |
| `scheduleParams.interval` | インターバルは 2 つの連続したフロー実行の間隔を指定します。インターバルの値はゼロ以外の整数にしてください。頻度が `once` に設定されている場合、間隔は必須ではありません。また、頻度は他の頻度の値に対して、`15` よりも大きいか、等しい必要があります。 |

**応答**

正常な応答は、新しく作成したデータフローの ID（`id`）を返します。この ID を使用して、データフローを監視、更新または削除できます。

```json
{
     "id": "70045189-42f0-493d-9b9e-be1045a9f4fa",
     "etag": "\"1601e900-0000-0200-0000-621f1b080000\""
}
```

### データフローの監視

データフローが作成されると、それを通して取り込まれるデータを監視し、フローの実行状況、完了状況、エラーなどの情報を確認することができます。

**API 形式**

```http
GET /runs?property=flowId=={FLOW_ID}
```

**リクエスト**

次のリクエストは、既存のデータフローの仕様を取得します。

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/runs?property=flowId==70045189-42f0-493d-9b9e-be1045a9f4fa' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、作成日、ソース接続、ターゲット接続に関する情報、フロー実行の一意の識別子（`id`）など、フロー実行に関する詳細を返します。

>[!NOTE]
>
>The JSON response payload below is hidden for brevity. 「このアイコンをクリックして、応答ペイロードを表示」を選択します。

+++クリック

```json
{
    "items": [
        {
            "id": "ddf75be8-873a-4e08-a117-219d6282ae31",
            "createdAt": 1646211900381,
            "updatedAt": 1646211977541,
            "createdBy": "acp_foundation_connectors@AdobeID",
            "updatedBy": "acp_foundation_connectors@AdobeID",
            "createdClient": "acp_foundation_connectors",
            "updatedClient": "acp_foundation_connectors",
            "sandboxId": "fbfa7dea-7eb5-4286-ba7d-ea7eb572864c",
            "sandboxName": "onetrustsandboxae",
            "imsOrgId": "EFE243245DB9D3DD0A495E80@AdobeOrg",
            "flowId": "70045189-42f0-493d-9b9e-be1045a9f4fa",
            "flowSpec": {
                "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
                "version": "1.0"
            },
            "providerRefId": "M-70045189-42f0-493d-9b9e-be1045a9f4fa-incremental-0:4dc1be81-962e-4c45-b9be-c5573288cef6",
            "etag": "\"670182cc-0000-0200-0000-621f33890000\"",
            "metrics": {
                "durationSummary": {
                    "startedAtUTC": 1646211873954,
                    "completedAtUTC": 1646211915903
                },
                "sizeSummary": {
                    "inputBytes": 365
                },
                "recordSummary": {
                    "inputRecordCount": 0
                },
                "statusSummary": {
                    "status": "success"
                }
            },
            "activities": [
                {
                    "id": "4f008a00-3a04-11eb-adc1-0242ac120002",
                    "name": "Copy Activity",
                    "updatedAtUTC": 0,
                    "durationSummary": {
                        "startedAtUTC": 1646211873954,
                        "completedAtUTC": 1646211915903
                    },
                    "latencySummary": {},
                    "sizeSummary": {
                        "inputBytes": 365
                    },
                    "recordSummary": {
                        "inputRecordCount": 0
                    },
                    "fileSummary": {},
                    "statusSummary": {
                        "status": "success",
                        "extensions": {}
                    }
                }
            ]
        },
        {
            "id": "679aede2-1d11-42f8-998a-419ab7da46ec",
            "createdAt": 1646210990081,
            "updatedAt": 1646211077998,
            "createdBy": "acp_foundation_connectors@AdobeID",
            "updatedBy": "acp_foundation_connectors@AdobeID",
            "createdClient": "acp_foundation_connectors",
            "updatedClient": "acp_foundation_connectors",
            "sandboxId": "fbfa7dea-7eb5-4286-ba7d-ea7eb572864c",
            "sandboxName": "onetrustsandboxae",
            "imsOrgId": "EFE243245DB9D3DD0A495E80@AdobeOrg",
            "flowId": "70045189-42f0-493d-9b9e-be1045a9f4fa",
            "flowSpec": {
                "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
                "version": "1.0"
            },
            "providerRefId": "M-70045189-42f0-493d-9b9e-be1045a9f4fa-incremental-0:e1915c1a-62df-45d9-ab3c-0fcb61a22bac",
            "etag": "\"6701bf6d-0000-0200-0000-621f30050000\"",
            "metrics": {
                "durationSummary": {
                    "startedAtUTC": 1646210975361,
                    "completedAtUTC": 1646211005985
                },
                "sizeSummary": {
                    "inputBytes": 365
                },
                "recordSummary": {
                    "inputRecordCount": 0
                },
                "statusSummary": {
                    "status": "success"
                }
            },
            "activities": [
                {
                    "id": "4f008a00-3a04-11eb-adc1-0242ac120002",
                    "name": "Copy Activity",
                    "updatedAtUTC": 0,
                    "durationSummary": {
                        "startedAtUTC": 1646210975361,
                        "completedAtUTC": 1646211005985
                    },
                    "latencySummary": {},
                    "sizeSummary": {
                        "inputBytes": 365
                    },
                    "recordSummary": {
                        "inputRecordCount": 0
                    },
                    "fileSummary": {},
                    "statusSummary": {
                        "status": "success",
                        "extensions": {}
                    }
                }
            ]
        },
        {
            "id": "e2624b0e-e37a-4e4a-9729-b969b72d71a2",
            "createdAt": 1646210088741,
            "updatedAt": 1646210170755,
            "createdBy": "acp_foundation_connectors@AdobeID",
            "updatedBy": "acp_foundation_connectors@AdobeID",
            "createdClient": "acp_foundation_connectors",
            "updatedClient": "acp_foundation_connectors",
            "sandboxId": "fbfa7dea-7eb5-4286-ba7d-ea7eb572864c",
            "sandboxName": "onetrustsandboxae",
            "imsOrgId": "EFE243245DB9D3DD0A495E80@AdobeOrg",
            "flowId": "70045189-42f0-493d-9b9e-be1045a9f4fa",
            "flowSpec": {
                "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
                "version": "1.0"
            },
            "providerRefId": "M-70045189-42f0-493d-9b9e-be1045a9f4fa-incremental-0:ead43ed2-a8cf-4f3b-9cde-3d868afd77f3",
            "etag": "\"6701ad52-0000-0200-0000-621f2c7a0000\"",
            "metrics": {
                "durationSummary": {
                    "startedAtUTC": 1646210074926,
                    "completedAtUTC": 1646210102982
                },
                "sizeSummary": {
                    "inputBytes": 365
                },
                "recordSummary": {
                    "inputRecordCount": 0
                },
                "statusSummary": {
                    "status": "success"
                }
            },
            "activities": [
                {
                    "id": "4f008a00-3a04-11eb-adc1-0242ac120002",
                    "name": "Copy Activity",
                    "updatedAtUTC": 0,
                    "durationSummary": {
                        "startedAtUTC": 1646210074926,
                        "completedAtUTC": 1646210102982
                    },
                    "latencySummary": {},
                    "sizeSummary": {
                        "inputBytes": 365
                    },
                    "recordSummary": {
                        "inputRecordCount": 0
                    },
                    "fileSummary": {},
                    "statusSummary": {
                        "status": "success",
                        "extensions": {}
                    }
                }
            ]
        },
        {
            "id": "247fb5e8-8b1c-43d3-a5c9-26733b68e3aa",
            "createdAt": 1646209190186,
            "updatedAt": 1646209280004,
            "createdBy": "acp_foundation_connectors@AdobeID",
            "updatedBy": "acp_foundation_connectors@AdobeID",
            "createdClient": "acp_foundation_connectors",
            "updatedClient": "acp_foundation_connectors",
            "sandboxId": "fbfa7dea-7eb5-4286-ba7d-ea7eb572864c",
            "sandboxName": "onetrustsandboxae",
            "imsOrgId": "EFE243245DB9D3DD0A495E80@AdobeOrg",
            "flowId": "70045189-42f0-493d-9b9e-be1045a9f4fa",
            "flowSpec": {
                "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
                "version": "1.0"
            },
            "providerRefId": "M-70045189-42f0-493d-9b9e-be1045a9f4fa-incremental-0:d9a827af-09a4-4b39-93b5-e4915a4537d5",
            "etag": "\"6701f937-0000-0200-0000-621f29000000\"",
            "metrics": {
                "durationSummary": {
                    "startedAtUTC": 1646209174878,
                    "completedAtUTC": 1646209203161
                },
                "sizeSummary": {
                    "inputBytes": 365
                },
                "recordSummary": {
                    "inputRecordCount": 0
                },
                "statusSummary": {
                    "status": "success"
                }
            },
            "activities": [
                {
                    "id": "4f008a00-3a04-11eb-adc1-0242ac120002",
                    "name": "Copy Activity",
                    "updatedAtUTC": 0,
                    "durationSummary": {
                        "startedAtUTC": 1646209174878,
                        "completedAtUTC": 1646209203161
                    },
                    "latencySummary": {},
                    "sizeSummary": {
                        "inputBytes": 365
                    },
                    "recordSummary": {
                        "inputRecordCount": 0
                    },
                    "fileSummary": {},
                    "statusSummary": {
                        "status": "success",
                        "extensions": {}
                    }
                }
            ]
        },
        {
            "id": "9e414296-dc66-4ae7-8316-becafa6c8987",
            "createdAt": 1646208290695,
            "updatedAt": 1646208386995,
            "createdBy": "acp_foundation_connectors@AdobeID",
            "updatedBy": "acp_foundation_connectors@AdobeID",
            "createdClient": "acp_foundation_connectors",
            "updatedClient": "acp_foundation_connectors",
            "sandboxId": "fbfa7dea-7eb5-4286-ba7d-ea7eb572864c",
            "sandboxName": "onetrustsandboxae",
            "imsOrgId": "EFE243245DB9D3DD0A495E80@AdobeOrg",
            "flowId": "70045189-42f0-493d-9b9e-be1045a9f4fa",
            "flowSpec": {
                "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
                "version": "1.0"
            },
            "providerRefId": "M-70045189-42f0-493d-9b9e-be1045a9f4fa-incremental-0:e119c163-516a-4c1f-b32b-1595d1260b9b",
            "etag": "\"67017b1f-0000-0200-0000-621f25820000\"",
            "metrics": {
                "durationSummary": {
                    "startedAtUTC": 1646208274150,
                    "completedAtUTC": 1646208316024
                },
                "sizeSummary": {
                    "inputBytes": 365
                },
                "recordSummary": {
                    "inputRecordCount": 0
                },
                "statusSummary": {
                    "status": "success"
                }
            },
            "activities": [
                {
                    "id": "4f008a00-3a04-11eb-adc1-0242ac120002",
                    "name": "Copy Activity",
                    "updatedAtUTC": 0,
                    "durationSummary": {
                        "startedAtUTC": 1646208274150,
                        "completedAtUTC": 1646208316024
                    },
                    "latencySummary": {},
                    "sizeSummary": {
                        "inputBytes": 365
                    },
                    "recordSummary": {
                        "inputRecordCount": 0
                    },
                    "fileSummary": {},
                    "statusSummary": {
                        "status": "success",
                        "extensions": {}
                    }
                }
            ]
        },
        {
            "id": "cf77ecd9-6bce-4ebb-bfa7-cf96bf2f11bf",
            "createdAt": 1646207400183,
            "updatedAt": 1646207475693,
            "createdBy": "acp_foundation_connectors@AdobeID",
            "updatedBy": "acp_foundation_connectors@AdobeID",
            "createdClient": "acp_foundation_connectors",
            "updatedClient": "acp_foundation_connectors",
            "sandboxId": "fbfa7dea-7eb5-4286-ba7d-ea7eb572864c",
            "sandboxName": "onetrustsandboxae",
            "imsOrgId": "EFE243245DB9D3DD0A495E80@AdobeOrg",
            "flowId": "70045189-42f0-493d-9b9e-be1045a9f4fa",
            "flowSpec": {
                "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
                "version": "1.0"
            },
            "providerRefId": "M-70045189-42f0-493d-9b9e-be1045a9f4fa-incremental-0:166a26a4-e889-4963-ab97-317cc0bc903b",
            "etag": "\"6701c506-0000-0200-0000-621f21f30000\"",
            "metrics": {
                "durationSummary": {
                    "startedAtUTC": 1646207374481,
                    "completedAtUTC": 1646207414251
                },
                "sizeSummary": {
                    "inputBytes": 365
                },
                "recordSummary": {
                    "inputRecordCount": 0
                },
                "statusSummary": {
                    "status": "success"
                }
            },
            "activities": [
                {
                    "id": "4f008a00-3a04-11eb-adc1-0242ac120002",
                    "name": "Copy Activity",
                    "updatedAtUTC": 0,
                    "durationSummary": {
                        "startedAtUTC": 1646207374481,
                        "completedAtUTC": 1646207414251
                    },
                    "latencySummary": {},
                    "sizeSummary": {
                        "inputBytes": 365
                    },
                    "recordSummary": {
                        "inputRecordCount": 0
                    },
                    "fileSummary": {},
                    "statusSummary": {
                        "status": "success",
                        "extensions": {}
                    }
                }
            ]
        },
        {
            "id": "614835ea-840f-4cd2-b760-197b48bfc30f",
            "createdAt": 1646206494244,
            "updatedAt": 1646206571947,
            "createdBy": "acp_foundation_connectors@AdobeID",
            "updatedBy": "acp_foundation_connectors@AdobeID",
            "createdClient": "acp_foundation_connectors",
            "updatedClient": "acp_foundation_connectors",
            "sandboxId": "fbfa7dea-7eb5-4286-ba7d-ea7eb572864c",
            "sandboxName": "onetrustsandboxae",
            "imsOrgId": "EFE243245DB9D3DD0A495E80@AdobeOrg",
            "flowId": "70045189-42f0-493d-9b9e-be1045a9f4fa",
            "flowSpec": {
                "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
                "version": "1.0"
            },
            "providerRefId": "M-70045189-42f0-493d-9b9e-be1045a9f4fa-incremental-0:d7529655-90fb-4fc7-b798-3fb5dc548c4d",
            "etag": "\"660141f0-0000-0200-0000-621f1e6c0000\"",
            "metrics": {
                "durationSummary": {
                    "startedAtUTC": 1646206477847,
                    "completedAtUTC": 1646206518858
                },
                "sizeSummary": {
                    "inputBytes": 365
                },
                "recordSummary": {
                    "inputRecordCount": 0
                },
                "statusSummary": {
                    "status": "success"
                }
            },
            "activities": [
                {
                    "id": "4f008a00-3a04-11eb-adc1-0242ac120002",
                    "name": "Copy Activity",
                    "updatedAtUTC": 0,
                    "durationSummary": {
                        "startedAtUTC": 1646206477847,
                        "completedAtUTC": 1646206518858
                    },
                    "latencySummary": {},
                    "sizeSummary": {
                        "inputBytes": 365
                    },
                    "recordSummary": {
                        "inputRecordCount": 0
                    },
                    "fileSummary": {},
                    "statusSummary": {
                        "status": "success",
                        "extensions": {}
                    }
                }
            ]
        },
        {
            "id": "04513721-e3bd-4906-ae0e-8fe060fe2a01",
            "createdAt": 1646205728328,
            "updatedAt": 1646205777796,
            "createdBy": "acp_foundation_connectors@AdobeID",
            "updatedBy": "acp_foundation_connectors@AdobeID",
            "createdClient": "acp_foundation_connectors",
            "updatedClient": "acp_foundation_connectors",
            "sandboxId": "fbfa7dea-7eb5-4286-ba7d-ea7eb572864c",
            "sandboxName": "onetrustsandboxae",
            "imsOrgId": "EFE243245DB9D3DD0A495E80@AdobeOrg",
            "flowId": "70045189-42f0-493d-9b9e-be1045a9f4fa",
            "flowSpec": {
                "id": "6499120c-0b15-42dc-936e-847ea3c24d72",
                "version": "1.0"
            },
            "providerRefId": "M-70045189-42f0-493d-9b9e-be1045a9f4fa-backfill-0:098d5b08-9019-4f10-8ebd-a1468a516890",
            "etag": "\"660190d1-0000-0200-0000-621f1b510000\"",
            "metrics": {
                "durationSummary": {
                    "startedAtUTC": 1646205714389,
                    "completedAtUTC": 1646205763241
                },
                "sizeSummary": {
                    "inputBytes": 396123,
                    "outputBytes": 32436
                },
                "recordSummary": {
                    "inputRecordCount": 208,
                    "createdRecordCount": 208,
                    "outputRecordCount": 208,
                    "failedRecordCount": 0
                },
                "fileSummary": {
                    "outputFileCount": 1,
                    "activityRefs": [
                        "37b34f84-1ada-11eb-adc1-0242ac120002"
                    ]
                },
                "statusSummary": {
                    "status": "success"
                }
            },
            "activities": [
                {
                    "id": "4f008a00-3a04-11eb-adc1-0242ac120002",
                    "name": "Copy Activity",
                    "updatedAtUTC": 0,
                    "durationSummary": {
                        "startedAtUTC": 1646205714389,
                        "completedAtUTC": 1646205744612
                    },
                    "latencySummary": {},
                    "sizeSummary": {
                        "inputBytes": 396123,
                        "outputBytes": 314924
                    },
                    "recordSummary": {
                        "inputRecordCount": 208,
                        "createdRecordCount": 208,
                        "outputRecordCount": 208
                    },
                    "fileSummary": {},
                    "statusSummary": {
                        "status": "success",
                        "extensions": {}
                    }
                },
                {
                    "id": "37b34f84-1ada-11eb-adc1-0242ac120002",
                    "name": "Promotion Activity",
                    "updatedAtUTC": 0,
                    "durationSummary": {
                        "startedAtUTC": 1646205752158,
                        "completedAtUTC": 1646205763241
                    },
                    "latencySummary": {},
                    "sizeSummary": {
                        "inputBytes": 314924,
                        "outputBytes": 32436
                    },
                    "recordSummary": {
                        "inputRecordCount": 208,
                        "createdRecordCount": 208,
                        "outputRecordCount": 208,
                        "failedRecordCount": 0
                    },
                    "fileSummary": {
                        "inputFileCount": 2,
                        "outputFileCount": 1,
                        "extensions": {
                            "manifest": {
                                "fileInfo": "https://platform.adobe.io/data/foundation/export/batches/01FX4R4MRFDN3C2PDZ11NNYFYP/meta?path=input_files"
                            }
                        }
                    },
                    "statusSummary": {
                        "status": "success",
                        "extensions": {}
                    }
                }
            ]
        }
    ],
    "_links": {}
}
```

+++

| プロパティ | 説明 |
| -------- | ----------- |
| `items` | 特定のフロー実行に関連付けられたメタデータの単一のペイロードが含まれます。 |
| `metrics` | フロー実行のデータの特性を定義します。 |
| `activities` | データの変換方法を定義します。 |
| `durationSummary` | フロー実行の開始と終了の時間を定義します。 |
| `sizeSummary` | データのボリュームをバイト単位で定義します。 |
| `recordSummary` | データのレコード数を定義します。 |
| `fileSummary` | データのファイル数を定義します。 |
| `statusSummary` | フロー実行が成功か失敗かを定義します。 |

### データフローの更新

データフローの実行スケジュール、名前、説明を更新するには、[!DNL Flow Service] API に PATCH リクエストを実行し、その際にフロー ID、バージョン、使用する新しいスケジュールを指定します。

>[!IMPORTANT]
>
>`If-Match` ヘッダーは、PATCH リクエストを行う際に必要です。このヘッダーの値は、更新するデータフローの一意の ETag です。

**API 形式**

```http
PATCH /flows/{FLOW_ID}
```

**リクエスト**

次のリクエストは、フロー実行スケジュールと、データフローの名前および説明を更新します。

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/flows/70045189-42f0-493d-9b9e-be1045a9f4fa' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: "1a0037e4-0000-0200-0000-602e06f60000"' \
    -d '[
            {
                "op": "replace",
                "path": "/scheduleParams/frequency",
                "value": "day"
            },
            {
                "op": "replace",
                "path": "/name",
                "value": "New dataflow name"
            },
            {
                "op": "replace",
                "path": "/description",
                "value": "Updated dataflow description"
            }
        ]'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `op` | データフローの更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するパラメーターのパス。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

リクエストが成功した場合は、フロー ID と更新された etag が返されます。[!DNL Flow Service] API に GET リクエストを実行し、その際にフロー ID を指定することで、更新を検証できます。

```json
{
    "id": "993f908f-3342-4d9c-9f3c-5aa9a189ca1a",
    "etag": "\"50014cc8-0000-0200-0000-6036eb720000\""
}
```

### データフローの削除

既存のフロー ID を使用すると、[!DNL Flow Service] API に DELETE リクエストを実行することでデータフローを削除できます。

**API 形式**

```http
DELETE /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 削除するデータフローの一意の `id` の値。 |

**リクエスト**

```shell
curl -X DELETE \
    'https://platform-int.adobe.io/data/foundation/flowservice/flows/70045189-42f0-493d-9b9e-be1045a9f4fa' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。データフローに対してルックアップ（GET）リクエストを試みることで、削除を確認することができます。API はデータフローが削除されたことを示す HTTP 404（見つかりません）エラーを返します。

### 接続を更新

接続の名前、説明、資格情報を更新するには、[!DNL Flow Service] API に PATCH リクエストを実行し、その際にベース接続 ID、バージョン、使用する新しい情報を指定します。

>[!IMPORTANT]
>
>PATCH リクエストを実行する際は、`If-Match` ヘッダーが必要です。このヘッダーの値は、更新する接続の一意のバージョンです。

**API 形式**

```http
PATCH /connections/{BASE_CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | 更新したい接続の一意の `id` 値。 |

**リクエスト**

次のリクエストでは、新しい名前と説明、一連の新しい資格情報を提供して接続を更新します。

```shell
curl -X PATCH \
    'https://platform.adobe.io/data/foundation/flowservice/connections/622124ca-6d18-47f7-999c-66f599955309' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
    -H 'If-Match: 3402ba40-0000-0200-0000-621f3a470000' \
    -d '[
        {
            "op": "replace",
            "path": "/auth/params",
            "value": {
                "username": "salesforce-connector-username",
                "password": "{NEW_PASSWORD}",
                "securityToken": "{NEW_SECURITY_TOKEN}"
            }
        },
        {
            "op": "replace",
            "path": "/name",
            "value": "Any New Value"
        },
        {
            "op": "add",
            "path": "/description",
            "value": "Any new description"
        }
    ]'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `op` | 接続の更新に必要なアクションを定義するために使用される操作呼び出し。操作には、`add`、`replace`、`remove` があります。 |
| `path` | 更新するパラメーターのパス。 |
| `value` | パラメーターの更新に使用する新しい値。 |

**応答**

正常な応答では、ベース接続 ID と更新された etag が返されます。[!DNL Flow Service] API に GET リクエストを実行し、その際に接続 ID を指定することで、更新を検証できます。

```json
{
    "id": "139f6a5f-a78b-4744-9f6a-5fa78bd74431",
    "etag": "\"3600e378-0000-0200-0000-5f40212f0000\""
}
```

### 接続の削除

既存のベース接続 ID が用意できたら、[!DNL Flow Service] API に DELETE リクエストを実行します。

**API 形式**

```http
DELETE /connections/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{BASE_CONNECTION_ID}` | 削除するベース接続の一意の `id` の値。 |

**リクエスト**

```shell
curl -X DELETE \
    'https://platform-int.adobe.io/data/foundation/flowservice/connections/622124ca-6d18-47f7-999c-66f599955309' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

接続先へのルックアップ（GET）リクエストを試みることで、削除を確認できます。
