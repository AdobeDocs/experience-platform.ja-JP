---
title: Experience Platform プレリリースノート
description: Adobe Experience Platformの最新のリリースノートのプレビュー。
exl-id: f2c41dc8-9255-4570-b459-4f9fc28ee58b
source-git-commit: 9b191535ba96c8791a4528361a1945ae27c6456c
workflow-type: tm+mt
source-wordcount: '1428'
ht-degree: 17%

---

# Adobe Experience Platform プレリリースノート

>[!IMPORTANT]
>
>このドキュメントは、今月のリリースノートの&#x200B;**プレビュー**&#x200B;として作成されます。 リリース項目は変更される可能性があり、最終リリースで追加または削除される場合があります。

>[!TIP]
>
>他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/releases/latest)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/en/docs/federated-audience-composition/using/release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/en/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2026年4月**

Adobe Experience Platformの新機能と既存の機能の更新：

- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [クエリサービス](#query-service)
- [Real-Time CDP](#rtcdp)
- [サンドボックス](#sandboxes)
- [セグメント化サービス](#segmentation-service)
- [ソース](#sources)

## 宛先 {#destinations}

[!DNL Destinations]は、Experience Platformからのデータのシームレスなアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} [Microsoft Ads カスタマーマッチ &#x200B;](../destinations/catalog/advertising/microsoft-ads-customer-match.md) | 電子メールアドレスで顧客をマッチングし、検索広告やオーディエンス広告を含め、[!DNL Microsoft Advertising Network]をまたいで顧客と再エンゲージできます。 [!DNL Microsoft Advertising] アカウントをReal-Time CDPにリンクして、Experience Platformから直接カスタマーマッチリストの作成と管理を自動化します。 アクセスするには、Adobeのアカウントマネージャーにお問い合わせください。 |
| [!BADGE Beta]{type=Informative} [&#x200B; カスタムオーディエンスを編集](../destinations/catalog/advertising/reddit-custom-audience.md) | Experience Platformから[!DNL Reddit Ads]にオーディエンスを送信します。 [!DNL Reddit] アカウントを接続し、IDをマップ化し、オーディエンスをアクティブ化して、[!DNL Reddit]で興味を積極的に探しているユーザーにリーチします。 |
| [Amazon Ads v2](../destinations/catalog/advertising/amazon-ads-v2.md) | [!DNL Amazon Ads v2]は、すべての新しい[!DNL Amazon Ads]接続の現在の宛先です。 既存の[&#x200B; （レガシー）  [!DNL Amazon Ads]](../destinations/catalog/advertising/amazon-ads.md)接続がある場合、必要な変更を加えずに引き続き機能します。 [!DNL Amazon Ads v2]は[!DNL Ads Data Manager]に接続します。これにより、[!DNL Amazon Ads]製品でのID タイプの拡張、アドレス関連フィールド、データ共有がサポートされ、[&#x200B; （レガシー）  [!DNL Amazon Ads]](../destinations/catalog/advertising/amazon-ads.md)と比較して、ターゲティングとオーディエンスの一致率が向上します。 |
| [!DNL Rokt] | [!DNL Rokt]を使用して、Experience PlatformのオーディエンスをAIを活用したリアルタイムの意思決定に結びつけ、より正確なターゲティング、抑制、パーソナライズによってキャンペーンのパフォーマンスを向上させます。 |
| [Criteo](../destinations/catalog/advertising/criteo.md)の外部オーディエンスのサポート | カスタムアップロードオーディエンス（CSVからインポート）、類似オーディエンス、連合オーディエンス、[!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリで作成されたオーディエンスを含む、セグメント化サービスから[!DNL Criteo]までのオリジンからオーディエンスをアクティベートします。 詳しくは、[&#x200B; サポートされているオーディエンス &#x200B;](../destinations/catalog/advertising/criteo.md#supported-audiences)の節を参照してください。 |
| [Acxiom オーディエンス接続](../destinations/catalog/advertising/acxiom-audience-connection.md) | [!DNL Acxiom Audience Connection]の宛先が一般公開されました。 [!DNL Acxiom's Real ID] テクノロジーでオーディエンスを強化し、[!DNL Altice]、[!DNL Ampersand]、[!DNL Comcast]、[!DNL Cox]、[!DNL LG Ads]、[!DNL Spectrum]および[!DNL Viant]などの追加のプラットフォームにアクティベートするために使用します。 |
| [Acxiom Real ID オーディエンス接続](../destinations/catalog/advertising/acxiom-real-id-audience-connection.md) | [!DNL Acxiom Real ID Audience Connection]の宛先が一般公開されました。 このツールを使用すると、[!DNL Altice]、[!DNL Ampersand]、[!DNL Comcast]、[!DNL Cox]、[!DNL LG Ads]、[!DNL Spectrum]および[!DNL Viant]など、サポートされている同じプラットフォームのセットで一致キーとして[!DNL Acxiom's Real ID]を使用してオーディエンスをアクティブ化できます。 |

{style="table-layout:auto"}

**修正点および改善点**

| 修正 | 説明 |
| --- | --- |
| カスタム Personalization モニタリングのサポート | 宛先の監視ダッシュボードで[!DNL Custom Personalization]宛先がサポートされるようになりました。 監視から[!DNL Custom Personalization]を除外した制限事項が削除されました。 |
| アクティベーションレビューでのプロファイル数 | アクティベーションレビュー手順で、既にアクティベートされているオーディエンスのプロファイル数が表示されるようになりました。 プロファイル数は、バッチ宛先だけでなく、ストリーミング宛先にも表示されます。 |
| [!DNL Pinterest] トークンの有効期限の可視化 | [!DNL Pinterest]宛先は、[!DNL Pinterest]から直接返されたトークンの有効期限を表示するようになりました。これにより、再認証が必要な場合に確認できます。 |
| 無効なスケジュールに対するファイルの書き出しが無効になりました | オーディエンススケジュールが無効または古い場合、**[!UICONTROL Export file now]** アクションが無効になりました。 ツールヒントでは、アクションが使用できない理由を説明します。 |
| アクティベーションワークフローの列の可視性の修正 | 1つのテーブル内の表示列を変更すると、アクティベーションワークフロー内の他のテーブルに誤って影響を与える問題を修正しました。 |

{style="table-layout:auto"}

詳しくは、[宛先の概要](../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDMは、Experience Platformに取り込まれるデータの一般的な構造と定義（スキーマ）を提供するオープンソース仕様です。 XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| フィールドグループスキーマ使用状況の可視化 | どのスキーマが詳細ページのフィールドグループを使用しているかを表示し、スキーマメタデータを含む並べ替え可能なダイアログでそれらを探索します。 これにより、他社に後れを取ることなく、依存関係と影響を迅速に評価できます。 |

{style="table-layout:auto"}

詳しくは、[XDM システムの概要](../xdm/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用して、標準SQLを使用してAdobe Experience Platform [!DNL Data Lake]のデータをクエリします。 [!DNL Data Lake]の任意のデータセットを結合し、クエリの結果を、レポート、Data Science Workspace、またはReal-Time Customer Profileへの取り込みで使用する新しいデータセットとして取得します。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Data Distiller Accelerator | Adobeで管理され、パラメータ化されたSQL テンプレートをQuery Service UIで実行およびスケジュールして、SQLを記述することなく一般的な分析を実行できます。 これにより、分析ワークフローを標準化し、組織全体で信頼できるクエリロジックを再利用することができます。 |

{style="table-layout:auto"}

詳しくは、[&#x200B; クエリサービスの概要](../query-service/home.md)を参照してください。

## Real-Time CDP {#rtcdp}

[!DNL Real-Time CDP]は、複数のチャネルをまたいでデータをリアルタイムで取り込み、処理、アクティブ化することで、統合された実用的な顧客プロファイルを提供します。 Real-Time CDPなら、Experience Platform内から直接、既存のデータソースを連携し、豊富なオーディエンスを構築して活用し、あらゆる配信先をまたいでプライバシーに準拠したアクティベーションを実現できます。 これにより、マーケター、アナリスト、IT部門は、シームレスなクロスチャネルのマーケティングキャンペーンを通じて、詳細にパーソナライズされたタイムリーな顧客体験を提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Real-Time CDP MCP （Beta） | Real-Time CDP MCPを使用して、Real-Time CDPをAI エージェントおよびMCP互換クライアントに取り込み、ネイティブのLLM エクスペリエンスを通じてReal-Time CDPツールと直接やり取りできるようにします。 MCP互換クライアント（Claude、ChatGPT、Claude Code、Codex、Cursor、VS Codeなど）をAdobe担当者が提供するエンドポイントに接続することで、Experience Platform REST API呼び出しの記述や複数のUI ワークフローの操作なしに、自然言語を使用してオーディエンス、宛先設定、アクティベーション実行履歴を調べることができます。 ブラウザーベースのAdobe ログインを完了すると、次のようなツールへの読み取り専用アクセス権が付与されます。 <ul><li>既存オーディエンスの検索</li><li>オーディエンスメンバーシップのプレビュー</li><li>宛先タイプのリスト</li><li>設定済みアカウントのリスト</li><li>設定済み宛先のリスト</li><li>Sourceとの連携のリスト</li><li>ターゲット接続のリスト</li><li>アクティブ化実行の検査</li></ul>. アクションが組織とサンドボックスにスコープされるように、各リクエストには`imsOrgId`と`sandboxName`のパラメーターが必要です。 このBeta リリースでは、書き込み操作はサポートされていません。 |

{style="table-layout:auto"}

詳しくは、[Real-Time CDPの概要](../rtcdp/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。 企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Express Copy | Express Copyを使用して、[&#x200B; サンドボックスツール UI](/help/sandboxes/ui/sandbox-tooling.md#express-copy)から1回のアクションでターゲットサンドボックスにオブジェクトをコピーします。 依存オブジェクトは自動的に検出され、ターゲットサンドボックスに作成されるか、すでに存在する場合に再利用されます。 |

{style="table-layout:auto"}

詳しくは、[&#x200B; サンドボックスの概要](../sandboxes/home.md)を参照してください。

## セグメント化サービス {#segmentation-service}

セグメンテーションサービスを利用して、顧客データからオーディエンスを構築し、Experience Platformでライフサイクル全体を管理できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| ストリーミングセグメント化モニタリング | サンドボックス、データセット、セグメントレベルでの評価率、取り込み遅延、データ品質指標をリアルタイムで可視化し、ストリーミングセグメンテーションを監視します。 評価レート、P95取り込み待ち時間、受信したレコード、評価されたレコード、失敗したレコード、スキップされたレコードなどの指標を表示します。 また、セグメントごとに適格および失格の純新規プロファイルも表示します。 これらのインサイトを活用して、データに影響を与える前にキャパシティ違反と取り込み問題を特定します。 |

{style="table-layout:auto"}

詳しくは、[&#x200B; オーディエンスの概要](../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。 これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新規または更新されたソース**

| ソース | 説明 |
| --- | --- |
| 自動データフロー無効化 | 30日間連続して失敗するソース取り込みデータフローは自動的に無効になり、不健全なデータフローを表示し、繰り返し失敗する実行を減らすことができます。 |
| [!DNL Delta Sharing] | [!DNL Delta Sharing] ソースを使用すると、安全でオープンなデータ共有プロトコルを通じて、Delta テーブルをExperience Platformに取り込むことができます。 [!DNL Delta Sharing]接続を設定し、取り込む共有とテーブルを選択すると、Platformはそのデータを自動的にデータセットに取り込むため、分析、セグメント化、アクティベーションに使用できます。 |
| [!DNL Meta Ads]（ベータ版） | ソース ワークスペースの[!DNL Meta Ads] ソースコネクタ （Beta）を使用して、[!DNL Meta]への認証、広告アカウントの選択、キャンペーンおよびパフォーマンスデータのExperience Platform データセットへの取り込みのスケジュール設定を行うことができます。[!DNL Meta Ads] |
| [!DNL Talon.One] | 新しい[!DNL Talon.One] バッチおよびストリーミングソースを使用して、Experience Platformを[!DNL Talon.One]に接続できるようになりました。 新しいソースを使用して、ロイヤルティプロファイルデータとトランザクションおよびロイヤルティアクティビティイベントをExperience Platformに取り込みます。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../sources/home.md)を参照してください。
