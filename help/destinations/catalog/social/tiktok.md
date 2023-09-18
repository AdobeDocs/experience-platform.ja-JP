---
title: TikTok 接続
description: お持ちのデータを使用して TikTok でカスタムオーディエンスを作成し、広告キャンペーンのターゲティングを行えます。これらのオーディエンスは、Web サイトを訪問した人や、コンテンツに対して何らかのアクションを起こした人のものです。 AdobeのTikTok Ads Manager とのリアルタイム統合を使用して、目的のオーディエンスをAdobe Experience PlatformからTikTokにすばやく安全にプッシュします。
last-substantial-update: 2023-03-20T00:00:00Z
exl-id: 7b12d17f-7d9a-4615-9830-92bffe3f6927
source-git-commit: 05e996f9e33e0d8be3d15a9ab3baaaf6d8152b5a
workflow-type: tm+mt
source-wordcount: '1046'
ht-degree: 44%

---

# TikTok 接続

## 概要 {#overview}

お持ちのデータを使用して TikTok でカスタムオーディエンスを作成し、広告キャンペーンのターゲティングを行えます。これらのオーディエンスは、Web サイトを訪問した人や、コンテンツに対して何らかのアクションを起こした人のものです。 AdobeのTikTok Ads Manager とのリアルタイム統合を使用して、目的のオーディエンスをAdobe Experience PlatformからTikTokにすばやく安全にプッシュします。 訪問 [TikTok Business Help Center](https://ads.tiktok.com/help/article/audiences?lang=en) を参照してください。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、TikTokチームが作成および管理します。 お問い合わせや更新のご依頼は、直接お問い合わせください。 [https://ads.tiktok.com/help/](https://ads.tiktok.com/help/).

## ユースケース {#use-cases}

TikTokの宛先をいつどのように使用するかを理解しやすくするために、Adobe Experience Platformのお客様向けの使用例を次に示します。

### ユースケース {#use-case-1}

スポーツアパレルブランドは、ソーシャルメディアアカウントを通じて既存の顧客にリーチしたいと考えています。 アパレルブランドは、独自の CRM からAdobe Experience Platformに電子メールアドレスを取り込み、独自のオフラインデータからオーディエンスを構築し、TikTokに送信して、顧客のソーシャルメディアフィードに広告を表示できます。

## 前提条件 {#prerequisites}

次が必要です： [!DNL Admin] または [!DNL Operator] オーディエンスの送信先のTikTok Ads Manager アカウントへのアクセス。 その他の手順については、 [TikTok Help Center](https://ads.tiktok.com/help/article/add-users-tiktok-business-center?lang=en).

データをTikTok Ads Manager アカウントに送信する前に、Adobe Experience Platformに次の広告アカウントへのアクセス権を付与する必要があります： `Audience Management`. この権限は、次の方法で指定できます。 [Ads Manager ID の入力](#authenticate) (Experience PlatformUI) をクリックし、TikTok Ads Manager アカウントにリダイレクトされた後での権限の付与 ) を参照してください。

## サポートされている ID {#supported-identities}

TikTokでは、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | Google Advertising ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| 電話番号 | SHA256 アルゴリズムでハッシュ化された電話番号 | プレーンテキストと SHA256 ハッシュ化された電話番号の両方がAdobe Experience Platformでサポートされており、E.164 形式である必要があります。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| メール | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | TikTokの宛先で使用されている識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先への認証をおこなうには、次の場所にログインするようにリダイレクトされます： [!DNL TikTok Ads Manager] アカウントと認証Adobeが自分に代わってオーディエンスを管理するようにします。

![TikTok権限の選択](/help/destinations/assets/catalog/social/tiktok/tiktok-authenticate-destination.png "権限を選択するためのTikTok UI の画像")

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![宛先接続の詳細](/help/destinations/assets/catalog/social/tiktok/tiktok-configure-destination-details.png "Platform UI の画像。入力する宛先接続の詳細を示します。")

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL TikTok Ads Manager ID]**: [!DNL TikTok Ads Manager ID]. これは、 [!DNL TikTok Ads manager] アカウント。

![TikTok Ads Manager ID](/help/destinations/assets/catalog/social/tiktok/tiktok-ads-manager-ID.png "TikTok Ads Manager UI の画像 (TikTok Ads Manager ID の取得方法を示す )")

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### ID のマッピング {#map}

オーディエンスをTikTok Ads Manager にエクスポートする際の正しい ID マッピングの例を以下に示します。

ソースフィールドを選択しています。

* 識別子を選択します ( 例：` Email_LC_SHA256`) を、Adobe Experience Platformでプロファイルを一意に識別するソース ID として、および [!DNL TikTok Ads Manager].

ターゲットフィールドの選択：

* ターゲット ID として電子メール名前空間を選択します。

![ID マッピング](/help/destinations/assets/catalog/social/tiktok/tiktok-map-identity.png "Platform UI の画像、ID のマッピング")

## 書き出したデータ {#exported-data}

以下を確認します。 [!DNL TikTok Ads Manager] アカウント ( **Assets /オーディエンス**) をクリックして、Experience Platformオーディエンスが正常に書き出されたかどうかを確認します。 オーディエンスは、オーディエンスタイプとして入力されます。 `Partner Audience`.

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

詳しくは、 [TikTok Help Center ページ](https://ads.tiktok.com/help/article/audiences?lang=en) を参照してください。
