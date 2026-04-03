---
title: データ収集での1st パーティデバイス IDの使用
description: Edge Networkにデータを送信するweb実装で永続的なIDを使用するために、ファーストパーティデバイス ID （FPID）を設定します。
source-git-commit: 696e5098ebf556bfc0fa4fc22ff637cb0835eee0
workflow-type: tm+mt
source-wordcount: '1905'
ht-degree: 0%

---

# データ収集での1st パーティデバイス IDの使用

Experience Platform Edge Networkでは、Experience Cloud ID （ECID）を使用してweb サイト訪問者を識別します。 所有しているプロパティのIDの耐久性を向上させるには、1st パーティデバイス ID （FPID）と呼ばれる独自のデバイス IDを設定して管理します。 Edge Networkでは、FPIDを使用して、Adobe ソリューションが使用するECIDのシードを設定します。

このページでは、ECIDと`identityMap`について詳しいユーザーを前提としています。 詳しくは、[&#x200B; データ収集のID](./overview.md)を参照してください。

## FPIDを使用するタイミング {#when-to-use}

ブラウザーの制限により、Adobeが再訪問者を認識するために使用するCookieの有効期間を短縮できます。 組織が所有および管理するサイトでより耐久性のあるIDが必要な場合、FPIDを使用して独自のデバイス IDを管理し、それを使用してECIDをシード化できます。

FPIDは、Web SDK タグ拡張機能を含む、Web SDKを使用するWeb実装でサポートされています。 主な目標が、組織が所有するドメインでより強力なIDの保持である場合や、所有しているweb プロパティでのレポートとパーソナライゼーションの継続性が必要な場合に最適です。 また、制御するインフラストラクチャから1st パーティ Cookieを設定および管理することもできます。

FPIDは、アプリからwebへの引き継ぎや、複数のドメイン間でのIDの連続性を主な目標としている場合に適したツールではありません。 これらのシナリオについては、[&#x200B; モバイルからwebへのID共有](./mobile-to-web.md)および[&#x200B; クロスドメイン共有](./cross-domain-sharing.md)を参照してください。

FPIDを利用する利点は次のとおりです。

* 所有プロパティの永続性を向上：
* デバイス識別子の生成と管理方法をより詳細に制御します。
* 分析とパーソナライゼーションのための耐久性のある基盤。

FPIDを利用するためのトレードオフには、次のようなものがあります。

* デフォルトのID行動に依存するよりも実装責任が大きい。
* サーバーサイドのCookie ロジックとデータ収集設定全体で調整する。
* 識別子が期待どおりに使用されていることを確認するための追加の検証。

### 高レベル設定パス

1. 制御するインフラストラクチャ上で1st パーティデバイス IDを生成および管理します。
1. このIDを[&#x200B; ファーストパーティ Cookie](#setting-cookie-datastreams)または[ID ペイロード &#x200B;](#identityMap)から読み取るように実装を設定します。
1. 再訪問者が、自社のプロパティで一貫性のあるIDを継続的に維持していることを検証しましょう。

## FPIDの仕組み {#how-fpids-work}

Adobe Experience Cloudに渡されたFPIDは、決定論的アルゴリズムを使用してECIDに変換されます。 同じFPIDがEdge Networkに送信されるたびに、同じECIDがFPIDからシードされます。 FPIDを使用してECIDのシードを設定すると、そのFPIDは`identityMap`から削除され、生成されたECIDに置き換えられます。 FPIDは、Adobe Experience PlatformまたはExperience Cloud ソリューションには保存されません。

ECIDとFPIDの両方が存在する場合、ECIDは常に最初にユーザーを識別するために使用されます。 この優先順位付けにより、既存のECIDがブラウザーのcookie ストアに存在する場合、そのECIDがプライマリ IDのままとなり、既存の訪問者数がインフレーションのリスクを受けないようにします。 既存のユーザーの場合、ECIDが期限切れになるか、ブラウザーポリシーまたは手動アクションの結果として削除されるまで、FPIDはプライマリ IDにはなりません。

IDは、次の順序で優先順位が付けられます。

1. `identityMap`に含まれるECID
1. Cookieに保存されたECID
1. `identityMap`に含まれるFPID
1. Cookieに保存されたFPID

## FPID Cookieの生成と設定 {#set-fpid-cookie}

Edge Networkでは、[UUIDv4形式](https://datatracker.ietf.org/doc/html/rfc4122)に準拠するIDのみを使用できます。 UUIDv4形式でないデバイス IDは拒否されます。

* UUIDは一意かつランダムで、衝突の可能性は無視できます。
* UUIDv4は、IP アドレスまたはその他の個人を特定できる情報（PII）を使用してシードすることはできません。
* UUIDを生成するライブラリは、すべてのプログラミング言語で利用できます。

### サーバーサイド Cookie設定 {#set-cookie-server}

独自のサーバーを通じてCookieを設定する場合、様々な方法を使用して、Cookieがブラウザーポリシーによって制限されるのを防ぐことができます。

* サーバーサイドスクリプト言語を使用したCookieの生成
* サイト上のサブドメインまたはその他のエンドポイントに対して行われたAPI リクエストに応じて、Cookieを設定します
* コンテンツ管理システムを使用したCookieの生成（CMS）
* コンテンツ配信ネットワーク（CDN）を使用したCookieの生成

ファーストパーティ Cookieが最も効果を発揮するのは、DNS [やJavaScript コードではなく、DNS &#x200B;](https://datatracker.ietf.org/doc/html/rfc1035)A レコード [&#x200B; （IPv4の場合）または](https://datatracker.ietf.org/doc/html/rfc3596)AAAA レコード `CNAME` （IPv6の場合）を使用するサーバーを使用している場合です。

>[!IMPORTANT]
>
>JavaScriptの`document.cookie` メソッドを使用して設定されたCookie （タグメソッド [`cookie.set()`](../tags/cookie.md)を使用する場合を含む）は、Cookieの期間を制限するブラウザーポリシーからほとんど保護されません。

`A`または`AAAA` レコードは、Cookieの設定と追跡でのみサポートされています。 データ収集の主な方法は、DNS `CNAME`を使用することです。 FPIDは`A`または`AAAA` レコードを使用して設定され、`CNAME`を使用してAdobeに送信されます。 [Adobeで管理されている証明書プログラム &#x200B;](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-first-party.html#adobe-managed-certificate-program)を使用すると、データ収集用に`CNAME`を設定できます。

### Cookieの設定時期 {#when-to-set-cookie}

FPID Cookieは、Edge Networkにデータを送信する前に設定するのが理想的です。 実装でデータを収集する前に同意が必要な場合は、FPID Cookieと同意フローの調整に関するガイダンスについて、[&#x200B; ファーストパーティデバイス IDを使用した同意](./consent.md#consent-with-fpids)を参照してください。 訪問者のインフレーションは、FPIDが最初のリクエストからECIDのシードを取得できるようにすると減少します。 それが不可能なシナリオでは、ECIDは既存のメソッドを使用して生成され、Cookieが存在する限りプライマリ識別子として機能します。 生成されたFPIDは、ECIDが存在しなくなるまで、プライマリ IDにはなりません。 ECIDが最終的にブラウザー削除ポリシーの影響を受けるが、FPIDが影響を受けないと仮定すると、FPIDは次回の訪問時のプライマリ識別子となり、その後の訪問時にECIDをシードするために使用されます。

### 有効期限の設定 {#set-expiration}

Adobeでは、FPID Cookieの有効期間を慎重に検討することをお勧めします。 組織のプライバシーポリシーと、組織が活動している国や地域の法律やポリシーを考慮することが重要です。 組織の設定に応じて、全社的なCookie設定ポリシーを採用するか、事業展開するロケールごとにユーザーによって異なるCookie設定ポリシーを採用することができます。 最初のCookieの有効期限に関係なく、新しいサイト訪問が発生するたびに有効期限を拡張するロジックを含めることを確認します。

### Cookie フラグ {#cookie-flags}

Cookie フラグには、様々なブラウザーでのCookieの扱い方に影響を与えるものがいくつかあります。

* **`HTTPOnly`**: `HTTPOnly` フラグを使用して設定されたCookieは、クライアントサイドスクリプトを使用してアクセスできません。 つまり、FPIDの設定時に`HTTPOnly` フラグを設定した場合、`identityMap`に含めるcookie値を読み取るには、サーバーサイドのスクリプト言語を使用する必要があります。 Edge NetworkにFPID Cookieの値を読み取ってもらう場合、`HTTPOnly` フラグを設定すると、値がクライアントサイドスクリプトからアクセスできないことが保証されますが、Edge Networkのcookieの読み取り機能に悪影響を与えることはありません。 `HTTPOnly` フラグの使用は、Cookieの有効期間を制限できるCookie ポリシーには影響しません。 ただし、FPIDの値を設定して読み取る際には、まだ考慮する必要があります。
* **`Secure`**: `Secure`属性で設定されたCookieは、HTTPS プロトコル経由で暗号化された要求を持つサーバーにのみ送信されます。 このフラグを使用すると、中間者攻撃者がCookie値に簡単にアクセスできないようにすることができます。 可能であれば、常に`Secure` フラグを設定することをお勧めします。
* **`SameSite`**: `SameSite`属性を使用すると、サーバーはCookieがクロスサイトリクエストと共に送信されるかどうかを判断できます。 この属性は、クロスサイトフォージェリー攻撃に対するある程度の保護を提供します。 使用可能な値は`Strict`、`Lax`、`None`の3つです。 社内チームと相談して、自社に最適な設定を決定します。 `SameSite`属性が指定されていない場合、一部のブラウザーのデフォルト設定は`SameSite=Lax`です。

## FPIDをEdge Networkに送信する {#send-fpid}

Edge NetworkにFPIDを送信するには、次の2つの方法があります。

* **[方法1](#setting-cookie-datastreams)**: Web SDK呼び出しに`CNAME`を設定し、データストリーム設定にFPID Cookieの名前を含めます。
* **[方法2](#identityMap)**: ID マップにFPIDを含めます。

### 方法1: `CNAME`を設定し、データストリームにファーストパーティ ID Cookieを設定する {#setting-cookie-datastreams}

独自のドメインからFPID Cookieを設定するには、Web SDK呼び出しに独自の`CNAME`を設定し、データストリーム設定でファーストパーティ ID Cookie機能を有効にする必要があります。 DNS内の`CNAME` レコードを使用すると、あるドメイン名から別のドメイン名にエイリアスを作成できます。 このエイリアスは、サードパーティサービスを独自のドメインの一部であるかのように見せ、そのCookieをファーストパーティ Cookieのように見せるのに役立ちます。 `CNAME`を使用してファーストパーティデータ収集を有効にすると、データ収集エンドポイントに対して行われたリクエストに対して、ドメインのすべてのCookieが送信されます。

1. Adobeと連携して、組織で使用するデータ収集用の`CNAME` レコードを作成します。 完全なプロセスについては、[Adobeが管理する証明書プログラム &#x200B;](https://experienceleague.adobe.com/en/docs/core-services/interface/data-collection/adobe-managed-cert)を参照してください。
1. データストリームで&#x200B;**[!UICONTROL First Party ID Cookie]** オプションを有効にします。 この設定は、ID マップの値を検索する代わりに、ファーストパーティデバイス IDを検索する際に、指定されたCookieを参照するようにEdge Networkに指示します。 この設定を有効にする場合は、FPIDを保存する必要があるCookieの名前を指定する必要があります。 詳しくは、[&#x200B; データストリームの作成と設定](/help/datastreams/configure.md#advanced-options)を参照してください。

   ![&#x200B; ファーストパーティ ID Cookie設定を強調表示するデータストリーム設定を示すPlatform UI画像](/help/collection/js/assets/first-party-id-datastreams.png)

### 方法2: `identityMap`でFPIDを使用する {#identityMap}

独自のCookieにFPIDを保存する代わりに、ID マップを介してFPIDをEdge Networkに送信できます。

次に、`identityMap`でFPIDを設定する方法の例を示します。

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

他のID タイプと同様に、`identityMap`内の他のIDにFPIDを含めることができます。 次の例では、認証済みCRM IDを持つFPIDが含まれています。

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
        "id": "user@example.com",
        "authenticatedState": "authenticated",
        "primary": true
      }
    ]
  }
}
```

ファーストパーティデータ収集が有効になっている場合に、Edge Networkが読み取るCookieにFPIDが含まれている場合は、認証済みのCRM IDのみをキャプチャします。

```json
{
  "identityMap": {
    "EMAIL": [
      {
        "id": "user@example.com",
        "authenticatedState": "authenticated",
        "primary": true
      }
    ]
  }
}
```

次の`identityMap`は、FPIDの`primary` インジケーターが見つからないため、Edge Networkからのエラー応答の結果です。 `identityMap`に存在するIDのうち、少なくとも1つは`primary`としてマークされている必要があります。

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
        "id": "user@example.com",
        "authenticatedState": "authenticated"
      }
    ]
  }
}
```

## FPIDへの移行 {#migrating-to-fpid}

以前の実装から1st パーティデバイス IDに移行する場合、移行が低レベルでどのように見えるかを視覚化するのは難しい場合があります。 このプロセスを説明するために、以前にサイトを訪問したことがあるお客様と、FPIDの移行がAdobe ソリューションでのお客様の特定方法にどのような影響を及ぼすのかを考慮してください。

![FPIDに移行後、訪問間で顧客のID値がどのように更新されるかを示す図](/help/collection/js/assets/identity/tracking/visits.png)

| 訪問 | 説明 |
| --- | --- |
| 初回訪問 | まだFPID Cookieの設定を開始していないとします。 [AMCV cookie](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html#section-c55af54828dc4cce89f6118655d694c8)に含まれるECIDは、訪問者の識別に使用される識別子です。 |
| 2回目の訪問 | FPID ソリューションのロールアウトが開始されました。 既存のECIDは引き続き存在し、訪問者を識別するためのプライマリ IDのままです。 |
| 3回目の訪問 | 2回目と3回目の訪問の間に、ブラウザーポリシーのためにECIDが削除されたのに十分な時間が経過しました。 ただし、FPIDはDNS `A` レコードを使用して設定されているため、FPIDは保持されます。 FPIDはプライマリ IDと見なされ、エンドユーザーデバイスに書き込まれるECIDのシード処理に使用されます。 ユーザーは、Adobe Experience PlatformおよびExperience Cloud ソリューションの新しい訪問者とみなされます。 |
| 4回目の訪問 | 3回目と4回目の訪問の間に、ブラウザーポリシーのためにECIDが削除されたのに十分な時間が経過しました。 前回の訪問と同様に、FPIDは設定された方法に応じて残ります。 今回は、前回の訪問と同じECIDが生成されます。 Adobe Experience PlatformおよびExperience Cloud ソリューション全体で、前回の訪問と同じユーザーが表示されます。 |
| 5回目の訪問 | 4回目と5回目の訪問の間に、エンドユーザーはブラウザー内のすべてのCookieをクリアしました。 新しいFPIDが生成され、新しいECIDの作成にシードを設定するために使用されます。 ユーザーは、Adobe Experience PlatformおよびExperience Cloud ソリューションの新しい訪問者とみなされます。 |
