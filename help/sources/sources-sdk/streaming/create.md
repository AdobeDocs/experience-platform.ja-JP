---
title: Flow Service API を使用したストリーミングSDKの新しい接続仕様の作成
description: 次のドキュメントでは、Flow Service API を使用して接続仕様を作成し、セルフサービスソースを通じて新しいソースを統合する手順を説明します。
exl-id: ad8f6004-4e82-49b5-aede-413d72a1482d
badge: ベータ版
source-git-commit: 16cc811a545414021b8686ae303d6112bcf6cebb
workflow-type: tm+mt
source-wordcount: '744'
ht-degree: 37%

---

# [!DNL Flow Service] API を使用して、新しい接続仕様を作成します。

>[!NOTE]
>
>セルフサービスソースのストリーミング SDKはベータ版です。 ベータラベル付きソースの使用について詳しくは、[&#x200B; ソースの概要 &#x200B;](../../home.md#terms-and-conditions) を参照してください。

接続仕様は、ソースの構造を表します。ソースの認証要件に関する情報が含まれ、ソースデータの調査および検査方法が定義され、特定のソースの属性に関する情報が提供されます。[!DNL Flow Service] API の `/connectionSpecs` エンドポイントを使用すると、組織内の接続仕様をプログラムで管理できます。

次のドキュメントでは、[!DNL Flow Service] API を使用して接続仕様を作成し、セルフサービスソース（ストリーミング SDK）を使用して新しいソースを統合する手順を説明します。

## はじめに

先に進む前に、[はじめる前に](./getting-started.md)を参照し、関連ドキュメントへのリンク、このドキュメントのサンプル API 呼び出しを読み取るためのガイドおよび任意の Experience Platform API を正常に呼び出すために必要なヘッダーに関する重要な情報を確認してください。

## アーティファクトを収集

セルフサービスソースを使用して新しいストリーミングソースを作成するには、まずAdobeと調整し、プライベート Git リポジトリーをリクエストし、ソースのラベル、説明、カテゴリ、アイコンに関する詳細をAdobeと調整する必要があります。

指定したら、以下のようにプライベート Git リポジトリを構築する必要があります。

* ソース
   * {your_source}
      * アーティファクト
         * {your_source}-category.txt
         * {your_source}-description.txt
         * {your_source}-icon.svg
         * {your_source}-label.txt
         * {your_source}-connectionSpec.json

| アーティファクト（ファイル名） | 説明 | 例 |
| --- | --- | --- |
| {your_source} | ソースの名前。 このフォルダーには、プライベート Git リポジトリー内の、ソースに関連するすべてのアーティファクトを格納する必要があります。 | `medallia` |
| {your_source}-category.txt | ソースが属するカテゴリで、テキストファイルとして書式設定されます。 **メモ**：ソースが上記のカテゴリに当てはまらないと思われる場合は、Adobeの担当者にお問い合わせください。 | `medallia-category.txt` ファイル内で、ソースのカテゴリを次のように指定してください：`streaming`。 |
| {your_source}-description.txt | ソースの簡単な説明。 | [!DNL Medallia] は、マーケティングオートメーションソースで、データをExperience Platformに取り込む [!DNL Medallia] めに使用できます。 |
| {your_source}-icon.svg | Experience Platform ソースカタログ内のソースを表すために使用される画像。 このアイコンは、SVG ファイルである必要があります。 |  |
| {your_source}-label.txt | Experience Platform ソースカタログに表示されるソースの名前。 | Medallia |
| {your_source}-connectionSpec.json | ソースの接続仕様を含む JSON ファイル。 このファイルは最初は必要ありません。このガイドの完了時に、接続仕様にデータが入力されるからです。 | `medallia-connectionSpec.json` |

{style="table-layout:auto"}

>[!TIP]
>
>接続仕様のテスト期間中に、キー値の代わりに接続仕様の `text` を使用できます。

必要なファイルをプライベート Git リポジトリに追加したら、Adobeがレビューするためのプルリクエスト（PR）を作成する必要があります。 PR が承認されて統合されると、ソースのラベル、説明、アイコンを参照するために接続仕様に使用できる ID が提供されます。

次に、以下に説明する手順に従って、接続仕様を設定します。 高度なスケジュール、カスタムスキーマ、様々なページネーションタイプなど、ソースに追加できる様々な機能に関する追加のガイダンスについては、[&#x200B; ソース仕様の設定 &#x200B;](../config/sourcespec.md) に関するガイドを参照してください。

## 接続仕様テンプレートをコピー

必要なアーティファクトを収集したら、以下の接続仕様テンプレートを選択したテキストエディターにコピー＆ペーストし、括弧内の属性 `{}` を特定のソースに関連する情報で更新します。

```json
{
  "name": "generic-streaming",
  "type": "generic-streaming",
  "description": "{DESCRIPTION}",
  "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
  "version": "1.0",
  "attributes": {
    "category": "Streaming",
    "isSource": true,
    "documentationLink": "https://docs.adobe.com/content/help/ja-JP/platform-learn/tutorials/data-ingestion/understanding-streaming-ingestion.html",
    "uiAttributes": {
      "apiFeatures": {
        "updateSupported": false
      }
    }
  },
  "authSpec": [],
  "name": "generic-streaming",
  "permissionsInfo": {
    "view": [
      {
        "name": "StreamingSource",
        "@type": "lowLevel",
        "permissions": [
          "read"
        ]
      }
    ],
    "manage": [
      {
        "name": "StreamingSource",
        "@type": "lowLevel",
        "permissions": [
          "write"
        ]
      }
    ]
  },
  "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
  "sourceSpec": {
    "attributes": {
      "authRequired": false,
      "uiAttributes": {
        "documentationLink": "http://www.adobe.com/go/understanding-data-streaming-ingestion-en",
        "isSource": true,
        "monitoringSupported": false,
        "category": {
          "key": "streaming"
        },
        "icon": {
          "key": "generic"
        },
        "description": {
          "text": "Generic Streaming For Authentication Testing 2"
        },
        "label": {
          "text": "Generic Streaming For Authentication Testing 2"
        },
        "frequency": {
          "text": "Generic Streaming"
        }
      }
    }
  },
  "exploreSpec": {
    "type": "StreamingConnection"
  }
}
```

## 接続仕様を作成 {#create}

接続仕様テンプレートを取得したら、ソースに対応する適切な値を入力して、新しい接続仕様のオーサリングを開始できます。

接続仕様は、ソース仕様と参照仕様の 2 つの異なる部分に分けることができます。

接続仕様のセクションについて詳しくは、次のドキュメントを参照してください。

* [ソース仕様を設定](../config/sourcespec.md)
* [参照仕様を設定](../config/explorespec.md)

仕様情報が更新されたら、[!DNL Flow Service] API の `/connectionSpecs` エンドポイントに POST リクエストを行うことで、新しい接続仕様を送信できます。

**API 形式**

```http
POST /connectionSpecs
```

**リクエスト**

次のリクエストは、ストリーミングソースに対する完全オーサリングされた接続仕様の例です。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connectionSpecs' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "generic-streaming",
      "type": "generic-streaming",
      "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
      "version": "1.0",
      "attributes": {
          "category": "Streaming",
          "isSource": true,
          "documentationLink": "https://docs.adobe.com/content/help/ja-JP/platform-learn/tutorials/data-ingestion/understanding-streaming-ingestion.html",
          "uiAttributes": {
            "apiFeatures": {
              "updateSupported": false
            }
          }
        },
        "authSpec": [],
        "name": "generic-streaming",
        "permissionsInfo": {
          "view": [
            {
              "name": "StreamingSource",
              "@type": "lowLevel",
              "permissions": [
                "read"
              ]
            }
          ],
          "manage": [
            {
              "name": "StreamingSource",
              "@type": "lowLevel",
              "permissions": [
                "write"
              ]
            }
          ]
        },
        "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
        "sourceSpec": {
          "attributes": {
            "uiAttributes": {
              "documentationLink": "http://www.adobe.com/go/understanding-data-streaming-ingestion-en",
              "isSource": true,
              "monitoringSupported": false,
              "category": {
                "key": "streaming"
              },
              "icon": {
                "key": "Generic-Streaming"
              },
              "description": {
                "text": "Generic Streaming Connector"
              },
              "label": {
                "text": "Generic"
              },
              "frequency": {
                "text": "streaming"
              }
            }
          }
        },
        "exploreSpec": {
          "type": "StreamingConnection"
          },
        "type": "generic-streaming",
        "version": "1.0"
      }'
```

**応答**

正常に応答すると、一意の `id` を含む新しく作成された接続仕様が返されます。

```json
{
  "items": [
    {
      "id": "bdb5b792-451b-42de-acf8-15f3195821de",
      "createdAt": 1667536504101,
      "updatedAt": 1667536504101,
      "createdBy": "{CREATED_BY}",
      "updatedBy": "{UPDATED_BY}",
      "createdClient": "{CREATED_CLIENT}",
      "updatedClient": "{CREATED_CLIENT}",
      "sandboxId": "d537df80-c5d7-11e9-aafb-87c71c35cac8",
      "sandboxName": "prod",
      "imsOrgId": "{ORG_ID}",
      "name": "generic-streaming",
      "providerId": "521eee4d-8cbe-4906-bb48-fb6bd4450033",
      "version": "1.0",
      "type": "generic-streaming",
      "sourceSpec": {
        "attributes": {
          "authRequired": false,
          "uiAttributes": {
            "documentationLink": "http://www.adobe.com/go/understanding-data-streaming-ingestion-en",
            "isSource": true,
            "monitoringSupported": false,
            "category": {
              "key": "streaming"
            },
            "icon": {
              "key": "Generic-Streaming"
            },
            "description": {
              "text": "Generic Streaming Connector"
            },
            "label": {
              "text": "Generic"
            },
            "frequency": {
              "text": "streaming"
            }
          }
        }
      },
      "exploreSpec": {
        "type": "StreamingConnection"
      },
      "attributes": {
        "category": "Streaming",
        "isSource": true,
        "documentationLink": "https://docs.adobe.com/content/help/ja-JP/platform-learn/tutorials/data-ingestion/understanding-streaming-ingestion.html",
        "uiAttributes": {
          "apiFeatures": {
            "updateSupported": false
          }
        }
      },
      "permissionsInfo": {
        "view": [
          {
            "@type": "lowLevel",
            "name": "StreamingSource",
            "permissions": [
              "read"
            ]
          }
        ],
        "manage": [
          {
            "@type": "lowLevel",
            "name": "StreamingSource",
            "permissions": [
              "write"
            ]
          }
        ]
      }
    }
  ]
}
```

## 次の手順

新しい接続仕様を作成したら、既存のフロー仕様に対応する接続仕様 ID を追加する必要があります。 詳しくは、[フロー仕様の更新](./update-flow-specs.md)に関するチュートリアルを参照してください。

作成した接続仕様を変更するには、[接続仕様の更新](./update-connection-specs.md)に関するチュートリアルを参照してください。 
