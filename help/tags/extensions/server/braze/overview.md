---
keywords: イベント転送拡張機能；braze;braze イベント転送拡張機能
title: Braze イベント転送拡張機能
description: このAdobe Experience Platform イベント転送拡張機能は、Edge Networkイベントを Braze に送信します。
last-substantial-update: 2023-03-29T00:00:00Z
exl-id: 297f48f8-2c3b-41c2-8820-35f4558c67b3
source-git-commit: d81c4c8630598597ec4e253ef5be9f26c8987203
workflow-type: tm+mt
source-wordcount: '1692'
ht-degree: 3%

---

# [!DNL Braze Track Events API] イベント転送拡張機能

[[!DNL Braze]](https://www.braze.com) は、消費者とブランドの間の顧客中心インタラクションをリアルタイムで強化する顧客エンゲージメントプラットフォームです。 [!DNL Braze] を使用すると、次の操作を実行できます。

- 言語の好み、場所の好みなどに基づいてターゲットユーザーにデータ（マーケティングメッセージなど）を配信して、コンバージョン率を高め、主要なビジネス目標をサポートします。
- 顧客は、メール、プッシュ通知、アプリ内メッセージなど、複数のチャネルをまたいで、適切なタイミングで、好みの言語でパーソナライズされたメッセージを送信します。
- マーケティングおよびプロモーションキャンペーンの特定のユーザーをターゲットにして、リピート顧客の数を増やします。
- ユーザーの行動とパターンを調査し、カスタマイズされたメッセージで特定のオーディエンスをターゲットに設定します。これは、売上高の増加に役立つ可能性があります。

[!DNL Braze Track Events API] [&#x200B; イベント転送 &#x200B;](../../../ui/event-forwarding/overview.md) 拡張機能を使用すると、Adobe Experience Platform Edge Networkで取得したデータを活用したり、[[!DNL Braze User Track]](https://www.braze.com/docs/api/endpoints/user_data/post_user_track) API を使用してサーバーサイドイベントの形式で [!DNL Braze] に送信したりできます。

このドキュメントでは、拡張機能のユースケース、イベント転送ライブラリへのインストール方法およびイベント転送 [&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md) でその機能を使用する方法について説明します。

## ユースケース

この拡張機能は、Edge Networkのデータを使用して、カスタマー分析とターゲティング機能を活用 [!DNL Braze] る場合に使用します。

例えば、マルチチャネルプレゼンス（web サイトとモバイル）を持ち、web サイトやモバイルプラットフォームからイベントデータとしてトランザクション入力や会話入力を取り込む小売組織について考えてみます。 様々な [&#x200B; タグ &#x200B;](../../../home.md) ルールを使用して、このデータはリアルタイムでEdge Networkに送信されます。 ここから、[!DNL Braze] イベント転送拡張機能は、関連するイベントをサーバーサイドから [!DNL Braze] に自動的に送信します。

データが送信されたら、組織の分析チームはその機能を活用してデータセットを処理し、ビジネスインサイトを導き出して、グラフ、ダッシュボード、その他のビジュアライゼーションを生成し、ビジネス関係者に情報を提供で [!DNL Braze's] ます。 プラットフォームの様々なユースケースについて詳しくは、[[!DNL Braze]  顧客 &#x200B;](https://www.braze.com/customers) ページを参照してください。

## [!DNL Braze] の前提条件とガードレール {#prerequisites}

そのテクノロジーを使用するには、[!DNL Braze] アカウントが必要です。 アカウントをお持ちでない場合は、[!DNL Braze] の [&#x200B; 基本を学ぶ &#x200B;](https://www.braze.com/get-started/) ページに移動し、[!DNL Braze Sales] に接続してアカウント作成プロセスを開始します。

### API ガードレール

この拡張機能では、[!DNL Braze] の API のうち 2 つを使用します。その制限について以下に概説します。

| API | レート制限 |
| --- | --- |
| [!DNL User Track] | 1 分あたり 50,000 リクエスト。 <br> 詳しくは、[[!DNL User Track] API ドキュメント &#x200B;](https://www.braze.com/docs/api/endpoints/user_data/post_user_track#rate-limit) を参照してください。 |
| [!DNL User Identify] | 1 分あたり 20,000 リクエスト。 <br> 詳しくは、[[!DNL User Identify] API ドキュメント &#x200B;](https://www.braze.com/docs/api/endpoints/user_data/post_user_identify#rate-limit) を参照してください。 |

>[!NOTE]
>
> 適用される制限について詳しくは、[[!DNL Braze] API 制限 &#x200B;](https://www.braze.com/docs/api/api_limits/) に関するガイドを参照してください。

### 請求可能なデータポイント

追加のカスタム属性を [!DNL Braze] に送信すると、[!DNL Braze] データポイントの消費量が増える可能性があります。 追加のカスタム属性を送信する前に、[!DNL Braze] アカウントマネージャーにお問い合わせください。 詳しくは、[&#x200B; 請求可能なデータポイント &#x200B;](https://www.braze.com/docs/user_guide/data_and_analytics/data_points/?tab=billable) に関する [!DNL Braze] のドキュメントを参照してください。

### 必要な設定の詳細の収集 {#configuration-details}

Edge Networkを [!DNL Braze] に接続するには、次の入力が必要です。

| キータイプ | 説明 | 例 |
| --- | --- | --- |
| [!DNL Braze] Instance | [!DNL Braze] アカウントに関連付けられた REST エンドポイント。 詳しくは、[&#x200B; インスタンス &#x200B;](https://www.braze.com/docs/user_guide/administrative/access_braze/sdk_endpoints) に関する [!DNL Braze] のドキュメントを参照してください。 | `https://rest.iad-03.braze.com` |
| API キー | [!DNL Braze] アカウントに関連付けられた [!DNL Braze] API キー。 <br/> 詳しくは、[REST API キー &#x200B;](https://www.braze.com/docs/api/basics/#rest-api-key) に関する [!DNL Braze] ドキュメントを参照してください。 | `YOUR-BRAZE-REST-API-KEY` |

### 秘密鍵の作成

新しい [&#x200B; イベント転送の秘密鍵 &#x200B;](../../../ui/event-forwarding/secrets.md) を作成し、値を [[!DNL Braze] API キー &#x200B;](#configuration-details) に設定します。 これは、値を保護しながら、アカウントへの接続を認証するために使用されます。

## [!DNL Braze] 拡張機能のインストールと設定 {#install}

拡張機能をインストールするには、[&#x200B; イベント転送プロパティを作成 &#x200B;](../../../ui/event-forwarding/overview.md#properties) するか、代わりに編集する既存のプロパティを選択します。

左側のナビゲーションの「**[!UICONTROL 拡張機能]**」をクリックします。「**[!UICONTROL カタログ]**」タブで、[!DNL Braze] 拡張機能のカードの **[!UICONTROL インストール]** を選択します。

![[!DNL Braze] 拡張機能をインストールします。](../../../images/extensions/server/braze/install-extension.png)

次の画面で、以前に [!DNL Braze] から収集した次の [&#x200B; 設定値 &#x200B;](#configuration-details) を入力します。

- **[!UICONTROL Braze Rest エンドポイント URL]**:[!DNL Braze] REST エンドポイント URL の値を、指定された入力にプレーンテキストとして入力できます。
- **[!UICONTROL API キー]**:[!DNL Braze] API キーを含む、以前に作成した [&#x200B; 秘密鍵データ要素 &#x200B;](#create-a-secret) を選択します。

完了したら、「**[!UICONTROL 保存]**」をクリックします。

![[!DNL Braze] 拡張機能の設定ページ &#x200B;](../../../images/extensions/server/braze/configure-extension.png)

## [!DNL Send Event] ルールの作成 {#tracking-rule}

拡張機能をインストールした後、新しいイベント転送 [&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md) を作成し、必要に応じてその条件を設定します。 ルールのアクションを設定する場合は、**[!UICONTROL Braze]** 拡張機能を選択してから、アクションタイプとして **[!UICONTROL イベントを送信]** を選択します。

![&#x200B; イベント転送ルールアクション設定を追加します &#x200B;](../../../images/extensions/server/braze/braze-event-action.png)。

**[!UICONTROL ユーザーの識別]**

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL &#x200B; 外部ユーザー ID] | 長く、ランダムで、分散した UUID または GUID です。 ユーザー ID に名前を付ける別の方法を選択する場合は、長く、ランダムで、分散している必要があります。 詳しくは、[&#x200B; 推奨されるユーザー ID 命名規則 &#x200B;](https://www.braze.com/docs/developer_guide/platform_integration_guides/web/analytics/setting_user_ids#suggested-user-id-naming-convention) を参照してください。 |
| [!UICONTROL Braze ユーザー ID] | Braze ユーザー識別子。 |
| [!UICONTROL &#x200B; ユーザーエイリアス &#x200B;] | エイリアスは、一意のユーザー ID の代替として機能します。 エイリアスを使用して、コアユーザー ID とは異なるディメンションに沿ってユーザーを識別します。 <br><br> ユーザーの alias オブジェクトは、識別子自体の alias_name と、エイリアスのタイプを示す alias_label の 2 つの部分で構成されています。 ユーザーは、異なるラベルを持つ複数のエイリアスを持つことができますが、alias_label ごとに 1 つの alias_name のみです。 |

{style="table-layout:auto"}

>[!NOTE]
>
> ユーザーにイベントを結び付けるには、「[!UICONTROL &#x200B; 外部ユーザー ID]」フィールド、「[!UICONTROL Braze ユーザー識別子 &#x200B;]」フィールドまたは「[!UICONTROL &#x200B; ユーザーエイリアス &#x200B;]」セクションのいずれかを入力する必要があります。

**[!UICONTROL イベントデータ]**

| 入力 | 説明 | 必須 |
| --- | --- | --- |
| [!UICONTROL &#x200B; イベント名&#x200B;] | イベントの名前。 | ○ |
| [!UICONTROL &#x200B; イベント時間 &#x200B;] | ISO 8601 形式または `yyyy-MM-dd'T'HH:mm:ss:SSSZ` 形式の文字列としての日時。 | ○ |
| [!UICONTROL &#x200B; アプリ識別子 &#x200B;] | アプリ識別子（<strong>app_id</strong> は、アクティビティをアプリグループ内の特定のアプリに関連付けるパラメーターです。 操作するアプリグループ内のアプリを指定します。 [API 識別子のタイプ &#x200B;](https://www.braze.com/docs/api/identifier_types/) について詳しく説明します。 | |
| [!UICONTROL &#x200B; イベントのプロパティ &#x200B;] | イベントのカスタムプロパティを含む JSON オブジェクト。 |  |

{style="table-layout:auto"}

>[!NOTE]
>
> **[!UICONTROL Braze イベントの送信]** アクションでは、**[!UICONTROL イベント名]** と **[!UICONTROL イベント時間]** のみを指定する必要がありますが、カスタムプロパティフィールドにはできるだけ多くの情報を含める必要があります。 [!DNL Braze] イベントオブジェクトについて詳しくは、[&#x200B; 公式ドキュメント &#x200B;](https://www.braze.com/docs/api/objects_filters/event_object/) を参照してください。

**[!UICONTROL ユーザー属性]**

ユーザー属性は、指定されたユーザープロファイル上で、指定された名前と値で属性を作成または更新するフィールドを含む JSON オブジェクトにすることができます。 次のプロパティがサポートされています。

| ユーザー属性 | 説明 |
| --- | --- |
| [!UICONTROL &#x200B; 名 &#x200B;] | |
| [!UICONTROL &#x200B; 姓 &#x200B;] | |
| [!UICONTROL 電話] | |
| [!UICONTROL メール] | |
| [!UICONTROL &#x200B; 性別 &#x200B;] | 次の文字列の 1 つ：「M」、「F」、「O」（その他）、「N」（該当なし）、「P」（言いたくない）。 |
| [!UICONTROL &#x200B; 市区町村 &#x200B;] | |
| [!UICONTROL 国] | [ISO-3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) 形式の文字列としての国。 |
| [!UICONTROL 言語] | [ISO-639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 形式の文字列としての言語。 |
| [!UICONTROL &#x200B; 生年月日 &#x200B;] | 「YYYY-MM-DD」形式の文字列（例：1980-12-21）。 |
| [!UICONTROL タイムゾーン] | [IANA タイムゾーンデータベース &#x200B;](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) からのタイムゾーン名（「アメリカ/ニューヨーク」や「東部時間（米国およびカナダ）」など）。 |
| [!UICONTROL Facebook] | ID （文字列）、likes （文字列の配列）、num_friends （整数）のいずれかを含むハッシュ。 |
| [!UICONTROL Twitter] | ID （整数）、screen_name （文字列、Twitterハンドル）、followers_count （整数）、friends_count （整数）、states_count （整数）のいずれかを含むハッシュ。 |

{style="table-layout:auto"}

## [!DNL Send Purchase Event] ルールの作成 {#purchase-rule}

拡張機能をインストールした後、新しいイベント転送 [&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md) を作成し、必要に応じてその条件を設定します。 ルールのアクションを設定する際に、**[!UICONTROL Braze]** 拡張機能を選択してから、アクションタイプに **[!UICONTROL 購入イベントを送信]** を選択します。

![Braze 購入アクションタイプのイベント転送ルールアクション設定を追加します。](../../../images/extensions/server/braze/braze-purchase-event-action.png)

**[!UICONTROL ユーザーの識別]**

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL &#x200B; 外部ユーザー ID] | 長く、ランダムで、分散した UUID または GUID です。 ユーザー ID に名前を付ける別の方法を選択する場合は、長く、ランダムで、分散している必要があります。 詳しくは、[&#x200B; 推奨されるユーザー ID 命名規則 &#x200B;](https://www.braze.com/docs/developer_guide/platform_integration_guides/web/analytics/setting_user_ids#suggested-user-id-naming-convention) を参照してください。 |
| [!UICONTROL Braze ユーザー ID] | Braze ユーザー識別子。 |
| [!UICONTROL &#x200B; ユーザーエイリアス &#x200B;] | エイリアスは、一意のユーザー ID の代替として機能します。 エイリアスを使用して、コアユーザー ID とは異なるディメンションに沿ってユーザーを識別します。 <br><br> ユーザーの alias オブジェクトは、識別子自体の alias_name と、エイリアスのタイプを示す alias_label の 2 つの部分で構成されています。 ユーザーは、異なるラベルを持つ複数のエイリアスを持つことができますが、alias_label ごとに 1 つの alias_name のみです。 |

{style="table-layout:auto"}

>[!NOTE]
>
> イベントをユーザーにリンクするには、「[!UICONTROL &#x200B; 外部ユーザー ID]」フィールド、「[!UICONTROL Braze ユーザー識別子 &#x200B;]」フィールド、「[!UICONTROL &#x200B; ユーザーエイリアス &#x200B;]」セクションのいずれかを入力する必要があります。

**[!UICONTROL 購入データ]**

| 入力 | 説明 | 必須 |
| --- | --- | --- |
| [!UICONTROL &#x200B; 製品 ID &#x200B;] | 購入の識別子。 （例：製品名または製品カテゴリ） | ○ |
| [!UICONTROL &#x200B; 購入時間 &#x200B;] | ISO 8601 形式または `yyyy-MM-dd'T'HH:mm:ss:SSSZ` 形式の文字列としての日時。 | ○ |
| [!UICONTROL &#x200B; 通貨&#x200B;] | [ISO 4217](https://ja.wikipedia.org/wiki/ISO_4217) アルファベット通貨コード形式の文字列としての通貨。 | ○ |
| [!UICONTROL &#x200B; 価&#x200B;] | 価格： | ○ |
| [!UICONTROL &#x200B; 数量&#x200B;] | 指定しない場合、デフォルト値は 1 になります。 最大値は 100 未満である必要があります。 | |
| [!UICONTROL &#x200B; アプリ識別子 &#x200B;] | アプリ識別子（<strong>app_id</strong> は、アクティビティをアプリグループ内の特定のアプリに関連付けるパラメーターです。 操作するアプリグループ内のアプリを指定します。 [API 識別子のタイプ &#x200B;](https://www.braze.com/docs/api/identifier_types/) について詳しく説明します。 | |
| [!UICONTROL &#x200B; 購入プロパティ &#x200B;] | 購入のカスタムプロパティを含む JSON オブジェクト。 |  |

{style="table-layout:auto"}

>[!NOTE]
>
> **[!UICONTROL Braze イベントの送信]** アクションでは、**[!UICONTROL イベント名]** と **[!UICONTROL イベント時間]** のみを指定する必要がありますが、カスタムプロパティフィールドにはできるだけ多くの情報を含める必要があります。 [!DNL Braze] イベントオブジェクトについて詳しくは、[&#x200B; 公式ドキュメント &#x200B;](https://www.braze.com/docs/api/objects_filters/event_object/) を参照してください。

**[!UICONTROL ユーザー属性]**

ユーザー属性は、指定されたユーザープロファイル上で、指定された名前と値で属性を作成または更新するフィールドを含む JSON オブジェクトにすることができます。 次のプロパティがサポートされています。

| ユーザー属性 | 説明 |
| --- | --- |
| [!UICONTROL &#x200B; 名 &#x200B;] | |
| [!UICONTROL &#x200B; 姓 &#x200B;] | |
| [!UICONTROL 電話] | |
| [!UICONTROL メール] | |
| [!UICONTROL &#x200B; 性別 &#x200B;] | 次の文字列の 1 つ：「M」、「F」、「O」（その他）、「N」（該当なし）、「P」（言いたくない）。 |
| [!UICONTROL &#x200B; 市区町村 &#x200B;] | |
| [!UICONTROL 国] | [ISO-3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) 形式の文字列としての国。 |
| [!UICONTROL 言語] | [ISO-639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) 形式の文字列としての言語。 |
| [!UICONTROL &#x200B; 生年月日 &#x200B;] | 「YYYY-MM-DD」形式の文字列（例：1980-12-21）。 |
| [!UICONTROL タイムゾーン] | [IANA タイムゾーンデータベース &#x200B;](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) からのタイムゾーン名（「アメリカ/ニューヨーク」や「東部時間（米国およびカナダ）」など）。 |
| [!UICONTROL Facebook] | ID （文字列）、likes （文字列の配列）、num_friends （整数）のいずれかを含むハッシュ。 |
| [!UICONTROL Twitter] | ID （整数）、screen_name （文字列、Twitterハンドル）、followers_count （整数）、friends_count （整数）、states_count （整数）のいずれかを含むハッシュ。 |

{style="table-layout:auto"}

## [!DNL Braze] 内のデータの検証 {#validate}

イベントの収集と [!DNL Adobe Experience Platform] の統合が成功した場合は、[&#x200B; ユーザープロファイルの表示 &#x200B;](https://www.braze.com/docs/user_guide/engagement_tools/segments/user_profiles/) 時に [!DNL Braze] コンソール内にイベントが表示されます。 特に、[!DNL Braze] に送信された新しいイベントデータは、特定のユーザーの [&#x200B; 概要タブ &#x200B;](https://www.braze.com/docs/user_guide/engagement_tools/segments/user_profiles/#overview-tab) の [!DNL Purchases] セクションに反映されます。

## 次の手順

このガイドでは、イベント転送を使用してコンバージョンイベントを [!DNL Braze] に送信する方法について説明しました。 [!DNL Braze] に送信されるイベントデータのダウンストリームアプリケーションについて詳しくは、[&#x200B; 公式ドキュメント &#x200B;](https://www.braze.com/docs) を参照してください。

Experience Platformのイベント転送機能について詳しくは、[&#x200B; イベント転送の概要 &#x200B;](../../../ui/event-forwarding/overview.md) を参照してください。
