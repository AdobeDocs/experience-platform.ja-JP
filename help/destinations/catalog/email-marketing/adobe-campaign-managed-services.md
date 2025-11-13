---
title: Adobe Campaign Managed Cloud Services 接続
description: Adobe Campaign Managed Cloud Servicesは、クロスチャネルのカスタマーエクスペリエンスを設計するためのプラットフォームと、視覚的なキャンペーンオーケストレーション、リアルタイムインタラクション管理およびクロスチャネル実行のための環境を提供します。
exl-id: fe151ad3-c431-4b5a-b453-9d1d9aedf775
source-git-commit: f0db626401d76997e19632c3e27a133f577bc571
workflow-type: tm+mt
source-wordcount: '1610'
ht-degree: 26%

---

# Adobe Campaign Managed Cloud Services 接続 {#adobe-campaign-managed-services}

>[!IMPORTANT]
>
>この統合は、[Adobe Campaign バージョン 8.4 以降 &#x200B;](https://experienceleague.adobe.com/docs/campaign/campaign-v8/new/release-notes.html#release-8-4-1) で機能します。

## 概要 {#overview}

Adobe Campaign Managed Cloud Services は、クロスチャネルのカスタマーエクスペリエンスを設計するためのプラットフォームと、視覚的なキャンペーンオーケストレーション、リアルタイムインタラクション管理、クロスチャネル実行のための環境を提供します。[Campaign の概要 &#x200B;](https://experienceleague.adobe.com/docs/campaign/campaign-v8/start/get-started.html?lang=ja)

Campaign を使用すると、次のことが可能になります。

* 顧客に関する情報をアクセス可能な単一のビューにまとめ、パーソナライズとエンゲージメントを推進
* メール、モバイル、オンライン、オフラインの各チャネルをカスタマージャーニーに統合
* 有意義でタイムリーなメッセージやオファーの配信を自動化。

## ガードレール {#guardrails}

Adobe Campaign Managed Cloud Services接続を使用する際は、次のガードレールに注意してください。

* この宛先に対して最大 25 個のオーディエンスを [&#x200B; アクティブ化 &#x200B;](#activate) できます。

  この制限を変更するには、Campaign エクスプローラーの **>** > **[!UICONTROL Administration]** フォルダーで **[!UICONTROL Platform]** NmsCdp_Aep_Audience_List_Limit **[!UICONTROL Options]** オプションの値を更新します。 このガードレールでは、設定済みのすべての宛先にわたって 1 つのExperience Platform インスタンスに書き出すことができる Campaign オーディエンスの合計数が制限されます。

* オーディエンスごとに、最大 20 個のフィールドをAdobe Campaignに [&#x200B; マッピング &#x200B;](#map) できます。

  この制限を変更するには、Campaign エクスプローラーの **>** > **[!UICONTROL Administration]** フォルダーにある **[!UICONTROL Platform]** NmsCdp_Aep_Destinations_Max_Columns **[!UICONTROL Options]** オプションの値を更新します。

* Azure Blob Storage Data Landing Zone （DLZ）のデータ保持：7 日。
* アクティブ化の頻度は最低 3 時間です。
* この接続でサポートされるファイル名の最大長は 255 文字です。 [&#x200B; 書き出すファイル名を設定 &#x200B;](../../ui/activate-batch-profile-destinations.md#configure-file-names) する場合は、ファイル名が 255 文字を超えないようにしてください。 ファイル名の最大長を超えると、アクティベーションエラーが発生します。
* 特殊文字（例：`&`）を含むセグメント/オーディエンスは、オーディエンスをAdobe Campaignに書き出す場合はサポートされません。

## ユースケース {#use-cases}

Adobe Campaign Manage Service 宛先を使用する方法とタイミングをより深く理解するために、Adobe Experience Platformのお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

* Adobe Experience Platformは、ID グラフや analytics の行動データなどの情報を組み込み、オフラインのデータとオンラインのデータを結合する顧客プロファイルを作成します。 この統合により、Adobe Experience Platformを利用したオーディエンスでAdobe Campaign内に既に存在するセグメント化機能を強化し、Campaign でそのデータをアクティブ化できます。

  例えば、スポーツ衣料品の会社は、Adobe Experience Platformを活用したオーディエンスを活用し、Adobe Campaignを使用してアクティブ化して、Adobe Campaignがサポートする様々なチャネルをまたいで顧客ベースに連絡したいと考えています。 メッセージが送信されると、送信数、開封数、クリック数など、Adobe Campaignのエクスペリエンスデータを使用して、Adobe Experience Platform の顧客プロファイルを強化する必要があります。

  その結果、Adobe Experience Cloud エコシステム全体でより一貫性のあるクロスチャネルキャンペーンと、迅速に適応し学習する豊富な顧客プロファイルが実現します。


* Campaign の Audience Activation に加えて、Adobe Campaign Managed Servicesの宛先を利用して、Adobe Experience Platform上のプロファイルに関連付けられている追加のプロファイル属性を取り込み、Adobe Campaign データベースで更新されるように同期プロセスを導入することもできます。

  例えば、Adobe Experience Platform でオプトインとオプトアウトの値を取り込んでいるとします。この接続を使用すると、これらの値を Adobe Campaign に取り込み、定期的に更新されるように同期プロセスを導入できます。

  >[!NOTE]
  >
  >プロファイル属性同期は、Adobe Campaign データベースに既に存在するプロファイルに対して使用できます。

[&#x200B; 詳しくは、Adobe Experience PlatformとのAdobe Campaign統合を参照してください &#x200B;](https://experienceleague.adobe.com/docs/campaign/campaign-v8/connect/ac-aep.html?lang=ja)

## サポートされている ID {#supported-identities}

*Adobe Campaign Managed Cloud Services* では、以下の表に示す ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| external_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 この ID を使用し、顧客を表す Campaign インスタンスの ID （loyalty_ID、account_ID、customer_ID など）にマッピングすることをお勧めします |
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。詳しくは、[ECID](/help/identity-service/features/ecid.md) に関する次のドキュメントを参照してください。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL Apply transformation]**」オプションをオンにして、アクティブ化時にがデータ [!DNL Experience Platform] 自動的にハッシュ化するように設定します。 |
| GAID | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | [&#x200B; 宛先のアクティベーションワークフロー &#x200B;](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes) のプロファイル属性選択画面で選択したように、目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、オーディエンスのすべてのメンバーを書き出しています。 |
| 書き出し頻度 | **[!UICONTROL Batch]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL View Destinations]** および **[!UICONTROL Manage Destinations]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![](../../assets/catalog/email-marketing/adobe-campaign-managed-services/destination-details.png)

* **[!UICONTROL Name]**：今後この宛先を認識するための名前。
* **[!UICONTROL Description]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL Select instance]**：お使いの **[!DNL Campaign]** マーケティングインスタンス。
* **[!UICONTROL Target mapping]**：配信の送信に使用するターゲットマッピング **[!DNL Adobe Campaign]** 選択します。 [詳細情報](https://experienceleague.adobe.com/docs/campaign/campaign-v8/profiles-and-audiences/add-profiles/target-mappings.html)
* **[!UICONTROL Select sync type]**：

   * **[!UICONTROL Audience sync]**:Adobe Experience Platform オーディエンスをAdobe Campaignに送信する場合は、このオプションを使用します。
   * **[!UICONTROL Profile sync (Update only)]**:Adobe Experience Platform プロファイル属性をAdobe Campaignに取り込み、同期プロセスを設定して、定期的に更新できるようにする場合は、このオプションを使用します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読 &#x200B;](../../ui/alerts.md) についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

### ガバナンスポリシーと実施アクション {#governance}

宛先に書き出すデータに適用できるマーケティングアクションを選択します。Adobe Campaignの場合は、**[!UICONTROL Email Targeting]** マーケティングアクションを選択することをお勧めします。

マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](/help/data-governance/policies/overview.md)ページを参照してください。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスデータを有効化する手順については、[&#x200B; バッチプロファイル書き出し宛先に対するオーディエンスデータの有効化 &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html) を参照してください。

### 属性と ID のマッピング {#map}

プロファイルと共に書き出す XDM フィールドを選択し、対応するAdobe Campaign フィールドにマッピングします。[&#x200B; メールマーケティングの宛先の ID および属性の選択について詳しくは、こちらを参照してください &#x200B;](overview.md)

1. ソースフィールドを選択：

   * Adobe Experience PlatformおよびAdobe Campaignでプロファイルを一意に識別するソース ID として、「**識別子**」（例：メールフィールド）を選択します。

   * Adobe Campaignに書き出す必要がある他のすべての **XDM ソースプロファイル属性** を選択します。

   >[!NOTE]
   >
   >「segmentMembershipStatus」フィールドは、segmentMembership のステータスを反映するために必須のマッピングです。 このフィールドはデフォルトで追加され、変更または削除できません。

1. 各フィールドをAdobe Campaignのターゲットフィールドにマッピングします。 使用可能なターゲットフィールドは、[&#x200B; 宛先の作成 &#x200B;](#destination-details) 時に選択したターゲットマッピングによって決定されます。

1. 必須属性と重複排除キーを特定します。 「必須」または「重複排除キー」とマークされた属性の値は、null にはできません。

   * [&#x200B; 必須属性 &#x200B;](../../ui/activate-batch-profile-destinations.md#mandatory-attributes) 選択した属性がすべてのプロファイルレコードに含まれていることを確認します。 例：書き出されるすべてのプロファイルには、メールアドレスが含まれます。 ID フィールドと重複排除キーとして使用するフィールドの両方を必須に設定することをお勧めします。
   * [&#x200B; 重複排除キー &#x200B;](../../ui/activate-batch-profile-destinations.md#mandatory-attributes) は、ユーザーがプロファイルの重複排除する ID を決定するプライマリキーです。

     >[!IMPORTANT]
     >
     >重複排除キー属性の名前が、選択したターゲットマッピングの列名と一致することを確認してください。

   ![](../../assets/catalog/email-marketing/adobe-campaign-managed-services/mapping.png)

1. マッピングが実行されたら、宛先設定を確認および完了して、**[!DNL Campaign]** へのデータの送信を開始できます。
   [&#x200B; 宛先設定を確認して完了する方法を説明します &#x200B;](/help/destinations/destination-types.md#review)。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

宛先がアクティブ化されると、Campaign で対応するジョブおよび書き出されたデータにアクセスできます。

### データ書き出しジョブの監視 {#jobs}

**[!UICONTROL Administration]** / **[!UICONTROL Audit]** / **[!UICONTROL Audience load jobs]** メニューに移動し、Adobe Experience Platformからアクティブ化されたすべてのエクスポートジョブを監視します。

![](../../assets/catalog/email-marketing/adobe-campaign-managed-services/campaign-jobs.png)

### 書き出したデータへのアクセス {#data}

**[!UICONTROL Audience sync]** えば、**[!UICONTROL Profile and target]** / **[!UICONTROL List]** / オーディエンス メニューに移動して、書き出されたオーディ **[!UICONTROL AEP audiences]** ンスを確認できます。

![](../../assets/catalog/email-marketing/adobe-campaign-managed-services/campaign-audiences.png)

**[!UICONTROL Profile sync (Update only)]** えば、宛先でアクティブ化されたオーディエンスのターゲットとなる各プロファイルの Campaign データベースにデータが自動的に更新されます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
