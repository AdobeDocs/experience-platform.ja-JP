---
title: Acxiom Real ID&trade; Audience Connection
description: ' [!DNL Acxiom Real ID&trade; Audience Connection]  宛先を使用すると、 [!DNL Acxiom''s Real ID]  テクノロジーでオーディエンスを強化し、 [!DNL Altice]、 [!DNL Ampersand]、 [!DNL Comcast] などの複数のプラットフォームに対してオーディエンスをアクティブ化できます。'
badge: label="ベータ版" type="Informative"
exl-id: 5f1f0f7f-ac46-42bd-8002-be50fab5a76b
source-git-commit: 1013487e2c38aeb1e2b0388f0c317afdcf02ba62
workflow-type: tm+mt
source-wordcount: '939'
ht-degree: 12%

---

# [!DNL Acxiom Real ID™ Audience Connection] の宛先

>[!NOTE]
>
>[!DNL Acxiom Real ID™ Audience Connection] の宛先はベータ版です。 この宛先コネクタとドキュメントページは、[!DNL Acxiom] チームが作成および管理します。 お問い合わせや更新のリクエストについては、Acxiom に直接お問い合わせください [ こちら ](mailto:acxiom-adobe-help@acxiom.com)。

[!DNL Acxiom Real ID Audience Connection] の宛先を使用すると、[!DNL Acxiom's]Real ID™[ テクノロジー ](https://www.acxiom.com/real-id/real-id/) 使用してオーディエンスを強化し、[!DNL Altice]、[!DNL Ampersand]、[!DNL Comcast] などの複数のプラットフォームに対してオーディエンスをアクティブ化できます。

このチュートリアルでは、[!DNL Acxiom Real ID Audience Connection] ユーザーインターフェイスを使用して [!DNL Adobe Experience Platform] 宛先コネクタを作成する手順を説明します。 このコネクタは、オーディエンスを作成し、選択した宛先に配信するために使用されます。

## ユースケース {#use-cases}

このコネクタは、ID として Acxiom Real Identity がReal-Time CDPにロードされているクライアントをサポートします。 [!DNL Acxiom Real ID Audience Connection] の宛先を使用する方法とタイミングをより深く理解するために、[!DNL Adobe Experience Platform] のお客様がこのコネクタを使用して解決できるサンプルユースケースを以下に示します。

### Experience Platformから Acxiom アカウントへのオーディエンスの送信 {#send-audiences}

マーケティング専門家が [!DNL Experience Platform] から [!DNL Acxiom] アカウントにオーディエンスを送信する場合、クロスチャネル獲得のために、この宛先コネクタを使用します。

例えば、グローバルな金融サービスブランドのマーケティング運営部門は、複数の広告プラットフォームを通じたクロスチャネルの顧客獲得に関心を持っています。 [!DNL Acxiom Real ID Audience Connection] 宛先コネクタを使用して、オーディエンスを [!DNL Experience Platform] から [!DNL Acxiom] に送信したり、[!DNL Acxiom's Real ID] のテクノロジーでオーディエンスを強化したり、[!DNL Altice]、[!DNL Ampersand]、[!DNL Comcast] などの複数のプラットフォームに対してオーディエンスをアクティブ化したりできます。


## 前提条件 {#prerequisites}

* **利用条件を確認する：** 新しい [!DNL Acxiom Real ID Audience Connection] の宛先を設定する前に、利用条件を読み、署名す [!DNL Acxiom's] 必要があります。 実行した受注が完了すると、基本契約へのリンクが表示されます。 契約書に署名するまで、Experience Platformの宛先カタログに [!DNL Acxiom Real ID Audience Connection] の宛先カードは表示されません。 契約書に同意して署名すると、オンボーディングプロセス [!DNL Adobe] 完了し、[!DNL Acxiom Real ID Audience Connection] の宛先カードが表示されます。
* **Adobe組織 ID を把握：** ユーザー契約の条件を満たすには、[!DNL Adobe] 組織 ID が必要です。 [!DNL Adobe's] 組織 ID を表示 ** する方法について詳しくは、[Experience Cloudの組織 ](https://experienceleague.adobe.com/ja/docs/core-services/interface/administration/organizations#concept_EA8AEE5B02CF46ACBDAD6A8508646255) を参照してください。
* **製品のライセンス [!DNL Acxiom's Real ID] 取得：** ライセンスを取得したら、Real-Time CDP内で Acxiom の Real ID を使用できるようにします。 詳細については、「[Acxiom Data Enhancement](https://experienceleague.adobe.com/en/docs/experience-platform/destinations/catalog/data-partner/acxiom-data-enhancement)」を参照してください。


## サポートされている ID {#supported-identities}

宛先 [!DNL Acxiom's Real ID Audience Connection]、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/ja/docs/experience-platform/identity/features/namespaces) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---------------|----------------|----------------|
| 実際の ID | [!DNL Acxiom Real ID] | ソース ID が Acxiom の実 ID 名前空間である場合は、このターゲット ID を選択します。 |
| extern_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------------|----------------|----------------|
| セグメント化サービス | ✓ | Experience Platform [ セグメント化サービス ](https://experienceleague.adobe.com/ja/docs/experience-platform/segmentation/home) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](https://experienceleague.adobe.com/ja/docs/experience-platform/segmentation/ui/audience-portal#import-audience)オーディエンス。 |


## サポートされる宛先 {#supported-destinations}

現在 [!DNL Acxiom's Real ID Audience Connection] 宛先では、次のプラットフォームへの Audience Activation をサポートしています。

* [!DNL Altice]
* [!DNL Ampersand]
* [!DNL Comcast]
* [!DNL Cox]
* [[!DNL LG Ads]](#lg-ads)
* [!DNL Spectrum]
* [!DNL Viant]

## 宛先への接続 {#connect}

宛先へ [!DNL Acxiom's Real ID Audience Connection] 認証は、ユーザーの利便性のために、バックグラウンドで自動的に処理されます。

## 宛先固有の設定 {#destination-settings}

一部 [!DNL Acxiom Real ID Audience Connection] 宛先には、追加情報が必要です。 以下の節では、これらのオプションの設定方法に関する詳細なガイダンスを示します。

### [!DNL LG Ads] {#lg-ads}

宛先の詳細を設定するには、以下のフィールドを入力します。

* **セグメントカテゴリ**：セグメントの対象となるターゲットカテゴリまたは垂直方向です。 例：金融サービス、自動車、医療など

![LG 広告の宛先の詳細 ](../../assets/catalog/advertising/acxiom-real-id-audience-connection/real_id_lg_ads_destination_details.png)


## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}



この宛先に対してオーディエンスをアクティブ化する手順については、[バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](https://experienceleague.adobe.com/en/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations)を参照してください。

>[!NOTE]
>
>[!DNL Acxiom Real ID Audience Connection] の宛先では、完全ファイルの書き出しのみがサポートされます。


### 属性と ID のマッピング {#map}

[!DNL Acxiom Real ID Audience Connection] の宛先がオーディエンスデータを正しく受け取るには、Experience Platformのソースフィールドを正しい [!DNL Acxiom Real ID Audience Connection] ターゲットフィールドにマッピングする必要があります。

[!DNL Acxiom Real ID Audience Connection] では、次のターゲットフィールドへのマッピングのみが許可されます。

| フィールド名 | 説明 | 必須 |
|--------------------|------------|--------| 
| 実際の ID | 実際の ID は、Acxiom 独自の ID 解決グラフに基づく一意の 36 バイトの英数字識別子（ID）で、リレーショナル・データベースのプライマリ・キーと似ています。 人、世帯または住所を表す識別子です。 | ○ |



**[!UICONTROL Source フィールド]** 列で、対応するターゲットフィールドにマッピングするソース属性の名前を入力するか、矢印アイコンを選択して **[!UICONTROL ソースフィールドを選択]** 画面を開きます。 次に、「**[!UICONTROL 次へ]**」を選択します。
![ マッピング画面 ](../../assets/catalog/advertising/acxiom-real-id-audience-connection/real_id_mapping_screen.png)


標準スキーマを使用していない場合 [!DNL Adobe's]、クエリサービスを使用して [ 標準スキーマにフィールド名を入力する方法について詳しくは ](../../../query-service/ui/overview.md) クエリサービス UI ガイド [!DNL Adobe] ドキュメントを参照してください。


### レビュー {#review}

上記のすべての手順を完了すると、宛先をアクティブ化（配布）する前に、宛先接続のステータスとオーディエンスの詳細を確認できます。 選択したオーディエンスは、リストの下部に表示されます。 各オーディエンスは、[!DNL Acxiom Real ID Audience Connection] API への個別の呼び出しになります。

結果に満足している場合は、「**[!UICONTROL 完了]**」を選択して、宛先をアクティベートします。

![ オーディエンスをレビュー ](../../assets/catalog/advertising/acxiom-real-id-audience-connection/real_id_review_audience.png)


## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/en/docs/experience-platform/data-governance/home)を参照してください。

## トラブルシューティング {#troubleshooting}

宛先担当者がオーディエンスを見つけることができない場合は、[!DNL Adobe] 担当者にお問い合わせください。

[!DNL Adobe] 担当者に次の情報を提供する必要があります。

* オーディエンス名
* 宛先名
* Audience Activation の日付
* 書き出すファイルの名前

## 次の手順 {#next-steps}

このチュートリアルでは、選択した宛先プラットフォームへのオーディエンスを正常にアクティブ化しました。 次に、宛先プラットフォームの担当者に連絡して、キャンペーンの設定を開始します。
