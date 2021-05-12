---
title: offer decisioningとプラットフォームWeb SDKの使用
description: Adobe Experience PlatformWeb SDKは、Offer decisioningで管理されるパーソナライズされたオファーを配信およびレンダリングできます。 offer decisioningのUIまたはAPIを使用して、オファーやその他の関連オブジェクトを作成できます。
keywords: offer decisioning；判定；Web SDK；プラットフォームWeb SDK；パーソナライズされたオファー；配信オファー;オファー配信;オファーのパーソナライズ；
exl-id: 4ab51f9d-3c44-4855-b900-aa2cde673a9a
source-git-commit: 20adb26fbd55302ac8005978968a0d69bdda8755
workflow-type: tm+mt
source-wordcount: '842'
ht-degree: 16%

---

# offer decisioningとプラットフォームWeb SDKの使用

>[!NOTE]
>
>現在、Adobe Experience Platform Web SDK での Offer Decisioning の使用は、一部のユーザーが早期にアクセスできます。この機能は、すべての IMS 組織で使用できるわけではありません。

Adobe Experience Platform[!DNL Web SDK]は、Offer decisioningで管理されるパーソナライズされたオファーを配信およびレンダリングできます。 offer decisioningユーザーインターフェイス(UI)またはAPIを使用して、オファーやその他の関連オブジェクトを作成できます。

## 前提条件

* IMS組織がedge decisioningに対して有効になっている
* オファー、作成されたアクティビティ
* データストリームが発行される

## 用語

