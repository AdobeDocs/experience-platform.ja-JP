---
title: User agent client hints
description: Web SDKでの User Agent クライアントヒントの仕組みを説明します。 Client Hints は、web サイト所有者に、ユーザーエージェント文字列で利用できるのとほぼ同じ量の情報に、よりプライバシーが保護された方法でアクセスできます。
keywords: user-agent;client hints；文字列；user-agent string；低エントロピー；高エントロピー
exl-id: a909b1d1-be9d-43ba-bb4b-d28b0c609f65
source-git-commit: 35429ec2dffacb9c0f2c60b608561988ea487606
workflow-type: tm+mt
source-wordcount: '1244'
ht-degree: 4%

---

# User agent client hints

## 概要 {#overview}

Web ブラウザーが web サーバーにリクエストを送信するたびに、リクエストのヘッダーにブラウザーとブラウザーが動作している環境に関する情報が含まれます。 これらのデータはすべて、ユーザーエージェント文字列と呼ばれる文字列に集計されます。

[!DNL Mac OS] デバイスで動作しているChrome ブラウザーからのリクエストでユーザーエージェント文字列がどのように表示されるかを次の例に示します。

>[!NOTE]
>
>ユーザーエージェント文字列に含まれるブラウザーおよびデバイス情報の量は、長年にわたって、何度も増加および変更されています。 以下の例に、最も一般的なユーザーエージェント情報の選択を示します。

