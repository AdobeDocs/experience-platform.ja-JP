---
title: Platform Web SDK のファーストパーティデバイス ID
description: Adobe Experience Platform Web SDK 用のファーストパーティデバイス ID(FPID) の設定方法について説明します。
exl-id: c3b17175-8a57-43c9-b8a0-b874fecca952
source-git-commit: 3272db15283d427eb4741708dffeb8141f61d5ff
workflow-type: tm+mt
source-wordcount: '1774'
ht-degree: 2%

---

# Platform Web SDK のファーストパーティデバイス ID

Adobe Experience Platform Web SDK は、 [Adobe Experience Cloud ID(ECID)](https://experienceleague.adobe.com/docs/experience-platform/identity/ecid.html?lang=en) を web サイト訪問者に対して送信します。 cookie のライフスパンに関するブラウザーの制限を考慮するには、代わりに、独自のデバイス識別子を設定および管理するように選択できます。 これらは、ファーストパーティデバイス ID（FPID）と呼ばれます。

>[!NOTE]
>
>ファーストパーティデバイス ID のサポートは、Platform Web SDK を介して Platform Edge Network にデータを送信する場合にのみ使用できます。

このドキュメントでは、Platform Web SDK 実装でファーストパーティデバイス ID を設定する方法について説明します。

## 前提条件

このガイドは、ECID や ECID の役割など、Platform Web SDK での ID データの動作について既に詳しいことを前提としています `identityMap`. 概要については、 [Web SDK の ID データ](./overview.md) を参照してください。

## FPID の使用

FPID は、ファーストパーティ cookie を使用して訪問者を追跡します。 ファーストパーティ cookie は、DNS を利用するサーバーを使用して設定される場合に最も効果的です。 [レコード](https://datatracker.ietf.org/doc/html/rfc1035) （IPv4 の場合）または [AAA レコード](https://datatracker.ietf.org/doc/html/rfc3596) （IPv6 の場合）。DNS CNAME または JavaScript コードとは異なります。

>[!IMPORTANT]
>
>レコードまたは AAAA レコードは、Cookie の設定とトラッキングに対してのみサポートされます。 データ収集の主な方法は、DNS CNAME を使用する方法です。 つまり、FPID は A レコードまたは AAAA レコードを使用して設定され、CNAME を使用してAdobeに送信されます。
>
>The [Adobe管理証明書プログラム](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html#adobe-managed-certificate-program) は、ファーストパーティのデータ収集でも引き続きサポートされます。

FPID Cookie が設定されると、その値を取得し、イベントデータの収集時にAdobeに送信できます。 収集された FPID は、ECID を生成するシードとして使用されます。ECID は、引き続きAdobe Experience Cloudアプリケーションの主な識別子です。

Web サイト訪問者の FPID を Platform Edge Network に送信するには、 `identityMap` を選択します。 このドキュメントの後半の節を参照してください。 [での FPID の使用 `identityMap`](#identityMap) を参照してください。

### ID 形式の要件

Platform Edge ネットワークは、 [UUIDv4 形式](https://datatracker.ietf.org/doc/html/rfc4122). UUIDv4 形式でないデバイス ID は拒否されます。

UUID を生成すると、ほとんどの場合、一意のランダムな ID が生成され、衝突の発生確率は無視できます。 UUIDv4 は、IP アドレスやその他の個人情報 (PII) を使用してシードすることはできません。 UUID はあらゆる場所に存在し、ほぼすべてのプログラミング言語でライブラリを見つけて生成できます。

## 独自のサーバーを使用した cookie の設定

所有しているサーバーを使用して Cookie を設定する場合、ブラウザーポリシーによって Cookie が制限されるのを防ぐために、様々な方法を使用できます。

* サーバー側スクリプティング言語を使用して Cookie を生成する
* サイトのサブドメインまたは他のエンドポイントに対しておこなわれた API リクエストに応じて、Cookie を設定します。
* CMS を使用して Cookie を生成する
* CDN を使用して Cookie を生成する

>[!IMPORTANT]
>
>JavaScript の `document.cookie` メソッドは、cookie の期間を制限するブラウザーポリシーからほとんど保護されません。

### Cookie を設定するタイミング

FPID Cookie は、Edge ネットワークにリクエストを送信する前に設定するのが理想的です。 ただし、この方法が使用できない場合でも、ECID は既存のメソッドを使用して生成され、cookie が存在する限り、プライマリ識別子として機能します。

ECID が最終的にブラウザー削除ポリシーの影響を受けますが、FPID が影響を受けない場合、FPID は次の訪問時の主な識別子になり、後続の訪問のたびに ECID のシードに使用されます。

### cookie の有効期限の設定

Cookie の有効期限の設定は、FPID 機能を実装する際に慎重に検討する必要があります。 この決定を下す際は、組織が活動する国や地域と、それらの各地域の法律やポリシーを考慮に入れる必要があります。

この決定の一環として、会社全体の Cookie 設定ポリシーまたは、運用する各ロケールのユーザーごとに異なる Cookie 設定を採用することができます。

cookie の初回有効期限に対して選択した設定に関係なく、サイトへの新しい訪問が発生するたびに cookie の有効期限を延長するロジックを含める必要があります。

## cookie フラグの影響

異なるブラウザーでの cookie の処理方法に影響する cookie フラグには、様々なものがあります。

* [&#39;HTTPOnly&#39;](#http-only)
* [`安全`](#secure)
* [`SameSite`](#same-site)

### `HTTPOnly` {#http-only}

を使用して設定された Cookie `HTTPOnly` フラグには、クライアント側のスクリプトを使用してアクセスできません。 つまり、 `HTTPOnly` フラグを設定する場合、サーバー側のスクリプティング言語を利用して、cookie の値を読み取り、 `identityMap`.

Platform Edge Network に FPID Cookie の値を読み取らせる場合は、 `HTTPOnly` フラグを設定すると、クライアント側のスクリプトによって値にアクセスできなくなり、Platform Edge ネットワークの cookie を読み取る機能に悪影響を与えません。

>[!NOTE]
>
>の使用 `HTTPOnly` フラグは、cookie の有効期間を制限する可能性のある cookie ポリシーには影響しません。 ただし、FPID の値を設定して読み取る際には、まだ考慮すべき点です。

### `Secure` {#secure}

Cookie の設定に基づいて、 `Secure` 属性は、暗号化されたリクエストで HTTPS プロトコル経由でのみサーバーに送信されます。 このフラグを使用すると、中間者の攻撃者が容易に cookie の値にアクセスできないようにすることができます。 可能な場合は、 `Secure` フラグ。

### `SameSite` {#same-site}

The `SameSite` 属性を使用すると、サーバーは、クロスサイトリクエストで cookie を送信するかどうかを決定できます。 属性は、クロスサイトフォージェリ攻撃に対する保護を提供します。 次の 3 つの値が使用可能です。 `Strict`, `Lax` および `None`. お客様の組織に適した設定を判断するには、社内チームにお問い合わせください。

いいえの場合 `SameSite` 属性が指定されている場合、一部のブラウザーのデフォルト設定がになりました。 `SameSite=Lax`.

## での FPID の使用 `identityMap` {#identityMap}

以下に、で独自に FPID を設定する方法の例を示します。 `identityMap`:

```json
{
  "identityMap": {
    "FPID": [
      {
        "id": "123e4567-e89b-42d3-9456-426614174000",
        "authenticatedState": "ambiguous",
        "primary": true
      }
    ]
  }
}
```

他の ID タイプと同様に、内に他の ID を含めることができます `identityMap`. 認証済み CRM ID に含まれる FPID の例を次に示します。

```json
{
  "identityMap": {
    "FPID": [
      {
        "id": "123e4567-e89b-42d3-9456-426614174000",
        "authenticatedState": "ambiguous",
        "primary": true
      }
    ],
    "EMAIL": [
      {
        "id": "email@mail.com",
        "authenticatedState": "authenticated",
        "primary": true
      }
    ]
  }
}
```

ファーストパーティデータ収集が有効な場合に、Edge ネットワークが読み取る Cookie に FPID が含まれている場合は、認証済みの CRM ID のみを取り込む必要があります。

```json
{
  "identityMap": {
    "EMAIL": [
      {
        "id": "email@mail.com",
        "authenticatedState": "authenticated",
        "primary": true
      }
    ]
  }
}
```

次の `identityMap` Edge ネットワークに `primary` FPID の指標。 最後に、に存在する ID の 1 つ `identityMap` は、 `primary`.

```json
{
  "identityMap": {
    "FPID": [
      {
        "id": "123e4567-e89b-12d3-a456-426614174000",
        "authenticatedState": "ambiguous"
      }
    ],
    "EMAIL": [
      {
        "id": "email@mail.com",
        "authenticatedState": "authenticated"
      }
    ]
  }
}
```

この場合、Edge ネットワークから返されるエラー応答は次のようになります。

```json
{
    "type": "https://ns.adobe.com/aep/errors/EXEG-0306-400",
    "status": 400,
    "title": "No primary identity set in request (event)",
    "detail": "No primary identity found in the input event. Update the request accordingly to your schema and try again.",
    "report": {
        "requestId": "{REQUEST_ID}",
        "configId": "{CONFIG_ID}",
        "orgId": "{ORG_ID}"
    }
}
```

## ID 階層

ECID と FPID の両方が存在する場合、ユーザーを識別する際に ECID が優先されます。 これにより、既存の ECID がブラウザーの Cookie ストアに存在する場合でも、その ECID は引き続き主な識別子となり、既存の訪問者のカウントが影響を受けるリスクを回避できます。 既存のユーザーの場合、ECID が期限切れになるか、ブラウザーポリシーまたは手動プロセスの結果として ECID が削除されるまで、FPID はプライマリ ID になりません。

ID は次の順序で優先されます。

1. 次に含まれる ECID: `identityMap`
1. cookie に保存された ECID
1. に含まれる FPID `identityMap`
1. Cookie に保存される FPID

## ファーストパーティデバイス ID への移行

以前の実装から FPID の使用に移行する場合は、トランジションが低レベルでどのように表示されるかを視覚化するのが困難な場合があります。

このプロセスを説明するために、以前にサイトを訪問した顧客と、Adobeソリューションでの顧客の識別方法に FPID 移行が及ぼす影響を考慮します。

![FPID への移行後に訪問間で顧客の ID 値がどのように更新されるかを示す図](../assets/identity/tracking/visits.png)

| 訪問 | 説明 |
| --- | --- |
| 初回訪問 | FPID Cookie の設定をまだ開始していないとします。 ECID は [AMCV cookie](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html#section-c55af54828dc4cce89f6118655d694c8) は、訪問者を識別するために使用される識別子です。 |
| 2 回目の訪問 | ファーストパーティデバイス ID ソリューションのロールアウトが開始しました。 既存の ECID は引き続き存在し、訪問者を識別するための主な識別子になります。 |
| 3 回目の訪問 | 2 回目と 3 回目の訪問の間に、ブラウザーポリシーが原因で ECID が削除されたのに十分な時間が経過しました。 ただし、FPID は DNS A レコードを使用して設定されたので、FPID は保持されます。 この FPID はプライマリ ID と見なされ、ECID のシードに使用されます。ECID はエンドユーザーデバイスに書き込まれます。 これで、このユーザーは、Adobe Experience PlatformおよびExperience Cloudソリューションで新しい訪問者と見なされます。 |
| 4 回目の訪問 | 3 回目と 4 回目の訪問の間に、ブラウザーポリシーが原因で ECID が削除されたのに十分な時間が経過しました。 以前の訪問と同様、FPID は設定された方法が原因で残ります。 今回は、前回の訪問と同じ ECID が生成されます。 ユーザーは、Experience PlatformおよびExperience Cloudソリューション全体で、以前の訪問と同じユーザーとして表示されます。 |
| 5 回目の訪問 | 4 回目と 5 回目の訪問の間に、エンドユーザーはブラウザー内のすべての cookie をクリアしました。 新しい FPID が生成され、新しい ECID の作成をシードするために使用されます。 これで、このユーザーは、Adobe Experience PlatformおよびExperience Cloudソリューションで新しい訪問者と見なされます。 |

{style="table-layout:auto"}

## FAQ

以下は、ファーストパーティデバイス ID に関するよくある質問に対する回答のリストです。

### 単に ID を生成するのと、ID をシードするのとでは、どのように異なりますか。

シードの概念は、Adobe Experience Cloudに渡された FPID が決定論的アルゴリズムを使用して ECID に変換される点が一意です。 同じ FPID がAdobe Experience Platform Edge Network に送信されるたびに、同じ ECID が FPID からシードされます。

### ファーストパーティデバイス ID は、いつ生成する必要がありますか？

訪問者の水増しの可能性を減らすには、Platform Web SDK を使用して最初のリクエストをおこなう前に FPID を生成する必要があります。 ただし、これができない場合、ECID はそのユーザーに対して生成され、プライマリ識別子として使用されます。 生成された FPID は、ECID が存在しなくなるまで、プライマリ識別子になりません。

### ファーストパーティデバイス ID をサポートしているのは、どのデータ収集方法ですか？

現在、FPID をサポートしているのは、Platform Web SDK のみです。

### FPID は、どのプラットフォームまたはExperience Cloudソリューションにも保存されますか。

FPID を使用して ECID をシードすると、その FPID は `identityMap` とは、生成された ECID に置き換えられています。 FPID は、Adobe Experience PlatformやExperience Cloudソリューションには保存されません。
