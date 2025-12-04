---
title: getVisitorId
description: Experience Cloud訪問者 ID サービスタグ拡張機能インスタンスを取得します。
source-git-commit: 434d6913ea391b127b4b52c8494730c496bbcfe2
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 1%

---

# `getVisitorId()`

`_satellite.getVisitorId()` メソッドは、ID サービス拡張機能がインストールされ公開されている場合は [ タグプロパティ内の ](https://experienceleague.adobe.com/ja/docs/id-service/using/home)1}Adobe Experience Cloud ID サービス **のインスタンスを返します。**&#x200B;この方法は、訪問者 ID インスタンスに直接アクセスしてカスタムコードブロックで使用する場合、詳細なデータ要素の設定を行う場合、訪問者 ID の問題のトラブルシューティングを行う場合に役立ちます。

>[!IMPORTANT]
>
>この方法は、スタンドアロンのExperience Cloud ID サービスタグ拡張機能を含むプロパティにのみ適用されます。 Web SDK タグ拡張機能で使用できる暗黙の ID サービス機能には適用されません。 Web SDKの暗黙的 ID サービス機能を使用して訪問者 ID を取得する必要がある場合は、[`getIdentity`](/help/collection/js/commands/getidentity.md) コマンドを参照してください。

ID サービス拡張機能をインストールして公開した状態でこのメソッドを呼び出すと、[`Visitor.getInstance()`](https://experienceleague.adobe.com/en/docs/id-service/using/id-service-api/methods/getinstance) メソッドを呼び出した後に取得したオブジェクトと同様に、オブジェクトが返されます。 ID サービス拡張機能がインストールされていないか、公開されていないときにこのメソッドを呼び出すと、メソッドは `null` を返します。

```ts
_satellite.getVisitorId(): Visitor | null
```

## 使用可能なフィールドとメソッド

使用可能なフィールドとメソッドについては、Experience Cloud訪問者 ID サービスのドキュメントのExperience Cloud ID サービス [ メソッド ](https://experienceleague.adobe.com/en/docs/id-service/using/id-service-api/methods/get-set) を参照してください。

```js
// Retrieve a visitor's ECID
_satellite.getVisitorId().getMarketingCloudVisitorID();

// Retrieve a visitor's legacy Analytics ID
_satellite.getVisitorId().getAnalyticsVisitorID();

// Retrieve a visitor's Audience Manager blob for audience sharing
_satellite.getVisitorId().getAudienceManagerBlob();
```
