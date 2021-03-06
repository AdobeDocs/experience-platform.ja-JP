---
title: Adobe Experience Platform Web SDK の設定
description: Adobe Experience Platform Web SDK の設定方法について説明します。
seo-description: Learn how to configure the Experience Platform Web SDK
keywords: 設定；設定；SDK；エッジ；Web SDK；設定；edgeConfigId；コンテキスト；web；デバイス；環境；placeContext;debugEnabled;edgeDomain;orgId;clickCollectionEnabled;onBeforeEventSend;defaultConsent;web sdk 設定；prehidingStyle;cookieDestinationsEnabled;DestinationsEnabled;idMigrationEnabled;thirdPartyCookiesEnabled;
exl-id: d1e95afc-0b8a-49c0-a20e-e2ab3d657e45
source-git-commit: 4d0f1b3e064bd7b24e17ff0fafb50d930b128968
workflow-type: tm+mt
source-wordcount: '860'
ht-degree: 39%

---

# Platform Web SDK の設定

SDK の設定は、`configure` コマンドを使用しておこないます。

>[!IMPORTANT]
>
>`configure` が *常に* 最初に呼び出されたコマンド。

```javascript
alloy("configure", {
  "edgeConfigId": "ebebf826-a01f-4458-8cec-ef61de241c93",
  "orgId":"ADB3LETTERSANDNUMBERS@AdobeOrg"
});
```

設定時には、さまざまなオプションを設定できます。すべてのオプションは、以下で確認できます（カテゴリ別にグループ化されています）。

## 一般オプション

### `edgeConfigId`

>[!NOTE]
>
>**Edge 設定は Datastreams に商標変更されました。 データストリーム ID は設定 ID と同じです。**

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

「[自動情報](../data-collection/automatic-information.md)」の説明に従って、自動的に収集するコンテキストカテゴリを示します。この設定を指定しない場合、すべてのカテゴリがデフォルトで使用されます。

### `debugEnabled`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| ブール値 | × | `false` |

{style=&quot;table-layout:auto&quot;}

デバッグが有効かどうかを示します。 この設定を `true` に設定すると、次の機能が有効になります。

| **機能** | **関数** |
| ---------------------- | ------------------ |
| コンソールログ | ブラウザーの JavaScript コンソールにデバッグメッセージを表示できるようにします |

{style=&quot;table-layout:auto&quot;}

### `edgeDomain` {#edge-domain}

