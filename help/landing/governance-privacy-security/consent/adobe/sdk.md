---
title: Adobe Experience PlatformWeb SDKを使用した顧客の同意データの処理
topic-legacy: getting started
description: Adobe2.0標準を使用して、Adobe Experience PlatformのWeb SDKを統合し、Adobe Experience Platformの顧客の同意データを処理する方法を説明します。
exl-id: 3a53d908-fc61-452b-bec3-af519dfefa41
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1243'
ht-degree: 1%

---

# Adobe2.0標準を使用して顧客の同意データを処理するために、プラットフォームWeb SDKを統合する

Adobe Experience PlatformWeb SDKを使用すると、Consent Management Platforms(CMP)によって生成された顧客の同意シグナルを取得し、同意変更イベントが発生した場合に必ずAdobe Experience Platformに送信できます。

**SDKは、CMPとのインターフェイスを初期状態で行いません**。SDKをWebサイトに統合する方法を決定し、CMPでの同意の変更をリッスンして、適切なコマンドを呼び出す必要があります。 このドキュメントでは、CMPをプラットフォームWeb SDKと統合する方法に関する一般的なガイダンスを提供します。

## 前提条件

このチュートリアルでは、CMP内で同意データを生成する方法を既に決定済みであり、リアルタイム顧客プロファイルに対して有効になっている同意フィールドを含むデータセットを作成済みであることを前提としています。 これらの手順の詳細については、このガイドに戻る前に、Experience Platform](./overview.md)の[同意処理の概要を参照してください。

また、このガイドでは、Adobe Experience Platform Launch拡張機能と、それらがWebアプリケーションにどのようにインストールされるかについて、十分に理解している必要があります。 詳しくは、次のドキュメントを参照してください。

* [platform launchの概要](https://experienceleague.adobe.com/docs/launch/using/home.html?lang=ja)
* [クイックスタートガイド](https://experienceleague.adobe.com/docs/launch/using/get-started/quick-start.html)
* [パブリッシュの概要](https://experienceleague.adobe.com/docs/launch/using/publish/overview.html)

## エッジ設定の設定

SDKがExperience Platformにデータを送信するには、Adobe Experience Platform Launchでプラットフォーム用の既存のエッジ設定を設定する必要があります。 また、設定に対して選択する[!UICONTROL プロファイルデータセット]には、標準化された同意フィールドが含まれている必要があります。

新しい設定を作成した後、または編集する既存の設定を選択した後、**[!UICONTROL Adobe Experience Platform]**&#x200B;の横のトグルボタンを選択します。 次に、以下に示す値を使用してフォームに入力します。

![](../../../images/governance-privacy-security/consent/adobe/sdk/edge-config.png)

| エッジ設定フィールド | 値 |
| --- | --- |
| [!UICONTROL サンドボックス] | エッジ設定を設定するために必要なストリーミング接続とデータセットが含まれている、プラットフォーム[サンドボックス](../../../../sandboxes/home.md)の名前です。 |
| [!UICONTROL ストリーミングインレット] | Experience Platform用の有効なストリーミング接続。 既存のストリーミングインレットがない場合は、[ストリーミング接続](../../../../ingestion/tutorials/create-streaming-connection-ui.md)の作成のチュートリアルを参照してください。 |
| [!UICONTROL イベントデータセット] | SDKを使用してイベントデータを送信する予定の[!DNL XDM ExperienceEvent]データセット。 Platformエッジ設定を作成するにはイベントデータセットを提供する必要がありますが、イベントを介した同意データの直接送信は現在サポートされていないことに注意してください。 |
| [!UICONTROL プロファイルデータセット] | 先ほど作成した顧客の同意フィールドを含む[!DNL Profile]対応のデータセット。 |

完了したら、画面の下部にある「**[!UICONTROL 保存]**」を選択し、追加のプロンプトに従って設定を完了します。


## Platform Web SDK Extensionのインストールと設定

前の節で説明したようにエッジ設定を作成したら、最終的にサイトにデプロイするプラットフォームWeb SDK拡張を設定する必要があります。 SDK拡張機能がPlatform launchのプロパティにインストールされていない場合は、左側のナビゲーションで「**[!UICONTROL 拡張機能]**」を選択し、次に「**[!UICONTROL カタログ]**」タブを選択します。 次に、使用可能な拡張機能のリスト内のPlatform SDK extensionの下で、「**[!UICONTROL Install]**」を選択します。

![](../../../images/governance-privacy-security/consent/adobe/sdk/install.png)

SDKを設定する際に、「**[!UICONTROL Edge Configurations]**」で、前の手順で作成した設定を選択します。

![](../../../images/governance-privacy-security/consent/adobe/sdk/config-sdk.png)

「**[!UICONTROL 保存]**」を選択して、拡張機能をインストールします。

### デフォルトの同意を設定するデータ要素を作成します

SDK拡張機能がインストールされている場合は、ユーザーのデフォルトのデータ収集同意値(`collect.val`)を表すデータ要素を作成するオプションがあります。 これは、ユーザーに応じて異なるデフォルト値を使用したい場合に便利です。例えば、ヨーロッパ和集合ユーザーの場合は`pending`、北米ユーザーの場合は`in`です。

この場合、次を実装して、ユーザーの地域に基づいてデフォルトの同意を設定できます。

1. Webサーバー上のユーザーの地域を特定します。
1. Webページ上のPlatform launchスクリプトタグ（埋め込みコード）の前に、ユーザーの領域に基づいて`adobeDefaultConsent`変数を設定する別のスクリプトタグをレンダリングします。
1. `adobeDefaultConsent` JavaScript変数を使用するデータ要素を設定し、このデータ要素をユーザーのデフォルトの同意値として使用します。

ユーザーの領域がCMPによって決定される場合は、代わりに次の手順を使用できます。

1. ページ上の「CMP読み込み」イベントを処理します。
1. イベントハンドラーで、ユーザーの地域に基づいて`adobeDefaultConsent`変数を設定し、JavaScriptを使用してPlatform launchライブラリスクリプトを読み込みます。
1. `adobeDefaultConsent` JavaScript変数を使用するデータ要素を設定し、このデータ要素をユーザーのデフォルトの同意値として使用します。

platform launchUIでデータ要素を作成するには、左側のナビゲーションで「**[!UICONTROL データ要素]**」を選択し、「**[!UICONTROL 追加データ要素]**」を選択してデータ要素作成ダイアログに移動します。

ここから、`adobeDefaultConsent`に基づいて[!UICONTROL JavaScript変数]データ要素を作成する必要があります。 終了したら「**[!UICONTROL 保存]**」を選択します。

![](../../../images/governance-privacy-security/consent/adobe/sdk/data-element.png)

データ要素が作成されたら、Web SDK拡張機能の設定ページに戻ります。 「[!UICONTROL プライバシー]」セクションで、「**[!UICONTROL 提供元データ要素]**」を選択し、提供されたダイアログを使用して、前に作成したデフォルトの同意データ要素を選択します。

![](../../../images/governance-privacy-security/consent/adobe/sdk/default-consent.png)

### Webサイトでの拡張機能のデプロイ

拡張機能の設定が完了すると、Webサイトに統合できます。 更新されたライブラリビルドの展開方法の詳細については、Platform launchドキュメントの[パブリッシングガイド](https://experienceleague.adobe.com/docs/launch/using/publish/overview.html)を参照してください。

## 同意変更コマンドの作成

SDK拡張機能をWebサイトに統合すると、Platform Web SDK `setConsent`コマンドを使用して開始し、プラットフォームに同意データを送信できます。

サイト上で`setConsent`を呼び出す2つのシナリオがあります。

1. ページに同意が読み込まれた場合（つまり、ページが読み込まれるたび）
1. 同意設定の変更を検出するCMPフックまたはイベントリスナーの一部として

>[!NOTE]
>
>プラットフォームSDKコマンドの一般的な構文の紹介については、[コマンド](../../../../edge/fundamentals/executing-commands.md)の実行に関するドキュメントを参照してください。

`setConsent`コマンドには2つの引数が必要です。

1. コマンドの種類を示す文字列（この場合は`"setConsent"`）
1. 単一の配列タイプのプロパティを含むペイロードオブジェクト。`consent`. `consent`配列には、Adobe標準に必要な同意フィールドを提供する少なくとも1つのオブジェクトが含まれている必要があります。

Adobe標準に必要な同意フィールドは、次の`setConsent`呼び出し例に表示されます。

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
| `standard` | 使用される同意基準。 Adobe標準では、この値を`Adobe`に設定する必要があります。 |
| `version` | `standard`で示された同意基準のバージョン番号。 Adobe標準の同意処理を行うには、この値を`2.0`に設定する必要があります。 |
| `value` | プロファイル対応データセットの同意フィールドの構造に適合するXDMオブジェクトとして提供される、ユーザーの更新された同意情報。 |

>[!NOTE]
>
>`Adobe` （`IAB TCF`など）と共に他の同意標準を使用する場合は、各標準の`consent`配列にオブジェクトを追加できます。 各オブジェクトには、`standard`、`version`、`value`の各値が含まれている必要があります。これらの値は、オブジェクトが表す同意基準に適しています。

次のJavaScriptは、Webサイト上での同意の環境設定の変更を処理する関数の例を示しています。この関数は、イベントリスナーまたはCMPフック内のコールバックとして使用できます。

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

## SDKレスポンスの処理

すべての[!DNL Platform SDK]コマンドは、呼び出しが成功したか失敗したかを示す約束を返します。 その後、これらの応答を、確認メッセージを顧客に表示するなどの追加のロジックに使用できます。 特定の例については、SDKコマンドの実行に関するガイドの[成功または失敗の処理](../../../../edge/fundamentals/executing-commands.md#handling-success-or-failure)に関する節を参照してください。

## 次の手順

このガイドに従って、Experience Platformに同意データを送信するようにPlatform Web SDK Extensionを設定しました。 これで、[導入](./overview.md#test-implementation)をテストする手順について、同意処理の概要に戻ることができます。
