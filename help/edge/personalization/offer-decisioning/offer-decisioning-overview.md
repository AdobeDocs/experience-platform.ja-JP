---
title: Platform Web SDK でのOffer decisioningの使用
description: Adobe Experience Platform Web SDK は、Offer decisioningで管理されるパーソナライズされたオファーを配信およびレンダリングできます。 オファー UI または API を使用して、オファーやその他の関連オブジェクトをOffer decisioningできます。
keywords: offer decisioning；判定；Web SDK;Platform Web SDK；パーソナライズされたオファー；オファーの配信；オファーの配信；オファーのパーソナライズ；
exl-id: 4ab51f9d-3c44-4855-b900-aa2cde673a9a
source-git-commit: fb0d8aedbb88aad8ed65592e0b706bd17840406b
workflow-type: tm+mt
source-wordcount: '870'
ht-degree: 18%

---

# Platform Web SDK でのOffer decisioningの使用

>[!NOTE]
>
>Adobe Experience Platform Web SDK でのOffer decisioningの使用は、一部のユーザーに対して早期にアクセスできます。 この機能は、一部の IMS 組織ではご利用いただけきません。

Adobe Experience Platform [!DNL Web SDK] では、Offer decisioningで管理されるパーソナライズされたオファーを配信およびレンダリングできます。 Offer Decisioning ユーザーインターフェイス（UI）または API を使用して、オファーとその他の関連オブジェクトを作成できます。

## 前提条件

* IMS 組織がエッジ判定に対して有効になっています
* オファー、作成されたアクティビティ
* データストリームが公開されています

## 用語

