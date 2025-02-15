---
title: Web SDK タグ拡張機能の設定
description: タグ UI でExperience Platform Web SDK タグ拡張機能を設定する方法について説明します。
exl-id: 22425daa-10bd-4f06-92de-dff9f48ef16e
source-git-commit: f2f61c8e68fa794317e3b4f845f1950cebc59ec7
workflow-type: tm+mt
source-wordcount: '2525'
ht-degree: 4%

---

# Web SDK 拡張機能の設定

[!DNL Web SDK] タグ拡張機能は、web プロパティからExperience PlatformEdge Networkを介してAdobe Experience Cloudにデータを送信します。

拡張機能を使用すると、データを Platform にストリーミングし、ID を同期し、顧客同意シグナルを処理して、コンテキストデータを自動的に収集できます。

このドキュメントでは、タグ UI でタグ拡張機能を設定する方法について説明します。

## Web SDK タグ拡張機能のインストール {#install}

Web SDK タグ拡張機能には、プロパティをインストールする必要があります。 まだ行っていない場合は、[ タグプロパティの作成 ](https://experienceleague.adobe.com/docs/platform-learn/implement-in-websites/configure-tags/create-a-property.html?lang=ja) に関するドキュメントを参照してください。

プロパティを作成したら、プロパティを開き、左側のバーにある **[!UICONTROL 拡張機能]** タブを選択します。

「**[!UICONTROL カタログ]**」タブを選択します。 使用可能な拡張機能のリストから、[!DNL Web SDK] の拡張機能を見つけて **[!UICONTROL インストール]** を選択します。

![Web SDK 拡張機能が選択されたタグ UI を示す画像 ](assets/web-sdk-install.png)

**[!UICONTROL インストール]** を選択した後、Web SDK タグ拡張機能を設定し、設定を保存する必要があります。

>[!NOTE]
>
>タグ拡張機能は、設定を保存した後にのみインストールされます。 タグ拡張機能の設定方法については、次の節を参照してください。

## インスタンス設定を指定 {#general}

ページ上部の設定オプションは、データのルーティング先と、サーバーで使用する設定をAdobe Experience Platformに指示します。

![ タグ UI の Web SDK タグ拡張機能の一般設定を示す画像 ](assets/web-sdk-ext-general.png)

* **[!UICONTROL 名前]**:Adobe Experience Platform Web SDK 拡張機能は、ページ上の複数のインスタンスをサポートします。 この名前は、タグ設定を持つ複数の組織にデータを送信する場合に使用します。 インスタンス名のデフォルトは `alloy` です。 ただし、インスタンス名を任意の有効なJavaScript オブジェクト名に変更できます。
* **[!UICONTROL IMS 組織 ID]**:Adobe時にデータを送信する組織の ID。 ほとんどの場合、自動入力されるデフォルト値を使用します。 ページ上に複数のインスタンスがある場合、データの送信先の 2 番目の組織の値をこのフィールドに入力します。
* **[!UICONTROL Edge ドメイン]**：拡張機能がデータを送受信するドメイン。 Adobeでは、この拡張機能にファーストパーティドメイン（CNAME）を使用することをお勧めします。 デフォルトのサードパーティドメインは開発環境で使用できますが、実稼動環境には適していません。ファーストパーティ CNAME の設定方法については、[ここ](https://experienceleague.adobe.com/docs/core-services/interface/ec-cookies/cookies-first-party.html?lang=ja)で説明します。

## データストリーム設定の指定 {#datastreams}

このセクションでは、使用可能な 3 つの環境（実稼働、ステージング、開発）のそれぞれに使用するデータストリームを選択できます。

リクエストがサーバーに送信されると、Edge Network側の設定を参照するために、データストリーム ID が使用されます。 Web サイト上でコードを変更することなく、設定を更新できます。

データストリームの設定方法については、[ データストリーム ](../../../../datastreams/overview.md) に関するガイドを参照してください。

使用可能なドロップダウンメニューからデータストリームを選択するか、**[!UICONTROL 値を入力]** を選択して、各環境のカスタムデータストリーム ID を入力できます。

![ タグ UI の Web SDK タグ拡張機能のデータストリーム設定を示す画像 ](assets/web-sdk-ext-datastreams.png)

## プライバシー設定の指定 {#privacy}

この節では、Web SDK が web サイトからのユーザー同意シグナルをどのように処理するかを設定できます。 特に、他の明示的な同意環境設定が指定されていない場合にユーザーが想定されるデフォルトの同意レベルを選択できます。

デフォルトの同意レベルは、ユーザープロファイルに保存されません。

![ タグ UI の Web SDK タグ拡張機能のプライバシー設定を示す画像 ](assets/web-sdk-ext-privacy.png)

| [!UICONTROL  デフォルトの同意レベル ] | 説明 |
| --- | --- |
| [!UICONTROL  イン ] | ユーザーが同意環境設定を指定する前に発生するイベントを収集します。 |
| [!UICONTROL  アウト ] | ユーザーが同意環境設定を指定する前に発生するイベントを破棄します。 |
| [!UICONTROL 保留中] | ユーザーが同意環境設定を指定する前に発生するイベントをキューに追加します。 同意の環境設定が指定されると、指定された環境設定に応じてイベントが収集または破棄されます。 |
| [!UICONTROL  データ要素によって提供 ] | デフォルトの同意レベルは、定義した別のデータ要素によって決まります。 このオプションを使用する場合は、提供されたドロップダウンメニューを使用してデータ要素を指定する必要があります。 |

>[!TIP]
>
>ビジネス運営に明示的なユーザー同意が必要な場合は、「**[!UICONTROL アウト]**」または **[!UICONTROL 保留中]** を使用します。

## ID 設定の指定 {#identity}

この節では、ユーザー識別の処理に関する Web SDK の動作を定義できます。

![ タグ UI の Web SDK タグ拡張機能の ID 設定を示す画像 ](assets/web-sdk-ext-identity.png)

* **[!UICONTROL VisitorAPI から ECID を移行]**：このオプションはデフォルトで有効になっています。 この機能が有効な場合、SDK では、`AMCV` および `s_ecid` Cookie を読み取り、[!DNL Visitor.js] で使用される `AMCV` Cookie を設定できます。 この機能は、一部のページがまだ [!DNL Visitor.js] を使用している可能性があるので、Web SDK に移行する際に重要です。 このオプションを使用すると、SDK は引き続き同じ [!DNL ECID] を使用できるので、ユーザーが 2 つの異なるユーザーとして識別されることはありません。
* **[!UICONTROL サードパーティ cookie を使用]**：このオプションを有効にすると、Web SDK はユーザー識別子をサードパーティ cookie に保存しようとします。 成功した場合、ユーザーは、各ドメインで個別のユーザーとして識別されるのではなく、複数のドメインを移動する際に単一のユーザーとして識別されます。 このオプションが有効になっている場合、ブラウザーがサードパーティ cookie をサポートしていない場合や、ユーザーによってサードパーティ cookie を許可しないように設定されている場合には、SDK はサードパーティ cookie にユーザー識別子を保存できない可能性があります。 この場合、SDK はファーストパーティドメインにのみ識別子を保存します。

  >[!IMPORTANT]
  >>サードパーティ cookie は、Web SDK の [ ファーストパーティデバイス ID](../../../../web-sdk/identity/first-party-device-ids.md) 機能と互換性がありません。
ファーストパーティデバイス ID またはサードパーティ Cookie のいずれかを使用できますが、両方の機能を同時に使用することはできません。
  >
## パーソナライゼーション設定の指定 {#personalization}

このセクションでは、パーソナライズされたコンテンツの読み込み中にページの特定の部分を非表示にする方法を設定できます。 これにより、訪問者にはパーソナライズされたページのみが表示されます。

![ タグ UI の Web SDK タグ拡張機能のパーソナライゼーション設定を示す画像 ](assets/web-sdk-ext-personalization.png)

* **[!UICONTROL Target を at.js から Web SDK に移行]**：このオプションを使用すると、at.js `1.x` または `2.x` ライブラリで使用されるレガシー `mbox` および `mboxEdgeCluster` Cookie の読み取りと書き込みが [!DNL Web SDK] ーザーに対して有効になります。 これにより、訪問者プロファイルを維持しながら、Web SDK を使用するページから at.js `1.x` または `2.x` ライブラリを使用するページに（またはその逆に）移行できます。

### スタイルの事前非表示 {#prehiding-style}

事前非表示のスタイルエディターでは、ページの特定のセクションを非表示にするカスタム CSS ルールを定義できます。 ページが読み込まれると、Web SDK はこのスタイルを使用して、パーソナライズの必要なセクションを非表示にし、パーソナライゼーションを取得した後、パーソナライズされたページセクションの非表示を解除します。 これにより、訪問者には、パーソナライゼーション取得プロセスが表示されずに、既にパーソナライズされたページが表示されます。

### 事前非表示のスニペット {#prehiding-snippet}

事前非表示スニペットは、Web SDK ライブラリが非同期で読み込まれる際に役立ちます。 この場合、ちらつきを避けるために、Web SDK ライブラリが読み込まれる前にコンテンツを非表示にすることをお勧めします。

事前非表示のスニペットを使用するには、コピーしてページの `<head>` 要素内に貼り付けます。

>[!IMPORTANT]
>
事前非表示スニペットを使用する場合、Adobeでは [ 事前非表示スタイル ](#prehiding-style) で使用したのと同じ [!DNL CSS] ルールを使用することをお勧めします。

## データ収集設定の指定 {#data-collection}

データ収集設定を管理します。 [`configure`](/help/web-sdk/commands/configure/overview.md) コマンドを使用すると、JavaScript ライブラリの同様の設定を利用できます。

![ タグ UI の Web SDK タグ拡張機能のデータ収集設定を示す画像。](assets/web-sdk-ext-collection.png)

* **[!UICONTROL On before event send callback]**: Adobeに送信されたペイロードを評価して変更するコールバック関数。 コールバック関数内の `content` 変数を使用して、ペイロードを変更します。 このコールバックは、JavaScript ライブラリの [`onBeforeEventSend`](/help/web-sdk/commands/configure/onbeforeeventsend.md) と同等のタグです。
* **[!UICONTROL 内部リンククリック数を収集]**：サイトまたはプロパティ内部のリンクトラッキングデータの収集を有効にするチェックボックス。 このチェックボックスを有効にすると、イベントのグループ化オプションが表示されます。
   * **[!UICONTROL イベントのグループ化なし]**：リンクトラッキングデータは、別のイベントでAdobeに送信されます。 別々のイベントで送信されるリンククリック数は、Adobe Experience Platformに送信されるデータの契約上の使用を増やす可能性があります。
   * **[!UICONTROL セッションストレージを使用したイベントのグループ化]**：次のページイベントまで、リンクトラッキングデータをセッションストレージに保存します。 次のページでは、保存されたリンクトラッキングデータとページビューデータが同時にAdobeに送信されます。 Adobeでは、内部リンクをトラッキングする場合にこの設定を有効にすることをお勧めします。
   * **[!UICONTROL ローカルオブジェクトを使用したイベントのグループ化]**：次のページイベントまで、リンクトラッキングデータをローカルオブジェクトに保存します。 訪問者が新しいページに移動すると、リンクトラッキングデータが失われます。 この設定は、単一ページアプリケーションのコンテキストで最も役立ちます。
* **[!UICONTROL 外部リンククリック数を収集]**：外部リンクの収集を有効にするチェックボックス。
* **[!UICONTROL ダウンロードリンクのクリック数を収集]**：ダウンロードリンクの収集を有効にするチェックボックス。
* **[!UICONTROL ダウンロードリンク修飾子]**：リンク URL をダウンロードリンクとして認定する正規表現。
* **[!UICONTROL クリックのプロパティをフィルター]**：コレクション前にクリック関連のプロパティを評価および変更するコールバック関数。 この関数は、イベント送信コールバックの前 [!UICONTROL On] に実行されます。
* **コンテキスト設定**：特定の XDM フィールドに値を入力する訪問者情報を自動的に収集します。 **[!UICONTROL すべてのデフォルトのコンテキスト情報]** または **[!UICONTROL 特定のコンテキスト情報]** を選択できます。 これは、JavaScript ライブラリの [`context`](/help/web-sdk/commands/configure/context.md) と同等のタグです。
   * **[!UICONTROL Web]**：現在のページに関する情報を収集します。
   * **[!UICONTROL デバイス]**：ユーザーのデバイスに関する情報を収集します。
   * **[!UICONTROL 環境]**：ユーザーのブラウザーに関する情報を収集します。
   * **[!UICONTROL 場所のコンテキスト]**：ユーザーの場所に関する情報を収集します。
   * **[!UICONTROL 高エントロピーのユーザーエージェントヒント]**：ユーザーのデバイスに関するより詳細な情報を収集します。

>[!TIP]
>
**[!UICONTROL リンククリックの前にオン送信]** フィールドは、既に設定されているプロパティにのみ表示される非推奨のコールバックです。 これは、JavaScript ライブラリの [`onBeforeLinkClickSend`](/help/web-sdk/commands/configure/onbeforelinkclicksend.md) と同等のタグです。 **[!UICONTROL フィルタークリックプロパティ]** コールバックを使用してクリックデータをフィルタリングまたは調整するか、**[!UICONTROL On before event send コールバック]** を使用して、Adobeに送信されるペイロード全体をフィルタリングまたは調整します。 **[!UICONTROL フィルタークリックのプロパティ]** コールバックと **[!UICONTROL リンククリックの送信前にオン]** コールバックの両方が設定されている場合、**[!UICONTROL フィルタークリックのプロパティ]** コールバックのみが実行されます。

## メディアコレクション設定の指定 {#media-collection}

メディア収集機能は、web サイト上のメディアセッションに関連するデータを収集するのに役立ちます。

収集されたデータには、メディアプレイバック、一時停止、完了およびその他の関連イベントに関する情報を含めることができます。 収集したら、このデータをAdobe Experience PlatformやAdobe Analyticsに送信して、レポートを生成できます。 この機能は、web サイトでのメディア消費行動を追跡および把握するための包括的なソリューションを提供します。

![ タグ UI の Web SDK タグ拡張機能のメディアコレクション設定を示す画像 ](assets/media-collection.png)


* **[!UICONTROL Channel]**：メディアコレクションが実行されるチャネルの名前。 例：`Video channel`。
* **[!UICONTROL Player Name]**：メディアプレーヤーの名前。
* **[!UICONTROL アプリケーションのバージョン]**：メディアプレーヤーアプリケーションのバージョン。
* **[!UICONTROL メイン ping 間隔]**：メインコンテンツに対する ping の頻度（秒単位）。 デフォルト値は `10` です。値の範囲は `10` ～ `50` 秒です。  値を指定しない場合、[ 自動的にトラッキングされるセッション ](../../../../web-sdk/commands/createmediasession.md#automatic) を使用するときにデフォルト値が使用されます。
* **[!UICONTROL 広告 ping 間隔]**：広告コンテンツに対する ping の頻度（秒単位）。 デフォルト値は `10` です。値の範囲は `1` ～ `10` 秒です。 値を指定しない場合、[ 自動的にトラッキングされるセッション ](../../../../web-sdk/commands/createmediasession.md#automatic) を使用するときにデフォルト値が使用されます

## データストリームの上書きの設定 {#datastream-overrides}

データストリームの上書きを使用すると、Web SDK を介して Edge Network に渡されるデータストリームの追加設定を定義できます。

これにより、新しいデータストリームを作成したり、既存の設定を変更したりすることなく、デフォルトとは異なるデータストリームの動作をトリガーできます。

データストリーム設定の上書きは、次の 2 つの手順で構成されます。

1. 最初に、[データストリーム設定ページ](/help/datastreams/configure.md)でデータストリーム設定の上書きを定義する必要があります。
2. 次に、Web SDK コマンドまたは Web SDK タグ拡張機能を使用して、上書きをEdge Networkに送信する必要があります。

データストリーム設定を上書きする方法について詳しくは、データストリーム [ 設定の上書きドキュメント ](/help/datastreams/overrides.md) を参照してください。

Web SDK コマンドを使用して上書きを渡す代わりに、以下に示すタグ拡張機能画面で上書きを設定できます。

>[!IMPORTANT]
>
データストリームの上書きは、環境ごとに設定する必要があります。 開発環境、ステージング環境および実稼動環境では、すべて別々のオーバーライドが行われます。 次の画面に示す専用オプションを使用して、設定間でコピーできます。

![Web SDK タグ拡張機能ページを使用したデータストリーム設定の上書きを示す画像。](assets/datastream-overrides.png)

デフォルトでは、データストリーム設定の上書きは無効になっています。 「**[!UICONTROL データストリーム設定を一致させる]**」オプションがデフォルトで選択されています。

![ データストリーム設定がデフォルト設定を上書きすることを示す Web SDK タグ拡張機能ユーザーインターフェイス ](assets/datastream-override-default.png)

タグ拡張機能でデータストリームの上書きを有効にするには、ドロップダウンメニューから「**[!UICONTROL 有効]**」を選択します。

![ データストリーム設定の上書きの有効設定を示す Web SDK タグ拡張機能ユーザーインターフェイス。](assets/datastream-override-enabled.png)

データストリーム設定の上書きを有効にすると、以下に説明する各サービスの上書きを設定できます。

以下のデータストリーム優先設定は、選択した環境のサーバーサイドのデータストリーム設定およびルールを上書きします。

### Adobe Analytics {#analytics}

このセクションの設定を使用して、Adobe Analytics サービスへのデータルーティングを上書きします。

![Adobe Analytics データストリーム上書き設定を示す Web SDK タグ拡張機能 UI 画像。](assets/datastream-override-analytics.png)

* **[!UICONTROL 有効]**/**[!UICONTROL 無効]**：このドロップダウンメニューを使用して、Adobe Analytics サービスへのデータルーティングを有効または無効にします。
* **[!UICONTROL レポートスイート]**:Adobe Analyticsの宛先レポートスイートの ID。 値は、データストリーム設定から事前設定された上書きレポートスイート（またはレポートスイートのコンマ区切りリスト）である必要があります。 この設定は、プライマリレポートスイートを上書きします。
* **[!UICONTROL レポートスイートを追加]**：レポートスイートを追加するには、このオプションを選択します。

### Adobe Audience Manager {#audience-manager}

このセクションの設定を使用して、Adobe Audience Manager サービスへのデータルーティングを上書きします。

![Adobe Audience Manager データストリーム上書き設定を示す Web SDK タグ拡張機能 UI 画像。](assets/datastream-override-audience-manager.png)

* **[!UICONTROL 有効]**/**[!UICONTROL 無効]**：このドロップダウンメニューを使用して、Adobe Audience Manager サービスへのデータルーティングを有効または無効にします。
* **[!UICONTROL サードパーティ ID 同期コンテナ]**:Audience Manager内の宛先サードパーティ ID 同期コンテナの ID。 この値は、データストリーム設定から事前設定されたセカンダリコンテナである必要があり、プライマリコンテナを上書きします。

### Adobe Experience Platform {#experience-platform}

このセクションの設定を使用して、Adobe Experience Platform サービスへのデータルーティングを上書きします。

![Adobe Experience Platform データストリーム上書き設定を示す Web SDK タグ拡張機能 UI 画像。](assets/datastream-override-experience-platform.png)

* **[!UICONTROL 有効]**/**[!UICONTROL 無効]**：このドロップダウンメニューを使用して、Adobe Experience Platform サービスへのデータルーティングを有効または無効にします。
* **[!UICONTROL イベントデータセット]**:Adobe Experience Platformの宛先イベントデータセットの ID。 値は、データストリーム設定の事前設定済みセカンダリデータセットである必要があります。
* **[!UICONTROL Offer decisioning]**：このドロップダウン メニューを使用して、[!DNL Offer Decisioning] サービスへのデータ ルーティングを有効または無効にします。
* **[!UICONTROL Edge セグメント化]**：このドロップダウンメニューを使用して、[!DNL Edge Segmentation] サービスへのデータルーティングを有効または無効にします。
* **[!UICONTROL Personalizationの宛先]**：このドロップダウンメニューを使用して、パーソナライゼーションの宛先へのデータルーティングを有効または無効にします。
* **[!UICONTROL Adobe Journey Optimizer]**：このドロップダウン メニューを使用して、[!DNL Adobe Journey Optimizer] サービスへのデータ ルーティングを有効または無効にします。

### Adobeサーバーサイドイベント転送 {#ssf}

このセクションの設定を使用して、Adobeサーバーサイドイベント転送サービスへのデータルーティングを上書きします。

![Adobeサーバーサイドイベント転送データストリーム上書き設定を示す Web SDK タグ拡張機能 UI 画像。](assets/datastream-override-ssf.png)

* **[!UICONTROL 有効]**/**[!UICONTROL 無効]**：このドロップダウンメニューを使用して、Adobeサーバーサイドイベント転送サービスへのデータルーティングを有効または無効にします。

### Adobe Target {#target}

このセクションの設定を使用して、Adobe Target サービスへのデータルーティングを上書きします。

![Adobe Target データストリーム上書き設定を示す Web SDK タグ拡張機能 UI 画像。](assets/datastream-override-target.png)

* **[!UICONTROL 有効]**/**[!UICONTROL 無効]**：このドロップダウンメニューを使用して、Adobe Target サービスへのデータルーティングを有効または無効にします。

## 詳細設定を指定

Edge Networkとのやり取りに使用するベースパスを変更する必要がある場合は ]****[!UICONTROL  Edge ベースパス } フィールドを使用します。 アップデートは必要ありませんが、ベータ版やアルファ版の場合は、Adobeからフィールドの変更を求められる場合があります。

![Web SDK タグ拡張機能ページを使用した詳細設定を示す画像。](assets/advanced-settings.png)
