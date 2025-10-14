---
title: TikTok 接続
description: 広告キャンペーンでターゲティングするためのデータを使用して、TikTokでカスタムオーディエンスを作成します。 これらのオーディエンスは、web サイトを訪問したユーザーや、コンテンツを操作したユーザーである可能性があります。 TikTok Ads Manager とAdobeのリアルタイム統合を使用して、目的のオーディエンスをAdobe Experience PlatformからTikTokにすばやく安全にプッシュします。
last-substantial-update: 2023-03-20T00:00:00Z
exl-id: 7b12d17f-7d9a-4615-9830-92bffe3f6927
source-git-commit: c1f54e02bbc4affb775b3dc9e95f3852dc5a8e39
workflow-type: tm+mt
source-wordcount: '1144'
ht-degree: 40%

---

# TikTok 接続

## 概要 {#overview}

広告キャンペーンでターゲティングするためのデータを使用して、TikTokでカスタムオーディエンスを作成します。 これらのオーディエンスは、web サイトを訪問したユーザーや、コンテンツを操作したユーザーである可能性があります。 TikTok Ads Manager とAdobeのリアルタイム統合を使用して、目的のオーディエンスをAdobe Experience PlatformからTikTokにすばやく安全にプッシュします。 詳しくは、[TikTokのビジネスヘルプセンター &#x200B;](https://ads.tiktok.com/help/article/audiences) を参照してください。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、TikTok チームが作成および管理します。 お問い合わせや更新のリクエストについては、[https://ads.tiktok.com/help/](https://ads.tiktok.com/help/) まで直接ご連絡ください。

## ユースケース {#use-cases}

TikTokの宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様向けのサンプルユースケースを以下に示します。

### ユースケース {#use-case-1}

あるアスレチックアパレルブランドは、ソーシャルメディアアカウントを通じて既存の顧客にリーチしたいと考えています。 アパレルブランドは、独自の CRM からAdobe Experience Platformにメールアドレスを取り込み、独自のオフラインデータからオーディエンスを作成し、これらのオーディエンスをTikTokに送信して、顧客のソーシャルメディアフィードに広告を表示できます。

## 前提条件 {#prerequisites}

オーディエンスの送信先のTikTok Ads Manager アカウントに対するアクセス権が [!DNL Admin] または [!DNL Operator] である必要があります。 詳細については、[TikTok ヘルプセンター &#x200B;](https://ads.tiktok.com/help/article/add-users-tiktok-business-center) を参照してください。

TikTok Ads Manager アカウントにデータを送信する前に、`Audience Management` の広告アカウントにアクセスするための権限をAdobe Experience Platformに付与する必要があります。 この権限を付与するには、Experience Platform UI で [Ads Manager ID を入力 &#x200B;](#authenticate) し、TikTok Ads Manager アカウントにリダイレクトされた後に権限を付与します。

## サポートされている ID {#supported-identities}

TikTokでは、以下の表に示す ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 Adobe Experience Platformでは、プレーンテキストと SHA256 でハッシュ化された GAID 値の両方がサポートされています。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Experience Platform] がデータを自動的にハッシュ化するように設定します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 Adobe Experience Platformでは、プレーンテキストと SHA256 でハッシュ化された IDFA 値の両方がサポートされています。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Experience Platform] がデータを自動的にハッシュ化するように設定します。 |
| 電話番号 | SHA256 アルゴリズムでハッシュ化された電話番号 | プレーンテキストと SHA256 でハッシュ化された電話番号の両方がAdobe Experience Platformでサポートされており、E.164 形式である必要があります。 ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Experience Platform] がデータを自動的にハッシュ化するように設定します。 |
| メール | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Experience Platform] がデータを自動的にハッシュ化するように設定します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |
| [!DNL Federated Audience Composition] | ✓ | [Federated Audience Composition](https://experienceleague.adobe.com/ja/docs/federated-audience-composition/using/start/audiences) を通じてExperience Platformにインポートされたオーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | TikTokの宛先で使用される識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先への認証を行うと、[!DNL TikTok Ads Manager] アカウントへのログインにリダイレクトされ、お客様に代わってオーディエンスを管理するAdobeを認証します。

![TikTok権限の選択 &#x200B;](/help/destinations/assets/catalog/social/tiktok/tiktok-authenticate-destination.png " 権限を選択するためのTikTok UI の画像 ")

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![&#x200B; 宛先接続の詳細 &#x200B;](/help/destinations/assets/catalog/social/tiktok/tiktok-configure-destination-details.png " 入力する宛先接続の詳細を示す、Experience Platform UI の画像 ")

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL TikTok広告マネージャー ID]**:[!DNL TikTok Ads Manager ID]。 これは [!DNL TikTok Ads manager] アカウントで確認できます。

![TikTok Ads Manager ID](/help/destinations/assets/catalog/social/tiktok/tiktok-ads-manager-ID.png "TikTok Ads Manager ID の取得方法を示すTikTok Ads Manager UI の画像 ")

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### ID のマッピング {#map}

オーディエンスをTikTok Ads Manager に書き出す際の正しい ID マッピングの例を以下に示します。

ソースフィールドを選択中：

* Adobe Experience Platformおよび ` Email_LC_SHA256` でプロファイルを一意に識別するソース ID として、識別子（例：[!DNL TikTok Ads Manager]）を選択します。

ターゲットフィールドを選択：

* メール名前空間をターゲット ID として選択します。

![ID マッピング &#x200B;](/help/destinations/assets/catalog/social/tiktok/tiktok-map-identity.png "Experience Platform UI の画像、ID のマッピング ")

## 書き出したデータ {#exported-data}

（[!DNL TikTok Ads Manager]Assets / Audiences **の下にある）** アカウントを確認して、Experience Platform オーディエンスが正常に書き出されたかどうかを確認します。 オーディエンスは、オーディエンスタイプ `Partner Audience` として入力されます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

詳しくは、[TikTok ヘルプセンター &#x200B;](https://ads.tiktok.com/help/article/audiences) ページを参照してください。
