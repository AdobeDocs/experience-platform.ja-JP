---
title: Web SDK のファーストパーティデバイス ID
description: Adobe Experience Platform Web SDK でファーストパーティデバイス ID （FPID）を設定する方法について説明します。
exl-id: c3b17175-8a57-43c9-b8a0-b874fecca952
source-git-commit: 1cb38e3eaa83f2ad0e7dffef185d5edaf5e6c38c
workflow-type: tm+mt
source-wordcount: '1904'
ht-degree: 0%

---


# Web SDK のファーストパーティデバイス ID

Adobe Experience Platform Web SDK は、ユーザーの行動を追跡するために、cookie を使用して web サイトの訪問者に [Adobe Experience Cloud ID （ECID） ](https://experienceleague.adobe.com/docs/experience-platform/identity/ecid.html?lang=ja) を割り当てます。 cookie の有効期間に関するブラウザーの制限を考慮するには、代わりに、独自のデバイス識別子を設定および管理することを選択できます。 これらは、ファーストパーティデバイス ID （`FPIDs`）と呼ばれます。

>[!NOTE]
>
>ファーストパーティデバイス ID のサポートは、Web SDK を使用してExperience PlatformEdge Networkにデータを送信する場合にのみ使用できます。

>[!IMPORTANT]
>
>ファーストパーティデバイス ID は Web SDK の [ サードパーティ cookie](../../tags/extensions/client/web-sdk/web-sdk-extension-configuration.md#identity) 機能と互換性がありません。
>ファーストパーティデバイス ID またはサードパーティ Cookie のいずれかを使用できますが、両方の機能を同時に使用することはできません。

このドキュメントでは、Web SDK 実装用にファーストパーティデバイス ID を設定する方法について説明します。

## 前提条件

このガイドは、ECID や `identityMap` の役割など、Platform Web SDK の ID データの仕組みに精通していることを前提としています。 詳しくは、[Web SDK の ID データ ](./overview.md) に関する概要を参照してください。

## ファーストパーティデバイス ID （FPID）の使用 {#using-fpid}

ファーストパーティデバイス ID （[!DNL FPIDs]）は、ファーストパーティ cookie を使用して訪問者を追跡します。 ファーストパーティ cookie は、DNS [!DNL CNAME] または [!DNL JavaScript] コードとは異なり、DNS [A レコード ](https://datatracker.ietf.org/doc/html/rfc1035) （IPv4 の場合）または [AAAA レコード ](https://datatracker.ietf.org/doc/html/rfc3596) （IPv6 の場合）を使用するサーバーを使用して設定する場合に最も効果的です。

>[!IMPORTANT]
>
>[!DNL A] または [!DNL AAAA] レコードは、cookie の設定とトラッキングでのみサポートされています。 データ収集の主な方法は、[!DNL DNS] [!DNL CNAME] を使用することです。 つまり、[!DNL A] レコード [!DNL FPIDs] たは [!DNL AAAA] レコードを使用して設定され、[!DNL CNAME] を使用してAdobeに送信されます。
>
>[Adobe管理証明書プログラム ](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html#adobe-managed-certificate-program) も、ファーストパーティのデータ収集で引き続きサポートされています。

[!DNL FPID] cookie が設定されると、その値を取得し、イベントデータが収集されたときにAdobeに送信できます。 収集された [!DNL FPIDs] は、[!DNL ECIDs] を生成するためのシードとして使用され、Adobe Experience Cloud アプリケーションでは引き続き主要な識別情報となります。

ある web サイトの訪問者の [!DNL FPID] をEdge Networkに送信するには、その訪問者の `identityMap` に [!DNL FPID] を含める必要があります。 詳しくは、このドキュメントの下部にある [`identityMap`](#identityMap) での FPID の使用」の節を参照してください。

### ファーストパーティデバイス ID のフォーマット要件 {#formatting-requirements}

Edge Networkは、[UUIDv4 形式 ](https://datatracker.ietf.org/doc/html/rfc4122) に準拠する [!DNL IDs] ールのみを受け付けます。 [!DNL UUIDv4] 形式でないデバイス ID は拒否されます。

[!DNL UUID] の生成は、ほとんどの場合、一意のランダム ID となり、衝突が発生する確率はごくわずかです。 IP アドレスやその他の個人を特定できる情報（[!DNL PII]）を使用して [!DNL UUIDv4] ーザーをシードすることはできません。 ライブラリ [!DNL UUIDs] どこにでも存在し、事実上すべてのプログラミング言語に対応したライブラリが用意されています。

## データストリーム UI でのファーストパーティ ID Cookie の設定 {#setting-cookie-datastreams}

Cookie の値を読み取って ID マップに含めるのではなく、データストリームのユーザーインターフェイスで [!DNL FPID] を格納できる [!DNL FPID] で、cookie 名を指定できます。

>[!IMPORTANT]
>
>この機能を使用するには、[ ファーストパーティデータ収集 ](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html?lang=en) を有効にしておく必要があります。

データストリームの設定方法について詳しくは、[ データストリームのドキュメント ](../../datastreams/configure.md) を参照してください。

データストリームを設定する際に、「**[!UICONTROL ファーストパーティ ID Cookie]** オプションを有効にします。 この設定は、ファーストパーティデバイス ID を参照する際に、この値を [ID マップ ](#identityMap) で参照するのではなく、指定された Cookie を参照するようにEdge Networkに指示します。

Adobe Experience Cloudとの連携方法について詳しくは、[ ファーストパーティ cookie](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html?lang=ja) に関するドキュメントを参照してください。

![ ファーストパーティ ID の Cookie 設定を強調表示したデータストリーム設定を示す Platform UI 画像 ](../assets/first-party-id-datastreams.png)

この設定を有効にする場合、ID が保存されていると期待される Cookie の名前を指定する必要があります。

ファーストパーティ ID を使用する場合、サードパーティ ID の同期を実行することはできません。 サードパーティの ID 同期は、[!DNL Visitor ID] サービスと、そのサービスによって生成される `UUID` に依存します。 ファーストパーティ ID 機能を使用する場合、[!DNL Visitor ID] サービスを使用せずに [!DNL ECID] が生成されるので、サードパーティ ID の同期が不可能になります。

ファーストパーティ ID を使用する場合、Audience Managerパートナー ID の同期が主に `UUIDs` または `DIDs` に基づいていることを考慮すると ](https://experienceleague.adobe.com/en/docs/audience-manager)[ Audience Manager} パートナープラットフォームでのアクティベーションをターゲットにした機能はサポートされません。 ファーストパーティ ID から派生する [!DNL ECID] は、`UUID` にリンクされていないので、アドレス指定できません。

## 独自のサーバーを使用した cookie の設定 {#set-cookie-server}

所有しているサーバーを使用して cookie を設定する場合、様々な方法を使用して、ブラウザーポリシーによる cookie の制限を防ぐことができます。

* サーバーサイドスクリプティング言語を使用した cookie の生成
* サイトのサブドメインまたは他のエンドポイントに対する API リクエストに応答して cookie を設定します
* [!DNL CMS] を使用した cookie の生成
* [!DNL CDN] を使用した cookie の生成

>[!IMPORTANT]
>
>JavaScriptの `document.cookie` メソッドを使用して設定された Cookie は、cookie の期間を制限するブラウザーポリシーからほとんど保護されなくなります。

### cookie を設定するタイミング {#when-to-set-cookie}

Edge Networkにリクエストを送信する前に、[!DNL FPID] Cookie を設定するのが理想的です。 ただし、それが不可能なシナリオでは、引き続き既存のメソッドを使用して [!DNL ECID] が生成され、cookie が存在する限り、プライマリ識別子として機能します。

最終的に [!DNL ECID] がブラウザー削除ポリシーの影響を受けたが、[!DNL FPID] がそうでない場合、[!DNL FPID] は次の訪問でプライマリ識別子となり、以降の各訪問で [!DNL ECID] をシードするために使用されます。

### cookie の有効期限の設定 {#set-expiration}

cookie の有効期限の設定は、[!DNL FPID] 機能を実装する際に慎重に考慮する必要があります。 これを決定する際には、組織が事業を行う国や地域と、それぞれの地域の法律や政策を考慮する必要があります。

この決定の一環として、会社全体の Cookie 設定ポリシーを採用するか、事業を行うロケールごとにユーザーに応じて異なるポリシーを採用する場合があります。

Cookie の初回有効期限に選択した設定に関係なく、サイトへの新しい訪問が発生するたびに cookie の有効期限を拡張するロジックを含める必要があります。

## cookie フラグの影響 {#cookie-flag-impact}

様々なブラウザー間での cookie の処理方法に影響する様々な cookie フラグがあります。

* [&#39;HTTPOnly&#39;](#http-only)
* [&#39;セキュア&#39;](#secure)
* [&#39;SameSite&#39;](#same-site)

### `HTTPOnly` {#http-only}

`HTTPOnly` フラグを使用して設定された Cookie には、クライアントサイドスクリプトを使用してアクセスできません。 つまり、[!DNL FPID] を設定する際に `HTTPOnly` フラグを設定する場合、`identityMap` に含める cookie の値を読み取るために、サーバーサイドのスクリプト言語を使用する必要があります。

Edge Networkに [!DNL FPID] cookie の値を読み取らせることを選択した場合、`HTTPOnly` フラグを設定することで、クライアントサイドスクリプトから値にアクセスできなくなりますが、Edge Networkの cookie 読み取り機能には悪影響を与えません。

>[!NOTE]
>
>`HTTPOnly` フラグを使用しても、cookie の有効期間を制限する可能性のある cookie ポリシーには影響しません。 ただし、[!DNL FPID] の値を設定して読み取る際には、引き続き考慮する必要があります。

### `Secure` {#secure}

`Secure` 属性で設定された cookie は、[!DNL HTTPS] プロトコルで暗号化されたリクエストと共にのみサーバーに送信されます。 このフラグを使用すると、MAN-IN-THE-MIDDLE 攻撃者が cookie の値に容易にアクセスできないようにすることができます。 可能であれば、常に `Secure` フラグを設定することをお勧めします。

### `SameSite` {#same-site}

`SameSite` 属性を使用すると、サーバーはクロスサイトリクエストで cookie が送信されるかどうかを決定できます。 この属性は、クロスサイト偽造攻撃に対する保護を提供します。 `Strict`、`Lax`、`None` の 3 つの値が存在する可能性があります。 社内チームに問い合わせて、組織に適した設定を決定してください。

`SameSite` 属性が指定されていない場合、一部のブラウザーのデフォルト設定は `SameSite=Lax` になりました。

## `identityMap` での FPID の使用 {#identityMap}

`identityMap` で [!DNL FPID] を設定する方法の例を以下に示します。

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

他の ID タイプと同様に、`identityMap` 内の他の ID と共に [!DNL FPID] を含めることができます。 次に、認証済み [!DNL CRM ID] に含まれる [!DNL FPID] の例を示します。

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

ファーストパーティデータ収集が有効な場合にEdge Networkによって読み取られる Cookie に [!DNL FPID] が含まれている場合は、認証済みの [!DNL CRM ID] のみを取得する必要があります。

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

[!DNL FPID] の `primary` インジケーターがないため、次の `identityMap` では、Edge Networkからのエラー応答が返されます。 最後に、`identityMap` に存在する ID のうち 1 つを `primary` としてマークする必要があります。

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

## ID 階層 {#id-hierarchy}

[!DNL ECID] と [!DNL FPID] の両方が存在する場合、ユーザーの識別では [!DNL ECID] が優先されます。 これにより、既存の [!DNL ECID] がブラウザーの cookie ストアに存在する場合、その ID がプライマリ識別子のままとなり、既存の訪問者数が影響を受けるリスクがなくなります。 既存のユーザーの場合、[!DNL FPID] の有効期限が切れるか、ブラウザーポリシーまたは手動プロセスの結果として削除されるまで、[!DNL ECID] はプライマリ ID になりません。

ID の優先順位は次の順序で設定されます。

1. `identityMap` に含まれる [!DNL ECID]
1. cookie に保存された [!DNL ECID]
1. `identityMap` に含まれる [!DNL FPID]
1. cookie に保存された [!DNL FPID]

## ファーストパーティデバイス ID への移行 {#migrating-to-fpid}

以前の実装からファーストパーティデバイス ID に移行している場合は、移行がどのように見えるかを低レベルで視覚化するのが難しい可能性があります。

このプロセスを説明するために、以前にサイトを訪問した顧客が関与するシナリオや、[!DNL FPID] 移行がAdobeソリューションでその顧客を特定する方法にどのような影響を与えるかを考えてみましょう。

![FPID への移行後、訪問間に顧客の ID 値が更新される方法を示す図 ](../assets/identity/tracking/visits.png)

>[!IMPORTANT]
>
>`ECID` cookie は常に `FPID` よりも優先されます。

| 訪問 | 説明 |
| --- | --- |
| 初回訪問 | [!DNL FPID] cookie の設定をまだ開始していないとします。 [AMCV cookie](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html#section-c55af54828dc4cce89f6118655d694c8) に含まれる [!DNL ECID] は、訪問者の識別に使用される識別子になります。 |
| 2 回目の訪問 | [!DNL FPID] ソリューションのロールアウトが開始されました。 既存の [!DNL ECID] は引き続き存在し、訪問者を識別するためのプライマリ識別子として残ります。 |
| 3 回目の訪問 | 2 回目と 3 回目の訪問の間に、ブラウザーポリシーが原因で [!DNL ECID] が削除されるまでに十分な時間が経過しました。 ただし、[!DNL FPID] は [!DNL DNS] [!DNL A] レコードを使用して設定されているので、[!DNL FPID] は保持されます。 [!DNL FPID] はプライマリ ID と見なされ、エンドユーザーデバイスに書き込まれる [!DNL ECID] のシード処理に使用されるようになりました。 これで、このユーザーは、Adobe Experience PlatformおよびExperience Cloudソリューションの新規訪問者と見なされます。 |
| 4 回目の訪問 | 3 回目と 4 回目の訪問の間に、ブラウザーポリシーが原因で [!DNL ECID] が削除されるまでに十分な時間が経過しました。 前回の訪問同様、[!DNL FPID] の設定の仕方が原因で残っています。 今回は、前回の訪問と同じ [!DNL ECID] が生成されます。 ユーザーは、Experience PlatformおよびExperience Cloudのソリューション全体で、前回の訪問と同じユーザーとして表示されます。 |
| 5 回目の訪問 | 4 回目と 5 回目の訪問の間に、エンドユーザーはブラウザー内のすべての Cookie をクリアしました。 新しい [!DNL FPID] が生成され、新しい [!DNL ECID] の作成をシードするために使用されます。 これで、このユーザーは、Adobe Experience PlatformおよびExperience Cloudソリューションの新規訪問者と見なされます。 |

{style="table-layout:auto"}

## FAQ {#faq}

以下は、ファーストパーティデバイス ID に関するよくある質問への回答のリストです。

### 単に ID を生成することと ID のシード処理はどのように異なりますか？

シーディングの概念は、Adobe Experience Cloudに渡された [!DNL FPID] を決定論的アルゴリズムを使用して [!DNL ECID] に変換するという点でユニークです。 同じ [!DNL FPID] がEdge Networkに送信されるたびに、同じ [!DNL ECID] が [!DNL FPID] からシードされます。

### ファーストパーティデバイス ID はいつ生成されますか？

潜在的な訪問者の水増しを減らすには、Web SDK を使用して最初のリクエストを行う前に [!DNL FPID] を生成する必要があります。 ただし、これを行えない場合は、そのユーザーの [!DNL ECID] が引き続き生成され、プライマリ識別子として使用されます。 生成された [!DNL FPID] は、[!DNL ECID] が存在しなくなるまで、プライマリ識別子になりません。

### ファーストパーティデバイス ID をサポートしているデータ収集方法はどれですか？

現在、Web SDK のみがファーストパーティデバイス ID をサポートしています。

### ファーストパーティデバイス ID は Platform またはExperience Cloudソリューションに保存されていますか？

[!DNL FPID] を使用して [!DNL ECID] をシードすると、`identityMap` からドロップされて、生成された [!DNL ECID] に置き換えられます。 [!DNL FPID] は、Adobe Experience PlatformまたはExperience Cloudのソリューションには保存されません。
