---
keywords: Experience Platform;ホーム;人気のトピック;アクセス制御;adobe admin console
solution: Experience Platform
title: アクセス制御の概要
description: Adobe Experience Platform のアクセス制御は、Adobe Admin Console を通じて提供されます。この機能は、Admin Console の製品プロファイルを利用して、ユーザーを権限およびサンドボックスにリンクします。
exl-id: 591d59ad-2784-4ae4-a509-23649ce712c9
source-git-commit: 50d768eeb0dc8fa85af113db7790ad3e3258ea64
workflow-type: tm+mt
source-wordcount: '3816'
ht-degree: 33%

---

# アクセス制御の概要

Adobe Experience Platform のアクセス制御は、[Adobe Experience Cloud](https://experience.adobe.com/) の&#x200B;**[!UICONTROL 権限]**&#x200B;を通じて提供されます。この機能では、ユーザーを権限とサンドボックスにリンクする役割とポリシーを活用します。

## アクセス制御階層とワークフロー

Experience Platform のアクセス制御を設定するには、Experience Platform 製品を所有する組織のシステム管理者または製品管理者の権限が必要です。権限を付与または取り消すことができる最小の役割は、製品管理者です。権限を管理できる他の管理者の役割は、システム管理者です（制限なし）。詳しくは、[管理者の役割](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html) に関する Adobe Help Center の記事を参照してください。

>[!NOTE]
>
>これ以降、このドキュメントでの「管理者」は、（前述の説明に従って）製品管理者以上を指します。

アクセス権限を取得し、割り当てるための高度なワークフローを、次のように要約できます。

- Adobe Experience Platform のライセンス認証、または Experience Platform を使用するアプリケーション／アプリケーションサービスのライセンス認証が完了すると、ライセンス認証時に指定した管理者に電子メールが送信されます。
- 管理者は [Adobe Admin Console](#adobe-admin-console) にログインし、概要ページの製品のリストから **Adobe Experience Platform** を選択します。
- Experience Platformへのアクセス権を付与するには、管理者がデフォルトの製品プロファイル `AEP-Default-All-Users` にユーザーを追加することをお勧めします。
- Experience Platform の権限では、管理者は、新しい役割を作成したり、既存の役割の権限とユーザーを編集したりできます。
- 役割を作成または編集する際、管理者は、**[!UICONTROL ユーザー]**&#x200B;タブを使用してユーザーを役割に追加し、役割の権限を編集してこれらのユーザーに権限（「[!UICONTROL データセットを読み取り]」や「[!UICONTROL スキーマを管理]」など）を付与します。同様に、管理者は、同じ編集オプションを使用してサンドボックスへのアクセス権を割り当てることができます。
- ユーザーが Experience Platform ユーザーインターフェイスにログインすると、Experience Platform の機能へのアクセスは、前の手順で付与された権限によって決まります。例えば、ユーザーが[!UICONTROL データセットを表示]権限を持っていない場合、サイドメニューの「**[!UICONTROL データセット]**」タブはそのユーザーには表示されません。

Experience Platform でアクセス制御を管理する方法について詳しくは、『[アクセス制御ユーザーガイド](./ui/overview.md)』を参照してください。

Experience Platform API へのすべての呼び出しは、権限が検証され、適切な権限が現在のユーザーコンテキストで見つからない場合はエラーを返します。UI 内では、現在のユーザーに付与されている権限に応じて、要素が非表示になるか変更されます。

## 権限 {#platform-permissions}

[!UICONTROL 権限]を使用すると、組織の Experience Platform アクセスを一元的に管理できます。[!UICONTROL 権限]を通じて、[!UICONTROL データセットを管理]、[!UICONTROL データセットを表示]、[!UICONTROL プロファイルを管理]などの様々な Experience Platform 機能に対するアクセス権をユーザーのグループに付与できます。

### 役割

「[!UICONTROL 役割]」セクションでは、役割を使用することでユーザーに権限が割り当てられます。役割を使用すると、1 人または複数のユーザーに権限を付与でき、また、役割を通じてユーザーに割り当てられるサンドボックスの範囲に対するアクセス権を含めることもできます。ユーザーは、組織に属する 1 つ以上の役割に割り当てることができます。

### デフォルトの役割

Experience Platform には、事前設定済みのデフォルトの役割が 2 つあります。次の表に、各デフォルトプロファイルで提供される内容を示します。これには、ユーザーがアクセスを許可するサンドボックスと、そのサンドボックスの範囲内で許可する権限が含まれます。

| 役割 | サンドボックスアクセス | 権限 |
| --- | --- | --- |
| デフォルトの実稼働 - すべてのアクセス | Prod | サンドボックス管理権限を除く、Experience Platform に適用されるすべての権限。 |
| サンドボックス管理者 | なし | `Prod` サンドボックスへのアクセスとサンドボックス管理権限へのアクセスを提供します。 |

## サンドボックスと権限

非実稼働サンドボックスは、他のサンドボックスからデータを分離できるデータ仮想化の一種で、通常は開発実験、テストまたは試用に使用されます。役割の権限により、役割のユーザーは、アクセス権が付与されたサンドボックス環境内の Experience Platform 機能にアクセスできるようになります。デフォルトの Experience Platform ライセンスでは、5 つのサンドボックス（1 つの実稼動環境と 4 つの非実稼動環境）が許可されます。10 個の非実稼動用サンドボックス（合計 75 個まで）のパックを追加できます。詳しくは、組織の管理者またはアドビのセールス担当者にお問い合わせください。

Experience Platform のサンドボックスについて詳しくは、「[サンドボックスの概要](../sandboxes/home.md)」を参照してください。

### サンドボックスへのアクセス

サンドボックスへのアクセスは、役割を通じて管理されます。製品のサンドボックスへのアクセスを有効にする方法の手順について詳しくは、[属性ベースのアクセス制御役割ガイド](./abac/ui/roles.md)を参照してください。

ユーザーには、役割内の 1 つ以上のサンドボックスへのアクセス権を付与できます。1 人のユーザーが複数の役割に含まれている場合、そのユーザーはそれらの役割に含まれるすべてのサンドボックスにアクセスできます。

「サンドボックスの管理」権限を使用すると、ユーザーはサンドボックスの管理、表示またはリセットをおこなえます。

### リソース権限 {#permissions}

リソース権限は、特定のExperience Platform機能へのアクセス権を付与します。 リソースは、関連する権限のセットを含むカテゴリに分類されており、役割に個別に割り当てることができます。

[!UICONTROL  権限 ] では、役割のリソースワークスペースに、その役割に対してアクティブなサンドボックスと権限が表示されます。

![ 選択したカテゴリと権限のリストを含む役割のリソースワークスペース。](./images/permissions.png)

次の表に、Experience Platformと権限で管理されるアプリケーションの両方で使用可能なリソースカテゴリの概要を示します。

| カテゴリ | 説明 |
| --- | --- |
| [!DNL Adobe Mix Modeler] | [!DNL Adobe Mix Modeler] の権限を設定、管理、表示します。 |
| [!DNL AI Assistant] | [!DNL AI Assistant] の権限の設定 |
| [!DNL Alerts] | アラートおよびアラート履歴の管理、解決、および表示権限を設定します。 |
| [!DNL B2B Account Lists] | アカウントリストのアカウントの追加、削除、インポート、削除などのアクションを含む、B2B アカウントリストの管理、表示、公開権限を設定します。 |
| [!DNL B2B Admin Configurations] | デジタルアセット管理接続、アセットリポジトリー、イベントを含む、B2B 管理設定の管理権限と表示権限を設定します。 |
| [!DNL B2B Assets] | メール、SMS、ランディングページ、フラグメント、テンプレート、画像を含む、B2B アセットの管理権限と表示権限を設定します。 |
| [!DNL B2B Buying Groups] | ソリューションの関心、役割のテンプレート、購入グループのステータスなどの機能を含む、B2B 購入グループの管理権限と表示権限を設定します。 |
| [!DNL B2B Channel Configurations] | 通信制限、API 資格情報、セキュリティ設定などの設定を含む、B2B チャネル設定の管理権限と表示権限を設定します。 |
| [!DNL B2B Dashboards] | アカウントエンゲージメント、購入グループステージ、サージングアカウント、連絡先の対応状況などの機能を含む、B2B ダッシュボードの表示権限を設定します。 |
| [!DNL B2B Journeys] | アカウントおよび人物のアクション、イベントリスナー、分割パスなどの機能を含む、B2B ジャーニーの管理、表示、公開権限を設定します。 |
| [!DNL Campaigns] | Journey Optimizerでキャンペーンに対する管理、公開、表示権限を設定します。 |
| [!DNL Channel Configurations] | サブドメイン、IP プール、メッセージプリセット、PTR レコード、抑制リスト、ランディングページ設定、SMS 設定、ファイルのルーティングなどのチャネル設定機能の管理、表示、書き出しを設定します。 |
| [!DNL Collaborations] | リアルタイム顧客データプロファイルのCollaboration機能に対する管理権限と表示権限を設定します。 |
| [!DNL Computed Attributes] | ドラフトまたは公開済みの計算属性に対する管理権限と表示権限を設定します。 |
| [!DNL Customer Managed Keys] | 顧客管理キーへの管理権限を設定します。 |
| [!DNL Dashboards] | 標準ダッシュボード、カスタムダッシュボード、ライセンス済みダッシュボードに対する管理権限と表示権限を設定します。 |
| [!DNL Data Collection] | データストリームに対する管理権限と表示権限を設定します。 |
| [!DNL Data Governance] | ラベル、ポリシー、アクティビティログなどのデータガバナンス機能に対する管理、適用、表示権限を設定します。 |
| [!DNL Data Ingestion] | ソースやオーディエンス共有などのデータ取得機能に対する管理権限と表示権限を設定します。 |
| [!DNL Data Lifecycle] | データハイジーン機能に対する管理および表示権限を設定します。 |
| [!DNL Data Management] | データセットおよび監視データセットとストリームなどのデータ管理機能に対する管理権限と表示権限を設定します。 |
| [!DNL Data Modeling] | スキーマ、関係、ID メタデータなどのデータモデリング機能に対する管理権限と表示権限を設定します。 |
| [!DNL Data Science Workspace] | [!DNL Data Science Workspace] への管理権限を設定します。 |
| [!DNL Decision Management] | 意思決定管理で、決定、オファーおよびランキング戦略機能に対する管理権限と表示権限を設定します。 |
| [!DNL Destinations] | 宛先SDKを使用したアクティベーションやオーサリングなどの機能を含む、宛先に対する管理権限と表示権限を設定します。 |
| [!DNL Federated Data] | 連合データ機能に対する管理権限と表示権限を設定します。 |
| [!DNL Identity Management] | ID 名前空間や ID グラフなど、ID サービス機能に対する管理権限と表示権限を設定します。 |
| [!DNL Intelligent Service] | インテリジェントサービスでのアトリビューション AI および顧客 AI に対する管理権限と表示権限の設定。 |
| [!DNL IP Warmup Configurations] | IP ウォームアッププランに対する管理権限と表示権限を設定し、IP ウォームアップレポートを表示する権限を設定します。 |
| [!DNL Journey Optimizer Library] | Adobe Journey Optimizerのライブラリ項目に対する管理権限を設定します。 |
| [!DNL Journey Optimizer Rules] | Adobe Journey Optimizerの頻度ルールに対する管理権限と表示権限を設定します。 |
| [!DNL Journeys] | ジャーニーレポート、イベント、データソース、アクションなどの機能を含む、ジャーニーに対する管理、公開、表示権限を設定します。 |
| [!DNL Messages] | メッセージのプレビューやテストなどの機能を含む、メッセージに対する管理、公開、表示権限を設定します。 |
| [!DNL Privacy Service] | Privacy Service機能に対する管理権限と表示権限を設定します。 |
| [!DNL Profile Management] | オーディエンス、プロファイル、結合ポリシーなどのプロファイルサービス機能に対する管理、表示、エクスポートおよび評価権限を設定します。 |
| [!DNL Prospects] | 見込み客アコーディオンの表示などの機能を含む、見込み客スキーマ、プロファイルおよびオーディエンスに対する管理権限と表示権限を設定します。 |
| [!DNL Query Service] | 有効期限のない資格情報や構造化 SQL クエリなどのクエリサービス機能に対する管理権限を設定します。 |
| [!DNL Reports] | レポートをチャネルする表示権限を設定します。 |
| [!DNL Sandbox Administration] | サンドボックスの管理時に、管理、表示、リセット権限を設定します。 |
| [!DNL Traits Configuration] | 計算属性 UI を使用して特性の管理と表示を設定します。 |
| [!DNL Translation Services] | プロジェクト、タスク、レビュー、社内、設定、プロバイダーの翻訳サービスに対する管理権限と表示権限を設定します。 |

次の表に、役割で Experience Platform に使用できる権限の概要と、それらの権限でアクセス権が付与される特定の Experience Platform 機能の説明を示します。役割に権限を追加する方法の手順について詳しくは、[属性ベースのアクセス制御役割ガイド](./abac/ui/roles.md)を参照してください。

| カテゴリ | 権限 | 説明 |
| --- | --- | --- |
| [!DNL Adobe Mix Modeler] | [!UICONTROL Adobe Mix Modeler統一データの管理 ] | 統一データを表示および変更する機能。 |
| [!DNL Adobe Mix Modeler] | [!UICONTROL Adobe Mix Modeler統一データの表示 ] | 統一データへの読み取り専用アクセス。 |
| [!DNL Adobe Mix Modeler] | [!UICONTROL Adobe Mix Modeler モデル設定の管理 ] | モデル設定を表示および変更する機能。 |
| [!DNL Adobe Mix Modeler] | [!UICONTROL Adobe Mix Modeler モデルの設定を表示 ] | モデル設定への読み取り専用アクセス |
| [!DNL Adobe Mix Modeler] | [!UICONTROL Adobe Mix Modeler モデルプラン設定の管理 ] | プラン設定を表示および変更する機能。 |
| [!DNL Adobe Mix Modeler] | [!UICONTROL Adobe Mix Modeler モデルプラン設定の表示 ] | 計画の構成への読取り専用アクセス |
| [!DNL AI Assistant] | [!UICONTROL AI アシスタントを有効にする ] | [!DNL [AI assistant]](../ai-assistant/access.md) の質問をする能力。 |
| [!DNL AI Assistant] | [!UICONTROL  運用インサイトの表示 ] | へのアクセスで、[ 運用インサイト ](../ai-assistant/home.md##operational-insights) クエリに対する応答を取得します。 |
| [!DNL AI Assistant] | [!UICONTROL  コンテンツを生成 ] | ユーザーが [!DNL AI Assistant] を使用してコンテンツを生成できるようにします。 |
| [!DNL AI Assistant] | [!UICONTROL  ブランドキットの管理 ] | ユーザーが [!DNL AI Assistant] を使用してブランドガイドラインを作成できるようにします。 |
| [!DNL Alerts] | [!UICONTROL アラート履歴の表示] | アラート履歴への読み取り専用アクセス。 |
| [!DNL Alerts] | [!UICONTROL アラートの解決] | アラートの読み取り、編集、削除へのアクセス。 |
| [!DNL Alerts] | [!UICONTROL アラートの表示] | アラートへの読み取り専用アクセス。 |
| [!DNL Alerts] | [!UICONTROL アラートの管理] | アラートの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL B2B Account Lists] | [!UICONTROL B2B アカウントリストの管理 ] | 左側のナビゲーションに **[!UICONTROL アカウントリスト]** を表示してアクセスする機能。 **[!UICONTROL アカウントリスト]** にアクセスできるユーザーは、すべてのアカウントリスト CRUD 機能（`/accounts-list`）にアクセスできる必要があります。 |
| [!DNL B2B Admin Configurations] | [!UICONTROL B2B 管理設定の管理 ] | 左側のナビゲーションに **[!UICONTROL B2B 管理設定]** を表示してアクセスする機能。 **[!UICONTROL B2B 管理設定にアクセスできるユーザーは]** すべての SMS API 資格情報 CRUD 関数（`/admin-configs`）にアクセスできる必要があります。 |
| [!DNL B2B Assets] | [!UICONTROL B2B Assetsの管理 ] | 左側のナビゲーションで **[!UICONTROL Assets]** を表示してアクセスする機能。 **[!UICONTROL Assets]** にアクセスできるユーザーは、すべてのAssets CRUD 機能（`/assets-listing`）にアクセスできる必要があります。 |
| [!DNL B2B Assets] | [!UICONTROL B2B テンプレートの管理 ] | 左側のナビゲーションで **[!UICONTROL テンプレート]** を表示してアクセスする機能。 **[!UICONTROL テンプレート]** にアクセスできるユーザーは、すべてのテンプレート CRUD 関数（`/b2b-content-templates`）にアクセスできる必要があります。 |
| [!DNL B2B Assets] | [!UICONTROL B2B フラグメントの管理 ] | 左側のナビゲーションで **[!UICONTROL フラグメント]** を表示してアクセスする機能。 **[!UICONTROL フラグメント]** にアクセスできるユーザーは、すべてのフラグメント CRUD 関数（`/fragments`）にアクセスできる必要があります。 |
| [!DNL B2B Buying Groups] | [!UICONTROL B2B 購入グループの管理 ] | 左側のナビゲーションで **[!UICONTROL 購入グループ]** を表示してアクセスする機能。 **[!UICONTROL 購入グループ]** へのアクセス権を持つユーザーは、すべての購入グループ CRUD 機能（`/buying-groups`）にアクセスできる必要があります。 |
| [!DNL B2B Dashboards] | [!UICONTROL B2B エンゲージメントダッシュボードの管理 ] | 左側のナビゲーションで **[!UICONTROL ダッシュボード]** を表示してアクセスする機能。 **[!UICONTROL ダッシュボード]** へのアクセス権を持つユーザーは、すべてのダッシュボード CRUD 関数（`/insights-dashboard`）にアクセスできる必要があります。 |
| [!DNL B2B Channel Configurations] | [!UICONTROL B2B チャネル設定の管理 ] | 左側のナビゲーションで **[!UICONTROL チャネル]** を表示してアクセスする機能。 **[!UICONTROL チャネル]** にアクセスできるユーザーは、すべてのチャネル CRUD 関数（`/channels-config`）にアクセスできる必要があります。 |
| [!DNL B2B Journeys] | [!UICONTROL B2B アカウントのジャーニーの管理 ] | 左側のナビゲーションで **[!UICONTROL アカウントジャーニー]** を表示してアクセスする機能。 **[!UICONTROL アカウントジャーニー]** にアクセスできるユーザーは、すべてのアカウントジャーニーの CRUD 機能（`/account-journeys`）にアクセスできる必要があります。 |
| [!DNL Campaigns] | [!UICONTROL  キャンペーンの管理 ] | キャンペーンの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Campaigns] | [!UICONTROL  キャンペーンの承認と公開 ] | キャンペーンを承認および公開する機能。 |
| [!DNL Campaigns] | [!UICONTROL  キャンペーンの公開 ] | キャンペーンを公開する機能 |
| [!DNL Campaigns] | [!UICONTROL  キャンペーンの表示 ] | キャンペーンへの読み取り専用アクセス |
| [!DNL Campaigns] | [!UICONTROL  キャンペーンレポートを表示 ] | キャンペーンレポートへの読み取り専用アクセス |
| [!DNL Channel Configurations] | [!UICONTROL  メッセージの一般設定の表示 ] | メッセージの一般設定への読み取り専用アクセス |
| [!DNL Channel Configurations] | [!UICONTROL  サブドメインのデリゲーションの管理 ] | サブドメインデリゲーションの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL IP プールの管理 ] | IP プールの読み取り、作成および編集へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL  メッセージの一般設定の管理 ] | メッセージの一般設定への読み取り、作成、編集、および削除アクセス |
| [!DNL Channel Configurations] | [!UICONTROL  メッセージプリセットの管理 ] | メッセージプリセットの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL  メッセージプリセットの表示 ] | メッセージプリセットへの読み取り専用アクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL PTR レコードの管理 ] | PTR レコードの読み取りと編集へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL PTR レコードの表示 ] | PTR レコードへの読み取り専用アクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL  抑制の管理 ] | 抑制ルールの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL  抑制リストの表示 ] | 抑制リストへの読み取り専用アクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL  抑制リストの書き出し ] | 抑制リストを CSV ファイルとして書き出すためのアクセス権。 |
| [!DNL Channel Configurations] | [!UICONTROL  ランディングページ設定の管理 ] | ランディングページ設定の読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL SMS 設定の管理 ] | SMS 設定の読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL SMS サブドメインの管理 ] | SMS サブドメインの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL  ファイルのルーティングの管理 ] | ファイル ルーティングの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL  ファイルのルーティングを表示 ] | ファイルルーティングへの読み取り専用アクセス。 |
| [!DNL Channel Configurations] | [!UICONTROL  シードリストの管理 ] | シードリストを作成および編集する機能。 |
| [!DNL Channel Configurations] | [!UICONTROL  言語設定の管理 ] | 言語設定を作成および編集する機能。 |
| [!DNL Channel Configurations] | [!UICONTROL Web サブドメインの管理 ] | CJM web サブドメインを作成および編集する機能。 |
| [!DNL Channel Configurations] | [!UICONTROL  プッシュ資格情報の管理 ] | プッシュ認証情報を作成、編集、削除する機能。 |
| [!DNL Collaborations] | [!UICONTROL Collaboration インスタンスの管理 ] | 組織のコラボレーション インスタンスを表示、作成、更新、および削除します。 他の組織のコラボレーション インスタンスを検出します。 |
| [!DNL Collaborations] | [!UICONTROL Collaboration インスタンスの読み取り ] | 組織のコラボレーションインスタンスを読み取り、他の組織のコラボレーションインスタンスを検出します。 |
| [!DNL Collaborations] | [!UICONTROL  接続招待の管理 ] | 組織が開始した接続招待を表示、作成および削除します。 他の組織によって開始された接続招待を承認または拒否します。 |
| [!DNL Collaborations] | [!UICONTROL  接続招待を読み取り ] | 接続招待への読み取り専用アクセス。 |
| [!DNL Collaborations] | [!UICONTROL Collaboration接続の管理 ] | 広告主は、設定の表示、作成、更新のほか、接続の送信と削除を行うことができます。 パブリッシャーは、接続を表示、許可、拒否できます。 |
| [!DNL Collaborations] | [!UICONTROL Collaboration Connections を読む ] | 接続への読み取り専用アクセス |
| [!DNL Collaborations] | [!UICONTROL  オーディエンスデータの管理 ] | オーディエンスのオンボーディングと検出。 パブリック、プライベートおよびカスタムオーディエンスを更新し、オーディエンスインベントリメタデータ設定を管理します。 |
| [!DNL Collaborations] | [!UICONTROL  オーディエンスデータの読み取り ] | オーディエンスの読み取り、検出。 |
| [!DNL Collaborations] | [!UICONTROL  測定データの管理 ] | 測定データのオンボーディング、更新、削除 |
| [!DNL Collaborations] | [!UICONTROL  測定データの読み取り ] | 測定データへの読み取り専用アクセス |
| [!DNL Collaborations] | [!UICONTROL  プロジェクトの管理 ] | 検出、共有、アクティブ化、測定の各アクティビティに対して、プロジェクトを表示、作成、更新、削除します。 |
| [!DNL Collaborations] | [!UICONTROL  プロジェクトの読み取り ] | 検出、共有、アクティブ化、測定の各アクティビティのプロジェクトを表示します。 |
| [!DNL Collaborations] | [!UICONTROL  ユーザーの読み取りアクティビティ ] | ユーザーアクティビティへの読み取り専用アクセス |
| [!DNL Collaborations] | [!UICONTROL  エクスポートユーザーアクティビティ ] | ユーザーアクティビティを書き出します。 |
| [!DNL Collaborations] | [!UICONTROL Collaborationの与信モニタリングを参照 ] | 組織レベルおよびインスタンスレベルでのクレジットの監視。 |
| [!DNL Computed Attributes] | [!UICONTROL  計算済み属性の表示 ] | 「計算属性」タブ、在庫および詳細への読み取り専用アクセス |
| [!DNL Computed Attributes] | [!UICONTROL  計算属性の管理 ] | ドラフトの読み取り、作成、削除、計算属性の非アクティブ化へのアクセス。 |
| [!DNL Customer Managed Keys] | [!UICONTROL  顧客管理キーの管理 ] | 顧客管理キーを表示および設定するためのアクセス。 |
| [!DNL Dashboards] | [!UICONTROL ライセンス使用状況ダッシュボードの表示] | ライセンス使用状況ダッシュボードを表示する読み取り専用アクセス。 |
| [!DNL Dashboards] | [!UICONTROL 標準ダッシュボードの管理] | Data Warehouse にないカスタム属性の追加。 |
| [!DNL Dashboards] | [!UICONTROL  標準ダッシュボードの表示 ] | ライセンス使用状況ダッシュボードを表示する読み取り専用アクセス。 |
| [!DNL Dashboards] | [!UICONTROL  カスタムダッシュボードの管理 ] | ダッシュボードの作成または編集へのアクセス。 |
| [!DNL Dashboards] | [!UICONTROL  カスタムダッシュボードの表示 ] | ユーザー定義ダッシュボードへの読み取り専用アクセス |
| [!DNL Dashboards] | [!UICONTROL  レポートスケジュールの管理 ] | スケジュールを作成する機能 |
| [!DNL Data Collection] | [!UICONTROL  データストリームの管理 ] | データストリームの読み取り、作成、編集へのアクセス。 |
| [!DNL Data Collection] | [!UICONTROL  データストリームの表示 ] | データストリームへの読み取り専用アクセス |
| [!DNL Data Governance] | [!UICONTROL  使用状況ラベルの管理 ] | 使用ラベルを読み取り、作成および削除するアクセス権。 |
| [!DNL Data Governance] | [!UICONTROL データ使用ポリシーの管理] | データ使用ポリシーの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Data Governance] | [!UICONTROL データ使用ポリシーの表示] | 組織に属するデータ使用ポリシーに対する読み取り専用アクセス。 |
| [!DNL Data Governance] | [!UICONTROL ユーザーアクティビティログを表示] | Platform のアクティビティを記録した [監査ログ](../landing/governance-privacy-security/audit-logs/overview.md) を表示する読み取り専用アクセス。 |
| [!DNL Data Governance] | [!UICONTROL  プライバシーコンソールを表示 ] | プライバシーコンソールへの読み取り専用アクセス |
| [!DNL Data Ingestion] | [!UICONTROL ソースの管理] | ソースへの読み取り、作成、編集、無効化アクセス |
| [!DNL Data Ingestion] | [!UICONTROL ソースの表示] | 「**[!UICONTROL カタログ]**」タブでの使用可能なソースおよび「**[!UICONTROL 参照]**」タブでの認証済みのソースへの読み取り専用アクセス |
| [!DNL Data Ingestion] | [!DNL Manage Audience Share Connections] | 2 つの組織を接続し、[!DNL Segment Match] のフローを有効にするパートナー共有を作成、承認、拒否するためのアクセス権。 |
| [!DNL Data Ingestion] | [!DNL Manage Audience Share] | アクティブなパートナーで [!DNL Segment Match] フィードを読み取り、作成、編集、公開するためのアクセス権。 |
| [!DNL Data Lifecycle] | [!UICONTROL  データのライフサイクルの表示 ] | データ ライフサイクルへの読み取り専用アクセス |
| [!DNL Data Lifecycle] | [!UICONTROL  データのライフサイクルの管理 ] | データ ライフサイクルの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Data Modeling] | [!UICONTROL スキーマの管理] | 各スキーマと関連リソースへの読み取り、作成、編集および削除アクセス |
| [!DNL Data Modeling] | [!UICONTROL スキーマの表示] | スキーマおよび関連リソースへの読み取り専用アクセス |
| [!DNL Data Modeling] | [!UICONTROL 関係の管理] | スキーマ関係の読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Data Modeling] | [!UICONTROL ID メタデータの管理] | スキーマ の ID メタデータの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Data Management] | [!UICONTROL データセットの管理] | データセットへの読み取り、作成、編集、削除アクセススキーマへの読み取り専用アクセス |
| [!DNL Data Management] | [!UICONTROL データセットの表示] | データセットおよびスキーマへの読み取り専用アクセス |
| [!DNL Data Management] | [!UICONTROL データ監視] | 監視データセットおよびストリームへの読み取り専用アクセス |
| [!DNL Data Science Workspace] | [!UICONTROL Data Science Workspace の管理] | [!DNL Data Science Workspace] での読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Decision Management] | [!UICONTROL  エクスペリエンス決定の管理 ] | エクスペリエンス決定エンティティを管理する機能。 |
| [!DNL Decision Management] | [!UICONTROL  エクスペリエンス決定を表示 ] | Experience Decisioning エンティティへの読み取り専用アクセス。 |
| [!DNL Decision Management] | [!UICONTROL  決定の管理 ] | 決定エンティティの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Decisions Management] | [!UICONTROL  決定の表示 ] | 決定エンティティへの読み取り専用アクセス。 |
| [!DNL Decision Management] | [!UICONTROL  オファーの管理 ] | すべてのオファーとコンポーネントへの読み取り、作成、編集および削除アクセス 決定およびコレクションへの読み取り専用アクセス |
| [!DNL Decsion Management] | [!UICONTROL  ランキング戦略の管理 ] | カスタムレポートへの読み取り、作成、編集、削除アクセス、およびアクション機能の使用。 |
| [!DNL Destinations] | [!UICONTROL 宛先の表示] | 「**[!UICONTROL カタログ]**」タブの使用可能な宛先と「**[!UICONTROL 参照]**」タブの認証済みの宛先を表示する読み取り専用アクセス。 |
| [!DNL Destinations] | [!UICONTROL 宛先の管理] | 宛先接続および宛先アカウントの読み取り、作成、削除へのアクセス。 |
| [!DNL Destinations] | [!UICONTROL 宛先のアクティブ化] | 作成済みのアクティブな宛先に対するデータのアクティブ化機能また、この権限の場合は、宛先をアクティブ化するユーザーに [!UICONTROL  宛先の表示 ] または [!UICONTROL  宛先の管理 ] のいずれかを付与する必要があります。 |
| [!DNL Destinations] | [!UICONTROL マッピングを使用しないセグメントアクティブ化] | [ マッピングステップ ](../destinations/ui/activate-batch-profile-destinations.md#mapping) を表示せずに、既存の宛先に対してオーディエンスをアクティブ化する機能。 ユーザーは、アクティベーションワークフローでオーディエンスを追加および削除できますが、マッピングされた属性や ID を追加または削除することはできません。 また、この権限の場合は、宛先に対してデータをアクティブ化するユーザーに [!UICONTROL  宛先の表示 ] 権限を付与する必要があります。 |
| [!DNL Destinations] | [!UICONTROL データセット宛先の管理とアクティブ化] | データセット書き出しフローを読み取り、作成、編集および無効化する機能。作成済みのアクティブなデータセットに対してデータもアクティブ化する機能。 また、この権限の場合は、宛先に対してデータをアクティブ化するユーザーに [!UICONTROL  宛先の表示 ] 権限を付与する必要があります。 |
| [!DNL Destinations] | [!UICONTROL 宛先のオーサリング] | [Adobe Experience Platform Destination SDK](../destinations/destination-sdk/overview.md) を使用して宛先を作成する機能。 |
| [!DNL Federated Data] | [!UICONTROL Federated Data の管理 ] | スキーマ、モデル、コンポジションの作成など、すべての連合データ機能にアクセスする機能。 |
| [!DNL Identity Management] | [!UICONTROL ID 名前空間の管理] | ID 名前空間への読み取り、作成、編集および削除アクセス |
| [!DNL Identity Management] | [!UICONTROL ID 名前空間の表示] | ID 名前空間への読み取り専用アクセス |
| [!DNL Identity Management] | [!UICONTROL ID グラフの表示] | ID グラフへの読み取り専用アクセス |
| [!DNL Identity Management] | [!UICONTROL ID 設定の管理 ] | ID 設定への読み取り、作成および編集アクセス |
| [!DNL Identity Management] | [!UICONTROL ID 設定の表示 ] | ID 設定への読み取り専用アクセス |
| [!DNL Intelligent Services] | [!UICONTROL  アトリビューション AI を表示 ] | アトリビューション AI 設定およびインサイトに対する読み取り専用アクセス。 |
| [!DNL Intelligent Services] | [!UICONTROL  アトリビューション AI の管理 ] | アトリビューション AI モデルの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Intelligent Services] | [!UICONTROL  顧客 AI を表示 ] | 顧客 AI モデルの読み取りまたは表示へのアクセス。 |
| [!DNL Intelligent Services] | [!UICONTROL  顧客 AI の管理 ] | 顧客 AI モデルを作成、更新、削除、有効化または無効化するためのアクセス権。 |
| [!DNL IP Warmup Configurations] | [!UICONTROL IP ウォームアッププランを表示 ] | IP ウォームアッププランへの読み取り専用アクセス。 |
| [!DNL IP Warmup Configurations] | [!UICONTROL IP ウォームアッププランの管理 ] | IP ウォームアッププランを管理する機能。 |
| [!DNL IP Warmup Configurations] | [!UICONTROL IP ウォームアップレポートの表示 ] | IP ウォームアップレポートへの読み取り専用アクセス。 |
| [!DNL Journeys] | [!UICONTROL ジャーニーの管理 ] | ジャーニーの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Journeys] | [!UICONTROL ジャーニーの表示 ] | ジャーニーへの読み取り専用アクセス。 |
| [!DNL Journeys] | [!UICONTROL ジャーニーレポートの表示 ] | ジャーニーレポートへの読み取り専用アクセス。 |
| [!DNL Journeys] | [!UICONTROL ジャーニーイベント、データソース、アクションの管理 ] | イベント、データソース、アクションの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Journeys] | [!UICONTROL ジャーニーイベント、データソース、アクションの表示 ] | イベント、データソース、アクションへの読み取り専用アクセス |
| [!DNL Journeys] | [!UICONTROL ジャーニーの承認と公開 ] | ポリシーが適用されたときにジャーニーを承認および公開する機能。 |
| [!DNL Journeys] | [!UICONTROL ジャーニーの公開 ] | ジャーニーを公開する機能 |
| [!DNL Journey Optimizer Library] | [!UICONTROL  ライブラリ項目の管理 ] | 保存済み式を追加および削除する機能。 |
| [!DNL Journey Optimizer Library] | [!UICONTROL  フラグメントの公開 ] | コンテンツフラグメントを公開する機能。 |
| [!DNL Journey Optimizer Library] | [!UICONTROL  コンテンツをシミュレート ] | プレビューおよびプルーフ用に「コンテンツをシミュレート」オプションへのアクセス。 |
| [!DNL Journey Optimizer Rules] | [!UICONTROL  頻度ルールの表示 ] | 頻度ルールへの読み取り専用アクセス。 |
| [!DNL Journey Optimizer Rules] | [!UICONTROL  頻度ルールの管理 ] | 頻度ルールの読み取り、作成、編集、または削除へのアクセス。 |
| [!DNL Messages] | [!UICONTROL  メッセージの管理 ] | メッセージの読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Messages] | [!UICONTROL  メッセージの表示 ] | メッセージへの読み取り専用アクセス |
| [!DNL Messages] | [!UICONTROL  メッセージレポートの表示 ] | メッセージレポートへの読み取りおよび編集アクセス。 |
| [!DNL Messages] | [!UICONTROL  メッセージの公開 ] | メッセージを公開する機能 |
| [!DNL Messages] | [!UICONTROL  メッセージのプレビューとテストの管理 ] | ポリシーが適用されたときにメッセージを承認および公開する機能。 |
| [!DNL Privacy Service] | [!UICONTROL Privacy Serviceの管理 ] | 読み取りおよび書き込みプライバシーワークフローへのアクセス。 |
| [!DNL Privacy Service] | [!UICONTROL Privacy Serviceを表示 ] | プライバシーワークフローへの読み取り専用アクセス |
| [!DNL Profile Management] | [!UICONTROL プロファイルの管理] | 顧客プロファイルに使用するデータセットへの読み取り、作成、編集、削除アクセス使用可能なプロファイルへの読み取り専用アクセス |
| [!DNL Profile Management] | [!UICONTROL プロファイルの表示] | 使用可能なプロファイルへの読み取り専用アクセス |
| [!DNL Profile Management] | [!UICONTROL セグメントの管理] | オーディエンスへの読み取り、作成、編集、削除アクセス |
| [!DNL Profile Management] | [!UICONTROL セグメントの表示] | 使用可能なオーディエンスへの読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL 結合ポリシーの管理] | 結合ポリシーの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Profile Management] | [!UICONTROL 結合ポリシーの表示] | 使用可能な結合ポリシーへの読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL  オーディエンスのインポート ] | CSV アップロードワークフローを使用して、新しいオーディエンスを読み込む機能。 |
| [!DNL Profile Management] | [!UICONTROL  オーディエンスセグメントを書き出し ] | 評価済みのオーディエンスをデータセットに書き出す機能。 |
| [!DNL Profile Management] | [!UICONTROL オーディエンスに対するセグメントの評価] | セグメント定義を評価して、オーディエンスのプロファイルを生成する機能。 |
| [!DNL Profile Management] | [!UICONTROL B2B AI を表示 ] | すべての B2B AI/ML サービスの設定への読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL B2B AI の管理 ] | すべての B2B AI/ML サービスの設定および設定の読み取り、作成、編集、削除へのアクセス。 |
| [!DNL Profile Management] | [!UICONTROL B2B プロファイルの表示 ] | B2B エンティティプロファイル（アカウント、商談など）、すべての B2B AI/ML サービスの設定および設定、B2B ダッシュボードウィジェットへの読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL B2B プロファイルの管理 ] | B2B エンティティプロファイル （アカウント、オポチュニティなど）の読み取り、作成、編集、および削除へのアクセス。 すべての B2B AI/ML サービスおよび B2B ダッシュボードウィジェットの設定と設定に対する読み取り専用アクセス。 |
| [!DNL Profile Management] | [!UICONTROL  ルックアップの管理 ] | 類似オーディエンスを作成または削除できます。 |
| [!DNL Profile Management] | [!UICONTROL B2B エクスペリエンスを表示 ] | B2B のプロファイルと属性を表示する機能。 |
| [!DNL Profile Management] | [!UICONTROL  プロファイル設定の表示 ] | すべてのプロファイル設定への読み取り専用アクセス |
| [!DNL Profile Management] | [!UICONTROL  プロファイル設定の管理 ] | すべてのプロファイル設定の読み取りおよび編集へのアクセス。 |
| [!DNL Prospects] | [!UICONTROL  見込み客の表示 ] | 見込み客スキーマ、プロファイル、オーディエンス、見込み客アコーディオンへの読み取り専用アクセス。 |
| [!DNL Prospects] | [!UICONTROL  見込み客の管理 ] | 見込み客スキーマ、プロファイル、オーディエンスを作成および管理する機能。 見込顧客アコーディオンへの読み取り専用アクセス。 |
| [!DNL Query Service] | [!UICONTROL クエリの管理] | Platform データの構造化 SQL クエリの読み取り、作成、編集、および削除へのアクセス。 |
| [!DNL Query Service] | [!UICONTROL クエリサービス統合の管理] | クエリサービスアクセスの有効期限が切れていない資格情報を作成、更新、削除するためのアクセス。 |
| [!DNL Query Service] | [!UICONTROL  クエリセッションの管理 ] | 既存のセッションを削除できます。 |
| [!DNL Query Service] | [!UICONTROL 許可リストの管理 ] | 組織の IP 制限を管理する機能。 |
| [!DNL Reports] | [!UICONTROL  チャネルレポートの表示 ] | チャネルレポートの表示および変更機能。 |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスの管理] | サンドボックスへの読み取り、作成、編集、削除アクセス |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスの表示] | 組織に属するサンドボックスへの読み取り専用アクセス |
| [!DNL Sandbox Administration] | [!UICONTROL サンドボックスのリセット] | サンドボックスをリセットする機能 |
| [!DNL Sandbox Administration] | [!UICONTROL  パッケージの管理 ] | パッケージの作成、インポート、エクスポートへのアクセス。 |
| [!DNL Sandbox Administration] | [!UICONTROL  パッケージの共有 ] | 異なる組織間でパッケージを共有するためのアクセス。 |
| [!DNL Traits Configurations] | [!UICONTROL  特性を表示 ] | 特性への読み取り専用アクセス |
| [!DNL Traits Configurations] | [!UICONTROL  特性の管理 ] | 特性を管理するためのアクセス権。 |
| [!DNL Translation Service] | [!UICONTROL  翻訳プロジェクトの管理 ] | 翻訳プロジェクトを管理する機能 |
| [!DNL Translation Service] | [!UICONTROL  翻訳プロジェクトの表示 ] | 翻訳プロジェクトへの読み取り専用アクセス |
| [!DNL Translation Service] | [!UICONTROL  翻訳タスクの管理 ] | 翻訳タスクを管理する機能 |
| [!DNL Translation Service] | [!UICONTROL  翻訳タスクの表示 ] | 翻訳タスクへの読み取り専用アクセス |
| [!DNL Translation Service] | [!UICONTROL  翻訳レビューの管理 ] | 翻訳レビューを管理する機能。 |
| [!DNL Translation Service] | [!UICONTROL  翻訳のレビューの表示 ] | 翻訳レビューへの読み取り専用アクセス |
| [!DNL Translation Service] | [!UICONTROL  翻訳を社内で管理 ] | 翻訳を社内で管理する機能。 |
| [!DNL Translation Service] | [!UICONTROL  翻訳を社内で表示 ] | 社内翻訳への読み取り専用アクセス |
| [!DNL Translation Service] | [!UICONTROL  翻訳設定の管理 ] | 管理者が翻訳設定を管理する機能。 |
| [!DNL Translation Service] | [!UICONTROL  翻訳プロバイダーの管理 ] | 翻訳プロバイダーを管理する機能 |

## 次の手順

このガイドを読むことで、Experience Platform. のアクセス制御の主な原則を学びました。これで、[属性ベースのアクセス制御ユーザーガイド](./abac/overview.md)に進み、Experience Cloud を使用して役割を作成し、Experience Platform に権限を割り当てる方法の詳細な手順を確認できます。
