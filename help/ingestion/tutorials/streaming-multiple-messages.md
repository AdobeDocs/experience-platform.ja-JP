---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 単一のHTTP要求での複数のメッセージのストリーミング
topic: tutorial
translation-type: tm+mt
source-git-commit: 79466c78fd78c0f99f198b11a9117c946736f47a

---


# 単一のHTTPリクエストでの複数のメッセージの送信

データをAdobe Experience Platformにストリーミングする場合、多数のHTTP呼び出しを作成すると高コストになる可能性があります。 例えば、1 KBのペイロードで200個のHTTPリクエストを作成する代わりに、200 KBの各メッセージで200個のペイロードを持つ1個のHTTPリクエストを作成する方が、より効率的です。 正しく使用すると、1つのリクエスト内で複数のメッセージをグループ化することが、エクスペリエンスプラットフォームに送信されるデータを最適化する優れた方法です。

このドキュメントでは、ストリーミング取り込みを使用して単一のHTTPリクエスト内で複数のメッセージをエクスペリエンスプラットフォームに送信するためのチュートリアルを提供します。

## はじめに

このチュートリアルでは、Adobe Experience Platform Data Ingestの実際の理解が必要です。 このチュートリアルを開始する前に、次のドキュメントを確認してください。

- [データ取り込みの概要](../home.md):インジェストメソッドやData Connectorsを含む、エクスペリエンスプラットフォームデータ取り込みの中核的な概念をカバーします。
- [ストリーミング取り込みの概要](../streaming-ingestion/overview.md):ストリーミング接続、データセット、XDM個人プロファイル、XDM ExperienceEventなど、ストリーミング取り込みのワークフローと構築ブロック。

また、このチュートリアルでは、プラットフォームAPIを正しく呼び出す [ために、Adobe Experience Platform](../../tutorials/authentication.md) （Adobe Experience Platformへの認証）のチュートリアルを完了している必要があります。 このチュートリアルでは、すべてのAPI呼び出しで必要な認証ヘッダーの値を、認証チュートリアルで入力します。 ヘッダーは、次のようなサンプル呼び出しで表示されます。

- 認証：無記名 `{ACCESS_TOKEN}`

すべてのPOSTリクエストには、次のヘッダーが必要です。

- コンテンツタイプ：application/json

## ストリーミング接続の作成

Experience Platformにストリーミングデータを開始する前に、ストリーミング接続を作成する必要があります。 ストリーミング [接続の作成方法については](./create-streaming-connection.md) 、『ストリーミング接続の作成』ガイドを参照してください。

ストリーミング接続を登録すると、データプロデューサーとして、一意のURLを持ち、データをプラットフォームにストリーミングするために使用できます。

## データセットへのストリーミング

次の例は、単一のHTTPリクエスト内で複数のメッセージを特定のデータセットに送信する方法を示しています。 メッセージヘッダーにデータセットIDを挿入して、そのメッセージを直接取り込みます。

