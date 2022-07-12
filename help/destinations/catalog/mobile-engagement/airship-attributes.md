---
keywords: 飛行船の属性；飛行船の宛先
title: Airship Attributes 接続
description: Airship 内でターゲティングするために、Adobeのオーディエンスデータをオーディエンス属性として Airship にシームレスに渡します。
exl-id: bfc1b52f-2d68-40d6-9052-c2ee1e877961
source-git-commit: fd2019feb25b540612a278cbea5bf5efafe284dc
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 8%

---

# [!DNL Airship Attributes] 接続 {#airship-attributes-destination}

## 概要 {#overview}

[!DNL Airship] は、顧客ライフサイクルの各段階で、意味のあるパーソナライズされたオムニチャネルメッセージをユーザーに届けるのに役立つ、主要な顧客エンゲージメントプラットフォームです。

この統合により、Adobeプロファイルデータが [!DNL Airship] as [属性](https://docs.airship.com/guides/audience/attributes/) （ターゲティングまたはトリガーの場合）

詳しくは、以下を参照してください。 [!DNL Airship]を参照し、 [航空船ドキュメント](https://docs.airship.com).

>[!TIP]
>
>このドキュメントページは、 [!DNL Airship] チーム。 お問い合わせや更新のご依頼は、直接 [support.airship.com](https://support.airship.com/).

## 前提条件 {#prerequisites}

オーディエンスセグメントを [!DNL Airship]を使用する場合は、次の操作を行う必要があります。

* 属性を [!DNL Airship] プロジェクト。
* 認証用の bearer トークンを生成します。

>[!TIP]
>
>の作成 [!DNL Airship] 経由のアカウント [この登録リンク](https://go.airship.eu/accounts/register/plan/starter/) まだお持ちでない場合は、

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：フィールドマッピングに従った電子メールアドレス、電話番号、姓 ) や ID。 |
| 書き出し頻度 | **[!UICONTROL ストリーミング]** | ストリーミングの宛先は、API ベースの接続です。 セグメント評価に基づいてExperience Platform内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。 詳細を表示 [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations). |

{style=&quot;table-layout:auto&quot;}

## 属性を有効にする {#enable-attributes}

Adobe Experience Platformプロファイル属性は、 [!DNL Airship] このページで後述するマッピングツールを使用して、属性を Platform 内で簡単に相互にマッピングできます。

[!DNL Airship] プロジェクトには、事前定義済みの属性とデフォルトの属性がいくつかあります。 カスタム属性がある場合、その属性を [!DNL Airship] 1 つ目は 詳しくは、 [属性の設定と管理](https://docs.airship.com/tutorials/audience/attributes/) 」を参照してください。

## bearer トークンを生成 {#bearer-token}

に移動します。 **[!UICONTROL 設定]** &quot; **[!UICONTROL API と統合]** 内 [飛行船ダッシュボード](https://go.airship.com) を選択し、 **[!UICONTROL トークン]** をクリックします。

クリック **[!UICONTROL トークンを作成]**.

トークンのわかりやすい名前 ( 例：「Adobe属性の宛先」) を指定し、役割に「すべてのアクセス」を選択します。

クリック **[!UICONTROL トークンを作成]** 詳細を機密として保存します。

## ユースケース {#use-cases}

をいつどのように使用するかをより深く理解するのに役立ちます。 [!DNL Airship Attributes] の宛先について、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

### 使用例#1

Adobe Experience Platform内で収集されたプロファイルデータを活用して、任意の [!DNL Airship]のチャネル。 例えば、 [!DNL Experience Platform] 内で場所属性を設定するプロファイルデータ [!DNL Airship]. これにより、ホテルブランドは各ユーザーに最も近いホテルの場所の画像を表示できます。

### 使用例#2

Adobe Experience Platformの属性を活用してさらに強化 [!DNL Airship] プロファイルを作成し、SDK または [!DNL Airship] 予測データ。 例えば、小売業者は、ロイヤリティステータスと場所のデータ（Platform の属性）を含むセグメントを作成し、 [!DNL Airship] では、強いターゲットを絞ったメッセージを、ネバダ州ラスベガスに住む、チャーンの確率が高いゴールドロイヤリティステータスのユーザーに送信するようにデータをチャーン化する予測がおこなわれていました。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションに記載されているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

* **[!UICONTROL Bearer トークン]**:から生成した bearer トークン [!DNL Airship] ダッシュボード。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。 UI でフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL ドメイン]**:米国または EU のデータセンターを選択します（どちらかに応じて選択します）。 [!DNL Airship] データセンターがこの宛先に適用されます。

### アラートの有効化 {#enable-alerts}

アラートを有効にして、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の指定が完了したら、 **[!UICONTROL 次へ]**.

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [ストリーミングセグメントの書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-segment-streaming-destinations.md) を参照してください。

## マッピングに関する考慮事項 {#mapping-considerations}

[!DNL Airship] 属性は、デバイスインスタンス (iPhoneなど ) を表すチャネルに対して設定できます。また、すべてのユーザーのデバイスを顧客 ID などの共通の識別子にマッピングする名前付きユーザーに対して設定することもできます。 スキーマにプレーンテキスト（ハッシュ化されていない）の電子メールアドレスがプライマリ ID である場合は、 **[!UICONTROL ソース属性]** と [!DNL Airship] の下の右側の列にユーザーという名前が付けられました **[!UICONTROL ターゲット ID]**、以下に示すように。

![特定ユーザーマッピング](../../assets/catalog/mobile-engagement/airship/mapping.png)

チャネルにマッピングする必要がある識別子（デバイスなど）については、ソースに基づいて適切なチャネルにマッピングされます。 次の画像は、2 つのマッピングの作成方法を示しています。

* IDFA iOS Advertising ID を [!DNL Airship] iOSチャネル
* Adobe `fullName` 属性 [!DNL Airship] 「フルネーム」属性

>[!NOTE]
>
>ユーザーにわかりやすい名前を使用します。この名前は、 [!DNL Airship] ダッシュボードを使用して属性マッピングのターゲットフィールドを選択する場合。

**ID をマッピング**

ソースフィールドを選択:

![飛行船属性に接続](../../assets/catalog/mobile-engagement/airship/select-source-identity.png)

ターゲットフィールドを選択：

![飛行船属性に接続](../../assets/catalog/mobile-engagement/airship/select-target-identity.png)

**Map 属性**

ソース属性を選択：

![ソースフィールドを選択](../../assets/catalog/mobile-engagement/airship/select-source-attributes.png)

ターゲット属性を選択：

![ターゲットフィールドを選択](../../assets/catalog/mobile-engagement/airship/select-target-attribute.png)

マッピングの検証：

![チャネルマッピング](../../assets/catalog/mobile-engagement/airship/mapping.png)


## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。詳しくは、 [!DNL Adobe Experience Platform] データガバナンスを強制します。詳しくは、 [データガバナンスの概要](../../../data-governance/home.md).
