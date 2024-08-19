---
title: Web SDK のファーストパーティデバイス ID
description: Adobe Experience Platform Web SDK 用にファーストパーティデバイス ID （FPID）を設定する方法について説明します。
exl-id: c3b17175-8a57-43c9-b8a0-b874fecca952
source-git-commit: b35a4316ca4ef82e545a7718f1b986f978003a0e
workflow-type: tm+mt
source-wordcount: '1990'
ht-degree: 0%

---


# Web SDK のファーストパーティデバイス ID

Adobe Experience Platform Web SDK は、ユーザーの行動を追跡するために、cookie を使用して web サイトの訪問者に [Adobe Experience Cloud ID （ECID） ](https://experienceleague.adobe.com/docs/experience-platform/identity/ecid.html?lang=ja) を割り当てます。 cookie の有効期間に関するブラウザーの制限を考慮するには、代わりに、独自のデバイス識別子を設定および管理することを選択できます。 これらは、ファーストパーティデバイス ID （FPID）と呼ばれます。

>[!NOTE]
>
>ファーストパーティデバイス ID のサポートは、Platform Web SDK を使用して Platform Edge Networkにデータを送信する場合にのみ使用できます。

>[!IMPORTANT]
>
>ファーストパーティデバイス ID は Web SDK の [ サードパーティ cookie](../../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md#identity) 機能と互換性がありません。
>ファーストパーティデバイス ID またはサードパーティ Cookie のいずれかを使用できますが、両方の機能を同時に使用することはできません。

このドキュメントでは、Platform Web SDK 実装用にファーストパーティデバイス ID を設定する方法について説明します。

## 前提条件

このガイドは、ECID や `identityMap` の役割など、Platform Web SDK の ID データの仕組みに精通していることを前提としています。 詳しくは、[Web SDK の ID データ ](./overview.md) に関する概要を参照してください。

## FPID の使用

FPID は、ファーストパーティ cookie を使用して訪問者を追跡します。 ファーストパーティ cookie は、DNS CNAME やJavaScript コードとは異なり、DNS [A レコード ](https://datatracker.ietf.org/doc/html/rfc1035) （IPv4 の場合）または [AAAA レコード ](https://datatracker.ietf.org/doc/html/rfc3596) （IPv6 の場合）を使用するサーバーを使用して設定される場合に最も効果的です。

>[!IMPORTANT]
>
>`A` または `AAAA` レコードは、cookie の設定とトラッキングでのみサポートされています。 データ収集の主な方法は、DNS CNAME を使用することです。 つまり、FPID は A レコードまたは AAAA レコードを使用して設定され、CNAME を使用してAdobeに送信されます。
>
>[Adobe管理証明書プログラム ](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html#adobe-managed-certificate-program) も、ファーストパーティのデータ収集で引き続きサポートされています。

FPID cookie を設定すると、その値を取得し、イベントデータが収集されたときにAdobeに送信できます。 収集された FPID は、ECID を生成するためのシードとして使用され、Adobe Experience Cloud アプリケーションでは引き続き主要な識別情報となります。

ある web サイトの訪問者の FPID を Platform Edge Networkに送信するには、その訪問者の `identityMap` に含める必要があります。 詳しくは、このドキュメントの後半の [`identityMap`](#identityMap) での FPID の使用」の節を参照してください。

### ID 形式の要件

Platform Edge Networkは、[UUIDv4 形式 ](https://datatracker.ietf.org/doc/html/rfc4122) に準拠する ID のみを受け入れます。 UUIDv4 形式でないデバイス ID は拒否されます。

UUID を生成すると、ほとんどの場合、一意のランダム ID が生成され、衝突が発生する確率はごくわずかです。 UUIDv4 は、IP アドレスやその他の個人を特定できる情報（PII）を使用してシードすることはできません。 UUID はユビキタスで、ライブラリはほとんどすべてのプログラミング言語で見つけて生成できます。

## データストリーム UI でのファーストパーティ ID Cookie の設定 {#setting-cookie-datastreams}

データストリーム UI で cookie 名を指定できます。この場合、[!DNL FPID] は cookie 値を読み取って ID マップに FPID を含める必要がありません。

>[!IMPORTANT]
>
>この機能を使用するには、[ ファーストパーティデータ収集 ](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html?lang=en) を有効にしておく必要があります。

データストリームの設定方法について詳しくは、[ データストリームのドキュメント ](../../datastreams/configure.md) を参照してください。

データストリームを設定する際に、「**[!UICONTROL ファーストパーティ ID Cookie]** オプションを有効にします。 この設定は、ファーストパーティデバイス ID を参照する際に、この値を [ID マップ ](#identityMap) で参照するのではなく、指定された Cookie を参照するようにEdge Networkに指示します。

Adobe Experience Cloudとの連携方法について詳しくは、[ ファーストパーティ cookie](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html?lang=ja) に関するドキュメントを参照してください。

![ ファーストパーティ ID の Cookie 設定を強調表示したデータストリーム設定を示す Platform UI 画像 ](../assets/first-party-id-datastreams.png)

この設定を有効にする場合、ID が保存されていると期待される Cookie の名前を指定する必要があります。

ファーストパーティ ID を使用する場合、サードパーティ ID の同期を実行することはできません。 サードパーティ ID 同期は、[!DNL Visitor ID] サービスと、そのサービスによって生成される `UUID` に依存します。 ファーストパーティ ID 機能を使用する場合、[!DNL Visitor ID] サービスを使用せずに ECID が生成されるので、サードパーティ ID の同期が不可能になります。

ファーストパーティ ID を使用する場合、Audience Managerのパートナー ID の同期が主に `UUIDs` または `DIDs` に基づいていることを考慮すると、パートナープラットフォームでのアクティベーションをターゲットとしたAudience Manager機能はサポートされません。 ファーストパーティ ID から派生する ECID は `UUID` にリンクされていないので、アドレス指定できません。

## 独自のサーバーを使用した cookie の設定

所有しているサーバーを使用して cookie を設定する場合、様々な方法を使用して、ブラウザーポリシーによる cookie の制限を防ぐことができます。

* サーバーサイドスクリプティング言語を使用した cookie の生成
* サイトのサブドメインまたは他のエンドポイントに対する API リクエストに応答して cookie を設定します
* CMSを使用した cookie の生成
* CDN を使用した cookie の生成

>[!IMPORTANT]
>
>JavaScriptの `document.cookie` メソッドを使用して設定された Cookie は、cookie の期間を制限するブラウザーポリシーからほとんど保護されなくなります。

### cookie を設定するタイミング

FPID cookie は、Edge Networkにリクエストを送信する前に設定するのが理想的です。 ただし、それが不可能なシナリオでは、引き続き既存のメソッドを使用して ECID が生成され、Cookie が存在する限り、プライマリ ID として機能します。

最終的に ECID がブラウザー削除ポリシーの影響を受けたが、FPID がそうでない場合、FPID は次の訪問でプライマリ識別子になり、以降の各訪問で ECID のシードに使用されます。

### cookie の有効期限の設定

Cookie の有効期限の設定は、FPID 機能を実装する際に慎重に考慮する必要があります。 これを決定する際には、組織が事業を行う国や地域と、それぞれの地域の法律や政策を考慮する必要があります。

この決定の一環として、会社全体の Cookie 設定ポリシーを採用するか、事業を行うロケールごとにユーザーに応じて異なるポリシーを採用する場合があります。

Cookie の初回有効期限に選択した設定に関係なく、サイトへの新しい訪問が発生するたびに cookie の有効期限を拡張するロジックを含める必要があります。

## cookie フラグの影響

様々なブラウザー間での cookie の処理方法に影響する様々な cookie フラグがあります。

* [&#39;HTTPOnly&#39;](#http-only)
* [&#39;セキュア&#39;](#secure)
* [&#39;SameSite&#39;](#same-site)

### `HTTPOnly` {#http-only}

`HTTPOnly` フラグを使用して設定された Cookie には、クライアントサイドスクリプトを使用してアクセスできません。 つまり、FPID の設定時に `HTTPOnly` フラグを設定する場合、`identityMap` に含める cookie の値を読み取るために、サーバーサイドのスクリプト言語を使用する必要があります。

Platform Edge Networkに FPID cookie の値を読み取らせることを選択した場合、`HTTPOnly` フラグを設定すると、クライアントサイドスクリプトはこの値にアクセスできなくなりますが、Platform Edge Networkの Cookie 読み取り機能には悪影響はありません。

>[!NOTE]
>
>`HTTPOnly` フラグを使用しても、cookie の有効期間を制限する可能性のある cookie ポリシーには影響しません。 ただし、FPID の値を設定および読み取る際には、この点を考慮する必要があります。

### `Secure` {#secure}

`Secure` 属性で設定された Cookie は、HTTPS プロトコル経由で暗号化されたリクエストと共にのみサーバーに送信されます。 このフラグを使用すると、MAN-IN-THE-MIDDLE 攻撃者が cookie の値に容易にアクセスできないようにすることができます。 可能であれば、常に `Secure` フラグを設定することをお勧めします。

### `SameSite` {#same-site}

`SameSite` 属性を使用すると、サーバーはクロスサイトリクエストで cookie が送信されるかどうかを決定できます。 この属性は、クロスサイト偽造攻撃に対する保護を提供します。 `Strict`、`Lax`、`None` の 3 つの値が存在する可能性があります。 社内チームに問い合わせて、組織に適した設定を決定してください。

`SameSite` 属性が指定されていない場合、一部のブラウザーのデフォルト設定は `SameSite=Lax` になりました。

## `identityMap` での FPID の使用 {#identityMap}

`identityMap` での FPID の設定方法の例を次に示します。

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

他の ID タイプと同様に、`identityMap` 内の他の ID を使用して FPID を含めることができます。 認証済み CRM ID に含まれる FPID の例を次に示します。

```json
{
  "identityMap": {
    "FPID": [
      {
        "id": "123e4567-e89b-42d3-9456-426614174000",
        "authenticatedState": "ambiguous",
        "primary": false
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

ファーストパーティデータ収集が有効になっているときにEdge Networkによって読み取られる Cookie に FPID が含まれている場合は、認証済みの CRM ID のみを取り込む必要があります。

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

次の `identityMap` では、FPID の `primary` インジケーターがないため、Edge Networkからのエラー応答が返ります。 最後に、`identityMap` に存在する ID のうち 1 つを `primary` としてマークする必要があります。

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

この場合、Edge Networkから返されるエラー応答は次のようになります。

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

ECID と FPID の両方が存在する場合は、ユーザーを識別する際に ECID が優先されます。 これにより、既存の ECID がブラウザーの cookie ストアに存在する場合、その ECID がプライマリ識別子として維持され、既存の訪問者数に影響が及ぶリスクがなくなります。 既存のユーザーの場合、ブラウザーポリシーまたは手動のプロセスの結果として ECID が期限切れになるか削除されるまで、FPID はプライマリ ID になりません。

ID の優先順位は次の順序で設定されます。

1. `identityMap` に含まれる ECID
1. cookie に格納された ECID
1. `identityMap` に含まれる FPID
1. cookie に格納された FPID

## ファーストパーティデバイス ID への移行

以前の実装から FPID を使用してに移行する場合は、トランジションが低いレベルでどのように表示されるかを視覚化するのが難しい場合があります。

このプロセスを説明するために、以前にサイトを訪問した顧客が関与するシナリオや、FPID 移行がAdobeソリューションでその顧客を特定する方法に与える影響を考えてみましょう。

![FPID への移行後、訪問間に顧客の ID 値が更新される方法を示す図 ](../assets/identity/tracking/visits.png)

>[!IMPORTANT]
>
>`ECID` cookie は常に `FPID` よりも優先されます。

| 訪問 | 説明 |
| --- | --- |
| 初回訪問 | FPID cookie の設定をまだ開始していないとします。 [AMCV cookie](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html#section-c55af54828dc4cce89f6118655d694c8) に含まれる ECID は、訪問者の識別に使用される識別子になります。 |
| 2 回目の訪問 | ファーストパーティデバイス ID ソリューションのロールアウトが開始されました。 既存の ECID は引き続き存在し、訪問者を識別するためのプライマリ識別子として残ります。 |
| 3 回目 | 2 回目と 3 回目の訪問の間に、ブラウザーポリシーが原因で ECID が削除されるまでに十分な時間が経過しました。 ただし、FPID が DNS A レコードを使用して設定されているので、FPID は保持されます。 FPID がプライマリ ID と見なされ、エンドユーザーデバイスに書き込まれる ECID のシードに使用されるようになりました。 これで、このユーザーは、Adobe Experience PlatformおよびExperience Cloudソリューションの新規訪問者と見なされます。 |
| 4 回目の訪問 | 3 回目と 4 回目の訪問の間に、ブラウザーポリシーが原因で ECID が削除されるまでに十分な時間が経過しました。 前回の訪問と同様に、FPID は、設定方法が原因で残ります。 今回は、前回の訪問と同じ ECID が生成されます。 ユーザーは、Experience PlatformおよびExperience Cloudのソリューション全体で、前回の訪問と同じユーザーとして表示されます。 |
| 5 回目の訪問 | 4 回目と 5 回目の訪問の間に、エンドユーザーはブラウザー内のすべての Cookie をクリアしました。 新しい FPID が生成され、それを使用して新しい ECID の作成がシードされます。 これで、このユーザーは、Adobe Experience PlatformおよびExperience Cloudソリューションの新規訪問者と見なされます。 |

{style="table-layout:auto"}

## FAQ

以下は、ファーストパーティデバイス ID に関するよくある質問への回答のリストです。

### 単に ID を生成することと ID のシード処理はどのように異なりますか？

シーディングの概念は、Adobe Experience Cloudに渡された FPID が決定論的アルゴリズムを使用して ECID に変換されるという点でユニークです。 同じ FPID がAdobe Experience Platform Edge Networkに送信されるたびに、同じ ECID が FPID からシードされます。

### ファーストパーティデバイス ID はいつ生成されますか？

潜在的な訪問者の膨張を減らすには、Web SDK を使用して最初のリクエストを行う前に FPID を生成する必要があります。 ただし、これを行えない場合でも、そのユーザーの ECID は生成され、プライマリ ID として使用されます。 生成された FPID は、ECID が存在しなくなるまで、プライマリ識別子になりません。

### ファーストパーティデバイス ID をサポートしているデータ収集方法はどれですか？

現在は、Web SDK のみが FPID をサポートしています。

### FPID はプラットフォームやExperience Cloudのソリューションに保存されていますか？

FPID を使用して ECID をシードすると、`identityMap` からドロップされ、生成された ECID に置き換えられます。 FPID は、Adobe Experience PlatformまたはExperience Cloudのソリューションには保存されません。
