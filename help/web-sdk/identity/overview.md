---
title: Web SDK の ID データ
description: Adobe Experience Platform Web SDK を使用してAdobe Experience Cloud ID （ECID）を取得および管理する方法について説明します。
exl-id: 03060cdb-becc-430a-b527-60c055c2a906
source-git-commit: 6b58d72628b58b75a950892e7c16d397e3c107e2
workflow-type: tm+mt
source-wordcount: '1481'
ht-degree: 2%

---

# Web SDK の ID データ

Adobe Experience Platform Web SDK はを使用します [Adobe Experience Cloud ID （ECID）](../../identity-service/features/ecid.md) を使用して訪問者の行動を追跡します。 ECID を使用すると、各デバイスに、複数のセッションにわたって保持できる一意の ID を設定し、web セッション中およびセッション間で発生するすべてのヒットを特定のデバイスに結び付けることができます。

このドキュメントでは、Platform Web SDK を使用して ECID を管理する方法の概要を説明します。

## SDK を使用した ECID のトラッキング

Platform Web SDK は、これらの cookie の生成方法を設定できる複数の方法を使用して、cookie を使用して ECID の割り当てと追跡を行います。

新しいユーザーが web サイトにアクセスすると、Adobe Experience Cloud ID サービスは、そのユーザーのデバイス ID cookie の設定を試みます。 初回の訪問者の場合、ECID が生成され、Adobe Experience Platform Edge Networkからの最初の応答で返されます。 リピート訪問者の場合、ECID はから取得されます `kndctr_{YOUR-ORG-ID}_AdobeOrg_identity` cookie であり、Edge Networkによってペイロードに追加されました。

ECID を含む Cookie を設定すると、Web SDK で生成される後続の各リクエストには、にエンコードされた ECID が含まれます `kndctr_{YOUR-ORG-ID}_AdobeOrg_identity` cookie。

デバイスの識別に Cookie を使用する場合、Edge Networkを操作するには 2 つの選択肢があります。

