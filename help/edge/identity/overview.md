---
title: Adobe Experience Platform Web SDKを使用したExperience CloudIDの取得
description: Adobe Experience Platform Web SDKを使用してAdobe Experience Cloud ID(ECID)を取得する方法について説明します。
seo-description: Adobe Experience Cloud Idの取得方法を説明します。
keywords: ID；ファーストパーティID;IDサービス；サードパーティID;IDの移行；訪問者ID；サードパーティID;thirdPartyCookiesEnabled;idMigrationEnabled;getId；同期ID;syncEvent;identityMap；プライマリ；ID名前空間；ID；認証状態hashEnabled;
exl-id: 03060cdb-becc-430a-b527-60c055c2a906
source-git-commit: c3d66e50f647c2203fcdd5ad36ad86ed223733e3
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 3%

---

# Adobe Experience Cloud IDの取得

Adobe Experience Platform Web SDKは、[AdobeIDサービス](../../identity-service/ecid.md)を利用します。 これにより、各デバイスには、デバイス上で保持される一意の識別子が割り当てられ、ページ間のアクティビティを結び付けることができます。

## ファーストパーティID

[!DNL Identity Service]は、IDをファーストパーティドメインのcookieに保存します。 [!DNL Identity Service]は、ドメインのHTTPヘッダーを使用してCookieの設定を試みます。 失敗した場合、[!DNL Identity Service]はJavaScriptを使用したcookieの設定にフォールバックされます。 Adobeでは、クライアント側のITP制限によってCookieの上限が設定されないように、CNAMEを設定することをお勧めします。

## サードパーティID

[!DNL Identity Service]には、IDをサードパーティドメイン(demdex.net)と同期して、サイト間での追跡を有効にする機能があります。 これを有効にすると、訪問者（ECIDのない訪問者など）に対する最初のリクエストがdemdex.netに対しておこなわれます。 これは、ChromeなどのChromeを許可するブラウザーでのみ実行され、設定の`thirdPartyCookiesEnabled`パラメーターで制御されます。 この機能をすべて一緒に無効にする場合は、`thirdPartyCookiesEnabled`をfalseに設定します。

## IDの移行

訪問者APIを使用してから移行する場合は、既存のAMCV Cookieを移行することもできます。 ECID移行を有効にするには、設定で`idMigrationEnabled`パラメーターを設定します。 ID移行を使用すると、次のような場合に役立ちます。

* ドメインの一部のページが訪問者APIを使用し、他のページがこのSDKを使用している場合。 この場合をサポートするために、SDKは既存のAMCV Cookieを読み取り、既存のECIDで新しいCookieを書き込みます。 さらに、SDKは、SDKが実装されたページで最初にECIDを取得した場合、訪問者APIが実装された後続のページで同じECIDを持つように、AMCV Cookieを書き込みます。
* Adobe Experience Platform Web SDKが、訪問者APIも持つページで設定されている場合。 このケースをサポートするために、AMCV Cookieが設定されていない場合、SDKはページ上で訪問者APIを探し、呼び出してECIDを取得します。
* サイト全体でAdobe Experience Platform Web SDKを使用していて、訪問者APIがない場合は、再訪問者の情報を保持できるように、ECIDを移行すると便利です。 `idMigrationEnabled`を使用して一定期間SDKをデプロイした後、ほとんどの訪問者Cookieが移行されるように、設定をオフにできます。

## 移行用の特性の更新

XDM形式のデータがAudience Managerに送信される場合、このデータは移行時にシグナルに変換する必要があります。 XDMが提供する新しいキーを反映するには、特性を更新する必要があります。 このプロセスは、Audience Managerが作成した[BAAAMツール](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/bulk-management-tools/bulk-management-intro.html#getting-started-with-bulk-management)を使用することで容易になります。

## サーバー側転送

現在サーバー側転送が有効になっていて、`appmeasurement.js`を使用している場合。 `visitor.js`サーバー側転送機能を有効にしたままにしておくと、問題は発生しません。 バックエンドで、AdobeはAAMセグメントを取得し、Analyticsへの呼び出しに追加します。 Analyticsへの呼び出しにこれらのセグメントが含まれる場合、Analyticsは、データを転送するAudience Managerを呼び出さないので、二重のデータ収集はおこなわれません。 また、Web SDKを使用する際にロケーションヒントを使用する必要もありません。これは、同じセグメント化エンドポイントがバックエンドで呼び出されるからです。

## 訪問者IDおよび地域IDの取得

一意の訪問者IDを使用する場合は、`getIdentity`コマンドを使用します。 `getIdentity` は、現在の訪問者の既存のECIDを返します。ECIDを持たない初回訪問者の場合、このコマンドは新しいECIDを生成します。 `getIdentity` は、訪問者の地域IDも返します。詳しくは、『Adobe Audience Managerユーザーガイド[』を参照してください。](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-regions.html)

>[!NOTE]
>
>このメソッドは、通常、[!DNL Experience Cloud] IDを読み取るか、Adobe Audience Managerのロケーションヒントが必要なカスタムソリューションで使用されます。 標準の実装では使用されません。

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
>`syncIdentity`メソッドは、ハッシュ機能に加えて、バージョン2.1.0で削除されました。 バージョン2.1.0以降を使用してIDを同期する場合は、`identityMap`フィールドの`sendEvent`コマンドの`xdm`オプションで直接IDを送信できます。

さらに、[!DNL Identity Service]を使用すると、`syncIdentity`コマンドを使用して独自のIDをECIDと同期できます。

>[!NOTE]
>
>`sendEvent`コマンドのたびに、使用可能なIDをすべて渡すことを強くお勧めします。 これにより、パーソナライゼーションを含む様々な使用例のロックが解除されます。 これらのIDを`sendEvent`コマンドで渡せるようになったので、DataLayerに直接配置できます。

IDの同期では、複数のIDを使用してデバイスやユーザーを識別し、認証状態を設定して、どのIDを主要な識別子と見なすかを決定できます。 識別子が`primary`として設定されていない場合、プライマリはデフォルトで`ECID`になります。

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

`identityMap`内の各プロパティは、特定の[ID名前空間](../../identity-service/namespaces.md)に属するIDを表します。 プロパティ名は、ID名前空間のシンボルである必要があります。このシンボルは、Adobe Experience Platformのユーザーインターフェイスの「[!UICONTROL ID]」の下に表示されます。 プロパティ値は、そのID名前空間に関連するIDの配列である必要があります。

ID配列内の各IDオブジェクトは、次のように構造化されています。

### `id`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 文字列 | ○ | なし |

これは、指定した名前空間の同期するIDです。

### `authenticationState`

| **タイプ** | **必須** | **デフォルト値** | **可能な値** |
| -------- | ------------ | ----------------- | ------------------------------------ |
| 文字列 | ○ | 曖昧な | あいまい、認証済み、ログアウト済み |

IDの認証状態。

### `primary`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| Boolean | オプション | false |

このIDを、統合プロファイルでプライマリフラグメントとして使用するかどうかを指定します。 デフォルトでは、ECIDがユーザーのプライマリ識別子として設定されています。
