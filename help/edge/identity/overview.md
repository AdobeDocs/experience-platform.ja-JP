---
title: Experience CloudIDを取得しています
seo-title: Adobe Experience PlatformWeb SDKによるExperience CloudIDの取得
description: Adobe Experience CloudIDの取得方法を説明します。
seo-description: Adobe Experience CloudIDの取得方法を説明します。
keywords: Identity;First Party Identity;Identity Service;3rd Party Identity;ID Migration;Visitor ID;third party identity;thirdPartyCookiesEnabled;idMigrationEnabled;getIdentity;Syncing Identities;syncIdentity;sendEvent;identityMap;primary;ecid;Identity Namespace;namespace id;authenticationState;hashEnabled;
translation-type: tm+mt
source-git-commit: 1b5ee9b1f9bdc7835fa8de59020b3eebb4f59505
workflow-type: tm+mt
source-wordcount: '731'
ht-degree: 4%

---


# ID -Experience CloudIDの取得

Adobe Experience PlatformWeb SDKは、 [AdobeIDサービスを利用します](../../identity-service/ecid.md)。 これにより、各デバイスに固有の識別子が保持され、ページ間のアクティビティを相互に関連付けることができます。

## ファーストパーティID

は、そのIDをファーストパーティドメインのcookieに [!DNL Identity Service] 保存します。 は、ドメインのHTTPヘッダーを使用してcookieを設定しようとします。 [!DNL Identity Service] その場合、はJavaScriptを使用したCookieの設定に戻 [!DNL Identity Service] ります。 Adobeでは、クライアント側のITP制限によってcookieが制限されないようにCNAMEを設定することをお勧めします。

## サードパーティID

ID [!DNL Identity Service] をサードパーティドメイン(demdex.net)と同期して、サイト間の追跡を有効にする機能があります。 これが有効な場合、訪問者の最初のリクエスト（ECIDのないユーザーなど）は、demdex.netに対して行われます。 これは、Chromeなどを許可するブラウザーでのみ実行され、設定の `thirdPartyCookiesEnabled` パラメーターで制御されます。 この機能をすべて同時に無効にする場合は、false `thirdPartyCookiesEnabled` に設定します。

## IDの移行

訪問者APIを使用して移行する場合は、既存のAMCV cookieを移行することもできます。 ECIDの移行を有効にするには、設定で `idMigrationEnabled` パラメーターを設定します。 IDの移行により、次のような使用例が可能になります。

* ドメインの一部のページが訪問者APIを使用している場合と、他のページがこのSDKを使用している場合。 この場合、SDKは、既存のAMCV cookieを読み取り、既存のECIDを持つ新しいcookieを書き込みます。 また、SDKはAMCV cookieを書き込むので、SDKが実装されたページでECIDが最初に取得された場合、訪問者APIが実装された後続のページでも同じECIDを持つようになります。
* 訪問者APIも持つページでAdobe Experience PlatformWeb SDKが設定されている場合。 このケースをサポートするために、AMCV cookieが設定されていない場合、SDKはページ上で訪問者APIを探し、それを呼び出してECIDを取得します。
* サイト全体でAdobe Experience PlatformWeb SDKを使用しており、訪問者APIを持たない場合は、返される訪問者情報を保持できるように、ECIDを移行すると便利です。 でのSDKのデプロイ後、訪問者ーのCookie `idMigrationEnabled` のほとんどが移行されるまでの時間が経過すると、設定を無効にできます。

## 訪問者IDの取得

この一意のIDを使用する場合は、 `getIdentity` コマンドを使用します。 `getIdentity` 現在の訪問者の既存のECIDを返します。 ECIDをまだ持っていない初回訪問者の場合は、新しいECIDが生成されます。

>[!NOTE]
>
>このメソッドは、通常、 [!DNL Experience Cloud] IDを読み取る必要があるカスタムソリューションで使用されます。 標準の実装では使用されません。

```javascript
alloy("getIdentity")
  .then(function(result) {
    // The command succeeded.
    console.log(result.identity.ECID);
  })
  .catch(function(error) {
    // The command failed.
    // "error" will be an error object with additional information.
  });
```

## IDの同期

>[!NOTE]
>
>この `syncIdentity` メソッドは、ハッシュ機能に加えて、バージョン2.1.0で削除されました。 バージョン2.1.0以降を使用し、IDを同期する場合は、フ `xdm` ィールドの `sendEvent``identityMap` 下にあるコマンドのオプションでIDを直接送信できます。

また、コマンド [!DNL Identity Service] を使用して、独自のIDをECIDと同期でき `syncIdentity` ます。

>[!NOTE]
>
>使用可能なすべてのIDをすべての `sendEvent` コマンドで渡すことを強くお勧めします。 これにより、パーソナライゼーションを含む様々な使用例がロック解除されます。 これで、これらのIDを `sendEvent` コマンドに渡すことができるので、DataLayerに直接配置できます。

IDを同期すると、複数のIDを使用するデバイス/ユーザーを識別し、認証状態を設定して、どのIDを主要IDと見なすかを決定できます。 IDがに設定されていない場合 `primary`、主なデフォルトはになり `ECID`ます。

```javascript
alloy("sendEvent", {
  xdm: {
    "identityMap": {
      "ID_NAMESPACE": [ // Notice how each namespace can contain multiple identifiers.
        {
          "id": "1234",
          "authenticatedState": "ambiguous",
          "primary": true
        }
      ]
    }
  }
});
```

内の各プロパティ `identityMap` は、特定の [ID名前空間に属するIDを表します](../../identity-service/namespaces.md)。 プロパティ名は、ID名前空間記号にする必要があります。この記号は、「[!UICONTROL ID]」の下のAdobe Experience Platformユーザーインターフェイスに表示されます。 プロパティ値は、そのID名前空間に関するIDの配列にする必要があります。

ID配列内の各IDオブジェクトは、次のように構造化されます。

### `id`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 文字列 | ○ | なし |

これは、特定の名前空間に対して同期するIDです。

### `authenticationState`

| **タイプ** | **必須** | **デフォルト値** | **可能な値** |
| -------- | ------------ | ----------------- | ------------------------------------ |
| 文字列 | ○ | 曖昧な | あいまい、認証済み、ログアウト |

IDの認証状態。

### `primary`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| Boolean | オプション | false |

このIDを統合プロファイルのプライマリフラグメントとして使用するかどうかを指定します。 デフォルトでは、ECIDがユーザーの主識別子として設定されます。
