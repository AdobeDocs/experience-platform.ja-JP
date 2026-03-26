---
title: Audience Analysisの宛先
description: Customer Journey Analyticsで対象となるオーディエンスを表示します。
badgeLimitedAvailability: label="限定提供" type="Informative"
exl-id: 81437237-d746-4ce9-b938-7d2541f0ed32
hide: true
hidefromtoc: true
source-git-commit: 20427c4c8826905a77fac04d055d523b12a6f739
workflow-type: tm+mt
source-wordcount: '867'
ht-degree: 23%

---

# Audience Analysisの宛先

[!UICONTROL Audience Analysis]の宛先を使用して、[!DNL Adobe Experience Platform]のオーディエンスデータを[Customer Journey Analytics](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-overview/cja-overview.html?lang=ja)にエンリッチします。 生成されるエンリッチメントデータに含めるオーディエンスを選択できます。 オーディエンスの選定は、[Analysis Workspace](https://experienceleague.adobe.com/docs/analytics-platform/using/cja-workspace/home.html?lang=ja) レポートのディメンションとして使用できます。

>[!AVAILABILITY]
>
>この段階では、検証は限定的です。 この宛先の使用に関心がある場合は、Adobe アカウントチームにお問い合わせください。

## 前提条件 {#prerequisites}

この宛先を使用する前に、以下が必要です。

* Audience Analysisの宛先を使用するには、プロビジョニングする必要があります。 この宛先をまだ使用するようにプロビジョニングされていない場合は、Adobe アカウントチームにお問い合わせください。
* Customer Journey Analyticsを使用するには、プロビジョニングが必要です。
* [!DNL Adobe Experience Platform]に少なくとも1つのオーディエンスを作成する必要があります。

## サポートされている ID {#supported-identities}

Audience Analysisでは、次の表に示すIDのアクティベーションがサポートされています。 ID の詳細は[こちら](/help/identity-service/features/namespaces.md)から。通常、Experience Cloud ID （ECID）が使用されます。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | GOOGLE ADVERTISING ID | ソース IDがGAID名前空間である場合は、GAID ターゲット IDを選択します。 |
| IDFA | Apple の広告主 ID | ソース IDがIDFA名前空間の場合は、IDFA ターゲット IDを選択します。 |
| ECID | Experience Cloud ID | ECIDを表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「[!DNL Adobe Experience Cloud] ID」、「[!DNL Adobe Experience Platform] ID」というエイリアスでも参照できます。 詳しくは、[ECID](/help/identity-service/features/ecid.md)の次のドキュメントを参照してください。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | プレーンテキストとSHA256 ハッシュ化された電話番号の両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | プレーンテキストとSHA256 ハッシュ化された電子メールアドレスの両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| extern_id | カスタムユーザーID | ソース IDがカスタム名前空間である場合は、このターゲット IDを選択します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この宛先を使用する場合、次のタイプのオーディエンスがサポートされます。

| オーディエンスの由来 | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md)を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの生成元 | × | このカテゴリには、[!DNL Segmentation Service]を通じて生成されたオーディエンス以外のすべてのオーディエンスのオリジンが含まれます。 [様々なオーディエンスの起源](/help/segmentation/ui/audience-portal.md#customize)について読みます。 次に例を示します。 <ul><li> カスタムアップロードオーディエンス [がCSV ファイルからExperience Platformに](../../../segmentation/ui/audience-portal.md#import-audience)をインポートしました。</li><li> 類似オーディエンス， </li><li> 連合オーディエンス， </li><li> [!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで生成されたオーディエンス </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスのデータタイプ別にサポートされるオーディエンス：

| オーディエンスのデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [人物オーディエンス &#x200B;](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルにもとづいて、マーケティング施策の特定のグループをターゲットにすることができます。 | 買い物客やカートの放棄が多い |
| [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) | × | アカウントベースドマーケティング戦略のために、特定の組織内の個人をターゲットにします。 | B2B マーケティング |
| [見込みオーディエンス &#x200B;](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないが、ターゲットオーディエンスと特徴を共有する個人をターゲットにします。 | サードパーティデータによる見込み顧客の開拓 |
| [&#x200B; データセットの書き出し](/help/catalog/datasets/overview.md) | × | [!DNL Adobe Experience Platform] データ レイクに保存されている構造化データのコレクション。 | レポート，データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Audience export]** | オーディエンス分析の宛先で使用されている識別子（名前、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいてExperience Platformでプロファイルが更新されると、コネクターは更新をダウンストリームの宛先プラットフォームに送信します。 [ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## 新しい宛先を設定 {#configure-destination}

>[!IMPORTANT]
>
>宛先を作成するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

この宛先を作成するには、[宛先設定チュートリアル &#x200B;](../../ui/connect-destination.md)で説明されている手順に従います。

### 宛先の詳細 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：宛先名。
* **[!UICONTROL Description]**：宛先の説明。
* **[!UICONTROL Datastream ID]**：条件を満たすオーディエンスでエンリッチするデータストリーム ID。 このIDは、[&#x200B; データストリームマネージャー](/help/datastreams/overview.md)で取得できます。
* **[!UICONTROL Integration alias]**：統合エイリアス。

### アラート {#alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

* **[!UICONTROL Activation Skipped Rate Exceed]**: アクティベーション スキップ率がしきい値を超えた場合に通知されます。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

### ガバナンスポリシーと履行アクション {#governance-policy}

このオプションのセクションでは、データガバナンスポリシーを定義し、オーディエンスが送信され、アクティブな場合に使用されるデータがコンプライアンスに準拠していることを確認できます。

宛先の目的のマーケティングアクションの選択が完了したら、**[!UICONTROL Create]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

宛先を作成したら、宛先に対して目的のオーディエンスをアクティブ化できます。

1. 作成した宛先にまだ存在しない場合は、**[!UICONTROL Destinations]** > **[!UICONTROL Browse]**&#x200B;に移動して、もう一度見つけることができます。
1. **[!UICONTROL Activate audiences]** を選択します。
1. 選定を分析する目的のオーディエンスを選択します。 終了したら「**[!UICONTROL Next]**」を選択します。
1. 宛先設定とオーディエンス設定を確認し、**[!UICONTROL Finish]**&#x200B;を選択します。

**[!UICONTROL Activate audiences]** ページに戻って、今後分析するオーディエンスをさらに追加できます。 アクティベートしたオーディエンスを削除することはできません。
