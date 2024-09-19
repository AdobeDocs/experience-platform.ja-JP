---
title: Web SDK の ID データ
description: Adobe Experience Platform Web SDK を使用してAdobe Experience Cloud ID （ECID）を取得および管理する方法について説明します。
exl-id: 03060cdb-becc-430a-b527-60c055c2a906
source-git-commit: c99831cf2bb1b862d65851701b38c6d3dfe99000
workflow-type: tm+mt
source-wordcount: '1554'
ht-degree: 1%

---


# Web SDK の ID データ

Adobe Experience Platform Web SDK は、[Adobe Experience Cloud ID （ECID） ](../../identity-service/features/ecid.md) を使用して訪問者の行動を追跡します。 [!DNL ECIDs] を使用すると、各デバイスに一意の ID を設定し、複数のセッションにわたって保持し、web セッション中およびセッション間で発生するすべてのヒットを特定のデバイスに結び付けることができます。

このドキュメントでは、Web SDK を使用して [!DNL ECIDs] と [!DNL CORE IDs] を管理する方法の概要を説明します。

## Web SDK を使用した ECID のトラッキング {#tracking-ecids-web-sdk}

Web SDK は、これらの cookie の生成方法を設定できる複数の方法を使用して、cookie を使用して [!DNL ECIDs] ータを割り当て、追跡します。

Web サイトに新しいユーザーが到達すると、[Adobe Experience Cloud ID サービスは ](../../identity-service/home.md) そのユーザーのデバイス ID Cookie の設定を試みます。

* 初回の訪問者の場合、[!DNL ECID] が生成され、Experience PlatformEdge Networkからの最初の応答で返されます。
* 再訪問者の場合、[!DNL ECID] は `kndctr_{YOUR-ORG-ID}_AdobeOrg_identity` cookie から取得され、Edge Networkによってリクエストペイロードに追加されます。

[!DNL ECID] を含む cookie が設定されると、Web SDK によって生成される後続の各リクエストでは、`kndctr_{YOUR-ORG-ID}_AdobeOrg_identity` の cookie にエンコードされた [!DNL ECID] が含まれます。

デバイスの識別に Cookie を使用する場合、Edge Networkを操作するには次の 2 つの方法があります。

