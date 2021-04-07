---
title: Adobe Experience PlatformWeb SDKの設定
description: Adobe Experience PlatformWeb SDKの設定方法を説明します。
seo-description: Experience Platform Web SDK の設定方法について説明します
keywords: 設定；設定；SDK；エッジ；Web SDK；設定；edgeConfigId；コンテキスト；web;環境;placeContext;debugEnabled;edgeDomain;orgId;clickBeforeEventSend;defaultConsent;web設定；prehidingStyle;cookieDestinations;enationsDestinationsEnabled;idMigrationEnabled;thirdPartyCookiesEnabled;
exl-id: d1e95afc-0b8a-49c0-a20e-e2ab3d657e45
translation-type: tm+mt
source-git-commit: 2895975b9c103e6afba7db221223b4ef2116caf3
workflow-type: tm+mt
source-wordcount: '803'
ht-degree: 39%

---

# プラットフォームWeb SDKの設定

SDK の設定は、`configure` コマンドを使用しておこないます。

>[!IMPORTANT]
>
>`configure` は、常に最初に呼び出されるコマンドです。 ** 

```javascript
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId":"ADB3LETTERSANDNUMBERS@AdobeOrg"
});
```

設定時には、さまざまなオプションを設定できます。すべてのオプションは、以下で確認できます（カテゴリ別にグループ化されています）。

## 一般オプション

### `edgeConfigId`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 文字列 | ○ | なし |

{style=&quot;table-layout:auto&quot;}

割り当てられた設定 ID。SDK を適切なアカウントと設定にリンクします。1 つのページ内で複数のインスタンスを設定する場合は、インスタンスごとに異なる `edgeConfigId` を設定する必要があります。

### `context`

| **タイプ** | **必須** | **デフォルト値** |
| ---------------- | ------------ | -------------------------------------------------- |
| 文字列の配列 | × | `["web", "device", "environment", "placeContext"]` |

{style=&quot;table-layout:auto&quot;}

「[自動情報](../data-collection/automatic-information.md)」の説明に従って、自動的に収集するコンテキストカテゴリを示します。この設定を指定しない場合、デフォルトではすべてのカテゴリが使用されます。

### `debugEnabled`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| Boolean | × | `false` |

{style=&quot;table-layout:auto&quot;}

デバッグが有効かどうかを示します。 この設定を `true` に設定すると、次の機能が有効になります。

| **機能** | **関数** |
| ---------------------- | ------------------ |
| 同期検証 | 収集されたデータをスキーマと照らし合わせて検証し、ラベル `collect:error OR success` の下の応答でエラーを返します。 |
| コンソールログ | ブラウザーの JavaScript コンソールにデバッグメッセージを表示できるようにします |

{style=&quot;table-layout:auto&quot;}

### `edgeDomain` {#edge-domain}

