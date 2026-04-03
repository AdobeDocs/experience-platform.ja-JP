---
keywords: イベント転送拡張機能；pinterest;pinterest イベント転送拡張機能
title: Pinterest イベント転送拡張機能
description: このAdobe Experience Platform イベント転送拡張機能を使用すると、ビジネス要件に合わせてPinterestにイベントを取り込むことができます。
last-substantial-update: 2023-04-27T00:00:00Z
exl-id: 44f38a9b-0a28-4b51-bead-ee460eb8405e
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1427'
ht-degree: 6%

---

# [!DNL Pinterest] イベント転送拡張機能

[!DNL Pinterest]は、レシピ、ホームデコレーション、スタイルのインスピレーションなどのアイデアを見つけるためのビジュアル検索エンジンです。 [!DNL Pinterest]には何十億ものピンがあり、[!DNL Pinterest]に他のユーザーと共有することもできます。 ユーザーのインタラクションイベントを照合し、[!DNL Pinterest Analytics]を活用してユーザーの行動を把握し、ターゲット広告を実行できます。

[[!DNL Pinterest] Conversions](https://developers.pinterest.com/docs/conversions/conversion-management/) API [ イベント転送](../../../ui/event-forwarding/overview.md)拡張機能を使用すると、Adobe Experience Platform Edge Networkでキャプチャしたデータを活用して、[!DNL Pinterest]に送信できます。 このドキュメントでは、拡張機能の使用例、インストール方法、イベント転送[ ルール ](../../../ui/managing-resources/rules.md)に拡張機能を統合する方法について説明します。

コンバージョンアクセストークンは、[!DNL Pinterest]が[!DNL Pinterest] APIとやり取りする際に使用する認証方法です。

## ユースケース

この拡張機能は、[!DNL Pinterest]のEdge Networkのデータを使用してCustomer Analytics機能を活用する場合に使用します。

例えば、組織内のあるマーケティングチームを考えてみましょう。 チームは、ユーザーのインタラクションイベントデータをweb サイトから取得し、このイベント転送拡張機能を使用して[!DNL Pinterest]に読み込みます。

マーケティング部門と分析部門は、[!DNL Pinterest]の分析機能を活用して、主要な利用者のインタラクションと行動を把握できます。これにより、利用者をより深く理解し、ターゲットを絞った広告施策を展開できるようになります。

[!DNL Pinterest]に固有のユースケースについて詳しくは、[[!DNL Pinterest]  ユースケース ](https://business.pinterest.com/en/success-stories)のドキュメントを参照してください。

## [!DNL Pinterest] 前提条件 {#prerequisites}

この拡張機能を使用するには、有効な[!DNL Pinterest] [法人アカウント ](https://help.pinterest.com/en/business/article/get-a-business-account)が必要です。 [[!DNL Pinterest] 登録ページ ](https://www.pinterest.com/business/create/)に移動してアカウントを登録し、まだアカウントをお持ちでない場合は作成します。

また、[!DNL Pinterest]開発者アカウントも必要です。このアカウントは、[!DNL Pinterest]法人アカウントに関連付ける必要があります。 開発者アカウントをビジネスアカウントに関連付けるには、[[!DNL Pinterest ] 開発者アカウント ](https://developers.pinterest.com/account-setup/)を参照してください。

### 必要な設定の詳細を収集する {#configuration-details}

Experience Platformを[!DNL Pinterest]に接続するには、次の入力が必要です。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| 広告アカウント Id | お客様の[!DNL Pinterest]広告アカウント Id。 詳しくは、[[!DNL Pinterest] ドキュメント](https://help.pinterest.com/en/business/article/find-ids-in-ads-manager)を参照してください。 | 123456789012 |
| コンバージョンアクセストークン | [!DNL Pinterest] コンバージョンアクセストークン。 ガイダンスについては、[[!DNL Pinterest]  コンバージョン API](https://developers.pinterest.com/docs/conversions/conversions/#Get%20the%20conversion%20token) ドキュメントを参照してください。<br> **このトークンは期限切れではないため、この操作は1回のみ行う必要があります。** | {YOUR_PINTEREST_BEARER_TOKEN} |

## [!DNL Pinterest]拡張機能のインストールと設定 {#install}

拡張機能をインストールするには、[ イベント転送プロパティ ](../../../ui/event-forwarding/overview.md#properties)を作成するか、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションで、**[!UICONTROL Extensions]**&#x200B;を選択します。 **[!UICONTROL Install]** タブの[!DNL Pinterest]拡張機能のカードで&#x200B;**[!UICONTROL Catalog]**&#x200B;を選択します。

![がハイライト表示された[!DNL Pinterest]拡張機能を表示する[!UICONTROL Install] カタログ。](../../../images/extensions/server/pinterest/install.png)

### [!DNL Pinterest] 拡張機能の設定

>[!IMPORTANT]
>
>実装のニーズに応じて、拡張機能を設定する前に、スキーマ、データ要素、データセットを作成する必要がある場合があります。 ユースケースに合わせて設定する必要のあるエンティティを判断するには、開始する前にすべての設定手順を確認してください。

左側のナビゲーションで、**[!UICONTROL Extensions]**&#x200B;を選択します。 **[!UICONTROL Configure]**** タブの[!DNL Pinterest]拡張機能のカードで[!UICONTROL Installed]を選択します。

![[!DNL Pinterest]がハイライト表示された[!UICONTROL Install] タブに表示されている[!UICONTROL Configure]拡張機能。](../../../images/extensions/server/pinterest/configure.png)

次の画面で、以前に[!UICONTROL Ads Account Id]設定の詳細[!UICONTROL Conversion Access Token] セクションで収集した[と](#configuration-details)を入力します。 完了したら、**[!UICONTROL Save]**&#x200B;を選択します。

![入力フィールド [!DNL Pinterest]と[!UICONTROL Configure]を強調表示する[!UICONTROL Ads Account Id] [!UICONTROL Conversion Access Token]画面。](../../../images/extensions/server/pinterest/input.png)

## イベント転送ルールの設定 {#config-rule}

すべてのデータ要素を設定したら、イベントをいつ、どのように[!DNL Pinterest]に送信するかを決定するイベント転送ルールの作成を開始できます。

イベント転送プロパティに新しい[ ルール ](../../../ui/managing-resources/rules.md)を作成します。 **[!UICONTROL Actions]**&#x200B;で、新しいアクションを追加し、拡張機能を&#x200B;**[!UICONTROL Pinterest]**&#x200B;に設定します。 Edge Network イベントを[!DNL Pinterest]に送信するには、**[!UICONTROL Action Type]**&#x200B;を&#x200B;**[!UICONTROL Send Event]に設定します。**

![[!DNL Pinterest] [!UICONTROL Send Event] ルールの作成。](../../../images/extensions/server/pinterest/rule.png)

選択後、追加のコントロールが表示され、イベントがさらに設定されます。 [!DNL Pinterest] イベントプロパティを、以前に作成したデータ要素にマッピングする必要があります。

### [!UICONTROL Event Data]

新しいルールを作成するには、次のイベントデータが必要です。

| フィールド名 | 説明 | 例 |
| --- | --- | --- |
| [!UICONTROL Event Name] | ユーザーイベントのタイプ。 ただし、これは任意のイベントタイプにできますが、[!DNL Pinterest Analytics]を活用するには、[[!DNL Pinterest]  イベントコード ](https://help.pinterest.com/en/business/article/add-event-codes)を使用することをお勧めします | &amp;ast; チェックアウト <br> &amp;ast; add_to_cart <br> &amp;ast; page_visit <br> &amp;ast；登録<br> &amp;ast; [ ユーザー定義イベント ] |
| [!UICONTROL Action Source] | コンバージョンイベントが発生した場所を示すソース。 | &amp;ast; app_android <br> &amp;ast; app_ios <br> &amp;ast; web <br> &amp;ast; オフライン |
| [!UICONTROL Event Time] | これはイベント時間を指します。 使用されるデフォルトの時間形式はUNIXで、ローカルのタイムゾーンに応じて`<seconds>.<miliseconds>`の形式になります。 詳しくは、[[!DNL Pinterest] API](https://developers.pinterest.com/docs/api/v5/#operation/events/create)を参照してください。 | 1433188255.500は、1433188255秒後500 ミリ秒後、または2015年6月1日月曜日の午後7:50:55 GMTを示します。 |
| [!UICONTROL Event ID] | このイベントを識別する一意のID文字列。コンバージョン APIとPinterest トラッキングの両方を介して取り込まれたイベント間の重複排除に使用できます。 この機能がなければ、イベントのデータは二重にカウントされる可能性が高く、指標のインフレーションを報告します。 | ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad |
| [!UICONTROL Event Properties] | イベントのカスタムプロパティを含むJSON オブジェクト。 生のJSONを提供するか、簡素化されたキー値入力セットを使用するかを選択します。 | { &quot;event_source_url&quot;: &quot;http://site.com&quot; } |

![ ルール アクションで[!DNL Pinterest] [!UICONTROL Event Data]が強調表示されました。](../../../images/extensions/server/pinterest/event-data.png)

次のイベントプロパティを設定できます。

| フィールド名 | 説明 |
| --- | --- |
| イベント Source URL | Web コンバージョンイベントのURL。 |
| アプリケーションストア ID | アプリストアのアプリ ID。 |
| アプリケーション名 | アプリケーションの名前。 |
| Application Version | アプリケーションのバージョン。 |
| デバイスブランド | ユーザーが使用しているデバイスのブランド。 |
| デバイスキャリア | ユーザーのデバイス用モバイルキャリア。 |
| デバイスモデル | ユーザーのデバイスのモデル。 |
| Device Type | ユーザーが使用しているデバイスのタイプ。 |
| OS バージョン | デバイスのオペレーティングシステムのバージョン。 |
| ユーザー言語 | ユーザーの言語を示す2文字のISO-639-1言語コード。 |

### [!UICONTROL User Data]

次のユーザーデータはによって入力できます。必須フィールドではありません。

| フィールド名 | 説明 | 例 |
| --- | --- | --- |
| [!UICONTROL Email] | ユーザーメールアドレス、またはユーザーアドレスメールのSHA256 ハッシュ。 | ebd543592...f2b7e1 |
| [!UICONTROL Mobile Adverstising IDs] | ユーザーの「Google Advertising ID」（GAID）または「Appleの広告主向けID」（IDFA）のSha256 ハッシュ | ebd543592...f2b7e1 |
| [!UICONTROL Client IP Address] | ユーザーのIP アドレス （IPv4形式またはIPv6形式）。 マッチングに使用されます。 | 192.168.0.1 |
| [!UICONTROL Client User Agent] | ユーザーのweb ブラウザーのユーザーエージェント文字列。 | Mozilla/5.0 （platform; rv:geckoversion） Gecko/geckotrail Firefox/firefoxversion |
| [!UICONTROL Customer information data] | 他の顧客情報を含むJSON オブジェクト。 生のJSONを提供するか、簡素化されたキー値入力セットを使用するかを選択します。 | { &quot;ph&quot;: &quot;122333445&quot; } |

![ ルール アクションで[!DNL Pinterest] [!UICONTROL User Data]が強調表示されました。](../../../images/extensions/server/pinterest/user-data.png)

設定可能な顧客情報プロパティは次のとおりです。

| フィールド名 | 説明 |
| --- | --- |
| Phone | ユーザーの連絡先番号。 数字のみが使用でき、国コード、市外局番、および番号を入力する必要があります。 |
| 性別 | 性別は、女性の場合は「f」、男性の場合は「m」、非バイナリの場合は「n」として入力できます。 |
| 生年月日 | 年、月、日として入力された生年月日。 |
| 姓 | ユーザーの姓。 |
| 名 | ユーザーの名前。 |
| 市区町村 | ユーザーの居住地： これは主に請求目的で使用されます。 |
| 都道府県 | 小文字の2文字コードとして提供されるユーザーの状態。 |
| 郵便番号 | ユーザーの郵便番号。主に請求目的で使用されます。 |
| 国 | ユーザーの国を示す2文字のISO-3166国コード。 |
| 外部 ID | スペース内のユーザーを識別する広告主からの一意のID。 例えば、ユーザーID、ロイヤルティ IDなどです。 |
| クリック ID | ドメインの_epik cookieに保存されている一意の識別子、またはURLの&amp;epik= クエリパラメーター。 |

>[!IMPORTANT]
>
>データを[!DNL Pinterest] API エンドポイントに送信する前に、拡張機能は、電子メール、電話番号、名、姓、性別、生年月日、都市、都道府県、郵便番号、国、外部IDの各フィールドの値をハッシュ化して正規化します。 SHA256文字列が既に存在する場合、拡張機能はこれらのフィールドの値をハッシュしません。

### [!UICONTROL Custom Data]

ルールには、次のカスタムデータを入力できます。

| フィールド名 | 説明 |
| --- | --- |
| 通貨 | ISO-4217通貨コード。 これを指定しない場合、[!DNL Pinterest]はアカウント作成時に設定された広告主の通貨にデフォルトで設定されます。 |
| 値 | イベントの合計値。 リクエストで文字列として受け入れられました。 これは2桁に解析されます。 |
| 検索文字列 | ユーザー変換イベントに関連する検索文字列。 |
| 注文ID | 注文ID。 `order_id`を送信すると、必要に応じて[!DNL Pinterest]件のイベントが重複しなくなります。 |
| 製品数 | イベントの製品の合計数。 たとえば、チェックアウトイベントで購入された商品の合計数などです。 |
| コンテンツ ID | 製品IDのリスト（配列）。 |
| 目次 | 価格や数量など、製品に関する情報を含むオブジェクトのリスト（配列）。 |

![ ルール アクションで[!DNL Pinterest] [!UICONTROL Custom Data]が強調表示されました。](../../../images/extensions/server/pinterest/custom-data.png)

## [!DNL Pinterest]内のデータを検証

イベント転送ルールを作成して実行したら、[!DNL Pinterest] APIに送信されたイベントが[!DNL Pinterest] UIで期待どおりに表示されているかどうかを検証します。

イベントの収集と[!DNL Experience Platform]統合が成功した場合、[!DNL Pinterest] UI内にイベントが表示されます。

![ イベント マネージャー[!DNL Pinterest]](../../../images/extensions/server/pinterest/event-history.png)

[!DNL Pinterest] イベントデータ配布をさらにドリルスルーして表示できます。

![[!DNL Pinterest] データ配布](../../../images/extensions/server/pinterest/event-history-distribution.png)

## 次の手順

このガイドでは、UIに[!DNL Pinterest] イベント転送拡張機能をインストールして設定する方法について説明しました。 詳しくは、公式ドキュメントを参照してください。

* [[!DNL Pinterest] API](https://developers.pinterest.com/docs/api/v5/)
* [[!DNL Pinterest]  コンバージョン APIの概要](https://help.pinterest.com/en/business/article/the-pinterest-api-for-conversions)
