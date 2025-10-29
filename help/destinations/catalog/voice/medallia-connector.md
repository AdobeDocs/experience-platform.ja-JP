---
title: Medallia 接続
description: ターゲットを絞った Medalia の調査とフィードバックの収集のプロファイルを活用して、顧客のニーズと期待をより深く理解します。
exl-id: 2c2766eb-7be1-418c-bf17-d119d244de92
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '1098'
ht-degree: 29%

---

# Medallia 接続

## 概要 {#overview}

ターゲットを絞った Medalia の調査とフィードバックの収集のプロファイルを活用して、顧客のニーズと期待をより深く理解します。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメントページは、Medallia チームが作成および管理します。 お問い合わせや更新のリクエストについては、adobe-integrations@medallia.comまで直接ご連絡ください。

## ユースケース {#use-cases}

Medallia 宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるサンプルユースケースを以下に示します。

### のユースケース#1

B2B ブランドは、オンボーディングプログラムを評価し効率化したいと考えています。 オンボーディングプロセスを完了したばかりのクライアントに、パーソナライズされた調査をリアルタイムで送信したいと考えています。

### のユースケース#2

retailerは、注文の受け渡しに関するお客様の好みをより深く理解したいと考えています。 この 1 か月間にオンラインおよび店舗での購入を行った顧客に、短い 1 質問の SMS 調査を送信したいと考えています。

## 前提条件 {#prerequisites}

Medallia 接続を確立するには、次の情報が必要です。

* **OAuth トークンエンドポイント URL**
* **クライアント ID**
* **クライアント秘密鍵**
* **API ゲートウェイ URL**
* **読み込み API 名**

Medallia 配信チームと協力して、これらの詳細を取得します。

## サポートされている ID {#supported-identities}

Medallia では、以下の表に示す ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| メール | メールアドレス | 電子メール招待アンケートを送信する際に、電子メールターゲット ID を選択します。 プロファイルが複数のメールアドレスに関連付けられている場合、Medallia は最初のメールにのみ招待状をトリガーします。 |
| 電話番号 | E.164 形式でハッシュ化された電話番号 | SMS ベースの調査を送信する場合は、電話ターゲット ID を選択します。 電話番号は E.164 形式である必要があります。これには、プラス記号（+）、国際国番号、市外局番、電話番号が含まれます。 例：（+）（国コード）（市外局番）（電話番号） プロファイルが複数の電話番号に関連付けられている場合、Medallia は最初の電話番号にのみ招待状をトリガーします。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | [ 宛先のアクティベーションワークフロー ](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes) のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントの新しく選定されたすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対する認証を行うには、必須フィールドに入力し、「**[!UICONTROL Connect to destination]**」を選択します。

* **[!UICONTROL OAuth Token Endpoint URL]**：通常、https://instance.medallia.tld/oauth/tenant/tokenの形式で指定します。
* **[!UICONTROL Client ID]**: Medallia 配信チームから入手してください。
* **[!UICONTROL Client Secret]**: Medallia 配信チームから入手してください。

![ この宛先の認証画面を示す画像。](/help/destinations/assets/catalog/voice/medallia-destination-oauth.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL API Gateway URL]**: Medallia 配信チームから入手してください。 通常、https://instance-tenant.apis.medallia.comの形式で指定します。
* **[!UICONTROL Import API Name]**: Medallia 配信チームから入手してください。 この接続で使用される Medallia Import API （Web フィードとも呼ばれます）の名前です。 異なるオーディエンスを異なる読み込み API に対してアクティブ化して、異なる調査プログラムをトリガーにすることができます。

![ この宛先の宛先の詳細画面を示す画像。](/help/destinations/assets/catalog/voice/medallia-destination-details.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

以下のターゲット ID 名前空間は、ユースケースに応じてマッピングする必要があります。

* メールベースの調査の場合は、**ターゲットフィールド**/**ID 名前空間を選択**/**メール** を使用して、**メール** をターゲットフィールドとしてマッピングする必要があります
* SMS ベースの調査の場合は、**ターゲットフィールド**/**ID 名前空間を選択**/**電話** を使用して、**電話** をターゲットフィールドとしてマッピングする必要があります。 電話番号は、E.164 形式である必要があります。E.164 形式には、プラス記号（+）、国際国番号、市外局番、電話番号が含まれます

また、追加のターゲットのカスタム属性をマッピングしてパーソナライズされた調査を作成し、顧客に関する追加情報を調査レコードに追加することを強くお勧めします。

* パーソナライズされた調査は、通常、顧客を名前で対処します

   * 顧客の名を **ターゲットフィールド**/**カスタム属性を選択**/**属性名**/**firstname** にマッピングします
   * 顧客の姓を **ターゲットフィールド**/**カスタム属性を選択**/**属性名**/**lastname** にマッピングします

* 必要に応じて、他のターゲットカスタム属性のマッピングを追加します

![ID と属性のサンプルマッピングを示す画像 ](/help/destinations/assets/catalog/voice/medallia-destination-mapping.png)

>[!IMPORTANT]
> 
> **ターゲットフィールド**/**カスタム属性を選択**/**属性名** を使用してマッピングするすべてのターゲットカスタム属性について、Medallia 配信チームと正確な **属性名** を共有します。 マッピングページのスクリーンショットを撮って、直接共有することもできます。

## 書き出したデータ {#exported-data}

宛先に対してセグメントをアクティブ化したら、Medallia 配信チームに通知します。このチームは、Adobe Experience Platformから Medallia に書き出されたデータを検証できます。 調査は、データ検証が成功した後にのみ Medallia 内でアクティブ化できることに注意してください。これより前に、データは Medallia に書き出されますが、調査をお客様にトリガーすることはありません。

書き出されたデータのサンプル JSON を以下に示します。ここでは、上記の **属性と ID をマッピング** セクションのスクリーンショットのマッピング例を使用しています。

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
