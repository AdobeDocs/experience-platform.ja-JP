---
title: Platform Web SDK の ID データ
description: Adobe Experience Platform Web SDK を使用してAdobe Experience Cloud ID(ECID) を取得および管理する方法について説明します。
keywords: ID；ファーストパーティ ID;ID サービス；サードパーティ ID;ID の移行；訪問者 ID；サードパーティ ID;thirdPartyCookiesEnabled;idMigrationEnabled;getId；同期 ID;sendEvent;identityMap；プライマリ；ID 名前空間；id；認証状態 hashEnabled;
exl-id: 03060cdb-becc-430a-b527-60c055c2a906
source-git-commit: 85ff35e0e7f7e892de5252e8f3ad069eff83aa15
workflow-type: tm+mt
source-wordcount: '1334'
ht-degree: 2%

---

# Platform Web SDK の ID データ

Adobe Experience Platform Web SDK は、 [Adobe Experience Cloud ID(ECID)](../../identity-service/ecid.md) 訪問者の行動を追跡するには、をクリックします。 ECID を使用すると、各デバイスに複数のセッション間で保持できる一意の ID が割り当てられていることを確認でき、Web セッション中および複数の Web セッション間で発生したすべてのヒットを特定のデバイスに結び付けることができます。

このドキュメントでは、Platform Web SDK を使用して ECID を管理する方法の概要を説明します。

## SDK を使用した ECID のトラッキング

Platform Web SDK は、Cookie を使用して ECID を割り当て、追跡します。これらの Cookie の生成方法を設定するために利用可能な複数のメソッドが用意されています。

新しいユーザーが Web サイトに到達すると、Adobe Experience Cloud Identity Service は、そのユーザーに対してデバイス ID Cookie の設定を試みます。 初回訪問者の場合、ECID が生成され、Adobe Experience Platform Edge Network からの最初の応答で返されます。 リピート訪問者の場合、ECID は `kndctr_{YOUR-ORG-ID}_AdobeOrg_identity` cookie に保存され、Edge ネットワークによってペイロードに追加されました。

ECID を含む Cookie を設定した後、Web SDK によって生成される各リクエストでは、エンコードされた ECID が `kndctr_{YOUR-ORG-ID}_AdobeOrg_identity` cookie.

デバイスを識別するために Cookie を使用する場合、Edge ネットワークとやり取りする方法は 2 つあります。