このフィールドにファーストパーティドメインを入力します。 詳しくは、[ドキュメント](https://experienceleague.adobe.com/docs/core-services/interface/ec-cookies/cookies-first-party.html)を参照してください。

ドメインは、wwwのWebサイトの`data.{customerdomain.com}`に似ています。{customerdomain.com}。

### `orgId`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 文字列 | ○ | なし |

{style=&quot;table-layout:auto&quot;}

割り当てられた[!DNL Experience Cloud]組織ID。 1 つのページ内で複数のインスタンスを設定する場合は、インスタンスごとに異なる `orgId` を設定する必要があります。

## データの収集

### `clickCollectionEnabled` {#clickCollectionEnabled}

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| ブール値 | × | `true` |

{style=&quot;table-layout:auto&quot;}

リンククリックに関連付けられたデータが自動的に収集されるかどうかを示します。 詳しくは、[自動リンクトラッキング](../data-collection/track-links.md#automaticLinkTracking)を参照してください。

### `onBeforeEventSend`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 関数 | × | () => 未定義 |

{style=&quot;table-layout:auto&quot;}

すべてのイベントが送信される直前に呼び出されるコールバックを設定します。 フィールド `xdm` を持つオブジェクトがコールバックに送信されます。送信内容を変更するには、`xdm`オブジェクトを変更します。 コールバック内では、`xdm`オブジェクトには既にイベントコマンドに渡されたデータと、自動的に収集された情報が含まれています。 このコールバックのタイミングと例について詳しくは、[イベントのグローバルな変更](tracking-events.md#modifying-events-globally)を参照してください。

## プライバシーオプション

### `defaultConsent` {#default-consent}

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| オブジェクト | × | `"in"` |

{style=&quot;table-layout:auto&quot;}

ユーザーのデフォルトの同意を設定します。この設定は、ユーザーの同意の環境設定が既に保存されていない場合に使用します。 他の有効な値は`"pending"`と`"out"`です。 このデフォルト値は、ユーザーのプロファイルに保持されません。 ユーザーのプロファイルは、`setConsent`が呼び出された場合にのみ更新されます。
* `"in"`:この設定が設定されている場合、または値が指定されていない場合は、ユーザーの同意を得ずに作業を続行します。
* `"pending"`:この設定を指定すると、ユーザーが同意を求めるプリファレンスが表示されるまで作業はキューに入れられます。
* `"out"`:この設定を指定すると、ユーザーが同意の環境設定を行うまで作業は破棄されます。ユーザーの環境設定を指定した後、作業を続行するか、ユーザーの環境設定に基づいて中止します。詳しくは、[同意のサポート](../consent/supporting-consent.md)を参照してください。

## パーソナライゼーションオプション

### `prehidingStyle`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 文字列 | × | なし |

{style=&quot;table-layout:auto&quot;}

パーソナライズされたコンテンツをサーバーから読み込む際に、Web ページのコンテンツ領域を非表示にする CSS スタイル定義を作成するために使用します。このオプションを指定しない場合、SDKは、パーソナライズされたコンテンツの読み込み中にコンテンツ領域を非表示にしません。その結果、「ちらつき」が発生する可能性があります。

例えば、Webページ上の要素のIDが`container`で、その要素のパーソナライズされたコンテンツがサーバーから読み込まれる間にデフォルトのコンテンツを非表示にする場合は、次のプレヒードスタイルを使用します。

```javascript
  prehidingStyle: "#container { opacity: 0 !important }"
```

## オーディエンスのオプション

### `cookieDestinationsEnabled`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| ブール値 | × | `true` |

{style=&quot;table-layout:auto&quot;}

[!DNL Audience Manager] cookieの宛先を有効にします。これにより、セグメントクオリフィケーションに基づいたcookieの設定が可能になります。

### `urlDestinationsEnabled`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| ブール値 | × | `true` |

{style=&quot;table-layout:auto&quot;}

[!DNL Audience Manager] URLの送信先を有効にします。これにより、セグメントクオリフィケーションに基づいてURLを呼び出すことができます。

## ID オプション

### `idMigrationEnabled` {#id-migration-enabled}

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| ブール値 | × | `true` |

{style=&quot;table-layout:auto&quot;}

trueの場合、SDKは古いAMCV cookieを読み取って設定します。 このオプションは、サイトの一部で訪問者.jsを使用している場合でも、Adobe Experience PlatformWeb SDKの使用に移行する際に役立ちます。 訪問者APIがページで定義されている場合、ECIDのSDKクエリ訪問者API。 このオプションを使用すると、Adobe Experience PlatformWeb SDKを使用してページを2重タグでき、同じECIDを持つことができます。

### `thirdPartyCookiesEnabled`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| ブール値 | × | `true` |

{style=&quot;table-layout:auto&quot;}

アドビのサードパーティ Cookie の設定を有効にします。SDKは、サードパーティのコンテキストで訪問者IDを保持して、サイト間で同じ訪問者IDを使用できるようにします。 複数のサイトがある場合、またはパートナーとデータを共有する場合は、このオプションを使用します。ただし、プライバシー上の理由からこのオプションが望ましくない場合もあります。
