---
title: PubMatic 接続
description: PubMatic は、将来のプログラムによるデジタルマーケティングsupply chainを提供することで、お客様の価値を最大化します。 PubMatic Connect は、プラットフォーム技術と専用サービスを組み合わせて、在庫とデータのパッケージ化および取引方法を強化します。
last-substantial-update: 2025-02-12T00:00:00Z
exl-id: 21e07d2c-9a6a-4cfa-a4b8-7ca48613956c
source-git-commit: 82ff222d22255b9c99de76111d25d4a3cf6f2d5c
workflow-type: tm+mt
source-wordcount: '1151'
ht-degree: 27%

---


# PubMatic Connect の宛先 {#pubmatic-connect}

## 概要 {#overview}

[!DNL PubMatic Connect] を使用すると、将来のプログラムによるデジタルマーケティングsupply chainを提供することで、顧客価値を最大化できます。 [!DNL PubMatic Connect] は、プラットフォームテクノロジーと専用のサービスを組み合わせて、在庫とデータのパッケージ化および取引方法を強化します。

オーディエンスデータを PubMatic Connect プラットフォームに送信できる宛先は 2 つあります。 これらの機能は少し異なります。

1. PubMatic 接続

   最初のアクティベーション時に、この宛先は PubMatic プラットフォームにオーディエンスを自動的に登録し、マッピングに内部Adobe Experience Platform ID を使用します。

2. PubMatic Connect （カスタムオーディエンス ID マッピング）

   この宛先を使用すると、アクティベーションワークフロー中に、マッピング ID を手動で追加することを選択できます。 データを PubMatic プラットフォームの既存のオーディエンスに送信する必要がある場合や、カスタムの「Source オーディエンス ID」が必要な場合に、この宛先を使用します。

![ 宛先カタログ内の 2 つの PubMatic コネクタを並べて表示します。](/help/destinations/assets/catalog/advertising/pubmatic/two-pubmatic-connectors-side-by-side.png)

>[!IMPORTANT]
>
> 宛先コネクタとドキュメントページは、[!DNL PubMatic] チームが作成および管理します。 お問い合わせや更新のリクエストについては、`support@pubmatic.com` まで直接ご連絡ください。

## ユースケース {#use-cases}

[!DNL PubMatic Connect] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

### モバイル、web および CTV プラットフォームでのユーザーのターゲティング {#targeting}

パブリッシャーやデータプロバイダーは、幅広い識別子を使用して、オーディエンスをAdobe Experience Platformから [!DNL PubMatic Connect] に送信し、モバイル、web、CTV プラットフォームのユーザーをターゲットにしたいと考えています。

## 前提条件 {#prerequisites}

アカウントが正しく設定され、オーディエンスセグメントのオンボーディングをサポートしていることを確認するには、[!DNL PubMatic] アカウントマネージャーにお問い合わせください。 また、この宛先を使用するための関連する詳細がすべて用意され、設定中にサポートが提供されます。

## サポートされている ID {#supported-identities}

