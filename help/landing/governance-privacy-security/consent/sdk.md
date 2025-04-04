---
title: Adobe Experience Platform Web SDKを使用して顧客同意データを処理する
description: Adobe Experience Platform Web SDKを統合して、Adobe Experience Platformで顧客同意データを処理する方法を説明します。
role: Developer
feature: Consent, Web SDK
exl-id: 3a53d908-fc61-452b-bec3-af519dfefa41
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1322'
ht-degree: 2%

---

# Experience Platform Web SDKを統合して顧客の同意データを処理する

Adobe Experience Platform Web SDKを使用すると、Consent Management Platform （CMP）によって生成された顧客の同意シグナルを取得し、同意変更イベントが発生するたびにAdobe Experience Platformに送信できます。

**SDKは、初期設定では、どの CMP ともインターフェイスを行いません**。 SDKを web サイトに統合する方法を決め、CMP で同意の変更をリッスンして、適切なコマンドを呼び出すかどうかは、ユーザー次第です。 このドキュメントでは、CMP をExperience Platform web SDKと統合する方法に関する一般的なガイダンスを提供します。

## 前提条件 {#prerequisites}

このチュートリアルでは、CMP 内で同意データを生成する方法を既に決定しており、Adobe標準または IAB Transparency and Consent Framework （TCF） 2.0 標準に準拠する同意フィールドを含むデータセットを作成していることを前提としています。 このデータセットをまだ作成していない場合は、このガイドに戻る前に次のチュートリアルを参照してください。

* [Adobe標準を使用したデータセットの作成](./adobe/dataset.md)
* [TCF 2.0 標準を使用したデータセットの作成](./iab/dataset.md)

このガイドは、UI でタグ拡張機能を使用してSDKをセットアップするワークフローに従います。 拡張機能を使用せず、スタンドアロンバージョンのSDKをサイトに直接埋め込む場合は、このガイドの代わりに次のドキュメントを参照してください。

* [データストリームの設定](/help/datastreams/overview.md)
* [SDK のインストール](/help/web-sdk/install/overview.md)
* [同意コマンドに対するSDKの設定](/help/web-sdk/commands/configure/defaultconsent.md)

このガイドのインストール手順では、タグ拡張機能と、web アプリケーションへのインストール方法について、実際に理解しておく必要があります。 詳しくは、次のドキュメントを参照してください。

* [タグの概要](/help/tags/home.md)
* [クイックスタートガイド](/help/tags/quick-start/quick-start.md)
* [公開の概要](/help/tags/ui/publishing/overview.md)

## データストリームの設定

SDKがExperience Platformにデータを送信するには、まずデータストリームを設定する必要があります。 データ収集 UI またはExperience Platform UI で、左側のナビゲーションから **[!UICONTROL データストリーム]** を選択します。

新しいデータストリームを作成するか、既存のデータストリームを選択して編集した後、**[!UICONTROL Adobe Experience Platform]** の横にある切り替えボタンを選択します。 次に、以下に示す値を使用してフォームを完成させます。

![](../../images/governance-privacy-security/consent/adobe/sdk/edge-config.png)