1. Edge ネットワークドメインに直接データを送信 `adobedc.net`. このメソッドは、 [サードパーティデータ収集](#third-party).
1. を指す CNAME を独自のドメインで作成する `adobedc.net`. このメソッドは、 [ファーストパーティデータ収集](#first-party).

以下の節で説明するように、使用するように選択したデータ収集方法は、ブラウザー全体での cookie の有効期間に直接影響します。

### サードパーティのデータ収集 {#third-party}

サードパーティのデータ収集には、Edge ネットワークドメインへの直接のデータ送信が含まれます `adobedc.net`.

近年、Web ブラウザーは、サードパーティによって設定された Cookie の処理に対する制限を厳しくしています。 一部のブラウザーは、デフォルトでサードパーティ Cookie をブロックします。 サードパーティ cookie を使用してサイトの訪問者を識別する場合、これらの cookie の有効期間は、それ以外の場合、ファーストパーティ cookie を使用して有効期間に入るよりも常に短くなります。 場合によっては、サードパーティ Cookie の有効期限が 7 日以内に切れることがあります。

さらに、サードパーティのデータ収集が使用される場合、一部の広告ブロッカーは、トラフィックをAdobeのデータ収集エンドポイントに完全に制限します。

### ファーストパーティデータ収集 {#first-party}

ファーストパーティデータ収集では、お客様独自のドメイン上の CNAME を介してを指す cookie を設定する必要があります。 `adobedc.net`.

ブラウザーでは、サイトが所有するエンドポイントと同様に CNAME エンドポイントで設定された Cookie が長く処理されていますが、ブラウザーによって実装された最近の変更によって、CNAME Cookie の処理方法が区別されています。 現在、ファーストパーティ CNAME Cookie をデフォルトでブロックしているブラウザーはありませんが、一部のブラウザーでは、CNAME を使用して設定された Cookie の有効期間がわずか 7 日に制限されます。

### cookie の寿命がAdobe Experience Cloudアプリケーションに及ぼす影響 {#lifespans}

ファーストパーティとサードパーティのどちらのデータ収集を選択した場合でも、Cookie を保持できる期間は、Adobe AnalyticsおよびCustomer Journey Analyticsでの訪問者数に直接影響します。 また、エンドユーザーがサイトでAdobe TargetまたはOffer decisioningを使用すると、パーソナライゼーションエクスペリエンスに一貫性がなくなる場合があります。

例えば、過去 7 日間にユーザーが 3 回閲覧した項目をホームページにプロモーションするパーソナライゼーションエクスペリエンスを作成したとします。

エンドユーザーが 1 週間に 3 回訪問し、その後 7 日間サイトに戻らなかった場合、（サイトの訪問時に使用していたブラウザーによっては）ブラウザーポリシーによって cookie が削除されている可能性があるので、サイトに戻ると新規ユーザーと見なされます。 この場合、Analytics ツールは、訪問者がわずか 7 日以上前にサイトを訪問した場合でも、訪問者を新しいユーザーとして扱います。 さらに、ユーザーのエクスペリエンスをパーソナライズするための取り組みが再び開始されます。

### ファーストパーティデバイス ID

上記の cookie の寿命の影響を考慮するには、代わりに独自のデバイス識別子を設定および管理することを選択できます。 詳しくは、[ファーストパーティデバイス ID](./first-party-device-ids.md) に関するガイドを参照してください。

## 現在のユーザーの ECID および地域の取得

現在の訪問者の一意の ECID を取得するには、 `getIdentity` コマンドを使用します。 ECID を持たない初めての訪問者の場合、このコマンドは新しい ECID を生成します。 `getIdentity` また、訪問者の地域 ID も返します。

>[!NOTE]
>
>このメソッドは、通常、 [!DNL Experience Cloud] ID またはAdobe Audience Managerのロケーションヒントが必要です。 標準の実装では使用されません。

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

XDM の使用 [`identityMap` フィールド](../../xdm/schema/composition.md#identityMap)を使用すると、複数の ID を使用してデバイスやユーザーを識別し、認証状態を設定して、プライマリ ID と見なす識別子を決定できます。 識別子が設定されていない場合 `primary`の場合、プライマリは `ECID`.

`identityMap` フィールドは `sentEvent` コマンドを使用します。

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

内の各プロパティ `identityMap` は、特定のに属する ID を表します [id 名前空間](../../identity-service/namespaces.md). プロパティ名は、ID 名前空間シンボルである必要があります。このシンボルは、Adobe Experience Platformユーザーインターフェイスの「[!UICONTROL ID]&quot;. プロパティ値は、その ID 名前空間に関する ID の配列である必要があります。

ID 配列内の各 ID オブジェクトには、次のプロパティが含まれます。

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `id` | 文字列 | **（必須）** 指定された名前空間に設定する ID。 |
| `authenticationState` | 文字列 | **（必須）** ID の認証状態。 有効な値は `ambiguous`、`authenticated`、および `loggedOut` です。 |
| `primary` | ブール値 | この ID をプロファイル内のプライマリフラグメントとして使用するかどうかを決定します。 デフォルトでは、ECID はユーザーのプライマリ識別子として設定されています。 省略した場合、この値はデフォルトで `false` になります。 |

## 訪問者 API から ECID への移行

訪問者 API を使用して移行する際に、既存の AMCV Cookie を移行することもできます。 ECID の移行を有効にするには、 `idMigrationEnabled` パラメーターを設定に含める必要があります。 ID 移行により、次のような使用例が可能になります。

* ドメインの一部のページが訪問者 API を使用していて、他のページがこの SDK を使用している場合。 この場合に対応するため、SDK は既存の AMCV Cookie を読み取り、既存の ECID で新しい Cookie を書き込みます。 さらに、SDK は AMCV Cookie を書き込むので、ECID が SDK に実装されたページで最初に取得された場合に、Visitor API に実装された後続のページで同じ ECID を持つようにします。
* 訪問者 API も持つページでAdobe Experience Platform Web SDK が設定されている場合。 このような状況をサポートするために、AMCV Cookie が設定されていない場合、SDK はページ上で訪問者 API を探し、呼び出して ECID を取得します。
* サイト全体でAdobe Experience Platform Web SDK を使用していて、訪問者 API がない場合は、ECID を移行して再訪問者の情報を保持すると便利です。 で SDK をデプロイした後 `idMigrationEnabled` 訪問者の cookie のほとんどが移行されるまでの期間、設定をオフにできます。

### 移行用の特性の更新

XDM 形式のデータがAudience Managerに送信される場合、このデータは移行時にシグナルに変換する必要があります。 XDM が提供する新しいキーを反映するには、特性を更新する必要があります。 このプロセスは、 [BAAAM ツール](https://experienceleague.adobe.com/docs/audience-manager/user-guide/reference/bulk-management-tools/bulk-management-intro.html#getting-started-with-bulk-management) そのAudience Managerが作成されました。

## イベント転送で使用

現在 [イベント転送](../../tags/ui/event-forwarding/overview.md) 有効で、使用している `appmeasurement.js` および `visitor.js`を使用すると、イベント転送機能を有効にしたままにできます。この機能を有効にしても問題は発生しません。 バックエンドでは、AdobeはAAMセグメントを取得し、それらを Analytics への呼び出しに追加します。 Analytics への呼び出しにこれらのセグメントが含まれる場合、Analytics はデータを転送するAudience Managerを呼び出さないので、二重のデータ収集はおこなわれません。 また、Web SDK を使用する際にロケーションヒントは必要ありません。これは、同じセグメント化エンドポイントがバックエンドで呼び出されるからです。
