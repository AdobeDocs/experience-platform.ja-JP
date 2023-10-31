---
keywords: イベント転送拡張機能；pinterest;pinterestイベント転送拡張機能
title: Pinterest event forwarding extension
description: このAdobe Experience Platformイベント転送拡張機能を使用すると、ビジネス要件に合わせてPinterestにイベントを取り込むことができます。
last-substantial-update: 2023-04-27T00:00:00Z
exl-id: 44f38a9b-0a28-4b51-bead-ee460eb8405e
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '1548'
ht-degree: 7%

---

# [!DNL Pinterest] イベント転送拡張機能

[!DNL Pinterest] は、レシピ、ホームデコール、スタイルのインスピレーションなどのアイデアを見つけるための視覚的発見エンジンです。 何十億ものピンが [!DNL Pinterest]（で他のユーザーと共有することもできます） [!DNL Pinterest]. ユーザーインタラクションイベントを照合し、 [!DNL Pinterest Analytics] ユーザーの行動を把握し、ターゲット広告を実行する。

The [[!DNL Pinterest] コンバージョン](https://developers.pinterest.com/docs/conversions/conversion-management/) API [イベント転送](../../../ui/event-forwarding/overview.md) 拡張機能を使用すると、 Adobe Experience Platform Edge Network で取得したデータを活用し、に送信できます。 [!DNL Pinterest]. このドキュメントでは、拡張機能の使用例、拡張機能のインストール方法、イベント転送に拡張機能を統合する方法について説明します [ルール](../../../ui/managing-resources/rules.md).

コンバージョンアクセストークンは、 [!DNL Pinterest] 操作中に [!DNL Pinterest] API.

## ユースケース

この拡張機能は、 [!DNL Pinterest] を使用して、顧客分析機能を活用します。

例えば、組織内のマーケティングチームについて考えてみましょう。 チームは、Web サイトからユーザーインタラクションイベントデータをキャプチャし、 [!DNL Pinterest] このイベント転送拡張機能を使用する。

マーケティングチームと分析チームは、 [!DNL Pinterest] Analytics の主なユーザーインタラクションおよび動作を把握し、ユーザーをより深く理解し、ターゲット化された広告キャンペーンのターゲットに設定する機能。

特有の使用例について詳しくは、 [!DNL Pinterest]（を参照） [[!DNL Pinterest] 使用例](https://business.pinterest.com/en/success-stories) ドキュメント。

## [!DNL Pinterest] 前提条件 {#prerequisites}

有効な [!DNL Pinterest] [ビジネスアカウント](https://help.pinterest.com/en/business/article/get-a-business-account) この拡張機能を使用するには、を参照してください。 次に移動： [[!DNL Pinterest] 登録ページ](https://www.pinterest.com/business/create/) をクリックして、アカウントを作成します（まだ持っていない場合）。

また、 [!DNL Pinterest] 開発者アカウント ( [!DNL Pinterest] ビジネスアカウント。 開発者アカウントをビジネスアカウントに関連付けるには、 [[!DNL Pinterest ] 開発者アカウント](https://developers.pinterest.com/account-setup/).

### 必要な設定の詳細の収集 {#configuration-details}

Experience Platformを [!DNL Pinterest]の場合、次の入力が必要です。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| 広告アカウント ID | お使いの [!DNL Pinterest] Ads アカウント ID。 詳しくは、[[!DNL Pinterest] ドキュメント](https://help.pinterest.com/en/business/article/find-ids-in-ads-manager)を参照してください。 | 123456789012 |
| コンバージョンアクセストークン | お使いの [!DNL Pinterest] コンバージョンアクセストークン。 詳しくは、 [[!DNL Pinterest] コンバージョン API](https://developers.pinterest.com/docs/conversions/conversions/#Get%20the%20conversion%20token) ガイダンス用のドキュメント。 <br> **このトークンは期限切れにならないので、この操作は 1 回のみ実行する必要があります。** | {YOUR_PINTEREST_BEARER_TOKEN} |

## のインストールと設定 [!DNL Pinterest] 拡張 {#install}

拡張機能をインストールするには、以下を実行します。 [イベント転送プロパティの作成](../../../ui/event-forwarding/overview.md#properties) または、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションで、「 **[!UICONTROL 拡張機能]**. 選択 **[!UICONTROL インストール]** ～のためのカードで [!DNL Pinterest] の拡張 **[!UICONTROL カタログ]** タブをクリックします。

![を表示するカタログ [!DNL Pinterest] 拡張 [!UICONTROL インストール] ハイライト表示されました。](../../../images/extensions/server/pinterest/install.png)

### [!DNL Pinterest] 拡張機能の設定

>[!IMPORTANT]
>
>実装のニーズに応じて、拡張機能を設定する前に、スキーマ、データ要素、データセットの作成が必要になる場合があります。 ユースケースに合わせて設定する必要のあるエンティティを判断するには、開始する前にすべての設定手順を確認してください。

左側のナビゲーションで、「 **[!UICONTROL 拡張機能]**. 選択 **[!UICONTROL 設定]** ～のためのカードで [!DNL Pinterest] の拡張 [!UICONTROL インストール済み]**タブをクリックします。

![[!DNL Pinterest] 拡張機能 [!UICONTROL インストール] タブ [!UICONTROL 設定] ハイライト表示されました。](../../../images/extensions/server/pinterest/configure.png)

次の画面で、 [!UICONTROL 広告アカウント ID] および [!UICONTROL コンバージョンアクセストークン] 以前に [設定の詳細](#configuration-details) 」セクションに入力します。 完了したら「**[!UICONTROL 保存]**」を選択します。

![The [!DNL Pinterest] [!UICONTROL 設定] 画面のハイライト [!UICONTROL 広告アカウント ID] および [!UICONTROL コンバージョンアクセストークン] 入力フィールド。](../../../images/extensions/server/pinterest/input.png)

## イベント転送ルールの設定 {#config-rule}

すべてのデータ要素を設定したら、イベントの送信先と送信方法を決定するイベント転送ルールの作成を開始できます。 [!DNL Pinterest].

新規作成 [ルール](../../../ui/managing-resources/rules.md) を設定します。 の下 **[!UICONTROL アクション]**、新しいアクションを追加し、拡張機能をに設定します。 **[!UICONTROL Pinterest]**. Edge ネットワークイベントをに送信するには、以下を実行します。 [!DNL Pinterest]を設定し、 **[!UICONTROL アクションタイプ]** から **[!UICONTROL イベントの送信].**

![The [!DNL Pinterest] [!UICONTROL イベントの送信] ルールの作成。](../../../images/extensions/server/pinterest/rule.png)

選択後、イベントをさらに設定するための追加のコントロールが表示されます。 次をマッピングする必要があります。 [!DNL Pinterest] イベントのプロパティを、以前に作成したデータ要素に追加します。

### [!UICONTROL イベントデータ]

新しいルールを作成するには、次のイベントデータが必要です。

| フィールド名 | 説明 | 例 |
| --- | --- | --- | 
| [!UICONTROL イベント名] | ユーザーイベントのタイプ。 ただし、これは、 [!DNL Pinterest Analytics] 使用をお勧めします。 [[!DNL Pinterest] イベントコード](https://help.pinterest.com/en/business/article/add-event-codes) | * checkout <br> * add_to_cart <br> * page_visit <br> *サインアップ <br> * [ユーザー定義イベント] |
| [!UICONTROL アクションソース] | コンバージョンイベントが発生した場所を示すソース。 | * app_android <br> * app_ios <br> * web <br> *オフライン |
| [!UICONTROL イベント時刻] | これはイベント時刻を参照します。 デフォルトの時刻形式は UNIX で、形式は `<seconds>.<miliseconds>` ローカルタイムゾーンに応じて異なります。 詳しくは、 [[!DNL Pinterest] API](https://developers.pinterest.com/docs/api/v5/#operation/events/create). | 1433188255.500 は、エポックの1433188255秒後、または 2015 年 6 月 1 日（月）の 7 時点を示します。:50:午後 55 時 (GMT)。 |
| [!UICONTROL イベント ID] | このイベントを識別し、コンバージョン API とPinterestの両方のトラッキングを通じて取り込まれたイベント間の重複排除に使用できる一意の ID 文字列。 これがないと、イベントのデータは二重にカウントされる可能性が高く、指標の水増しがレポートされます。 | ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad |
| [!UICONTROL イベントのプロパティ] | イベントのカスタムプロパティを含む JSON オブジェクト。 生の JSON を提供するか、シンプル化されたキーと値の入力セットを使用するかを選択します。 | { &quot;event_source_url&quot;: &quot;http://site.com&quot; } |

![The [!DNL Pinterest] [!UICONTROL イベントデータ] ルールアクション内でハイライト表示されています。](../../../images/extensions/server/pinterest/event-data.png)

次のイベントプロパティを設定できます。

| フィールド名 | 説明 |
| --- | --- |
| イベントソース URL | Web コンバージョンイベントの URL。 |
| アプリケーションストア ID | アプリストアのアプリ ID。 |
| アプリ名 |  アプリケーションの名前 |
| Application Version | アプリケーションのバージョン。 |
| デバイスのブランド | ユーザーが使用しているデバイスのブランド。 |
| デバイスキャリア | デバイスのユーザーの携帯電話会社。 |
| デバイスモデル | ユーザーのデバイスのモデル。 |
| Device Type | ユーザーが使用しているデバイスのタイプ。 |
| OS バージョン | デバイスのオペレーティングシステムのバージョン。 |
| ユーザーの言語 | ユーザーの言語を示す 2 文字の ISO-639-1 言語コード。 |

### [!UICONTROL ユーザーデータ]

次のユーザーデータは、必須フィールドではありません：

| フィールド名 | 説明 | 例 |
| --- | --- | --- | 
| [!UICONTROL メール] | ユーザー電子メールアドレス、またはユーザーアドレス電子メールの SHA256 ハッシュ。 | ebd543592...f2b7e1 |
| [!UICONTROL モバイル広告 ID] | ユーザーの「Google Advertising ID」(GAID) または「Apple for Advertisers」(IDFA) の Sha256 ハッシュ | ebd543592...f2b7e1 |
| [!UICONTROL クライアント IP アドレス] | ユーザーの IP アドレス。IPv4 形式または IPv6 形式を使用できます。 照合に使用されます。 | 192.168.0.1 |
| [!UICONTROL クライアントユーザーエージェント] | ユーザーの Web ブラウザーのユーザーエージェント文字列。 | Mozilla/5.0 (platform; rv:geckoversion) Gecko/geckotrail Firefox/firefoxversion |
| [!UICONTROL 顧客情報データ] | 他の顧客情報を含む JSON オブジェクト。 生の JSON を提供するか、シンプル化されたキーと値の入力セットを使用するかを選択します。 | { &quot;ph&quot;: &quot;122333445&quot; } |

![The [!DNL Pinterest] [!UICONTROL ユーザーデータ] ルールアクション内でハイライト表示されています。](../../../images/extensions/server/pinterest/user-data.png)

設定できる顧客情報プロパティは次のとおりです。

| フィールド名 | 説明 |
| --- | --- |
| Phone | ユーザーの連絡先番号。 数字のみを使用でき、国コード、市外局番、および数値を使用して入力する必要があります。 |
| 性別 | 性別は、「f」（女性の場合）、「m」（男性の場合）、または「n」（バイナリ以外の場合）のどちらかとして入力できます。 |
| 生年月日 | 生年月日（年、月、日として入力） |
| 姓 | ユーザーの姓。 |
| 名 | ユーザーの名。 |
| 市区町村 | ユーザーの居住都市。 これは主に請求目的で使用されます。 |
| 都道府県 | ユーザーの状態。2 文字のコードで小文字で指定されます。 |
| 郵便番号 | ユーザーの郵便番号。主に請求目的で使用されます。 |
| 国 | ユーザーの国を示す 2 文字の ISO-3166 国コード。 |
| 外部 ID | スペース内のユーザーを識別する広告主の一意の ID。 例えば、ユーザー ID、ロイヤリティー ID などです。 |
| クリック ID | ドメインの_epik cookie または URL の&amp;epik=クエリパラメーターに保存される一意の識別子。 |

>[!IMPORTANT]
>
>データをに送信する前に [!DNL Pinterest] API エンドポイントです。拡張機能は、電子メール、電話番号、名、姓、性別、生年月日、市区町村、都道府県、郵便番号、国、外部 ID の各フィールドの値をハッシュ化して標準化します。 SHA256 文字列が既に存在する場合、拡張機能はこれらのフィールドの値をハッシュ化しません。

### [!UICONTROL カスタムデータ]

ルールには、次のカスタムデータを入力できます。

| フィールド名 | 説明 |
| --- | --- |
| 通貨 | ISO-4217 通貨コード。 これを指定しない場合、 [!DNL Pinterest] は、デフォルトで、アカウント作成時に設定された広告主の通貨に設定されます。 |
| 値 | イベントの合計値。 リクエスト内の文字列として受け入れられます。 これは 2 桁の数字に解析されます。 |
| 検索文字列 | ユーザーコンバージョンイベントに関連する検索文字列。 |
| 注文 ID | 注文 ID。 送信中 `order_id` 役に立つ情報 [!DNL Pinterest] 必要に応じて、イベントの重複を排除します。 |
| 製品数 | イベントの製品の合計数。 例えば、チェックアウトイベントで購入された品目の合計数などです。 |
| コンテンツ ID | 製品 ID のリスト（配列）。 |
| 内容 | 価格や数量など、製品に関する情報を含むオブジェクトのリスト（配列）。 |

![The [!DNL Pinterest] [!UICONTROL カスタムデータ] ルールアクション内でハイライト表示されています。](../../../images/extensions/server/pinterest/custom-data.png)

## 内のデータの検証 [!DNL Pinterest]

イベント転送ルールを作成して実行したら、に送信されたイベントかどうかを検証します。 [!DNL Pinterest] API は、 [!DNL Pinterest] UI

イベント収集および [!DNL Experience Platform] 統合に成功した場合は、 [!DNL Pinterest] UI

![The [!DNL Pinterest] イベントマネージャー](../../../images/extensions/server/pinterest/event-history.png)

さらにドリルスルーして、 [!DNL Pinterest] イベントデータ配分。

![The [!DNL Pinterest] データ配分](../../../images/extensions/server/pinterest/event-history-distribution.png)

## 次の手順

このガイドでは、 [!DNL Pinterest] UI のイベント転送拡張機能。 詳しくは、次の公式ドキュメントを参照してください。

* [[!DNL Pinterest] API](https://developers.pinterest.com/docs/api/v5/)
* [[!DNL Pinterest] コンバージョン API の概要](https://help.pinterest.com/en/business/article/the-pinterest-api-for-conversions)