| データストリームフィールド | 値 |
| --- | --- |
| [!UICONTROL  サンドボックス ] | データストリームを設定するために必要なストリーミング接続とデータセットを含む、Experience Platform[ サンドボックス ](../../../sandboxes/home.md) の名前。 |
| [!UICONTROL イベントデータセット] | SDKを使用してにイベントデータを送信する予定の [!DNL XDM ExperienceEvent] データセット。 Experience Platform データストリームを作成するにはイベントデータセットを指定する必要がありますが、イベントを介して送信された同意データは、ダウンストリーム実施ワークフローでは順守されないことに注意してください。 |
| [!UICONTROL プロファイルデータセット] | 作成した顧客同意フィールドを持つ [!DNL Profile] 対応データセット [ 以前 ](#prerequisites)。 |

終了したら、画面の下部にある「**[!UICONTROL 保存]**」を選択し、追加のプロンプトに従って続行して設定を完了します。

## Experience Platform Web SDKのインストールと設定

前の節で説明したようにデータストリームを作成したら、最終的にサイトにデプロイするExperience Platform Web SDK拡張機能を設定する必要があります。 タグプロパティにSDK拡張機能がインストールされていない場合は、左側のナビゲーションにある **[!UICONTROL 拡張機能]** を選択してから、「**[!UICONTROL カタログ]**」タブを選択します。 次に、使用可能な拡張機能のリスト内にあるExperience Platform SDK拡張機能の下の「**[!UICONTROL インストール]**」を選択します。

![](../../images/governance-privacy-security/consent/adobe/sdk/install.png)

SDKを設定する際、「**[!UICONTROL Edge設定]**」で、前の手順で作成したデータストリームを選択します。

![](../../images/governance-privacy-security/consent/adobe/sdk/config-sdk.png)

「**[!UICONTROL 保存]**」を選択して、拡張機能をインストールします。

### データ要素を作成してデフォルトの同意を設定

SDK拡張機能がインストールされている状態で、デフォルトのデータ収集同意値（`collect.val`）を表すデータ要素を作成するオプションがあります。 これは、欧州連合（EU）ユーザーの `pending` や北米ユーザーの `in` など、ユーザーに応じて異なるデフォルト値を使用する場合に便利です。

このユースケースでは、次のコードを実装して、ユーザーの地域に基づいてデフォルトの同意を設定できます。

1. Web サーバー上のユーザーの地域を特定します。
1. Web ページの `script` タグ（埋め込みコード）の前に、ユーザーの地域に基づいて `adobeDefaultConsent` 変数を設定する個別の `script` タグをレンダリングします。
1. `adobeDefaultConsent` JavaScript変数を使用するデータ要素を設定し、このデータ要素をユーザーのデフォルトの同意値として使用します。

ユーザーの地域が CMP によって決定される場合は、代わりに次の手順を使用できます。

1. ページ上の「CMP の読み込み」イベントを処理します。
1. イベントハンドラーで、ユーザーのリージョンに基づいて `adobeDefaultConsent` 変数を設定し、JavaScriptを使用してタグライブラリスクリプトを読み込みます。
1. `adobeDefaultConsent` JavaScript変数を使用するデータ要素を設定し、このデータ要素をユーザーのデフォルトの同意値として使用します。

UI でデータ要素を作成するには、左側のナビゲーションで **[!UICONTROL データ要素]** を選択してから、「**[!UICONTROL データ要素の追加]** を選択して、データ要素作成ダイアログに移動します。

ここから、`adobeDefaultConsent` に基づいて [!UICONTROL JavaScript変数 ] データ要素を作成する必要があります。 完了したら、「**[!UICONTROL 保存]**」をクリックします。

![](../../images/governance-privacy-security/consent/adobe/sdk/data-element.png)

データ要素が作成されたら、Web SDK拡張機能の設定ページに戻ります。 「[!UICONTROL  プライバシー ]」セクションで **[!UICONTROL データ要素によって提供]** を選択し、提供されたダイアログを使用して、前に作成したデフォルトの同意データ要素を選択します。

![](../../images/governance-privacy-security/consent/adobe/sdk/default-consent.png)

### Web サイトへの拡張機能のデプロイ

拡張機能の設定が完了したら、web サイトに統合できます。 更新されたライブラリビルドのデプロイ方法について詳しくは、タグのドキュメントの [ 公開ガイド ](../../../tags/ui/publishing/overview.md) を参照してください。

## 同意変更コマンドの実行 {#commands}

SDK拡張機能を web サイトに統合したら、Experience Platform Web SDK `setConsent` コマンドを使用して、同意データをExperience Platformに送信することができます。

`setConsent` コマンドは、次の 2 つのアクションを実行します。

1. プロファイルストア内でユーザーのプロファイル属性を直接更新します。 これは、データレイクにデータを送信しません。
1. 同意変更イベントのタイムスタンプ付きアカウントを記録する [ エクスペリエンスイベント ](../../../xdm/classes/experienceevent.md) を作成します。 このデータはデータレイクに直接送信され、同意の環境設定の変化を経時的に追跡するために使用できます。

### `setConsent` を呼び出すタイミング

サイトで `setConsent` を呼び出す必要があるシナリオは 2 つあります。

1. 同意がページに読み込まれる（つまり、ページが読み込まれるたびに読み込まれる）場合
1. 同意設定の変更を検出する CMP フックまたはイベントリスナーの一部として

### `setConsent` 構文

[`setConsent`](/help/web-sdk/commands/setconsent.md) コマンドは、単一の配列タイプのプロパティ `consent` を含むペイロードオブジェクトを想定しています。 `consent` 配列には、Adobe標準に必要な同意フィールドを提供するオブジェクトが少なくとも 1 つ含まれている必要があります。

次の呼び出しの例では、Adobe標準の必須同意フィールド `setConsent` 示しています。

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
        time: "YYYY-10-12T15:52:25+00:00"
      }
    }
  }]
});
```

| ペイロードプロパティ | 説明 |
| --- | --- |
| `standard` | 使用されている同意標準。 Adobe標準の場合、この値は `Adobe` に設定する必要があります。 |
| `version` | `standard` に示されている同意標準のバージョン番号。 Adobe標準の同意処理では、この値を `2.0` に設定する必要があります。 |
| `value` | 顧客の更新された同意情報（プロファイル対応データセットの同意フィールドの構造に準拠する XDM オブジェクトとして提供）。 |

>[!NOTE]
>
>`Adobe` （`IAB TCF` など）と他の同意標準を組み合わせて使用している場合は、各標準の `consent` 配列にオブジェクトを追加できます。 各オブジェクトには、表す同意標準の `standard`、`version`、`value` に適した値が含まれている必要があります。

次のJavaScriptは、web サイト上の同意環境設定の変更を処理する関数の例を示しています。この関数は、イベントリスナーまたは CMP フック内のコールバックとして使用できます。

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

  // Pass the XDM object to the Experience Platform Web SDK
  alloy("setConsent", {
    consent: [{
      standard: "Adobe",
      version: "2.0",
      value: consentXDM
    }]
  });
});
```

## SDKの応答の処理

すべての [!DNL Experience Platform SDK] コマンドは、呼び出しが成功したか失敗したかを示すプロミスを返します。 その後、これらの応答を使用して、顧客に確認メッセージを表示するなどの追加のロジックを実行できます。 詳細については、「[ コマンドの応答 ](/help/web-sdk/commands/command-responses.md)」を参照してください。

SDKで `setConsent` 呼び出しを正常に実行したら、Experience Platform UI のプロファイルビューアを使用して、データがプロファイルストアにランディングされているかどうかを確認できます。 詳しくは、[ID によるプロファイルの参照 ](../../../profile/ui/user-guide.md#browse-identity) の節を参照してください。

## 次の手順

このガイドに従って、同意データをExperience Platformに送信するようにExperience Platform Web SDK拡張機能を設定しました。 実装のテストに関するガイダンスについては、実装している同意標準のドキュメントを参照してください。

* [Adobe標準](./adobe/overview.md#test)
* [TCF 2.0 規格](./iab/overview.md#test)
