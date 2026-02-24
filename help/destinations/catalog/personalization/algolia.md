---
title: アルゴリア
description: このコネクタを使用して、オーディエンスをアルゴリアに対してアクティブ化し、検索やレコメンデーション全体でパーソナライゼーションと使用を行います。 その後、Algolia ユーザープロファイルソースコネクタを使用して、プロファイルをReal-Time CDPに読み込み、豊富なオーディエンスを構築できます。
exl-id: 116a051a-1b47-4789-826e-c8f0fee60def
source-git-commit: 82ff222d22255b9c99de76111d25d4a3cf6f2d5c
workflow-type: tm+mt
source-wordcount: '1140'
ht-degree: 30%

---

# [!DNL Algolia] 接続

## 概要 {#overview}

>[!IMPORTANT]
>
>[!DNL Algolia] の宛先コネクタとドキュメントページは、Algolia Integration Services チームによって作成および管理されます。 お問い合わせや更新のリクエストについては、[adobe-algolia-solutions@algolia.com](mailto:adobe-algolia-solutions@algolia.com) までお問い合わせください。

[!DNL Algolia] の宛先接続を使用すると、Adobe Experience Platform オーディエンスをアルゴリアに送信して、パーソナライズされた検索やお勧めを受け取ることができます。 [!DNL Algolia] の宛先コネクタを使用する前に、まず [[!DNL Algolia User Profiles]](/help/sources/connectors/data-partners/algolia-user-profiles.md) ソースコネクタを設定する必要があります。 ソースコネクタの設定チュートリアルでは、Algolia ユーザートークン ID を作成します。 この ID は、宛先コネクタを設定する際にマッピングに必要です。

このチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用して、[!DNL Algolia] しい宛先接続とデータフローを作成する手順を説明します。

