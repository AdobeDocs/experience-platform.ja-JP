---
title: Meta Conversions API 拡張機能の概要
description: Adobe Experience Platformでのイベント転送用の Meta Conversions API 拡張機能について説明します。
exl-id: 6b5836d6-6674-4978-9165-0adc1d7087b7
source-git-commit: c2832821ea6f9f630e480c6412ca07af788efd66
workflow-type: tm+mt
source-wordcount: '2583'
ht-degree: 0%

---

# [!DNL Meta Conversions API] 拡張機能の概要

[[!DNL Meta Conversions API]](https://developers.facebook.com/docs/marketing-api/conversions-api/) を使用すると、サーバーサイドのマーケティングデータを [!DNL Meta] テクノロジーに接続して、広告ターゲティングの最適化、アクションあたりのコストの削減、結果の測定を行うことができます。 イベントは [[!DNL Meta Pixel]](https://developers.facebook.com/docs/meta-pixel/) ID にリンクされ、クライアントサイドイベントと同様の方法で処理されます。

[!DNL Meta Conversions API] 拡張機能を使用すると、[ イベント転送 ](../../../ui/event-forwarding/overview.md) ルールで API 機能を活用して、Adobe Experience Platform Edge Networkから [!DNL Meta] にデータを送信できます。 このドキュメントでは、拡張機能をインストールし、イベント転送 [ ルール ](../../../ui/managing-resources/rules.md) でその機能を使用する方法について説明します。

## デモ

次のビデオは、[!DNL Meta Conversions API] について理解を深めるためのものです。

>[!VIDEO](https://unlockmarketingdata.com/video-meta-conversions-api)

## 前提条件

[!DNL Meta Pixel] と [!DNL Conversions API] を使用して、クライアント側とサーバー側で同じイベントを共有して送信することを強くお勧めします。[!DNL Meta Pixel] で取得されなかったイベントの回復に役立つ可能性があるからです。 [!DNL Conversions API] 拡張機能をインストールする前に、[[!DNL Meta Pixel]  拡張機能 ](../../client/meta/overview.md) に関するガイドを参照して、クライアントサイドのタグ実装に統合する手順を確認してください。

>[!NOTE]
>
>このドキュメントの後半の [ イベントの重複排除 ](#deduplication) の節では、ブラウザーとサーバーの両方から受信する可能性があるので、同じイベントが二度と使用されないようにする手順を説明します。

[!DNL Conversions API] 拡張機能を使用するには、イベント転送にアクセスでき、[!DNL Ad Manager] および [!DNL Event Manager] にアクセスできる有効な [!DNL Meta] アカウントが必要です。 特に、既存の [[!DNL Meta Pixel]](https://www.facebook.com/business/help/952192354843755?id=1205376682832142) の ID をコピー（または [ 新しく作成）して  [!DNL Pixel]](https://www.facebook.com/business/help/952192354843755) アカウントに拡張機能を設定できるようにする必要があります。

>[!INFO]
>
>この拡張機能をモバイルアプリデータで使用する場合や、[!DNL Meta] キャンペーンでオフラインイベントデータも使用する場合は、既存のアプリを使用してデータセットを作成し、プロンプトが表示されたら **ピクセル ID から作成** を選択する必要があります。 詳しくは、[ ビジネスに適したデータセット作成オプションの決定 ](https://www.facebook.com/business/help/5270377362999582?id=490360542427371) の記事を参照してください。 必須およびオプションのすべてのアプリトラッキングパラメーターについては、[Conversions API for App Events](https://developers.facebook.com/docs/marketing-api/conversions-api/app-events) ドキュメントを参照してください。

## 拡張機能のインストール

[!DNL Meta Conversions API] 拡張機能をインストールするには、データ収集 UI またはExperience PlatformUI に移動し、左側のナビゲーションから **[!UICONTROL イベント転送]** を選択します。 ここから、拡張機能を追加するプロパティを選択するか、代わりに新しいプロパティを作成します。

目的のプロパティを選択または作成したら、左側のナビゲーションで **[!UICONTROL 拡張機能]** を選択し、「**[!UICONTROL カタログ]**」タブを選択します。 [!UICONTROL Meta Conversions API] カードを検索し、「**[!UICONTROL インストール]**」を選択します。

![ データ収集 UI の [!UICONTROL Meta Conversions API] 拡張機能用に選択されている [!UICONTROL  インストール ] オプション ](../../../images/extensions/server/meta/install.png)

表示される設定ビューで、拡張機能をアカウントにリンクするには、以前にコピーした [!DNL Pixel] ID を指定する必要があります。 ID を入力に直接貼り付けることも、代わりにデータ要素を使用することもできます。

また、特に [!DNL Conversions API] を使用するためにアクセストークンを提供する必要があります。 この値を取得する手順については、[ アクセストークンの生成 ](https://developers.facebook.com/docs/marketing-api/conversions-api/get-started#access-token) に関する [!DNL Conversions API] ドキュメントを参照してください。

終了したら「**[!UICONTROL 保存]**」を選択します

![ 拡張機能の設定ビューでデータ要素として提供された [!DNL Pixel] ID。](../../../images/extensions/server/meta/configure.png)

拡張機能がインストールされ、イベント転送ルールでその機能を使用できるようになりました。

## facebookおよびInstagram拡張機能との統合 {#facebook}

facebookとInstagramの拡張機能を使用した統合により、Meta ビジネスアカウントにすばやく認証できます。 これにより、[!UICONTROL Pixel ID] と Meta Conversions API[!UICONTROL  アクセストークン ] が自動入力され、Meta Conversions API のインストールと設定が容易になります。

[!UICONTROL Meta Conversions API] 拡張機能をインストールすると、FacebookとInstagramでの認証を求めるダイアログプロンプトが表示されます。

![Meta への接続 [!UICONTROL  がハイライト表示された ]Meta Conversions API 拡張機能 [!UICONTROL  インストールページ ]。](../../../images/extensions/server/meta/mbe-extension-install.png)

facebookとInstagramでの認証を求めるダイアログプロンプトは、イベント転送内のクイックスタートワークフロー UI にも表示されます。

![ ハイライト表示されたクイックスタートワークフロー UI[!UICONTROL Meta に接続 ].](../../../images/extensions/server/meta/mbe-extension-quick-start.png)

## イベント品質一致スコア （EMQ）との統合 {#emq}

イベント品質一致スコア（EMQ）との統合により、EMQ スコアを表示して実装の有効性を簡単に確認できます。 この統合により、コンテキストの切り替えが最小限に抑えられ、Meta Conversions API 実装の成功を向上させることができます。 これらのイベントスコアは、[!UICONTROL Meta Conversions API 拡張機能 ] 設定画面に表示されます。

![[!UICONTROL Meta Conversions API 拡張機能 ] 設定ページのハイライト表示 [!UICONTROL EMQ スコアの表示 ].](../../../images/extensions/server/meta/emq-score.png)

## LiveRamp との統合（Alpha） {#alpha}

[!DNL LiveRamp] の認証済みトラフィックソリューション（ATS）をサイトにデプロイしている [!DNL LiveRamp] のお客様は、顧客情報パラメーターとして RampID を共有することを選択できます。 [!DNL Meta] アカウントチームと協力して、この機能のAlphaプログラムに参加してください。

![Meta イベント転送 [!UICONTROL  ルール ] 設定ページで [!UICONTROL  パートナー名（アルファ） ] および [!UICONTROL  パートナー ID （アルファ） ] がハイライト表示される。](../../../images/extensions/server/meta/live-ramp.png)

## イベント転送ルールの設定 {#rule}

この節では、一般的なイベント転送ルールで [!DNL Conversions API] 拡張機能を使用する方法について説明します。 実際には、受け入れ可能なすべての [ 標準イベント ](https://developers.facebook.com/docs/meta-pixel/reference) を [!DNL Meta Pixel] と [!DNL Conversions API] で送信するために、いくつかのルールを設定する必要があります。 モバイルアプリデータについては、必須フィールド、アプリデータフィールド、顧客情報パラメーター、カスタムデータの詳細を参照してください [ こちら ](https://developers.facebook.com/docs/marketing-api/conversions-api/app-events)。

>[!NOTE]
>
>広告キャンペーンを最適化するには、イベントを [ リアルタイムで送信 ](https://www.facebook.com/business/help/379226453470947?id=818859032317965) するか、できるだけリアルタイムに近いタイミングで送信する必要があります。

新しいイベント転送ルールの作成を開始し、必要に応じてその条件を設定します。 ルールのアクションを選択する場合、拡張機能で **[!UICONTROL Meta Conversions API 拡張機能]** を選択し、アクションタイプで **[!UICONTROL Conversions API イベントを送信]** を選択します。

![ データ収集 UI でルールに対して選択されている [!UICONTROL  ページビューを送信 ] アクションタイプ ](../../../images/extensions/server/meta/select-action.png)

[!DNL Conversions API] 経由で [!DNL Meta] ーザーに送信されるイベントデータを設定できるコントロールが表示されます。 これらのオプションは、指定された入力に直接入力することも、代わりに値を表す既存のデータ要素を選択することもできます。 設定オプションは、以下に示すように、4 つの主なセクションに分かれています。

| Config セクション | 説明 |
| --- | --- |
| [!UICONTROL  サーバーイベントパラメーター ] | 発生した時刻やトリガーしたソースアクションなど、イベントに関する一般情報。 [!DNL Conversions API] が受け入れる [ 標準イベントパラメーター ](https://developers.facebook.com/docs/marketing-api/conversions-api/parameters/server-event) について詳しくは、[!DNL Meta] 開発者向けドキュメントを参照してください。<br><br>[!DNL Meta Pixel] と [!DNL Conversions API] の両方を使用してイベントを送信する場合は、すべてのイベントに **[!UICONTROL イベント名]** （`event_name`）と **[!UICONTROL イベント ID]** （`event_id`）の両方を必ず含めてください。これらの値は [ イベントの重複排除 ](#deduplication) に使用されるからです。<br><br> 顧客のオプトアウトに準拠するために、**[!UICONTROL 制限付きデータ使用を有効にする]** オプションもあります。 この機能について詳しくは、[ データ処理オプション ](https://developers.facebook.com/docs/marketing-apis/data-processing-options/) に関する [!DNL Conversions API] ドキュメントを参照してください。 |
| [!UICONTROL  顧客情報パラメーター ] | イベントを顧客に関連付けるために使用されるユーザー ID データ。 これらの値の一部は、API に送信する前にハッシュ化する必要があります。<br><br> 良好な共通 API 接続と高いイベント一致品質（EMQ）を確保するために、サーバーイベントと共にすべての [ 許可された顧客情報パラメーター ](https://developers.facebook.com/docs/marketing-api/conversions-api/parameters/customer-information-parameters) を送信することをお勧めします。 また、これらのパラメータは [ 重要度と EMQ への影響に基づいて優先順位を付ける ](https://www.facebook.com/business/help/765081237991954?id=818859032317965) 必要があります。 |
| [!UICONTROL  カスタムデータ ] | 広告配信の最適化に使用する追加データ（JSON オブジェクトの形式で提供）。 このオブジェクトの使用可能なプロパティの詳細については、[[!DNL Conversions API] documentation](https://developers.facebook.com/docs/marketing-api/conversions-api/parameters/custom-data) を参照してください。<br><br> 購入イベントを送信する場合は、このセクションを使用して `currency` および `value` に必要な属性を指定する必要があります。 |
| [!UICONTROL  テストイベント ] | このオプションは、設定が原因で [!DNL Meta] がサーバーイベントを想定どおりに受信しているかどうかを検証するために使用されます。 この機能を使用するには、「**[!UICONTROL テストイベントとして送信]**」チェックボックスをオンにし、以下の入力で選択したテストイベントコードを指定します。 イベント転送ルールがデプロイされると、拡張機能とアクションを正しく設定した場合は、[!DNL Meta Events Manager] の **[!DNL Test Events]** ビュー内にアクティビティが表示されます。 |

{style="table-layout:auto"}

完了したら、「**[!UICONTROL 変更を保持]**」を選択して、アクションをルール設定に追加します。

![[!UICONTROL  変更を保持 ] アクション設定に対して選択されています。](../../../images/extensions/server/meta/keep-changes.png)

ルールの設定が完了したら、「**[!UICONTROL ライブラリに保存]**」を選択します。 最後に、新しいイベント転送 [ ビルド ](../../../ui/publishing/builds.md) を公開して、ライブラリに加えられた変更を有効にします。

## イベントの重複排除 {#deduplication}

[ 前提条件の節 ](#prerequisites) で説明したように、[!DNL Meta Pixel] タグ拡張機能と [!DNL Conversions API] イベント転送拡張機能の両方を使用して、同じイベントをクライアントとサーバーから冗長な設定で送信することをお勧めします。 これは、いずれかの拡張機能で取得されなかったイベントを回復するのに役立ちます。

クライアントとサーバーから異なるイベントタイプを送信し、両者が重複していない場合は、重複排除は必要ありません。 ただし、[!DNL Meta Pixel] と [!DNL Conversions API] の両方が 1 つのイベントを共有する場合は、レポートに悪影響が及ばないよう、これらの冗長なイベントの重複を排除する必要があります。

共有イベントを送信する場合は、クライアントとサーバーの両方から送信するすべてのイベントに、イベント ID と名前が含まれていることを確認してください。 同じ ID と名前を持つ複数のイベントを受け取った場合、[!DNL Meta] では、自動的に、重複を排除して最も関連性の高いデータを保持するために、複数の戦略を採用します。 このプロセスについて詳しくは、[ イベント  [!DNL Meta Pixel]  よびイベントの重複排除 ](https://www.facebook.com/business/help/823677331451951?id=1205376682832142) に関する [!DNL Meta] のドキュメ  [!DNL Conversions API]  トを参照してください。

## クイックスタートワークフロー：Meta Conversions API 拡張機能（Beta） {#quick-start}

>[!IMPORTANT]
>
>* クイックスタート機能は、Real-Time CDP Prime および Ultimate パッケージを購入したお客様が利用できます。 詳しくは、アドビ担当者にお問い合わせください。
>* この機能は、まったく新しい実装を対象としており、現在、既存のタグおよびイベント転送プロパティへの拡張機能と設定の自動インストールをサポートしていません。

>[!NOTE]
>
>既存のクライアントは、クイックスタートワークフローを使用して、次の目的で使用できる参照実装を作成できます。
>* まったく新しい実装の開始として使用します。
>* 参照実装として活用します。参照実装を調べて、どのように設定されたかを確認し、現在の実稼動実装にレプリケートできます。

クイックスタート機能を使用すると、Meta Conversions API と Meta Pixel 拡張機能を簡単かつ効率的に設定できます。 Adobeタグとイベント転送の複数の手順を自動化し、設定時間を大幅に短縮します。

この機能は、新しく自動生成されたタグおよびイベント転送プロパティに、必要なルールとデータ要素を含む Meta Conversions API と Meta Pixel 拡張機能の両方を自動的にインストールして設定します。 さらに、Experience PlatformWeb SDK とデータストリームの自動インストールおよび設定も行います。 最後に、クイックスタート機能により、Edge Network環境で指定された URL にライブラリが自動公開され、イベント転送とExperience Platform開発を介したクライアントサイドのデータ収集とサーバーサイドのイベント転送がリアルタイムで可能になります。

次のビデオでは、クイックスタート機能の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/3416939?quality=12&learn=on)

### クイックスタート機能のインストール

>[!NOTE]
>
>この機能は、イベント転送の実装を開始するのに役立つように設計されています。 すべてのユースケースに対応するエンドツーエンドの完全に機能する実装は提供されません。

このセットアップでは、Meta Conversions API と Meta Pixel 拡張機能の両方が自動でインストールされます。 Meta では、このハイブリッド実装を使用して、イベントコンバージョンサーバーサイドでイベントを収集および転送することをお勧めします。
クイックセットアップ機能は、お客様がイベント転送の実装を開始するのを支援するように設計されており、すべてのユースケースに対応するエンドツーエンドの完全に機能する実装を提供するものではありません。

この機能をインストールするには、Adobe Experience Platform Data Collection **[!UICONTROL ホーム]** ページで「**[!UICONTROL はじめに]**」を選択して **[!DNL Send Conversions Data to Meta]** ださい。

![meta へのコンバージョンデータを示すデータ収集ホームページ ](../../../images/extensions/server/meta/conversion-data-to-meta.png)

**[!UICONTROL ドメイン]** を入力し、「**[!UICONTROL 次へ]**」を選択します。 このドメインは、自動生成されるタグとイベント転送のプロパティ、ルール、データ要素、データストリームなどの命名規則として使用されます。

![ ドメイン名をリクエストするようこそ画面 ](../../../images/extensions/server/meta/welcome.png)

**[!UICONTROL 初期設定]** ダイアログで、「**[!UICONTROL メタピクセル ID]**」、「**[!UICONTROL メタ変換 API アクセストークン]**」、「**[!UICONTROL データレイヤーパス]**」を入力して、「**[!UICONTROL 次へ]**」を選択します。

![ 初期設定ダイアログ ](../../../images/extensions/server/meta/initial-setup.png)

初期セットアッププロセスが完了するまで数分待ってから、「**[!UICONTROL 次へ]**」を選択します。

![Initial setup complete confirmation screen](../../../images/extensions/server/meta/setup-complete.png)

**[!UICONTROL サイトにコードを追加]** ダイアログで、コピー ![ コピー ](/help/images/icons/copy.png) 関数を使用して提供されたコードをコピーして、ソース web サイトの `<head>` に貼り付けます。 実装が完了したら、「**[!UICONTROL 検証を開始]**」を選択します。

![ サイトダイアログへのコードの追加 ](../../../images/extensions/server/meta/add-code-on-your-site.png)

[!UICONTROL  検証結果 ] ダイアログに、メタ拡張機能の実装結果が表示されます。 「**[!UICONTROL 次へ]**」を選択します。 また、「**[!UICONTROL Assurance]**」リンクを選択すると、追加の検証結果を確認できます。

![ 実装結果を表示するテスト結果ダイアログ ](../../../images/extensions/server/meta/test-results.png)

「**[!UICONTROL 次の手順]**」画面が表示され、設定が完了したことが確認されます。 ここから、新しいイベントを追加して実装を最適化するオプションがあります。このイベントについては、次の節で説明します。

イベントを追加しない場合は、「**[!UICONTROL 閉じる]**」を選択します。

![ 次の手順に進むダイアログ ](../../../images/extensions/server/meta/next-steps.png)

#### 追加イベントの追加

新しいイベントを追加するには、「**[!UICONTROL タグの web プロパティを編集]**」を選択します。

![ タグの web プロパティを編集する方法を示す次の手順ダイアログ ](../../../images/extensions/server/meta/edit-your-tags-web-property.png)

編集するメタイベントに対応するルールを選択します。 例えば、「**MetaConversion_AddToCart**」と入力します。

>[!NOTE]
>
>イベントがない場合、このルールは実行されません。 これは、すべてのルールに当てはまります。ただし、**MetaConversion_PageView** ルールは例外です。

イベントを追加するには、「**[!UICONTROL イベント]** 見出しの下にある「[!UICONTROL  追加 ] を選択します。

![ イベントが表示されていないタグプロパティページ ](../../../images/extensions/server/meta/edit-rule.png)

[!UICONTROL  イベントタイプ ] を選択します。 この例では、[!UICONTROL Click] イベントを選択し、**.add-to-cart-button** が選択されている場合にトリガーするように設定しました。 「**[!UICONTROL 変更を保持]**」を選択します。

![ クリックイベントを表示するイベント設定画面 ](../../../images/extensions/server/meta/event-configuration.png)

新しいイベントが保存されました。 「**[!UICONTROL 作業ライブラリを選択]**」を選択し、ビルド先のライブラリを選択します。

![ 作業ライブラリの選択ドロップダウン ](../../../images/extensions/server/meta/working-library.png)

次に、「**[!UICONTROL ライブラリに保存]**」の横にあるドロップダウンを選択し、「**[!UICONTROL ライブラリおよびビルドに保存]**」を選択します。 これにより、ライブラリに変更が公開されます。

![ 「ライブラリに保存してビルド」を選択する ](../../../images/extensions/server/meta/save-and-build.png)

設定したい他のメタコンバージョンイベントに対して、これらの手順を繰り返します。

#### データレイヤーの設定 {#configuration}

>[!IMPORTANT]
>
>このグローバルデータレイヤーを更新する方法は、web サイトのアーキテクチャによって異なります。 単一ページアプリケーションは、サーバーサイドレンダリングアプリケーションとは異なります。 また、タグ製品内でこのデータの作成と更新を完全に担当する可能性もあります。 すべての場合、各 `MetaConversion_* rules` ークフローの実行間にデータレイヤーを更新する必要があります。 ルール間でデータを更新しない場合は、現在のルー `MetaConversion_* rule` の最後の `MetaConversion_* rule` ージから古いデータを送信している場合もあります。

設定時に、データレイヤーの保存場所を尋ねられました。 デフォルトでは、これは `window.dataLayer.meta` であり、`meta` オブジェクト内では、次に示すように、データが想定されます。

![ データレイヤーのメタ情報 ](../../../images/extensions/server/meta/data-layer-meta.png)

これは、すべての `MetaConversion_*` ルールでこのデータ構造を使用して、関連するデータを [!DNL Meta Pixel] 拡張機能と [!DNL Meta Conversions API] に渡すので、理解することが重要です。 様々なメタイベントに必要なデータについて詳しくは、[ 標準イベント ](https://developers.facebook.com/docs/meta-pixel/reference#standard-events) に関するドキュメントを参照してください。

例えば、`MetaConversion_Subscribe` ルールを使用する場合は、[ 標準イベント ](https://developers.facebook.com/docs/meta-pixel/reference#standard-events) に関するドキュメントに記載されているオブジェクトプロパティに従って、`window.dataLayer.meta.currency`、`window.dataLayer.meta.predicted_ltv`、`window.dataLayer.meta.value` を更新する必要があります。

以下は、ルールを実行する前にデータレイヤーを更新するために web サイトで実行する必要がある処理の例です。

![ データレイヤーのメタ情報の更新 ](../../../images/extensions/server/meta/update-data-layer-meta.png)

デフォルトでは、どの `MetaConversion_* rules` ージでも、「新しいイベント ID を生成」アクションによって `<datalayerpath>.conversionData.eventId` ータがランダムに生成されます。

データレイヤーの外観のローカル参照については、プロパティの `MetaConversion_DataLayer` データ要素でカスタムコードエディターを開きます。

## 次の手順

このガイドでは、[!DNL Meta Conversions API] 拡張機能を使用してサーバーサイドのイベントデータを [!DNL Meta] に送信する方法について説明しました。 ここから、より多くの [!DNL Pixels] を接続し、該当する場合はより多くのイベントを共有することで、統合を拡張することをお勧めします。 次のいずれかの操作を行うと、広告パフォーマンスをさらに向上させることができます。

* [!DNL Conversions API] 統合にまだ接続されていない他の [!DNL Pixels] を接続します。
* 特定のイベントをクライアントサイドの [!DNL Meta Pixel] でのみ送信する場合は、サーバーサイドからも同じイベントを [!DNL Conversions API] に送信します。

統合を効果的に実装する方法について詳しくは、[ のベストプラクティス  [!DNL Conversions API]](https://www.facebook.com/business/help/308855623839366?id=818859032317965) に関する [!DNL Meta] のドキュメントを参照してください。 Adobe Experience Cloudのタグとイベント転送の一般的な情報については、[ タグの概要 ](../../../home.md) を参照してください。