offer decisioningを扱う際は、次の用語を理解することが重要です。 詳細および追加の用語の表示については、[Offer decisioning用語集](https://experienceleague.adobe.com/docs/offer-decisioning/using/get-started/glossary.html)を参照してください。

* **コンテナ:** コンテナとは、異なる懸念を切り離すための分離メカニズムです。コンテナ ID は、すべてのリポジトリー API の最初のパス要素です。すべての決定オブジェクトはコンテナ内に存在します。

* **デシジョンスコープ：Offer decisioning** の場合、これらは、オファーの提案にoffer decisioningサービスで使用するアクティビティIDとプレースメントIDを含むBase64エンコードされたJSON文字列です。

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
   >UIの&#x200B;**アクティビティの概要**&#x200B;ページから、決定範囲の値をコピーできます。

   ![](assets/decision-scope-copy.png)

* **データストリーム：** 詳しくは、 [](../../fundamentals/datastreams.md) datastreamsのドキュメントをお読みください。

* **ID**:詳しくは、 [プラットフォームWeb SDKがIdentity Serviceを利用する方法を説明するドキュメントをお読みください](../../identity/overview.md)。

## offer decisioningの有効化

offer decisioningを有効にするには、次の手順を実行する必要があります。

1. [データストリーム](../../fundamentals/datastreams.md)のAdobe Experience Platformを有効にし、「Offer decisioning」ボックスをオンにします
   ![オファー判定エッジ設定](./assets/offer-decisioning-edge-config.png)
2. [SDK](../../fundamentals/installing-the-sdk.md)のインストールの指示に従います(SDKは、スタンドアロンまたは[Adobe Experience Platform Launch](http://launch.adobe.com/jp)を介してインストールできます)。 以下に、[Platform launch](https://docs.adobe.com/content/help/ja-JP/launch/using/intro/get-started/quick-start.html)のクイック開始ガイドを示します。
3. [offer decisioning用の](../../fundamentals/configuring-the-sdk.md) SDKを設定します。その他のOffer decisioning固有の手順を以下に示します。
   * スタンドアロンでインストールされたSDK
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

   * platform launchがインストールしたSDK
      1. [platform launchプロパティの作成](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/admin/companies-and-properties.html)
      2. [platform launch追加埋め込みコード](https://docs.adobe.com/content/help/en/core-services-learn/implementing-in-websites-with-launch/configure-launch/launch-add-embed.html)
      3. 先ほど作成したデータストリームを使用して、「Datastream」ドロップダウンから設定を選択し、Platform Web SDK Extensionをインストールして設定します。 [拡張子](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/extensions/overview.html)のドキュメントを参照してください。
         ![install-aep-web-sdk-extension](./assets/install-aep-web-sdk-extension.png)

         ![configure-aep-web-sdk-extension](./assets/configure-aep-web-sdk-extension.png)
      4. 必要な[データ要素](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/data-elements.html)を作成します。 少なくとも、プラットフォームWeb SDK Identity MapとプラットフォームWeb SDK XDMオブジェクトデータ要素を作成する必要があります。
         ![identity-map-data-element](./assets/identity-map-data-element.png)

         ![xdm-object-data-element](./assets/xdm-object-data-element.png)
      5. [ルール](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/manage-resources/rules.html)を作成します。
         * プ追加ラットフォームWeb SDKのイベント送信アクションを追加し、関連する`decisionScopes`をそのアクションの設定に追加します
            ![send-イベント-action-decisionScopes](./assets/send-event-action-decisionScopes.png)
      6. [作成して発行するライブラリには、設定したすべての関連するルール、データ要素、拡張が含まれています](https://docs.adobe.com/content/help/ja-JP/launch/using/reference/publish/libraries.html) 



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
| `identityMap` | ○ | [IDサービスのドキュメント](../../identity/overview.md)を参照してください。 | 1つのリクエストにつき1つのID。 | `{ "identityMap": { "ECID": [ { "id": "91133425615229052182584359620783097099" } ] } }` |
| `decisionScopes` | ○ | アクティビティIDと配置IDを含むJSONのBase64エンコードされた文字列の配列。 | リクエストあたり最大30 `decisionScopes` | `"decisionScopes": ["eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="]` |

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
| `scope` | 提案されたオファーに導いた決定範囲。 | `"scope": "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="` |
| `activity.id` | オファーアクティビティの一意のID。 | `"id": "xcore:offer-activity:11cfb1fa93381aca"` |
| `placement.id` | オファー配置の一意のID。 | `"id": "xcore:offer-placement:1175009612b0100c"` |
| `items.id` | 提案されたオファーのID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `schema` | 提案されたオファーに関連付けられたコンテンツのスキーマ。 | `"schema": "https://ns.adobe.com/experience/offer-management/content-component-html"` |
| `data.id` | 提案されたオファーのID。 | `"id": "xcore:personalized-offer:124cc332095cfa74"` |
| `format` | 提案されたオファーに関連付けられたコンテンツの形式。 | `"format": "text/html"` |
| `language` | 提案されたオファーの内容に関連付けられた言語の配列です。 | `"language": [ "en-US" ]` |
| `content` | 提示されたオファーに関連付けられたコンテンツを、文字列の形式で示します。 | `"content": "<p style="color:red;">20% Off on shipping</p>"` |
| `deliveryUrl` | 提案されたオファーに関連付けられた画像コンテンツをURL形式で表示します。 | `"deliveryURL": "https://image.jpeg"` |
| `characteristics` | JSONオブジェクトの形式で、提案されたオファーに関連付けられた特性。 | `"characteristics": { "foo": "bar", "foo1": "bar1" }` |

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
| `identityMap` | ○ | [IDサービスのドキュメント](../../identity/overview.md)を参照してください。 | 1つのリクエストにつき1つのID。 | `{ "identityMap": { "ECID": [ { "id": "91133425615229052182584359620783097099" } ] } }` |
| `decisionScopes` | ○ | アクティビティIDと配置IDを含むJSONのBase64エンコードされた文字列の配列。 | リクエストあたり最大30 `decisionScopes` | `"decisionScopes":["eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ==", "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTIyMjA4YjNhODc0MDU1OCIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjEyMjIwNDUyOTUxNGEyYzAifQ=="` |

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
| `scope` | 提案されたオファーに導いた決定範囲。 | `"scope": "eyJhY3Rpdml0eUlkIjoieGNvcmU6b2ZmZXItYWN0aXZpdHk6MTFjZmIxZmE5MzM4MWFjYSIsInBsYWNlbWVudElkIjoieGNvcmU6b2ZmZXItcGxhY2VtZW50OjExNzUwMDk2MTJiMDEwMGMifQ=="` |
| `activity.id` | オファーアクティビティの一意のID。 | `"id": "xcore:offer-activity:11cfb1fa93381123"` |
| `placement.id` | オファー配置の一意のID。 | `"xcore:offer-placement:1175009612b01123"` |
| `items.id` | 提案されたオファーのID。 | `"id": "xcore:personalized-offer:11e36d4a22954123"` |
| `schema` | 提案されたオファーに関連付けられたコンテンツのスキーマ。 | `"schema": "https://ns.adobe.com/experience/offer-management/content-component-text"` |
| `data.id` | 提案されたオファーのID。 | `"id": "xcore:personalized-offer:11e36d4a22954123"` |
| `format` | 提案されたオファーに関連付けられたコンテンツの形式。 | `"format": "text/text"` |
| `language` | 提案されたオファーの内容に関連付けられた言語の配列です。 | `"language": [ "en-US" ]` |
| `content` | 提示されたオファーに関連付けられたコンテンツを、文字列の形式で示します。 | `"content": "<p style="color:red;">20% Off on shipping</p>"` |
| `deliveryUrl` | 提案されたオファーに関連付けられた画像コンテンツをURL形式で表示します。 | `"deliveryURL": "https://image.jpeg"` |
| `characteristics` | JSONオブジェクトの形式で、提案されたオファーに関連付けられた特性。 | `"characteristics": { "foo": "bar", "foo1": "bar1" }` |
