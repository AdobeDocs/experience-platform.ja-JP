---
title: Adobe Campaign Managed Cloud Services 接続
description: Adobe Campaign Managed Cloud Services は、クロスチャネルのカスタマーエクスペリエンスを設計するためのプラットフォームと、視覚的なキャンペーンオーケストレーション、リアルタイムインタラクション管理、クロスチャネル実行のための環境を提供します。
exl-id: fe151ad3-c431-4b5a-b453-9d1d9aedf775
source-git-commit: e300e57df998836a8c388511b446e90499185705
workflow-type: tm+mt
source-wordcount: '1582'
ht-degree: 41%

---

# Adobe Campaign Managed Cloud Services 接続 {#adobe-campaign-managed-services}

>[!IMPORTANT]
>
>この統合は、 [Adobe Campaignバージョン 8.4 以降](https://experienceleague.adobe.com/docs/campaign/campaign-v8/new/release-notes.html#release-8-4-1).

## 概要 {#overview}

Adobe Campaign Managed Cloud Services は、クロスチャネルのカスタマーエクスペリエンスを設計するためのプラットフォームと、視覚的なキャンペーンオーケストレーション、リアルタイムインタラクション管理、クロスチャネル実行のための環境を提供します。[Campaign の概要](https://experienceleague.adobe.com/docs/campaign/campaign-v8/start/get-started.html?lang=ja)

Campaign を使用すると、次のことを行えます。
* 顧客の単一のアクセス可能なビューを通じて、パーソナライズとエンゲージメントを推進,
* メール、モバイル、オンライン、オフラインの各チャネルをカスタマージャーニーに統合,
* 意味のあるタイムリーなメッセージやオファーの配信を自動化.

>[!IMPORTANT]
>
>Adobe Campaign Managed Cloud Services接続を使用する際は、次のガードレールに注意してください。
>
>* 最大 50 個のセグメントを指定できます [有効化済み](#activate) 宛先の場合、
>* 各セグメントについて、最大 20 個のフィールドを [マップ](#map) Adobe Campaignへ
>* Azure Blob ストレージデータランディングゾーン (DLZ) でのデータ保持：7 日
>* 有効化の頻度は 3 時間以上です。

## ユースケース {#use-cases}

Adobe Campaign Manage Service の宛先をいつどのように使用するかを理解しやすくするために、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

* Adobe Experience Platformは、ID グラフ、分析の行動データ、オフラインとオンラインのデータなどの情報を組み込んだ顧客プロファイルを作成します。 この統合を使用すると、Adobe Experience Platformを利用したオーディエンスを使用して、Adobe Campaign内に既に存在するセグメント化機能を拡張できます。その結果、Campaign でそのデータをアクティブ化できます。

  例えば、スポーツフォース会社は、Adobe Experience Platformを活用したスマートセグメントを活用し、Adobe Campaignを使用してそれらをアクティブ化して、Adobe Campaignがサポートする様々なチャネルをまたいで顧客ベースにリーチしたいと考えています。 メッセージが送信されたら、送信、開封、クリックなどのAdobe Campaignからのエクスペリエンスデータを使用して、AdobeExperience Platform の顧客プロファイルを強化したいと考えます。

  その結果、Adobeの Experience Cloud エコシステム全体で一貫性の高いクロスチャネルキャンペーンと、迅速な適応および学習をおこなう豊富な顧客プロファイルが実現します。


* Campaign のセグメントのアクティベーションに加えて、Adobe Campaign Managed Servicesの宛先を活用して、Adobe Experience Platform上のプロファイルに関連付けられ、同期プロセスを実行している追加のプロファイル属性を取り込み、Adobe Campaignデータベースで更新できます。

  例えば、Adobe Experience Platform でオプトインとオプトアウトの値を取り込んでいるとします。この接続を使用すると、これらの値を Adobe Campaign に取り込み、定期的に更新されるように同期プロセスを導入できます。

  >[!NOTE]
  >
  >プロファイル属性の同期は、Adobe Campaignデータベースに既に存在するプロファイルで使用できます。

[Adobe Experience PlatformとのAdobe Campaign統合の詳細](https://experienceleague.adobe.com/docs/campaign/campaign-v8/connect/ac-aep.html?lang=ja)

## サポートされている ID {#supported-identities}

*Adobe Campaign Managed Cloud Services* では、以下の表で説明する id のアクティブ化をサポートしています。 [ID](/help/identity-service/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| external_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 この ID を使用し、顧客 (loyalty_ID、account_ID、customer_ID...) を表す Campaign インスタンス内の ID にマッピングすることをお勧めします。 |
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。次のドキュメントを参照してください： [ECID](/help/identity-service/ecid.md) を参照してください。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| GAID | Google Advertising ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![](../../assets/catalog/email-marketing/adobe-campaign-managed-services/destination-details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL インスタンスを選択]**: **[!DNL Campaign]** マーケティングインスタンス。
* **[!UICONTROL ターゲットマッピング]**：で使用しているターゲットマッピングを選択します。 **[!DNL Adobe Campaign]** をクリックして配信を送信します。 [詳細情報](https://experienceleague.adobe.com/docs/campaign/campaign-v8/profiles-and-audiences/add-profiles/target-mappings.html)。
* **[!UICONTROL 同期タイプを選択]**:

   * **[!UICONTROL オーディエンスの同期]**：このオプションを使用して、Adobe Experience PlatformオーディエンスをAdobe Campaignに送信します。
   * **[!UICONTROL プロファイル同期（更新のみ）]**：このオプションを使用して、Adobe Experience Platformプロファイル属性をAdobe Campaignに取り込み、同期プロセスをおこない、定期的に更新できるようにします。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

### ガバナンスポリシーと実施アクション {#governance}

宛先に書き出すデータに適用できるマーケティングアクションを選択します。Adobe Campaignの場合、 **[!UICONTROL E メールのターゲティング]** マーケティングアクション。

マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](/help/data-governance/policies/overview.md)ページを参照してください。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。
>* 書き出す *id*、 **[!UICONTROL ID グラフを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). <br> ![ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png "ワークフローでハイライト表示された ID 名前空間を選択して、宛先に対するオーディエンスをアクティブ化します。"){width="100" zoomable="yes"}

読み取り [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html?lang=ja) を参照してください。

### 属性と ID のマッピング {#map}

プロファイルと共に書き出す XDM フィールドを選択し、対応するAdobe Campaignフィールドにマッピングします。[電子メールマーケティングの宛先の ID と属性の選択の詳細を説明します](overview.md)

1. ソースフィールドを選択：

   * を選択します。 **識別子** （例：E メールフィールド）Adobe Experience PlatformとAdobe Campaignでプロファイルを一意に識別するソース ID。

   * その他すべてを選択 **XDM ソースプロファイル属性** Adobe Campaignに書き出す必要がある

   >[!NOTE]
   >
   >「segmentMembershipStatus」フィールドは、segmentMembership ステータスを反映するために必須のマッピングです。 このフィールドはデフォルトで追加され、変更または削除できません。

1. Adobe Campaignで、各フィールドをそのターゲットフィールドにマッピングします。 使用可能なターゲットフィールドは、 [宛先の作成](#destination-details).

1. 必須の属性と重複排除キーを特定します。 「必須」または「重複排除キー」としてマークされた属性の値を null にすることはできません。

   * [必須の属性](../../ui/activate-batch-profile-destinations.md#mandatory-attributes) 選択した属性がすべてのプロファイルレコードに含まれていることを確認します。 例：エクスポートされるすべてのプロファイルには、E メールアドレスが含まれています。 重複排除キーとして使用する ID フィールドとフィールドの両方を必須に設定することをお勧めします。
   * [重複排除キー](../../ui/activate-batch-profile-destinations.md#mandatory-attributes) は、ユーザーがプロファイルの重複排除を希望する ID を決定するプライマリキーです。

     >[!IMPORTANT]
     >
     >重複排除キー属性の名前が、選択したターゲットマッピングの列名と一致していることを確認してください。

   ![](../../assets/catalog/email-marketing/adobe-campaign-managed-services/mapping.png)

1. マッピングが実行されたら、宛先設定を確認して完了し、へのデータの送信を開始できます。 **[!DNL Campaign]**.
   [宛先の設定を確認して完了する方法を説明します。](/help/destinations/destination-types.md#review).

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

宛先がアクティブ化されると、Campaign で対応するジョブおよびエクスポートしたデータにアクセスできます。

### データ書き出しジョブの監視 {#jobs}

次に移動： **[!UICONTROL 管理]** > **[!UICONTROL 監査]** > **[!UICONTROL オーディエンス読み込みジョブ]** メニューを使用して、Adobe Experience Platformからアクティブ化されたすべての書き出しジョブを監視できます。

![](../../assets/catalog/email-marketing/adobe-campaign-managed-services/campaign-jobs.png)

### 書き出されたデータにアクセス {#data}

の場合 **[!UICONTROL オーディエンスの同期]**&#x200B;に含まれている場合は、 **[!UICONTROL プロファイルとターゲット]** > **[!UICONTROL リスト]** > **[!UICONTROL AEP オーディエンス]** メニュー。

![](../../assets/catalog/email-marketing/adobe-campaign-managed-services/campaign-audiences.png)

の場合 **[!UICONTROL プロファイル同期（更新のみ）]**&#x200B;の場合、宛先でアクティブ化されたセグメントのターゲットとなる各プロファイルのデータが自動的に Campaign データベースに更新されます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
