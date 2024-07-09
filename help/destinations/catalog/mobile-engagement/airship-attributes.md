---
keywords: 飛行船属性；飛行船宛先
title: Airship Attributes 接続
description: Airship 内でターゲティングするためのオーディエンス属性として、Adobeのオーディエンスデータを Airship にシームレスに渡します。
exl-id: bfc1b52f-2d68-40d6-9052-c2ee1e877961
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '1039'
ht-degree: 31%

---

# [!DNL Airship Attributes] 接続 {#airship-attributes-destination}

## 概要 {#overview}

[!DNL Airship] は、優れたカスタマーエンゲージメントプラットフォームです。カスタマーライフサイクルのあらゆる段階において、ユーザーに対して有意義でパーソナライズされたオムニチャネルメッセージを提供するのを支援します。

この統合により、Adobeプロファイルデータがに渡されます [!DNL Airship] as [属性](https://docs.airship.com/guides/audience/attributes/) ターゲティングまたはトリガー用。

について詳しくは、 [!DNL Airship]を参照してください。 [Airship ドキュメント](https://docs.airship.com).

>[!TIP]
>
>この宛先コネクタとドキュメントページは、で作成および管理されます。 [!DNL Airship] チーム。 お問い合わせや更新のリクエストについては、 [support.airship.com](https://support.airship.com/).

## 前提条件 {#prerequisites}

オーディエンスをに送信する前に [!DNL Airship]は、以下を行う必要があります。

* で属性を有効にする [!DNL Airship] プロジェクト。
* 認証用のベアラートークンを生成します。

>[!TIP]
>
>を作成 [!DNL Airship] 次を使用してアカウント [このサインアップリンク](https://go.airship.eu/accounts/register/plan/starter/) まだの場合は、

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platformを通じて生成されたオーディエンス [セグメント化サービス](../../../segmentation/home.md). |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、フィールドマッピングに従って、必要なスキーマフィールド（例：メールアドレス、電話番号、姓）や ID と共に書き出します。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 属性を有効にする {#enable-attributes}

Adobe Experience Platform プロファイル属性は次に似ています [!DNL Airship] 属性とは、このページで後述するマッピングツールを使用して、Platform で相互に簡単にマッピングできます。

[!DNL Airship] プロジェクトには、事前定義済みのデフォルトの属性がいくつか用意されています。 カスタム属性がある場合、で定義する必要があります。 [!DNL Airship] 1 番目。 参照： [属性の設定と管理](https://docs.airship.com/tutorials/audience/attributes/) を参照してください。

## ベアラートークンの生成 {#bearer-token}

に移動 **[!UICONTROL 設定]** » **[!UICONTROL API と統合]** が含まれる [飛行船ダッシュボード](https://go.airship.com) を選択して、 **[!UICONTROL トークン]** 左側のメニューの

クリック **[!UICONTROL トークンを作成]**.

トークンのわかりやすい名前（例：「Adobe属性の宛先」）を指定し、ロールで「すべてのアクセス」を選択します。

クリック **[!UICONTROL トークンを作成]** 詳細を機密情報として保存します。

## ユースケース {#use-cases}

を使用する方法とタイミングをより深く理解するために、 [!DNL Airship Attributes] 宛先の場合、Adobe Experience Platformのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### のユースケース#1

Adobe Experience Platform内で収集したプロファイルデータを活用して、メッセージのパーソナライズや、 [!DNL Airship]のチャネル。 例えば、 [!DNL Experience Platform] 内で位置属性を設定するプロファイルデータ [!DNL Airship]. これにより、ホテルブランドは、各ユーザーの最寄りのホテルの場所の画像を表示することができます。

### のユースケース#2

Adobe Experience Platformの属性を活用してさらに強化する [!DNL Airship] プロファイルおよび SDK と組み合わせる [!DNL Airship] 予測データ。 例えば、小売業者は、ロイヤルティステータスと場所のデータ（Platform の属性）を含むオーディエンスおよびを作成できます。 [!DNL Airship] ネバダ州ラスベガスに住み、チャーンの可能性が高いゴールドロイヤルティステータスのユーザーにターゲットの絞られたメッセージを送信するために、データをチャーンすると予測しました。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL ベアラートークン]**：から生成したベアラートークン [!DNL Airship] ダッシュボード。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**：この宛先の説明を入力します。
* **[!UICONTROL ドメイン]**：米国または欧州のデータセンターを、次に応じて選択します [!DNL Airship] データセンターはこの宛先に適用されます。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* エクスポートする *id*、が必要です **[!UICONTROL ID グラフの表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png "宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。"){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## マッピングに関する考慮事項 {#mapping-considerations}

[!DNL Airship] 属性は、デバイスインスタンスを表すチャネル（例：iPhone）または、ユーザーのすべてのデバイスを共通の識別情報（例：カスタマー ID）にマッピングする名前付きユーザーで設定できます。 スキーマのプライマリ ID としてプレーンテキスト（ハッシュ化されていない）メールアドレスがある場合は、のメールフィールドを選択します **[!UICONTROL Source属性]** およびをにマッピングします [!DNL Airship] の右列の名前付きユーザー **[!UICONTROL ターゲット Id]**&#x200B;を参照してください。

![名前付きユーザーマッピング](../../assets/catalog/mobile-engagement/airship/mapping.png)

チャネルにマッピングする必要がある識別子（デバイス）については、ソースに基づいて適切なチャネルにマッピングします。 次の画像は、2 つのマッピングの作成方法を示しています。

* への IDFA iOS Advertising ID [!DNL Airship] iOS チャンネル
* Adobe `fullName` 属性先 [!DNL Airship] 「Full Name」属性

>[!NOTE]
>
>に表示されるわかりやすい名前を使用します。 [!DNL Airship] 属性マッピングのターゲットフィールドを選択する際のダッシュボード。

**ID をマッピング**

ソースフィールドを選択：

![Airship Attributes への接続](../../assets/catalog/mobile-engagement/airship/select-source-identity.png)

ターゲットフィールドを選択：

![Airship Attributes への接続](../../assets/catalog/mobile-engagement/airship/select-target-identity.png)

**属性をマッピング**

ソース属性を選択：

![ソースフィールドを選択](../../assets/catalog/mobile-engagement/airship/select-source-attributes.png)

ターゲット属性を選択：

![ターゲットフィールドを選択](../../assets/catalog/mobile-engagement/airship/select-target-attribute.png)

マッピングを確認：

![チャネルマッピング](../../assets/catalog/mobile-engagement/airship/mapping.png)


## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](../../../data-governance/home.md)を参照してください。
