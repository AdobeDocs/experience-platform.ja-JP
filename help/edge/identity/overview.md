---
title: Adobe Experience PlatformWeb SDKを使用したExperience CloudIDの取得
description: Adobe Experience PlatformWeb SDKを使用してAdobe Experience CloudID(ECID)を取得する方法を説明します。
seo-description: Adobe Experience CloudIDの取得方法を説明します。
keywords: Identity;First Party Identity;Identity Service；サードパーティID;IDの移行；訪問者ID；サードパーティID；サードパーティID;IDMigrationEnabled;getIdentity;SyncIdentity;sendEvent;primary;Id;Identity;名前空間;名前空間状態；認証；hashEnabled;
translation-type: tm+mt
source-git-commit: 882bcd2f9aa7a104270865783eed82089862dea3
workflow-type: tm+mt
source-wordcount: '963'
ht-degree: 3%

---


# Adobe Experience CloudIDの取得

Adobe Experience PlatformWeb SDKは、[AdobeIDサービス](../../identity-service/ecid.md)を利用します。 これにより、各デバイスに固有の識別子が保持され、ページ間のアクティビティを相互に関連付けることができます。

## ファーストパーティID

[!DNL Identity Service]は、IDをファーストパーティドメインのcookieに保存します。 [!DNL Identity Service]は、ドメインのHTTPヘッダーを使用してcookieを設定しようとします。 それが失敗すると、[!DNL Identity Service]はJavaScriptを使用したcookieの設定に戻ります。 Adobeでは、クライアント側のITP制限によってcookieが制限されないようにCNAMEを設定することをお勧めします。

## サードパーティID

[!DNL Identity Service]には、IDをサードパーティドメイン(demdex.net)と同期して、サイト間の追跡を有効にする機能があります。 これが有効な場合、訪問者の最初のリクエスト（例えば、ECIDのない訪問者）は、demdex.netに対して行われます。 これは、ChromeなどのChromeを許可するブラウザーでのみ実行され、設定の`thirdPartyCookiesEnabled`パラメーターで制御されます。 この機能をすべて一緒に無効にする場合は、`thirdPartyCookiesEnabled`をfalseに設定します。

## IDの移行

訪問者APIを使用して移行する場合は、既存のAMCV cookieを移行することもできます。 ECIDの移行を有効にするには、設定で`idMigrationEnabled`パラメーターを設定します。 IDの移行により、次のような使用例が可能になります。

* ドメインの一部のページが訪問者APIを使用している場合と、他のページがこのSDKを使用している場合。 この場合、SDKは、既存のAMCV cookieを読み取り、既存のECIDを持つ新しいcookieを書き込みます。 また、SDKはAMCV cookieを書き込むので、SDKが実装されたページでECIDが最初に取得された場合、訪問者APIが実装された後続のページでも同じECIDを持つようになります。
* 訪問者APIも持つページでAdobe Experience PlatformWeb SDKが設定されている場合。 このケースをサポートするために、AMCV cookieが設定されていない場合、SDKはページ上で訪問者APIを探し、それを呼び出してECIDを取得します。
* サイト全体でAdobe Experience PlatformWeb SDKを使用しており、訪問者APIを持たない場合は、返される訪問者情報を保持できるように、ECIDを移行すると便利です。 `idMigrationEnabled`を使用してSDKをデプロイして、訪問者ーのほとんどのCookieが移行されるまでの間、この設定をオフにできます。

## 移行の特性の更新

XDM形式のデータがAudience Managerに送信される場合、このデータは移行時にシグナルに変換する必要があります。 XDMが提供する新しいキーを反映するには、特性を更新する必要があります。 このプロセスは、Audience Managerが作成した[BAAMツール](https://docs.adobe.com/content/help/en/audience-manager/user-guide/reference/bulk-management-tools/bulk-management-intro.html#getting-started-with-bulk-management)を使用すると簡単になります。

## サーバー側転送

現在、サーバー側転送が有効になっていて、`appmeasurement.js`を使用している場合。 `visitor.js`サーバ側転送機能を有効にしておけば問題は発生しません。 バックエンドで、AdobeはAAMセグメントを取得し、それらをAnalyticsの呼び出しに追加します。 Analyticsへの呼び出しにこれらのセグメントが含まれる場合、AnalyticsはAudience Managerを呼び出してデータを転送しないので、重複のデータ収集は行われません。 また、Web SDKを使用する場合は、ロケーションヒントも不要です。これは、同じセグメントエンドポイントがバックエンドで呼び出されるためです。

## 訪問者IDと地域IDの取得

一意の訪問者IDを使用する場合は、`getIdentity`コマンドを使用します。 `getIdentity` 現在の訪問者の既存のECIDを返します。ECIDをまだ持っていない初回訪問者の場合は、新しいECIDが生成されます。 `getIdentity` 訪問者の地域IDも返します。詳しくは、『[Adobe Audience Managerユーザーガイド](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-regions.html)』を参照してください。

>[!NOTE]
>
>このメソッドは、通常、[!DNL Experience Cloud] IDを読み取るか、Adobe Audience Managerの場所のヒントを必要とするカスタムソリューションで使用されます。 標準の実装では使用されません。

```javascript
alloy("getIdentity")
  .then(function(result) {
    // The command succeeded.
    console.log("ECID:", result.identity.ECID);
    console.log("RegionId:", result.edge.regionId);
  })
  .catch(function(error) {
    // The command failed.
    // "error" will be an error object with additional information.
  });
```

## IDの同期

>[!NOTE]
>
>ハッシュ機能に加えて、バージョン2.1.0では`syncIdentity`メソッドが削除されました。 バージョン2.1.0以降を使用しており、IDを同期する場合は、`identityMap`フィールドの`sendEvent`コマンドの`xdm`オプションで直接IDを送信できます。

また、[!DNL Identity Service]では、`syncIdentity`コマンドを使用して、独自の識別子をECIDと同期できます。

>[!NOTE]
>
>`sendEvent`コマンドごとに使用可能なIDをすべて渡すことを強くお勧めします。 これにより、パーソナライゼーションを含む様々な使用例がロック解除されます。 これで、`sendEvent`コマンドでこれらのIDを渡すことができるようになり、IDをDataLayerに直接配置できます。

IDを同期すると、複数のIDを使用するデバイス/ユーザーを識別し、認証状態を設定して、どのIDを主要IDと見なすかを決定できます。 識別子が`primary`として設定されていない場合、主なデフォルトは`ECID`です。

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

`identityMap`内の各プロパティは、特定の[ID名前空間](../../identity-service/namespaces.md)に属するIDを表します。 プロパティ名は、ID名前空間の記号にする必要があります。この記号は、Adobe Experience Platformのユーザーインターフェイスの「[!UICONTROL ID]」の下に表示されます。 プロパティ値は、そのID名前空間に関するIDの配列にする必要があります。

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
