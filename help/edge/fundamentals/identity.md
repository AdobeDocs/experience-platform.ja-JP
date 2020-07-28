---
title: Experience CloudIDを取得しています
seo-title: Experience CloudIDを取得するAdobe Experience PlatformWeb SDK
description: Adobe Experience CloudIDの取得方法を説明します。
seo-description: Adobe Experience CloudIDの取得方法を説明します。
translation-type: tm+mt
source-git-commit: 7b07a974e29334cde2dee7027b9780a296db7b20
workflow-type: tm+mt
source-wordcount: '408'
ht-degree: 10%

---


# ID -Experience CloudIDの取得

Adobe Experience Platformは、 [!DNL Web SDK] AdobeIDサービスを [利用します](../../identity-service/ecid.md)。 これにより、各デバイスに固有の識別子が保持され、ページ間のアクティビティを相互に関連付けることができます。

## ファーストパーティID

は、そのIDをファーストパーティドメインのcookieに [!DNL Identity Service] 保存します。 は、ドメインのHTTPヘッダーを使用してcookieを設定しようとします。 [!DNL Identity Service] その場合、はJavaScriptを使用したCookieの設定に戻 [!DNL Identity Service] ります。 Adobeでは、クライアント側のITP制限によってcookieが制限されないようにCNAMEを設定することをお勧めします。

## サードパーティID

ID [!DNL Identity Service] をサードパーティドメイン(demdex.net)と同期して、サイト間の追跡を有効にする機能があります。 これが有効な場合、訪問者の最初のリクエスト（ECIDのないユーザーなど）は、demdex.netに対して行われます。 これは、Chromeなどを許可するブラウザーでのみ実行され、設定の `thirdPartyCookiesEnabled` パラメーターで制御されます。 この機能をすべて同時に無効にする場合は、false `thirdPartyCookiesEnabled` に設定します。

## 訪問者IDの取得

この一意のIDを使用する場合は、 `getIdentity` コマンドを使用します。 `getIdentity` 現在の訪問者の既存のECIDを返します。 ECIDをまだ持っていない初回訪問者の場合は、新しいECIDが生成されます。

>[!NOTE]
>
>このメソッドは、通常、 [!DNL Experience Cloud] IDを読み取る必要があるカスタムソリューションで使用されます。 標準の実装では使用されません。

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

また、コマンド [!DNL Identity Service] を使用して、独自のIDをECIDと同期でき `syncIdentity` ます。

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

オブジェクトのキーは、 [ID名前空間](../../identity-service/namespaces.md) (Identity Symbol)です。 これは、「 [!UICONTROL ID]」のAdobe Experience Platformユーザーインターフェイスに表示されます。

#### `id`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 文字列 | ○ | なし |

これは、特定の名前空間に対して同期するIDです。

#### `authenticationState`

| **タイプ** | **必須** | **デフォルト値** | **可能な値** |
| -------- | ------------ | ----------------- | ------------------------------------ |
| 文字列 | ○ | 曖昧な | あいまい、認証済み、ログアウト |

IDの認証状態。

#### `primary`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| Boolean | オプション | false |

このIDを統合プロファイルのプライマリフラグメントとして使用するかどうかを指定します。 デフォルトでは、ECIDがユーザーの主識別子として設定されます。

#### `hashEnabled`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| Boolean | オプション | false |

有効にすると、SHA256ハッシュを使用してIDがハッシュされます。
