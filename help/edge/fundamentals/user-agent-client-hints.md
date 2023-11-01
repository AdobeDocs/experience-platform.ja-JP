---
title: ユーザーエージェントクライアントのヒント
description: Web SDK でのユーザーエージェントのクライアントヒントの仕組みについて説明します。 クライアントヒントを使用すると、Web サイトの所有者は、ユーザーエージェント文字列で使用可能な情報の多くにアクセスできますが、プライバシーをより保持する方法でアクセスできます。
keywords: user-agent；クライアントヒント；文字列；ユーザーエージェント文字列；低エントロピー；高エントロピー
exl-id: a909b1d1-be9d-43ba-bb4b-d28b0c609f65
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '1192'
ht-degree: 7%

---

# ユーザーエージェントクライアントのヒント

## 概要 {#overview}

Web ブラウザーが Web サーバーに対してリクエストをおこなうたびに、リクエストのヘッダーには、ブラウザーと、ブラウザーが実行されている環境に関する情報が含まれます。 このデータはすべて、ユーザーエージェント文字列と呼ばれる文字列に集計されます。

次に、 [!DNL Mac OS] デバイス。

>[!NOTE]
>
>何年にもわたって、ユーザーエージェント文字列に含まれるブラウザーおよびデバイス情報の量が増え、複数回修正されています。 次の例は、最も一般的なユーザーエージェント情報の一部を示しています。

