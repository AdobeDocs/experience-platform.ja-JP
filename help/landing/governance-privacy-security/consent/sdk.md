---
title: Adobe Experience Platform Web SDKを使用した顧客の同意データの処理
topic-legacy: getting started
description: Adobe Experience Platform Web SDKを統合して、Adobe Experience Platformで顧客の同意データを処理する方法について説明します。
exl-id: 3a53d908-fc61-452b-bec3-af519dfefa41
source-git-commit: 1780e08a9e45fd87891bbb1727c464aadb0e9b31
workflow-type: tm+mt
source-wordcount: '1270'
ht-degree: 1%

---

# Platform Web SDKを統合して、顧客の同意データを処理する

Adobe Experience Platform Web SDKを使用すると、同意管理プラットフォーム(CMP)によって生成された顧客の同意シグナルを取得し、同意変更イベントが発生するたびにAdobe Experience Platformに送信できます。

**SDKは、標準の** CMPとのインターフェイスを提供しません。SDKをWebサイトに統合する方法を決定し、CMPで同意の変更をリッスンして、適切なコマンドを呼び出す必要があります。 このドキュメントでは、CMPをPlatform Web SDKと統合する方法に関する一般的なガイダンスを提供します。

## 前提条件 {#prerequisites}

このチュートリアルは、CMP内で同意データを生成する方法が既に決定済みであることと、Adobe標準またはIAB Transparency and Consent Framework(TCF)2.0標準に準拠する同意フィールドを含むデータセットが作成されていることを前提としています。 このデータセットをまだ作成していない場合は、このガイドに戻る前に、次のチュートリアルを参照してください。

* [データセット標準を使用したAdobeの作成](./adobe/dataset.md)
* [TCF 2.0標準を使用したデータセットの作成](./iab/dataset.md)

このガイドでは、データ収集UIのタグ拡張を使用してSDKを設定するワークフローに従います。 拡張機能を使用しない場合で、スタンドアロンバージョンのSDKをサイトに直接埋め込む場合は、このガイドの代わりに次のドキュメントを参照してください。

* [データストリームの設定](../../../edge/fundamentals/datastreams.md)
* [SDK のインストール](../../../edge/fundamentals/installing-the-sdk.md)
* [SDK の設定 同意コマンド](../../../edge/consent/supporting-consent.md)

このガイドのインストール手順では、タグ拡張機能と、Webアプリケーションでのタグ拡張機能のインストール方法に関する十分な知識が必要です。 詳しくは、次のドキュメントを参照してください。

* [タグの概要](../../../tags/home.md)
* [クイックスタートガイド](../../../tags/quick-start/quick-start.md)
* [公開の概要](../../../tags/ui/publishing/overview.md)

## データストリームの設定

SDKがデータをExperience Platformに送信するには、まずデータストリームを設定する必要があります。 データ収集UIで、左側のナビゲーションから「**[!UICONTROL Datastreams]**」を選択します。

新しいデータストリームを作成するか、編集する既存のデータストリームを選択した後、**[!UICONTROL Adobe Experience Platform]**&#x200B;の横にある切り替えボタンを選択します。 次に、以下に示す値を使用してフォームに入力します。

![](../../images/governance-privacy-security/consent/adobe/sdk/edge-config.png)