このフィールドに、ファーストパーティドメインを入力します。 詳しくは、 [ドキュメント](https://experienceleague.adobe.com/docs/core-services/interface/ec-cookies/cookies-first-party.html?lang=ja).

ドメインは、 `data.{customerdomain.com}` www 上の web サイトの場合は{customerdomain.com}。

### `edgeBasePath` {#edge-base-path}

Adobe サービスとの通信およびやり取りに使用される edgeDomain の後のパス。  多くの場合、これはデフォルトの実稼動環境を使用しない場合にのみ変更されます。

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 文字列 | × | ee |

{style=&quot;table-layout:auto&quot;}

### `orgId`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 文字列 | ○ | なし |

{style=&quot;table-layout:auto&quot;}

割り当てられた [!DNL Experience Cloud] 組織 ID。 1 つのページ内で複数のインスタンスを設定する場合は、インスタンスごとに異なる `orgId` を設定する必要があります。

## データの収集

### `clickCollectionEnabled` {#clickCollectionEnabled}

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| ブール値 | × | `true` |

{style=&quot;table-layout:auto&quot;}

リンククリック数に関連付けられたデータを自動的に収集するかどうかを示します。 詳しくは、 [自動リンクトラッキング](../data-collection/track-links.md#automaticLinkTracking) を参照してください。 ダウンロード属性を含むリンクや、リンクがファイル拡張子で終わるリンクは、ダウンロードリンクとしてもラベル付けされます。 ダウンロードリンク修飾子は、正規表現で設定できます。 デフォルト値は です。`"\\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$"`

### `onBeforeEventSend`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 関数 | × | () => 未定義 |

{style=&quot;table-layout:auto&quot;}

すべてのイベントに対して呼び出されるコールバックを送信直前に設定します。 フィールド `xdm` を持つオブジェクトがコールバックに送信されます。送信内容を変更するには、 `xdm` オブジェクト。 コールバック内では、 `xdm` オブジェクトには、既にイベントコマンドに渡されたデータと、自動的に収集された情報が含まれています。 このコールバックのタイミングと例について詳しくは、[イベントのグローバルな変更](tracking-events.md#modifying-events-globally)を参照してください。

## プライバシーオプション

### `defaultConsent` {#default-consent}

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| オブジェクト | × | `"in"` |

{style=&quot;table-layout:auto&quot;}

ユーザーのデフォルトの同意を設定します。この設定は、ユーザーの同意設定が既に保存されていない場合に使用します。 その他の有効な値は次のとおりです。 `"pending"` および `"out"`. このデフォルト値は、ユーザーのプロファイルに保持されません。 ユーザーのプロファイルは、 `setConsent` が呼び出されます。
* `"in"`:この設定が設定されている場合、または値が指定されていない場合は、ユーザーの同意設定なしで作業が続行されます。
* `"pending"`:この設定を指定した場合、作業はユーザーが同意設定を提供するまでキューに入れられます。
* `"out"`:この設定を指定すると、ユーザーが同意設定を指定するまで作業は破棄されます。
ユーザーの環境設定を指定した後、作業を続行するか、ユーザーの環境設定に基づいて中止します。詳しくは、[同意のサポート](../consent/supporting-consent.md)を参照してください。

## パーソナライゼーションオプション

### `prehidingStyle`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 文字列 | × | なし |

{style=&quot;table-layout:auto&quot;}

パーソナライズされたコンテンツをサーバーから読み込む際に、Web ページのコンテンツ領域を非表示にする CSS スタイル定義を作成するために使用します。このオプションを指定しない場合、SDK は、パーソナライズされたコンテンツの読み込み中に、コンテンツ領域を非表示にしようとしません。その結果、「ちらつき」が発生する可能性があります。

例えば、Web ページ上の要素の ID が `container`（パーソナライズされたコンテンツがサーバーから読み込まれる間、デフォルトコンテンツを非表示にする）は、次の事前非表示スタイルを使用します。

```javascript
  prehidingStyle: "#container { opacity: 0 !important }"
```

## オーディエンスのオプション

### `cookieDestinationsEnabled`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| ブール値 | × | `true` |

{style=&quot;table-layout:auto&quot;}

有効 [!DNL Audience Manager] Cookie の宛先：セグメントの資格に基づいて Cookie を設定できます。

### `urlDestinationsEnabled`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| ブール値 | × | `true` |

{style=&quot;table-layout:auto&quot;}

有効 [!DNL Audience Manager] URL の宛先。セグメントの選定に基づいて URL を呼び出すことができます。

## ID オプション

### `idMigrationEnabled` {#id-migration-enabled}

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| ブール値 | × | `true` |

{style=&quot;table-layout:auto&quot;}

true の場合、SDK は古い AMCV Cookie を読み取って設定します。 このオプションは、サイトの一部で引き続き Visitor.js を使用している間に、Adobe Experience Platform Web SDK の使用に移行する際に役立ちます。 訪問者 API がページで定義されている場合、SDK は訪問者 API に対して ECID に対してクエリを実行します。 このオプションを使用すると、Adobe Experience Platform Web SDK を使用し、同じ ECID を持つページをデュアルタグ付けできます。

### `thirdPartyCookiesEnabled`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| ブール値 | × | `true` |

{style=&quot;table-layout:auto&quot;}

アドビのサードパーティ Cookie の設定を有効にします。SDK は、訪問者 ID をサードパーティのコンテキストで保持し、同じ訪問者 ID をサイト全体で使用できるようにします。 複数のサイトがある場合や、データをパートナーと共有する場合は、このオプションを使用します。ただし、プライバシー上の理由から、このオプションが望ましくない場合があります。
