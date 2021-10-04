---
title: Adobe Experience Platform Web SDK の設定
description: Adobe Experience Platform Web SDK の設定方法について説明します。
seo-description: Learn how to configure the Experience Platform Web SDK
keywords: 設定；設定；SDK；エッジ；Web SDK；設定；edgeConfigId；コンテキスト；Web；デバイス；環境；placeContext;debugEnabled;edgeDomain;orgId;clickCollectionEnabled;onBeforeEventSend;defaultConsent;Web SDK 設定；prehidingStyle;DestinationsEnabled;DestinationsEnabled;idMigrationEnabled;thirdPartyCookiesEnabled;
exl-id: d1e95afc-0b8a-49c0-a20e-e2ab3d657e45
source-git-commit: c0e2d01bd21405f07f4857e1ccf45dd0e4d0f414
workflow-type: tm+mt
source-wordcount: '826'
ht-degree: 40%

---

# Platform Web SDK の設定

SDK の設定は、`configure` コマンドを使用しておこないます。

>[!IMPORTANT]
>
>`configure` は常 ** に、最初に呼び出すコマンドです。

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
>**エッジ設定が Datastreams にリブランドされました。データストリーム ID は設定 ID と同じです。**

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
| コンソールログ | ブラウザーの JavaScript コンソールにデバッグメッセージを表示できるようにします |

{style=&quot;table-layout:auto&quot;}

### `edgeDomain` {#edge-domain}

このフィールドにファーストパーティドメインを入力します。 詳しくは、[ ドキュメント ](https://experienceleague.adobe.com/docs/core-services/interface/ec-cookies/cookies-first-party.html?lang=ja) を参照してください。

ドメインは、www 上の Web サイトの場合の `data.{customerdomain.com}` に似ています。{customerdomain.com}。

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
| Boolean | × | `true` |

{style=&quot;table-layout:auto&quot;}

リンククリックに関連付けられたデータが自動的に収集されるかどうかを示します。 詳しくは、[ 自動リンクトラッキング ](../data-collection/track-links.md#automaticLinkTracking) を参照してください。 ダウンロード属性を含むリンクや、リンクの末尾がファイル拡張子の場合、リンクはダウンロードリンクとしてラベル付けされます。 ダウンロードリンク修飾子は、正規表現で設定できます。 デフォルト値は です。`"\\.(exe|zip|wav|mp3|mov|mpg|avi|wmv|pdf|doc|docx|xls|xlsx|ppt|pptx)$"`

### `onBeforeEventSend`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 関数 | × | () => 未定義 |

{style=&quot;table-layout:auto&quot;}

すべてのイベントに対して呼び出されるコールバックを送信直前に設定します。 フィールド `xdm` を持つオブジェクトがコールバックに送信されます。送信内容を変更するには、`xdm` オブジェクトを変更します。 このコールバック内では、 `xdm` オブジェクトには、イベントコマンドに渡されたデータと、自動的に収集された情報が既に含まれています。 このコールバックのタイミングと例について詳しくは、[イベントのグローバルな変更](tracking-events.md#modifying-events-globally)を参照してください。

## プライバシーオプション

### `defaultConsent` {#default-consent}

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| オブジェクト | × | `"in"` |

{style=&quot;table-layout:auto&quot;}

ユーザーのデフォルトの同意を設定します。この設定は、ユーザーに対して既に同意設定が保存されていない場合に使用します。 他の有効な値は `"pending"` と `"out"` です。 このデフォルト値は、ユーザーのプロファイルに保持されません。 ユーザーのプロファイルは、`setConsent` が呼び出された場合にのみ更新されます。
* `"in"`:この設定を指定した場合、または値を指定しなかった場合、作業はユーザーの同意を得ずに続行されます。
* `"pending"`:この設定を指定すると、ユーザーが同意設定を指定するまで作業はキューに入れられます。
* `"out"`:この設定を指定すると、ユーザーが同意設定を指定するまで作業は破棄されます。ユーザーの環境設定を指定した後、作業を続行するか、ユーザーの環境設定に基づいて中止します。詳しくは、[同意のサポート](../consent/supporting-consent.md)を参照してください。

## パーソナライゼーションオプション

### `prehidingStyle`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| 文字列 | × | なし |

{style=&quot;table-layout:auto&quot;}

パーソナライズされたコンテンツをサーバーから読み込む際に、Web ページのコンテンツ領域を非表示にする CSS スタイル定義を作成するために使用します。このオプションを指定しない場合、SDK は、パーソナライズされたコンテンツの読み込み中に、コンテンツ領域を非表示にしようとしません。その結果、「ちらつき」が発生する可能性があります。

例えば、パーソナライズされたコンテンツがサーバーから読み込まれる間、デフォルトのコンテンツを非表示にしたい ID が `container` の Web ページ上の要素の場合は、次の事前非表示スタイルを使用します。

```javascript
  prehidingStyle: "#container { opacity: 0 !important }"
```

## オーディエンスのオプション

### `cookieDestinationsEnabled`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| Boolean | × | `true` |

{style=&quot;table-layout:auto&quot;}

[!DNL Audience Manager] Cookie の宛先を有効にします。これにより、セグメント認定に基づいて Cookie を設定できます。

### `urlDestinationsEnabled`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| Boolean | × | `true` |

{style=&quot;table-layout:auto&quot;}

[!DNL Audience Manager] URL の宛先を有効にします。これにより、セグメントの選定に基づいて URL を呼び出すことができます。

## ID オプション

### `idMigrationEnabled` {#id-migration-enabled}

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| Boolean | × | `true` |

{style=&quot;table-layout:auto&quot;}

true の場合、SDK は古い AMCV Cookie を読み取って設定します。 このオプションは、サイトの一部で引き続き Visitor.js を使用している間に、Adobe Experience Platform Web SDK の使用に移行する際に役立ちます。 訪問者 API がページで定義されている場合、SDK は訪問者 API に対して ECID に対してクエリを実行します。 このオプションを使用すると、Adobe Experience Platform Web SDK を使用し、同じ ECID を持つページをデュアルタグ付けできます。

### `thirdPartyCookiesEnabled`

| **タイプ** | **必須** | **デフォルト値** |
| -------- | ------------ | ----------------- |
| Boolean | × | `true` |

{style=&quot;table-layout:auto&quot;}

アドビのサードパーティ Cookie の設定を有効にします。SDK は、訪問者 ID をサードパーティのコンテキストで保持し、同じ訪問者 ID をサイト全体で使用できます。 複数のサイトがある場合や、パートナーとデータを共有する場合は、このオプションを使用します。ただし、プライバシー上の理由から、このオプションが望ましくない場合があります。