1. `adobedc.net` を指す CNAME を独自のドメインに作成します。 このメソッドは、[ ファーストパーティデータ収集 ](#first-party) と呼ばれます。
1. データをEdge Networkドメイン `adobedc.net` に直接送信します。 この方法は、[ サードパーティのデータ収集 ](#third-party) と呼ばれます。

以下の節で説明するように、使用するデータ収集方法は、ブラウザー間の Cookie の有効期間に直接影響します。

## Web SDK を使用したコア ID のトラッキング {#tracking-coreid-web-sdk}

サードパーティ cookie を有効にしたGoogle Chromeを使用し、`kndctr_{YOUR-ORG-ID}_AdobeOrg_identity` の cookie が設定されていない場合、最初のEdge Networkリクエストは `demdex.net` ドメインを通過し、demdex cookie が設定されます。 この cookie には [!DNL CORE ID] が含まれています。 これは一意のユーザー ID で、[!DNL ECID] とは異なります。

実装によっては、[ にアクセス  [!DNL CORE ID]](#retrieve-coreid) する必要があります。

### ファーストパーティデータ収集 {#first-party}

ファーストパーティデータ収集では、`adobedc.net` を指す独自のドメイン上の `CNAME` を使用して Cookie を設定します。

ブラウザーは、サイトが所有するエンドポイントで設定された cookie と同様の方法で、`CNAME` エンドポイントで設定された cookie を長い間扱ってきましたが、ブラウザーが実装された最近の変更により、`CNAME` の cookie の処理方法が区別されるようになりました。 現在、デフォルトでファーストパーティ `CNAME` cookie をブロックするブラウザーはありませんが、一部のブラウザーでは、`CNAME` を使用して設定された cookie の有効期間が 7 日間に制限されています。

### サードパーティのデータ収集 {#third-party}

サードパーティのデータ収集では、データをEdge Networkドメイン `adobedc.net` に直接送信します。

近年、Web ブラウザーでは、サードパーティによって設定された Cookie の処理に対する制限がますます厳しくなっています。 一部のブラウザーでは、デフォルトでサードパーティ cookie がブロックされます。 サードパーティ cookie を使用してサイト訪問者を識別する場合、それらの cookie の有効期間は、ほとんどの場合、代わりにファーストパーティ cookie を使用して使用可能な場合よりも短くなります。 場合によっては、サードパーティ cookie の有効期限が 7 日以内に切れます。

また、サードパーティのデータ収集を使用する場合、一部の広告ブロッカーは、Adobeのデータ収集エンドポイントへのトラフィックを完全に制限します。

### Adobe Experience Cloud アプリケーションへの cookie の有効期間の影響 {#lifespans}

ファーストパーティとサードパーティのどちらのデータ収集を選択したかに関係なく、cookie が保持できる期間は、[Adobe Analytics](https://experienceleague.adobe.com/ja/docs/analytics) および [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/customer-journey-analytics) の訪問者数に直接影響します。 また、サイトで [Adobe Target](https://experienceleague.adobe.com/en/docs/target) または [Offer decisioning} を使用すると、エンドユーザーのパーソナライゼーションエクスペリエンスに一貫性がなくな ](https://experienceleague.adobe.com/en/docs/target/using/integrate/ajo/offer-decision) 場合があります。

例えば、過去 7 日間にユーザーが 3 回表示した項目を、ホームページに昇格させるパーソナライゼーションエクスペリエンスを作成した場合を考えてみましょう。

エンドユーザーが週に 3 回訪問し、その後 7 日間サイトに戻らない場合、そのユーザーの Cookie はブラウザーポリシーによって削除された可能性があるので、サイトに戻ったときに新しいユーザーと見なされる場合があります（サイトを訪問したときに使用していたブラウザーによって異なります）。 このような状況が発生した場合、Analytics ツールは、7 日以上前にサイトに訪問した訪問者であっても、その訪問者を新しいユーザーとして扱います。 また、ユーザーのエクスペリエンスをパーソナライズする作業も再び開始されます。

### ファーストパーティデバイス ID （FPID） {#fpid}

上記のように cookie の有効期間の影響を考慮して、代わりに、独自のデバイス識別子を設定および管理することを選択できます。 詳しくは、[ファーストパーティデバイス ID](./first-party-device-ids.md) に関するガイドを参照してください。

## 現在のユーザーの ECID と地域の取得 {#retrieve-ecid}

ユースケースに応じて、[!DNL ECID] にアクセスする方法は 2 つあります。

* [ データ収集用の  [!DNL ECID]  ルーデータ準備の取得 ](#retrieve-ecid-data-prep)：これは、使用する推奨の方法です。
* [`getIdentity()` コマンドを使用して  [!DNL ECID]  を取得 ](#retrieve-ecid-getidentity)：このメソッドは、クライアントサイドで [!DNL ECID] 情報が必要な場合にのみ使用します。

### データ収集のためのデータ準備を使用した [!DNL ECID] ータの取得 {#retrieve-ecid-data-prep}

[ データ収集のためのデータ準備 ](../../datastreams/data-prep.md) を使用して、[!DNL ECID] を [!DNL XDM] フィールドにマッピングします。 [!DNL ECID] にアクセスする場合は、この方法をお勧めします。

それには、ソースフィールドを次のパスに設定します。

```js
xdm.identityMap.ECID[0].id
```

次に、ターゲットフィールドを XDM パスに設定します。フィールドのタイプは `string` です。

![](../../tags/extensions/client/web-sdk/assets/access-ecid-data-prep.png)


### `getIdentity()` コマンドを使用して [!DNL ECID] を取得します {#retrieve-ecid-getidentity}

>[!IMPORTANT]
>
>クライアント側で ECID が必要な場合は、`getIdentity()` コマンドを使用してのみ [!DNL ECID] を取得する必要があります。 ECID のみを XDM フィールドにマッピングする場合は、代わりに [ データ収集のためのデータ準備 ](#retrieve-ecid-data-prep) を使用します。

現在の訪問者の一意の ECID を取得するには、`getIdentity` コマンドを使用します。 [!DNL ECID] ールをまだもっていない初めての訪問者の場合、このコマンドは新しい [!DNL ECID] を生成します。 `getIdentity` た、訪問者の地域 ID も返します。

>[!NOTE]
>
>この手法は、通常、[!DNL Experience Cloud] ID を読み取る必要がある、またはAdobe Audience Managerの場所のヒントが必要なカスタムソリューションで使用されます。 標準実装では使用されません。

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

## 現在のユーザーのコア ID を取得します {#retrieve-coreid}

ユーザーのコア ID を取得するには、以下に示すように、[`getIdentity()`](../commands/getidentity.md) コマンドを使用できます。

```js
alloy("getIdentity",{
  "namespaces": ["CORE"]
});
```


## 使用 `identityMap` {#using-identitymap}

XDM [`identityMap` フィールドを使用すると ](../../xdm/schema/composition.md#identityMap) 複数の ID を使用してデバイスやユーザーを識別し、認証状態を設定し、どの識別子をプライマリと見なすかを決定できます。 識別子を `primary` に設定していない場合、プライマリのデフォルト値は `ECID` になります。

`identityMap` フィールドは、`sentEvent` コマンドを使用して更新されます。

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
>Adobeでは、`CRMID` などの人物を表す名前空間をプライマリ ID として送信することをお勧めします。


`identityMap` 内の各プロパティは、特定 [ID 名前空間 ](../../identity-service/features/namespaces.md) に属する ID を表します。 プロパティ名は、ID 名前空間の記号である必要があります。この記号は、Adobe Experience Platform ユーザーインターフェイスの「[!UICONTROL ID]」の下に表示されます。 プロパティ値は、その ID 名前空間に関連する ID の配列である必要があります。

>[!IMPORTANT]
>
>`identityMap` で渡される名前空間 ID は、大文字と小文字が区別されます。 不完全なデータ収集を避けるために、正しい名前空間 ID を使用してください。

ID 配列内の各 ID オブジェクトには、次のプロパティが含まれます。

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `id` | 文字列 | **（必須）** 特定の名前空間に設定する ID。 |
| `authenticatedState` | 文字列 | **（必須）** ID の認証状態。 有効な値は `ambiguous`、`authenticated`、および `loggedOut` です。 |
| `primary` | ブール値 | この ID をプロファイル内のプライマリフラグメントとして使用する必要があるかどうかを決定します。 デフォルトでは、ECID がユーザーのプライマリ ID として設定されます。 省略した場合、この値はデフォルトで `false` になります。 |

`identityMap` フィールドを使用してデバイスまたはユーザーを識別すると、[!DNL ID Service API] から [`setCustomerIDs`](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html) メソッドを使用した場合と同じ結果が得られます。 詳しくは、[ID サービス API ドキュメント ](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/get-set.html) を参照してください。

## 訪問者 API から ECID への移行 {#migrating-visitor-api-ecid}

から Visitor API を使用して移行する場合、既存の AMCV Cookie も移行できます。 ECID 移行を有効にするには、設定で `idMigrationEnabled` パラメーターを設定します。 ID 移行により、次のユースケースが可能になります。

* ドメインの一部のページが訪問者 API を使用し、他のページがこの SDK を使用している場合。 この場合をサポートするために、SDK は既存の AMCV Cookie を読み取り、既存の ECID を使用して新しい Cookie を書き込みます。 また、SDK で実装されたページで ECID が最初に取得された場合、訪問者 API で実装された後続のページの ECID が同じになるように、SDK では AMCV Cookie を作成します。
* 訪問者 API も含むページにAdobe Experience Platform Web SDK が設定されている場合。 このケースをサポートするために、AMCV cookie が設定されていない場合、SDK はページで訪問者 API を検索し、呼び出して ECID を取得します。
* サイト全体でAdobe Experience Platform Web SDK を使用しており、Visitor API がない場合は、返された訪問者情報が保持されるように ECID を移行すると便利です。 SDK を `idMigrationEnabled` と共にしばらくデプロイし、訪問者 Cookie のほとんどを移行した後で、設定をオフにできます。

### 移行する特性の更新

XDM 形式のデータをAudience Managerに送信する場合、このデータはマイグレーション時にシグナルに変換される必要があります。 XDM が提供する新しいキーを反映するように特性を更新する必要があります。 このプロセスは、Audience Managerが作成した [BAAAM ツール ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/bulk-management-tools/bulk-management-intro.html#getting-started-with-bulk-management) を使用すると容易になります。

## イベント転送での使用

現在 [ イベント転送 ](../../tags/ui/event-forwarding/overview.md) を有効にしており、`appmeasurement.js` と `visitor.js` を使用している場合は、イベント転送機能を有効にしておくことができ、問題は発生しません。 バックエンドでは、AdobeはAAM セグメントを取得し、それらを Analytics への呼び出しに追加します。 Analytics への呼び出しにこれらのセグメントが含まれている場合、Analytics はAudience Managerを呼び出してデータを転送しないため、重複したデータ収集はありません。 また、同じセグメント化エンドポイントがバックエンドで呼び出されるので、Web SDK を使用する際に場所のヒントは必要ありません。