```shell
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.0.0 Safari/537.36`
```

| フィールド | 値 |
|---|---|
| ソフトウェア名 | Chrome |
| ソフトウェアのバージョン | 105 |
| 完全なソフトウェアのバージョン | 105.0.0.0 |
| レイアウトエンジン名 | AppleWebKit |
| レイアウトエンジンのバージョン | 537.36 |
| オペレーティングシステム | Mac OS X |
| オペレーティングシステムのバージョン | 10.15.7 |
| デバイス | インテルMac OS X 10_15_7 |

## ユースケース {#use-cases}

ユーザーエージェント文字列は、長い間、マーケティングチームや開発チームに、ブラウザー、オペレーティングシステム、デバイスでのサイトコンテンツの表示方法や Web サイトとのやり取りに関する重要なインサイトを提供してきました。

また、ユーザーエージェント文字列は、スパムをブロックし、様々な追加目的でサイトをクロールするボットをフィルタリングするためにも使用されます。

## Adobe Experience Cloudのユーザーエージェント文字列 {#user-agent-in-adobe}

Adobe Experience Cloudソリューションでは、ユーザーエージェント文字列を様々な方法で使用します。

* Adobe Analyticsでは、ユーザーエージェント文字列を使用して、Web サイトの訪問に使用するオペレーティングシステム、ブラウザー、デバイスに関する追加情報を追加し、導き出します。
* Adobe Audience ManagerとAdobe Targetは、ユーザーエージェント文字列から提供される情報に基づいて、エンドユーザーをセグメント化およびパーソナライゼーションキャンペーンの対象として認定します。

## ユーザーエージェントのクライアントヒントの概要 {#ua-ch}

近年、サイトの所有者やマーケティングベンダーは、リクエストヘッダーに含まれる他の情報と共に、ユーザーエージェント文字列を使用してデジタル指紋を作成しています。 これらの指紋は、知らないユーザーを識別する手段として使用できます。

ユーザーエージェント文字列がサイトの所有者に役立つ重要な目的にもかかわらず、ブラウザー開発者は、ユーザーエージェント文字列の動作方法を変更し、エンドユーザーにとって潜在的なプライバシーの問題を制限することにしました。

彼らが開発した解は、と呼ばれる [ユーザーエージェントクライアントヒント](https://developer.chrome.com/docs/privacy-sandbox/user-agent/). クライアントヒントを使用すると、Web サイトで必要なブラウザー、オペレーティングシステム、デバイスの情報を収集できるほか、フィンガープリントなどの変換トラッキング方法に対する保護も強化できます。

クライアントヒントを使用すると、Web サイトの所有者は、ユーザーエージェント文字列で使用可能な情報の多くにアクセスできますが、プライバシーをより保持する方法でアクセスできます。

最新のブラウザーでユーザーが Web サーバーに送信される場合、必要かどうかに関係なく、要求ごとにユーザーエージェント文字列全体が送信されます。 一方、クライアントヒントは、サーバがブラウザに対して、クライアントに関して知りたい追加情報を要求する必要があるモデルを適用します。 このリクエストを受け取ると、ブラウザーは独自のポリシーまたはユーザー設定を適用して、返されるデータを決定できます。 すべてのリクエストでデフォルトでユーザーエージェント文字列全体を公開する代わりに、明示的で監査可能な方法でアクセスを管理するようになりました。

## ブラウザーのサポート {#browser-support}

[ユーザーエージェントクライアントのヒント](https://developer.chrome.com/docs/privacy-sandbox/user-agent/) は、次の場合に導入されました： [!DNL Google Chrome]バージョン 89。

追加の Chromium ベースのブラウザーは、次のようなクライアントヒント API をサポートしています。

* [!DNL Microsoft Edge]
* [!DNL Opera]
* [!DNL Brave]
* [!DNL Chrome for Android]
* [!DNL Opera for Android]
* [!DNL Samsung Internet]

## カテゴリ {#categories}

ユーザーエージェントのクライアントヒントには次の 2 つのカテゴリがあります。

* [低エントロピーのクライアントヒント](#low-entropy)
* [高エントロピーのクライアントヒント](#high-entropy)

### 低エントロピーのクライアントヒント {#low-entropy}

低エントロピーのクライアントヒントには、ユーザーを指紋に使用できない基本情報が含まれます。 ブラウザーのブランド、プラットフォーム、リクエストがモバイルデバイスから送信されたかどうかなどの情報。

低エントロピーのクライアントヒントは、Web SDK ではデフォルトで有効になっており、リクエストごとに渡されます。

| HTTP ヘッダー | JavaScript | デフォルトで User-Agent に含まれる | デフォルトでクライアントヒントに含まれる |
|---|---|---|---|
| `Sec-CH-UA` | `brands` | ○ | ○ |
| `Sec-CH-UA-Platform` | `platform` | ○ | ○ |
| `Sec-CH-UA-Mobile` | `mobile` | ○ | ○ |

### 高エントロピーのクライアントヒント {#high-entropy}

高エントロピーのクライアントヒントは、プラットフォームのバージョン、アーキテクチャ、モデル、ビット数（64 ビットまたは 32 ビットプラットフォーム）、フルオペレーティングシステムのバージョンなど、クライアントデバイスに関する詳細な情報です。 この情報は、フィンガープリントで使用される可能性があります。

| HTTP ヘッダー | JavaScript | デフォルトでユーザーエージェントに含まれる | デフォルトでクライアントヒントに含まれる |
|---|---|---|---|
| `Sec-CH-UA-Platform-Version` | `platformVersion` | ○ | × |
| `Sec-CH-UA-Arc` | `architecture` | ○ | × |
| `Sec-CH-UA-Model` | `model` | ○ | × |
| `Sec-CH-UA-Bitness` | `Bitness` | ○ | × |
| `Sec-CH-UA-Full-Version-List` | `fullVersionList` | ○ | × |

高エントロピーのクライアントヒントは、Web SDK ではデフォルトで無効になっています。 これらを有効にするには、高エントロピーのクライアントヒントを要求するように Web SDK を手動で設定する必要があります。

## 高エントロピーのクライアントヒントがExperience Cloud解に与える影響 {#impact-in-experience-cloud-solutions}

一部のAdobe Experience Cloudソリューションは、レポート生成時に高エントロピーなクライアントヒントに含まれる情報に依存します。

お使いの環境で高エントロピーのクライアントヒントを有効にしない場合、以下に説明するAdobe AnalyticsとAudience Managerのレポートと特性は機能しません。

### Adobe Analyticsは、高エントロピーなクライアントヒントに頼るレポートを作成 {#analytics}

The [オペレーティングシステム](https://experienceleague.adobe.com/docs/analytics/components/dimensions/operating-systems.html?lang=ja) ディメンションには、高エントロピーのクライアントヒントとして保存されるオペレーティングシステムバージョンが含まれます。 高エントロピーのクライアントヒントが有効になっていない場合、Chromium ブラウザーから収集されたヒットに対して、オペレーティングシステムのバージョンが不正確になる可能性があります。

### Audience Manager特性は高エントロピーのクライアントヒントに依存 {#aam}

[!DNL Google] は更新済みです [!DNL Chrome] ブラウザー機能を使用して、 `User-Agent` ヘッダー。 その結果、を使用してAudience Managerをおこなうお客様は、 [DIL](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=ja) は、に基づく特性の信頼できる情報を受け取らなくなります [プラットフォームレベルのキー](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/traits/trait-device-targeting.html).

Audience Managerのお客様がターゲティングにプラットフォームレベルのキーを使用する場合は、 [Experience PlatformWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=ja) の代わりに [DIL](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=ja)、および有効 [高エントロピークライアントヒント](#enabling-high-entropy-client-hints) 信頼できる特性データを引き続き受信する。

## 高エントロピーのクライアントヒントの有効化 {#enabling-high-entropy-client-hints}

Web SDK デプロイメントで高エントロピーのクライアントヒントを有効にするには、 `highEntropyUserAgentHints` コンテキストオプション (「 [設定ドキュメント](configuring-the-sdk.md#context)（既存の設定と共に）

例えば、Web プロパティから高エントロピーのクライアントヒントを取得する場合、設定は次のようになります。

`context: ["highEntropyUserAgentHints", "web"]`

## 例 {#example}

ブラウザーが Web サーバーに対しておこなう最初のリクエストのヘッダーに含まれるクライアントヒントには、ブラウザーのブランド、ブラウザーのメジャーバージョンおよびクライアントがモバイルデバイスかどうかを示すインジケーターが含まれます。 次に示すように、データの各部分には、1 つのユーザーエージェント文字列にグループ化されるのではなく、独自のヘッダー値が含まれます。

```shell
Sec-CH-UA: "Chromium";v="101", "Google Chrome";v="101", " Not;A Brand";v="99"

