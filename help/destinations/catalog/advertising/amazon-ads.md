---
title: Amazon Ads
description: Amazon Ads には、登録販売者、ベンダー、書籍ベンダー、Kindle ダイレクトパブリッシング（KDP）の著者、アプリ開発者、代理店への広告掲載の目標を達成するのに役立つ様々なオプションが用意されています。Amazon Ads と Adobe Experience Platform の統合により、Amazon DSP（ADSP）などの Amazon Ads 製品へのターンキー統合が可能になります。Adobe Experience Platform で Amazon Ads 宛先を使用すると、ターゲティングとアクティブ化のための広告主オーディエンスを Amazon DSP で定義できます。
last-substantial-update: 2023-03-29T00:00:00Z
exl-id: 724f3d32-65e0-4612-a882-33333e07c5af
source-git-commit: d6402f22ff50963b06c849cf31cc25267ba62bb1
workflow-type: tm+mt
source-wordcount: '1399'
ht-degree: 92%

---

# （ベータ版）Amazon Ads 接続 {#amazon-ads}

## 概要 {#overview}

Amazon Ads には、登録販売者、ベンダー、書籍ベンダー、Kindle ダイレクトパブリッシング（KDP）の著者、アプリ開発者、代理店への広告掲載の目標を達成するのに役立つ様々なオプションが用意されています。

Amazon Ads と Adobe Experience Platform の統合により、Amazon DSP（ADSP）などの Amazon Ads 製品へのターンキー統合が可能になります。Adobe Experience Platform で Amazon Ads 宛先を使用すると、ターゲティングとアクティブ化のための広告主オーディエンスを Amazon DSP で定義できます。

>[!IMPORTANT]
>
>このドキュメントページは、*Amazon Ads* チームが作成したものです。現在はベータ版の製品であり、機能は変更される可能性があります。お問い合わせや更新のリクエストについては、*`amc-support@amazon.com`まで直接ご連絡ください。*

## ユースケース {#use-cases}

*Amazon Ads* 宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるサンプルユースケースを以下に示します。

### アクティブ化とターゲティング {#activation-and-targeting}

このAmazon DSPとの統合により、Amazon Ads 広告主は、広告主 CDP オーディエンスをAdobe Experience PlatformからAmazonのDSPに渡して、広告ターゲティング用の広告主オーディエンスを作成できます。 オーディエンスは、Amazon DSP 内でポジティブターゲティングとネガティブターゲティング（抑制）のために選択できます。

## 前提条件 {#prerequisites}

Adobe Experience Platform で Amazon Ads 接続を使用するには、まずユーザーが Amazon DSP 広告主アカウントにアクセスできる必要があります。これらのインスタンスをプロビジョニングするには、Amazon Ads web サイトの次のページにアクセスします。

