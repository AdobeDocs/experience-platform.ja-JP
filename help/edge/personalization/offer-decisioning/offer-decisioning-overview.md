---
title: Offer Decisioning概要
seo-title: Offer DecisioningおよびAdobe Experience PlatformWeb SDK
description: Adobe Experience PlatformWeb SDKは、Offer Decisioningで管理されるパーソナライズされたオファーを配信およびレンダリングできます。 Offer DecisioningのUIまたはAPIを使用して、オファーやその他の関連オブジェクトを作成できます。
seo-description: Adobe Experience PlatformWeb SDKは、Offer Decisioningで管理されるパーソナライズされたオファーを配信およびレンダリングできます。 Offer DecisioningのUIまたはAPIを使用して、オファーやその他の関連オブジェクトを作成できます。
keywords: offer decisioning;decisioning;Web SDK;Platform Web SDK;personalized offers;deliver offers;offer delivery;offer personalization;
translation-type: tm+mt
source-git-commit: 5f90f238a8845cc7bf07d54b89c5c6ccff40469a
workflow-type: tm+mt
source-wordcount: '831'
ht-degree: 10%

---


# [!DNL Offer Decisioning] 概要

>[!NOTE]
>
>現在、Adobe Experience PlatformWeb SDKでのOffer Decisioningの使用は、一部のユーザーに対して早期にアクセスできます。 この機能は、すべてのIMS組織で使用できるわけではありません。

Adobe Experience Platform [!DNL Web SDK] は、Offer Decisioningで管理されているパーソナライズされたオファーを配信し、レンダリングできます。 Offer Decisioningのユーザーインターフェイス(UI)またはAPIを使用して、オファーやその他の関連オブジェクトを作成できます。

## 前提条件

* IMS組織がEdge Decisioningで有効になっている
* オファー、作成されたアクティビティ
* エッジ設定が公開されている

## 用語