![&#x200B; アルゴリアの宛先を含む宛先カタログ &#x200B;](../../assets/catalog/personalization/algolia/catalog.png)

## ユースケース {#use-cases}

[!DNL Algolia] の宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### Personalization整合性 {#personalization-consistency}

この宛先コネクタを使用すると、ホームページから検索まで、サイト全体で一貫したパーソナライゼーションを提供できます。

例えば、マーケターが、アルゴリアを含む複数のユーザーデータソースから、Adobe Experience Platformで豊富なオーディエンスを構築するとします。 [!DNL Algolia] の宛先コネクタを使用して、ターゲティング戦略のオーディエンスを共有し、キャンペーンのパーソナライゼーションとコンバージョンの向上につなげることができます。

このユースケースを実装するには、[[!DNL Algolia User Profiles]](/help/sources/connectors/data-partners/algolia-user-profiles.md) ソースコネクタと [!DNL Algolia] 宛先コネクタの両方を使用する必要があります。

まず、既存の [!DNL Algolia] ユーザープロファイルをAdobe Experience Platform Real-Time CDPおよびその他のソースに読み込み、ソースコネクタを使用してリッチオーディエンスの作成を開始します。 マーケターは、検索とレコメンデーションのパーソナライゼーションのためにアルゴリアに送信できるプロファイルデータを使用して、オーディエンスを作成します。

次に、対応する [[!DNL Algolia User Profiles]](/help/sources/connectors/data-partners/algolia-user-profiles.md) ソースコネクタを使用して、顧客プロファイルをReal-Time CDPに取り込んで拡張します。

## 前提条件 {#prerequisites}

>[!IMPORTANT]
>
>* 宛先に接続するには、**[!UICONTROL View Destinations]** と **[!UICONTROL Manage Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

## サポートされている ID {#supported-identities}

[!DNL Algolia] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](https://experienceleague.adobe.com/ja/docs/experience-platform/identity/features/namespaces) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---------|---------|----------|
| userId | [!DNL Algolia] ユーザートークン | このターゲット ID を選択して、`AlgoliaUserToken` ソース ID を `userToken` プラットフォームの [!DNL Algolia] にマッピングします。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|---------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの接触チャネル | ○ | このカテゴリには、[!DNL Segmentation Service] を通じて生成されたオーディエンス以外のすべてのオーディエンスの接触チャネルが含まれます。 [&#x200B; 様々なオーディエンスのオリジン &#x200B;](/help/segmentation/ui/audience-portal.md#customize) について確認する。 次に例を示します。 <ul><li> csv ファイルからExperience Platformへのカスタムアップロードオーディエンス [&#x200B; 読み込み &#x200B;](../../../segmentation/ui/audience-portal.md#import-audience)</li><li> 類似オーディエンス、 </li><li> 連合オーディエンス、 </li><li> Adobe Journey Optimizerなど、他のExperience Platform アプリで生成されたオーディエンス。 </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスデータタイプでサポートされるオーディエンス：

| オーディエンスデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [&#x200B; 人物オーディエンス &#x200B;](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルに基づき、マーケティングキャンペーンの対象となる人物のグループを指定できます。 | 頻繁な購入、買い物かごの放棄 |
| [&#x200B; アカウントオーディエンス &#x200B;](/help/segmentation/types/account-audiences.md) | × | アカウントベースのマーケティング戦略では、特定の組織内の個人をターゲットに設定します。 | B2B マーケティング |
| [&#x200B; 見込み客オーディエンス &#x200B;](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないものの、ターゲットオーディエンスと特性を共有する個人をターゲットに設定します。 | サードパーティデータを使用した予測 |
| [&#x200B; データセットの書き出し &#x200B;](/help/catalog/datasets/overview.md) | × | Adobe Experience Platform Data Lake に保存された構造化データのコレクション。 | レポート、データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!DNL Audience export]** | [!DNL Algolia] 宛先で使用される識別子（氏名、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンス評価に基づいて Experience Platform 内でプロファイルが更新されるとすぐに、コネクタは更新を宛先プラットフォームに送信します。詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage and Activate Dataset Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対する認証を行うには、必須フィールドに入力し、「**[!UICONTROL Connect to destination]**」を選択します。

* **[!UICONTROL Application ID]**: [!DNL Algolia] アプリケーション ID は、[!DNL Algolia] アカウントに割り当てられた一意の ID です。
* **[!UICONTROL API Key]**:[!DNL Algolia] API キーは、[!DNL Algolia] の検索およびインデックス作成サービスに対する API リクエストの認証および承認に使用される資格情報です。

これらの資格情報について詳しくは、[!DNL Algolia] [&#x200B; 認証ドキュメント &#x200B;](https://www.algolia.com/doc/tools/cli/get-started/authentication/) を参照してください。

![&#x200B; 新規アカウント &#x200B;](../../assets/catalog/personalization/algolia/connection.png)

### 宛先の詳細を入力

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL Name]**：この宛先に希望する名前を入力します。
* **[!UICONTROL Description]**：宛先の目的の短い説明。
* **[!UICONTROL Region]**：オプションは **米国** または **EU** です。 顧客データが保存される地域を選択します。


![アカウントの詳細](../../assets/catalog/personalization/algolia/account.png)

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* ID を書き出すには、ID グラフの表示 [&#x200B; アクセス制御権限 &#x200B;](https://experienceleague.adobe.com/ja/docs/experience-platform/access-control/home#permissions) が必要です。

この宛先にオーディエンスをアクティベートする手順は、[ストリーミングオーディエンスの書き出し宛先へのプロファイルとオーディエンスのアクティベート](https://experienceleague.adobe.com/ja/docs/experience-platform/destinations/ui/activate/activate-segment-streaming-destinations)を参照してください。

### 属性と ID のマッピング {#mapping-attributes-identities}

[!UICONTROL Mapping step] の間、AlgoliaUserToken ソース ID を userId ターゲット ID にマッピングする必要があります。

![マッピング完了](../../assets/catalog/personalization/algolia/mapping-complete.png)

## データの書き出しを検証する {#exported-data}

オーディエンスがユーザープロファイルに正常に書き出されたかどうかを確認するには、[!DNL Algolia] ダッシュボードを確認し、**[!UICONTROL Advanced Personalization]** に移動して「**[!UICONTROL User Inspector]**」をクリックします。 書き出されたAdobe Experience Platform オーディエンスに関連付けられているユーザープロファイルを見つけ、ユーザーインスペクターで検索します。 「セグメント」セクションにオーディエンス ID が表示されます。

![Algolia User Inspector](../../assets/catalog/personalization/algolia/verify-segment-user-profile.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](https://experienceleague.adobe.com/docs/experience-platform/data-governance/home.html?lang=ja)を参照してください。

## その他のリソース {#additional-resources}

詳しくは、次の [!DNL Algolia] ドキュメントを参照してください。

* [&#x200B; 高度なPersonalizationとは &#x200B;](https://www.algolia.com/doc/guides/personalization/advanced-personalization/what-is-advanced-personalization/)
* [&#x200B; ユーザープロファイル &#x200B;](https://www.algolia.com/doc/guides/personalization/advanced-personalization/what-is-advanced-personalization/concepts/user-profiles/)
* [&#x200B; ルールコンテキストを持つセグメントユーザー &#x200B;](https://www.algolia.com/doc/guides/personalization/advanced-personalization/implement/guides/segment-users-with-rule-contexts/#assign-a-segment-context-at-query-time)

## 次の手順 {#next-steps}

このチュートリアルでは、Experience Platformから [!DNL Algolia] アプリケーションにオーディエンスを書き出すデータフローを正常に作成しました。 [!DNL Algolia] プラットフォームについて詳しくは、[Algolia のドキュメント &#x200B;](https://www.algolia.com/doc/) を参照してください。
