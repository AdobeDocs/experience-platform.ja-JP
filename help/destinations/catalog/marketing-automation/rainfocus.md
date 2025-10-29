---
title: RainFocus 参加者プロファイル
description: RainFocus 参加者プロファイル宛先コネクタを使用して、オーディエンスプロファイルを RainFocus グローバル参加者プロファイルと同期する方法を説明します。
last-substantial-update: 2024-12-17T00:00:00Z
exl-id: 27c3848c-411a-4305-a5d5-00b145b95287
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 37%

---

# RainFocus 参加者プロファイル {#rainfocus-destination}

## 概要 {#overview}

[!DNL RainFocus Attendee Profiles] の宛先を使用して、顧客プロファイルをAdobe Experience Platformから [!DNL RainFocus] platform にストリーミングし、出席者プロファイルを作成および更新します。

>[!IMPORTANT]
>
>宛先コネクタとドキュメントページは、[!DNL RainFocus] チームが作成および管理します。 お問い合わせや更新のリクエストについては、`clientcare@rainfocus.com` まで直接ご連絡いただくか、RainFocus [&#x200B; ヘルプセンター &#x200B;](https://help.rainfocus.com/hc/en-us) をご覧ください。

## ユースケース {#use-cases}

RainFocus 宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるサンプルユースケースを以下に示します。

### のユースケース#1 {#use-case-1}

ある大手企業テクノロジー企業は、今後のグローバルエキスポのオープン登録を予定しており、登録プロセスを合理化するために顧客プロファイルを [!DNL RainFocus] にプッシュしたいと考えています。

### のユースケース#2 {#use-case-2}

金融サービスのブランドは、新規顧客と既存顧客をターゲットとした一連のロードショーを開催する予定です。 Adobe Experience Platformには、ターゲットとなるお客様を対象とした一連のオーディエンスセグメントがあります。 [!DNL RainFocus] 宛先コネクタを使用すると、これらのプロファイルを [!DNL RainFocus] に簡単に送信してアクティブ化できます。

## 前提条件 {#prerequisites}

[!DNL RainFocus] の宛先を使用する前に、次の前提条件を満たしていることを確認してください。

* OAuth （グローバル）を使用して [!DNL RainFocus] API プロファイルを作成します。
   * **Attendee ストア** エンドポイントを有効にする必要があります。
   * **クライアント ID** と **クライアントシークレット** を生成する必要があります。

また、プロファイルの送信先となる RainFocus **イベントコード** 識別子も必要です。

## サポートされている ID {#supported-identities}

[!DNL RainFocus] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/overview.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対する認証を行うには、必須フィールドに入力し、「**[!UICONTROL Connect to destination]**」を選択します。

![RainFocus 宛先コネクタの認証の詳細を指定 &#x200B;](/help/destinations/assets/catalog/marketing-automation/rainfocus/rainfocus-destination-authentication.png)

* **[!UICONTROL Client ID]**:RainFocus API プロファイルから提供される [!DNL Client ID] を入力します。
* **[!UICONTROL Client secret]**:RainFocus API プロファイルから提供される [!DNL Client Secret] を入力します。
* **[!UICONTROL Environment]**：接続する RainFocus 環境を指定します（例：`dev`、`prod`）。
* **[!UICONTROL Org ID]**:RainFocus のインスタンスに一意の `orgid` を指定します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![RainFocus 宛先コネクタの接続の詳細を指定する &#x200B;](/help/destinations/assets/catalog/marketing-automation/rainfocus/rainfocus-configure-destination-details.png)

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Event ID]**：プロファイルの送信先となる [!DNL RainFocus] イベントコード識別子。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントをアクティベートする手順は、[ストリーミングセグメントの書き出し宛先へのプロファイルとセグメントのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

以下のターゲット ID 名前空間は、ユースケースに応じてマッピングする必要があります。

* **メール** は、**ターゲットフィールド/ID 名前空間を選択/メール** を使用して、ターゲットフィールドとしてマッピングする必要があります

![&#x200B; プロファイルと ID のフィールドをマッピングする方法 &#x200B;](/help/destinations/assets/catalog/marketing-automation/rainfocus/rainfocus-destination-mapping.png)

追加のプロファイルフィールドをマッピングすることをお勧めします。マッピングすると、[!DNL RainFocus] の参加者プロファイルが完全に入力されます。 [!DNL RainFocus] では、次のターゲットフィールドを使用できます。

| ターゲットフィールド | 説明 |
|------------|-------------|
| `address1` | 番地の最初の行 |
| `address2` | 番地の 2 行目（該当する場合） |
| `city` | 市区町村名 |
| `companyname` | 会社の名前 |
| `countryid` | 国の ISO 3166-1 アルファ–2 国コード識別子 |
| `email` | メールアドレス |
| `firstname` | 人物の名 |
| `lastname` | 人物の姓 |
| `jobtitle` | 人物の役職 |
| `phone` | 電話番号 |
| `state` | 州または都道府県の FIPS 州アルファ コード |
| `zip` | 郵便番号 |

{style="table-layout:auto"}

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

一連のプロファイルが [!DNL RainFocus] に送信されたら、[!DNL RainFocus] で API プロファイルのログを使用して、プロファイルが正常に取り込まれていることを検証します。

![RainFocus での API プロファイルへのログの表示 &#x200B;](/help/destinations/assets/catalog/marketing-automation/rainfocus/rainfocus-destination-api-profile.png)

![&#x200B; プロファイルが正常に取り込まれていることを検証 &#x200B;](/help/destinations/assets/catalog/marketing-automation/rainfocus/rainfocus-destination-api-logging.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

* [RainFocus ストリーミングSourceコネクタ &#x200B;](https://experienceleague.adobe.com/en/docs/experience-platform/sources/connectors/analytics/rainfocus)
