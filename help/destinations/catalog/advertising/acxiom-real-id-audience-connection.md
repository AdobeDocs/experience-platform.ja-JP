---
title: Acxiom Real ID Audience Connection
description: ' [!DNL Acxiom Real ID Audience Connection] 宛先を使用して [!DNL Acxiom''s Real ID]  テクノロジーでオーディエンスを強化し、 [!DNL Altice]、 [!DNL Ampersand]、 [!DNL Comcast]などの複数のプラットフォームにオーディエンスをアクティブ化します。'
badge: label="ベータ版" type="Informative"
exl-id: 5f1f0f7f-ac46-42bd-8002-be50fab5a76b
source-git-commit: 20427c4c8826905a77fac04d055d523b12a6f739
workflow-type: tm+mt
source-wordcount: '999'
ht-degree: 12%

---

# [!DNL Acxiom Real ID Audience Connection]宛先

>[!NOTE]
>
>[!DNL Acxiom Real ID Audience Connection]宛先はベータ版です。 この宛先コネクタとドキュメント ページは、[!DNL Acxiom] チームによって作成および管理されています。 問い合わせやアップデートのリクエストについては、Acxiomに直接[こちら](mailto:acxiom-adobe-help@acxiom.com)までお問い合わせください。

[!DNL Acxiom Real ID Audience Connection]宛先を使用して、[!DNL Acxiom's] [Real ID](https://www.acxiom.com/real-id/real-id/) テクノロジーでオーディエンスを強化し、[!DNL Altice]、[!DNL Ampersand]、[!DNL Comcast]などの複数のプラットフォームにオーディエンスをアクティブ化します。

このチュートリアルでは、[!DNL Acxiom Real ID Audience Connection] ユーザーインターフェイスを使用して[!DNL Adobe Experience Platform]宛先コネクタを作成する手順を説明します。 このコネクタは、選択した宛先にオーディエンスを構築および配布します。

## ユースケース {#use-cases}

このコネクタは、Acxiom Real Identityを識別子として[!DNL Real-Time CDP]に読み込んだクライアントをサポートします。 [!DNL Acxiom Real ID Audience Connection]宛先を使用する方法とタイミングをより正確に把握するために、このコネクタを使用して[!DNL Adobe Experience Platform]のお客様が解決できる使用例を次に示します。

### Experience PlatformからAcxiom アカウントへのオーディエンスの送信 {#send-audiences}

クロスチャネル獲得のために[!DNL Experience Platform]から[!DNL Acxiom] アカウントにオーディエンスを送信するマーケティング担当者の場合は、この宛先コネクタを使用します。

たとえば、世界的な金融機関ブランドのマーケティングオペレーション部門は、複数の広告プラットフォームを通じたクロスチャネルの顧客獲得に関心を示しています。 [!DNL Acxiom Real ID Audience Connection]宛先コネクタを使用して、[!DNL Experience Platform]から[!DNL Acxiom]までのオーディエンスを送信し、[!DNL Acxiom's Real ID]技術でオーディエンスを強化し、[!DNL Altice]や[!DNL Ampersand]、[!DNL Comcast]などの複数のプラットフォームにオーディエンスをアクティブ化できます。


## 前提条件 {#prerequisites}

* **利用条件の確認：**&#x200B;新しい[!DNL Acxiom Real ID Audience Connection]宛先を設定する前に、[!DNL Acxiom's]利用条件に関する契約書を読んで署名する必要があります。 実行した販売注文が完了すると、契約書へのリンクが届きます。
* **お客様のAdobe組織IDについて：**&#x200B;お客様の[!DNL Adobe]組織IDは、利用条件を満たすために必要です。 組織ID[!DNL Adobe's]を&#x200B;*表示する方法について詳しくは、* [Experience Cloudの組織](https://experienceleague.adobe.com/ja/docs/core-services/interface/administration/organizations#concept_EA8AEE5B02CF46ACBDAD6A8508646255)のトピックを参照してください。
* **[!DNL Acxiom's Real ID]製品のライセンスを取得：** ライセンスを取得したら、[!DNL Real-Time CDP]以内にAcxiomのReal IDを利用できるようにします。 詳しくは、[Acxiom データの強化](https://experienceleague.adobe.com/en/docs/experience-platform/destinations/catalog/data-partner/acxiom-data-enhancement)を参照してください。


## サポートされている ID {#supported-identities}

[!DNL Acxiom's Real ID Audience Connection]宛先は、次の表に示すIDのアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/ja/docs/experience-platform/identity/features/namespaces) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---------------|----------------|----------------|
| 実際のID | [!DNL Acxiom Real ID] | ソース IDがAcxiom Real ID名前空間である場合は、このターゲット IDを選択します。 |
| extern_id | カスタムユーザーID | ソース IDがカスタム名前空間である場合は、このターゲット IDを選択します。 |

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスの由来 | サポートあり | 説明 |
|---------------|----------------|----------------|
| セグメント化サービス | ○ | Experience Platform [ セグメント化サービス ](https://experienceleague.adobe.com/ja/docs/experience-platform/segmentation/home)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | ○ | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}

オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス ](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス ](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [ データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}

## サポートされる宛先 {#supported-destinations}

[!DNL Acxiom's Real ID Audience Connection]宛先は、現在、次のプラットフォームへのオーディエンスのアクティブ化をサポートしています。

* [!DNL Altice]
* [!DNL Ampersand]
* [!DNL Comcast]
* [!DNL Cox]
* [[!DNL LG Ads]](#lg-ads)
* [!DNL Spectrum]
* [!DNL Viant]

## 宛先への接続 {#connect}

[!DNL Acxiom's Real ID Audience Connection]宛先への認証は、便利な方法で自動的にバックグラウンドで処理されます。

## 配信先固有の設定 {#destination-settings}

一部の[!DNL Acxiom Real ID Audience Connection]宛先には追加情報が必要です。 以下の節では、これらのオプションの設定方法に関する詳細なガイダンスを提供します。

### [!DNL LG Ads] {#lg-ads}

宛先の詳細を設定するには、以下のフィールドに入力します。

* **セグメント カテゴリ**: セグメントが属するターゲット カテゴリまたは垂直方向。 例：金融、自動車、医療など

![LG広告宛先の詳細](../../assets/catalog/advertising/acxiom-real-id-audience-connection/real_id_lg_ads_destination_details.png)


## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![ ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}



この宛先に対してオーディエンスをアクティブ化する手順については、[バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](https://experienceleague.adobe.com/en/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations)を参照してください。

>[!NOTE]
>
>宛先[!DNL Acxiom Real ID Audience Connection]は、完全なファイル書き出しのみをサポートしています。


### 属性と ID のマッピング {#map}

宛先[!DNL Acxiom Real ID Audience Connection]がオーディエンスデータを正しく受け取るには、Experience Platformのソースフィールドを正しい[!DNL Acxiom Real ID Audience Connection] ターゲットフィールドにマッピングする必要があります。

[!DNL Acxiom Real ID Audience Connection]は、次のターゲットフィールドへのマッピングのみを許可します。

| フィールド名 | 説明 | 必須 |
|--------------------|------------|--------|
| 実際のID | Real IDは、Acxiomの独自のID解決グラフからの一意の36 バイト英数字ID （ID）です。リレーショナルデータベースのプライマリキーと同様です。 個人、世帯、住所を表す識別子です。 | ○ |

{style="table-layout:auto"}

**[!UICONTROL Source Field]**&#x200B;列に、対応するターゲットフィールドにマッピングするソース属性の名前を入力するか、矢印アイコンを選択して&#x200B;**[!UICONTROL Select source field]**&#x200B;画面を開きます。 次に、**[!UICONTROL Next]**を選択します。
![ マッピング画面](../../assets/catalog/advertising/acxiom-real-id-audience-connection/real_id_mapping_screen.png)


[!DNL Adobe's]標準スキーマを使用していない場合は、クエリサービスを使用して[標準スキーマにフィールド名を入力する方法について、](../../../query-service/ui/overview.md) クエリサービス UI ガイド [!DNL Adobe]のドキュメントを参照してください。


### レビュー {#review}

上記のすべての手順を完了したら、宛先の接続ステータスとオーディエンスの詳細を確認してからアクティブ化（配信）することができます。 選択したオーディエンスがリストの下部に表示されます。 各オーディエンスは、[!DNL Acxiom Real ID Audience Connection] APIへの個別の呼び出しになります。

結果に問題がなければ、**[!UICONTROL Finish]**&#x200B;を選択して宛先をアクティブ化します。

![ オーディエンスのレビュー](../../assets/catalog/advertising/acxiom-real-id-audience-connection/real_id_review_audience.png)


## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/en/docs/experience-platform/data-governance/home)を参照してください。

## トラブルシューティング {#troubleshooting}

宛先の担当者がオーディエンスを見つけられない場合は、[!DNL Adobe]担当者にお問い合わせください。

次の情報を[!DNL Adobe]担当者に提供する必要があります：

* オーディエンス名
* 宛先名
* オーディエンスのアクティブ化日
* エクスポートされたファイル名

## 次の手順 {#next-steps}

選択した宛先プラットフォームに対するオーディエンスのアクティベートが完了しました。 次に、宛先プラットフォームの担当者に連絡して、キャンペーンの設定を開始します。