offer decisioningを扱う際は、次の用語を理解することが重要です。 詳細および追加のキーワードを表示するには、 [offer decisioning用語集](https://experienceleague.adobe.com/docs/offer-decisioning/using/get-started/glossary.html).

* **コンテナ：** コンテナとは、異なる懸念を切り離すための分離メカニズムです。 コンテナ ID は、すべてのリポジトリ API の最初のパス要素です。すべての決定オブジェクトはコンテナ内に存在します。

* **決定範囲：** offer decisioningの場合、決定範囲は、offer decisioningサービスがオファーの提案に使用するアクティビティと配置 ID を含む、Base64 でエンコードされた JSON の文字列です。

   *決定範囲 JSON:*

   ```json
   {
     "activityId":"xcore:offer-activity:11cfb1fa93381aca",
     "placementId":"xcore:offer-placement:1175009612b0100c"
   }
   ```

   *決定範囲 Base64 エンコードされた文字列：*

   ```json
   "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="
   ```

   >[!TIP]
   >
   >決定範囲の値は、 **アクティビティの概要** ページを使用します。

   ![](assets/decision-scope-copy.png)

* **データストリーム：** 詳しくは、 [datastreams](../../datastreams/overview.md) ドキュメント。

* **ID**:詳しくは、このドキュメントを読んで、方法を説明してください [Platform Web SDK は ID サービスを使用します](../../identity/overview.md).

## offer decisioning

offer decisioningを有効にするには、次の手順を実行します。

1. でAdobe Experience Platformを有効にした [datastream](../../datastreams/overview.md) 「Offer decisioning」ボックスをオンにします。

   ![offer-decisioning-edge-config](./assets/offer-decisioning-edge-config.png)

1. 手順に従って、 [SDK のインストール](../../fundamentals/installing-the-sdk.md) (SDK は、スタンドアロンでインストールすることも、 [データ収集 UI](https://experience.adobe.com/#/data-collection/). 詳しくは、 [タグクイックスタートガイド](../../../tags/quick-start/quick-start.md)) を参照してください。
1. [SDK の設定](../../fundamentals/configuring-the-sdk.md) offer decisioning その他のOffer decisioning固有の手順を以下に示します。

   * スタンドアロン SDK のインストール

      1. 「sendEvent」アクションを `decisionScopes`

         ```javascript
          alloy("sendEvent", {
             ...
             "decisionScopes": [
                 "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTIxYWIwOWMxM2JkZDIyNCIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjEyMWFiMDZhODRkMDViMTEifQ==",
                 "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTIxYWIyNWI5NTUwNWIxZiIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjEyMWFiMjFmOTQzMDE0MmIifQ=="
             ]
          })
         ```
   * タグを使用した SDK のインストール

      1. [タグプロパティの作成](../../../tags/ui/administration/companies-and-properties.md)
      1. [埋め込みコードの追加](https://experienceleague.adobe.com/docs/core-services-learn/implementing-in-websites-with-launch/configure-launch/launch-add-embed.html)
      1. 「データストリーム」ドロップダウンから設定を選択し、作成したデータストリームを使用して、Platform Web SDK 拡張機能をインストールして設定します。 詳しくは、[拡張機能](../../../tags/ui/managing-resources/extensions/overview.md)に関するドキュメントを参照してください。

         ![install-aep-web-sdk-extension](./assets/install-aep-web-sdk-extension.png)

         ![configure-aep-web-sdk-extension](./assets/configure-aep-web-sdk-extension.png)

      1. 必要な[データ要素](../../../tags/ui/managing-resources/data-elements.md)を作成します。少なくとも、Platform Web SDK ID マップおよび Platform Web SDK XDM オブジェクトデータ要素を作成する必要があります。

         ![identity-map-data-element](./assets/identity-map-data-element.png)

         ![xdm-object-data-element](./assets/xdm-object-data-element.png)

      1. を [ルール](../../../tags/ui/managing-resources/rules.md).

         * Platform Web SDK の「イベントの送信」アクションを追加し、関連する `decisionScopes` アクションの設定に対して

            ![send-event-action-decisionScopes](./assets/send-event-action-decisionScopes.png)
      1. [ライブラリの作成と公開](../../../tags/ui/publishing/libraries.md) には、設定したすべての関連するルール、データ要素、拡張機能が含まれています。



## リクエストと応答のサンプル

### 1 `decisionScopes` 値

**リクエスト**

```json
{
  "events": [
    {
      "xdm": {
        "identityMap": {
          "ECID": [
            {
              "id": "91133425615229052182584359620783097099"
            }
          ]
        }
      },
      "query": {
        "personalization": {
          "decisionScopes": [
            "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="
          ]
        }
      }
    }
  ]
}
```

| プロパティ | 必須 | 説明 | 制限 | 例 |
|---|---|---|---|---|
| `identityMap` | ○ | 詳しくは、 [ID サービスドキュメント](../../identity/overview.md). | リクエストごとに 1 つの ID。 | `{ "identityMap": { "ECID": [ { "id": "91133425615229052182584359620783097099" } ] } }`。<br><br> 注意：ユーザーは、 `ECID` パラメーターを使用して設定する必要があります。 必要に応じて、このパラメーターは呼び出しに自動的に追加されます。 |
| `decisionScopes` | ○ | アクティビティ ID と配置 ID を含む、JSON の Base64 エンコードされた文字列の配列。 | 最大 30 `decisionScopes` リクエストごと。 | `"decisionScopes": ["eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="]` |

**応答**

```json
{
  "requestId": "94c4f2f1-9218-43ce-afd3-eb0d853c5174",
  "handle": [
    {
      "payload": [
        {
          "id": "2862bb89-5df2-4bc6-85c2-d8f7e1a091de",
          "scope": "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ==",
          "activity": {
            "id": "xcore:offer-activity:11cfb1fa93381aca",
            "etag": "2"
          },
          "placement": {
            "id": "xcore:offer-placement:1175009612b0100c",
            "etag": "1"
          },
          "items": [
            {
              "id": "xcore:personalized-offer:124cc332095cfa74",
              "schema": "https://ns.adobe.com/experience/offer-management/content-component-html",
              "etag": "1",
              "data": {
                "id": "xcore:personalized-offer:124cc332095cfa74",
                "format": "text/html",
                "language": [
                  "en-US"
                ],
                "content": "<p>20% Off on shipping</p>",
                "characteristics": {
                  "foo": "bar",
                  "foo1": "bar1"
                }
              }
            }
          ]
        }
      ],
      "type": "personalization:decisions",
      "eventIndex": 0
    }
  ]
}
```

| プロパティ | 説明 | 例 |
|---|---|---|
| `scope` | 提案されたオファーを導いた決定範囲。 | `"scope": "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="` |
| `activity.id` | オファーアクティビティの一意の ID。 | `"id": "xcore:offer-activity:11cfb1fa93381aca"` |
| `placement.id` | オファー配置の一意の ID。 | `"id": "xcore:offer-placement:1175009612b0100c"` |
| `items.id` | 提案されたオファーの ID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `schema` | 提案されたオファーに関連付けられたコンテンツのスキーマ。 | `"schema": "https://ns.adobe.com/experience/offer-management/content-component-html"` |
| `data.id` | 提案されたオファーの ID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `format` | 提案されたオファーに関連付けられたコンテンツの形式。 | `"format": "text/html"` |
| `language` | 提案されたオファーのコンテンツに関連付けられた言語の配列。 | `"language": [ "en-US" ]` |
| `content` | 提案されたオファーに関連付けられた、文字列の形式のコンテンツ。 | `"content": "<p style="color:red;">20% Off on shipping</p>"` |
| `deliveryUrl` | 提案されたオファーに関連付けられた画像コンテンツ（URL 形式）。 | `"deliveryURL": "https://image.jpeg"` |
| `characteristics` | 提案されたオファーに JSON オブジェクトの形式で関連付けられた特性。 | `"characteristics": { "foo": "bar", "foo1": "bar1" }` |

### 複数 `decisionScopes` 値

**リクエスト**

```json
{
  "events": [
    {
      "xdm": {
        "identityMap": {
          "ECID": [
            {
              "id": "91133425615229052182584359620783097099"
            }
          ]
        }
      },
      "query": {
        "personalization": {
          "decisionScopes": [
            "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ==",
            "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTIyMjA4YjNhODc0MDU1OCIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjEyMjIwNDUyOTUxNGEyYzAifQ==",
            "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTIyYzkxMzg1Mjc2MDE4YyIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjEyMzMxZjU2MTYyYWEyZjcifQ=="
          ]
        }
      }
    }
  ]
}
```

| プロパティ | 必須 | 説明 | 制限 | 例 |
|---|---|---|---|---|
| `identityMap` | ○ | 詳しくは、 [ID サービスドキュメント](../../identity/overview.md). | リクエストごとに 1 つの ID。 | `{ "identityMap": { "ECID": [ { "id": "91133425615229052182584359620783097099" } ] } }`。<br><br> 注意：ユーザーは、 `ECID` パラメーターを使用して設定する必要があります。 必要に応じて、このパラメーターは呼び出しに自動的に追加されます。 |
| `decisionScopes` | ○ | アクティビティ ID と配置 ID を含む、JSON の Base64 エンコードされた文字列の配列。 | 最大 30 `decisionScopes` リクエストごと。 | `"decisionScopes":["eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ==", "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTIyMjA4YjNhODc0MDU1OCIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjEyMjIwNDUyOTUxNGEyYzAifQ=="` |

**応答**

```json
{
  "requestId": "94c4f2f1-9218-43ce-afd3-eb0d853c5174",
  "handle": [
    {
      "payload": [
        {
          "id": "a2804dfb-a0ec-4df9-8311-59d3ecdeb642",
          "scope": "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MTEyMyIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDExMjMifQ==",
          "activity": {
            "id": "xcore:offer-activity:11cfb1fa93381123",
            "etag": "1"
          },
          "placement": {
            "id": "xcore:offer-placement:1175009612b01123",
            "etag": "3"
          },
          "items": [
            {
              "id": "xcore:personalized-offer:11e36d4a22954123",
              "schema": "https://ns.adobe.com/experience/offer-management/content-component-text",
              "etag": "2",
              "data": {
                "id": "xcore:personalized-offer:11e36d4a22954123",
                "format": "text/text",
                "language": [
                  "en"
                ],
                "content": "20% Off on shipping",
                "characteristics": {
                  "foo2": "bar2"
                }
              }
            }
          ]
        },
        {
          "id": "a2804dfb-a0ec-4df9-8311-59d3ecdeb642",
          "scope": "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ==",
          "activity": {
            "id": "xcore:offer-activity:11cfb1fa93381aca",
            "etag": "2"
          },
          "placement": {
            "id": "xcore:offer-placement:1175009612b0100c",
            "etag": "1"
          },
          "items": [
            {
              "id": "xcore:personalized-offer:11e36d4a2295415d",
              "schema": "https://ns.adobe.com/experience/offer-management/content-component-imagelink",
              "etag": "1",
              "data": {
                "id": "xcore:personalized-offer:11e36d4a2295415d",
                "format": "image/png",
                "language": [
                  "en"
                ],
                "deliveryURL": "https://image.jpeg",
                "characteristics": {
                  "foo": "bar",
                  "foo1": "bar1"
                }
              }
            }
          ]
        }
      ],
      "type": "personalization:decisions",
      "eventIndex": 0
    }
  ]
}
```

| プロパティ | 説明 | 例 |
|---|---|---|
| `scope` | 提案されたオファーを導いた決定範囲。 | `"scope": "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="` |
| `activity.id` | オファーアクティビティの一意の ID。 | `"id": "xcore:offer-activity:11cfb1fa93381123"` |
| `placement.id` | オファー配置の一意の ID。 | `"xcore:offer-placement:1175009612b01123"` |
| `items.id` | 提案されたオファーの ID。 | `"id": "xcore:personalized-offer:11e36d4a22954123"` |
| `schema` | 提案されたオファーに関連付けられたコンテンツのスキーマ。 | `"schema": "https://ns.adobe.com/experience/offer-management/content-component-text"` |
| `data.id` | 提案されたオファーの ID。 | `"id": "xcore:personalized-offer:11e36d4a22954123"` |
| `format` | 提案されたオファーに関連付けられたコンテンツの形式。 | `"format": "text/text"` |
| `language` | 提案されたオファーのコンテンツに関連付けられた言語の配列。 | `"language": [ "en-US" ]` |
| `content` | 提案されたオファーに関連付けられた、文字列の形式のコンテンツ。 | `"content": "<p style="color:red;">20% Off on shipping</p>"` |
| `deliveryUrl` | 提案されたオファーに関連付けられた画像コンテンツ（URL 形式）。 | `"deliveryURL": "https://image.jpeg"` |
| `characteristics` | 提案されたオファーに JSON オブジェクトの形式で関連付けられた特性。 | `"characteristics": { "foo": "bar", "foo1": "bar1" }` |

## 制限事項

キャッピングなど、一部のオファー制約は現在、モバイル Experience Edge ワークフローではサポートされていません。キャッピングフィールド値は、1 つのオファーをすべてのユーザーに対して提示できる回数を指定します。詳しくは、[オファーの実施要件ルールと制約に関するドキュメント](https://experienceleague.adobe.com/docs/offer-decisioning/using/managing-offers-in-the-offer-library/creating-personalized-offers.html#eligibility)を参照してください。
