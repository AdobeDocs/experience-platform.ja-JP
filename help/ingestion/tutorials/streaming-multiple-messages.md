---
keywords: Experience Platform；ホーム；人気のあるトピック；ストリーミング取り込み；取り込み；複数のメッセージのストリーミング；複数のメッセージ；
solution: Experience Platform
title: 単一の HTTP リクエストでの複数メッセージの送信
topic-legacy: tutorial
type: Tutorial
description: このドキュメントでは、ストリーミング取り込みを使用して単一の HTTP リクエスト内で複数のメッセージをAdobe Experience Platformに送信するためのチュートリアルを提供します。
exl-id: 04045090-8a2c-42b6-aefa-09c043ee414f
source-git-commit: 5160bc8057a7f71e6b0f7f2d594ba414bae9d8f6
workflow-type: tm+mt
source-wordcount: '1493'
ht-degree: 66%

---

# 単一の HTTP リクエストでの複数のメッセージの送信

データを Adobe Experience Platform にストリーミングする場合、多数の HTTP 呼び出しを作成すると高コストになる可能性があります。例えば、1 KB のペイロードで 200 個の HTTP リクエストを作成するよりも、それぞれが 1 KB の 200 個のメッセージが含まれた 1 個の HTTP リクエスト（200 KB の単一ペイロード）を作成する方がはるかに効率的です。正しく使用した場合、1 つのリクエスト内で複数のメッセージをグループ化すると、[!DNL Experience Platform] に送信されるデータを最適化できます。

このドキュメントでは、ストリーミング取り込みを使用して単一の HTTP リクエスト内で複数のメッセージを [!DNL Experience Platform] に送信する方法に関するチュートリアルを提供します。

## はじめに

このチュートリアルでは、Adobe Experience Platform [!DNL Data Ingestion] に関する十分な知識が必要です。 このチュートリアルを始める前に、次のドキュメントを確認してください。

- [データ取得の概要](../home.md):取り込み方法や Data Connectors な [!DNL Experience Platform Data Ingestion]ど、の主要概念について説明します。
- [ストリーミング取り込みの概要](../streaming-ingestion/overview.md):ストリーミング接続、データセット、 、など、ストリーミング取り込みのワークフローと構 [!DNL XDM Individual Profile]成ブロッ [!DNL XDM ExperienceEvent]ク。

また、このチュートリアルでは、 API を正しく呼び出すために、[Adobe Experience Platform に対する認証](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)のチュートリアルを完了していることが求められます。[!DNL Platform]認証に関するチュートリアルを完了すると、このチュートリアルで必要な認証ヘッダーの値が提供されます。このヘッダーは、サンプル呼び出しで次のように示されます。

- Authorization: Bearer `{ACCESS_TOKEN}`

すべての POST リクエストには、次の追加ヘッダーが必要です。

- Content-Type: application/json

## ストリーミング接続の作成

[!DNL Experience Platform] へのデータのストリーミングを開始するには、まずストリーミング接続を作成する必要があります。 ストリーミング接続の作成方法については、[ストリーミング接続の作成](./create-streaming-connection.md)に関するガイドを参照してください。

ストリーミング接続を登録すると、ユーザーにはデータプロデューサーとして、データを Platform にストリーミングするために使用できる一意の URL が付与されます。

## データセットへのストリーミング

次の例は、単一の HTTP リクエスト内で複数のメッセージを特定のデータセットに送信する方法を示しています。メッセージヘッダーにデータセット ID を挿入して、そのメッセージが直接取得されるようにします。

