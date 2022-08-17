---
title: Adobe Experience Platform Web SDK を使用した顧客の同意データの処理
topic-legacy: getting started
description: Adobe Experience Platform Web SDK を統合して、Adobe Experience Platformで顧客の同意データを処理する方法について説明します。
exl-id: 3a53d908-fc61-452b-bec3-af519dfefa41
source-git-commit: 79bc41c713425e14bb3c12646b9b71b2c630618b
workflow-type: tm+mt
source-wordcount: '1375'
ht-degree: 2%

---

# Platform Web SDK を統合して、顧客の同意データを処理する

Adobe Experience Platform Web SDK を使用すると、同意管理プラットフォーム (CMP) で生成された顧客の同意シグナルを取得し、同意変更イベントが発生した場合にそれらをAdobe Experience Platformに送信できます。

**SDK は、標準の CMP とのインターフェイスを提供しません**. SDK を Web サイトに統合する方法を決定し、CMP で同意の変更をリッスンして、適切なコマンドを呼び出すかどうかは、ユーザーが決定します。 このドキュメントでは、CMP を Platform Web SDK と統合する方法に関する一般的なガイダンスを提供します。

## 前提条件 {#prerequisites}

このチュートリアルは、CMP 内で同意データを生成する方法が既に決定済みであり、Adobe標準または IAB Transparency and Consent Framework(TCF)2.0 標準に準拠する同意フィールドを含むデータセットが作成されていることを前提としています。 このデータセットをまだ作成していない場合は、このガイドに戻る前に、次のチュートリアルを参照してください。

* [Adobe標準を使用したデータセットの作成](./adobe/dataset.md)
* [TCF 2.0 標準を使用したデータセットの作成](./iab/dataset.md)

このガイドでは、データ収集 UI のタグ拡張機能を使用した SDK のセットアップワークフローに従います。 拡張機能を使用しない場合で、スタンドアロンバージョンの SDK をサイトに直接埋め込む場合は、このガイドではなく、次のドキュメントを参照してください。

* [データストリームの設定](../../../edge/datastreams/overview.md)
* [SDK のインストール](../../../edge/fundamentals/installing-the-sdk.md)
* [SDK の設定 同意コマンド用](../../../edge/consent/supporting-consent.md)

このガイドのインストール手順では、タグの拡張機能と、Web アプリケーションでのタグのインストール方法に関する十分な知識が必要です。 詳しくは、次のドキュメントを参照してください。

* [タグの概要](../../../tags/home.md)
* [クイックスタートガイド](../../../tags/quick-start/quick-start.md)
* [公開の概要](../../../tags/ui/publishing/overview.md)

## データストリームの設定

SDK がデータをExperience Platformに送信するには、まずデータストリームを設定する必要があります。 データ収集 UI で、「 」を選択します。 **[!UICONTROL データストリーム]** をクリックします。

新しいデータストリームを作成するか、編集する既存のデータストリームを選択した後、の横にある切り替えボタンを選択します。 **[!UICONTROL Adobe Experience Platform]**. 次に、以下に示す値を使用して、フォームに入力します。

![](../../images/governance-privacy-security/consent/adobe/sdk/edge-config.png)