1. データをEdge Networkドメインに直接送信 `adobedc.net`. このメソッドは、次のように呼ばれます [サードパーティのデータ収集](#third-party).
1. を指す CNAME を独自のドメインに作成します `adobedc.net`. このメソッドは、次のように呼ばれます [ファーストパーティデータ収集](#first-party).

以下の節で説明するように、使用するデータ収集方法は、ブラウザー間の Cookie の有効期間に直接影響します。

### サードパーティのデータ収集 {#third-party}

サードパーティのデータ収集では、データをEdge Networkドメインに直接送信します `adobedc.net`.

近年、Web ブラウザーでは、サードパーティによって設定された Cookie の処理に対する制限がますます厳しくなっています。 一部のブラウザーでは、デフォルトでサードパーティ cookie がブロックされます。 サードパーティ cookie を使用してサイト訪問者を識別する場合、それらの cookie の有効期間は、ほとんどの場合、代わりにファーストパーティ cookie を使用して使用可能な場合よりも短くなります。 場合によっては、サードパーティ cookie の有効期限が 7 日以内に切れます。

また、サードパーティのデータ収集が使用される場合、一部の広告ブロッカーは、Adobeのデータ収集エンドポイントへのトラフィックを完全に制限します。

### ファーストパーティデータ収集 {#first-party}

ファーストパーティデータ収集では、を指す独自ドメインの CNAME を使用して Cookie を設定します `adobedc.net`.

ブラウザーは長い間、サイトが所有するエンドポイントと同様に、CNAME エンドポイントで設定された Cookie を処理してきましたが、ブラウザーで実装された最近の変更により、CNAME Cookie の処理方法に区別が生まれました。 現在、デフォルトでファーストパーティ CNAME Cookie をブロックするブラウザーはありませんが、一部のブラウザーでは、CNAME を使用して設定された Cookie の有効期間がわずか 7 日に制限されています。

### Adobe Experience Cloud アプリケーションへの cookie の有効期間の影響 {#lifespans}

ファーストパーティとサードパーティのどちらのデータ収集を選択した場合でも、cookie が保持される期間は、Adobe AnalyticsとCustomer Journey Analyticsの訪問者数に直接影響します。 また、サイトでAdobe TargetまたはOffer decisioningを使用した際に、一貫性のないパーソナライゼーションエクスペリエンスがエンドユーザーに提供される可能性があります。

例えば、過去 7 日間にユーザーが 3 回表示した項目を、ホームページに昇格させるパーソナライゼーションエクスペリエンスを作成した場合を考えてみましょう。

エンドユーザーが週に 3 回訪問し、その後 7 日間サイトに戻らない場合、そのユーザーの Cookie はブラウザーポリシーによって削除された可能性があるので、サイトに戻ったときに新しいユーザーと見なされる場合があります（サイトを訪問したときに使用していたブラウザーによって異なります）。 この問題が発生した場合、Analytics ツールは、7 日前にサイトに訪問したばかりの訪問者でも、新しいユーザーとして扱います。 また、ユーザーのエクスペリエンスをパーソナライズする作業も再び開始されます。

### ファーストパーティデバイス ID

上記のように cookie の有効期間の影響を考慮して、代わりに、独自のデバイス識別子を設定および管理することを選択できます。 詳しくは、[ファーストパーティデバイス ID](./first-party-device-ids.md) に関するガイドを参照してください。

## 現在のユーザーの ECID と地域の取得 {#retrieve-ecid}

ユースケースに応じて、にアクセスする方法は 2 つあります [!DNL ECID]:

* [を取得します [!DNL ECID] データ収集のためのデータ準備の使用](#retrieve-ecid-data-prep)：推奨される方法です。
* [を取得します [!DNL ECID] 経由 `getIdentity()` コマンド](#retrieve-ecid-getidentity)：この方法は、必要なときにのみ使用してください [!DNL ECID] クライアントサイドの情報。

### を取得します [!DNL ECID] データ収集のためのデータ準備の使用 {#retrieve-ecid-data-prep}

使用方法 [データ収集のためのデータ準備](../../datastreams/data-prep.md) をマッピングします [!DNL ECID] に [!DNL XDM] フィールド。 にアクセスするには、次の方法をお勧めします。 [!DNL ECID].

それには、ソースフィールドを次のパスに設定します。

```js
xdm.identityMap.ECID[0].id
```

次に、フィールドのタイプが XDM パスにターゲットフィールドを設定します `string`.

![](../../tags/extensions/client/web-sdk/assets/access-ecid-data-prep.png)


### を取得します [!DNL ECID] 経由 `getIdentity()` コマンド {#retrieve-ecid-getidentity}


>[!IMPORTANT]
>
>以下を使用した場合のみ、ECID を取得できます `getIdentity()` コマンドが必要な場合 [!DNL ECID] クライアントサイドの場合。 ECID のみを XDM フィールドにマッピングする場合は、を使用します [データ収集のためのデータ準備](#retrieve-ecid-data-prep) その代わり。

現在の訪問者の一意の ECID を取得するには、を使用します `getIdentity` コマンド。 を持たない初めての訪問者 [!DNL ECID] ただし、このコマンドは新しい [!DNL ECID]. `getIdentity` は、訪問者の地域 ID も返します。

>[!NOTE]
>
>このメソッドは、通常、 [!DNL Experience Cloud] Adobe Audience Managerの ID または場所のヒントが必要です。 標準実装では使用されません。

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

## 使用 `identityMap`

XDM の使用 [`identityMap` フィールド](../../xdm/schema/composition.md#identityMap)では、複数の ID を使用してデバイスやユーザーを識別し、その認証状態を設定し、どの識別子をプライマリと見なすかを決定できます。 識別子がに設定されていない場合 `primary`の場合、プライマリのデフォルトはです。 `ECID`.

`identityMap` フィールドは、を使用して更新されます `sentEvent` コマンド。

```javascript
alloy("sendEvent", {
  xdm: {
    "identityMap": {
      "ID_NAMESPACE": [ // Notice how each namespace can contain multiple identifiers.
        {
          "id": "1234",
          "authenticatedState": "authenticated",
          "primary": true
        }
      ]
    }
  }
});
```

>[!NOTE]
>
>Adobeでは、次のような、人物を表す名前空間を送信することをお勧めします `CRMID`、プライマリ ID として。


内の各プロパティ `identityMap` 特定に属する ID を表します [id 名前空間](../../identity-service/features/namespaces.md). プロパティ名は、ID 名前空間シンボルである必要があります。このシンボルは、Adobe Experience Platform ユーザーインターフェイスの「」に表示されます[!UICONTROL ID]」と入力します。 プロパティ値は、その ID 名前空間に関連する ID の配列である必要があります。

>[!IMPORTANT]
>
>で渡される名前空間 ID `identityMap` は大文字と小文字を区別します。 不完全なデータ収集を避けるために、正しい名前空間 ID を使用してください。

ID 配列内の各 ID オブジェクトには、次のプロパティが含まれます。

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `id` | 文字列 | **（必須）** 指定された名前空間に設定する ID。 |
| `authenticationState` | 文字列 | **（必須）** ID の認証状態。 有効な値は `ambiguous`、`authenticated`、および `loggedOut` です。 |
| `primary` | ブール値 | この ID をプロファイル内のプライマリフラグメントとして使用する必要があるかどうかを決定します。 デフォルトでは、ECID がユーザーのプライマリ ID として設定されます。 省略した場合、この値はデフォルトで `false` になります。 |

使用， `identityMap` デバイスまたはユーザーを識別するためのフィールドは、 [`setCustomerIDs`](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html) メソッドを次から [!DNL ID Service API]. を参照してください。 [ID サービス API ドキュメント](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/get-set.html) を参照してください。

## 訪問者 API から ECID への移行

から Visitor API を使用して移行する場合、既存の AMCV Cookie も移行できます。 ECID 移行を有効にするには、を設定します `idMigrationEnabled` パラメーターが設定されます。 ID 移行により、次のユースケースが可能になります。

* ドメインの一部のページが訪問者 API を使用し、他のページがこの SDK を使用している場合。 この場合をサポートするために、SDK は既存の AMCV Cookie を読み取り、既存の ECID を使用して新しい Cookie を書き込みます。 また、SDK で実装されたページで ECID が最初に取得された場合、訪問者 API で実装された後続のページの ECID が同じになるように、SDK では AMCV Cookie を作成します。
* 訪問者 API も含むページにAdobe Experience Platform Web SDK が設定されている場合。 このケースをサポートするために、AMCV cookie が設定されていない場合、SDK はページで訪問者 API を検索し、呼び出して ECID を取得します。
* サイト全体でAdobe Experience Platform Web SDK を使用しており、Visitor API がない場合は、返された訪問者情報が保持されるように ECID を移行すると便利です。 で SDK をデプロイした後 `idMigrationEnabled` ほとんどの訪問者 cookie が移行されるしばらくの間、設定をオフにできます。

### 移行する特性の更新

XDM 形式のデータをAudience Managerに送信する場合、このデータはマイグレーション時にシグナルに変換される必要があります。 XDM が提供する新しいキーを反映するように特性を更新する必要があります。 このプロセスは、 [BAAM ツール](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/bulk-management-tools/bulk-management-intro.html#getting-started-with-bulk-management) そのAudience Managerが作成しました。

## イベント転送での使用

現在次の項目がある場合： [イベント転送](../../tags/ui/event-forwarding/overview.md) が有効で、を使用している `appmeasurement.js` および `visitor.js`を選択した場合は、イベント転送機能を有効にしておくことができ、問題は発生しません。 バックエンドでは、AdobeはAAM セグメントを取得し、それらを Analytics への呼び出しに追加します。 Analytics への呼び出しにこれらのセグメントが含まれている場合、Analytics はAudience Managerを呼び出してデータを転送しないため、重複したデータ収集はありません。 また、同じセグメント化エンドポイントがバックエンドで呼び出されるので、Web SDK を使用する際に場所のヒントは必要ありません。
