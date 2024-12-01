---
title: Adobe Experience Platform リリースノート 2024年11月
description: Adobe Experience Platform の 2024年11月 のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: 3f43e120225bcca640cc46ebdce1e4d61100ad45
workflow-type: tm+mt
source-wordcount: '852'
ht-degree: 19%

---

# Adobe Experience Platform リリースノート

>[!TIP]
>
>新しい [AI アシスタント製品ドキュメント ](../../ai-assistant/landing.md) が利用できるようになりました。 このページを、AI アシスタント関連のすべてのリソースのハブとして使用します。

**リリース日：2024年11月26日（PT）**

Adobe Experience Platformの既存の機能およびドキュメントのアップデート：

- [AI アシスタント](#ai-assistant)
- [宛先](#destinations)
- [クエリサービス](#query-service)
- [サンドボックス](#sandboxes)
- [ドキュメントの更新](#documentation-updates)
   - [インタラクティブExperience PlatformAPI ドキュメント](#interactive-experience-platform-api-documentation)
   - [Experience Leagueの新しい目次](#new-table-of-contents-on-experience-league)
   - [新しい AI アシスタントのランディングページ](#new-ai-assistant-landing-page)

## AI アシスタント {#ai-assistant}

Adobe Experience Platformの AI アシスタントは、Adobeアプリケーションのワークフローを高速化するために使用できる対話型エクスペリエンスです。 AI アシスタントを使用すると、製品の知識をより深く理解したり、問題をトラブルシューティングしたり、情報を検索して運用インサイトを見つけたりできます。 AI アシスタントは、Experience Platform、Real-time Customer Data Platform、Adobe Journey Optimizer、およびCustomer Journey Analyticsをサポートします。

**新機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Alpha]{type=Informative} 大幅な変化を監視し、オーディエンスの増加を予測します | AI アシスタントを使用して、有意な変化を監視し、オーディエンスとデータセットのサイズの成長予測を提供します。 その後、この情報を使用して、オーディエンスデータの整合性を確保し、データに基づいた意思決定をサポートするための将来的な予測を提供できます。 詳しくは、[ 重要な変更の監視およびオーディエンスの増加の予測 ](../../ai-assistant/new-features/audience-forecasting.md) に関するガイドを参照してください。 |
| [!BADGE Alpha]{type=Informative} 自然言語の推定 | AI アシスタントの自然言語推定機能を使用して、簡単で会話的な質問に基づいてオーディエンスのサイズを推定し、オーディエンスの傾向を予測します。 詳しくは、[AI アシスタントでの自然言語推定の使用 ](../../ai-assistant/new-features/natural-language.md) に関するガイドを参照してください。 |

{style="table-layout:auto"}

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先** {#new-updated-destinations}

| 宛先 | 説明 |
| --- | --- |
| [Magnite ストリーミングリアルタイム ](/help/destinations/catalog/advertising/magnite-streaming.md) | Magnite ストリーミングプラットフォームでアクティブ化、ターゲティングまたは抑制するためにオーディエンスを書き出します。 オーディエンスを Magnite に正しく書き出すには、リアルタイム宛先とバッチ宛先の両方を使用する必要があります。 |
| [Magnite ストリーミングバッチ ](/help/destinations/catalog/advertising/magnite-batch.md) | Magnite ストリーミングプラットフォームでアクティブ化、ターゲティングまたは抑制するためにオーディエンスを書き出します。 オーディエンスを Magnite に正しく書き出すには、リアルタイム宛先とバッチ宛先の両方を使用する必要があります。 |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| --- | --- |
| [ エッジでのプロファイル属性のリアルタイム検索 ](/help/destinations/ui/activate-edge-profile-lookup.md) | カスタム Personalizationの宛先とEdge NetworkAPI を使用して、リアルタイムでエッジプロファイル属性を検索し、ダウンストリームアプリケーションを通じてパーソナライゼーションエクスペリエンスを提供したり、意思決定ルールを通知したりする方法を説明します。 |

{style="table-layout:auto"}

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用した標準 SQL を使用して、Adobe Experience Platform Data Lake でデータをクエリします。 データセットをシームレスに組み合わせ、クエリ結果から新しいデータセットを生成してレポートを強化したり、データサイエンスワークフローを有効にしたり、リアルタイム顧客プロファイルへの取り込みを容易にしたりします。 例えば、顧客トランザクションデータを行動データと結合して、ターゲットマーケティングキャンペーンの高価値オーディエンスを特定できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Dater Distiller Authorization API | クエリサービスサンドボックスの IP ベースのアクセス制限を管理および適用して、データのセキュリティを強化し、組織ポリシーへのコンプライアンスを確保します。 主な特長と機能について詳しくは、[Data Distiller Authorization API ガイド ](../../query-service/auth-api/overview.md) を、エンドポイントの詳細、パラメーターリスト、リクエスト/応答の例、テスト機能などの包括的な情報については、[OpenAPI ドキュメント ](https://developer.adobe.com/experience-platform-apis/references/data-distiller-auth/) を参照してください。 |

[!DNL Query Service] について詳しくは、[[!DNL Query Service]  の概要](../../query-service/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対応するために、Experience Platformでは、1 つの Platform インスタンスを別々の仮想環境に分割するサンドボックスを提供し、デジタルエクスペリエンスアプリケーションの開発と発展を支援しています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| サンドボックスツール API を使用したパッケージ共有 | [`/handshake`](../../sandboxes/sandbox-tooling-api/packages.md#org-linking) と [`/transfers`](../../sandboxes/sandbox-tooling-api/packages.md#transfer-packages) という 2 つの新しい API エンドポイントを使用して、組織間でのパッケージ共有を処理します。例えば、サンドボックスツール API を使用して、リクエスト承認、パッケージ表示、パッケージの読み込みなどです。 |

サンドボックスについて詳しくは、[ サンドボックスの概要 ](../../sandboxes/home.md) を参照してください。

## ドキュメントの更新 {#documentation-updates}

### インタラクティブExperience PlatformAPI ドキュメント {#interactive-api-documentation}

[Experience PlatformAPI ドキュメント ](https://developer.adobe.com/experience-platform-apis/) が完全にインタラクティブになり、API リファレンスドキュメントページで直接認証と API の探索ができるようになりました。 これで、目的の API リファレンスドキュメントページに移動し、API 認証資格情報を作成または取得して、**[!UICONTROL 試す]** ブロックに貼り付け、呼び出しを実行できます。 1 つのページ上のすべて。 機能について ](/help/landing/api-authentication.md#get-credentials-functionality) 詳細を参照 [。

### Experience Leagueの新しい目次 {#new-table-of-contents-on-experience-league}

Experience Leagueドキュメントページの目次が改善され、読者のエクスペリエンスが向上しました。必要な正確なページを検出するためのキーワードフィルター、すべてのページを展開する機能などが追加されました。<br> ![ キーワードフィルターや、すべてのページを展開する機能など、新しい目次エクスペリエンスが追加されました。](../2024/assets/november/new-toc-experience.gif " キーワードフィルターや、すべてのページを展開する機能を含む新しい目次エクスペリエンス "){width="250" align="center" zoomable="yes"}。

### 新しい AI アシスタントのランディングページ {#new-ai-assistant-landing-page}

新しい [AI アシスタント製品ドキュメント ](../../ai-assistant/landing.md) ページを、あらゆる AI アシスタントのハブとして使用します。 製品ドキュメントのビデオ チュートリアル、技術ドキュメント、使用例、および AI アシスタントに関するブログ投稿へのリンクを参照してください。