| Datastreamフィールド | 値 |
| --- | --- |
| [!UICONTROL サンドボックス] | 必要なストリーミング接続と、データストリームを設定するためのデータセットを含む、プラットフォーム[sandbox](../../../sandboxes/home.md)の名前。 |
| [!UICONTROL ストリーミングインレット] | Experience Platformの有効なストリーミング接続。 既存のストリーミングインレットがない場合は、[ストリーミング接続](../../../ingestion/tutorials/create-streaming-connection-ui.md)の作成に関するチュートリアルを参照してください。 |
| [!UICONTROL イベントデータセット] | SDKを使用してにイベントデータを送信する予定の[!DNL XDM ExperienceEvent]データセット。 Platformデータストリームを作成するにはイベントデータセットを提供する必要がありますが、イベントを介して送信される同意データは、ダウンストリーム実施ワークフローでは保持されないことに注意してください。 |
| [!UICONTROL プロファイルデータセット] | [以前に](#prerequisites)を作成した、顧客の同意フィールドを含む[!DNL Profile]が有効なデータセット。 |

終了したら、画面の下部にある「**[!UICONTROL 保存]**」を選択し、追加のプロンプトに従って設定を完了します。

## Platform Web SDKのインストールと設定

前の節で説明したようにデータストリームを作成したら、最終的にサイトにデプロイするPlatform Web SDK拡張機能を設定する必要があります。 タグプロパティにSDK拡張機能がインストールされていない場合は、左側のナビゲーションで「**[!UICONTROL 拡張機能]**」を選択し、「**[!UICONTROL カタログ]**」タブを選択します。 次に、使用可能な拡張機能のリストで、「Platform SDK拡張機能」の下の「**[!UICONTROL インストール]**」を選択します。

![](../../images/governance-privacy-security/consent/adobe/sdk/install.png)

SDKを設定する際に、「**[!UICONTROL エッジ設定]**」で、前の手順で作成したデータストリームを選択します。

![](../../images/governance-privacy-security/consent/adobe/sdk/config-sdk.png)

「**[!UICONTROL 保存]**」を選択して、拡張機能をインストールします。

### デフォルトの同意を設定するデータ要素の作成

SDK拡張機能がインストールされている場合は、ユーザーのデフォルトのデータ収集同意値(`collect.val`)を表すデータ要素を作成できます。 これは、EUユーザーに`pending`、北米ユーザーに`in`など、ユーザーに応じて異なるデフォルト値を使用する場合に役立ちます。

この使用例では、次の方法を導入して、ユーザーの地域に基づいてデフォルトの同意を設定できます。

1. Webサーバー上のユーザーの地域を特定します。
1. Webページ上の`script`タグ（埋め込みコード）の前に、ユーザーの地域に基づいて`adobeDefaultConsent`変数を設定する別の`script`タグをレンダリングします。
1. `adobeDefaultConsent` JavaScript変数を使用するデータ要素を設定し、このデータ要素をユーザーのデフォルトの同意値として使用します。

ユーザーの領域がCMPによって決定される場合は、次の手順を使用できます。

1. ページ上の「CMP読み込み」イベントを処理します。
1. イベントハンドラーで、ユーザーの地域に基づいて`adobeDefaultConsent`変数を設定し、JavaScriptを使用してタグライブラリスクリプトを読み込みます。
1. `adobeDefaultConsent` JavaScript変数を使用するデータ要素を設定し、このデータ要素をユーザーのデフォルトの同意値として使用します。

データ収集UIでデータ要素を作成するには、左側のナビゲーションで「**[!UICONTROL データ要素]**」を選択し、「**[!UICONTROL データ要素を追加]**」を選択して、データ要素作成ダイアログに移動します。

ここから、`adobeDefaultConsent`に基づく[!UICONTROL JavaScript変数]データ要素を作成する必要があります。 完了したら、「**[!UICONTROL 保存]**」をクリックします。

![](../../images/governance-privacy-security/consent/adobe/sdk/data-element.png)

データ要素を作成したら、 Web SDK拡張機能の設定ページに戻ります。 「[!UICONTROL プライバシー]」セクションで、「**[!UICONTROL データ要素で指定]**」を選択し、指定したダイアログを使用して、作成したデフォルトの同意データ要素を選択します。

![](../../images/governance-privacy-security/consent/adobe/sdk/default-consent.png)

### Webサイトに拡張機能をデプロイする

拡張機能の設定が完了したら、Webサイトに統合できます。 更新されたライブラリビルドのデプロイ方法について詳しくは、タグドキュメントの[パブリッシュガイド](../../../tags/ui/publishing/overview.md)を参照してください。

## 同意変更コマンドの実行 {#commands}

SDK拡張機能をWebサイトに統合したら、 Platform Web SDK `setConsent`コマンドを使用して、Platformに同意データを送信できます。

サイト上で`setConsent`が呼び出されるシナリオは2つあります。

1. 同意がページに読み込まれるとき（つまり、ページの読み込みごとに）
1. 同意設定の変更を検出するCMPフックまたはイベントリスナーの一部として

>[!NOTE]
>
>Platform SDKコマンドの一般的な構文の紹介については、[コマンド](../../../edge/fundamentals/executing-commands.md)の実行に関するドキュメントを参照してください。

`setConsent`コマンドは、次の2つの引数を受け取ります。

1. コマンドの種類を示す文字列（この場合は`"setConsent"`）
1. 単一の配列タイプのプロパティを含むペイロードオブジェクト：`consent`. `consent`配列には、Adobe標準に必要な同意フィールドを提供するオブジェクトを少なくとも1つ含める必要があります。

Adobe標準に必要な同意フィールドを、次の`setConsent`呼び出し例に示します。

```js
alloy("setConsent", {
  consent: [{
    standard: "Adobe",
    version: "2.0",
    value: {
      collect: {
        val: "y"
      },
      share: {
        val: "y"
      },
      personalize: {
        content: {
          val: "y"
        }
      },
      metadata: {
        time: "2020-10-12T15:52:25+00:00"
      }
    }
  }]
});
```

| ペイロードプロパティ | 説明 |
| --- | --- |
| `standard` | 使用される同意標準。 Adobe標準の場合、この値は`Adobe`に設定する必要があります。 |
| `version` | `standard`に示されている同意標準のバージョン番号。 Adobe標準の同意処理を行う場合は、この値を`2.0`に設定する必要があります。 |
| `value` | プロファイルが有効なデータセットの同意フィールドの構造に準拠するXDMオブジェクトとして提供される、顧客の更新された同意情報。 |

>[!NOTE]
>
>`Adobe`（`IAB TCF`など）と組み合わせて他の同意標準を使用する場合は、各標準の`consent`配列にオブジェクトを追加できます。 各オブジェクトには、表す同意基準の`standard`、`version`および`value`に対する適切な値が含まれている必要があります。

次のJavaScriptは、Webサイト上で同意設定の変更を処理する関数の例を示しています。この関数は、イベントリスナーまたはCMPフック内のコールバックとして使用できます。

```js
var setConsent = function () {

  // Retrieve the current consent data.
  var categories = getConsentData();

  // If the script is running on a consent change, generate a new timestamp.
  // If the script is running on page load, set the timestamp to when the consent values last changed.
  var now = new Date();
  var collectedAt = consentChanged ? now.toISOString() : categories.collectedAt;

  //  Map the consent values and timestamp to XDM
  var consentXDM = {
    collect: {
      val: categories.collect !== -1 ? "y" : "n"
    },
    personalize: {
      content: {
        val: categories.personalizeContent !== -1 ? "y" : "n"
      }
    },
    share: {
      val: categories.share !== -1 ? "y" : "n"
    },
    metadata: {
      time: collectedAt
    }
  };

  // Pass the XDM object to the Platform Web SDK
  alloy("setConsent", {
    consent: [{
      standard: "Adobe",
      version: "2.0",
      value: consentXDM
    }]
  });
});
```

## SDK応答の処理

すべての[!DNL Platform SDK]コマンドは、呼び出しが成功したか失敗したかを示すpromiseを返します。 その後、これらの応答を、顧客への確認メッセージの表示などの追加ロジックに使用できます。 具体的な例については、SDKコマンドの実行に関するガイドの[成功または失敗](../../../edge/fundamentals/executing-commands.md#handling-success-or-failure)の処理に関する節を参照してください。

## 次の手順

このガイドに従うことで、Platform Web SDK拡張機能を設定して、同意データをExperience Platformに送信できます。 実装のテストに関するガイダンスについては、実装する同意標準のドキュメントを参照してください。

* [Adobe 標準](./adobe/overview.md#test)
* [TCF 2.0標準](./iab/overview.md#test)
