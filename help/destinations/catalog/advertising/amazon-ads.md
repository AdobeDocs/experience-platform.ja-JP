---
title: Amazon Ads
description: Amazon Ads には、登録販売者、ベンダー、書籍ベンダー、Kindle ダイレクトパブリッシング（KDP）の著者、アプリ開発者、代理店への広告掲載の目標を達成するのに役立つ様々なオプションが用意されています。Amazon Ads と Adobe Experience Platform の統合により、Amazon DSP（ADSP）などの Amazon Ads 製品へのターンキー統合が可能になります。Adobe Experience Platform で Amazon Ads 宛先を使用すると、ターゲティングとアクティブ化のための広告主オーディエンスを Amazon DSP で定義できます。
last-substantial-update: 2024-02-20T00:00:00Z
exl-id: 724f3d32-65e0-4612-a882-33333e07c5af
source-git-commit: 24f7463f7005f77f8d93e7cb2c04efc0fb4e3a0b
workflow-type: tm+mt
source-wordcount: '1646'
ht-degree: 55%

---

# （ベータ版）Amazon Ads 接続 {#amazon-ads}

## 概要 {#overview}

[!DNL Amazon Ads] には、登録販売者、ベンダー、書籍ベンダー、Kindle Direct Publishing(KDP) 作成者、アプリ開発者、代理店に対して広告目標を達成するのに役立つ様々なオプションが用意されています。

The [!DNL Amazon Ads] Adobe Experience Platformとの統合により、 [!DNL Amazon Ads] Amazon DSP(ADSP) およびAmazon Marketing Cloud(AMC) を含む製品。

の使用 [!DNL Amazon Ads] Adobe Experience Platformの宛先に設定すると、ユーザーは、Amazon DSPでのターゲティングとアクティブ化のための広告主オーディエンスを定義できます。  また、ユーザーは、 [!DNL Amazon Marketing Cloud] オーディエンス、広告主が提供したディメンション、Amazonセグメントのメンバーシップ、または AMC で使用可能なその他のシグナルによるパフォーマンスを把握します。 広告主オーディエンスを AMC にアップロードした後、ユーザーは [!DNL Amazon Marketing Cloud] 内からAmazonシグナルを使用して、オーディエンスメンバーを変更、拡張または追加する [!DNL Amazon Marketing Cloud].

AMC は、ディスプレイ、ビデオ、ストリーミング TV、オーディオ、スポンサー付き広告を含む、Amazonの所有および運用プロパティ全体からの独自のシグナルを統合します。 厳選されたセグメントをAdobe Experience Platformから AMC に簡単に送信して、オーディエンスの市場内グループ、ライフスタイルコホート、ブランドエンゲージメントパターンなどの学習を強化できます。 拡張セグメントは、その後、Amazon DSPでメディアのアクティベーションを最適化するために使用できます。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、 *[!DNL Amazon Ads]* チーム。 現在はベータ版の製品であり、機能は変更される可能性があります。お問い合わせや更新のリクエストについては、*`amc-support@amazon.com`まで直接ご連絡ください。*

## ユースケース {#use-cases}

をいつどのように使用するかをより深く理解するのに役立ちます。 *[!DNL Amazon Ads]* の宛先について、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### アクティブ化とターゲティング {#activation-and-targeting}

このAmazon DSPとの統合により、 [!DNL Amazon Ads] 広告主を使用して、広告主 CDP オーディエンスをAdobe Experience PlatformからAmazonのDSPに渡し、広告ターゲティング用の広告主オーディエンスを作成します。 オーディエンスは、Amazon DSP 内でポジティブターゲティングとネガティブターゲティング（抑制）のために選択できます。

### 分析と測定 {#analytics-and-measurement}