* [Amazon DSP - デマンドサイドプラットフォームで広告を始める](https://advertising.amazon.com/solutions/products/amazon-dsp?ref_=a20m_us_hnav_p_dsp_adtech)

## サポートされている ID {#supported-identities}

*Amazon Ads* 接続では、以下の表に示す ID のアクティブ化をサポートしています。ID の詳細は[こちら](/help/identity-service/namespaces.md)から。Amazon Ads でサポートされている ID について詳しくは、[Amazon DSP サポートセンター](https://advertising.amazon.com/dsp/help/ss/en/audiences#GA6BC9BW52YFXBNE)を参照してください。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | オーディエンスのすべてのメンバーを、 *Amazon Ads* 宛先。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 [ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

Amazon Ads 接続インターフェイスに移動するので、まず接続先の広告主アカウントを選択します。接続すると、選択した広告主アカウントの ID が付与された新しい接続で Adobe Experience Platform にリダイレクトされます。宛先の設定画面で適切な広告主アカウントを選択して続行します。

* **[!UICONTROL ベアラートークン]**：宛先を認証するためのベアラートークンを入力します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Amazon Ads 広告主 ID]**：宛先に使用するターゲット Amazon Ads アカウントの ID を選択します。

>[!NOTE]
>
>宛先の設定を保存すると、Amazon アカウントを通じて再認証した場合でも、Amazon Ads 広告主 ID を変更できなくなります。別の Amazon Ads 広告主 ID を使用するには、新しい宛先接続を作成する必要があります。

* **[!UICONTROL 広告主地域]**：広告主がホストされている適切な地域を選択します。各地域でサポートされているマーケットプレイスについて詳しくは、[Amazon Ads ドキュメント](https://advertising.amazon.com/API/docs/en-us/info/api-overview#api-endpoints)を参照してください。



![新しい宛先の設定](../../assets/catalog/advertising/amazon_ads_image_4.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対するオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [ストリーミングオーディエンスの書き出し先に対するプロファイルとオーディエンスのアクティブ化](/help/destinations/ui/activate-segment-streaming-destinations.md) を参照してください。

### 属性と ID のマッピング {#map}

Amazon Ads 接続では、ID 照合のために、ハッシュ化されたメールアドレスとハッシュ化された電話番号をサポートしています。以下のスクリーンショットは、Amazon Ads 接続に適合する照合の例を示しています。

![アドビから Amazon Ads へのマッピング](../../assets/catalog/advertising/amazon_ads_image_2.png)

* ハッシュ化されたメールアドレスをマッピングするには、`Email_LC_SHA256` ID 名前空間をソースフィールドとして選択します。
* ハッシュ化された電話番号をマッピングするには、`Phone_SHA256` ID 名前空間をソースフィールドとして選択します。
* ハッシュ化されていないメールアドレスまたは電話番号をマッピングするには、対応する ID 名前空間をソースフィールドとして選択し、「`Apply Transformation`」オプションをオンにして、アクティブ化時に Platform で ID をハッシュ化するように設定します。

使用できる限りのフィールドをマッピングすることを強くお勧めします。使用可能なソース属性が 1 つしかない場合は、1 つのフィールドだけをマッピングできます。Amazon Ads 宛先では、マッピングされたすべてのフィールドをマッピング目的で使用し、より多くのフィールドを指定するほどマッチ率が高くなります。使用できる識別子について詳しくは、[Amazon Ads のハッシュ化されたオーディエンスのヘルプページ](https://advertising.amazon.com/dsp/help/ss/en/audiences#GA6BC9BW52YFXBNE)を参照してください。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

オーディエンスがアップロードされたら、オーディエンスが正しく作成およびアップロードされたことを次の手順に従って検証できます。

**Amazon DSP の場合**

広告主 ID／オーディエンス／広告主オーディエンスに移動します。オーディエンスが正常に作成され、オーディエンスメンバーの最小数を満たしている場合は、`Active` のステータスが表示されます。オーディエンスのサイズとリーチについて詳しくは、Amazon DSP ユーザーインターフェイスの右側にある予測リーチパネルを参照してください。

![Amazon DSP オーディエンス作成の検証](../../assets/catalog/advertising/amazon_ads_image_3.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

その他のヘルプドキュメントについては、次の Amazon Ads ヘルプリソースを参照してください。

* [Amazon DSP ヘルプセンター](https://www.amazon.com/ap/signin?openid.pape.max_auth_age=28800&amp;openid.return_to=https%3A%2F%2Fadvertising.amazon.com%2Fdsp%2Fhelp%2Fss%2Fen%2Faudiences&amp;openid.identity=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&amp;openid.assoc_handle=amzn_bt_desktop_us&amp;openid.mode=checkid_setup&amp;openid.claimed_id=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&amp;openid.ns=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0)

## 変更ログ {#changelog}

この節では、この宛先コネクタに対する機能の概要と重要なドキュメントの更新について説明します。

+++ 変更ログを表示

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2023年5月 | 機能とドキュメントの更新 | <ul><li>[宛先接続ワークフロー](#destination-details)での広告主地域選択のサポートを追加しました。</li><li>広告主地域の選択の追加を反映するようにドキュメントを更新しました。正しい広告主地域選択について詳しくは、[Amazon ドキュメント](https://advertising.amazon.com/API/docs/en-us/info/api-overview#api-endpoints)を参照してください。</li></ul> |
| 2023年3月 | 初回リリース | 宛先の初回リリースとドキュメントを公開しました。 |

{style="table-layout:auto"}

+++
