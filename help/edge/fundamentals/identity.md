---
title: Experience Cloud IDの取得
seo-title: Adobe Experience Platform Web SDK Experience Cloud IDの取得
description: Adobe Experience Cloud IDを取得する方法を説明します。
seo-description: Adobe Experience Cloud IDを取得する方法を説明します。
translation-type: tm+mt
source-git-commit: e9fb726ddb84d7a08afb8c0f083a643025b0f903
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 7%

---


# Experience Cloud IDの取得

Adobe Experience Platform Web SDKは、 [Adobe Identity Service](../../identity-service/ecid.md)を利用します。 これにより、各デバイスに固有の識別子が保持され、ページ間のアクティビティを相互に関連付けることができます。

## ファーストパーティID

IDサービスは、IDをファーストパーティドメインのcookieに保存します。 IDサービスは、ドメインのHTTPヘッダーを使用してcookieを設定しようとしますが、それに失敗すると、IDサービスがJavaScriptを使用したcookieの設定に戻ります。 クライアント側のITP制限によってcookieの上限が設定されないように、CNAMEを設定することをお勧めします。

## サードパーティID

IDサービスでは、IDをサードパーティドメイン(demdex.net)と同期して、サイト間での追跡を有効にすることができます。 これが有効な場合、訪問者の最初のリクエスト（ECIDのないユーザーなど）は、demdex.netに対して行われます。 これは、Chromeなどを許可するブラウザーでのみ実行され、設定の `thirdPartyCookiesEnabled` パラメーターで制御されます。 この機能を無効にする場合は、すべてfalseに設定 `thirdPartyCookiesEnabled` します。

## 訪問者IDの取得

この一意のIDを使用する場合は、 `getIdentity` コマンドを使用します。 `getIdentity` 現在の訪問者の既存のECIDを返します。 ECIDをまだ持っていない初回訪問者の場合は、新しいECIDが生成されます。

>[!NOTE]
>
>この方法は、通常、Experience Cloud IDを読み取る必要があるカスタムソリューションで使用されます。 標準の実装では使用されません。

```javascript
alloy("getIdentity")
  .then(function(result.identity.ECID) {
    // This function will get called with Adobe Experience Cloud Id when the command promise is resolved
  })
  .catch(function(error) {
    // The command failed.
    // "error" will be an error object with additional information
  })
```

## IDの同期

また、IDサービスでは、 `syncIdentity` コマンドを使用して独自のIDをECIDと同期できます。

```javascript
alloy("syncIdentity",{
    identity:{
      "AppNexus":{
        "id":"123456,
        "authenticationState":"ambiguous",
        "primary":false,
        "hashEnabled": true,
      }
    }
})
```

### IDの同期オプション

#### 識別名前空間記号

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 文字列 | ○ | なし |

オブジェクトのキーは、 [ID名前空間](../../identity-service/namespaces.md) (Identity Symbol)です。 これは、「ID」の下のAdobe Experience Platform UIに表示されます。

#### `id`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 文字列 | ○ | なし |

これは、特定の名前空間に対して同期するIDです。

#### `primary`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| Boolean | オプション | false |

このIDを統合プロファイルのプライマリフラグメントとして使用する場合。 デフォルトでは、ECIDがユーザーの主な識別子として設定されます。

#### `hashEnabled`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| Boolean | オプション | false |

有効にすると、SHA256ハッシュを使用してIDをハッシュします。