```shell
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.0.0 Safari/537.36`
```

| フィールド | 値 |
|---|---|
| ソフトウェア名 | Chrome |
| ソフトウェアバージョン | 105 |
| 完全なソフトウェアバージョン | 105.0.0.0 |
| レイアウトエンジン名 | AppleWebKit |
| レイアウトエンジンのバージョン | 537.36 |
| オペレーティングシステム | MAC OS X |
| オペレーティングシステムのバージョン | 10.15.7 |
| デバイス | インテル（R） Mac OS X 10_15_7 |

## ユースケース {#use-cases}

ユーザーエージェント文字列は、長い間、マーケティングチームや開発チームに対して、ブラウザー、オペレーティングシステム、デバイスによるサイトコンテンツの表示方法や、ユーザーによる web サイトとのやり取りに関する重要なインサイトを提供するために使用されてきました。

また、ユーザーエージェント文字列は、スパムのブロックやサイトをクロールするボットのフィルタリングなど、様々な追加の目的にも使用されます。

## Adobe Experience Cloudのユーザーエージェント文字列 {#user-agent-in-adobe}

Adobe Experience Cloud ソリューションは、様々な方法でユーザーエージェント文字列を使用します。

* Adobe Analyticsは、ユーザーエージェント文字列を利用して、web サイトへの訪問に使用するオペレーティングシステム、ブラウザー、デバイスに関連する追加情報を補強および派生します。
* Adobe Audience ManagerおよびAdobe Targetは、ユーザーエージェント文字列から提供される情報に基づいて、セグメント化およびパーソナライゼーションキャンペーンの対象をエンドユーザーに限定します。

## User Agent の概要クライアントヒント {#ua-ch}

近年、サイト所有者やマーケティングベンダーは、リクエストヘッダーに含まれる他の情報と共にユーザーエージェント文字列を使用して、デジタル指紋を作成しています。 これらの指紋は、ユーザーが知らないうちにユーザーを特定する手段として使用できます。

ユーザーエージェント文字列がサイト所有者に役立つという重要な目的にもかかわらず、ブラウザー開発者は、エンドユーザーに対して発生する可能性のあるプライバシーの問題を制限するために、ユーザーエージェント文字列の動作方法を変更することにしました。

彼らが開発したソリューションは、[user agent client hints](https://developer.chrome.com/docs/privacy-sandbox/user-agent/) と呼ばれます。 Client Hints を使用すると、web サイトで必要なブラウザー、オペレーティングシステム、デバイスの情報を収集できると同時に、フィンガープリントのような秘密の追跡方法に対する保護を強化できます。

Client Hints は、web サイト所有者に、ユーザーエージェント文字列で利用できるのとほぼ同じ量の情報に、よりプライバシーが保護された方法でアクセスできます。

モダンブラウザーがユーザーを web サーバーに送信する場合、それが必要かどうかにかかわらず、すべてのリクエストでユーザーエージェント文字列全体が送信されます。 一方、Client Hints は、サーバーがクライアントについて知りたい追加情報をブラウザーに問い合わせる必要があるモデルを強制します。 このリクエストを受け取ったブラウザーは、独自のポリシーやユーザー設定を適用して、どのデータが返されるかを決定できます。 デフォルトですべてのリクエストにユーザーエージェント文字列全体を公開する代わりに、アクセスを明示的かつ監査可能な方法で管理するようになりました。

## ブラウザーのサポート {#browser-support}

[User Agent client hints](https://developer.chrome.com/docs/privacy-sandbox/user-agent/) は、[!DNL Google Chrome] バージョン 89 で導入されました。

その他の Chromium ベースのブラウザーは、次のような Client Hints API をサポートしています。

* [!DNL Microsoft Edge]
* [!DNL Opera]
* [!DNL Brave]
* [!DNL Chrome for Android]
* [!DNL Opera for Android]
* [!DNL Samsung Internet]

## カテゴリ {#categories}

User Agent クライアントヒントには、次の 2 つのカテゴリがあります。

* [低エントロピーのクライアントヒント](#low-entropy)
* [高エントロピーのクライアントヒント](#high-entropy)

### 低エントロピーのクライアントヒント {#low-entropy}

低エントロピーのクライアントヒントには、ユーザーのフィンガープリントに使用できない基本情報が含まれています。 ブラウザーのブランド、プラットフォーム、要求の発信元がモバイルデバイスであるかどうかなどの情報。

Web SDKでは、低エントロピーのクライアントヒントはデフォルトで有効になっており、リクエストのたびに渡されます。

| HTTP ヘッダー | JavaScript | デフォルトで User-Agent に含まれる | デフォルトで client hints に含まれる |
|---|---|---|---|
| `Sec-CH-UA` | `brands` | ○ | ○ |
| `Sec-CH-UA-Platform` | `platform` | ○ | ○ |
| `Sec-CH-UA-Mobile` | `mobile` | ○ | ○ |

### 高エントロピーのクライアントヒント {#high-entropy}

高エントロピーのクライアントヒントは、プラットフォームのバージョン、アーキテクチャ、モデル、ビット数（64 ビットまたは 32 ビットプラットフォーム）、完全なオペレーティングシステムバージョンなど、クライアントデバイスに関するより詳細な情報です。 この情報は、フィンガープリントに使用できる可能性があります。

| プロパティ | 説明 | HTTP ヘッダー | XDM パス | 例 | デフォルトでユーザーエージェントに含まれる | デフォルトで client hints に含まれる |
| --- | --- | --- | --- | --- |---|---|
| オペレーティングシステムのバージョン | オペレーティングシステムのバージョン。 | `Sec-CH-UA-Platform-Version` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.platformVersion` | `10.15.7` | ○ | × |
| アーキテクチャ | 基盤となるCPU アーキテクチャ。 | `Sec-CH-UA-Arch` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.architecture` | `x86` | ○ | × |
| デバイスモデル | 使用されるデバイスの名前。 | `Sec-CH-UA-Model` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.model` | `Intel Mac OS X 10_15_7` | ○ | × |
| ビット数 | 基になるCPU アーキテクチャがサポートするビット数。 | `Sec-CH-UA-Bitness` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.bitness` | `64` | ○ | × |
| ブラウザーベンダー | ブラウザーを作成した会社。 低エントロピーのヒント `Sec-CH-UA` でも、この要素が収集されます。 | `Sec-CH-UA-Full-Version-List` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.vendor` | `Google` | ○ | × |
| ブラウザー名 | 使用されるブラウザー。 低エントロピーのヒント `Sec-CH-UA` でも、この要素が収集されます。 | `Sec-UA-Full-Version-List` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.brand` | `Chrome` | ○ | × |
| ブラウザーバージョン | ブラウザーの重要なバージョン。 低エントロピーのヒント `Sec-CH-UA` でも、この要素が収集されます。 正確なブラウザーバージョンは自動的に収集されません。 | `Sec-UA-Full-Version-List` | `xdm.environment.browserDetails.`<br>`userAgentClientHints.version` | `105` | ○ | × |


Web SDKでは、高エントロピーのクライアントヒントはデフォルトで無効になっています。 これを有効にするには、高エントロピーのクライアントヒントをリクエストするように web SDKを手動で設定する必要があります。

## 高エントロピーのクライアントヒントはExperience Cloud ソリューションに影響を与える {#impact-in-experience-cloud-solutions}

一部のAdobe Experience Cloud ソリューションでは、レポートの生成時に、高エントロピーのクライアントヒントに含まれる情報に依存します。

お使いの環境で高エントロピーのクライアントヒントを有効にしない場合、以下に説明するAdobe AnalyticsおよびAudience Managerのレポートと特性は機能しません。

### 高エントロピーのクライアントヒントに基づくAdobe Analytics レポート {#analytics}

[ オペレーティングシステム ](https://experienceleague.adobe.com/docs/analytics/components/dimensions/operating-systems.html) ディメンションには、高エントロピーのクライアントヒントとして保存されるオペレーティングシステムバージョンが含まれます。 高エントロピーのクライアントヒントが有効になっていない場合、Chromium ブラウザーから収集されたヒットのオペレーティングシステムバージョンが不正確になる場合があります。

### 高エントロピーのクライアントヒントに基づくAudience Manager特性 {#aam}

[!DNL Google] は、[!DNL Chrome] ヘッダーを介して収集される情報を最小限に抑えるために、`User-Agent` ブラウザー機能を更新しました。 その結果、[DIL](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=ja) を使用しているAudience Managerのお客様は、[ プラットフォームレベルのキー ](https://experienceleague.adobe.com/docs/audience-manager/user-guide/features/traits/trait-device-targeting.html) に基づく特性に関する信頼性の高い情報を受け取れなくなります。

ターゲティングにプラットフォームレベルのキーを使用するAudience Managerのお客様は、[DILではなく ](/help/web-sdk/home.md) [Experience Platform Web SDK](https://experienceleague.adobe.com/docs/audience-manager/user-guide/dil-api/dil-overview.html?lang=ja) に切り替え、[ 高エントロピーのクライアントヒント ](#enabling-high-entropy-client-hints) を有効にして、信頼性の高い特性データを引き続き受信できるようにする必要があります。

## 高エントロピーのクライアントヒントの有効化 {#enabling-high-entropy-client-hints}

Web SDK デプロイメントで高エントロピーのクライアントヒントを有効にするには、`highEntropyUserAgentHints` フィールドに [`context`](/help/web-sdk/commands/configure/context.md) context オプションを追加する必要があります。

例えば、web プロパティから高エントロピーのクライアントヒントを取得するには、設定は次のようになります。

`context: ["highEntropyUserAgentHints", "web"]`

## 例 {#example}

ブラウザーから web サーバーに送信される最初のリクエストのヘッダーに含まれる Client Hints には、ブラウザーのブランド、ブラウザーのメジャーバージョン、クライアントがモバイルデバイスであるかどうかのインジケーターが含まれます。 次に示すように、各データは、単一のユーザーエージェント文字列にグループ化されるのではなく、独自のヘッダー値を持ちます。

```shell
Sec-CH-UA: "Chromium";v="101", "Google Chrome";v="101", " Not;A Brand";v="99"

Sec-CH-UA-Mobile: ?0

Sec-CH-UA-Platform: "macOS
```

同じブラウザーに対して同等の [!DNL User-Agent] ヘッダーは、次のようになります。

```shell
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.54 Safari/537.36
```

情報は似ていますが、サーバーへの最初のリクエストには client hints が含まれています。 これには、ユーザーエージェント文字列で使用できるもののサブセットのみが含まれます。 リクエストに含まれていないのは、オペレーティングシステムのアーキテクチャ、完全なオペレーティングシステムのバージョン、レイアウトエンジン名、レイアウトエンジンのバージョンおよび完全なブラウザーのバージョンです。

ただし、その後のリクエストでは、[!DNL Client Hints API] を使用すると、web サーバーはデバイスに関する追加の詳細を問い合わせることができます。 これらの値がリクエストされた場合、ブラウザーのポリシーやユーザー設定によっては、ブラウザーの応答にその情報が含まれることがあります。

以下は、高エントロピー値がリクエストされた場合に [!DNL Client Hints API] によって返される JSON オブジェクトの例です。


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
