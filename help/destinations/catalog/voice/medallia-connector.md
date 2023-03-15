---
title: Medallia 接続
description: ターゲットを絞った Medalia の調査とフィードバックの収集のプロファイルを活用して、顧客のニーズと期待をより深く理解します。
exl-id: 2c2766eb-7be1-418c-bf17-d119d244de92
source-git-commit: dd18350387aa6bdeb61612f0ccf9d8d2223a8a5d
workflow-type: tm+mt
source-wordcount: '1096'
ht-degree: 37%

---

# Medallia 接続

## 概要 {#overview}

ターゲットを絞った Medalia の調査とフィードバックの収集のプロファイルを活用して、顧客のニーズと期待をより深く理解します。

>[!IMPORTANT]
>
>このドキュメントページは Medallia チームが作成したものです。 お問い合わせや更新のご依頼は、adobe-integrations@medallia.comから直接お問い合わせください。

## ユースケース {#use-cases}

Medalia の宛先をいつどのように使用するかを理解しやすくするために、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### 使用例#1

ある B2B ブランドは、オンボーディングプログラムを評価し、合理化したいと考えています。 オンボーディングプロセスを完了したばかりの顧客に、パーソナライズされた調査をリアルタイムで送信したいと考えています。

### 使用例#2

ある小売業者は、注文の受け渡しに関する顧客の好みをより深く理解したいと考えています。 過去 1 か月間にオンラインおよび店頭での購入をおこなった顧客に、短い 1 問の SMS 調査を送信したいと考えています。

## 前提条件 {#prerequisites}

Medalia 接続を確立するには、次の情報が必要です。
* **OAuth トークンエンドポイント URL**
* **クライアント ID**
* **クライアント秘密鍵**
* **API ゲートウェイ URL**
* **インポート API 名**

これらの詳細は、Medalia 配信チームと連携して入手してください。

## サポートされる ID {#supported-identities}

Medalia では、以下の表で説明する ID のアクティベーションをサポートしています。 [ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| 電子メール | メールアドレス | 電子メールの招待状の調査を送信する場合は、電子メールのターゲット ID を選択します。 プロファイルが複数の電子メールアドレスに関連付けられている場合、Medalia は最初の電子メールにのみ招待をトリガーします。 |
| phone | E.164 形式でハッシュ化された電話番号 | SMS ベースの調査を送信する場合は、電話のターゲット ID を選択します。 電話番号は E.164 形式である必要があります。この形式には、プラス記号 (+)、国際電話番号、市外局番、電話番号などが含まれます。 例：(+)（国コード）（市外局番）（電話番号）。 プロファイルが複数の電話番号に関連付けられている場合、Medalia は最初の電話番号への招待のみをトリガーにします。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントの新しく認定されたすべてのメンバーを、目的のスキーマフィールドと共に書き出します ( 例：（電子メールアドレス、電話番号、姓）。「プロファイル属性を選択」画面で選択します。 [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL OAuth トークンエンドポイント URL]**:通常はhttps://instance.medallia.tld/oauth/tenant/tokenの形式を取ります。
* **[!UICONTROL クライアント ID]**:Medalia 配信チームから入手します。
* **[!UICONTROL クライアント秘密鍵]**:Medalia 配信チームから入手します。

![この宛先の認証画面を示す画像。](/help/destinations/assets/catalog/voice/medallia-destination-oauth.png)

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL API ゲートウェイ URL]**:Medalia 配信チームから入手します。 通常はhttps://instance-tenant.apis.medallia.comの形式を取ります。
* **[!UICONTROL インポート API 名]**:Medalia 配信チームから入手します。 この接続で使用する Medalia 読み込み API（Web フィードとも呼ばれます）の名前。 様々なセグメントを様々な読み込み API に対してアクティブ化し、様々な調査プログラムをトリガーにすることができます。

![この宛先の宛先の詳細画面を示す画像。](/help/destinations/assets/catalog/voice/medallia-destination-details.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントをアクティベートする手順は、[ストリーミングセグメントの書き出し宛先へのプロファイルとセグメントのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

使用例に応じて、次のターゲット ID 名前空間をマッピングする必要があります。
* 電子メールベースの調査の場合、 **電子メール** は、 **ターゲットフィールド** > **ID 名前空間を選択** > **電子メール**
* SMS ベースの調査の場合、 **phone** は、 **ターゲットフィールド** > **ID 名前空間を選択** > **phone**. 電話番号は E.164 形式である必要があります。この形式には、プラス記号 (+)、国際電話番号、市外局番、電話番号が含まれます

追加のターゲットカスタム属性をマッピングして、パーソナライズされた調査を作成し、顧客に関する追加情報を調査レコードに追加することを強くお勧めします。

* パーソナライズされた調査は通常、顧客の名前別に処理されます
   * 顧客の名を **ターゲットフィールド** > **カスタム属性を選択** > **属性名** > **名**
   * 顧客の姓を **ターゲットフィールド** > **カスタム属性を選択** > **属性名** > **姓**
* 必要に応じて、他のターゲットカスタム属性のマッピングを追加します。

![ID と属性のサンプルマッピングを示す画像。](/help/destinations/assets/catalog/voice/medallia-destination-mapping.png)

>[!IMPORTANT]
> 
> 正確な情報を Medalia 配信チームと共有します **属性名** を使用してマッピングするすべてのターゲットカスタム属性に対して **ターゲットフィールド** > **カスタム属性を選択** > **属性名**. 直接共有するマッピングページのスクリーンショットを撮ることができます。

## 書き出したデータ {#exported-data}

宛先に対してセグメントをアクティブ化したら、Medalia 配信チームに通知します。このチームは、Adobe Experience Platformから Medalia に書き出されたデータを検証できるようになります。 調査は、データの検証が成功した後に Medalia 内でのみアクティブ化できます。これより前、データは Medalia に書き出されますが、お客様にはトリガー調査は送信されません。

書き出されたデータのサンプル JSON を以下に示します。これは、上記のスクリーンショットのマッピング例を使用しています ( **属性と ID のマッピング** セクション：

```json
[
    {
        "profile_raw_encoded": "eyJhdHRyaWJ1dGVzIjp7ImZpcnN",
        "email": "johnsmith@example.com",
        "aep_segments_new": ["c1c3edcc-07cb-4f66-b5dd-aff485148aba"],
        "aep_segments_existing": [],
        "aep_segments_removed": [],
        "firstname":  “John” ,
        "lastname":  “Smith”,
        "contactId": "jsmith120002",
    }
]
```

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