Sec-CH-UA-Mobile: ?0

Sec-CH-UA-Platform: "macOS
```

同等の [!DNL User-Agent] 同じブラウザーのヘッダーは次のようになります。

```shell
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.54 Safari/537.36
```

情報は似ていますが、サーバへの最初の要求にはクライアントのヒントが含まれます。 これには、ユーザーエージェント文字列で使用できる項目のサブセットのみが含まれます。 リクエストに見つからないのは、オペレーティングシステムのアーキテクチャ、完全なオペレーティングシステムのバージョン、レイアウトエンジン名、レイアウトエンジンのバージョン、完全なブラウザーのバージョンです。

ただし、後続のリクエストでは、 [!DNL Client Hints API] web サーバーがデバイスに関する追加の詳細を要求できるようにします。 これらの値が要求された場合、ブラウザーポリシーまたはユーザー設定に応じて、ブラウザーの応答にその情報が含まれる場合があります。

次に、 [!DNL Client Hints API] 高エントロピー値が要求される場合：


```json
{
   "architecture":"x86",
   "bitness":"64",
   "brands":[
      {
         "brand":" Not A;Brand",
         "version":"99"
      },
      {
         "brand":"Chromium",
         "version":"100"
      },
      {
         "brand":"Google Chrome",
         "version":"100"
      }
   ],
   "fullVersionList":[
      {
         "brand":" Not A;Brand",
         "version":"99.0.0.0"
      },
      {
         "brand":"Chromium",
         "version":"100.0.4896.127"
      },
      {
         "brand":"Google Chrome",
         "version":"100.0.4896.127"
      }
   ],
   "mobile":false,
   "model":"",
   "platformVersion":"12.2.1"
}
```