既存のデータセットの ID は、[!DNL Platform] UI を使用するか、API のリスト操作を使用して取得できます。 このデータセット ID は、[Experience Platform](https://platform.adobe.com) で、**[!UICONTROL 「データセット]**」タブに移動し、ID が必要なデータセットをクリックして、**[!UICONTROL 「情報]**」タブのデータセット ID フィールドから文字列をコピーすると見つかります。 API を使用してデータセットを取得する方法については、「[カタログサービスの概要](../../catalog/home.md)」を参照してください。

既存のデータセットを使用する代わりに、新しいデータセットを作成できます。API を使用してデータセットを作成する方法の詳細については、[API を使用したデータセットの作成](../../catalog/api/create-dataset.md)のチュートリアルを参照してください。

**API 形式**

```http
POST /collection/batch/{CONNECTION_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{CONNECTION_ID}` | 作成したストリーミング接続の ID。 |

**リクエスト**

```shell
curl -X POST https://dcs.adobedc.net/collection/batch/{CONNECTION_ID} \
  -H 'Content-Type: application/json' \
  -d '{
  "messages": [
    {
      "header": {
        "schemaRef": {
          "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
          "contentType": "application/vnd.adobe.xed-full+json;{SCHEMA_VERSION}"
        },
        "imsOrgId": "{IMS_ORG}",
        "datasetId": "{DATASET_ID}",
        "createdAt": 1526283801869
      },
      "body": {
        "xdmMeta": {
          "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
            "contentType": "application/vnd.adobe.xed-full+json;{SCHEMA_VERSION}"
          }
        },
        "xdmEntity": {
          "_id": "9af5adcc-db9c-4692-b826-65d3abe68c22",
          "timestamp": "2019-02-23T22:07:01Z",
          "environment": {
            "browserDetails": {
              "userAgent": "Mozilla/5.0 (Windows NT 5.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.57 Safari/537.36 OPR/16.0.1196.62",
              "acceptLanguage": "en-US",
              "cookiesEnabled": true,
              "javaScriptVersion": "1.6",
              "javaEnabled": true
            },
            "colorDepth": 32,
            "viewportHeight": 799,
            "viewportWidth": 414
          },
          "productListItems": [
            {
              "SKU": "CC",
              "name": "Fernie Snow",
              "quantity": 30,
              "priceTotal": 7.8
            }
          ],
          "commerce": {
            "productViews": {
              "value": 1
            }
          },
          "_experience": {
            "campaign": {
              "message": {
                "profileSnapshot": {
                  "workEmail": {
                    "address": "gregdorcey@example.com"
                  }
                }
              }
            }
          }
        }
      }
    },
    {
      "header": {
        "schemaRef": {
          "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
          "contentType": "application/vnd.adobe.xed-full+json;{SCHEMA_VERSION}"
        },
        "imsOrgId": "{IMS_ORG}",
        "datasetId": "{DATASET_ID}",
        "createdAt": 1526283801869
      },
      "body": {
        "xdmMeta": {
          "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
            "contentType": "application/vnd.adobe.xed-full+json;{SCHEMA_VERSION}"
          }
        },
        "xdmEntity": {
          "_id": "7af6adcc-dc9e-4692-b826-55d2abe68c11",
          "timestamp": "2019-02-23T22:07:31Z",
          "environment": {
            "browserDetails": {
              "userAgent": "Mozilla/5.0 (Windows NT 5.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.57 Safari/537.36 OPR/16.0.1196.62",
              "acceptLanguage": "en-US",
              "cookiesEnabled": true,
              "javaScriptVersion": "1.6",
              "javaEnabled": true
            },
            "colorDepth": 32,
            "viewportHeight": 799,
            "viewportWidth": 414
          },
          "productListItems": [
            {
              "SKU": "CC",
              "name": "Carmine Santiago",
              "quantity": 10,
              "priceTotal": 5.5
            }
          ],
          "commerce": {
            "productViews": {
              "value": 1
            }
          },
          "_experience": {
            "campaign": {
              "message": {
                "profileSnapshot": {
                  "workEmail": {
                    "address": "emilyong@example.com"
                  }
                }
              }
            }
          }
        }
      }
    }
  ]
}'
```

**応答** 

成功した応答は、HTTP ステータス 207 （マルチステータス）を返します。応答の本文では、リクエストで実行された各メソッドの成功または失敗に関する詳細を確認できます。リクエストメッセージ配列の各要素に対して応答が返されます。メッセージにエラーが発生しなかった、成功した応答の例を次に示します。

```json
{
    "inletId": "9b0cb233972f3b0092992284c7353f5eead496218e8441a79b25e9421ea127f5",
    "batchId": "1565628583792:1485:153",
    "receivedTimeMs": 1565628583854,
    "responses": [
        {
            "xactionId": "1565628583792:1485:153:0"
        },
        {
            "xactionId": "1565628583792:1485:153:1"
        }
    ]
}
```

ステータスコードの詳細については、このチュートリアルの付録にある「[応答コード](#response-codes)」の表を参照してください。

## 失敗したメッセージの識別

単一のメッセージが含まれるリクエストを送信する場合とは異なり、複数のメッセージが含まれる HTTP リクエストを送信する場合は、データの送信に失敗した日時を識別する方法、送信に失敗した特定のメッセージの識別とその取得方法、同じリクエスト内の他のメッセージが失敗した場合に成功したデータに何が起こるかなど、追加の要素を考慮する必要があります。

このチュートリアルを進める前に、[失敗したバッチの取得](../quality/retrieve-failed-batches.md)に関するガイドを確認することをお勧めします。

### 有効なメッセージと無効なメッセージを含むリクエストペイロードの送信

次の例は、バッチに有効なメッセージと無効なメッセージが含まれる場合の動作を示しています。

リクエストペイロードは、XDM スキーマのイベントを表す JSON オブジェクトの配列です。メッセージの検証を正しくおこなうには、次の条件が満たされている必要があります。
- メッセージヘッダーの `imsOrgId` フィールドは、インレット定義と一致する必要があります。リクエストペイロードに `imsOrgId` フィールドが含まれていない場合、 （DCCS）によってフィールドが自動的に追加されます。[!DNL Data Collection Core Service]
- メッセージのヘッダーは、[!DNL Platform] UI で作成された既存の XDM スキーマを参照する必要があります。
- `datasetId` フィールドは、[!DNL Platform] 内の既存のデータセットを参照し、そのスキーマは、リクエスト本文に含まれる各メッセージ内の `header` オブジェクトで提供されるスキーマと一致する必要があります。

**API 形式**

```http
POST /collection/batch/{CONNECTION_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{CONNECTION_ID}` | 作成したデータインレットの ID。 |

**リクエスト**

```shell
curl -X POST https://dcs.adobedc.net/collection/batch/{CONNECTION_ID} \
  -H 'Content-Type: application/json' \
  -d '{
  "messages": [
    {
      "header": {
        "schemaRef": {
          "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
          "contentType": "application/vnd.adobe.xed-full+json;{SCHEMA_VERSION}"
        },
        "imsOrgId": "{IMS_ORG}",
        "datasetId": "{DATASET_ID}",
        "createdAt": 1526283801869
      },
      "body": {
        "xdmMeta": {
          "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
            "contentType": "application/vnd.adobe.xed-full+json;{SCHEMA_VERSION}"
          }
        },
        "xdmEntity": {
          "_id": "9af5adcc-db9c-4692-b826-65d3abe68c22",
          "timestamp": "2019-02-23T22:07:01Z",
          "environment": {
            "browserDetails": {
              "userAgent": "Mozilla/5.0 (Windows NT 5.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.57 Safari/537.36 OPR/16.0.1196.62",
              "acceptLanguage": "en-US",
              "cookiesEnabled": true,
              "javaScriptVersion": "1.6",
              "javaEnabled": true
            },
            "colorDepth": 32,
            "viewportHeight": 799,
            "viewportWidth": 414
          },
          "productListItems": [
            {
              "SKU": "CC",
              "name": "Tip Top Collection",
              "quantity": 15,
              "priceTotal": 9.5
            }
          ],
          "commerce": {
            "productViews": {
              "value": 1
            }
          },
          "_experience": {
            "campaign": {
              "message": {
                "profileSnapshot": {
                  "workEmail": {
                    "address": "rogerkanagawa@example.com"
                  }
                }
              }
            }
          }
        }
      }
    },
    {
      "header": {
        "schemaRef": {
          "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
          "contentType": "application/vnd.adobe.xed-full+json;{SCHEMA_VERSION}"
        },
        "imsOrgId": "{IMS_ORG}",
        "datasetId": "{DATASET_ID}",
        "createdAt": 1526283801869
      }
    },
    {
      "header": {
        "schemaRef": {
          "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
          "contentType": "application/vnd.adobe.xed-full+json;{SCHEMA_VERSION}"
        },
        "imsOrgId": "invalidIMSOrg@AdobeOrg",
        "datasetId": "{DATASET_ID}",
        "createdAt": 1526283801869
      },
      "body": {
        "xdmMeta": {
          "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
            "contentType": "application/vnd.adobe.xed-full+json;{SCHEMA_VERSION}"
          }
        },
        "xdmEntity": {
          "_id": "9af5adcc-db9c-4692-b826-65d3abe68c22",
          "timestamp": "2019-02-23T22:07:01Z",
          "environment": {
            "browserDetails": {
              "userAgent": "Mozilla/5.0 (Windows NT 5.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.57 Safari/537.36 OPR/16.0.1196.62",
              "acceptLanguage": "en-US",
              "cookiesEnabled": true,
              "javaScriptVersion": "1.6",
              "javaEnabled": true
            },
            "colorDepth": 32,
            "viewportHeight": 799,
            "viewportWidth": 414
          },
          "productListItems": [
            {
              "SKU": "CC",
              "name": "Carmine Santiago",
              "quantity": 10,
              "priceTotal": 5.5
            }
          ],
          "commerce": {
            "productViews": {
              "value": 1
            }
          },
          "_experience": {
            "campaign": {
              "message": {
                "profileSnapshot": {
                  "workEmail": {
                    "address": "mohandeewar@example.com"
                  }
                }
              }
            }
          }
        }
      }
    },
   {
      "header": {
        "schemaRef": {
          "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
          "contentType": "application/vnd.adobe.xed-full+json;{SCHEMA_VERSION}"
        },
        "imsOrgId": "{IMS_ORG}",
        "datasetId": "{DATASET_ID}",
        "createdAt": 1526283801869
      },
      "body": {
        "xdmMeta": {
          "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
            "contentType": "application/vnd.adobe.xed-full+json;{SCHEMA_VERSION}"
          }
        },
        "xdmEntity": {
          "_id": "9af5adcc-db9c-4692-b826-65d3abe68c22",
          "timestamp": "2019-02-23T22:07:01Z",
          "environment": {
            "browserDetails": {
              "userAgent": "Mozilla/5.0 (Windows NT 5.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/29.0.1547.57 Safari/537.36 OPR/16.0.1196.62",
              "acceptLanguage": "en-US",
              "cookiesEnabled": true,
              "javaScriptVersion": "1.6",
              "javaEnabled": true
            },
            "colorDepth": 32,
            "viewportHeight": 799,
            "viewportWidth": 414
          },
          "productListItems": [
            {
              "SKU": "CC",
              "name": "Tip Top Collection",
              "quantity": 15,
              "priceTotal": 9.5
            }
          ],
          "commerce": {
            "productViews": {
              "value": 1
            }
          }
        }
      }
    },
    {
      "header": {
        "msgType": "xdmEntityCreate",
        "msgId": "79d2e715-f25f-4c36",
        "xdmSchema": {
          "name": "_xdm.context.experienceevent"
        },
        "imsOrgId": "{IMS_ORG}",
        "datasetId": "{DATASET_ID}",
        "createdAt": 1526283801869
      },
      "body": {
        "xdmMeta": {
          "xdmSchema": {
            "name": "_xdm.context.experienceevent"
          }
        },
        "xdmEntity": {
          "_id": "abc",
          "dataSource": {
            "_id": "http://abc.com/abc"
          },
          "timestamp": "2018-05-18T15:28:25Z",
          "endUserIDs": {
            "_vendor": {
              "adobe": {
                "experience": {
                  "mcId": {
                    "id": "http://abc.com/abc"
                  }
                }
              }
            }
          }
        }
      }
    }
  ]
}'
```

**応答** 

リクエストペイロードには、各メッセージのステータスに加えて、トレースに使用できる `xactionId` の GUID が含まれます。

```JSON
{
    "inletId": "9b0cb233972f3b0092992284c7353f5eead496218e8441a79b25e9421ea127f5",
    "batchId": "1565638336649:1750:244",
    "receivedTimeMs": 1565638336705,
    "responses": [
        {
            "xactionId": "1565650704337:2124:92:3"
        },
        {
            "statusCode": 400,
            "message": "inletId: [9b0cb233972f3b0092992284c7353f5eead496218e8441a79b25e9421ea127f5] imsOrgId: [{IMS_ORG}] Message has unknown xdm format"
        },
        {
            "statusCode": 400,
            "message": "inletId: [9b0cb233972f3b0092992284c7353f5eead496218e8441a79b25e9421ea127f5] imsOrgId: [{IMS_ORG}] Message has an absent or wrong ims org in the header"
        },
        {
            "statusCode": 400,
            "message": "inletId: [9b0cb233972f3b0092992284c7353f5eead496218e8441a79b25e9421ea127f5] imsOrgId: [{IMS_ORG}] Message has unknown xdm format"
        }
    ]
}
```

上記の応答例は、前のリクエストのエラーメッセージを示しています。この応答を以前の有効な応答と比較すると、リクエストが部分的に成功し、1 つのメッセージが正常に取得され、3 つのメッセージが失敗したことがわかります。どちらの応答も「207」ステータスコードを返していることに注意してください。ステータスコードの詳細については、このチュートリアルの付録にある「[応答コード](#response-codes)」の表を参照してください。

最初のメッセージは [!DNL Platform] に正常に送信され、他のメッセージの結果の影響を受けません。 そのため、失敗したメッセージを再送信する場合は、このメッセージを再度含める必要はありません。

2 番目のメッセージは、メッセージの本文がないため失敗しました。コレクションリクエストでは、メッセージ要素に有効なヘッダーセクションと本文セクションが含まれている必要があります。2 番目のメッセージのヘッダーの後に次のコードを追加すると、リクエストが修正され、2 番目のメッセージが検証に合格するようになります。

```JSON
      "body": {
        "xdmMeta": {
          "schemaRef": {
            "id": "https://ns.adobe.com/{TENANT_ID}/schemas/{SCHEMA_ID}",
            "contentType": "application/vnd.adobe.xed-full+json;{SCHEMA_VERSION}"
          }
        },
        "xdmEntity": {
          "_id": "9af5adcc-db9c-4692-b826-65d3abe68c22",
          "timestamp": "2019-02-23T22:07:01Z",
        }
    },
```

3 番目のメッセージは、ヘッダーで無効な IMS 組織 ID が使用されているため失敗しました。IMS 組織は、投稿先の {CONNECTION_ID} と一致する必要があります。使用しているストリーミング接続に一致する IMS 組織 ID を特定するには、[[!DNL Data Ingestion API]](https://www.adobe.io/experience-platform-apis/references/data-ingestion/) を使用して `GET inlet` リクエストを実行します。 以前に作成したストリーミング接続の取得方法の例については、[ストリーミング接続の取得](./create-streaming-connection.md#get-data-collection-url)に関する節を参照してください。

4 番目のメッセージは、予期された XDM スキーマに従わなかったため失敗しました。リクエストのヘッダーと本文に含まれる `xdmSchema` が、`{DATASET_ID}` の XDM スキーマと一致していません。メッセージのヘッダーと本文のスキーマを修正すると、DCCS 検証に合格し、[!DNL Platform] に正常に送信されます。 また、[!DNL Platform] のストリーミング検証に合格するには、メッセージの本文を更新して、`{DATASET_ID}` の XDM スキーマと一致させる必要があります。 Platform に正常にストリーミングされるメッセージに対する影響の詳細については、このチュートリアルの「[取得したメッセージの確認](#confirm-messages-ingested)」の節を参照してください。

###  からの失敗したメッセージの取得[!DNL Platform]

失敗したメッセージは、応答配列のエラーステータスコードで識別されます。無効なメッセージは、`{DATASET_ID}` で指定されたデータセット内の「エラー」バッチに収集および格納されます。

失敗したバッチメッセージの回復の詳細については、[失敗したバッチの取得](../quality/retrieve-failed-batches.md)に関するガイドを参照してください。

## 取得したメッセージの確認

DCCS 検証に合格したメッセージは、[!DNL Platform] にストリーミングされます。 [!DNL Platform] では、[!DNL Data Lake] に取り込まれる前に、バッチメッセージはストリーミング検証によってテストされます。 バッチのステータスは、成功したかどうかに関わらず、`{DATASET_ID}` で指定されたデータセット内に表示されます。

[Experience PlatformUI](https://platform.adobe.com) を使用して [!DNL Platform] に正常にストリーミングされたバッチメッセージのステータスを表示するには、**[!UICONTROL 「データセット]** 」タブに移動し、ストリーミング先のデータセットをクリックして、**[!UICONTROL 「データセットアクティビティ]** 」タブを確認します。

[!DNL Platform] 上のストリーミング検証に合格したバッチメッセージは、[!DNL Data Lake] に取り込まれます。 その後、メッセージを分析または書き出しすることができます。

## 次の手順

これで、1 回の要求で複数のメッセージを送信し、メッセージがターゲットデータセットに正常に取り込まれたかどうかを確認する方法がわかったので、独自のデータを [!DNL Platform] にストリーミングできます。 [!DNL Platform] から取り込んだデータのクエリと取得方法の概要については、[[!DNL Data Access]](../../data-access/tutorials/dataset-data.md) のガイドを参照してください。

## 付録

この節では、このチュートリアルの補足情報を提供します。

### 応答コード

次の表に、成功した応答メッセージと失敗した応答メッセージによって返されるステータスコードを示します。

| ステータスコード | 説明 |
| :---: | --- |
| 207 | 全体的な応答ステータスコードとして「207」が使用されていますが、メソッド実行の成功または失敗に関する詳細については、受信者が複数ステータス応答の本文の内容を調べる必要があります。 この応答コードは、成功や部分的な成功のほか、失敗した状況でも使用されます。 |
| 400 | リクエストに問題がありました。より具体的なエラーメッセージ（例えば、メッセージペイロードに必須フィールドがなかった、またはメッセージが不明な XDM 形式だった）については、応答の本文を参照してください。 |
| 401 | 未認証：リクエストに有効な認証ヘッダーがありません。このコードは、認証が有効になっているインレットのみに対して返されます。 |
| 403 | 未認証：指定された認証トークンが無効か期限切れです。このコードは、認証が有効になっているインレットのみに対して返されます。 |
| 413 | ペイロードが大きすぎる：ペイロードリクエストの合計が 1 MB を超えた場合に返されます。 |
| 429 | 指定された期間内のリクエストが多すぎます。 |
| 500 | ペイロードの処理中にエラーが発生しました。より具体的なエラーメッセージ（例えば、メッセージペイロードスキーマが指定されていない、または [!DNL Platform] の XDM 定義と一致しなかった）については、応答の本文を参照してください。 |
| 503 | サービスは現在利用できません。クライアントは、指数バックオフ戦略を使用して、少なくとも 3 回再試行する必要があります。 |
