---
title: Adobe Experience Platform Web SDK を使用したExperience CloudID の取得
description: Adobe Experience Platform Web SDK を使用してAdobe Experience Cloud ID(ECID) を取得する方法について説明します。
seo-description: Learn how to get Adobe Experience Cloud Id.
keywords: ID；ファーストパーティ ID;ID サービス；サードパーティ ID;ID の移行；訪問者 ID；サードパーティ ID；サードパーティ ID;thirdPartyCookiesEnabled;idMigrationEnabled;getIdentity；同期 ID;sendEvent;IDMap；プライマリ；ID 名前空間；ID；認証状態 hashEnabled;
exl-id: 03060cdb-becc-430a-b527-60c055c2a906
source-git-commit: d753cfca6f518dfe2cafa1cb30ad26bd0b591c54
workflow-type: tm+mt
source-wordcount: '1209'
ht-degree: 6%

---

# Adobe Experience Cloud ID

Adobe Experience Platform Web SDK は、[AdobeID サービス ](../../identity-service/ecid.md) を利用します。 これにより、各デバイスに固有の識別子が保持され、ページ間のアクティビティを結び付けることができます。

## ファーストパーティ ID

[!DNL Identity Service] は、ID をファーストパーティドメインの cookie に保存します。 [!DNL Identity Service] は、ドメインの HTTP ヘッダーを使用して Cookie の設定を試みます。 失敗した場合、[!DNL Identity Service] は JavaScript での cookie の設定にフォールバックします。 [Edge ドメイン設定 ](../fundamentals/configuring-the-sdk.md#edgeConfigId) には CNAME を設定することをお勧めします。

Platform Web SDK からのすべてのヒットには、Edge ネットワーク上の ID サービスによって ECID が追加されます。 初回訪問者の場合、ECID が生成され、ペイロードに追加されます。 繰り返し訪問者の場合、ECID は `kndctr_{YOUR-ORG-ID}_AdobeOrg_identity` cookie から取得され、ペイロードに追加されます。

ECID は `xdm` の `identityMap` フィールドの下に追加されます。 ブラウザーの開発ツールを使用して、タイプがのペイロードの下で、応答の ECID を確認できます。`identity:result` ですが、リクエスト内の ECID は表示されません。

CNAME 実装を使用すると、アドビで使用される収集ドメインをカスタマイズして、独自のドメインと一致させることができます。 これにより、アドビは JavaScript を使用して、クライアントサイドではなくサーバーサイドでファーストパーティ Cookie を設定できます。 以前は、これらのサーバー側のファーストパーティ Cookie には、Safari ブラウザーのApple Intelligent Tracking Prevention(ITP) ポリシーに基づいて課された制限は適用されませんでした。 ただし、2020 年 11 月に、Appleは、CNAME を介して設定される cookie にもこれらの制限が適用されるように、ポリシーを更新しました。 現在、CNAME によってサーバー側に設定された Cookie と JavaScript によってクライアント側に設定された Cookie の両方が、ITP での 7 日間または 24 時間の有効期限に制限されています。 ITP ポリシーについて詳しくは、[ トラッキングの防止 ](https://webkit.org/tracking-prevention/#intelligent-tracking-prevention-itp) に関するこのAppleのドキュメントを参照してください。

CNAME 実装は cookie の有効期間に関してメリットはありませんが、広告ブロッカーや一般的でないブラウザーなど、データがトラッカーとして分類するドメインに送信されないというメリットがあります。 このような場合、CNAME を使用すると、これらのツールを使用するユーザーのデータ収集が中断されなくなる可能性があります。

## サードパーティ ID

[!DNL Identity Service] には、ID をサードパーティドメイン (demdex.net) と同期して、サイト間での追跡を有効にする機能があります。 これを有効にすると、訪問者（ECID のない訪問者など）に対する最初のリクエストが demdex.net に対しておこなわれます。 これは、Chrome などの Chrome を許可しているブラウザーでのみ実行され、設定の `thirdPartyCookiesEnabled` パラメーターで制御されます。 この機能をすべて一緒に無効にする場合は、`thirdPartyCookiesEnabled` を false に設定します。

## ID の移行

訪問者 API を使用してから移行する場合は、既存の AMCV Cookie を移行することもできます。 ECID 移行を有効にするには、設定で `idMigrationEnabled` パラメーターを設定します。 ID 移行を使用すると、次のような使用例が可能になります。

* ドメインの一部のページが訪問者 API を使用し、他のページがこの SDK を使用している場合。 この場合をサポートするため、SDK は既存の AMCV Cookie を読み取り、既存の ECID で新しい Cookie を書き込みます。 さらに、SDK は AMCV Cookie を書き込み、SDK が実装されたページで最初に ECID を取得した場合、訪問者 API が実装された後続のページで同じ ECID を持つようにします。
* 訪問者 API も含むページでAdobe Experience Platform Web SDK が設定されたとき。 このケースをサポートするために、AMCV Cookie が設定されていない場合、SDK はページで訪問者 API を探し、それを呼び出して ECID を取得します。
* サイト全体でAdobe Experience Platform Web SDK を使用していて、訪問者 API がない場合は、ECID を移行して、再訪問者の情報を保持すると便利です。 `idMigrationEnabled` を使用して SDK をデプロイして、ほとんどの訪問者の Cookie が移行されるまでの時間が経過すると、設定をオフにできます。

## 移行用の特性の更新

XDM 形式のデータがAudience Managerに送信される場合、このデータは移行時にシグナルに変換する必要があります。 XDM が提供する新しいキーを反映するように特性を更新する必要があります。 このプロセスは、Audience Managerが作成した [BAAAM ツール ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/bulk-management-tools/bulk-management-intro.html#getting-started-with-bulk-management) を使用することで容易になります。

## サーバー側転送

現在、サーバー側転送が有効になっていて、`appmeasurement.js` を使用している場合。 `visitor.js` サーバー側転送機能を有効にしたままにしておくと、問題は発生しません。 バックエンドで、AdobeはAAMセグメントを取得し、Analytics への呼び出しに追加します。 Analytics への呼び出しにこれらのセグメントが含まれる場合、Analytics は、データを転送するAudience Managerを呼び出さないので、二重のデータ収集はありません。 また、Web SDK を使用する際にロケーションヒントを使用する必要はありません。これは、同じセグメント化エンドポイントがバックエンドで呼び出されるからです。

## 訪問者 ID と地域 ID の取得

一意の訪問者 ID を使用する場合は、`getIdentity` コマンドを使用します。 `getIdentity` は、現在の訪問者の既存の ECID を返します。まだ ECID を持たない初回訪問者の場合、このコマンドは新しい ECID を生成します。 `getIdentity` は、訪問者の地域 ID も返します。詳しくは、『Adobe Audience Managerユーザーガイド ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/api-and-sdk-code/dcs/dcs-api-reference/dcs-regions.html?lang=ja)』を参照してください。[

>[!NOTE]
>
>このメソッドは、通常、[!DNL Experience Cloud] ID を読み取るか、Adobe Audience Managerのロケーションヒントが必要なカスタムソリューションで使用されます。 標準の実装では使用されません。

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

## ID の同期

>[!NOTE]
>
>`syncIdentity` メソッドは、ハッシュ機能に加えて、バージョン 2.1.0 で削除されました。 バージョン 2.1.0 以降を使用して ID を同期する場合は、`identityMap` フィールドの `sendEvent` コマンドの `xdm` オプションで直接 ID を送信できます。

さらに、[!DNL Identity Service] では、`syncIdentity` コマンドを使用して独自の ID を ECID と同期できます。

>[!NOTE]
>
>すべての `sendEvent` コマンドで使用可能な ID を渡すことを強くお勧めします。 これにより、パーソナライゼーションを含む様々なユースケースのロックが解除されます。 これらの ID を `sendEvent` コマンドで渡せるようになったので、DataLayer に直接配置できます。

ID の同期では、複数の ID を使用してデバイスやユーザーを識別し、認証状態を設定して、どの ID を主要識別子と見なすかを決定できます。 識別子が `primary` として設定されていない場合、プライマリはデフォルトで `ECID` になります。

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

`identityMap` 内の各プロパティは、特定の [ID 名前空間 ](../../identity-service/namespaces.md) に属する ID を表します。 プロパティ名は、ID 名前空間のシンボルである必要があります。このシンボルは、Adobe Experience Platformのユーザーインターフェイスの「[!UICONTROL ID]」の下に表示されます。 プロパティ値は、その ID 名前空間に関連する ID の配列である必要があります。

ID 配列内の各 ID オブジェクトは、次のように構造化されます。

### `id`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 文字列 | ○ | なし |

これは、特定の名前空間の同期する ID です。

### `authenticationState`

| **タイプ** | **必須** | **デフォルト値** | **可能な値** |
| -------- | ------------ | ----------------- | ------------------------------------ |
| 文字列 | ○ | 曖昧な | あいまい、認証済み、ログアウト済み |

ID の認証状態。

### `primary`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| Boolean | オプション | false |

この ID を、統合プロファイルでプライマリフラグメントとして使用するかどうかを指定します。 デフォルトでは、ECID はユーザーの主識別子として設定されています。
