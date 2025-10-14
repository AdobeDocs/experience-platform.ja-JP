---
keywords: 飛行船属性；飛行船宛先
title: Airship Attributes 接続
description: Airship 内でターゲティングするためのオーディエンス属性として、Adobe オーディエンスデータを Airship にシームレスに渡します。
exl-id: bfc1b52f-2d68-40d6-9052-c2ee1e877961
source-git-commit: 7377b2ffecf59fdb5ca5449daf1387ae1586bd63
workflow-type: tm+mt
source-wordcount: '1042'
ht-degree: 31%

---

# [!DNL Airship Attributes] 接続 {#airship-attributes-destination}

## 概要 {#overview}

[!DNL Airship] は、主要なカスタマーエンゲージメント Experience Platformであり、カスタマーライフサイクルのあらゆる段階で、ユーザーに対して有意義でパーソナライズされたオムニチャネルメッセージを提供するのを支援します。

この統合では、Adobe プロファイルデータをターゲティングまたはトリガー用の [&#x200B; 属性 &#x200B;](https://docs.airship.com/guides/audience/attributes/) として [!DNL Airship] に渡します。

[!DNL Airship] について詳しくは、[Airship のドキュメント &#x200B;](https://docs.airship.com) を参照してください。

>[!TIP]
>
>この宛先コネクタとドキュメントページは、[!DNL Airship] チームが作成および管理します。 お問い合わせや更新のリクエストについては、[support.airship.com](https://support.airship.com/) まで直接ご連絡ください。

## 前提条件 {#prerequisites}

オーディエンスを [!DNL Airship] に送信する前に、次の操作を行う必要があります。

* [!DNL Airship] プロジェクトで属性を有効にします。
* 認証用のベアラートークンを生成します。

>[!TIP]
>
>[&#x200B; このサインアップリンク &#x200B;](https://go.airship.eu/accounts/register/plan/starter/) から [!DNL Airship] アカウントをまだ作成していない場合は、作成します。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
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

Adobe Experience Platform プロファイル属性は [!DNL Airship] 属性に似ており、このページで後述するマッピングツールを使用すると、Experience Platformで簡単に相互にマッピングできます。

[!DNL Airship] プロジェクトには、複数の事前定義済みのデフォルト属性があります。 カスタム属性がある場合は、最初に定義する必要 [!DNL Airship] あります。 詳しくは、[&#x200B; 属性の設定と管理 &#x200B;](https://docs.airship.com/tutorials/audience/attributes/) を参照してください。

## ベアラートークンの生成 {#bearer-token}

[Airship ダッシュボード **[!UICONTROL 設定]**/**[!UICONTROL API と統合]** に移動し &#x200B;](https://go.airship.com) 左側のメニューで **[!UICONTROL トークン]** を選択します。

**[!UICONTROL トークンを作成]** をクリックします。

トークンのわかりやすい名前（例：「Adobe属性の宛先」）を指定し、ロールで「すべてのアクセス」を選択します。

**[!UICONTROL トークンを作成]** をクリックし、詳細を機密として保存します。

## ユースケース {#use-cases}

[!DNL Airship Attributes] の宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### のユースケース#1

Adobe Experience Platform内で収集したプロファイルデータを活用して、[!DNL Airship] のチャネルのいずれかでメッセージとリッチコンテンツをパーソナライズします。 例えば、プロファイルデータ [!DNL Experience Platform] 活用して、[!DNL Airship] 内のロケーション属性を設定します。 これにより、ホテルブランドは、各ユーザーの最寄りのホテルの場所の画像を表示することができます。

### のユースケース#2

Adobe Experience Platformの属性を活用してプロファイル [!DNL Airship] さらに強化し、SDKや [!DNL Airship] の予測データと組み合わせます。 例えば、retailerは、ロイヤルティステータスおよび場所データ（Experience Platformの属性）を持つオーディエンスを作成し、チャーンデータに [!DNL Airship] る見込みを持つネバダ州ラスベガスに住み、チャーンの可能性が高いゴールドのロイヤルティステータスを持つユーザーにターゲットを絞ったメッセージを送信します。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL ベアラートークン]**:[!DNL Airship] ダッシュボードから生成したベアラートークンです。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**：この宛先の説明を入力します。
* **[!UICONTROL ドメイン]**：この宛先に適用される [!DNL Airship] データセンターに応じて、米国または欧州のデータセンターを選択します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスをアクティブ化する手順については、[ストリーミングオーディエンス書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## マッピングに関する考慮事項 {#mapping-considerations}

[!DNL Airship] 属性は、デバイスインスタンスを表すチャネル（例：iPhone）または、ユーザーのすべてのデバイスを共通の識別情報（例：カスタマー ID）にマッピングする名前付きユーザーで設定できます。 スキーマにプレーンテキスト（ハッシュ化されていない）メールアドレスがプライマリ ID として存在する場合は、**[!UICONTROL Source属性のメールフィールドを選択し]** 以下に示すように、**[!UICONTROL ターゲット ID]** の下の右側の列で [!DNL Airship] 名のユーザーにマッピングします。

![&#x200B; 名前付きユーザーマッピング &#x200B;](../../assets/catalog/mobile-engagement/airship/mapping.png)

チャネルにマッピングする必要がある識別子（デバイス）については、ソースに基づいて適切なチャネルにマッピングします。 次の画像は、2 つのマッピングの作成方法を示しています。

* [!DNL Airship] iOS チャネルへの IDFA iOS Advertising ID
* 「Full Name」属性 [!DNL Airship] 対するAdobe `fullName` 属性

>[!NOTE]
>
>属性マッピングのターゲットフィールドを選択する際に、[!DNL Airship] ダッシュボードに表示されるわかりやすい名前を使用します。

**ID をマッピング**

ソースフィールドを選択：

![&#x200B; 飛行船属性への接続 &#x200B;](../../assets/catalog/mobile-engagement/airship/select-source-identity.png)

ターゲットフィールドを選択：

![&#x200B; 飛行船属性への接続 &#x200B;](../../assets/catalog/mobile-engagement/airship/select-target-identity.png)

**Map 属性**

ソース属性を選択：

![ソースフィールドを選択](../../assets/catalog/mobile-engagement/airship/select-source-attributes.png)

ターゲット属性を選択：

![&#x200B; ターゲットフィールドを選択 &#x200B;](../../assets/catalog/mobile-engagement/airship/select-target-attribute.png)

マッピングを確認：

![&#x200B; チャネルマッピング &#x200B;](../../assets/catalog/mobile-engagement/airship/mapping.png)


## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](../../../data-governance/home.md)を参照してください。
