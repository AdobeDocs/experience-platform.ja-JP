---
keywords: イベント転送拡張機能；braze;braze イベント転送拡張機能
title: Braze イベント転送拡張機能
description: このAdobe Experience Platformイベント転送拡張機能は、Adobe Experience Edge ネットワークイベントを Braze に送信します。
source-git-commit: 88e589eb17c249a8bdc82fe7a041a5581a60c7e6
workflow-type: tm+mt
source-wordcount: '1874'
ht-degree: 5%

---


# [!DNL Braze Track Events API] イベント転送拡張機能

[[!DNL Braze]](https://www.braze.com) は、消費者とブランド間の顧客中心のインタラクションをリアルタイムで強化する顧客エンゲージメントプラットフォームです。 使用 [!DNL Braze]を使用すると、次の操作を実行できます。

- 言語の好みや場所の好みなどに基づいてターゲットユーザーにデータ（マーケティングメッセージなど）を配信し、コンバージョン率を高め、主要なビジネス目標をサポートします。
- 電子メール、プッシュ通知、アプリ内メッセージなど、複数のチャネルをまたいで、お客様向けにパーソナライズされたメッセージを、ちょうど適切なタイミングで、お客様の希望の言語で送信できます。
- マーケティングキャンペーンやプロモーションキャンペーンの特定のユーザーをターゲットにして、リピート顧客数を増やします。
- ユーザーの行動とパターンを調べ、カスタマイズされたメッセージを使用して特定のオーディエンスをターゲット設定し、売上高の増加に役立てます。

この [!DNL Braze Track Events API] [イベント転送](../../../ui/event-forwarding/overview.md) 拡張機能を使用すると、 Adobe Experience Platform Edge Network で取得したデータを活用し、に送信できます。 [!DNL Braze] を使用して、サーバー側のイベントの形式で [[!DNL Braze User Identify]](https://www.braze.com/docs/api/endpoints/user_data/post_user_identify) および [[!DNL Braze User Track]](https://www.braze.com/docs/api/endpoints/user_data/post_user_track) API

このドキュメントでは、拡張機能の使用例、イベント転送ライブラリでのインストール方法、イベント転送での拡張機能の使用方法について説明します [ルール](../../../ui/managing-resources/rules.md).

## ユースケース

この拡張機能は、 [!DNL Braze] を使用して、顧客分析およびターゲティング機能を活用します。

例えば、マルチチャネルの存在（Web サイトとモバイル）があり、Web サイトやモバイルプラットフォームからイベントデータとしてトランザクション入力や会話入力を取り込む小売組織について考えてみましょう。 様々な [タグ](../../../home.md) ルールを使用する場合、このデータはリアルタイムで Edge ネットワークに送信されます。 ここから、 [!DNL Braze] イベント転送拡張機能は、関連イベントを [!DNL Braze] をサーバー側から削除します。

データを送信したら、組織の分析チームは [!DNL Braze's] データセットを処理し、ビジネスインサイトを導き出してグラフ、ダッシュボード、その他のビジュアライゼーションを生成し、ビジネス関係者に通知する機能です。 詳しくは、 [[!DNL Braze] 顧客](https://www.braze.com/customers) ページを参照してください。

## [!DNL Braze] 前提条件とガードレール {#prerequisites}

次をお持ちの場合は、 [!DNL Braze] その技術を使用するためのアカウント。 アカウントがない場合は、 [はじめにページ](https://www.braze.com/get-started/) オン [!DNL Braze] 接続する [!DNL Braze Sales] アカウント作成プロセスを開始します。

### API ガードレール

拡張機能では、次の 2 つを使用します。 [!DNL Braze]の API とその制限について、以下で概要を説明します。

| API | レート制限 |
| --- | --- |
| [!DNL User Track] | 1 分あたり 50,000 件のリクエスト。 <br>詳しくは、 [[!DNL User Track] API ドキュメント](https://www.braze.com/docs/api/endpoints/user_data/post_user_track#rate-limit) 」を参照してください。 |
| [!DNL User Identify] | 1 分あたり 20,000 件のリクエスト。 <br>詳しくは、 [[!DNL User Identify] API ドキュメント](https://www.braze.com/docs/api/endpoints/user_data/post_user_identify#rate-limit) 」を参照してください。 |

>[!NOTE]
>
> に関するガイドを参照してください。 [[!DNL Braze] API の制限](https://www.braze.com/docs/api/api_limits/) 制限の詳細については、

### 課金対象のデータポイント

追加のカスタム属性の送信先 [!DNL Braze] を増やすかもしれません [!DNL Braze] データポイントの使用。 詳しくは、 [!DNL Braze] 追加のカスタム属性を送信する前に、アカウントマネージャーに問い合わせてください。 詳しくは、 [!DNL Braze] ドキュメント [課金対象のデータポイント](https://www.braze.com/docs/user_guide/onboarding_with_braze/data_points/#billable-data-points) を参照してください。

### 必要な設定の詳細の収集 {#configuration-details}

Edge ネットワークをに接続するには [!DNL Braze]の場合、次の入力が必要です。

| キータイプ | 説明 | 例 |
| --- | --- | --- |
| [!DNL Braze] インスタンス | に関連付けられた REST エンドポイント [!DNL Braze] アカウント 詳しくは、 [!DNL Braze] ドキュメント [インスタンス](https://www.braze.com/docs/user_guide/administrative/access_braze/braze_instances) 指導のために | `https://rest.iad-03.braze.com` |
| API キー | この [!DNL Braze] に関連付けられた API キー [!DNL Braze] アカウント <br/>詳しくは、 [!DNL Braze] に関するドキュメント [REST API キー](https://www.braze.com/docs/api/basics/#rest-api-key) 指導のために | `YOUR-BRAZE-REST-API-KEY` |

### 秘密鍵の作成

新しい [イベント転送秘密鍵](../../../ui/event-forwarding/secrets.md) の値を [[!DNL Braze] API キー](#configuration-details). これは、値のセキュリティを維持しながら、アカウントへの接続を認証するために使用されます。

## のインストールと設定 [!DNL Braze] 拡張 {#install}

拡張機能をインストールするには、以下を実行します。 [イベント転送プロパティの作成](../../../ui/event-forwarding/overview.md#properties) または、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL 拡張機能]**」をクリックします。内 **[!UICONTROL カタログ]** タブ、選択 **[!UICONTROL インストール]** ～のためのカードで [!DNL Braze] 拡張子。

![[!DNL Braze]拡張機能のインストール.](../../../images/extensions/server/braze/install-extension.png)

次の画面で、次の情報を入力します。 [設定値](#configuration-details) 以前に集めた [!DNL Braze]:

- **[!UICONTROL Rest エンドポイント URL をブレーズ]**:この [!DNL Braze] rest エンドポイント URL を指定された入力内のプレーンテキストとして指定します。
- **[!UICONTROL API キー]**:を選択します。 [シークレットデータ要素](#create-a-secret) 先ほど作成したが、 [!DNL Braze] API キー。

完了したら、「**[!UICONTROL 保存]**」をクリックします。

![この [!DNL Braze] 拡張機能の設定ページ。](../../../images/extensions/server/braze/configure-extension.png)

## の作成 [!DNL Send Event] ルール {#tracking-rule}

拡張機能をインストールしたら、新しいイベント転送を作成します [ルール](../../../ui/managing-resources/rules.md) 必要に応じて、条件を設定します。 ルールのアクションを設定する際に、 **[!UICONTROL ブレーズ]** 拡張機能、「 **[!UICONTROL イベントの送信]** （アクションタイプ）。

![イベント転送ルールのアクション設定を追加します。](../../../images/extensions/server/braze/braze-event-action.png)

**[!UICONTROL ユーザー ID]**

| 必要情報 | 説明 |
| --- | --- |
| [!UICONTROL 外部ユーザー ID] | 長く、ランダムで分散された UUID または GUID。 別の方法でユーザー ID に名前を付ける場合は、長い、ランダムで、十分に分散されている必要もあります。 詳細情報： [推奨されるユーザー ID の命名規則](https://www.braze.com/docs/developer_guide/platform_integration_guides/web/analytics/setting_user_ids#suggested-user-id-naming-convention). |
| [!UICONTROL ユーザー ID をブレーズ] | ユーザー識別子をブレーズします。 |
| [!UICONTROL ユーザーエイリアス] | エイリアスは、別の一意のユーザー識別子として機能します。 エイリアスを使用して、コアユーザー ID とは異なるディメンションでユーザーを識別します。 <br><br> user alias オブジェクトは、次の 2 つの部分で構成されます。識別子自体の alias_name と、エイリアスのタイプを示す alias_label。 ユーザーは異なるラベルを持つ複数の別名を持つことができますが、 alias_label 1 つにつき alias_name は 1 つだけです。 |

{style="table-layout:auto"}

>[!NOTE]
>
> イベントをユーザーに提供する場合は、 [!UICONTROL 外部ユーザー ID] フィールド、または [!UICONTROL ユーザー識別子を編集] フィールドまたは [!UICONTROL ユーザーエイリアス] 」セクションに入力します。

**[!UICONTROL イベントデータ]**

| 必要情報 | 説明 | 必須 |
| --- | --- | --- |
| [!UICONTROL イベント名 &#x200B;] | イベントの名前。 | ○ |
| [!UICONTROL イベント時刻 ] | ISO 8601 またはでの文字列としての日時 `yyyy-MM-dd'T'HH:mm:ss:SSSZ` 形式 | ○ |
| [!UICONTROL アプリ識別子] | アプリの識別子または <strong>app_id</strong> は、アクティビティをアプリグループ内の特定のアプリに関連付けるパラメーターです。 操作しているアプリグループ内のアプリを指定します。 詳しくは、 [API 識別子のタイプ](https://www.braze.com/docs/api/identifier_types/). |  |
| [!UICONTROL イベントのプロパテ&#x200B;ィ] | イベントのカスタムプロパティを含む JSON オブジェクト。 |  |

{style="table-layout:auto"}

>[!NOTE]
>
> この **[!UICONTROL 送信イベントをブレーズ]** アクションに必要なのは **[!UICONTROL イベント名]** および **[!UICONTROL イベント時刻]** を指定する必要がありますが、カスタムプロパティフィールドにできる限り多くの情報を含める必要があります。 詳しくは、 [!DNL Braze] イベントオブジェクト ( [公式ドキュメント](https://www.braze.com/docs/api/objects_filters/event_object/).

**[!UICONTROL ユーザー属性]**

ユーザー属性は、指定したユーザープロファイルで指定した名前と値で属性を作成または更新するフィールドを含む JSON オブジェクトにすることができます。 次のプロパティがサポートされています。

| ユーザー属性 | 説明 |
| --- | --- |
| [!UICONTROL 名] |  |
| [!UICONTROL 姓] |  |
| [!UICONTROL Phone] |  |
| [!UICONTROL メール] |  |
| [!UICONTROL 性別] | 次のいずれかの文字列：&quot;M&quot;、&quot;F&quot;、&quot;O&quot;（その他）、&quot;N&quot;（該当なし）、&quot;P&quot;（言わない方が良い）。 |
| [!UICONTROL 市区町村] |  |
| [!UICONTROL 国] | 国を文字列として [ISO-3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) 形式 |
| [!UICONTROL 言語] | の文字列としての言語 [ISO-639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 形式 |
| [!UICONTROL 生年月日] | 形式「YYYY-MM-DD」の文字列 ( 例：1980-12-21)。 |
| [!UICONTROL タイムゾーン] | タイムゾーン名： [IANA タイムゾーンデータベース](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) ( 例：「アメリカ/ニューヨーク」または「東部標準時（米国およびカナダ）」)。 |
| [!UICONTROL Facebook] | id （文字列）、likes （文字列の配列）、num_friends （整数）のいずれかを含むハッシュ。 |
| [!UICONTROL Twitter] | id（整数）、screen_name( 文字列、Twitterハンドル )、followers_count（整数）、friends_count（整数）、statuses_count（整数）のいずれかを含むハッシュ。 |

{style="table-layout:auto"}

## の作成 [!DNL Send Purchase Event] ルール {#purchase-rule}

拡張機能をインストールしたら、新しいイベント転送を作成します [ルール](../../../ui/managing-resources/rules.md) 必要に応じて、条件を設定します。 ルールのアクションを設定する際に、 **[!UICONTROL ブレーズ]** 拡張機能、「 **[!UICONTROL 購入イベントの送信]** （アクションタイプ）。

![「Braze Purchase」アクションタイプのイベント転送ルールアクション設定を追加します。](../../../images/extensions/server/braze/braze-purchase-event-action.png)

**[!UICONTROL ユーザー ID]**

| 必要情報 | 説明 |
| --- | --- |
| [!UICONTROL 外部ユーザー ID] | 長く、ランダムで分散された UUID または GUID。 別の方法でユーザー ID に名前を付ける場合は、長い、ランダムで、十分に分散されている必要もあります。 詳細情報： [推奨されるユーザー ID の命名規則](https://www.braze.com/docs/developer_guide/platform_integration_guides/web/analytics/setting_user_ids#suggested-user-id-naming-convention). |
| [!UICONTROL ユーザー ID をブレーズ] | ユーザー識別子をブレーズします。 |
| [!UICONTROL ユーザーエイリアス] | エイリアスは、別の一意のユーザー識別子として機能します。 エイリアスを使用して、コアユーザー ID とは異なるディメンションでユーザーを識別します。 <br><br> user alias オブジェクトは、次の 2 つの部分で構成されます。識別子自体の alias_name と、エイリアスのタイプを示す alias_label。 ユーザーは異なるラベルを持つ複数の別名を持つことができますが、 alias_label 1 つにつき alias_name は 1 つだけです。 |

{style="table-layout:auto"}

>[!NOTE]
>
> イベントをユーザーにリンクするには、 [!UICONTROL 外部ユーザー ID] フィールド、 [!UICONTROL ユーザー識別子を編集] フィールド、または [!UICONTROL ユーザーエイリアス] 」セクションに入力します。

**[!UICONTROL 購入データ]**

| 必要情報 | 説明 | 必須 |
| --- | --- | --- |
| [!UICONTROL 製品 ID &#x200B;] | 購入の識別子。 （例：製品名または製品カテゴリ） | ○ |
| [!UICONTROL 購入時間 ] | ISO 8601 またはでの文字列としての日時 `yyyy-MM-dd'T'HH:mm:ss:SSSZ` 形式 | ○ |
| [!UICONTROL 通貨 &#x200B;] | 通貨を文字列として [ISO 4217](https://ja.wikipedia.org/wiki/ISO_4217) 英字通貨コード形式。 | ○ |
| [!UICONTROL 価格 &#x200B;] | 価格. | ○ |
| [!UICONTROL 数量 &#x200B;] | 指定しない場合、デフォルト値は 1 です。 最大値は 100 より小さい値にする必要があります。 |  |
| [!UICONTROL アプリ識別子] | アプリの識別子または <strong>app_id</strong> は、アクティビティをアプリグループ内の特定のアプリに関連付けるパラメーターです。 操作しているアプリグループ内のアプリを指定します。 詳しくは、 [API 識別子のタイプ](https://www.braze.com/docs/api/identifier_types/). |  |
| [!UICONTROL 購入プロパティ&#x200B;] | 購入のカスタムプロパティを含む JSON オブジェクト。 |  |

{style="table-layout:auto"}

>[!NOTE]
>
> この **[!UICONTROL 送信イベントをブレーズ]** アクションに必要なのは **[!UICONTROL イベント名]** および **[!UICONTROL イベント時刻]** を指定する必要がありますが、カスタムプロパティフィールドにできる限り多くの情報を含める必要があります。 詳しくは、 [!DNL Braze] イベントオブジェクト ( [公式ドキュメント](https://www.braze.com/docs/api/objects_filters/event_object/).

**[!UICONTROL ユーザー属性]**

ユーザー属性は、指定したユーザープロファイルで指定した名前と値で属性を作成または更新するフィールドを含む JSON オブジェクトにすることができます。 次のプロパティがサポートされています。

| ユーザー属性 | 説明 |
| --- | --- |
| [!UICONTROL 名] |  |
| [!UICONTROL 姓] |  |
| [!UICONTROL Phone] |  |
| [!UICONTROL メール] |  |
| [!UICONTROL 性別] | 次のいずれかの文字列：&quot;M&quot;、&quot;F&quot;、&quot;O&quot;（その他）、&quot;N&quot;（該当なし）、&quot;P&quot;（言わない方が良い）。 |
| [!UICONTROL 市区町村] |  |
| [!UICONTROL 国] | 国を文字列として [ISO-3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) 形式 |
| [!UICONTROL 言語] | の文字列としての言語 [ISO-639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 形式 |
| [!UICONTROL 生年月日] | 形式「YYYY-MM-DD」の文字列 ( 例：1980-12-21)。 |
| [!UICONTROL タイムゾーン] | タイムゾーン名： [IANA タイムゾーンデータベース](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) ( 例：「アメリカ/ニューヨーク」または「東部標準時（米国およびカナダ）」)。 |
| [!UICONTROL Facebook] | id （文字列）、likes （文字列の配列）、num_friends （整数）のいずれかを含むハッシュ。 |
| [!UICONTROL Twitter] | id（整数）、screen_name( 文字列、Twitterハンドル )、followers_count（整数）、friends_count（整数）、statuses_count（整数）のいずれかを含むハッシュ。 |

{style="table-layout:auto"}

## 内のデータの検証 [!DNL Braze] {#validate}

イベント収集および [!DNL Adobe Experience Platform] 統合に成功した場合は、 [!DNL Braze] コンソールの場合 [ユーザープロファイルの表示](https://www.braze.com/docs/user_guide/engagement_tools/segments/user_profiles/). 特に、に送信される新しいイベントデータ [!DNL Braze] が [!DNL Purchases] 特定のユーザーの [「概要」タブ](https://www.braze.com/docs/user_guide/engagement_tools/segments/user_profiles/#overview-tab).

## 次の手順

このガイドでは、コンバージョンイベントをに送信する方法について説明しました。 [!DNL Braze] イベント転送を使用しています。 に送信されたイベントデータのダウンストリームアプリケーションの詳細 [!DNL Braze]（を参照） [公式ドキュメント](https://www.braze.com/docs).

Experience Platformのイベント転送機能について詳しくは、 [イベント転送の概要](../../../ui/event-forwarding/overview.md).
