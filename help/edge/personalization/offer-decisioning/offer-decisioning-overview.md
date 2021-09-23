---
title: Platform Web SDKでのOffer decisioningの使用
description: Adobe Experience Platform Web SDKは、Offer decisioningで管理されるパーソナライズされたオファーを配信し、レンダリングできます。 オファーUIまたはAPIを使用して、オファーやその他の関連オブジェクトをOffer decisioningできます。
keywords: offer decisioning；判定；Web SDK;Platform Web SDK；パーソナライズされたオファー；オファーの配信；オファーの配信；オファーのパーソナライズ；
exl-id: 4ab51f9d-3c44-4855-b900-aa2cde673a9a
source-git-commit: 5a688fed26a8f641347ed1c625bfe448004f75b0
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 7%

---

# Platform Web SDKでのOffer decisioningの使用

>[!NOTE]
>
>Adobe Experience Platform Web SDKでのOffer decisioningの使用は、一部のユーザーが早期にアクセスできる場合に便利です。 この機能は、一部の IMS 組織ではご利用いただけきません。

Adobe Experience Platform [!DNL Web SDK]は、Offer decisioningで管理されるパーソナライズされたオファーを配信し、レンダリングできます。 オファーユーザーインターフェイス(UI)またはAPIを使用して、オファーやその他の関連Offer decisioningを作成できます。

## 前提条件

* IMS組織がエッジ判定に対して有効になっている
* オファー、作成されたアクティビティ
* Datastreamが公開される

## 用語

