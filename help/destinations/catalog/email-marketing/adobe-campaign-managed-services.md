---
title: Adobe Campaign Managed Cloud Services 接続
description: Adobe Campaign Managed Cloud Servicesは、クロスチャネルのカスタマーエクスペリエンスを設計するためのプラットフォームと、視覚的なキャンペーンオーケストレーション、リアルタイムインタラクション管理およびクロスチャネル実行のための環境を提供します。
exl-id: fe151ad3-c431-4b5a-b453-9d1d9aedf775
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '1633'
ht-degree: 31%

---

# Adobe Campaign Managed Cloud Services 接続 {#adobe-campaign-managed-services}

>[!IMPORTANT]
>
>この統合は、[Adobe Campaign バージョン 8.4 以降 ](https://experienceleague.adobe.com/docs/campaign/campaign-v8/new/release-notes.html?lang=ja#release-8-4-1) で機能します。

## 概要 {#overview}

Adobe Campaign Managed Cloud Services は、クロスチャネルのカスタマーエクスペリエンスを設計するためのプラットフォームと、視覚的なキャンペーンオーケストレーション、リアルタイムインタラクション管理、クロスチャネル実行のための環境を提供します。[Campaign の概要 ](https://experienceleague.adobe.com/docs/campaign/campaign-v8/start/get-started.html?lang=ja)

Campaign を使用すると、次のことを行えます。

* 顧客に関する情報をアクセス可能な単一のビューにまとめ、パーソナライズとエンゲージメントを推進
* メール、モバイル、オンライン、オフラインの各チャネルをカスタマージャーニーに統合
* 有意義でタイムリーなメッセージやオファーの配信を自動化。

## ガードレール {#guardrails}

Adobe Campaign Managed Cloud Services接続を使用する際は、次のガードレールに注意してください。

* この宛先に対して最大 25 個のオーディエンスを [ アクティブ化 ](#activate) できます。

  この制限を変更するには、Campaign エクスプローラーの **管理**/**[!UICONTROL プラットフォーム]**/**[!UICONTROL オプション]** フォルダーにある **[!UICONTROL NmsCdp_Aep_Audience_List_Limit]** オプションの値を更新します。

* オーディエンスごとに、最大 20 個のフィールドをAdobe Campaignに [ マッピング ](#map) できます。

  この制限を変更するには、Campaign エクスプローラーの **管理**/**[!UICONTROL プラットフォーム]**/**[!UICONTROL オプション]** フォルダーにある **[!UICONTROL NmsCdp_Aep_Destinations_Max_Columns]** オプションの値を更新します。

* Azure Blob Storage Data Landing Zone （DLZ）のデータ保持：7 日。
* アクティブ化の頻度は最低 3 時間です。
* この接続でサポートされるファイル名の最大長は 255 文字です。 [ 書き出すファイル名を設定 ](../../ui/activate-batch-profile-destinations.md#configure-file-names) する場合は、ファイル名が 255 文字を超えないようにしてください。 ファイル名の最大長を超えると、アクティベーションエラーが発生します。

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

[ 詳しくは、Adobe Experience PlatformとのAdobe Campaign統合を参照してください ](https://experienceleague.adobe.com/docs/campaign/campaign-v8/connect/ac-aep.html?lang=ja)

## サポートされている ID {#supported-identities}

*Adobe Campaign Managed Cloud Services* では、以下の表に示す ID のアクティブ化をサポートしています。 [ID](/help/identity-service/features/namespaces.md) についての詳細情報。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| external_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 この ID を使用し、顧客を表す Campaign インスタンスの ID （loyalty_ID、account_ID、customer_ID など）にマッピングすることをお勧めします |
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。詳しくは、[ECID](/help/identity-service/features/ecid.md) に関する次のドキュメントを参照してください。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Experience Platform] がデータを自動的にハッシュ化するように設定します。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Experience Platform] がデータを自動的にハッシュ化するように設定します。 |
| GAID | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [ 宛先のアクティベーションワークフロー ](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes) のプロファイル属性選択画面で選択したように、目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、オーディエンスのすべてのメンバーを書き出しています。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の表示]** および **[!UICONTROL 宛先の管理]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![](../../assets/catalog/email-marketing/adobe-campaign-managed-services/destination-details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL インスタンスを選択]**:**[!DNL Campaign]** マーケティングインスタンス。
* **[!UICONTROL ターゲットマッピング]**：配信の送信に使用するターゲットマッピング **[!DNL Adobe Campaign]** 選択します。 [詳細情報](https://experienceleague.adobe.com/docs/campaign/campaign-v8/profiles-and-audiences/add-profiles/target-mappings.html?lang=ja)
* **[!UICONTROL 同期タイプを選択]**:

   * **[!UICONTROL オーディエンス同期]**:Adobe Experience Platform オーディエンスをAdobe Campaignに送信する場合は、このオプションを使用します。
   * **[!UICONTROL プロファイル同期（更新のみ）]**:Adobe Experience Platform プロファイル属性をAdobe Campaignに取り込み、同期プロセスを設定して、定期的に更新できるようにします。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読 ](../../ui/alerts.md) についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

### ガバナンスポリシーと実施アクション {#governance}

宛先に書き出すデータに適用できるマーケティングアクションを選択します。Adobe Campaignの場合は、「**[!UICONTROL メールターゲティング]** マーケティングアクションを選択することをお勧めします。

マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](/help/data-governance/policies/overview.md)ページを参照してください。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**&#x200B;[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}

この宛先にオーディエンスデータを有効化する手順については、[ バッチプロファイル書き出し宛先に対するオーディエンスデータの有効化 ](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html?lang=ja) を参照してください。

### 属性と ID のマッピング {#map}

プロファイルと共に書き出す XDM フィールドを選択し、対応するAdobe Campaign フィールドにマッピングします。[ メールマーケティングの宛先の ID および属性の選択について詳しくは、こちらを参照してください ](overview.md)

1. ソースフィールドを選択：

   * Adobe Experience PlatformおよびAdobe Campaignでプロファイルを一意に識別するソース ID として、「**識別子**」（例：メールフィールド）を選択します。

   * Adobe Campaignに書き出す必要がある他のすべての **XDM ソースプロファイル属性** を選択します。

   >[!NOTE]
   >
   >「segmentMembershipStatus」フィールドは、segmentMembership のステータスを反映するために必須のマッピングです。 このフィールドはデフォルトで追加され、変更または削除できません。

1. 各フィールドをAdobe Campaignのターゲットフィールドにマッピングします。 使用可能なターゲットフィールドは、[ 宛先の作成 ](#destination-details) 時に選択したターゲットマッピングによって決定されます。

1. 必須属性と重複排除キーを特定します。 「必須」または「重複排除キー」とマークされた属性の値は、null にはできません。

   * [ 必須属性 ](../../ui/activate-batch-profile-destinations.md#mandatory-attributes) 選択した属性がすべてのプロファイルレコードに含まれていることを確認します。 例：書き出されるすべてのプロファイルには、メールアドレスが含まれます。 ID フィールドと重複排除キーとして使用するフィールドの両方を必須に設定することをお勧めします。
   * [ 重複排除キー ](../../ui/activate-batch-profile-destinations.md#mandatory-attributes) は、ユーザーがプロファイルの重複排除する ID を決定するプライマリキーです。

     >[!IMPORTANT]
     >
     >重複排除キー属性の名前が、選択したターゲットマッピングの列名と一致することを確認してください。

   ![](../../assets/catalog/email-marketing/adobe-campaign-managed-services/mapping.png)

1. マッピングが実行されたら、宛先設定を確認および完了して、**[!DNL Campaign]** へのデータの送信を開始できます。
   [ 宛先設定を確認して完了する方法を説明します ](/help/destinations/destination-types.md#review)。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

宛先がアクティブ化されると、Campaign で対応するジョブおよび書き出されたデータにアクセスできます。

### データ書き出しジョブの監視 {#jobs}

**[!UICONTROL 管理]**/**[!UICONTROL 監査]**/**[!UICONTROL オーディエンス読み込みジョブ]** メニューに移動し、Adobe Experience Platformからアクティブ化されたすべての書き出しジョブを監視します。

![](../../assets/catalog/email-marketing/adobe-campaign-managed-services/campaign-jobs.png)

### 書き出したデータへのアクセス {#data}

**[!UICONTROL オーディエンス同期]** の場合は、**[!UICONTROL プロファイルとターゲット]**/**[!UICONTROL リスト]**/6&rbrace;AEP オーディエンス **メニューに移動して、書き出されたオーディエンスを確認できます。**

![](../../assets/catalog/email-marketing/adobe-campaign-managed-services/campaign-audiences.png)

**[!UICONTROL プロファイル同期（更新のみ）]** の場合、宛先でアクティブ化されたオーディエンスがターゲットとする各プロファイルの Campaign データベースにデータが自動的に更新されます。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。
