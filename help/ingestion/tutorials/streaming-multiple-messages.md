---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 単一のHTTP要求での複数のメッセージのストリーミング
topic: tutorial
translation-type: tm+mt
source-git-commit: 6a371aab5435bac97f714e5cf96a93adf4aa0303
workflow-type: tm+mt
source-wordcount: '1504'
ht-degree: 1%

---


# 単一のHTTPリクエストでの複数のメッセージの送信

データをAdobe Experience Platformにストリーミングする場合、多数のHTTP呼び出しを作成すると高コストになる場合があります。 例えば、1 KBのペイロードで200個のHTTPリクエストを作成する代わりに、200 KBのペイロードでそれぞれ1 KBのメッセージを200 KBずつ含む1個のHTTPリクエストを作成する方が、はるかに効率的です。 正しく使用すれば、1つのリクエスト内で複数のメッセージをグループ化することは、Experience Platformに送信されるデータを最適化する優れた方法です。

このドキュメントでは、ストリーミング取り込みを使用して、1つのHTTPリクエスト内で複数のメッセージをExperience Platformに送信するためのチュートリアルを提供します。

## はじめに

このチュートリアルでは、Adobe Experience Platformデータの取り込みに関する十分な理解が必要です。 このチュートリアルを開始する前に、次のドキュメントを確認してください。

- [データ取り込みの概要](../home.md): Experience Platformデータ取り込みの主要な概念（取り込みメソッドやData Connectorsを含む）について説明します。
- [ストリーミング取り込みの概要](../streaming-ingestion/overview.md): ストリーミング接続、データセット、XDM Individualプロファイル、XDM ExperienceEventなど、ストリーミング取り込みのワークフローと構築ブロックです。

また、PlatformAPIの呼び出しを正常に行うために、Adobe Experience Platform [](../../tutorials/authentication.md) 認証に関するチュートリアルを完了している必要があります。 このチュートリアルでは、すべてのAPI呼び出しに必要な認証ヘッダーの値を認証チュートリアルで説明します。 ヘッダーは、次のようなサンプル呼び出しに表示されます。

- 認証： 無記名 `{ACCESS_TOKEN}`

すべてのPOSTリクエストには、次の追加のヘッダーが必要です。

- Content-Type: application/json

## ストリーミング接続の作成

Experience Platformへのストリーミングデータの開始を行う前に、まずストリーミング接続を作成する必要があります。 ストリーミング接続の作成方法については、『ストリーミング接続の [作成](./create-streaming-connection.md) 』ガイドを参照してください。

ストリーミング接続を登録すると、データプロデューサーとして、一意のURLが割り当てられ、このURLを使用してPlatformにデータをストリーミングできます。

## データセットへのストリーミング

次の例は、単一のHTTPリクエスト内で特定のデータセットに複数のメッセージを送信する方法を示しています。 メッセージヘッダーにデータセットIDを挿入し、そのメッセージを直接取り込むようにします。