このとの統合 [!DNL Amazon Marketing Cloud] (AMC) により、 [!DNL Amazon Ads] 広告主が CDP セグメントをAdobe Experience Platformフォームから AMC に渡す場合。 その後、広告主は、 [!DNL Amazon Ads] メディアの影響、オーディエンスセグメント、カスタマージャーニーなどのトピックに関するシグナルを送信し、カスタム分析を実行します。また、プライバシーに準拠した形式でカスタムジャーニーを実行します。 例えば、広告主は既存の顧客のリストをアップロードして、広告キャンペーンの全体的な効果や、製品の詳細ページの表示、買い物かごへの製品の追加、製品の購入など、Amazon上のコンバージョンイベントの集計統計を把握できます。

### 広告の最適化：

このとの統合 [!DNL Amazon Marketing Cloud] (AMC) を使用すると、広告主は独自の顧客リストをアップロードし、 [!DNL Amazon Marketing Cloud] Amazon DSPでターゲティング用のアクティベーション対応オーディエンスを作成する前に、SQL を実行し、重複分析、抑制、追加またはオーディエンスの最適化を繰り返し実行します。

## 前提条件 {#prerequisites}

次の手順で [!DNL Amazon Ads] Adobe Experience Platformとの接続時には、最初にAmazon DSP Advertiser アカウントまたは [!DNL Amazon Marketing Cloud] インスタンス。 これらのインスタンスをプロビジョニングするには、 [!DNL Amazon Ads] web サイト：

