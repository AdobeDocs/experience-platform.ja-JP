---
title: ユーザエージェントクライアントのヒント
description: Web SDK での User-Agent Client Hints の仕組みについて説明します。 クライアントヒントを使用すると、Web サイトの所有者は、User-Agent 文字列で使用可能な情報の多くにアクセスできますが、プライバシーをより保持する方法でアクセスできます。
keywords: user-agent;client-hints;文字列；user-agent 文字列；低エントロピー；高エントロピー
exl-id: a909b1d1-be9d-43ba-bb4b-d28b0c609f65
source-git-commit: 29679e85943f16bcb02064cc60a249a3de61e022
workflow-type: tm+mt
source-wordcount: '1155'
ht-degree: 10%

---

# ユーザエージェントクライアントのヒント

## 概要 {#overview}

Web ブラウザーが Web サーバーに対してリクエストをおこなうたびに、リクエストのヘッダーには、ブラウザーと、ブラウザーが実行されている環境に関する情報が含まれます。 このすべてのデータは、 [!DNL User-Agent] 文字列。

次に例を示します。 [!DNL User-Agent] 文字列が、 [!DNL Mac OS] デバイス。

>[!NOTE]
>
>数年間で、に含まれるブラウザーおよびデバイス情報の量 [!DNL User-Agent] 文字列が複数回増え、変更されました。 次の例は、最も一般的な [!DNL User-Agent] 情報。

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

[!DNL User-Agent] 文字列は、長い間、マーケティングチームや開発チームに対して、ブラウザー、オペレーティングシステムおよびデバイスでのサイトコンテンツの表示方法やユーザーの Web サイトとのやり取りに関する重要なインサイトを提供してきました。

[!DNL User-Agent] また、文字列は、スパムをブロックし、様々な追加目的でサイトをクロールするボットをフィルタリングするためにも使用されます。

## [!DNL User-Agent] Adobe Experience Cloudの文字列 {#user-agent-in-adobe}

Adobe Experience Cloudソリューションは [!DNL User-Agent] 文字列を様々な方法で指定できます。

* Adobe Analyticsは [!DNL User-Agent] 文字列を使用して、Web サイトの訪問に使用するオペレーティングシステム、ブラウザー、デバイスに関する追加情報を増補し、導き出します。
* Adobe Audience ManagerとAdobe Targetは、 [!DNL User-Agent] 文字列。

## User-Agent クライアントヒントの概要 {#ua-ch}

近年、サイト所有者とマーケティングベンダーは [!DNL User-Agent] 文字列と、デジタルフィンガープリントを作成するためのリクエストヘッダーに含まれる他の情報。 これらの指紋は、知らないユーザーを識別する手段として使用できます。

という重要な目的にもかかわらず [!DNL User-Agent] 文字列はサイト所有者に役立ち、ブラウザ開発者は、 [!DNL User-Agent] 文字列は機能し、エンドユーザーにとって発生する可能性のあるプライバシーの問題を制限します。