プラットフォームUIまたはAPIのリスト操作を使用して、既存のデータセットのIDを取得できます。 このIDは、 [Experience Platformで「](https://platform.adobe.com) Datasets **」タブに移動し、IDを取得するデータセットをクリックして、「** Info **Info Dataset」タブの「** Dataset ID **」フィールドの文字列をコピーすることで確認できま** す。 APIを使用してデ [ータセットを取得する方法については](../../catalog/home.md) 、Catalog Serviceの概要を参照してください。

既存のデータセットを使用する代わりに、新しいデータセットを作成できます。 APIを使用したデータセ [ットの作成について詳しくは、](../../catalog/api/create-dataset.md) 「APIを使用したデータセットの作成」チュートリアルを参照してください。

**API形式**

```http
POST /collection/{CONNECTION_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{CONNECTION_ID}` | 作成されたストリーミング接続のID。 |

**リクエスト**

```shell
curl -X POST https://dcs.adobedc.net/collection/{CONNECTION_ID} \
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
        "source": {
          "name": "GettingStarted"
        },
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
        "source": {
          "name": "GettingStarted"
        },
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

成功した応答は、HTTPステータス207（マルチステータス）を返します。 応答本文を確認すると、リクエストで実行される各メソッドの成功または失敗に関する詳細が提供されます。 リクエストメッセージ配列の各要素に対して応答が返されます。 次の例は、正常に完了した応答で、メッセージにエラーが発生しなかった場合のものです。

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

ステータスコードの詳細については、このチュートリアルの付 [録にある応答コード](#response-codes) の表を参照してください。

## 失敗したメッセージの識別

単一のメッセージでリクエストを送信する場合と比較して、複数のメッセージを含むHTTPリクエストを送信する場合、次のような追加の要因を考慮する必要があります。データの送信に失敗した日時、送信に失敗した特定のメッセージと取得方法、同じ要求内の他のメッセージが失敗した場合に成功したデータに対する影響を識別する方法。

このチュートリアルに進む前に、まず失敗したバッチの取得に関するガイドを確 [認することをお勧めし](../quality/retrieve-failed-batches.md) ます。

### 有効なメッセージと無効なメッセージを含む要求ペイロードを送信する

次の例は、バッチに有効なメッセージと無効なメッセージが含まれる場合の動作を示しています。

要求ペイロードは、XDMペイロードのイベントを表すJSONオブジェクトの配列です。 メッセージの検証を正しく行うには、次の条件を満たす必要があります。
- メッセージ `imsOrgId` ヘッダーのフィールドは、インレット定義と一致する必要があります。 要求ペイロードにフィールドが含まれていな `imsOrgId` い場合、データ収集コアサービス(DCCS)はフィールドを自動的に追加します。
- メッセージのヘッダーは、プラットフォームUIで作成された既存のXDMスキーマを参照する必要があります。
- このフ `datasetId` ィールドは、Platformの既存のデータセットを参照し、そのスキーマは、リクエストの本文に含まれる各メッセージ内のオブジェクトで提供さ `header` れるスキーマと一致する必要があります。

**API形式**

```http
POST /collection/{CONNECTION_ID}
```

| プロパティ | 説明 |
| -------- | ----------- |
| `{CONNECTION_ID}` | 作成したデータインレットのID。 |

**リクエスト**

```shell
curl -X POST https://dcs.adobedc.net/collection/{CONNECTION_ID} \
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
        "source": {
          "name": "GettingStarted"
        },
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
        "source": {
          "name": "GettingStarted"
        },
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
        "source": {
          "name": "GettingStarted"
        },
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
        "source": {
          "name": "GettingStarted"
        },
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
        "source": {
          "name": "GettingStarted"
        },
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

応答ペイロードは、各メッセージのステータスと、トレースに使用でき `xactionId` る内のGUIDを含みます。

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

上記の応答例は、前の要求のエラーメッセージを示しています。 この応答を以前の有効な応答と比較すると、リクエストが部分的に成功し、1つのメッセージが正常に取り込まれ、3つのメッセージが失敗したことがわかります。 どちらの応答も「207」ステータスコードを返します。 ステータスコードの詳細については、このチュートリアルの付 [録にある応答コード](#response-codes) の表を参照してください。

最初のメッセージはプラットフォームに正常に送信され、他のメッセージの結果の影響を受けません。 そのため、失敗したメッセージを再送信する場合は、このメッセージを再度含める必要はありません。

2番目のメッセージは、メッセージの本文が不足しているため失敗しました。 コレクションリクエストでは、メッセージ要素に有効なヘッダーセクションとボディセクションが含まれている必要があります。 2番目のメッセージのヘッダーの後に次のコードを追加すると、リクエストが修正され、2番目のメッセージが検証に合格するようになります。

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

3番目のメッセージは、ヘッダーで無効なIMS組織IDが使用されているために失敗しました。 IMS組織は、投稿先の{CONNECTION_ID}と一致する必要があります。 使用しているストリーミング接続に一致するIMS組織IDを特定するには、データ取り込みAPIを使 `GET inlet` 用して要求を [実行します](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml)。 以前に作 [成したストリーミング接続の取得方法の例については](./create-streaming-connection.md#get-data-collection-url) 、「ストリーミング接続の取得」を参照してください。

4番目のメッセージは、予期されたXDMメッセージに従わなかったため失敗しました。スキーマ。 要求の `xdmSchema` ヘッダーと本文に含まれるが、のXDMスキーマと一致しない `{DATASET_ID}`。 メッセージヘッダーと本文のスキーマを修正すると、DCCS検証に合格し、プラットフォームに正常に送信できます。 また、プラットフォーム上のストリーミング検証に合格するには、メッセージ本文を、のXDM `{DATASET_ID}` スキーマに合わせて更新する必要があります。 プラットフォームに正常にストリーミングされるメッセージに対する影響について詳しくは、このチュートリアルの「取り込ま [れるメッセージの確認](#confirm-messages-ingested) 」の節を参照してください。

### プラットフォームからの失敗したメッセージの取得

失敗したメッセージは、応答配列のエラーステータスコードで識別されます。
無効なメッセージが収集され、によって指定されたデータセット内の「エラー」バッチに保存されま `{DATASET_ID}`す。

失敗したバッチメ [ッセージの回復の詳細については](../quality/retrieve-failed-batches.md) 、失敗したバッチの取得に関するガイドを参照してください。

## 取り込んだメッセージの確認

DCCS検証に合格したメッセージは、プラットフォームにストリーミングされます。 プラットフォームでは、データレークに取り込まれる前に、バッチメッセージがストリーミング検証によってテストされます。 バッチのステータスは、成功したかどうかに関わらず、で指定したデータセット内に表示されま `{DATASET_ID}`す。

[Experience Platform UIを使用して、プラットフォームに正常にストリーミングされるバッチメッセージのステータスを表示できます。そのためには、「](https://platform.adobe.com) Datasets **」タブに移動し、ストリーミング先のデータセットをクリックし、「データセットアクティビティ****** 」タブを確認します。

プラットフォーム上のストリーミング検証に合格したバッチメッセージは、データレークに取り込まれます。 その後、メッセージを分析または書き出しできます。

## 次の手順

1回のリクエストで複数のメッセージを送信する方法と、メッセージがターゲットデータセットに正常に取り込まれたことを確認できるようになったので、独自のデータを開始ストリーミングしてPlatformに送信できます。 プラットフォームから取り込んだデータのクエリ方法と取り出し方法の概要については、『データアクセス [ガイド](../../data-access/tutorials/dataset-data.md) 』を参照してください。

## 付録

この節では、このチュートリアルの補足情報を説明します。

### 応答コード

次の表に、成功した応答メッセージと失敗した応答メッセージによって返されるステータスコードを示します。

| ステータスコード | 説明 |
| :---: | --- |
| 207 | 全体的な応答ステータスコードとして「207」が使用されているが、受信者は、メソッドの実行の成功または失敗に関する詳細について、マルチステータス応答本体の内容を調べる必要がある。 応答コードは、成功、部分的な成功、および失敗の状況でも使用されます。 |
| 400 | 要求に問題がありました。 より具体的なエラーメッセージについては、応答本文を参照してください（例えば、Messageペイロードに必須フィールドがなかった、またはMessageが不明なxdm形式だった）。 |
| 401 | 未認証：要求に有効な認証ヘッダーがありません。 これは、認証が有効になっているインレットに対してのみ返されます。 |
| 403 | 未認証： 指定された認証トークンが無効か期限切れです。 これは、認証が有効になっているインレットに対してのみ返されます。 |
| 413 | ペイロードが大きすぎる — ペイロード要求の合計が1 MBを超える場合に発生します。 |
| 429 | 指定された期間内のリクエストが多すぎます。 |
| 500 | ペイロードの処理中にエラーが発生しました。 より具体的なエラーメッセージについては、応答本文を参照してください(例えば、Message payloadスキーマが指定されていないか、PlatformのXDM定義と一致しません)。 |
| 503 | サービスは現在利用できません。 クライアントは、指数バックオフ戦略を使用して少なくとも3回再試行する必要があります。 |