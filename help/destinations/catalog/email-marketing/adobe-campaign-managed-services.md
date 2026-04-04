---
title: Adobe Campaign Managed Cloud Services 接続
description: Adobe Campaign Managed Cloud Servicesは、クロスチャネルのカスタマーエクスペリエンスを設計するためのプラットフォームと、視覚的なキャンペーンオーケストレーション、リアルタイムのインタラクション管理、クロスチャネルの実行を実現する環境を提供します。
exl-id: fe151ad3-c431-4b5a-b453-9d1d9aedf775
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1721'
ht-degree: 18%

---

# [!DNL Adobe Campaign Managed Cloud Services] 接続 {#adobe-campaign-managed-services}

>[!IMPORTANT]
>
>この統合は、[[!DNL Adobe Campaign]  バージョン 8.4以降](https://experienceleague.adobe.com/docs/campaign/campaign-v8/new/release-notes.html?lang=ja#release-8-4-1)で機能します。

## 概要 {#overview}

[!DNL Adobe Campaign Managed Cloud Services]は、クロスチャネルの顧客体験を設計するためのプラットフォームと、視覚的なキャンペーンのオーケストレーション、リアルタイムのインタラクション管理、クロスチャネルの実行のための環境を提供します。 [ キャンペーンの概要](https://experienceleague.adobe.com/docs/campaign/campaign-v8/start/get-started.html?lang=ja)

Campaign を使用すると、次のことを行えます。

* 顧客の単一のアクセス可能なビューを通じて、パーソナライゼーションとエンゲージメントを促進し，
* メール、モバイル、オンライン、オフラインのチャネルをカスタマージャーニーに統合し，
* 有意義でタイムリーなメッセージやオファーを自動的に配信できます。

## ガードレール {#guardrails}

[!DNL Adobe Campaign Managed Cloud Services]接続を使用する場合は、次のガードレールに注意してください。

* この宛先には、最大25人のオーディエンスを[ アクティブ化](#activate)できます。

  この制限を変更するには、Campaign エクスプローラーの&#x200B;**>** > **[!UICONTROL Administration]** フォルダーの&#x200B;**[!UICONTROL Platform]** NmsCdp_Aep_Audience_List_Limit **[!UICONTROL Options]** オプションの値を更新します。 このガードレールは、設定されたすべての宛先で1つのCampaign インスタンスに書き出すことができるExperience Platform オーディエンスの合計数を制限します。

* 各オーディエンスに対して、最大20個のフィールドを[map](#map)～[!DNL Adobe Campaign]に追加できます。

  この制限を変更するには、Campaign エクスプローラーの&#x200B;**>** > **[!UICONTROL Administration]** フォルダーの&#x200B;**[!UICONTROL Platform]** NmsCdp_Aep_Destinations_Max_Columns **[!UICONTROL Options]** オプションの値を更新します。

* Azure Blob Storage Data Landing Zone （DLZ）でのデータ保持：7日
* アクティベーションの頻度は最低3時間です。
* この接続でサポートされるファイル名の最大長は255文字です。 書き出したファイル名[を](../../ui/activate-batch-profile-destinations.md#configure-file-names)設定する場合は、ファイル名が255文字を超えないようにしてください。 ファイル名の最大長を超えると、アクティベーションエラーが発生します。
* オーディエンスを`&`に書き出す場合、特殊文字（例：[!DNL Adobe Campaign]）を含むセグメント/オーディエンスはサポートされません。

## ユースケース {#use-cases}

[!DNL Adobe Campaign]管理サービスの宛先を使用する方法とタイミングをより理解しやすくするために、[!DNL Adobe Experience Platform]のお客様がこの宛先を使用して解決できる使用例を次に示します。

* [!DNL Adobe Experience Platform]は、ID グラフ、Analyticsからの行動データ、オフラインとオンラインのデータなどの情報を組み込んだ顧客プロファイルを作成します。 この統合を使用すると、既に[!DNL Adobe Campaign]内に存在するセグメント化機能を、それらの[!DNL Adobe Experience Platform]の強化されたオーディエンスで拡張できます。したがって、そのデータをCampaignでアクティブ化できます。

  例えば、スポーツウェア企業が、[!DNL Adobe Experience Platform]の強化されたオーディエンスを活用し、[!DNL Adobe Campaign]を使用してアクティベートし、[!DNL Adobe Campaign]がサポートする様々なチャネルをまたいで顧客層にリーチしたいと考えています。 メッセージが送信されたら、送信、開封数、クリック数など、[!DNL Adobe Experience Platform]のエクスペリエンスデータを使用して、[!DNL Adobe Campaign]の顧客プロファイルを強化します。

  その結果、Adobe Experience Cloudのエコシステム全体をまたいで、より一貫性のあるクロスチャネルキャンペーンと、迅速に適応し学習を進める豊富な顧客プロファイルを実現できます。


* Campaignでのオーディエンスのアクティブ化に加えて、[!DNL Adobe Campaign Managed Services]宛先を活用して、[!DNL Adobe Experience Platform]上のプロファイルに関連付けられ、[!DNL Adobe Campaign] データベースで更新されるように同期プロセスが設定されている追加のプロファイル属性を取り込むことができます。

  例えば、[!DNL Adobe Experience Platform]でオプトインとオプトアウトの値をキャプチャしているとします。 この接続を使用すると、これらの値を[!DNL Adobe Campaign]に取り込み、同期プロセスを設定して、定期的に更新できます。

  >[!NOTE]
  >
  >プロファイル属性の同期は、[!DNL Adobe Campaign] データベースに既に存在するプロファイルに対して使用できます。

[ [!DNL Adobe Campaign]  [!DNL Adobe Experience Platform]との](https://experienceleague.adobe.com/docs/campaign/campaign-v8/connect/ac-aep.html?lang=ja)統合について詳しく見る

## サポートされている ID {#supported-identities}

*[!DNL Adobe Campaign Managed Cloud Services]*&#x200B;は、次の表に示すIDのアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| external_id | カスタムユーザーID | ソース IDがカスタム名前空間である場合は、このターゲット IDを選択します。 このIDを使用して、顧客を表すCampaign インスタンスのID （loyalty_ID、account_ID、customer_ID...）にマッピングすることをお勧めします。 |
| ECID | Experience Cloud ID | ECIDを表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「[!DNL Adobe Experience Cloud] ID」、「[!DNL Adobe Experience Platform] ID」というエイリアスでも参照できます。 詳しくは、[ECID](/help/identity-service/features/ecid.md)の次のドキュメントを参照してください。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | プレーンテキストとSHA256 ハッシュ化された電子メールアドレスの両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | プレーンテキストとSHA256 ハッシュ化された電話番号の両方が[!DNL Adobe Experience Platform]でサポートされています。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、**[!UICONTROL Apply transformation]** オプションをチェックして、[!DNL Experience Platform]がアクティベーション時にデータを自動的にハッシュします。 |
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
| 書き出しタイプ | **[!UICONTROL Profile-based]** | [宛先アクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)の「プロファイル属性を選択」画面で選択した目的のスキーマフィールド（電子メールアドレス、電話番号、姓など）と共に、オーディエンスのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Batch]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
>
>宛先に接続するには、**[!UICONTROL View Destinations]**&#x200B;および&#x200B;**[!UICONTROL Manage Destinations]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

名前、説明、インスタンス選択、ターゲットマッピング、および同期タイプのフィールドを表示する![[!DNL Adobe Campaign Managed Cloud Services]宛先の詳細フォーム。](../../assets/catalog/email-marketing/adobe-campaign-managed-services/destination-details.png)

* **[!UICONTROL Name]**：今後この宛先を認識する際に使用する名前。
* **[!UICONTROL Description]**：今後この宛先を特定するのに役立つ説明です。
* **[!UICONTROL Select instance]**: **[!DNL Campaign]** マーケティングインスタンス。
* **[!UICONTROL Target mapping]**: **[!DNL Adobe Campaign]**&#x200B;で配信の送信に使用しているターゲットマッピングを選択します。 [詳細情報](https://experienceleague.adobe.com/docs/campaign/campaign-v8/profiles-and-audiences/add-profiles/target-mappings.html?lang=ja)
* **[!UICONTROL Select sync type]**：

   * **[!UICONTROL Audience sync]**：このオプションを使用して、[!DNL Adobe Experience Platform]人のオーディエンスを[!DNL Adobe Campaign]に送信します。
   * **[!UICONTROL Profile sync (Update only)]**：このオプションを使用して、[!DNL Adobe Experience Platform]個のプロファイル属性を[!DNL Adobe Campaign]に取り込み、定期的に更新できるように同期プロセスを配置します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UIを使用した宛先アラートの購読](../../ui/alerts.md)に関するガイドを参照してください。

宛先接続の詳細の提供が完了したら、**[!UICONTROL Next]**&#x200B;を選択します。

### ガバナンスポリシーと履行アクション {#governance}

宛先に書き出すデータに適用できるマーケティングアクションを選択します。[!DNL Adobe Campaign]の場合、**[!UICONTROL Email Targeting]** マーケティングアクションを選択することをお勧めします。

マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](/help/data-governance/policies/overview.md)ページを参照してください。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
>
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**&#x200B;および&#x200B;**[!UICONTROL View Segments]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID*&#x200B;をエクスポートするには、**[!UICONTROL View Identity Graph]** [ アクセス制御権限](/help/access-control/home.md#permissions)が必要です。<br> ![ ワークフローで強調表示されているID名前空間を選択して、オーディエンスを宛先にアクティブ化します。](/help/destinations/assets/overview/export-identities-to-destination.png " ワークフローで強調表示されたID名前空間を選択して、オーディエンスを宛先にアクティブ化します。"){width="100" zoomable="yes"}

この宛先に対するオーディエンスデータのアクティブ化の手順については、[ バッチプロファイル書き出し宛先へのオーディエンスデータのアクティブ化](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html?lang=ja)を参照してください。

### 属性と ID のマッピング {#map}

プロファイルと共に書き出すXDM フィールドを選択し、対応する[!DNL Adobe Campaign] フィールドにマッピングします。[電子メールマーケティング宛先のIDと属性の選択について詳しく見る](overview.md)

1. ソースフィールドを選択：

   * **識別子** （例：電子メールフィールド）を、[!DNL Adobe Experience Platform]および[!DNL Adobe Campaign]のプロファイルを一意に識別するソース IDとして選択します。

   * **に書き出す必要があるその他のすべての** XDM ソースプロファイル属性[!DNL Adobe Campaign]を選択します。

   >[!NOTE]
   >
   >「segmentMembershipStatus」フィールドは、segmentMembership ステータスを反映するために必須のマッピングです。 このフィールドはデフォルトで追加され、変更または削除できません。

1. 各フィールドを[!DNL Adobe Campaign]のターゲット フィールドとマッピングします。 使用可能なターゲットフィールドは、[宛先の作成時に選択したターゲットマッピングによって決まります](#destination-details)。

1. 必須の属性と重複排除キーを特定します。 「必須」または「重複排除キー」としてマークされた属性の値はnullにできないことに注意してください。

   * [必須属性](../../ui/activate-batch-profile-destinations.md#mandatory-attributes)は、すべてのプロファイルレコードに選択した属性が含まれていることを確認します。 例：書き出されたすべてのプロファイルに電子メールアドレスが含まれています。 推奨事項は、ID フィールドと重複排除キーとして使用されるフィールドの両方を必須に設定することです。
   * [重複排除キー](../../ui/activate-batch-profile-destinations.md#mandatory-attributes)は、ユーザーがプロファイルを重複排除するIDを決定するプライマリキーです。

     >[!IMPORTANT]
     >
     >重複排除キー属性の名前が、選択したターゲットマッピングの列名と一致していることを確認します。

   ![個のターゲットフィールドにマッピングされたXDM ソースフィールドを示す[!DNL Adobe Campaign]属性マッピング画面。必須および重複排除の主要指標が表示されている](../../assets/catalog/email-marketing/adobe-campaign-managed-services/mapping.png)。

1. マッピングが実行されたら、宛先設定を確認して完了し、**[!DNL Campaign]**へのデータ送信を開始できます。
   [宛先設定を確認して完了する方法について説明します](/help/destinations/destination-types.md#destination-types-and-categories)。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

宛先がアクティブ化されると、Campaignで対応するジョブと書き出されたデータにアクセスできます。

### データ書き出しジョブの監視 {#jobs}

**[!UICONTROL Administration]** > **[!UICONTROL Audit]** > **[!UICONTROL Audience load jobs]** メニューに移動して、[!DNL Adobe Experience Platform]からアクティブ化されたすべての書き出しジョブを監視します。

![[!DNL Adobe Campaign]からアクティブ化された書き出しジョブを表示する[!DNL Adobe Experience Platform] オーディエンス読み込みジョブ画面。](../../assets/catalog/email-marketing/adobe-campaign-managed-services/campaign-jobs.png)

### 書き出されたデータへのアクセス {#data}

**[!UICONTROL Audience sync]**&#x200B;の場合、**[!UICONTROL Profile and target]** > **[!UICONTROL List]** > **[!UICONTROL AEP audiences]** メニューに移動して、書き出されたオーディエンスを確認できます。

![[!DNL Adobe Campaign]件のAEP オーディエンスのリスト ビューで、Experience Platformから書き出されたオーディエンスがプロファイルとターゲットで利用可能であることを示しています。](../../assets/catalog/email-marketing/adobe-campaign-managed-services/campaign-audiences.png)

**[!UICONTROL Profile sync (Update only)]**&#x200B;の場合、データは、宛先でアクティブ化されたオーディエンスがターゲットとする各プロファイルのCampaign データベースに自動的に更新されます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