彼らが開発した解は、と呼ばれる [User-Agent クライアントのヒント](https://developer.chrome.com/docs/privacy-sandbox/user-agent/). クライアントヒントを使用すると、Web サイトで必要なブラウザー、オペレーティングシステム、デバイスの情報を収集できるほか、フィンガープリントなどの変換トラッキング方法に対する保護も強化できます。

Client Hints は、web サイト所有者に、[!DNL User-Agent] 文字列で利用できるのとほぼ同じ量の情報に、よりプライバシーが保護された方法でアクセスできます。

最新のブラウザーでは、Web サーバーにユーザーを送信すると、 [!DNL User-Agent] 文字列は、必要かどうかに関係なく、リクエストごとに送信されます。 一方、クライアントヒントは、サーバがブラウザに対して、クライアントに関して知りたい追加情報を要求する必要があるモデルを適用します。 このリクエストを受け取ると、ブラウザーは独自のポリシーまたはユーザー設定を適用して、返されるデータを決定できます。 全体を公開する代わりに、 [!DNL User-Agent] 文字列は、すべてのリクエストでデフォルトで、明示的で監査可能な方法でアクセスを管理するようになりました。

## ブラウザーのサポート {#browser-support}

[User-Agent クライアントのヒント](https://developer.chrome.com/docs/privacy-sandbox/user-agent/) が導入された [!DNL Google Chrome ]バージョン 89。

追加の Chromium ベースのブラウザーは、次のようなクライアントヒント API をサポートしています。

* [!DNL Microsoft Edge]
* [!DNL Opera]
* [!DNL Brave]
* [!DNL Chrome for Android]
* [!DNL Opera for Android]
* [!DNL Samsung Internet]

## カテゴリ {#categories}

ユーザーエージェントクライアントヒントには次の 2 つのカテゴリがあります。

* [低エントロピークライアントヒント](#low-entropy)
* [高エントロピーのクライアントヒント](#high-entropy)

### 低エントロピークライアントヒント {#low-entropy}

低エントロピーのクライアントヒントには、ユーザーを指紋に使用できない基本情報が含まれます。 ブラウザーのブランド、プラットフォーム、リクエストがモバイルデバイスから送信されたかどうかなどの情報。

低エントロピーのクライアントヒントは、Web SDK ではデフォルトで有効になっており、リクエストごとに渡されます。

| HTTP ヘッダー | JavaScript | デフォルトで User-Agent に含まれる | デフォルトでクライアントヒントに含まれる |
|---|---|---|---|
| `Sec-CH-UA` | `brands` | ○ | ○ |
| `Sec-CH-UA-Platform` | `platform` | ○ | ○ |
| `Sec-CH-UA-Mobile` | `mobile` | ○ | ○ |

### 高エントロピーのクライアントヒント {#high-entropy}

高エントロピーのクライアントヒントは、プラットフォームのバージョン、アーキテクチャ、モデル、ビット数（64 ビットまたは 32 ビットプラットフォーム）、フルオペレーティングシステムのバージョンなど、クライアントデバイスに関する詳細な情報です。 この情報は、フィンガープリントで使用される可能性があります。

| HTTP ヘッダー | JavaScript | デフォルトで User-Agent に含まれる | デフォルトでクライアントヒントに含まれる |
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

この [オペレーティングシステム](https://experienceleague.adobe.com/docs/analytics/components/dimensions/operating-systems.html?lang=ja) ディメンションには、高エントロピーのクライアントヒントとして保存されるオペレーティングシステムバージョンが含まれます。 高エントロピーのクライアントヒントが有効になっていない場合、Chromium ブラウザーから収集されたヒットに対して、オペレーティングシステムのバージョンが不正確になる可能性があります。

### Audience Manager特性は高エントロピーのクライアントヒントに依存 {#aam}

[!DNL Google] は更新済みです [!DNL Chrome] ブラウザー機能を使用して、 `User-Agent` ヘッダー。 その結果、を使用してAudience Managerをおこなうお客様は、 [DIL](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en) は、に基づく特性の信頼できる情報を受け取らなくなります [プラットフォームレベルのキー](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/traits/trait-device-targeting.html?lang=ja).

Audience Managerのお客様がターゲティングにプラットフォームレベルのキーを使用する場合は、 [Experience PlatformWeb SDK](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=ja) の代わりに [DIL](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=en)、有効 [高エントロピークライアントヒント](#enabling-high-entropy-client-hints) 信頼できる特性データを引き続き受信する。

## 高エントロピーのクライアントヒントの有効化 {#enabling-high-entropy-client-hints}

Web SDK デプロイメントで高エントロピーのクライアントヒントを有効にするには、 `highEntropyUserAgentHints` コンテキストオプション ( [設定ドキュメント](configuring-the-sdk.md#context)（既存の設定と共に）

例えば、Web プロパティから高エントロピーのクライアントヒントを取得する場合、設定は次のようになります。

`context: ["highEntropyUserAgentHints", "web"]`

## 例 {#example}

ブラウザーが Web サーバーに対しておこなう最初のリクエストのヘッダーに含まれるクライアントヒントには、ブラウザーのブランド、ブラウザーのメジャーバージョンおよびクライアントがモバイルデバイスかどうかを示すインジケーターが含まれます。 データの各部分には、1 つのヘッダーにグループ化されるのではなく、独自のヘッダー値が割り当てられます [!DNL User-Agent] 文字列に含まれます。

```shell
Sec-CH-UA: "Chromium";v="101", "Google Chrome";v="101", " Not;A Brand";v="99"

Sec-CH-UA-Mobile: ?0

Sec-CH-UA-Platform: "macOS
```

同等の [!DNL User-Agent] 同じブラウザーのヘッダーは次のようになります。

```shell
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.54 Safari/537.36
```

情報は似ていますが、サーバへの最初の要求にはクライアントのヒントが含まれます。 これらは、 [!DNL User-Agent] 文字列。 リクエストに見つからないのは、オペレーティングシステムのアーキテクチャ、完全なオペレーティングシステムのバージョン、レイアウトエンジン名、レイアウトエンジンのバージョン、完全なブラウザーのバージョンです。

ただし、以降のリクエストでは、 [!DNL Client Hints API] web サーバーがデバイスに関する追加の詳細を要求できるようにします。 これらの値が要求された場合、ブラウザーポリシーまたはユーザー設定に応じて、ブラウザーの応答にその情報が含まれる場合があります。

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