* [Amazon DSP - デマンドサイドプラットフォームで広告を始める](https://advertising.amazon.com/solutions/products/amazon-dsp)
* [AmazonMarketing Cloudの概要](https://advertising.amazon.com/solutions/products/amazon-marketing-cloud)

## サポートされている ID {#supported-identities}

The *[!DNL Amazon Ads]* 接続では、以下の表で説明する id のアクティブ化をサポートしています。 ID の詳細は[こちら](/help/identity-service//features/namespaces.md)から。でサポートされている ID の詳細 [!DNL Amazon Ads]、 [Amazon DSP Support Center](https://advertising.amazon.com/dsp/help/ss/en/audiences#GA6BC9BW52YFXBNE).

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | *[!DNL Amazon Ads]* 宛先で使用される識別子（氏名、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

次の場所に移動します。 [!DNL Amazon Ads] 最初に接続する広告主アカウントを選択する接続インターフェイス。 接続すると、選択した広告主アカウントの ID が付与された新しい接続で Adobe Experience Platform にリダイレクトされます。宛先の設定画面で適切な広告主アカウントを選択して続行します。

* **[!UICONTROL ベアラートークン]**：宛先を認証するためのベアラートークンを入力します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Amazon Ads Connection]**：ターゲットの ID を選択します。 [!DNL Amazon Ads] 宛先に使用するアカウント。

>[!NOTE]
>
>宛先の設定を保存した後は、 [!DNL Amazon Ads] 広告主 ID(Amazonアカウントを使用して再認証する場合でも )。 別の [!DNL Amazon Ads] 広告主 ID。新しい宛先接続を作成する必要があります。

* **[!UICONTROL 広告主地域]**：広告主がホストされている適切な地域を選択します。各地域でサポートされているマーケットプレイスについて詳しくは、[Amazon Ads ドキュメント](https://advertising.amazon.com/API/docs/en-us/info/api-overview#api-endpoints)を参照してください。



![新しい宛先の設定](../../assets/catalog/advertising/amazon_ads_image_4.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

The [!DNL Amazon Ads] 接続は、ID 照合のために、ハッシュ化された電子メールアドレスとハッシュ化された電話番号をサポートします。 以下のスクリーンショットは、 [!DNL Amazon Ads] 接続：

![アドビから Amazon Ads へのマッピング](../../assets/catalog/advertising/amazon_ads_image_2.png)

* ハッシュ化されたメールアドレスをマッピングするには、`Email_LC_SHA256` ID 名前空間をソースフィールドとして選択します。
* ハッシュ化された電話番号をマッピングするには、`Phone_SHA256` ID 名前空間をソースフィールドとして選択します。
* ハッシュ化されていないメールアドレスまたは電話番号をマッピングするには、対応する ID 名前空間をソースフィールドとして選択し、「`Apply Transformation`」オプションをオンにして、アクティブ化時に Platform で ID をハッシュ化するように設定します。

特定のターゲットフィールドは、 [!DNL Amazon Ads] コネクタ。  例えば、ビジネス用の電子メールを送信する場合、同じ宛先設定で個人用の電子メールをマッピングすることもできません。

使用できる限りのフィールドをマッピングすることを強くお勧めします。使用可能なソース属性が 1 つしかない場合は、1 つのフィールドだけをマッピングできます。The [!DNL Amazon Ads] 宛先は、マッピングの目的ですべてのマッピングされたフィールドを利用し、より多くのフィールドが提供される場合は、より高い一致率を生成します。 使用できる識別子について詳しくは、[Amazon Ads のハッシュ化されたオーディエンスのヘルプページ](https://advertising.amazon.com/dsp/help/ss/en/audiences#GA6BC9BW52YFXBNE)を参照してください。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

オーディエンスがアップロードされたら、オーディエンスが正しく作成およびアップロードされたことを次の手順に従って検証できます。

**Amazon DSP の場合**

次の場所に移動： **[!UICONTROL 広告主 ID]** > **[!UICONTROL オーディエンス]** > **[!UICONTROL 広告主オーディエンス]**. オーディエンスが正常に作成され、オーディエンスメンバーの最小数を満たしている場合は、`Active` のステータスが表示されます。オーディエンスのサイズとリーチについて詳しくは、Amazon DSP ユーザーインターフェイスの右側にある予測リーチパネルを参照してください。

![Amazon DSP オーディエンス作成の検証](../../assets/catalog/advertising/amazon_ads_image_3.png)

**の場合[!DNL Amazon Marketing Cloud]**

左側のスキーマブラウザーで、以下にオーディエンスを見つけます。 **[!UICONTROL 広告主がアップロードされました]** > **[!UICONTROL aep_audiences]**. 次の句を使用して、AMC SQL エディターでオーディエンスに対してクエリを実行できます。

`select count(user_id) from aep_audiences where audienceId = '1234567'`

![AmazonMarketing Cloudオーディエンス作成の検証](../../assets/catalog/advertising/amazon_ads_image_5.png)


## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

その他のヘルプドキュメントについては、次を参照してください [!DNL Amazon Ads] ヘルプリソース：

* [Amazon DSP ヘルプセンター](https://www.amazon.com/ap/signin?openid.pape.max_auth_age=28800&amp;openid.return_to=https%3A%2F%2Fadvertising.amazon.com%2Fdsp%2Fhelp%2Fss%2Fen%2Faudiences&amp;openid.identity=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&amp;openid.assoc_handle=amzn_bt_desktop_us&amp;openid.mode=checkid_setup&amp;openid.claimed_id=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0%2Fidentifier_select&amp;openid.ns=http%3A%2F%2Fspecs.openid.net%2Fauth%2F2.0)

## 変更ログ {#changelog}

この節では、この宛先コネクタに対する機能の概要と重要なドキュメントの更新について説明します。

+++ 変更ログを表示

| リリース月 | 更新タイプ | 説明 |
|---|---|---|
| 2024年2月 | 機能とドキュメントの更新 | 使用するオーディエンスをエクスポートするオプションが追加されました。 [!DNL Amazon Marketing Cloud] (AMC) を使用します。 |
| 2023年5月 | 機能とドキュメントの更新 | <ul><li>[宛先接続ワークフロー](#destination-details)での広告主地域選択のサポートを追加しました。</li><li>広告主地域の選択の追加を反映するようにドキュメントを更新しました。正しい広告主地域選択について詳しくは、[Amazon ドキュメント](https://advertising.amazon.com/API/docs/en-us/info/api-overview#api-endpoints)を参照してください。</li></ul> |
| 2023年3月 | 初回リリース | 宛先の初回リリースとドキュメントを公開しました。 |

{style="table-layout:auto"}

+++
