---
title: Gainsight PX 接続
description: Gainsight PX 宛先を使用して、セグメント化情報を Gainsight PX プラットフォームに送信します。
last-substantial-update: 2024-02-20T00:00:00Z
exl-id: 0ca0d34f-f866-4f59-80f8-60198fbb86be
source-git-commit: 82ff222d22255b9c99de76111d25d4a3cf6f2d5c
workflow-type: tm+mt
source-wordcount: '988'
ht-degree: 24%

---

# Gainsight PX 接続 {#gainsight-px}

## 概要 {#overview}

[[!DNL Gainsight PX]](https://www.gainsight.com/product-experience/) は、ユーザーが製品をどのように使用しているかを製品チームが理解できるようにする製品エクスペリエンスプラットフォームです。フィードバックを収集し、製品のウォークスルーなどのアプリ内エンゲージメントを作成して、ユーザーのオンボーディングと製品採用を促進します。

>[!IMPORTANT]
>
>宛先コネクタおよびドキュメントページは、*Gainsight PX* チームによって作成および管理されます。 お問い合わせや更新のリクエストについては、*`pxsupport@gainsight.com`* まで直接ご連絡ください。

## ユースケース {#use-cases}

*Gainsight PX* 宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるサンプルユースケースを以下に示します。

### アプリ内エンゲージメントのターゲティング {#targeting-in-app-engagements}

ある SaaS 企業は、Gainsight PX 上に構築されたアプリケーション内ガイドを通じて顧客とのエンゲージメントを図りたいと考えています。 このエンゲージメントを受け取るオーディエンスは、Adobe Experience Platformに基づいて作成されました。 Gainsight PX 宛先はオーディエンスを受け取り、Gainsight PX 環境内で利用できるようにします。

## 前提条件 {#prerequisites}

* [!DNL Gainsight] サポートチームに連絡し、サブスクリプションの外部セグメント機能のアクティベーションをリクエストします。
* **[!UICONTROL Generate New Secret]** 会社の詳細ページ [&#x200B; の下部にある「](https://app.aptrinsic.com/settings/subscription)」ボタンを使用して、PX サブスクリプションの OAuth シークレット値を生成します
  ![Gainsight PX の「新しいシークレットの生成」ボタンを示す会社の詳細画面 &#x200B;](../../assets/catalog/analytics/gainsight-px/generate_oauth_secret.png)

## サポートされている ID {#supported-identities}

Gainsight PX は、以下の表に示す ID のアクティブ化をサポートしています。 [ID](../../../identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 |
|---|----|
| IdentifyID | Gainsight PX およびAdobe Experience Platformでユーザーを一意に識別する共通ユーザー ID |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの接触チャネル | × | このカテゴリには、[!DNL Segmentation Service] を通じて生成されたオーディエンス以外のすべてのオーディエンスの接触チャネルが含まれます。 [&#x200B; 様々なオーディエンスのオリジン &#x200B;](/help/segmentation/ui/audience-portal.md#customize) について確認する。 次に例を示します。 <ul><li> csv ファイルからExperience Platformへのカスタムアップロードオーディエンス [&#x200B; 読み込み &#x200B;](../../../segmentation/ui/audience-portal.md#import-audience)</li><li> 類似オーディエンス、 </li><li> 連合オーディエンス、 </li><li> Adobe Journey Optimizerなど、他のExperience Platform アプリで生成されたオーディエンス。 </li><li> その他。 </li></ul> |

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
|---|---|---|
| 書き出しタイプ | **[!UICONTROL Segment export]** | [!DNL Gainsight PX] 宛先で使用される識別子（氏名、電話番号など）を使用して、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。オーディエンスの評価に基づいてExperience Platform内でプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL Manage Destinations]** アクセス制御権限 [&#x200B; が必要 &#x200B;](/help/access-control/home.md#permissions) す。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対する認証を行うには、必須フィールドに入力し、「**[!UICONTROL Connect to destination]**」を選択します。

![&#x200B; 認証スクリーンショット &#x200B;](../../assets/catalog/analytics/gainsight-px/auth-screen.png)

* **[!UICONTROL Password]**: [[!DNL Gainsight PX]](https://app.aptrinsic.com) へのログインに使用するパスワード
* **[!UICONTROL Client ID]**:[&#x200B; 会社詳細ページ &#x200B;](https://app.aptrinsic.com/settings/subscription) の Gainsight PX サブスクリプション ID
* **[!UICONTROL Client secret]**:[&#x200B; UI の &#x200B;](https://app.aptrinsic.com/settings/subscription) 会社の詳細ページ [!DNL Gainsight PX] の下部で生成される OAuth 秘密鍵。
* **[!UICONTROL Username]**:[[!DNL Gainsight PX]](https://app.aptrinsic.com) UI へのログインに使用するメール

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![&#x200B; 名前および説明フィールドへの入力方法を示す、Experience Platform ユーザーインターフェイスの宛先の詳細画面 &#x200B;](../../assets/catalog/analytics/gainsight-px/destination_details.png)

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL Manage Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスセグメントをアクティベートする手順は、[ストリーミングセグメントの書き出し宛先へのプロファイルとセグメントのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### ID のマッピング {#map}

この宛先は、プロファイル属性と ID 名前空間のマッピングをサポートしています。 ターゲットマッピングは、常に **[!UICONTROL IDENTIFY_ID]** ID 名前空間である必要があります。

マッピングの設定方法について詳しくは、以下の例を参照してください。

#### プロファイル属性のマッピング {#map-profile-attribute}

以下に示す例では、ソースフィールドは、ターゲット名前空間 IDENTIFY_ID にマッピングされる XDM プロファイル属性です。

![&#x200B; ソース値とターゲット値の選択方法を示す ID 名前空間のサンプルマッピング画面 &#x200B;](../../assets/catalog/analytics/gainsight-px/mapping_attribute.png)

#### ID 名前空間のマッピング {#map-identity-namespace}

以下に示す例では、ソースフィールドは ID 名前空間（**[!UICONTROL ECID]**）であり、**[!UICONTROL IDENTIFY_ID]** ターゲット名前空間にマッピングされます。

![&#x200B; ソース値とターゲット値の選択方法を示すマッピング画面の属性例 &#x200B;](../../assets/catalog/analytics/gainsight-px/mapping_identities.png)

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

セグメントデータはExperience Platformから Gainsight PX にストリーミングされます。

セグメントメタデータは、[!DNL Gainsight PX] UI のセグメント画面に表示されます。

![Gainsight PX のセグメントリスト画面に外部セグメントが表示されています。](../../assets/catalog/analytics/gainsight-px/segment_metadata.png)

セグメントメンバーシップ情報は、[!DNL Gainsight PX] UI のオーディエンスエクスプローラー画面の「セグメント」タブに表示されます。

![Gainsight PX のオーディエンスエクスプローラー画面。ユーザーの関連セグメントが表示されています。](../../assets/catalog/analytics/gainsight-px/PX_Segments.png)

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
