---
title: LiveRamp - Distribution Connection
description: LiveRamp - Distribution コネクタを使用して、以前 LiveRamp に転送されたオーディエンスを、ダウンストリーム広告の宛先に合わせて調整およびアクティブ化する方法について説明します。
source-git-commit: b97694dcf0d7fd600c6aada9600364e99ffe0902
workflow-type: tm+mt
source-wordcount: '2716'
ht-degree: 32%

---


# [!DNL LiveRamp - Distribution] 接続

The [!DNL LiveRamp - Distribution] 接続を使用すると、モバイル、Web、ディスプレイ、接続されたテレビメディアをまたいで、Experience Platformからプレミアムパブリッシャーに対してオーディエンスをアクティブ化できます。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、LiveRamp によって作成および管理されます。 お問い合わせや更新リクエストについては、LiveRamp に直接お問い合わせください。 [ここ](mailto:adobertcdp@liveramp.com).

## サポートされる宛先 {#supported-destinations}

[!DNL LiveRamp - Distribution] は、現在、次のプラットフォームに対するオーディエンスのアクティベーションをサポートしています。

* [[!DNL 4C Insights]](#insights)
* [[!DNL Acast]](#acast)
* [[!DNL Nexxen]](#nexxen)
* [[!DNL Ampersand.tv]](#ampersand-tv)
* [[!DNL Captify]](#captify)
* [[!DNL Cardlytics]](#cardlytics)
* [[!DNL Disney (Hulu/ESPN/ABC)]](#disney)
* [[!DNL iHeartMedia]](#iheartmedia)
* [[!DNL Index Exchange]](#index-exchange)
* [[!DNL Magnite CTV Platform]](#magnite)
* [[!DNL Magnite DV+ (Rubicon Project)]](#magnite-dv)
* [[!DNL One Fox]](#fox)
* [[!DNL Pandora]](#pandora)
* [[!DNL Reddit]](#reddit)
* [[!DNL Roku]](#roku)
* [[!DNL Spotify]](#spotify)
* [[!DNL Taboola]](#taboola)
* [[!DNL TargetSpot]](#targetspot)
* [[!DNL Teads]](#teads)
* [[!DNL WB Discovery]](#wb-discovery)

## ユースケース {#use-cases}

[!DNL LiveRamp - Distribution] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

スポーツアパレル小売業者のマーケティングチームが、 [LiveRamp - Onboarding](liveramp-onboarding.md) 接続を使用して、Experience Platformから LiveRamp アカウントにオーディエンスを送信する。

を通じて [!DNL LiveRamp - Distribution] 接続を使用して、オンボードオーディエンスのアクティベーションを、 [サポートされる宛先](#supported-destinations). その後、モバイル、オープン Web、ソーシャル、および [!DNL CTV] プラットフォーム。

## オーディエンスを LiveRamp にオンボーディングする {#onboarding}

を通じてオーディエンスをアクティブ化する前に [!DNL LiveRamp - Distribution] 接続、 [LiveRamp - Onboarding](liveramp-onboarding.md) 接続を使用して、Experience Platformオーディエンスを LiveRamp に書き出すことができます。

オーディエンスを LiveRamp にオンボーディングした後、 [宛先に接続](#connect) データのアクティベーション用にターゲットの宛先プラットフォームを選択し、設定する手順です。

## 宛先への接続 {#connect}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_identifier_settings"
>title="識別子設定"
>abstract="宛先でサポートされている識別子を選択します。各宛先でサポートされる識別子の完全なリストについては、ドキュメントを参照してください。"

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### LiveRamp に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

![宛先接続画面を示す Platform UI 画像。l](../../assets/catalog/advertising/liveramp-distribution/liveramp-distribution-new-connection.png)

* **[!UICONTROL LiveRamp 組織 ID]**:LiveRamp アカウントの組織 ID( _owner_org_ （LiveRamp が指定する資格情報）。
* **[!UICONTROL パスワード]**:LiveRamp アカウントのパスワード ( _secret_key_ （LiveRamp が指定する資格情報）。
* **[!UICONTROL トークン URL]**:LiveRamp トークンの URL。
* **[!UICONTROL ユーザー名]**:LiveRamp アカウントのユーザー名 ( _account_id_ （LiveRamp が指定する資格情報）。

### 宛先の詳細を設定 {#destination-details}

LiveRamp アカウントに正常に接続したら、オーディエンスをアクティブ化する宛先に接続するために必要な情報を入力します。

![宛先の詳細画面を示す Platform UI 画像。l](../../assets/catalog/advertising/liveramp-distribution/liveramp-distribution-destination-details.png)

* **[!UICONTROL 名前]**：宛先接続の名前を入力します。

>[!NOTE]
>
>宛先に名前を付ける場合、Adobeでは次の形式に従うことをお勧めします。 `LiveRamp - Downstream Destination Name`. この命名パターンにより、 [参照](../../ui/destinations-workspace.md#browse) 「宛先」ワークスペースの「 」タブ
><br>
>例：`LiveRamp - Roku`。

* **[!UICONTROL 説明]**：宛先についての説明を入力します。この宛先の目的を簡単に識別できる説明を使用します。
* **[!UICONTROL 宛先]**：ドロップダウンメニューを使用して、オーディエンスをアクティブ化する宛先を選択します。 ここで選択する宛先は、 [宛先固有の設定](#destination-settings) 画面。
* **[!UICONTROL 統合]**：宛先に使用する統合アカウントを選択します。
* **[!UICONTROL 識別子]**：宛先でサポートされている識別子を選択します。 現在、すべての宛先には、ドロップダウンメニューで事前に入力された、サポートされている識別子があります。

## 宛先固有の設定 {#destination-settings}

各宛先 [サポート](#supported-destinations) 作成者 [!DNL LiveRamp - Distribution] では、特定の設定オプションを入力する必要があります。

各宛先の設定方法に関する詳細なガイダンスについては、以下の節を参照してください。

### [!DNL 4C Insights] {#insights}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_4cinsights_profile_id"
>title="4C ブランドプロファイル ID"
>abstract="4C ブランドプロファイルに関連付けられている数値 ID を入力します。 この ID がない場合は、4C クライアントサービスの担当者にお問い合わせください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![4C インサイトの宛先の顧客データフィールドを示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_4C_DestSpecific.png)

* **[!UICONTROL 4C ブランドプロファイル ID]**:4C ブランドプロファイルに関連付けられている数値 ID を入力します。 この ID がない場合は、4C クライアントサービスの担当者にお問い合わせください。

### [!DNL Acast] {#acast}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_acast_client"
>title="クライアント名"
>abstract="宛先パートナーに表示する広告主のアカウント名。会社名を使用します。スペースや特殊文字は使用しないでください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![Acast の宛先の顧客データフィールドを示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_Acast_DestSpecific.png)

* **[!UICONTROL クライアント名]**：宛先パートナーに表示する広告主アカウント名。 会社名を使用します。スペースや特殊文字は使用しないでください。

### [!DNL Ampersand.tv] {#ampersand-tv}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_ampersand_company_name"
>title="会社名"
>abstract="宛先パートナーに表示する会社名。 スペースや特殊文字は使用しないでください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![アンパサンドの宛先の顧客データフィールドを示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_Ampersand_DestSpecific.png)

* **[!UICONTROL 会社名]**：宛先パートナーに表示する会社名。 スペースや特殊文字は使用しないでください。

### [!DNL Captify] {#captify}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_captify_client"
>title="クライアント名"
>abstract="宛先パートナーに表示する広告主のアカウント名。会社名を使用します。スペースや特殊文字は使用しないでください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![Captify の宛先の顧客データフィールドを示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_Captify_DestSpecific.png)

* **[!UICONTROL クライアント名]**：宛先パートナーに表示する広告主アカウント名。 会社名を使用します。スペースや特殊文字は使用しないでください。

### [!DNL Cardlytics] {#cardlytics}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_cardlytics_client"
>title="クライアント名"
>abstract="宛先パートナーに表示する広告主のアカウント名。会社名を使用します。スペースや特殊文字は使用しないでください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![Cardlytics の宛先の顧客データフィールドを示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_Cardlytics_DestSpecific.png)

* **[!UICONTROL クライアント名]**：宛先パートナーに表示する広告主アカウント名。 会社名を使用します。スペースや特殊文字は使用しないでください。

### [!DNL Disney (Hulu/ESPN/ABC)] {#disney}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_agreement"
>title="広告主データの宛先に関する利用規約"
>abstract="入力 `I AGREE` をクリックして、Disney 広告主のデータ利用条件に対する確認および同意を確認します。"

<!-- >additional-url="<https://www.disneyadvertising.com/ADVERTISER-DATA-DESTINATION-TERMS/>" text="Read the agreement" -->

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_disney_client"
>title="クライアント名"
>abstract="宛先パートナーに表示する広告主のアカウント名。会社名を使用します。スペースや特殊文字は使用しないでください。"

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_disney_email"
>title="メールアドレス"
>abstract="個人に関連付けられたメールアドレスを入力します。このメールアドレスは、広告主データの利用規約への署名として機能します。このメールアドレスは、必要に応じて連絡にも使用されます。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![Disney の宛先の顧客データフィールドを示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/liveramp-distribution-disney-fields.png)

* **[!UICONTROL 広告主データの宛先に関する利用条件の契約]**：に入力します。 `I AGREE` をクリックして、Disney 広告主のデータ利用条件に対する確認および同意を確認します。
* **[!UICONTROL クライアント名]**：宛先パートナーに表示する会社名を入力します。
* **[!UICONTROL 電子メールアドレス]**：個人に関連付けられた電子メールアドレスを入力します。 このメールアドレスは、広告主データの利用規約への署名として機能します。

### [!DNL iHeartMedia] {#iheartmedia}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_iheartmedia_client"
>title="クライアント名"
>abstract="宛先パートナーに表示する広告主のアカウント名。会社名を使用します。スペースや特殊文字は使用しないでください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![iHeartMedia の宛先の顧客データフィールドを示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_iHeart_DestSpecific.png)

* **[!UICONTROL クライアント名]**：宛先パートナーに表示する広告主アカウント名。 会社名を使用します。スペースや特殊文字は使用しないでください。

### [!DNL Index Exchange] {#index-exchange}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_index_advertiseraccountname"
>title="アカウント名"
>abstract="Index Exchange クライアントアカウント名。 スペースや特殊文字は使用しないでください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![Index Exchange の宛先の顧客データフィールドを示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_IndexExchange_DestSpecific.png)

* **[!UICONTROL アカウント名]**:Index Exchange クライアントアカウント名。 スペースや特殊文字は使用しないでください。

### [!DNL Magnite CTV Platform] {#magnite}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_magnitectv_client"
>title="クライアント"
>abstract="宛先パートナーに表示するクライアント名です。 会社名を使用します。スペースや特殊文字は使用しないでください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![Magnite CTV の宛先の顧客データフィールドを示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_MagniteCTV_DestSpecific.png)

* **[!UICONTROL クライアント]**：宛先パートナーに表示するクライアント名。 会社名を使用します。スペースや特殊文字は使用しないでください。

### [!DNL Magnite DV+ (Rubicon Project)] {#magnite-dv}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_magnitedv+_partnerid"
>title="パートナー ID"
>abstract="セグメント/データを所有するパブリッシャーに関連付けられた Rubicon プロジェクトパートナー ID。 使用すべき値が不明な場合は、Rubicon Project アカウント担当者にお問い合わせください。"

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_magnitedv+_seatid"
>title="シート ID"
>abstract="Magnite アカウントマネージャーから提供される Magnite DV+ Seat ID"

宛先の詳細を設定するには、以下のフィールドに入力します。

![Magnite DV+宛先の顧客データフィールドを示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_MagniteDV_DestSpecific.png)

* **[!UICONTROL パートナー ID]**：セグメント/データを所有する発行者に関連付けられた Rubicon プロジェクトパートナー ID。 使用すべき値が不明な場合は、Rubicon Project アカウント担当者にお問い合わせください。
* **[!UICONTROL シート ID]**:Magnite アカウントマネージャーから提供される Magnite DV+ Seat ID

### [!DNL Nexxen (formerly known as [!DNL Amobee])] {#nexxen}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_nexxen_ratetype"
>title="レートタイプ"
>abstract="レートタイプは、データ使用量の請求方法を表します。 0.00 ドルの料金はすべて定額料金にする必要があります。 使用するレートの種類が不明な場合は、Nexxen の担当者に確認してください。"

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_nexxen_marketid"
>title="市場 ID"
>abstract="Nexxen データ契約の作成先となる数値の市場 ID を入力します。 Nexxen プラットフォームのすべての市場で「AlwaysOn」シンジケーションを実行する場合は、-1 と入力します。"

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_nexxen_advertiserid"
>title="広告主 ID"
>abstract="Nexxen プラットフォームの単一の広告主にデータを送信する場合は、Amobe Advertiser ID という数値を入力します。 データを市場のすべての広告主が利用できるようにする場合、またはこれらのセグメントが「常にオン」の場合は、-1 と入力してください。"

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_nexxen_contactemail"
>title="連絡先メール"
>abstract="Nexxen がデータ契約の詳細を送信する際に使用する電子メールアドレスを入力します。 おそらくこれは自分のメールアドレスですが、メールのエイリアスでもかまいません。 複数の受信者の場合は、コンマ (`email1@domain.com`,`email2@domain.com`など )。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![Nexxen の宛先の顧客データフィールドを示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_Nexxen_DestSpecific.png)

* **[!UICONTROL レートタイプ]**：レートタイプは、データ使用量の請求方法を表します。 0.00 ドルの料金はすべて定額料金にする必要があります。 使用するレートの種類が不明な場合は、Nexxen の担当者に確認してください。
* **[!UICONTROL 市場 ID]**:Nexxen データ契約の作成先となる数値の市場 ID を入力します。 Nexxen プラットフォームのすべての市場で「AlwaysOn」シンジケーションを実行する場合は、-1 と入力します。
* **[!UICONTROL 広告主 ID]**:Nexxen プラットフォームの単一の広告主にデータを送信する場合は、Nexxen Advertiser ID という数値を入力します。 データを市場のすべての広告主が利用できるようにする場合、またはこれらのセグメントが「AlwaysOn」の場合は、-1 と入力します。
* **[!UICONTROL 連絡先メール]**:Nexxen がデータ契約の詳細を送信する際に使用する電子メールアドレスを入力します。 おそらくこれは自分のメールアドレスですが、メールのエイリアスでもかまいません。 複数の受信者の場合は、コンマ ( `email1@domain.com`, `email2@domain.com`) をクリックします。

### [!DNL One Fox] {#fox}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_fox_client"
>title="クライアント"
>abstract="パートナーに表示する会社/配信アカウントの名前。 使用する名前が不明な場合は、パートナーアカウント担当者にお問い合わせください。 スペースや特殊文字は使用しないでください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![One Fox 宛先の顧客データフィールドを示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_Fox_DestSpecific.png)

* **[!UICONTROL クライアント]**：パートナーに表示する会社/配信アカウントの名前。 デフォルトで会社名を使用します。 使用する名前が不明な場合は、パートナーアカウント担当者にお問い合わせください。 スペースや特殊文字は使用しないでください。

### [!DNL Pandora] {#pandora}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_pandora_account_name"
>title="アカウント名"
>abstract="お客様の Pandora アカウントの名前。アカウント名が不明な場合は、Pandora のアカウント担当者にお問い合わせください。スペースや特殊文字は使用しないでください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![Pandora の宛先の顧客データフィールドを示すプラットフォーム UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_Pandora_DestSpecific.png)

* **[!UICONTROL アカウント名]**:Pandora アカウントの名前。 アカウント名が不明な場合は、Pandora のアカウント担当者にお問い合わせください。スペースや特殊文字は使用しないでください。

### [!DNL Reddit] {#reddit}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_reddit_advertiser_id"
>title="Reddit の広告主 ID"
>abstract="Reddit の広告主 ID。「t2_」または「a2_」で始まる必要があります。広告主 ID が不明な場合は、Reddit の担当者にお問い合わせください。"

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_reddit_advertiser_name"
>title="Reddit の広告主名"
>abstract="Reddit の広告主名。スペースや特殊文字は使用しないでください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![Reddit の宛先の顧客データフィールドを示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_Reddit_DestSpecific.png)

* **[!UICONTROL 広告主 ID をレディット]**:Reddit 広告主 ID。 「t2_」または「a2_」で始まる必要があります。広告主 ID が不明な場合は、Reddit の担当者にお問い合わせください。
* **[!UICONTROL 広告主名をレディット]**:Reddit 広告主の名前。 スペースや特殊文字は使用しないでください。

### [!DNL Roku] {#roku}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_roku_email"
>title="Roku アカウントのメールアドレス"
>abstract="Roku アカウントに関連付けられたメールアドレスを入力します。"

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_roku_representative_email"
>title="Roku アカウント担当者のメールアドレス"
>abstract="Roku アカウント担当者のメールアドレスを入力します。このアドレスは、分類の更新を送信するために使用されます。複数のアドレスを入力する場合は、コンマで区切ります。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![Roku の宛先でサポートされている識別子を示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/liveramp-distribution-roku-fields.png)

* **[!UICONTROL Roku アカウントの電子メールアドレス]**:Roku アカウントに関連付けられている電子メールアドレスを入力します。
* **[!UICONTROL Roku アカウント担当者の電子メールアドレス]**:Roku アカウント担当者の電子メールアドレスを入力します。 複数のアドレスを入力する場合は、コンマで区切ります。

### [!DNL Spotify] {#spotify}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_spotify_client"
>title="クライアント名"
>abstract="宛先パートナーに表示する広告主のアカウント名。会社名を使用します。スペースや特殊文字は使用しないでください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![Spotify の宛先でサポートされている識別子を示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_Spotify_DestSpecific.png)

* **[!UICONTROL クライアント名]**：宛先パートナーに表示する広告主アカウント名。 会社名を使用します。スペースや特殊文字は使用しないでください。

### [!DNL Taboola] {#taboola}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_taboola_rep_email"
>title="アカウントマネージャーのメールアドレス"
>abstract="Taboola アカウントマネージャーのメールアドレス。"

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_taboola_seg_type"
>title="セグメントタイプ"
>abstract="セグメントタイプ。 現在、ファーストパーティセグメントのみがサポートされています。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![Taboola の宛先でサポートされている ID を示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_Taboola_DestSpecific.png)

* **[!UICONTROL アカウントマネージャーの電子メールアドレス]**:Taboola アカウントマネージャーの電子メールアドレス。
* **[!UICONTROL セグメントタイプ]**：セグメントタイプ。 現在、ファーストパーティセグメントのみがサポートされています。

### [!DNL TargetSpot] {#targetspot}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_targetspot_client"
>title="クライアント名"
>abstract="宛先パートナーに表示する広告主のアカウント名。会社名を使用します。スペースや特殊文字は使用しないでください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![TargetSpot の宛先でサポートされている識別子を示す、Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_TargetSpot_DestSpecific.png)

* **[!UICONTROL クライアント名]**：宛先パートナーに表示する広告主アカウント名。 会社名を使用します。スペースや特殊文字は使用しないでください。

### [!DNL Teads] {#teads}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_teads_teadsid"
>title="ティード ID"
>abstract="ティード ID"

宛先の詳細を設定するには、以下のフィールドに入力します。

![TargetSpot の宛先でサポートされている識別子を示す、Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_Teads_DestSpecific.png)

* **[!UICONTROL ティード ID]**：ティード ID

### [!DNL WB Discovery] {#wb-discovery}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_distribution_wb_client"
>title="クライアント名"
>abstract="宛先パートナーに表示する広告主のアカウント名。会社名を使用します。スペースや特殊文字は使用しないでください。"

宛先の詳細を設定するには、以下のフィールドに入力します。

![WB Discovery 宛先でサポートされている ID を示す Platform UI 画像。](../../assets/catalog/advertising/liveramp-distribution/LR_WBD_DestSpecific.png)

* **[!UICONTROL クライアント名]**：宛先パートナーに表示する広告主アカウント名。 会社名を使用します。スペースや特殊文字は使用しないでください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。データフローのステータスに関する通知を受け取るには、リストからアラートを選択します。 アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)に関するガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

The [!DNL LiveRamp - Distribution] 接続は、 [LiveRamp - Onboarding](liveramp-onboarding.md) 接続。

オーディエンスを正常にアクティブ化するには、 **同じオーディエンス** あなたが持っている [以前オンボード済み](liveramp-onboarding.md) を LiveRamp に追加します。

>[!IMPORTANT]
>
>以前に [LiveRamp - Onboarding](liveramp-onboarding.md) 接続は、新しいオーディエンスのオンボーディングをトリガーしません。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

オーディエンスのアクティベーションを確認および監視するには、LiveRamp アカウントにログインし、アクティベーション指標を確認します。

オーディエンスのアクティベーションに関するご質問は、LiveRamp のアカウント担当者にお問い合わせください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

の設定方法の詳細については、 [!DNL LiveRamp - Onboarding] 宛先 (「 [LiveRamp — オンボーディングドキュメント](liveramp-onboarding.md).
