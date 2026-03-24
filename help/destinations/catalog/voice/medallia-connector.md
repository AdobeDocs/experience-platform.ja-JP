---
title: Medallia 接続
description: ターゲットを絞った Medalia の調査とフィードバックの収集のプロファイルを活用して、顧客のニーズと期待をより深く理解します。
exl-id: 2c2766eb-7be1-418c-bf17-d119d244de92
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '1256'
ht-degree: 27%

---

# Medallia 接続

## 概要 {#overview}

ターゲットを絞った Medalia の調査とフィードバックの収集のプロファイルを活用して、顧客のニーズと期待をより深く理解します。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、Medallia チームによって作成および管理されます。 お問い合わせやアップデートのご依頼は、adobe-integrations@medallia.comまで直接お問い合わせください。

## ユースケース {#use-cases}

Medalliaの宛先を使用する方法とタイミングをより深く理解するために、この宛先を使用して[!DNL Adobe Experience Platform]のお客様が解決できるユースケースの例を次に示します。

### ユースケース #1 {#use-case-1}

B2B企業は、オンボーディングプログラムを評価し、合理化したいと考えています。 オンボーディングプロセスを終えた顧客に対して、パーソナライズされたアンケートをリアルタイムで実施したいと考えています。

### ユースケース #2 {#use-case-2}

A retailerは、注文フルフィルメントにおける顧客の嗜好をより深く理解したいと考えています。 この1か月間にオンラインや実店舗で購入した顧客に対して、1質問のSMS アンケートを短く送信したいと考えています。

## 前提条件 {#prerequisites}

Medallia接続を確立するには、次の情報が必要です。

* **OAuth トークン エンドポイント URL**
* **クライアント ID**
* **クライアント秘密鍵**
* **API ゲートウェイ URL**
* **API名のインポート**

Medalliaのデリバリーチームと協力して、これらの詳細を入手してください。

## サポートされている ID {#supported-identities}

Medalliaは、以下の表に記載されているIDのアクティベーションをサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | 電子メールアドレス | メール招待調査を送信する場合は、メールターゲット IDを選択します。 プロファイルが複数のメールアドレスに関連付けられている場合、Medalliaは最初のメールにのみ招待状をトリガーします。 |
| 電話 | E.164形式でハッシュ化された電話番号 | SMS ベースのアンケートを送信する場合は、電話のターゲット IDを選択します。 電話番号はE.164形式にする必要があります。これには、プラス記号（+）、国際電話コード、市外局番、電話番号が含まれます。 例：（+）（国コード）（市外局番）（電話番号）。 プロファイルが複数の電話番号に関連付けられている場合、Medalliaは最初の電話番号にのみ招待状をトリガーします。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | × | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス ](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス ](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [ データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | [宛先アクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)の「プロファイル属性を選択」画面で選択した目的のスキーマフィールド（電子メールアドレス、電話番号、姓など）と共に、セグメントのすべての新しく適格なメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証を行うには、必須フィールドに入力し、**[!UICONTROL Connect to destination]**&#x200B;を選択します。

* **[!UICONTROL OAuth Token Endpoint URL]**：通常はhttps://instance.medallia.tld/oauth/tenant/tokenの形式になります。
* **[!UICONTROL Client ID]**: Medallia デリバリーチームから取得します。
* **[!UICONTROL Client Secret]**: Medallia デリバリーチームから取得します。

![この宛先の認証画面を示す画像。](/help/destinations/assets/catalog/voice/medallia-destination-oauth.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。
* **[!UICONTROL API Gateway URL]**: Medallia デリバリーチームから取得します。 通常はhttps://instance-tenant.apis.medallia.comの形式になります。
* **[!UICONTROL Import API Name]**: Medallia デリバリーチームから取得します。 この接続で使用するMedallia Import API （Web Feedとも呼ばれます）の名前。 様々なオーディエンスをアクティベートして様々なインポート APIを使用し、様々な調査プログラムをトリガーできます。

![この宛先の宛先の詳細画面を示す画像。](/help/destinations/assets/catalog/voice/medallia-destination-details.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![ ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

ユースケースに応じて、次のターゲット ID名前空間をマッピングする必要があります。

* メールベースの調査の場合、**電子メール**&#x200B;は、**ターゲットフィールド** > **ID名前空間を選択** > **電子メール**&#x200B;を使用して、ターゲットフィールドとしてマッピングする必要があります
* SMS ベースの調査の場合、**phone**&#x200B;は、**ターゲットフィールド** > **ID名前空間を選択** > **phone**&#x200B;を使用して、ターゲットフィールドとしてマッピングする必要があります。 電話番号はE.164形式にする必要があります。これには、プラス記号（+）、国際電話コード、市外局番、電話番号が含まれます

パーソナライズされたアンケートを作成し、顧客に関する追加情報をアンケートレコードに追加するために、追加のターゲットカスタム属性をマッピングすることも強くお勧めします。

* パーソナライズされたアンケートでは、通常、顧客の名前で回答します

   * 顧客の名を&#x200B;**ターゲットフィールド** > **カスタム属性を選択** > **属性名** > **firstname**&#x200B;にマッピングします
   * 顧客の姓を&#x200B;**ターゲットフィールド** > **カスタム属性を選択** > **属性名** > **lastname**&#x200B;にマッピングします

* 必要に応じて、他のターゲットカスタム属性のマッピングを追加します

![IDと属性のサンプルマッピングを示す画像。](/help/destinations/assets/catalog/voice/medallia-destination-mapping.png)

>[!IMPORTANT]
>
> **ターゲットフィールド** > **カスタム属性を選択** > **属性名**&#x200B;を使用して、マッピングしたターゲットカスタム属性ごとに正確な&#x200B;**属性名**&#x200B;をMedallia配信チームと共有します。 マッピングページのスクリーンショットを撮って、直接共有することもできます。

## 書き出したデータ {#exported-data}

宛先に対してセグメントをアクティブ化したら、Medallia配信チームに通知します。チームは、[!DNL Adobe Experience Platform]からMedalliaへの書き出されたデータを検証できます。 調査は、データ検証が成功した後にのみMedallia内でアクティブ化できます。この前に、データはMedalliaに書き出されますが、お客様に調査をトリガーすることはありません。

書き出されたデータのサンプル JSONを以下に示します。これは、**Map属性とID** セクションの上記のスクリーンショットのマッピング例を使用しています。

```json
[
    {
        "profile_raw_encoded": "eyJhdHRyaWJ1dGVzIjp7ImZpcnN",
        "email": "johnsmith@example.com",
        "aep_segments_new": ["c1c3edcc-07cb-4f66-b5dd-aff485148aba"],
        "aep_segments_existing": [],
        "aep_segments_removed": [],
        "firstname":  "John" ,
        "lastname":  "Smith",
        "contactId": "jsmith120002",
    }
]
```

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