[!DNL PubMatic Connect] では、以下の表で説明する ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
| --------------- | ------------------------ | ------------------------------------------------------------------------------- |
| GAID | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| extern_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
|---------|----------|----------|
| [!DNL Segmentation Service] | ○ | Experience Platform [ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| その他すべてのオーディエンスの接触チャネル | × | このカテゴリには、[!DNL Segmentation Service] を通じて生成されたオーディエンス以外のすべてのオーディエンスの接触チャネルが含まれます。 [ 様々なオーディエンスのオリジン ](/help/segmentation/ui/audience-portal.md#customize) について確認する。 次に例を示します。 <ul><li> csv ファイルからExperience Platformへのカスタムアップロードオーディエンス [ 読み込み ](../../../segmentation/ui/audience-portal.md#import-audience)</li><li> 類似オーディエンス、 </li><li> 連合オーディエンス、 </li><li> Adobe Journey Optimizerなど、他のExperience Platform アプリで生成されたオーディエンス。 </li><li> その他。 </li></ul> |

{style="table-layout:auto"}



オーディエンスデータタイプでサポートされるオーディエンス：

| オーディエンスデータタイプ | サポートあり | 説明 | ユースケース |
|--------------------|-----------|-------------|-----------|
| [ 人物オーディエンス ](/help/segmentation/types/people-audiences.md) | ○ | 顧客プロファイルに基づき、マーケティングキャンペーンの対象となる人物のグループを指定できます。 | 頻繁な購入、買い物かごの放棄 |
| [ アカウントオーディエンス ](/help/segmentation/types/account-audiences.md) | × | アカウントベースのマーケティング戦略では、特定の組織内の個人をターゲットに設定します。 | B2B マーケティング |
| [ 見込み客オーディエンス ](/help/segmentation/types/prospect-audiences.md) | × | まだ顧客ではないものの、ターゲットオーディエンスと特性を共有する個人をターゲットに設定します。 | サードパーティデータを使用した予測 |
| [ データセットの書き出し ](/help/catalog/datasets/overview.md) | × | Adobe Experience Platform Data Lake に保存された構造化データのコレクション。 | レポート、データサイエンスワークフロー |

{style="table-layout:auto"}


## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
| ---------------- | ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 書き出しタイプ | **[!UICONTROL Segment export]** | PubMatic Connect 宛先で使用される識別子（名前、電話番号など）を使用して、セグメント（オーディエンス）のすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Streaming]** | ストリーミングの宛先は常に、API ベースの接続です。セグメント評価に基づいてExperience Platform内でプロファイルが更新されると、コネクタは更新を宛先プラットフォームに送信します。 詳しくは、[ストリーミングの宛先](/help/destinations/destination-types.md#streaming-destinations)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
> 宛先に接続するには、**[!UICONTROL Manage Destinations]** アクセス制御権限 [ が必要 ](/help/access-control/home.md#permissions) す。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対する認証を行うには、必須フィールドに入力し、「**[!UICONTROL Connect to destination]**」を選択します。

![ 認証方法 ](../../assets/catalog/advertising/pubmatic/authenticate-destination.png)

- **[!UICONTROL Bearer token]**：宛先を認証するためのベアラートークンを入力します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![ 宛先の詳細 ](../../assets/catalog/advertising/pubmatic/destination-details.png)

- **[!UICONTROL Name]**：今後この宛先を認識するための名前。
- **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
- **[!UICONTROL Data partner ID]**：この統合用に [!DNL PubMatic] アカウントで設定されたデータパートナー ID。
- **[!UICONTROL Default country code]**: プロファイルで指定されていない場合にすべての ID に適用されるデフォルトの国コード。
- **[!UICONTROL Account ID]**:[!DNL PubMatic Connect] アカウント ID。
- **[!UICONTROL Account type]**:[!DNL PubMatic] Platform アカウントのアカウントタイプ。 ご質問がある場合は、[!DNL PubMatic] のアカウントマネージャーにお問い合わせください。 使用できる選択肢は次のとおりです。
   - [!UICONTROL PUBLISHER]
   - [!UICONTROL DEMAND_PARTNER]
   - [!UICONTROL BUYER]

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
>
> - データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>
> - _ID_ を書き出すには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](../../assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスセグメントをアクティベートする手順は、[ストリーミングセグメントの書き出し宛先へのプロファイルとセグメントのアクティベート](/help/destinations/ui/activate-segment-streaming-destinations.md)を参照してください。

### 属性と ID のマッピング {#map}

ソースフィールドを選択中：

- 識別子（通常は IDFA やカスタム ID 名前空間などの名前空間）を選択します。

ターゲットフィールドを選択：

- この手順で正しいUIDの種類については、[!DNL PubMatic] 担当営業または販売店にお問い合わせください。
- 最初の手順で選択した識別子に一致する [!DNL PubMatic UID] タイプ番号を選択します。

![ 属性と ID のマッピング ](../..//assets/catalog/advertising/pubmatic/export-identities-to-destination.png)

### オーディエンスのスケジュール

PubMatic Connect （カスタムオーディエンス ID マッピング）宛先を使用する場合、PubMatic プラットフォームの「Source オーディエンス ID」に対応する各オーディエンスのマッピング ID を指定する必要があります。

![ オーディエンスのスケジュール設定 ](../..//assets/catalog/advertising/pubmatic/audience-scheduling-mapping-id.png)

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

[!DNL PubMatic] UI を使用すると、データが正しくプッシュされているかどうか、およびセグメントが使用可能かどうかを確認できます。 [!DNL PubMatic] UI の更新のためにデータがプッシュされてから最大 24 時間かかる場合があります。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
