---
keywords: イベント転送拡張機能；pinterest;pinterest イベント転送拡張機能
title: Pinterest イベント転送拡張機能
description: このAdobe Experience Platform イベント転送拡張機能を使用すると、ビジネス要件に合わせてPinterestにイベントを取り込むことができます。
last-substantial-update: 2023-04-27T00:00:00Z
exl-id: 44f38a9b-0a28-4b51-bead-ee460eb8405e
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '1478'
ht-degree: 6%

---

# [!DNL Pinterest] イベント転送拡張機能

[!DNL Pinterest] は、レシピ、家の装飾、スタイルのインスピレーションなどのアイデアを見つけるための視覚的な発見エンジンです。 [!DNL Pinterest] には数十億のピンがあり、[!DNL Pinterest] 上の他のユーザーと共有することもできます。 ユーザーインタラクションイベントを照合し、[!DNL Pinterest Analytics] れを活用してユーザーの行動を把握し、ターゲット広告を実行できます。

[[!DNL Pinterest] Conversions](https://developers.pinterest.com/docs/conversions/conversion-management/) API [&#x200B; イベント転送 &#x200B;](../../../ui/event-forwarding/overview.md) 拡張機能を使用すると、Adobe Experience Platform Edge Networkで取得したデータを活用して [!DNL Pinterest] に送信できます。 このドキュメントでは、拡張機能のユースケース、インストール方法および機能をイベント転送 [&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md) に統合する方法について説明します。

コンバージョンアクセストークンは、[!DNL Pinterest] が [!DNL Pinterest] API を操作する際に使用する認証方法です。

## ユースケース

この拡張機能は、Edge Networkのデータを使用して customer analytics の機能 [!DNL Pinterest] 活用する場合に使用します。

例えば、組織のマーケティングチームについて考えてみましょう。 チームは、web サイトからユーザーインタラクションイベントデータを取得し、このイベント転送拡張機能を使用して [!DNL Pinterest] に読み込みます。

その後、マーケティングチームと分析チームは、[!DNL Pinterest] Analytics 機能を活用して主要なユーザーのインタラクションと行動を理解することで、ユーザーをより深く理解し、ターゲット広告キャンペーンのターゲットに設定できます。

[!DNL Pinterest] 固有のユースケースについて詳しくは、[[!DNL Pinterest]  ユースケース &#x200B;](https://business.pinterest.com/en/success-stories) ドキュメントを参照してください。

## [!DNL Pinterest] 前提条件 {#prerequisites}

この拡張機能を使用するには、有効な [!DNL Pinterest] [&#x200B; ビジネスアカウント &#x200B;](https://help.pinterest.com/en/business/article/get-a-business-account) が必要です。 アカウントをまだお持ちでない場合は、[[!DNL Pinterest]  登録ページ &#x200B;](https://www.pinterest.com/business/create/) に移動し、アカウントを登録して作成してください。

また、[!DNL Pinterest] 開発者アカウントも必要です。このアカウントは、[!DNL Pinterest] ビジネスアカウントに関連付ける必要があります。 開発者アカウントをビジネスアカウントに関連付けるには、[[!DNL Pinterest &#x200B;]  開発者アカウント &#x200B;](https://developers.pinterest.com/account-setup/) を参照してください。

### 必要な設定の詳細の収集 {#configuration-details}

Experience Platformを [!DNL Pinterest] に接続するには、次の入力が必要です。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| 広告アカウント Id | [!DNL Pinterest] 広告アカウント Id。 詳しくは、[[!DNL Pinterest] ドキュメント](https://help.pinterest.com/en/business/article/find-ids-in-ads-manager)を参照してください。 | 123456789012 |
| コンバージョンアクセストークン | [!DNL Pinterest] コンバージョンアクセストークン。 詳しくは、[[!DNL Pinterest] Conversions API](https://developers.pinterest.com/docs/conversions/conversions/#Get%20the%20conversion%20token) ドキュメントを参照してください。<br> **このトークンは期限切れではないので、これは 1 回だけ実行する必要があります。** | {YOUR_PINTEREST_BEARER_TOKEN} |

## [!DNL Pinterest] 拡張機能のインストールと設定 {#install}

拡張機能をインストールするには、[&#x200B; イベント転送プロパティを作成 &#x200B;](../../../ui/event-forwarding/overview.md#properties) するか、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションで、「**[!UICONTROL 拡張機能]**」を選択します。 「**[!UICONTROL カタログ]**」タブの [!DNL Pinterest] 拡張機能のカードにある「**[!UICONTROL インストール]**」を選択します。

![[!UICONTROL &#x200B; インストール &#x200B;] がハイライト表示された [!DNL Pinterest] 拡張機能を表示するカタログ &#x200B;](../../../images/extensions/server/pinterest/install.png)

### [!DNL Pinterest] 拡張機能の設定

>[!IMPORTANT]
>
>実装のニーズに応じて、拡張機能を設定する前に、スキーマ、データ要素、データセットの作成が必要になる場合があります。 ユースケースに合わせて設定する必要のあるエンティティを判断するには、開始する前にすべての設定手順を確認してください。

左側のナビゲーションで、「**[!UICONTROL 拡張機能]**」を選択します。 **[!UICONTROL インストール済み]** タブの [!DNL Pinterest] 拡張機能のカードで [!UICONTROL &#x200B; 設定 &#x200B;]**を選択します。

![[!DNL Pinterest] 設定 [!UICONTROL &#x200B; がハイライト表示された &#x200B;] インストール [!UICONTROL &#x200B; タブに表示され &#x200B;] 拡張機能 &#x200B;](../../../images/extensions/server/pinterest/configure.png)

次の画面で、以前に「設定の詳細 [!UICONTROL &#x200B; セクションで収集した &#x200B;] 広告アカウント ID と [&#x200B; コンバージョンアクセストークン &#x200B;](#configuration-details) を入力します。 完了したら「**[!UICONTROL 保存]**」を選択します。

![[!UICONTROL &#x200B; 広告アカウント ID] および [!UICONTROL &#x200B; コンバージョンアクセストークン &#x200B;] 入力フィールドがハイライト表示された [!DNL Pinterest] [!UICONTROL &#x200B; 設定 &#x200B;] 画面 &#x200B;](../../../images/extensions/server/pinterest/input.png)

## イベント転送ルールの設定 {#config-rule}

すべてのデータ要素を設定したら、イベントを [!DNL Pinterest] に送信するタイミングと方法を決定するイベント転送ルールの作成を開始できます。

イベント転送プロパティに新しい [&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md) を作成します。 「**[!UICONTROL アクション]**」で、新しいアクションを追加し、拡張機能を「**[!UICONTROL Pinterest]**」に設定します。 Edge Networkイベントを [!DNL Pinterest] に送信するには、**[!UICONTROL アクションタイプ]** を **[!UICONTROL イベントを送信 &#x200B;].** に設定します。

![[!DNL Pinterest] [!UICONTROL &#x200B; イベントを送信 &#x200B;] ルールの作成。](../../../images/extensions/server/pinterest/rule.png)

選択後、追加のコントロールが表示され、イベントをさらに設定できます。 [!DNL Pinterest] のイベントプロパティを、以前に作成したデータ要素にマッピングする必要があります。

### [!UICONTROL &#x200B; イベントデータ &#x200B;]

新しいルールを作成するには、次のイベントデータが必要です。

| フィールド名 | 説明 | 例 |
| --- | --- | --- | 
| [!UICONTROL &#x200B; イベント名 &#x200B;] | ユーザーイベントのタイプ。 任意のイベントタイプを使用できますが、[!DNL Pinterest Analytics] れを活用するには、[[!DNL Pinterest]  イベントコード &#x200B;](https://help.pinterest.com/en/business/article/add-event-codes) を使用することをお勧めします。 | * チェックアウト <br> * add_to_cart <br> * page_visit <br> * サインアップ <br> * [ ユーザー定義イベント ] |
| [!UICONTROL &#x200B; アクションSource] | コンバージョンイベントが発生した場所を示すソース。 | * app_android <br> * app_ios <br> * web <br> * offline |
| [!UICONTROL &#x200B; イベント時間 &#x200B;] | これはイベント時間を指します。 使用されるデフォルトの時間形式は UNIX で、形式はローカルタイムゾーンに応じて `<seconds>.<miliseconds>` なります。 詳しくは、[[!DNL Pinterest] API](https://developers.pinterest.com/docs/api/v5/#operation/events/create) を参照してください。 | 1433188255.500 は、エポックから 1433188255 秒と 500 ミリ秒後、すなわち 2015 年 6 月 1 日（月）の午後 7:50:55 （GMT）を示します。 |
| [!UICONTROL &#x200B; イベント ID] | このイベントを識別する一意の ID 文字列。コンバージョン API とPinterest トラッキングの両方を介して取り込まれたイベント間の重複排除に使用できます。 そうしないと、イベントのデータが二重にカウントされ、指標の膨張を報告する可能性があります。 | ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad |
| [!UICONTROL &#x200B; イベントのプロパティ &#x200B;] | イベントのカスタムプロパティを含む JSON オブジェクト。 生の JSON を提供するか、キー値の入力のシンプルなセットを使用するかを選択します。 | { &quot;event_source_url&quot;: &quot;http://site.com&quot; } |

![&#x200B; ルールアクションでハイライト表示されている [!DNL Pinterest]&#x200B;[!UICONTROL &#x200B; イベントデータ &#x200B;]。](../../../images/extensions/server/pinterest/event-data.png)

次のイベントプロパティを設定できます。

| フィールド名 | 説明 |
| --- | --- |
| イベントSourceの URL | Web コンバージョンイベントの URL。 |
| アプリケーション ストア ID | App Store アプリ ID。 |
| アプリケーション名 | アプリケーションの名前。 |
| Application Version | アプリケーションのバージョン。 |
| デバイスブランド | ユーザーが使用しているデバイスのブランド。 |
| デバイス キャリア | 自分のデバイスでのユーザーの携帯電話会社。 |
| デバイスモデル | ユーザーのデバイスのモデル。 |
| Device Type | ユーザーが使用しているデバイスのタイプ。 |
| OS バージョン | デバイスのオペレーティングシステムのバージョン。 |
| ユーザーの言語 | ユーザーの言語を示す 2 文字の ISO-639-1 言語コード。 |

### [!UICONTROL &#x200B; ユーザーデータ &#x200B;]

必須フィールドではないユーザーが次のユーザーデータを入力できます。

| フィールド名 | 説明 | 例 |
| --- | --- | --- | 
| [!UICONTROL メール] | ユーザーの電子メールアドレス、またはユーザーのアドレスの電子メールの SHA256 ハッシュ。 | ebd543592...f2b7e1 |
| [!UICONTROL &#x200B; モバイル広告 ID] | ユーザーの「Google Advertising ID」（GAID）または「Appleの広告主識別子」（IDFA）の SHA256 ハッシュ | ebd543592...f2b7e1 |
| [!UICONTROL &#x200B; クライアント IP アドレス &#x200B;] | ユーザーの IP アドレス。IPv4 形式または IPv6 形式のいずれかです。 照合に使用します。 | 192.168.0.1 |
| [!UICONTROL &#x200B; クライアントユーザーエージェント &#x200B;] | ユーザーの Web ブラウザーのユーザーエージェント文字列。 | Mozilla/5.0 （platform; rv:geckoversion） Gecko/geckotrail Firefox/firefoxversion |
| [!UICONTROL &#x200B; 顧客情報データ &#x200B;] | その他の顧客情報を含む JSON オブジェクト。 生の JSON を提供するか、キー値の入力のシンプルなセットを使用するかを選択します。 | { &quot;ph&quot;: &quot;122333445&quot; } |

![&#x200B; ルールアクションでハイライト表示されている [!DNL Pinterest]&#x200B;[!UICONTROL &#x200B; ユーザーデータ &#x200B;]。](../../../images/extensions/server/pinterest/user-data.png)

設定可能な顧客情報プロパティは次のとおりです。

| フィールド名 | 説明 |
| --- | --- |
| Phone | ユーザーの連絡先番号。 数字のみを使用でき、国コード、市外局番、および番号を入力する必要があります。 |
| 性別 | 性別は、女性の場合は「f」、男性の場合は「m」、バイナリ以外の場合は「n」のいずれかを入力できます。 |
| 生年月日 | 生年月日。年、月、日と入力します。 |
| 姓 | ユーザーの姓。 |
| 名 | ユーザーの名。 |
| 市区町村 | ユーザーの居住都市。 主に課金目的で使用されます。 |
| 都道府県 | ユーザーの状態（小文字の 2 文字コードで提供）。 |
| 郵便番号 | ユーザーの郵便番号。主に請求目的で使用されます。 |
| 国 | ユーザーの国を示す 2 文字の ISO-3166 国コード。 |
| 外部 ID | スペース内のユーザーを識別する広告主からの一意の ID。 例えば、ユーザー ID、ロイヤルティ ID などです。 |
| クリック ID | ドメインの_epik cookie または URL の&amp;epik= クエリパラメーターに保存された一意の識別子。 |

>[!IMPORTANT]
>
>データを [!DNL Pinterest] API エンドポイントに送信する前に、拡張機能は、次のフィールド（メール、電話番号、名、姓、性別、生年月日、市区町村、都道府県、郵便番号、国、外部 ID）の値をハッシュ化して正規化します。 SHA256 文字列が既に存在する場合、拡張機能はこれらのフィールドの値をハッシュ化しません。

### [!UICONTROL &#x200B; カスタムデータ &#x200B;]

ルールに次のカスタムデータを入力できます。

| フィールド名 | 説明 |
| --- | --- |
| 通貨 | ISO-4217 通貨コード。 これを指定しない場 [!DNL Pinterest]、アカウントの作成時に設定された広告主の通貨がデフォルトで使用されます。 |
| 値 | イベントの合計値。 リクエスト内の文字列として許可されます。 これは 2 桁に解析されます。 |
| 検索文字列 | ユーザー変換イベントに関連する検索文字列。 |
| 注文 ID | 注文 ID。 `order_id` を送信すると、必要に応じてイベント [!DNL Pinterest] 重複排除できます。 |
| 製品の数 | イベントの製品の合計数。 例えば、チェックアウトイベントで購入した項目の合計数などです。 |
| コンテンツ ID | 製品 ID のリスト（配列）。 |
| 目次 | 価格や数量など、商品に関する情報を含んだオブジェクトのリスト（配列）。 |

![&#x200B; ルールアクションでハイライト表示されている [!DNL Pinterest]&#x200B;[!UICONTROL &#x200B; カスタムデータ &#x200B;]。](../../../images/extensions/server/pinterest/custom-data.png)

## [!DNL Pinterest] 内のデータの検証

イベント転送ルールを作成して実行したら、[!DNL Pinterest] API に送信されたイベントが [!DNL Pinterest] UI で期待どおりに表示されるかどうかを検証します。

イベントの収集と [!DNL Experience Platform] の統合が成功すると、[!DNL Pinterest] UI 内にイベントが表示されます。

![[!DNL Pinterest] イベントマネージャー &#x200B;](../../../images/extensions/server/pinterest/event-history.png)

さらにドリルスルーして、[!DNL Pinterest] のイベント・データ配布を表示できます。

![[!DNL Pinterest] データ配分 &#x200B;](../../../images/extensions/server/pinterest/event-history-distribution.png)

## 次の手順

このガイドでは、UI で [!DNL Pinterest] イベント転送拡張機能をインストールおよび設定する方法について説明しました。 詳しくは、次の公式ドキュメントを参照してください。

* [[!DNL Pinterest] API](https://developers.pinterest.com/docs/api/v5/)
* [[!DNL Pinterest] Conversions API の概要 &#x200B;](https://help.pinterest.com/en/business/article/the-pinterest-api-for-conversions)