| データストリームフィールド | 値 |
| --- | --- |
| [!UICONTROL サンドボックス] | プラットフォームの名前 [サンドボックス](../../../sandboxes/home.md) データストリームを設定するために必要なストリーミング接続とデータセットを含む |
| [!UICONTROL ストリーミングインレット] | 有効なストリーミングExperience Platform。 に関するチュートリアルを参照してください。 [ストリーミング接続の作成](../../../ingestion/tutorials/create-streaming-connection-ui.md) 既存のストリーミングインレットがない場合。 |
| [!UICONTROL イベントデータセット] | An [!DNL XDM ExperienceEvent] SDK を使用してにイベントデータを送信する予定のデータセット。 Platform データストリームを作成するにはイベントデータセットを提供する必要がありますが、イベントを介して送信される同意データは、ダウンストリーム実施ワークフローでは受け入れられないことに注意してください。 |
| [!UICONTROL プロファイルデータセット] | この [!DNL Profile]作成した顧客の同意フィールドを含む有効なデータセット [以前](#prerequisites). |

終了したら、「 」を選択します。 **[!UICONTROL 保存]** 画面の下部で、追加のプロンプトに従って設定を完了します。

## Platform Web SDK のインストールと設定

前の節で説明したようにデータストリームを作成したら、最終的にサイトにデプロイする Platform Web SDK 拡張機能を設定する必要があります。 タグプロパティに SDK 拡張機能がインストールされていない場合は、「 **[!UICONTROL 拡張機能]** 左のナビゲーションで、 **[!UICONTROL カタログ]** タブをクリックします。 次に、 **[!UICONTROL インストール]** をクリックします。

![](../../images/governance-privacy-security/consent/adobe/sdk/install.png)

SDK を設定する際に、 **[!UICONTROL エッジ設定]**」で、前の手順で作成したデータストリームを選択します。

![](../../images/governance-privacy-security/consent/adobe/sdk/config-sdk.png)

選択 **[!UICONTROL 保存]** をクリックして、拡張機能をインストールします。

### デフォルトの同意を設定するデータ要素の作成

SDK 拡張機能がインストールされている場合、デフォルトのデータ収集同意値 (`collect.val`) を使用します。 これは、ユーザーに応じて異なるデフォルト値 ( `pending` （EU ユーザー用）および `in` （北米のユーザー向け）

この使用例では、次の機能を実装して、ユーザーの地域に基づいてデフォルトの同意を設定できます。

1. Web サーバー上のユーザーの地域を特定します。
1. の前 `script` タグ（埋め込みコード）を Web ページ上で設定し、 `script` タグを設定します。 `adobeDefaultConsent` 変数の値を指定します。
1. を使用するデータ要素を設定する `adobeDefaultConsent` JavaScript 変数を使用し、このデータ要素をユーザーのデフォルトの同意値として使用します。

ユーザーの領域が CMP によって決定される場合は、代わりに次の手順を使用できます。

1. ページ上の「CMP loaded」イベントを処理します。
1. イベントハンドラで、 `adobeDefaultConsent` 変数を読み込み、JavaScript を使用してタグライブラリスクリプトを読み込みます。
1. を使用するデータ要素を設定する `adobeDefaultConsent` JavaScript 変数を使用し、このデータ要素をユーザーのデフォルトの同意値として使用します。

データ収集 UI でデータ要素を作成するには、「 **[!UICONTROL データ要素]** 左側のナビゲーションで、「 **[!UICONTROL データ要素を追加]** をクリックして、データ要素作成ダイアログに移動します。

ここから、 [!UICONTROL JavaScript 変数] に基づくデータ要素 `adobeDefaultConsent`. 完了したら、「**[!UICONTROL 保存]**」をクリックします。

![](../../images/governance-privacy-security/consent/adobe/sdk/data-element.png)

データ要素を作成したら、 Web SDK 拡張機能の設定ページに戻ります。 以下 [!UICONTROL プライバシー] セクション、選択 **[!UICONTROL データ要素によって指定]**&#x200B;を選択し、指定したダイアログを使用して、前に作成したデフォルトの同意データ要素を選択します。

![](../../images/governance-privacy-security/consent/adobe/sdk/default-consent.png)

### Web サイトに拡張機能をデプロイする

拡張機能の設定が完了したら、Web サイトに統合できます。 詳しくは、 [公開ガイド](../../../tags/ui/publishing/overview.md) タグに関するドキュメントを参照してください。

## 同意変更コマンドの実行 {#commands}

SDK 拡張機能を Web サイトに統合したら、Platform Web SDK の使用を開始できます `setConsent` 」コマンドを使用して、Platform に同意データを送信する必要があります。

この `setConsent` コマンドは、次の 2 つのアクションを実行します。

1. ユーザーのプロファイル属性をプロファイルストアで直接更新します。 データレイクにはデータは送信されません。
1. を [エクスペリエンスイベント](../../../xdm/classes/experienceevent.md) には、同意の変更イベントのタイムスタンプ付きのアカウントが記録されます。 このデータはデータレイクに直接送信され、経時的に同意設定の変更を追跡するために使用できます。

### を呼び出すタイミング `setConsent`

次の 2 つのシナリオが考えられます。 `setConsent` サイトでを呼び出す必要があります。

1. 同意がページに読み込まれた場合（つまり、ページが読み込まれるたび）
1. 同意設定の変更を検出する CMP フックまたはイベントリスナーの一部として

### `setConsent` 構文

>[!NOTE]
>
>Platform SDK コマンドの一般的な構文の概要については、 [コマンドの実行](../../../edge/fundamentals/executing-commands.md).

この `setConsent` コマンドには次の 2 つの引数が必要です。

1. コマンドの種類を示す文字列 ( この場合は `"setConsent"`)
1. 単一の配列タイププロパティを含むペイロードオブジェクト： `consent`. この `consent` 配列には、Adobe標準に必要な同意フィールドを提供するオブジェクトが少なくとも 1 つ含まれている必要があります。

次の例に、Adobe標準に必要な同意フィールドを示します `setConsent` 呼び出し：

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
| `standard` | 使用される同意の規格。 Adobe標準の場合、この値をに設定する必要があります。 `Adobe`. |
| `version` | 以下に示す同意標準のバージョン番号 `standard`. この値はに設定する必要があります。 `2.0` Adobe標準の同意処理用。 |
| `value` | プロファイルが有効なデータセットの同意フィールドの構造に準拠する XDM オブジェクトとして提供される、顧客の更新された同意情報。 |

>[!NOTE]
>
>他の同意標準を `Adobe` ( `IAB TCF`を参照 )、 `consent` 配列を作成します。 各オブジェクトには、 `standard`, `version`、および `value` 彼らが表す同意基準のために

次の JavaScript は、Web サイトで同意設定の変更を処理する関数の例を示しています。この関数は、イベントリスナーまたは CMP フック内のコールバックとして使用できます。

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

## SDK 応答の処理

すべて [!DNL Platform SDK] コマンドは、呼び出しが成功したか失敗したかを示す promise を返します。 その後、これらの応答を、顧客への確認メッセージの表示などの追加ロジックに使用できます。 詳しくは、 [成功または失敗の処理](../../../edge/fundamentals/executing-commands.md#handling-success-or-failure) （特定の例に関する SDK コマンドの実行に関するガイド）。

正常に `setConsent` SDK を呼び出す場合、Platform UI のプロファイルビューアを使用して、データがプロファイルストアにランディングしているかどうかを検証できます。 詳しくは、 [ID によるプロファイルの参照](../../../profile/ui/user-guide.md#browse-identity) を参照してください。

## 次の手順

このガイドに従うことで、同意データをExperience Platformに送信するように Platform Web SDK 拡張機能を設定しました。 実装のテストに関するガイダンスについては、実装する同意標準のドキュメントを参照してください。

* [Adobe 標準](./adobe/overview.md#test)
* [TCF 2.0 標準](./iab/overview.md#test)
