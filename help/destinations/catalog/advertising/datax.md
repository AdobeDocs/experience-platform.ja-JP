---
title: Verizon MediaYahoo DataX接続
description: DataX は Verizon Media／Yahoo の集約インフラストラクチャです。安全で自動化されたスケーラブルな方法で Verizon Media／Yahoo が外部パートナーとデータを交換できるよう様々なコンポーネントをホストしています。
exl-id: 7d02671d-8650-407d-9c9f-fad7da3156bc
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '934'
ht-degree: 31%

---

# [!DNL Verizon Media/Yahoo DataX] 接続

## 概要 {#overview}

[!DNL DataX]は、[!DNL Verizon Media/Yahoo]が安全で自動化されたスケーラブルな方法で外部パートナーとデータを交換できるようにする、様々なコンポーネントをホストする集約[!DNL Verizon Media/Yahoo] インフラストラクチャです。

>[!IMPORTANT]
>
>この宛先コネクタとドキュメント ページは、[!DNL Verizon Media/Yahoo]の[!DNL DataX] チームによって作成および管理されています。 問い合わせや更新のリクエストについては、[dataoperations@yahooinc.com](mailto:dataoperations@yahooinc.com)から直接お問い合わせください

## 前提条件 {#prerequisites}

**MDM ID**

これは[!DNL Yahoo DataX]の一意の識別子であり、この宛先へのデータ書き出しを設定するための必須フィールドです。 このIDがわからない場合は、[!DNL Yahoo DataX] アカウント マネージャーにお問い合わせください。

**分類メタデータ**

分類リソースは、基本[!DNL DataX] メタデータ構造上の拡張機能を定義します

```json
{

  >>(Base DataX Metadata)<<

        "extensions": { "action":
        {string}, "incrementalData":
        {
                "taxonomyId": {string}
                },
                "links": [{
                "rel": "https://datax.yahooapis.com/rels/fullTaxonomy", "title": "Full
                Taxonomy post processing",
                "href": {string}
                ]
        }
}
```

[分類メタデータ ](https://developer.verizonmedia.com/datax/guide/taxonomy/taxo-metadata/)の詳細については、[!DNL DataX]開発者ドキュメントをご覧ください。

## レート制限とガードレール {#rate-limits-guardrails}

>[!IMPORTANT]
>
>100を超えるオーディエンスを[!DNL Verizon Media/Yahoo DataX]にアクティブ化すると、宛先からレート制限エラーが発生する場合があります。 この宛先に対してオーディエンスをアクティブ化する場合は、1つのアクティベーションデータフローで100未満のオーディエンスをアクティブ化してみてください。 さらにセグメントをアクティブ化する必要がある場合は、同じアカウントに新しい宛先を作成します。

[!DNL DataX]は、[DataX ドキュメント ](https://developer.verizonmedia.com/datax/guide/rate-limits/)に記載されている分類およびオーディエンスへの投稿のクォータ制限に従ってレート制限されています。


| エラーコード | エラーメッセージ | 説明 |
|---------|----------|---------|
| 429 リクエストが多すぎます | レート制限が1時間あたりの上限を超えました&#x200B;**（制限：100）** | プロバイダーごとに1時間で許可されるリクエストの数。 |

{style="table-layout:auto"}

## サポートされている ID {#supported-identities}

[!DNL Verizon Media]は、次の表に示すIDのアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html#getting-started) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | プレーンテキストとSHA256 ハッシュ化された電子メールアドレスの両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| GAID | GOOGLE ADVERTISING ID | ソース IDがGAID名前空間である場合は、GAID ターゲット IDを選択します。 |
| IDFA | Apple の広告主 ID | ソース IDがIDFA名前空間の場合は、IDFA ターゲット IDを選択します。 |

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
| 書き出しタイプ | **[!UICONTROL Audience export]** | Verizon Mediaの宛先で使用されている識別子（電子メール、GAID、IDFA）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)の詳細についてはこちらを参照してください。 |

{style="table-layout:auto"}

## ユースケース {#use-cases}

[!DNL DataX] APIは、[!DNL Verizon Media] （VMG）の電子メールアドレスにキーを設定した特定のオーディエンスグループをターゲットにする広告主が利用できます。新しいオーディエンスをすばやく作成し、VMGのほぼリアルタイム APIを使用して、目的のオーディエンスグループをプッシュできます。

## 宛先に接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

Experience Platform UI![の](/help/destinations/assets/catalog/advertising/yahoo-datax/catalog.png)Yahoo DataXの宛先カード

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。
* **[!UICONTROL MDM ID]**：これは[!DNL Yahoo DataX]の一意の識別子であり、この宛先へのデータ書き出しを設定するための必須フィールドです。 このIDがわからない場合は、[!DNL Yahoo DataX] アカウント マネージャーにお問い合わせください。  MDM IDでは、特定の排他的なユーザー（広告主向けのファーストパーティデータなど）に対してのみデータを使用するように制限できます。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![ ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

宛先に対するオーディエンスのアクティブ化の手順については、[宛先に対するプロファイルとオーディエンスのアクティブ化](../../ui/activate-segment-streaming-destinations.md)を参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのようにデータガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。

## その他のリソース {#additional-resources}

詳しくは、[!DNL Yahoo/Verizon Media][の [!DNL DataX] ](https://developer.verizonmedia.com/datax/guide/) ドキュメントを参照してください。