offer decisioningを扱う際には、次の用語を理解することが重要です。 詳細および追加の用語については、[Offer decisioningの用語集](https://experienceleague.adobe.com/docs/offer-decisioning/using/get-started/glossary.html)を参照してください。

* **コンテナ：** コンテナは、様々な懸念事項を分離するための分離メカニズムです。コンテナ ID は、すべてのリポジトリー API の最初のパス要素です。すべての決定オブジェクトはコンテナ内に存在します。

* **判定範囲：** Offer decisioningの場合、判定範囲は、offer decisioningサービスでオファーの提案に使用するアクティビティIDと配置IDを含む、Base64でエンコードされたJSON文字列です。

   *判定範囲JSON:*

   ```json
   {
     "activityId":"xcore:offer-activity:11cfb1fa93381aca",
     "placementId":"xcore:offer-placement:1175009612b0100c"
   }
   ```

   *判定範囲Base64エンコードされた文字列：*

   ```json
   "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="
   ```

   >[!TIP]
   >
   >UIの&#x200B;**アクティビティの概要**&#x200B;ページから判定範囲の値をコピーできます。

   ![](assets/decision-scope-copy.png)

* **データストリーム：** 詳しくは、datastreamsのドキュメントを参照して [](../../fundamentals/datastreams.md) ください。

* **ID**:詳しくは、 [Platform Web SDKがIDサービスを使用する方法を概要する、このドキュメントをお読みください](../../identity/overview.md)。

## 有効化Offer decisioning

offer decisioningを有効にするには、次の手順を実行します。

1. [datastream](../../fundamentals/datastreams.md)でAdobe Experience Platformを有効にし、「Offer decisioning」ボックスをオンにします

   ![offer-decisioning-edge-config](./assets/offer-decisioning-edge-config.png)

1. [SDK](../../fundamentals/installing-the-sdk.md)のインストールの手順に従います（SDKは、スタンドアロンで、または[データ収集UI](https://experience.adobe.com/#/data-collection/)を使用してインストールできます）。 詳しくは、[タグのクイックスタートガイド](../../../tags/quick-start/quick-start.md)を参照してください。
1. [SDKを](../../fundamentals/configuring-the-sdk.md) Offer decisioningその他のOffer decisioning固有の手順を以下に示します。

   * スタンドアロンSDKのインストール

      1. `decisionScopes`で「sendEvent」アクションを設定します。

         ```javascript
          alloy("sendEvent", {
             ...
             "decisionScopes": [
                 "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTIxYWIwOWMxM2JkZDIyNCIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjEyMWFiMDZhODRkMDViMTEifQ==",
                 "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTIxYWIyNWI5NTUwNWIxZiIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjEyMWFiMjFmOTQzMDE0MmIifQ=="
             ]
          })
         ```
   * タグを使用したSDKのインストール

      1. [タグプロパティの作成](../../../tags/ui/administration/companies-and-properties.md)
      1. [ 埋め込みコードの追加](https://experienceleague.adobe.com/docs/core-services-learn/implementing-in-websites-with-launch/configure-launch/launch-add-embed.html)
      1. 「Datastream」ドロップダウンから設定を選択し、作成したDatastreamを使用して、Platform Web SDK拡張機能をインストールおよび設定します。 [拡張機能](../../../tags/ui/managing-resources/extensions/overview.md)に関するドキュメントを参照してください。

         ![install-aep-web-sdk-extension](./assets/install-aep-web-sdk-extension.png)

         ![configure-aep-web-sdk-extension](./assets/configure-aep-web-sdk-extension.png)

      1. 必要な[データ要素](../../../tags/ui/managing-resources/data-elements.md)を作成します。 少なくとも、Platform Web SDK Identity MapとPlatform Web SDK XDMオブジェクトデータ要素を作成する必要があります。

         ![identity-map-data-element](./assets/identity-map-data-element.png)

         ![xdm-object-data-element](./assets/xdm-object-data-element.png)

      1. [ルール](../../../tags/ui/managing-resources/rules.md)を作成します。

         * Platform Web SDKの「イベントを送信」アクションを追加し、関連する`decisionScopes`をそのアクションの設定に追加します。

            ![send-event-action-decisionScopes](./assets/send-event-action-decisionScopes.png)
      1. [設定したすべての関連](../../../tags/ui/publishing/libraries.md) するルール、データ要素および拡張機能を含むライブラリを作成して公開します。



## リクエストと応答のサンプル

### 1つの`decisionScopes`値

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
| `identityMap` | ○ | この[IDサービスのドキュメント](../../identity/overview.md)を参照してください。 | リクエストごとに1つのID。 | `{ "identityMap": { "ECID": [ { "id": "91133425615229052182584359620783097099" } ] } }` |
| `decisionScopes` | ○ | アクティビティIDと配置IDを含む、JSONのBase64エンコードされた文字列の配列。 | リクエストあたり最大30 `decisionScopes`。 | `"decisionScopes": ["eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="]` |

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
| `activity.id` | オファーアクティビティの一意のID。 | `"id": "xcore:offer-activity:11cfb1fa93381aca"` |
| `placement.id` | オファー配置の一意のID。 | `"id": "xcore:offer-placement:1175009612b0100c"` |
| `items.id` | 提案されたオファーのID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `schema` | 提案されたオファーに関連付けられたコンテンツのスキーマ。 | `"schema": "https://ns.adobe.com/experience/offer-management/content-component-html"` |
| `data.id` | 提案されたオファーのID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `format` | 提案されたオファーに関連付けられているコンテンツの形式。 | `"format": "text/html"` |
| `language` | 提案されたオファーのコンテンツに関連付けられた言語の配列。 | `"language": [ "en-US" ]` |
| `content` | 提案されたオファーに関連付けられたコンテンツ（文字列形式）。 | `"content": "<p style="color:red;">20% Off on shipping</p>"` |
| `deliveryUrl` | 提案されたオファーに関連付けられた画像コンテンツ（URL形式）。 | `"deliveryURL": "https://image.jpeg"` |
| `characteristics` | 提案されたオファーに関連付けられている特性（JSONオブジェクトの形式）。 | `"characteristics": { "foo": "bar", "foo1": "bar1" }` |

### 複数の`decisionScopes`値

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
| `identityMap` | ○ | この[IDサービスのドキュメント](../../identity/overview.md)を参照してください。 | リクエストごとに1つのID。 | `{ "identityMap": { "ECID": [ { "id": "91133425615229052182584359620783097099" } ] } }` |
| `decisionScopes` | ○ | アクティビティIDと配置IDを含む、JSONのBase64エンコードされた文字列の配列。 | リクエストあたり最大30 `decisionScopes`。 | `"decisionScopes":["eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ==", "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTIyMjA4YjNhODc0MDU1OCIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjEyMjIwNDUyOTUxNGEyYzAifQ=="` |

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
| `activity.id` | オファーアクティビティの一意のID。 | `"id": "xcore:offer-activity:11cfb1fa93381123"` |
| `placement.id` | オファー配置の一意のID。 | `"xcore:offer-placement:1175009612b01123"` |
| `items.id` | 提案されたオファーのID。 | `"id": "xcore:personalized-offer:11e36d4a22954123"` |
| `schema` | 提案されたオファーに関連付けられたコンテンツのスキーマ。 | `"schema": "https://ns.adobe.com/experience/offer-management/content-component-text"` |
| `data.id` | 提案されたオファーのID。 | `"id": "xcore:personalized-offer:11e36d4a22954123"` |
| `format` | 提案されたオファーに関連付けられているコンテンツの形式。 | `"format": "text/text"` |
| `language` | 提案されたオファーのコンテンツに関連付けられた言語の配列。 | `"language": [ "en-US" ]` |
| `content` | 提案されたオファーに関連付けられたコンテンツ（文字列形式）。 | `"content": "<p style="color:red;">20% Off on shipping</p>"` |
| `deliveryUrl` | 提案されたオファーに関連付けられた画像コンテンツ（URL形式）。 | `"deliveryURL": "https://image.jpeg"` |
| `characteristics` | 提案されたオファーに関連付けられている特性（JSONオブジェクトの形式）。 | `"characteristics": { "foo": "bar", "foo1": "bar1" }` |

## 制限事項

キャッピングなど、モバイルエクスペリエンスエッジワークフローでは、現在サポートされていないオファー制約もあります。 「キャッピング」フィールドの値は、すべてのユーザーに対してオファーを提示できる回数を指定します。 詳しくは、[オファーの実施要件ルールと制約のドキュメント](https://experienceleague.adobe.com/docs/offer-decisioning/using/managing-offers-in-the-offer-library/creating-personalized-offers.html#eligibility)を参照してください。
