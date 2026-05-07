---
title: Adobe Experience Platform リリースノート 2026年4月
description: Adobe Experience Platformの2026年4月リリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: 9ebf498257378f4c5002276a84f104cf2d337601
workflow-type: tm+mt
source-wordcount: '1580'
ht-degree: 18%

---

# Adobe Experience Platform リリースノート

>[!TIP]
>
>他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/releases/latest)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/ja/docs/federated-audience-composition/using/release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/ja/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2026年4月28日**

Adobe Experience Platformの新機能と既存の機能の更新：

- [データ収集](#data-collection)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [クエリサービス](#query-service)
- [Real-Time CDP](#rtcdp)
- [サンドボックス](#sandboxes)
- [ソース](#sources)

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| ビルドの詳細を表示 | ライブラリまたは環境からビルドとビルドの詳細にアクセスして、現在ライブのビルドを表示し、コンテンツ（拡張機能、データ要素、ルール）を調べることができるようになりました。 詳しくは、[&#x200B; ビルドの概要](../../tags/ui/publishing/builds.md#build-details)を参照してください。 |

{style="table-layout:auto"}

詳しくは、[&#x200B; データ収集の概要](../../tags/home.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations]は、宛先プラットフォームとの事前定義済みの統合です。 クロスチャネルマーケティング施策、メールキャンペーン、ターゲット広告、その他多くのユースケースで、既知および未知のデータを宛先にアクティベートできます。

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} [Microsoft Ads カスタマーマッチ &#x200B;](../../destinations/catalog/advertising/microsoft-ads-customer-match.md) | 電子メールアドレスで顧客をマッチングし、検索広告やオーディエンス広告を含め、[!DNL Microsoft Advertising Network]をまたいで顧客と再エンゲージできます。 [!DNL Microsoft Advertising] アカウントをReal-Time CDPにリンクして、Experience Platformから直接カスタマーマッチリストの作成と管理を自動化します。 アクセスするには、Adobeのアカウントマネージャーにお問い合わせください。 |
| [!BADGE Beta]{type=Informative} [&#x200B; カスタムオーディエンスを編集](../../destinations/catalog/advertising/reddit-custom-audience.md) | Experience Platformから[!DNL Reddit Ads]にオーディエンスを送信します。 [!DNL Reddit] アカウントを接続し、IDをマップ化し、オーディエンスをアクティブ化して、[!DNL Reddit]で興味を積極的に探しているユーザーにリーチします。 |
| [Amazon Ads v2](../../destinations/catalog/advertising/amazon-ads-v2.md) | すべての新しい[!DNL Amazon Ads]接続に[!DNL Amazon Ads v2] カードを使用します。 [!DNL Amazon Ads v2]は[!DNL Ads Data Manager]に接続し、ID タイプの拡張、アドレス関連フィールド、データ共有を[!DNL Amazon Ads]製品でサポートします。これにより、ターゲティングとオーディエンスの一致率が向上します。 カタログ内の既存の[!DNL Amazon Ads] コネクタの名前が[&#x200B; （Legacy）  [!DNL Amazon Ads]](../../destinations/catalog/advertising/amazon-ads.md)に変更されました。 既存のレガシー接続がある場合は、必要な変更を加えずに引き続き機能します。 |
| [[!DNL Rokt]](../../destinations/catalog/advertising/rokt.md) | [!DNL Rokt]を使用して、Experience PlatformのオーディエンスをAIを活用したリアルタイムの意思決定に結びつけ、より正確なターゲティング、抑制、パーソナライズによってキャンペーンのパフォーマンスを向上させます。 |
| [Acxiom オーディエンス接続](../../destinations/catalog/advertising/acxiom-audience-connection.md) | [!DNL Acxiom Audience Connection]の宛先が一般公開されました。 [!DNL Acxiom's Real ID] テクノロジーでオーディエンスを強化し、[!DNL Altice]、[!DNL Ampersand]、[!DNL Comcast]、[!DNL Cox]、[!DNL Facebook]、[!DNL Amazon]、[!DNL Pinterest]、[!DNL Vizio]、[!DNL LG Ads]、[!DNL Spectrum]、および[!DNL Viant]にアクティベートするために使用します。 |
| [Acxiom Real ID オーディエンス接続](../../destinations/catalog/advertising/acxiom-real-id-audience-connection.md) | [!DNL Acxiom Real ID Audience Connection]の宛先が一般公開されました。 [!DNL Acxiom's Real ID]を一致キーとして[!DNL Altice]、[!DNL Ampersand]、[!DNL Comcast]、[!DNL Cox]、[!DNL Facebook]、[!DNL Amazon]、[!DNL Pinterest]、[!DNL Vizio]、[!DNL LG Ads]、[!DNL Spectrum]および[!DNL Viant]に使用してオーディエンスをアクティブ化するために使用します。 |

{style="table-layout:auto"}

**修正点および改善点**

| 修正 | 説明 |
| --- | --- |
| [Snowflake ストリーミング &#x200B;](../../destinations/catalog/warehouses/snowflake.md)宛先の新しい`TS`列 | [Snowflake ストリーミング &#x200B;](../../destinations/catalog/warehouses/snowflake.md)の宛先に、各行が最後に更新された日時を示す`TS` タイムスタンプ列が共有テーブルに含まれるようになりました。 この更新は4月末までロールアウトされます。 |
| [&#x200B; カスタム Personalization](../../destinations/catalog/personalization/custom-personalization.md)宛先の監視サポート | [&#x200B; データフロー実行ページ &#x200B;](../../dataflows/ui/monitor-destinations.md#dataflow-runs-for-streaming-destinations)に、[&#x200B; カスタム Personalization](../../destinations/catalog/personalization/custom-personalization.md)宛先の指標が表示されるようになりました。 以前は、これらの指標はこの宛先タイプでは使用できませんでした。 これらのツールを使用して、オーディエンスが期待どおりにアクティブ化していることを確認し、問題を診断します。<br> ![Dataflowは、カスタム Personalizationの宛先に対して表示される指標を実行し、アクティブ化、除外、失敗したIDを表示します。](../2026/assets/april/dataflow-run-custom-personalization.png " データフローは、カスタム Personalizationの宛先に対して指標を実行します。"){zoomable="yes"} |
| アクティベーションワークフローのレビューステップでのプロファイル数 | アクティベーションワークフローのレビューステップで、既にアクティベートされているオーディエンスのプロファイル数が表示されるようになりました。 [&#x200B; バッチ宛先](../../destinations/ui/activate-batch-profile-destinations.md)だけでなく、[&#x200B; ストリーミング宛先](../../destinations/ui/activate-segment-streaming-destinations.md)のプロファイル数も表示されます。<br> ![既にアクティブ化されたオーディエンスとストリーミング済みオーディエンスのアクティベーション ワークフローのレビュー手順に表示されるプロファイル数。アクティブ化ワークフローのレビュー手順の](../2026/assets/april/profile-count-review.png " プロファイル数。"){zoomable="yes"} |
| [!DNL Pinterest] トークンの有効期限の可視化 | [[!DNL Pinterest]](../../destinations/catalog/advertising/pinterest.md)宛先にトークンの有効期限が表示されるようになりました。これにより、再認証が必要なタイミングを確認できます。 [!DNL Pinterest] トークンは30日ごとに有効期限が切れます。 トークンの有効期限が切れると、データの書き出しが機能しなくなります。 中断を回避するには、トークンの有効期限が切れる前に[認証情報を更新します](../../destinations/catalog/advertising/pinterest.md#refresh-authentication-credentials)。 |
| 期限切れのスケジュールではファイルの書き出しが無効になりました | オーディエンススケジュールが期限切れになると、使用を試みる前に&#x200B;**[!UICONTROL Export file now]**&#x200B;が無効になり、その理由をツールヒントで説明できます。 以前は、アクションを選択するとエラーが発生していました。<br> ![&#x200B; ファイルの書き出しアクションが無効になり、そのアクションが使用できない理由を説明するツールヒントが表示されるようになりました。](../2026/assets/april/export-file-now-disabled.png " ファイルの書き出しアクションが無効になりました。"){zoomable="yes"} |
| アクティベーションワークフローの列の可視性の修正 | 1つのテーブル内の表示列を変更すると、アクティベーションワークフロー内の他のテーブルに誤って影響を与える問題を修正しました。 |

{style="table-layout:auto"}

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDMは、Experience Platformに取り込まれるデータに共通の構造と定義（スキーマ）を提供するオープンソース仕様です。 XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。 顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

| 機能 | 説明 |
| --- | --- |
| フィールドグループの使用と検出の機能強化 | フィールドグループを使用するスキーマを表示し、互換性のあるクラス、必須属性、ガバナンスラベルなどのメタデータにUIから直接アクセスできます。 また、クラスの互換性や業界タグによってフィールドグループをフィルタリングし、関連リソースをより効率的に発見して、変更する前に影響を評価することができます。 詳しくは、[&#x200B; フィールドグループの探索ガイド &#x200B;](../../xdm/ui/explore.md#explore-field-groups.md)を参照してください。 |

詳しくは、[XDMの概要](../../xdm/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用して、標準SQLを使用してAdobe Experience Platform [!DNL Data Lake]のデータをクエリします。 [!DNL Data Lake]の任意のデータセットを結合し、クエリの結果を、レポート、Data Science Workspace、またはReal-Time Customer Profileへの取り込みで使用する新しいデータセットとして取得します。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| クエリサービスセッション管理 | 使用状況と空きアイドルセッション容量を監視するために、[!UICONTROL Admin] タブからアクティブなクエリサービスセッションを表示および終了します。 これにより、管理者は、非アクティブなセッションから容量を再利用して、信頼性の高いData Distiller ワークフローを維持できます。 詳しくは、[&#x200B; クエリサービスの管理セッションのガイド &#x200B;](../../query-service/ui/session-management.md)を参照してください。 |

{style="table-layout:auto"}

詳しくは、[&#x200B; クエリサービスの概要](../../query-service/home.md)を参照してください。

## Real-Time CDP {#rtcdp}

Real-Time CDPは、複数のチャネルをまたいでデータをリアルタイムで取り込み、処理、活用することで、統合された実用的な顧客プロファイルを提供します。 Real-Time CDPなら、Experience Platform内から直接、既存のデータソースを連携し、豊富なオーディエンスを構築して活用し、あらゆる配信先をまたいでプライバシーに準拠したアクティベーションを実現できます。 これにより、マーケター、アナリスト、IT部門は、シームレスなクロスチャネルのマーケティングキャンペーンを通じて、詳細にパーソナライズされたタイムリーな顧客体験を提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Real-Time CDP MCP （Beta） | [Real-Time CDP MCP](../../rtcdp/rtcdp-mcp.md)を使用して、Real-Time CDPをAI エージェントおよびMCP互換クライアントに取り込み、ネイティブ LLM エクスペリエンスを通じてReal-Time CDP ツールと直接やり取りできるようにします。 MCP互換クライアント（Claude、ChatGPT、Claude Code、Codex、Cursor、VS Codeなど）をAdobe担当者が提供するエンドポイントに接続することで、Experience Platform REST API呼び出しの記述や複数のUI ワークフローの操作なしに、自然言語を使用してオーディエンス、宛先設定、アクティベーション実行履歴を調べることができます。 ブラウザーベースのAdobe ログインを完了すると、次のようなツールへの読み取り専用アクセス権が付与されます。 <ul><li>既存オーディエンスの検索</li><li>オーディエンスメンバーシップのプレビュー</li><li>宛先タイプのリスト</li><li>設定済みアカウントのリスト</li><li>設定済み宛先のリスト</li><li>Sourceとの連携のリスト</li><li>ターゲット接続のリスト</li><li>アクティブ化実行の検査</li></ul>. アクションが組織とサンドボックスにスコープされるように、各リクエストには`imsOrgId`と`sandboxName`のパラメーターが必要です。 **注意**：このBeta リリースでは、書き込み操作はサポートされていません。 |

{style="table-layout:auto"}

詳しくは、[Real-Time CDPの概要](../../rtcdp/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。 企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Express Copy | Express Copyを使用して、[&#x200B; サンドボックスツール UI](/help/sandboxes/ui/sandbox-tooling.md#express-copy)から1回のアクションでターゲットサンドボックスにオブジェクトをコピーします。 依存オブジェクトは自動的に検出され、ターゲットサンドボックスに作成されるか、すでに存在する場合に再利用されます。 |

{style="table-layout:auto"}

詳しくは、[&#x200B; サンドボックスの概要](../../sandboxes/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。 これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新規または更新されたソース**

| ソース | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} [!DNL Talon.One] | Experience Platformの[[!DNL Talon.One] source](../../sources/connectors/loyalty/talon-one.md)が、バッチ モードとストリーミング モードの両方で利用できるようになりました。 [[!DNL Talon.One Batch Source Connector]](../../sources/tutorials/ui/create/loyalty/talon-one-batch.md)を使用して、閉じたセッションと過去のロイヤルティ取引を定期的に取り込み、[[!DNL Talon.One Streaming Events]](../../sources/tutorials/ui/create/loyalty/talon-one-streaming.md) ソースを使用して、[!DNL Talon.One] イベントをほぼリアルタイムでExperience Platformに取り込みます。 Real-Time CDP、Adobe Journey Optimizer、Offer Decisioningをまたいで、[!DNL Talon.One]件のロイヤルティデータの読み込みと活用が容易になります。 |
| SOQLを使用した[!DNL Salesforce]の行レベルのフィルタリングのサポート | [!DNL Salesforce] Object Query Language （SOQL） フィルターを[!DNL Salesforce] ソース接続に直接適用できるようになりました。これにより、行レベルのデータを制限してからExperience Platformに取り込むことができます。 機能を使用して、以下を行います。 <ul><li>Salesforce オブジェクトに対してSOQL where-clause スタイル条件を定義します（例えば、メール！=nullのリードや特定のステージの商談のみ）</li><li>取り込み量を基準を満たす行のみに制限することで、不要なデータの移動、ストレージ、下流工程の処理を削減します</li><li>ソースからExperience Platformに取り込まれるレコードを制御することで、Experience Platformの取り込みとCRM データアクセスおよびコンプライアンスのルールをより密接に連携できます</li></ul>. 詳しくは、[&#x200B; ソースの行レベルのフィルタリング &#x200B;](../../sources/tutorials/api/filter.md)に関するガイドを参照してください。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。

<!--

| Data Distiller Accelerators | Run and schedule Adobe-managed, parameterized SQL templates in the Query Service UI to perform common analyses without writing SQL. This helps you standardize analytics workflows and reuse trusted query logic across your organization. See the [Data Distiller accelerators guide](../../query-service/ui/accelerators.md) for more details. |

| Automatic dataflow disabling | Sources ingestion dataflows that fail continuously for 30 days are automatically disabled, helping to surface unhealthy dataflows and reduce repeated failed runs. |

--->