Offer Decisioningとの連携に際しては、以下の用語を理解することが重要です。 詳細およびその他の用語の表示については、 [Offer Decisioningの用語集を参照してください](https://experienceleague.adobe.com/docs/offer-decisioning/using/get-started/glossary.html)。

* **コンテナ:** コンテナとは、異なる懸念を切り離すための分離メカニズムです。 コンテナIDは、すべてのリポジトリAPIの最初のパス要素です。 すべての判定オブジェクトはコンテナ内に存在します。

* **決定範囲：** Offer Decisioningの場合、これらはBase64エンコードされたJSONの文字列で、オファーの提案にオファー判定サービスで使用するアクティビティIDとプレースメントIDが含まれます。

   *Decision scope JSON:*

   ```json
   {
     "activityId":"xcore:offer-activity:11cfb1fa93381aca",
     "placementId":"xcore:offer-placement:1175009612b0100c"
   }
   ```

   *Decision scope Base64エンコードされた文字列：*

   ```json
   "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="
   ```

   >[!TIP]
   >
   >UIの **アクティビティの概要** ページから、決定範囲の値をコピーできます。

   ![](assets/decision-scope-copy.png)

* **エッジ設定：** 詳しくは、 [エッジ設定ドキュメントを参照してください](../../fundamentals/edge-configuration.md) 。

* **ID**:詳しくは、 [プラットフォームWeb SDKがIdentity Serviceを利用する方法を説明するドキュメントをお読みください](../../identity/overview.md)。

## Offer Decisioningを有効にする

Offer Decisioningを有効にするには、次の手順を実行する必要があります。

1. エッ [ジ設定でAdobe Experience Platformを有効にし](../../fundamentals/edge-configuration.md) 、「Offer Decisioning」ボックスをオンにします。
   ![オファー判定エッジ設定](./assets/offer-decisioning-edge-config.png)
2. 手順に従ってSDK [をインストールします](../../fundamentals/installing-the-sdk.md) (SDKはスタンドアロンまたは [Adobe Experience Platform Launch経由でインストールできます](http://launch.adobe.com/jp))。 ここでは、Platform Launchの [クイック開始ガイドを示します](https://docs.adobe.com/content/help/ja-JP/launch/using/intro/get-started/quick-start.html))。
3. [Offer Decisioning向けSDK](../../fundamentals/configuring-the-sdk.md) を設定します。 以下に、Offer Decisioning固有の手順を示します。
   * スタンドアロンでインストールされたSDK
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
   * プラットフォームの起動がインストールされたSDK
      1. [Platform Launchプロパティの作成](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/companies-and-properties.html)
      2. [プ追加ラットフォームの埋め込みコードの起動](https://docs.adobe.com/content/help/en/core-services-learn/implementing-in-websites-with-launch/configure-launch/launch-add-embed.html)
      3. 先ほど作成したエッジ設定で、「エッジ設定」ドロップダウンから設定を選択し、AEP Web SDK Extensionをインストールして設定します。 [拡張機能に関する便利なドキュメント](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/extensions/overview.html)。
         ![install-aep-web-sdk-extension](./assets/install-aep-web-sdk-extension.png)

         ![configure-aep-web-sdk-extension](./assets/configure-aep-web-sdk-extension.png)
      4. 必要な [データ要素を作成します](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/data-elements.html)。 最低でも、プラットフォームWeb SDK Identity MapとプラットフォームWeb SDK XDMオブジェクトデータ要素を作成する必要があります。
         ![identity-map-data-element](./assets/identity-map-data-element.png)

         ![xdm-object-data-element](./assets/xdm-object-data-element.png)
      5. ルー [ルの作成](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/rules.html)。
         * プ追加ラットフォームWeb SDKの送信イベントアクションを追加し、そのアクション `decisionScopes` の設定に関連する
            ![send-イベント-action-decisionScopes](./assets/send-event-action-decisionScopes.png)
      6. [作成して発行するライブラリには](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/publish/libraries.html) 、設定したすべての関連ルール、データ要素、拡張子が含まれています


## リクエストと応答のサンプル

### 1つの `decisionScopes` 値

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
| `identityMap` | ○ | この『 [IDサービスドキュメント](../../identity/overview.md)』を参照してください。 | 1つのリクエストにつき1つのID。 | `{ "identityMap": { "ECID": [ { "id": "91133425615229052182584359620783097099" } ] } }` |
| `decisionScopes` | ○ | アクティビティIDと配置IDを含むJSONのBase64エンコードされた文字列の配列。 | リクエスト `decisionScopes` あたり最大30件。 | `"decisionScopes": ["eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="]` |

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
          "items": [
            {
              "id": "xcore:personalized-offer:124cc332095cfa74",
              "schema": "https://ns.adobe.com/experience/offer-management/content-component-html",
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
| `scope` | 提案されたオファーに導いた決定範囲。 | `"scope": "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="` |
| `items.id` | 提案されたオファーのID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `schema` | 提案されたオファーに関連付けられたコンテンツのスキーマ。 | `"schema": "https://ns.adobe.com/experience/offer-management/content-component-html"` |
| `data.id` | 提案されたオファーのID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `format` | 提案されたオファーに関連付けられたコンテンツの形式。 | `"format": "text/html"` |
| `language` | 提案されたオファーの内容に関連付けられた言語の配列です。 | `"language": [ "en-US" ]` |
| `content` | 提示されたオファーに関連付けられたコンテンツを、文字列の形式で示します。 | `"content": "<p style="color:red;">20% Off on shipping</p>"` |
| `deliveryUrl` | 提案されたオファーに関連付けられた画像コンテンツをURL形式で表示します。 | `"deliveryURL": "https://image.jpeg"` |
| `characteristics` | JSONオブジェクトの形式で、提案されたオファーに関連付けられた特性。 | `"characteristics": { "foo": "bar", "foo1": "bar1" }` |

### 複数の `decisionScopes` 値

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
| `identityMap` | ○ | この『 [IDサービスドキュメント](../../identity/overview.md)』を参照してください。 | 1つのリクエストにつき1つのID。 | `{ "identityMap": { "ECID": [ { "id": "91133425615229052182584359620783097099" } ] } }` |
| `decisionScopes` | ○ | アクティビティIDと配置IDを含むJSONのBase64エンコードされた文字列の配列。 | リクエスト `decisionScopes` あたり最大30件。 | `"decisionScopes":["eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ==", "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTIyMjA4YjNhODc0MDU1OCIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjEyMjIwNDUyOTUxNGEyYzAifQ=="` |

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
          "items": [
            {
              "id": "xcore:personalized-offer:124cc332095cfa74",
              "schema": "https://ns.adobe.com/experience/offer-management/content-component-html",
              "data": {
                "id": "xcore:personalized-offer:124cc332095cfa74",
                "format": "text/html",
                "language": [
                  "en-US"
                ],
                "content": "<p style=\"color:red;\">20% Off on shipping</p>",
                "characteristics": {
                  "foo": "bar",
                  "foo1": "bar1"
                }
              }
            }
          ]
        }
      ],
      "payload": [
        {
          "id": "2862bb89-5df2-4bc6-85c2-d8f7e1a091de",
          "scope": "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTIyMjA4YjNhODc0MDU1OCIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjEyMjIwNDUyOTUxNGEyYzAifQ==",
          "items": [
            {
              "id": "xcore:personalized-offer:235fe313094cdb75",
              "schema": "https://ns.adobe.com/experience/offer-management/content-component-text",
              "data": {
                "id": "xcore:personalized-offer:235fe313094cdb75",
                "format": "text/text",
                "language": [
                  "en-US"
                ],
                "content": "20% Off on shipping",
                "characteristics": {
                  "foo2": "bar2",
                  "foo3": "bar3"
                }
              }
            }
          ]
        }
      ],
      "payload": [
        {
          "id": "2862bb89-5df2-4bc6-85c2-d8f7e1a091de",
          "scope": "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTIyYzkxMzg1Mjc2MDE4YyIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjEyMzMxZjU2MTYyYWEyZjcifQ==",
          "items": [
            {
              "id": "xcore:personalized-offer:312de312095cda65",
              "schema": "https://ns.adobe.com/experience/offer-management/content-component-imagelink",
              "data": {
                "id": "xcore:personalized-offer:312de312095cda65",
                "format": "image/png",
                "language": [
                  "en-US"
                ],
                "deliveryURL": "https://image.jpeg"
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
| `scope` | 提案されたオファーに導いた決定範囲。 | `"scope": "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="` |
| `items.id` | 提案されたオファーのID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `schema` | 提案されたオファーに関連付けられたコンテンツのスキーマ。 | `"schema": "https://ns.adobe.com/experience/offer-management/content-component-html"` |
| `data.id` | 提案されたオファーのID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `format` | 提案されたオファーに関連付けられたコンテンツの形式。 | `"format": "text/html"` |
| `language` | 提案されたオファーの内容に関連付けられた言語の配列です。 | `"language": [ "en-US" ]` |
| `content` | 提示されたオファーに関連付けられたコンテンツを、文字列の形式で示します。 | `"content": "<p style="color:red;">20% Off on shipping</p>"` |
| `deliveryUrl` | 提案されたオファーに関連付けられた画像コンテンツをURL形式で表示します。 | `"deliveryURL": "https://image.jpeg"` |
| `characteristics` | JSONオブジェクトの形式で、提案されたオファーに関連付けられた特性。 | `"characteristics": { "foo": "bar", "foo1": "bar1" }` |