PlatformUIまたはAPIのリスト操作を使用して、既存のデータセットのIDを取得できます。 データセットIDは、 [Experience Platform](https://platform.adobe.com) で「 **Datasets** 」タブに移動し、IDを設定するデータセットをクリックして、「 **Dataset ID** (データセットID **)」フィールド(** Info)タブ)からコピーすることで確認できます。 APIを使用してデータセットを取得する方法について詳しくは、 [カタログサービスの概要](../../catalog/home.md) を参照してください。

既存のデータセットを使用する代わりに、新しいデータセットを作成できます。 APIを使用したデータセットの作成について詳しくは、「APIを使用したデータセットの [作成](../../catalog/api/create-dataset.md) 」チュートリアルを参照してください。

**API形式**

```http
POST /collection/batch/{CONNECTION_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{CONNECTION_ID}` | 作成されたストリーミング接続のID。 |

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

応答が成功すると、HTTPステータス207（マルチステータス）が返されます。 応答の本文を確認すると、要求で実行される各メソッドの成功または失敗に関して、より詳細な情報が得られます。 リクエストメッセージ配列の各要素に対して応答が返されます。 次に、メッセージに失敗しなかった正常な応答の例を示します。

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

ステータスコードの詳細については、このチュートリアルの付録にある [応答コード](#response-codes) の表を参照してください。

## 失敗したメッセージの識別

単一のメッセージでリクエストを送信する場合と比較して、複数のメッセージでHTTPリクエストを送信する場合は、次のような追加の要因を考慮する必要があります。 データの送信に失敗した日時、送信に失敗した特定のメッセージと取得方法、同じ要求内の他のメッセージが失敗した場合に成功したデータはどうなるかを識別する方法。

このチュートリアルに進む前に、失敗したバッチの [取得に関するガイドを確認することをお勧めします](../quality/retrieve-failed-batches.md) 。

### 有効なメッセージと無効なメッセージを含む要求ペイロードを送信する

次の例は、バッチに有効なメッセージと無効なメッセージが含まれている場合の動作を示しています。

リクエストペイロードは、XDMスキーマ内のイベントを表すJSONオブジェクトの配列です。 メッセージの検証が成功するには、次の条件を満たす必要があります。
- メッセージヘッダーの `imsOrgId` フィールドは、インレット定義と一致する必要があります。 要求ペイロードにフィールドが含まれていない場合、データ収集コアサービス(DCCS)は、フィールドを自動的に追加します。 `imsOrgId`
- メッセージのヘッダーは、PlatformUIで作成された既存のXDMスキーマを参照する必要があります。
- この `datasetId` フィールドでは、Platform内の既存のデータセットを参照する必要があり、そのスキーマは、リクエストの本文に含まれる各メッセージ内で、オブジェクトに指定されるスキーマと一致する必要があり `header` ます。

**API形式**

```http
POST /collection/batch/{CONNECTION_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{CONNECTION_ID}` | 作成したデータインレットのID。 |

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

応答ペイロードには、各メッセージのステータスと、トレースに使用できる内のGUIDが含ま `xactionId` れます。

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

上記の応答例は、前のリクエストに対するエラーメッセージを示しています。 この応答を以前の有効な応答と比較すると、要求が部分的に成功し、1つのメッセージが正常に取り込まれ、3つのメッセージが失敗したことがわかります。 どちらの応答も「207」ステータスコードを返します。 ステータスコードの詳細については、このチュートリアルの付録にある [応答コード](#response-codes) の表を参照してください。

最初のメッセージは正常にPlatformに送信され、他のメッセージの結果の影響を受けません。 その結果、失敗したメッセージを再送信する場合は、このメッセージを再度含める必要はありません。

2番目のメッセージは、メッセージの本文が不足しているため失敗しました。 コレクションリクエストでは、メッセージ要素に有効なヘッダーセクションと本文セクションが含まれている必要があります。 2番目のメッセージのヘッダーの後に次のコードを追加すると、リクエストが修正され、2番目のメッセージが検証に合格するようになります。

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

3つ目のメッセージは、ヘッダーで無効なIMS組織IDが使用されているために失敗しました。 IMS組織は、投稿先の{CONNECTION_ID}と一致する必要があります。 使用しているストリーミング接続に一致するIMS組織IDを特定するには、 `GET inlet` Data Ingestion API [](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml). 以前に作成したストリーミング接続の取得方法の例については、 [](./create-streaming-connection.md#get-data-collection-url) ストリーミング接続の取得を参照してください。

4番目のメッセージは、予期されたXDMスキーマに従わなかったため失敗しました。 要求のヘッダーと本文 `xdmSchema` に含まれるIDが、のXDMスキーマと一致しません `{DATASET_ID}`。 メッセージヘッダーと本文のスキーマを修正すると、DCCSの検証に合格し、Platformに正常に送信できます。 また、Platformのストリーミング検証に合格するには、メッセージ本文を更新して、XDMスキーマ `{DATASET_ID}` に合致させる必要があります。 Platformに正常に送信されるメッセージに対して何が起こるかについての詳細は、このチュートリアルの「取り込まれた [メッセージを](#confirm-messages-ingested) 確認する」の節を参照してください。

### Platformからの失敗したメッセージの取得

失敗したメッセージは、応答配列のエラーステータスコードによって識別されます。
無効なメッセージが収集され、によって指定されたデータセット内の「エラー」バッチに保存され `{DATASET_ID}`ます。

失敗したバッチメッセージの回復の詳細については、失敗したバッチ [の](../quality/retrieve-failed-batches.md) 取得に関するガイドを参照してください。

## 取り込まれたメッセージの確認

DCCS検証に合格したメッセージは、Platformにストリーミングされます。 Platform時に、バッチメッセージは、データレークに取り込まれる前に、ストリーミング検証によってテストされます。 バッチのステータス（成功したかどうか）は、で指定したデータセット内に表示され `{DATASET_ID}`ます。

PlatformUIを使用して、に正常にストリーミングされるバッチメッセージのステータスを表示するには、「 [Experience Platform](https://platform.adobe.com) 」タブに移動し、ストリーミング先のデータセットをクリックし、「 **データセットアクティビティ****** 」タブをチェックします。

Platform上のストリーミング検証に合格したバッチメッセージは、データレークに取り込まれます。 その後、メッセージを分析または書き出しで使用できます。

## 次の手順

1回のリクエストで複数のメッセージを送信する方法と、ターゲットが正常にメッセージデータセットに取り込まれたかどうかを確認できるようになったので、独自のデータをPlatformに開始ストリーミングできます。 取り込んだデータをクエリし、Platformから取り込む方法の概要については、『 [データアクセス](../../data-access/tutorials/dataset-data.md) 』ガイドを参照してください。

## 付録

この節では、チュートリアルの補足情報を説明します。

### 応答コード

次の表に、成功および失敗した応答メッセージによって返されるステータスコードを示します。

| ステータスコード | 説明 |
| :---: | --- |
| 207 | 全体的な応答状態コードとして「207」が使用されていますが、受信者は、マルチステータス応答本体の内容を参照して、メソッドの実行の成功または失敗に関する詳細を確認する必要があります。 応答コードは、成功、部分的な成功、および失敗の場合にも使用されます。 |
| 400 | 要求に問題がありました。 より具体的なエラーメッセージについては、応答本文を参照してください（例えば、Messageペイロードに必須フィールドがなかったか、Messageがxdm形式が不明でした）。 |
| 401 | 未認証： 要求に有効な認証ヘッダーがありません。 これは、認証が有効になっているインレットに対してのみ返されます。 |
| 403 | 未認証：  指定された認証トークンは無効か期限切れです。 これは、認証が有効になっているインレットに対してのみ返されます。 |
| 413 | ペイロードが大きすぎます — ペイロード要求の合計が1 MBを超える場合に発生します。 |
| 429 | 指定された期間内のリクエストが多すぎます。 |
| 500 | ペイロードの処理中にエラーが発生しました。 より具体的なエラーメッセージについては、応答本文を参照してください(例えば、Message payloadスキーマが指定されていないか、Platform内のXDM定義と一致しません)。 |
| 503 | 現在、サービスを利用できません。 クライアントは、指数的なバックオフ戦略を使用して、少なくとも3回再試行する必要があります。 |