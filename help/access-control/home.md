---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;adobe admin console
solution: Experience Platform
title: アクセス制御の概要
description: Adobe Experience Platform のアクセス制御は、Adobe Admin Console を通じて提供されます。この機能は、Admin Console の製品プロファイルを利用して、ユーザーを権限およびサンドボックスにリンクします。
exl-id: 591d59ad-2784-4ae4-a509-23649ce712c9
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '3279'
ht-degree: 30%

---

# アクセス制御の概要

Adobe Experience Platformのアクセス制御は、**[!UICONTROL Permissions]** Adobe Experience Cloud[の](https://experience.adobe.com/)を通じて提供されます。 この機能では、ユーザーを権限とサンドボックスにリンクする役割とポリシーを活用します。

## アクセス制御階層とワークフロー

Experience Platform のアクセス制御を設定するには、Experience Platform 製品を所有する組織のシステム管理者または製品管理者の権限が必要です。権限を付与または取り消すことができる最小の役割は、製品管理者です。権限を管理できる他の管理者の役割は、システム管理者です（制限なし）。詳しくは、[管理者の役割](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html) に関する Adobe Help Center の記事を参照してください。

>[!NOTE]
>
>これ以降、このドキュメントでの「管理者」は、（前述の説明に従って）製品管理者以上を指します。

アクセス権限を取得し、割り当てるための高度なワークフローを、次のように要約できます。

- Adobe Experience Platform のライセンス認証、または Experience Platform を使用するアプリケーション／アプリケーションサービスのライセンス認証が完了すると、ライセンス認証時に指定した管理者に電子メールが送信されます。
- 管理者は [Adobe Admin Console](#adobe-admin-console) にログインし、概要ページの製品のリストから **Adobe Experience Platform** を選択します。
- Experience Platformへのアクセス権を付与するには、管理者がユーザーをデフォルトの製品プロファイル `AEP-Default-All-Users`に追加することをお勧めします。
- Experience Platform の権限では、管理者は、新しい役割を作成したり、既存の役割の権限とユーザーを編集したりできます。
- 役割を作成または編集する際、管理者は&#x200B;**[!UICONTROL users]** タブを使用して役割にユーザーを追加し、役割の権限を編集して、これらのユーザー（「[!UICONTROL Read Datasets]」または「[!UICONTROL Manage Schemas]」など）に権限を付与します。 同様に、管理者は、同じ編集オプションを使用してサンドボックスへのアクセス権を割り当てることができます。
- ユーザーが Experience Platform ユーザーインターフェイスにログインすると、Experience Platform の機能へのアクセスは、前の手順で付与された権限によって決まります。例えば、ユーザーが[!UICONTROL View Datasets]権限を持っていない場合、サイドメニューの「**[!UICONTROL Datasets]**」タブはそのユーザーには表示されません。

Experience Platform でアクセス制御を管理する方法について詳しくは、『[アクセス制御ユーザーガイド](./ui/overview.md)』を参照してください。

Experience Platform API へのすべての呼び出しは、権限が検証され、適切な権限が現在のユーザーコンテキストで見つからない場合はエラーを返します。UI 内では、現在のユーザーに付与されている権限に応じて、要素が非表示になるか変更されます。

## 権限 {#platform-permissions}

[!UICONTROL Permissions]は、組織のExperience Platform アクセスを一元管理する場所を提供します。 [!UICONTROL Permissions]を通じて、[!UICONTROL Manage Datasets]、[!UICONTROL View Datasets]、[!UICONTROL Manage Profiles]など、様々なExperience Platform機能に対するアクセス権限をユーザーグループに付与できます。

### 役割

[!UICONTROL Roles] セクションでは、権限は役割を使用してユーザーに割り当てられます。 役割を使用すると、1 人または複数のユーザーに権限を付与でき、また、役割を通じてユーザーに割り当てられるサンドボックスの範囲に対するアクセス権を含めることもできます。ユーザーは、組織に属する 1 つ以上の役割に割り当てることができます。

### デフォルトの役割

Experience Platform には、事前設定済みのデフォルトの役割が 2 つあります。次の表に、各デフォルトプロファイルで提供される内容を示します。これには、ユーザーがアクセスを許可するサンドボックスと、そのサンドボックスの範囲内で許可する権限が含まれます。

| 役割 | サンドボックスアクセス | 権限 |
| --- | --- | --- |
| デフォルトの本番稼働 - すべてのアクセス | 製品 | サンドボックス管理権限を除く、Experience Platform に適用されるすべての権限。 |
| サンドボックス管理者 | なし | `Prod` サンドボックスおよびサンドボックス管理権限へのアクセスを提供します。 |

## サンドボックスと権限

非本番稼働用サンドボックスは、他のサンドボックスからデータを分離できるデータ仮想化の一種で、通常は開発実験、テストまたは試用に使用されます。役割の権限により、役割のユーザーは、アクセス権が付与されたサンドボックス環境内の Experience Platform 機能にアクセスできるようになります。デフォルトの Experience Platform ライセンスでは、5 つのサンドボックス（1 つの本番環境と 4 つの非本番環境）が許可されます。10 個の非本番稼働用サンドボックス（合計 75 個まで）のパックを追加できます。詳しくは、組織の管理者またはアドビのセールス担当者にお問い合わせください。

Experience Platform のサンドボックスについて詳しくは、「[サンドボックスの概要](../sandboxes/home.md)」を参照してください。

### サンドボックスへのアクセス

サンドボックスへのアクセスは、役割を通じて管理されます。製品のサンドボックスへのアクセスを有効にする方法の手順について詳しくは、[属性ベースのアクセス制御役割ガイド](./abac/ui/roles.md)を参照してください。

ユーザーには、役割内の 1 つ以上のサンドボックスへのアクセス権を付与できます。1 人のユーザーが複数の役割に含まれている場合、そのユーザーはそれらの役割に含まれるすべてのサンドボックスにアクセスできます。

「サンドボックスの管理」権限を使用すると、ユーザーはサンドボックスの管理、表示またはリセットをおこなえます。

### リソース権限 {#permissions}

リソース権限は、特定のExperience Platform機能へのアクセス権を付与します。 リソースは、関連する一連の権限を含むカテゴリーに分けられ、各メンバーに個別に割り当てることができます。

[!UICONTROL Permissions]では、役割のリソースワークスペースに、その役割に対してアクティブなサンドボックスと権限が表示されます。

![選択したカテゴリと権限のリストを含む役割のリソースワークスペース。](./images/permissions.png)

次の表に、Experience Platformと権限を介して管理されるアプリケーションの両方で使用可能なリソースカテゴリの概要を示します。

| カテゴリ | 説明 |
| --- | --- |
| [!DNL Adobe Mix Modeler] | [!DNL Adobe Mix Modeler]の権限を設定、管理、表示します。 |
| [!DNL AI Assistant] | [!DNL AI Assistant]の権限を設定します。 |
| [!DNL Alerts] | アラートとアラート履歴の管理、解決、表示の権限を設定します。 |
| [!DNL B2B Account Lists] | アカウントリストのアカウントの追加、削除、インポート、削除などのアクションを含め、B2B アカウントリストの管理、表示、公開の権限を設定します。 |
| [!DNL B2B Admin Configurations] | デジタルアセット管理の接続、アセットリポジトリ、イベントなど、B2B管理者設定の管理および表示権限を設定できます。 |
| [!DNL B2B Assets] | 電子メール、SMS、ランディングページ、フラグメント、テンプレート、画像など、B2B アセットの管理と表示権限を設定できます。 |
| [!DNL B2B Buying Groups] | ソリューションの関心、役割テンプレート、購買グループのステータスなどの機能を含む、B2B購買グループの権限を管理および表示します。 |
| [!DNL B2B Channel Configurations] | 通信制限、API資格情報、セキュリティ設定などの設定を含む、B2B チャネル設定の管理および表示権限を構成します。 |
| [!DNL B2B Dashboards] | アカウントエンゲージメント、購買グループのステージ、急増アカウント、連絡先のカバー範囲などの機能を含む、B2B ダッシュボードの表示権限を設定します。 |
| [!DNL B2B Journeys] | アカウントと個人のアクション、イベントリスナー、スプリットパスなどの機能を含む、B2B ジャーニーの権限を管理、表示、公開します。 |
| [!DNL Campaigns] | Journey Optimizerのキャンペーンに対する管理、公開、表示権限を設定します。 |
| [!DNL Channel Configurations] | サブドメイン、IP プール、メッセージプリセット、PTR レコード、抑制リスト、ランディングページ設定、SMS設定、ファイルルーティングなどのチャネル設定機能を管理、表示、エクスポートします。 |
| [!DNL Collaborations] | Real-Time Customer Data Profile Collaboration機能の管理および表示権限を設定します。 |
| [!DNL Computed Attributes] | ドラフトまたはパブリッシュされた計算属性に対する管理権限と表示権限を設定します。 |
| [!DNL Customer Managed Keys] | 顧客管理キーに対する管理権限を設定します。 |
| [!DNL Dashboards] | 標準、カスタム、ライセンス済みのダッシュボードに対する管理および表示権限を設定できます。 |
| [!DNL Data Collection] | データストリームに対する管理権限と表示権限を設定します。 |
| [!DNL Data Governance] | ラベル、ポリシー、アクティビティログなどのデータガバナンス機能に対して、管理、適用、表示の権限を設定できます。 |
| [!DNL Data Ingestion] | ソースやオーディエンス共有などのデータ取り込み機能に対する管理および表示権限を設定できます。 |
| [!DNL Data Lifecycle] | データハイジーン機能に対する管理および表示権限を設定します。 |
| [!DNL Data Management] | データセットや監視データセットおよびストリームなどのデータ管理機能に対する管理および表示権限を設定できます。 |
| [!DNL Data Modeling] | スキーマ、関係、ID メタデータなどのデータモデリング機能に対する管理および表示権限を設定できます。 |
| [!DNL Data Science Workspace] | [!DNL Data Science Workspace]への管理権限を設定します。 |
| [!DNL Decision Management] | 意思決定管理の意思決定、オファー、ランキング戦略機能に対する権限を管理および表示できます。 |
| [!DNL Destinations] | Destinations SDKでのアクティベーションやオーサリングなどの機能を含め、宛先に対する管理および表示権限を設定します。 |
| [!DNL Federated Data] | 連合データ機能の管理と表示権限を設定します。 |
| [!DNL Identity Management] | ID名前空間やID グラフなどのID サービス機能に対する管理および表示権限を設定します。 |
| [!DNL Intelligent Service] | インテリジェントサービスのアトリビューション AIと顧客AIに対する権限を管理および表示するための設定を行います。 |
| [!DNL IP Warmup Configurations] | IP ウォームアッププランに対する管理および表示権限を設定し、IP ウォームアップレポートを表示するための表示権限を表示します。 |
| [!DNL Journey Optimizer Library] | Adobe Journey Optimizerのライブラリ項目に対する管理権限を設定します。 |
| [!DNL Journey Optimizer Rules] | Adobe Journey Optimizerの頻度ルールに対する管理および表示権限を設定します。 |
| [!DNL Journeys] | ジャーニーレポート、イベント、データソース、アクションなどの機能を含む、ジャーニーへの権限を管理、公開、表示します。 |
| [!DNL Messages] | メッセージのプレビューやテストなどの機能を含め、メッセージの管理、公開、表示の権限を設定します。 |
| [!DNL Privacy Service] | Privacy Service機能の管理および表示権限を設定します。 |
| [!DNL Profile Management] | オーディエンス、プロファイル、結合ポリシーなどのプロファイルサービス機能に対して、管理、表示、書き出し、評価の権限を設定できます。 |
| [!DNL Prospects] | 見込み客のアコーディオン表示などの機能を含め、見込み客のスキーマ、プロファイル、オーディエンスに対する管理および表示権限を設定できます。 |
| [!DNL Query Service] | 有効期限のない資格情報や構造化SQL クエリなどのサービス機能をクエリするための管理権限を設定します。 |
| [!DNL Reports] | チャネルレポートに対する表示権限を設定します。 |
| [!DNL Run and Operate] | ヘルスチェックやジョブスケジュールなどの実行および操作機能の表示権限を設定します。 |
| [!DNL Sandbox Administration] | サンドボックスの管理時に権限の管理、表示、リセットを設定します。 |
| [!DNL Traits Configuration] | 計算属性UIを使用して特性を管理および表示します。 |
| [!DNL Translation Services] | プロジェクト、タスク、レビュー、社内、設定、プロバイダーの翻訳サービスに対する権限を管理および表示できます。 |

次の表に、役割で Experience Platform に使用できる権限の概要と、それらの権限でアクセス権が付与される特定の Experience Platform 機能の説明を示します。役割に権限を追加する方法の手順について詳しくは、[属性ベースのアクセス制御役割ガイド](./abac/ui/roles.md)を参照してください。

| カテゴリ | 権限 | 説明 |
| --- | --- | --- |
| [!DNL Adobe Mix Modeler] | [!UICONTROL Manage Adobe Mix Modeler Harmonized Data] | 調和されたデータの表示と変更機能。 |
| [!DNL Adobe Mix Modeler] | [!UICONTROL View Adobe Mix Modeler Harmonized Data] | 調和データへの読み取り専用アクセス。 |
| [!DNL Adobe Mix Modeler] | [!UICONTROL Manage Adobe Mix Modeler Models Configurations] | モデル設定の表示と変更機能。 |
| [!DNL Adobe Mix Modeler] | [!UICONTROL View Adobe Mix Modeler Models Configurations] | モデル設定への読み取り専用アクセス。 |
| [!DNL Adobe Mix Modeler] | [!UICONTROL Manage Adobe Mix Modeler Models Plans Configurations] | プラン設定の表示と変更機能。 |
| [!DNL Adobe Mix Modeler] | [!UICONTROL View Adobe Mix Modeler Models Plans Configurations] | プラン設定への読み取り専用アクセス。 |
| [!DNL AI Assistant] | [!UICONTROL Enable AI Assistant] | [!DNL [AI assistant]](../ai-assistant/access.md)件の質問を実行できます。 |
| [!DNL AI Assistant] | [!UICONTROL View Operational Insights] | [運用上のインサイト ](../ai-assistant/home.md##operational-insights) クエリへの応答を取得するためのアクセス権。 |
| [!DNL AI Assistant] | [!UICONTROL Generate Content] | ユーザーが[!DNL AI Assistant]を使用してコンテンツを生成できるようにします。 |
| [!DNL AI Assistant] | [!UICONTROL Manage Brand Kit] | ユーザーが[!DNL AI Assistant]を使用してブランドガイドラインを作成できるようにします。 |
| [!DNL Alerts] | [!UICONTROL View Alerts History] | アラート履歴への読み取り専用アクセス。 |
| [!DNL Alerts] | [!UICONTROL Resolve Alerts] | アラートの読み取り、編集、削除へのアクセス。 |
| [!DNL Alerts] | [!UICONTROL View Alerts] | アラートへの読み取り専用アクセス。 |
| [!DNL Alerts] | [!UICONTROL Manage Alerts] | アラートの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL B2B Account Lists] | [!UICONTROL Manage B2B Account Lists] | 左側のナビゲーションで&#x200B;**[!UICONTROL Account Lists]**&#x200B;を表示してアクセスできます。 **[!UICONTROL Account Lists]**&#x200B;へのアクセス権を持つユーザーは、すべてのアカウントリスト CRUD関数`/accounts-list`にアクセスできる必要があります。 |
| [!DNL B2B Admin Configurations] | [!UICONTROL Manage B2B Admin Configurations] | 左側のナビゲーションで&#x200B;**[!UICONTROL B2B Admin Configurations]**&#x200B;を表示してアクセスできます。 **[!UICONTROL B2B Admin Configurations]**&#x200B;へのアクセス権を持つユーザーは、すべてのSMS API資格情報CRUD関数`/admin-configs`にアクセスできる必要があります。 |
| [!DNL B2B Assets] | [!UICONTROL Manage B2B Assets] | 左側のナビゲーションで&#x200B;**[!UICONTROL Assets]**&#x200B;を表示してアクセスできます。 **[!UICONTROL Assets]**&#x200B;へのアクセス権を持つユーザーは、すべてのAssets CRUD関数`/assets-listing`にアクセスできる必要があります。 |
| [!DNL B2B Assets] | [!UICONTROL Manage B2B Templates] | 左側のナビゲーションで&#x200B;**[!UICONTROL Templates]**&#x200B;を表示してアクセスできます。 **[!UICONTROL Templates]**&#x200B;へのアクセス権を持つユーザーは、すべてのテンプレート CRUD関数`/b2b-content-templates`にアクセスできます。 |
| [!DNL B2B Assets] | [!UICONTROL Manage B2B Fragments] | 左側のナビゲーションで&#x200B;**[!UICONTROL Fragments]**&#x200B;を表示してアクセスできます。 **[!UICONTROL Fragments]**&#x200B;へのアクセス権を持つユーザーは、すべてのフラグメント CRUD関数`/fragments`にアクセスできる必要があります。 |
| [!DNL B2B Buying Groups] | [!UICONTROL Manage B2B Buying Groups] | 左側のナビゲーションで&#x200B;**[!UICONTROL Buying Groups]**&#x200B;を表示してアクセスできます。 **[!UICONTROL Buying Groups]**&#x200B;へのアクセス権を持つユーザーは、すべての購買グループ CRUD関数`/buying-groups`にアクセスできる必要があります。 |
| [!DNL B2B Dashboards] | [!UICONTROL Manage B2B Engagement Dashboards] | 左側のナビゲーションで&#x200B;**[!UICONTROL Dashboard]**&#x200B;を表示してアクセスできます。 **[!UICONTROL Dashboards]**&#x200B;へのアクセス権を持つユーザーは、すべてのダッシュボードのCRUD関数`/insights-dashboard`にアクセスできる必要があります。 |
| [!DNL B2B Channel Configurations] | [!UICONTROL Manage B2B Channels Configurations] | 左側のナビゲーションで&#x200B;**[!UICONTROL Channels]**&#x200B;を表示してアクセスできます。 **[!UICONTROL Channels]**&#x200B;へのアクセス権を持つユーザーは、すべてのチャネル CRUD関数`/channels-config`にアクセスできる必要があります。 |
| [!DNL B2B Journeys] | [!UICONTROL Manage B2B Account Journeys] | 左側のナビゲーションで&#x200B;**[!UICONTROL Account Journeys]**&#x200B;を表示してアクセスできます。 **[!UICONTROL Account Journeys]**&#x200B;へのアクセス権を持つユーザーは、すべてのアカウントジャーニーCRUD関数`/account-journeys`にアクセスできる必要があります。 |
| [!DNL Campaigns] | [!UICONTROL Manage Campaigns] | キャンペーンの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Campaigns] | [!UICONTROL Approve and Publish Campaigns] | キャンペーンを承認して公開する機能。 |
| [!DNL Campaigns] | [!UICONTROL Publish Campaigns] | キャンペーンを公開する機能： |
| [!DNL Campaigns] | [!UICONTROL View Campaigns] | キャンペーンへの読み取り専用アクセス： |
| [!DNL Campaigns] | [!UICONTROL View Campaigns Report] | キャンペーンレポートへの読み取り専用アクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL View Messages General Settings] | メッセージの一般設定への読み取り専用アクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage Subdomains Delegations] | サブドメインデリゲーションの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage IP Pools] | IP プールの読み取り、作成、編集へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage Messages General Settings] | メッセージの読み取り、作成、編集、削除の一般設定へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage Messages Presets] | メッセージプリセットの読み取り、作成、編集および削除へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL View Messages Presets] | メッセージプリセットへの読み取り専用アクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage PTR Records] | PTR レコードの読み取りおよび編集へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL View PTR Records] | PTR レコードへの読み取り専用アクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage Suppression] | 抑制ルールの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL View Suppression List] | 抑制リストへの読み取り専用アクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL Export Suppression List] | 抑制リストをCSV ファイルとして書き出すためのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage Landing Page Settings] | ランディングページ設定の読み取り、作成、編集および削除へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage SMS Settings] | SMS設定の読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage SMS Subdomains] | SMS サブドメインの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage File Routing] | ファイルのルーティングを読み取り、作成、編集、削除するためのアクセス権。 |
| [!DNL Channel Configurations] | [!UICONTROL View File Routing] | ファイルのルーティングへの読み取り専用アクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage Seedlist] | シードリストの作成と編集機能。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage Language Settings] | 言語設定を作成および編集する機能。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage Web Subdomains] | CJM web サブドメインの作成と編集機能。 |
| [!DNL Channel Configurations] | [!UICONTROL Manage Push Credentials] | プッシュ資格情報を作成、編集、削除する機能。 |
| [!DNL Collaborations] | [!UICONTROL Manage Collaboration Instances] | 組織のコラボレーションインスタンスを表示、作成、更新、削除します。 その他の組織のコラボレーションインスタンスの詳細。 |
| [!DNL Collaborations] | [!UICONTROL Read Collaboration Instances] | 組織のコラボレーションインスタンスを読み、他の組織のコラボレーションインスタンスを見つけます。 |
| [!DNL Collaborations] | [!UICONTROL Manage Connection Invites] | 組織によって開始された接続招待を表示、作成、削除します。 他の組織によって開始された接続の招待を受け入れたり拒否したりします。 |
| [!DNL Collaborations] | [!UICONTROL Read Connection Invites] | 接続招待への読み取り専用アクセス。 |
| [!DNL Collaborations] | [!UICONTROL Manage Collaboration Connections] | 広告主は、設定の表示、作成、更新、および送信と削除を行うことができます。 パブリッシャーは、接続を表示、承認、または辞退できます。 |
| [!DNL Collaborations] | [!UICONTROL Read Collaboration Connections] | 接続への読み取り専用アクセス。 |
| [!DNL Collaborations] | [!UICONTROL Manage Audience Data] | オーディエンスのオンボーディングと発見。 パブリック、プライベート、カスタムのオーディエンスを更新し、オーディエンスインベントリのメタデータ設定を管理できます。 |
| [!DNL Collaborations] | [!UICONTROL Read Audience Data] | オーディエンスの読み取りと発見： |
| [!DNL Collaborations] | [!UICONTROL Manage Measurement Data] | 測定データのオンボーディング、更新、削除： |
| [!DNL Collaborations] | [!UICONTROL Read Measurement Data] | 測定データへの読み取り専用アクセス： |
| [!DNL Collaborations] | [!UICONTROL Manage Projects] | 検出、共有、アクティブ化、測定アクティビティのプロジェクトを表示、作成、更新、削除できます。 |
| [!DNL Collaborations] | [!UICONTROL Read Projects] | 検出、共有、アクティブ化、測定アクティビティのいずれかのプロジェクトを表示します。 |
| [!DNL Collaborations] | [!UICONTROL Read User Activities] | ユーザーアクティビティへの読み取り専用アクセス。 |
| [!DNL Collaborations] | [!UICONTROL Export User Activities] | ユーザーアクティビティの書き出し： |
| [!DNL Collaborations] | [!UICONTROL Read Collaboration Credit Monitoring] | 組織およびインスタンスレベルでの信用調査。 |
| [!DNL Computed Attributes] | [!UICONTROL View Computed attributes] | 計算属性のタブ、在庫、詳細への読み取り専用アクセス。 |
| [!DNL Computed Attributes] | [!UICONTROL Manage Computed attributes] | 計算属性の読み取り、作成、ドラフトの削除、非アクティブ化へのアクセス。 |
| [!DNL Customer Managed Keys] | [!UICONTROL Manage Customer Managed Keys] | 顧客管理キーを表示および設定するためのアクセス。 |
| [!DNL Dashboards] | [!UICONTROL View License Usage Dashboard] | ライセンス使用状況ダッシュボードを表示する読み取り専用アクセス。 |
| [!DNL Dashboards] | [!UICONTROL Manage Standard Dashboards] | Data Warehouse にないカスタム属性の追加。 |
| [!DNL Dashboards] | [!UICONTROL View Standard Dashboards] | プロファイル、宛先、セグメント ダッシュボードへの読み取り専用アクセス。 左側のナビゲーションの「ダッシュボード」および「ダッシュボードのインベントリと統合」タブへのアクセスも有効にします。 |
| [!DNL Dashboards] | [!UICONTROL Manage Custom Dashboards] | ダッシュボードの作成または編集へのアクセス |
| [!DNL Dashboards] | [!UICONTROL View Custom Dashboards] | ユーザー定義ダッシュボードへの読み取り専用アクセス。 |
| [!DNL Dashboards] | [!UICONTROL Manage Report Schedules] | スケジュールの作成機能： |
| [!DNL Dashboards] | [!UICONTROL Export Dashboard Data] | クエリプロモードダッシュボードから表形式のデータを書き出すユーザーの機能を制御します。 |
| [!DNL Data Collection] | [!UICONTROL Manage Datastreams] | データストリームの読み取り、作成、編集へのアクセス。 |
| [!DNL Data Collection] | [!UICONTROL View Datastreams] | データストリームへの読み取り専用アクセス。 |
| [!DNL Data Governance] | [!UICONTROL Manage Usage Labels] | 使用ラベルを読み取り、作成および削除するアクセス権。 |
| [!DNL Data Governance] | [!UICONTROL Manage Data Usage Policies] | データ使用ポリシーの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Data Governance] | [!UICONTROL View Data Usage Policies] | 組織に属するデータ使用ポリシーに対する読み取り専用アクセス。 |
| [!DNL Data Governance] | [!UICONTROL View User Activity Log] | Experience Platform アクティビティの記録された[監査ログ ](../landing/governance-privacy-security/audit-logs/overview.md)を表示するための読み取り専用アクセス。 |
| [!DNL Data Governance] | [!UICONTROL View Privacy Console] | プライバシーコンソールへの読み取り専用アクセス。 |
| [!DNL Data Ingestion] | [!UICONTROL Manage Sources] | ソースへの読み取り、作成、編集、無効化アクセス |
| [!DNL Data Ingestion] | [!UICONTROL View Sources] | 「**[!UICONTROL Catalog]**」タブの利用可能なソースと「**[!UICONTROL Browse]**」タブの認証済みソースへの読み取り専用アクセス。 |
| [!DNL Data Ingestion] | [!DNL Manage Audience Share Connections] | 2つの組織を接続し、[!DNL Segment Match] フローを有効にするために、パートナー共有を作成、承認、拒否するためのアクセス権。 |
| [!DNL Data Ingestion] | [!DNL Manage Audience Share] | アクティブなパートナーで [!DNL Segment Match] フィードを読み取り、作成、編集、公開するためのアクセス権。 |
| [!DNL Data Lifecycle] | [!UICONTROL View Data Lifecycle] | データライフサイクルの読み取り専用アクセス。 |
| [!DNL Data Lifecycle] | [!UICONTROL Manage Data Lifecycle] | データライフサイクルの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Data Modeling] | [!UICONTROL Manage Schemas] | 各スキーマと関連リソースへの読み取り、作成、編集および削除アクセス |
| [!DNL Data Modeling] | [!UICONTROL View Schemas] | スキーマおよび関連リソースへの読み取り専用アクセス |
| [!DNL Data Modeling] | [!UICONTROL Manage Relationships] | スキーマ関係の読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Data Modeling] | [!UICONTROL Manage Identity Metadata] | スキーマ の ID メタデータの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Data Management] | [!UICONTROL Manage Datasets] | データセットへの読み取り、作成、編集、削除アクセススキーマへの読み取り専用アクセス |
| [!DNL Data Management] | [!UICONTROL View Datasets] | データセットおよびスキーマへの読み取り専用アクセス |
| [!DNL Data Management] | [!UICONTROL Data Monitoring] | モニタリングデータセットおよびストリームへの読み取り専用アクセス |
| [!DNL Data Science Workspace] | [!UICONTROL Manage Data Science Workspace] | [!DNL Data Science Workspace] での読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Decision Management] | [!UICONTROL Manage Experience Decisioning] | エクスペリエンス決定エンティティの管理機能。 |
| [!DNL Decision Management] | [!UICONTROL View Experience Decisioning] | Experience Decisioning エンティティへの読み取り専用アクセス。 |
| [!DNL Decision Management] | [!UICONTROL Manage Decisions] | 決定エンティティの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Decisions Management] | [!UICONTROL View Decisions] | 決定エンティティへの読み取り専用アクセス。 |
| [!DNL Decision Management] | [!UICONTROL Manage Offers] | すべてのオファーとコンポーネントの読み取り、作成、編集および削除へのアクセス。 決定とコレクションへの読み取り専用アクセス。 |
| [!DNL Decsion Management] | [!UICONTROL Manage Ranking Strategies] | カスタムレポートの読み取り、作成、編集、削除、アクション機能の使用へのアクセス。 |
| [!DNL Destinations] | [!UICONTROL View Destinations] | **[!UICONTROL Catalog]** タブで利用可能な宛先を表示するための読み取り専用アクセスと、**[!UICONTROL Browse]** タブで認証された宛先。 |
| [!DNL Destinations] | [!UICONTROL Manage Destinations] | 宛先接続と宛先アカウントの読み取り、作成、削除へのアクセス。 |
| [!DNL Destinations] | [!UICONTROL Activate Destinations] | 作成済みのアクティブな宛先に対するデータのアクティブ化機能この権限には、宛先をアクティブ化するユーザーに[!UICONTROL View Destinations]または[!UICONTROL Manage Destinations]のいずれかを付与する必要もあります。 |
| [!DNL Destinations] | [!UICONTROL Activate Segment without Mapping] | [ マッピング手順](../destinations/ui/activate-batch-profile-destinations.md#mapping)を表示せずに、既存の宛先にオーディエンスをアクティブ化する機能。 ユーザーはアクティベーションワークフローでオーディエンスを追加および削除できますが、マッピングされた属性またはIDを追加または削除することはできません。 この権限には、宛先に対してデータをアクティブ化するユーザーに[!UICONTROL View Destinations]権限を付与する必要もあります。 |
| [!DNL Destinations] | [!UICONTROL Manage and Activate Dataset Destinations] | データセット書き出しフローを読み取り、作成、編集および無効化する機能。作成されたアクティブなデータセットにデータをアクティベートする機能。 この権限には、宛先に対してデータをアクティブ化するユーザーに[!UICONTROL View Destinations]権限を付与する必要もあります。 |
| [!DNL Destinations] | [!UICONTROL Destination Authoring] | [Adobe Experience Platform Destination SDK](../destinations/destination-sdk/overview.md) を使用して宛先を作成する機能。 |
| [!DNL Federated Data] | [!UICONTROL Manage Federated Data] | スキーマ、モデル、コンポジションの作成など、あらゆる連合データ機能にアクセスする機能。 |
| [!DNL Identity Management] | [!UICONTROL Manage Identity Namespaces] | ID 名前空間への読み取り、作成、編集および削除アクセス |
| [!DNL Identity Management] | [!UICONTROL View Identity Namespaces] | ID 名前空間への読み取り専用アクセス |
| [!DNL Identity Management] | [!UICONTROL View Identity Graph] | ID グラフへの読み取り専用アクセス |
| [!DNL Identity Management] | [!UICONTROL Manage Identity Settings] | ID設定の読み取り、作成、編集へのアクセス。 |
| [!DNL Identity Management] | [!UICONTROL View Identity Settings] | ID設定への読み取り専用アクセス。 |
| [!DNL Intelligent Services] | [!UICONTROL View Attribution AI] | アトリビューション AIの設定とインサイトへの読み取り専用アクセス。 |
| [!DNL Intelligent Services] | [!UICONTROL Manage Attribution AI] | アトリビューション AI モデルの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Intelligent Services] | [!UICONTROL View Customer AI] | Customer AI モデルの読み取りまたは表示へのアクセス。 |
| [!DNL Intelligent Services] | [!UICONTROL Manage Customer AI] | Customer AI モデルの作成、更新、削除、有効化、または無効化へのアクセス。 |
| [!DNL IP Warmup Configurations] | [!UICONTROL View IP Warmup Plans] | IP ウォームアッププランへの読み取り専用アクセス。 |
| [!DNL IP Warmup Configurations] | [!UICONTROL Manage IP Warmup Plans] | IP ウォームアッププランを管理する機能。 |
| [!DNL IP Warmup Configurations] | [!UICONTROL View IP Warmup Reports] | IP ウォームアップレポートへの読み取り専用アクセス。 |
| [!DNL Journeys] | [!UICONTROL Manage Journeys] | ジャーニーの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Journeys] | [!UICONTROL View Journeys] | ジャーニーへの読み取り専用アクセス： |
| [!DNL Journeys] | [!UICONTROL View Journeys Report] | ジャーニーレポートへの読み取り専用アクセス。 |
| [!DNL Journeys] | [!UICONTROL Manage Journeys Events, Data Sources and Actions] | イベント、データソース、アクションの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Journeys] | [!UICONTROL View Journeys Events, Data Sources and Actions] | イベント、データソース、アクションへの読み取り専用アクセス。 |
| [!DNL Journeys] | [!UICONTROL Approve and Publish Journeys] | ポリシーが適用されたときにジャーニーを承認および公開する機能。 |
| [!DNL Journeys] | [!UICONTROL Publish Journeys] | ジャーニーを公開する機能： |
| [!DNL Journey Optimizer Library] | [!UICONTROL Manage Library Items] | 保存した式を追加および削除する機能。 |
| [!DNL Journey Optimizer Library] | [!UICONTROL Publish Fragments] | コンテンツフラグメントを公開する機能。 |
| [!DNL Journey Optimizer Library] | [!UICONTROL Simulate Content] | プレビューとプルーフのための「コンテンツをシミュレート」オプションへのアクセス。 |
| [!DNL Journey Optimizer Rules] | [!UICONTROL View Frequency Rules] | 頻度ルールへの読み取り専用アクセス。 |
| [!DNL Journey Optimizer Rules] | [!UICONTROL Manage Frequency Rules] | 頻度ルールの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Messages] | [!UICONTROL Manage Messages] | メッセージの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Messages] | [!UICONTROL View Messages] | メッセージへの読み取り専用アクセス。 |
| [!DNL Messages] | [!UICONTROL View Messages Report] | メッセージレポートの読み取りと編集へのアクセス。 |
| [!DNL Messages] | [!UICONTROL Publish Messages] | メッセージを公開する機能。 |
| [!DNL Messages] | [!UICONTROL Manage Messages Preview and Test] | ポリシーが適用されたときにメッセージを承認および公開する機能。 |
| [!DNL Privacy Service] | [!UICONTROL Manage Privacy Service] | プライバシーワークフローの読み取りおよび書き込みへのアクセス。 |
| [!DNL Privacy Service] | [!UICONTROL View Privacy Service] | プライバシーワークフローへの読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL Manage Profiles] | 顧客プロファイルに使用するデータセットへの読み取り、作成、編集、削除アクセス使用可能なプロファイルへの読み取り専用アクセス |
| [!DNL Profile Management] | [!UICONTROL View Profiles] | 使用可能なプロファイルへの読み取り専用アクセス |
| [!DNL Profile Management] | [!UICONTROL Manage Segments] | オーディエンスの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Profile Management] | [!UICONTROL View Segments] | 利用可能なオーディエンスへの読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL Manage Merge Policies] | 結合ポリシーの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Profile Management] | [!UICONTROL View Merge Policies] | 使用可能な結合ポリシーへの読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL Import Audiences] | CSV アップロードワークフローを使用して新しいオーディエンスをインポートする機能。 |
| [!DNL Profile Management] | [!UICONTROL Export Audience Segment] | 評価オーディエンスをデータセットに書き出す機能。 |
| [!DNL Profile Management] | [!UICONTROL Evaluate a Segment to an Audience] | セグメント定義を評価して、オーディエンスのプロファイルを生成する機能。 |
| [!DNL Profile Management] | [!UICONTROL View B2B AI] | すべてのB2B AI/ML サービスの設定と設定への読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL Manage B2B AI] | すべてのB2B AI/ML サービスの設定と設定を読み取り、作成、編集、削除するためのアクセス権。 |
| [!DNL Profile Management] | [!UICONTROL View B2B Profile] | B2B エンティティプロファイル（アカウント、商談など）、すべてのB2B AI/ML サービス、B2B ダッシュボードウィジェットの設定と設定への読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL Manage B2B Profile] | B2B エンティティプロファイル（アカウント、商談など）の読み取り、作成、編集、削除へのアクセス。 すべてのB2B AI/ML サービスおよびB2B ダッシュボードウィジェットの設定と設定に対する読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL Manage Lookalikes] | 類似オーディエンスの作成および削除機能。 |
| [!DNL Profile Management] | [!UICONTROL View B2B Experience] | B2B プロファイルと属性を表示する機能。 |
| [!DNL Profile Management] | [!UICONTROL View Profile Settings] | すべてのプロファイル設定への読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL Manage Profile Settings] | すべてのプロファイル設定の読み取りと編集へのアクセス。 |
| [!DNL Prospects] | [!UICONTROL View Prospects] | 見込み客のスキーマ、プロファイル、オーディエンス、見込み客アコーディオンへの読み取り専用アクセス。 |
| [!DNL Prospects] | [!UICONTROL Manage Prospects] | 見込み客のスキーマ、プロファイル、オーディエンスを作成、管理する能力。 見込み客アコーディオンへの読み取り専用アクセス。 |
| [!DNL Query Service] | [!UICONTROL Manage Queries] | Experience Platform データの構造化SQL クエリの読み取り、作成、編集および削除へのアクセス。 |
| [!DNL Query Service] | [!UICONTROL Manage Query Service Integration] | クエリサービスアクセスの有効期限が切れていない資格情報を作成、更新、削除するためのアクセス。 |
| [!DNL Query Service] | [!UICONTROL Manage Query Sessions] | 既存のセッションを削除する機能。 |
| [!DNL Query Service] | [!UICONTROL Manage Allow List] | 組織のIP制限を管理する機能。 |
| [!DNL Reports] | [!UICONTROL View Channel Reports] | チャネルレポートの表示と変更機能。 |
| [!DNL Run and Operate] | [!UICONTROL View Health Checks] | ヘルスチェックへの読み取り専用アクセス。 |
| [!DNL Run and Operate] | [!UICONTROL View Job Schedules] | ジョブスケジュールへの読み取り専用アクセス。 |
| [!DNL Sandbox Administration] | [!UICONTROL Manage Sandboxes] | サンドボックスへの読み取り、作成、編集、削除アクセス |
| [!DNL Sandbox Administration] | [!UICONTROL View Sandboxes] | 組織に属するサンドボックスへの読み取り専用アクセス |
| [!DNL Sandbox Administration] | [!UICONTROL Reset a Sandbox] | サンドボックスをリセットする機能 |
| [!DNL Sandbox Administration] | [!UICONTROL Manage Packages] | パッケージの作成、読み込み、書き出しのアクセス。 |
| [!DNL Sandbox Administration] | [!UICONTROL Share Packages] | 組織全体でパッケージを共有するためのアクセス権。 |
| [!DNL Traits Configurations] | [!UICONTROL View Traits] | 特性の読み取り専用アクセス。 |
| [!DNL Traits Configurations] | [!UICONTROL Manage Traits] | 特性の管理： |
| [!DNL Translation Service] | [!UICONTROL Manage Translation Projects] | 翻訳プロジェクトを管理する機能。 |
| [!DNL Translation Service] | [!UICONTROL View Translation Projects] | 翻訳プロジェクトへの読み取り専用アクセス。 |
| [!DNL Translation Service] | [!UICONTROL Manage Translation Tasks] | 翻訳タスクを管理する機能。 |
| [!DNL Translation Service] | [!UICONTROL View Translation Tasks] | 翻訳タスクへの読み取り専用アクセス。 |
| [!DNL Translation Service] | [!UICONTROL Manage Translation Reviews] | 翻訳レビューの管理機能。 |
| [!DNL Translation Service] | [!UICONTROL View Translation Reviews] | 翻訳レビューへの読み取り専用アクセス。 |
| [!DNL Translation Service] | [!UICONTROL Manage Translation In-house] | 自社で翻訳を管理する機能。 |
| [!DNL Translation Service] | [!UICONTROL View Translation In-house] | 社内での翻訳作業への読み取り専用アクセス： |
| [!DNL Translation Service] | [!UICONTROL Manage Translation Settings] | 管理者が翻訳設定を管理する機能。 |
| [!DNL Translation Service] | [!UICONTROL Manage Translation Providers] | 翻訳プロバイダーの管理機能。 |

## 次の手順

このガイドを読むことで、Experience Platform. のアクセス制御の主な原則を学びました。これで、[属性ベースのアクセス制御ユーザーガイド](./abac/overview.md)に進み、Experience Cloud を使用して役割を作成し、Experience Platform に権限を割り当てる方法の詳細な手順を確認できます。
