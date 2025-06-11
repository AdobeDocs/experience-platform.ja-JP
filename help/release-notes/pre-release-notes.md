---
title: Experience Platformのプレリリースノート
description: Adobe Experience Platformの最新のリリースノートのプレビュー。
hide: true
hidefromtoc: true
source-git-commit: c34c41d384fbc4f309dffa8bba97a0f6f3468efc
workflow-type: tm+mt
source-wordcount: '1480'
ht-degree: 23%

---


# Adobe Experience Platformのプレリリースノート

>[!IMPORTANT]
>
>このドキュメントは、今月のリリースノートの **プレビュー** として提供することを目的としています。 リリース項目は変更される場合があり、最終リリースで追加または削除される場合があります。

>[!TIP]
>
>その他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/releases/latest)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/en/docs/federated-audience-composition/using/release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/en/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2025年6月18日（PT）**

Adobe Experience Platformの既存の機能に対する新機能とアップデート：

- [アクセス制御](#access-control)
- [高度なデータ・ライフサイクル管理](#advanced-data-lifecycle-management)
- [ダッシュボード](#dashboards)
- [データガバナンス](#data-governance)
- [宛先](#destinations)
- [連合オーディエンス構成](#fac)
- [Privacy Service](#privacy-service)
- [クエリサービス](#query-service)
- [サンドボックス](#sandboxes)
- [ソース](#sources)

## アクセス制御 {#access-control}

Experience Platform は、[Adobe Admin Console](https://adminconsole.adobe.com) 製品プロファイルを活用し、ユーザーを権限とサンドボックスにリンクします。権限は、データモデリング、プロファイル管理、サンドボックス管理など、様々なExperience Platform機能へのアクセスを制御します。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| ダッシュボードデータの書き出し権限 | ダッシュボードの **[!UICONTROL CSV をダウンロード]** および **[!UICONTROL メールとして送信]** オプションに **[!UICONTROL ダッシュボードデータの書き出し]** 権限が必要になりました。 この権限を付与されたユーザーのみが表形式のinsight データを書き出せるので、ガバナンスポリシーとデータアクセス制御ポリシーを強化できます。 |

詳しくは、「[アクセス制御の概要](../access-control/home.md)」を参照してください。

## 高度なデータ・ライフサイクル管理 {#advanced-data-lifecycle-management}

Experience Platformは、消費者レコードとデータセットをプログラムで削除することで、保存されたデータを管理できる、一連のデータハイジーン機能を提供します。 UI のデータライフサイクルワークスペース、または Data Hygiene API への呼び出しを使用して、データストアを効果的に管理できます。 これらの機能を使用して、情報が期待どおりに使用され、必要な場合は不適切なデータの修正が更新され、組織のポリシーで必要と判断された場合は削除されるようにします。

**新しいドキュメント**

| 新しいドキュメント | 説明 |
| --- | --- |
| レコード削除一般提供 | UI または API を使用して、ID フィールドに基づく個々のレコードを削除できるようになりました。 この機能は、単一のデータセットまたはすべてのデータセットにわたる削除を許可することで、ストレージの削減、ガバナンスの実施、データハイジーンの向上に役立ちます。 ボリュームの制限と使用権限の要件が適用されます。 |

詳しくは、[ 高度なデータライフサイクル管理の概要 ](../hygiene/home.md) を参照してください。

## ダッシュボード {#dashboards}

Experience Platformでは、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを表示できる複数のダッシュボードを提供しています。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| メールとして送信の書き出しオプション | **[!UICONTROL 詳細を表示]** メニューから「**[!UICONTROL メールとして送信]**」を選択して、Query Pro モードダッシュボードから最大 10,000 件のレコードをエクスポートできるようになりました。 このオプションを選択すると、ダウンロードリンクがAdobe関連メールに安全に送信され、大量の書き出しが可能になります。 |

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../dashboards/home.md)を参照してください。

## データガバナンス {#data-governance}

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。これは、[!DNL Experience Platform] において様々なレベルで重要な役割を果たします。例えば、カタログ作成、データ系列、データ使用のラベル付け、データアクセスポリシー、マーケティングアクションのデータに関するアクセス制御などです。

**新機能**

| 機能 | 説明 |
| --- | --- |
| Azure CMK アラートと IP許可リストの設定 | Azure Key Vault のAdobeの静的 IP アドレスを許可リストして、ネットワーク制限が有効な場合に引き続きアクセスできるようになりました。 これにより、キーへのアクセスが制限されることで Platform サービスが中断されるのを防ぐことができます。 |
| CMK 設定のアラートと解決策 | Experience Platformは、Adobe サービスが Azure Key Vault へのアクセス権を失った場合（例えば、IP 許可リストエントリの削除やキーの無効化による場合）、トリガーアラートを送信するようになりました。 新しいガイドは、各アラートを理解し、是正措置を講じるのに役立ちます。 |

詳しくは、[ データガバナンスの概要 ](../data-governance/home.md) を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先**

| 宛先 | 説明 |
| --- | --- |
| アルゴリアのユーザーセグメント | アルゴリアユーザーセグメントの宛先を使用すると、マーケティング担当者は、ホームページから検索まで、サイト間で一貫したパーソナライゼーションを提供できます。 複数のデータソースからリッチオーディエンスを作成し、様々なチャネルで共有することで、ターゲティング戦略とキャンペーンのパーソナライゼーションを向上させます。 |

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| LinkedIn アカウントの有効期限に関する情報 | LinkedIn 宛先のアカウントの有効期限に関する情報が、[!UICONTROL &#x200B; 参照 &#x200B;] ビューと [!UICONTROL &#x200B; アカウント &#x200B;] ビューで直接利用できるようになりました。 以前は、この情報はドキュメントでのみ提供されていました。 この機能強化により、LinkedIn の認証ステータスと資格情報管理がよりわかりやすくなっています。 |
| Google カスタマーマッチ + DV360 の一般提供と機能強化 | Google カスタマーマッチ + DV360 の宛先を、すべてのExperience Platform ユーザーが使用できるようになりました。 ドキュメントに、AdobeとGoogle広告アカウント間のアカウントリンクに関する詳細なガイダンスが含まれるようになりました。 |
| データランディングゾーン（DLZ）宛先暗号化のサポート | データランディングゾーン宛先の暗号化のサポートを追加しました。 RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できるようになりました。 |
| エンタープライズ宛先のオーディエンスレベルの監視 | オーディエンスレベルの監視が、[[!DNL Azure Event Hubs]](/help/destinations/catalog/cloud-storage/azure-event-hubs.md)、[[!DNL HTTP API]](/help/destinations/catalog/streaming/http-destination.md)、[[!DNL Amazon Kinesis]](/help/destinations/catalog/cloud-storage/amazon-kinesis.md) のエンタープライズ宛先で使用できるようになりました。 |

{style="table-layout:auto"}

詳しくは、[ 宛先の概要 ](../destinations/home.md) を参照してください。

## 連合オーディエンス構成 {#fac}

Federated Audience Composition を使用すると、企業はデータを作成して、様々なユースケースでより優れたアプリケーションを実現できます。 この新しいアプローチでは、Adobe Real-Time Customer Data PlatformまたはAdobe Journey Optimizerのユーザーとして、既存の Data Warehouse からデータセットを直接統合して、Adobe Experience Platform オーディエンスと属性をすべて 1 つのシステムで作成および強化できます。

| 新機能 | 説明 |
| ----------- | ----------- |
| HIPAA 対応 | Federated Audience Composition は、HIPAA に準拠するようになりました。 Federated Audience Composition のプライバシーとセキュリティの対策について詳しくは、[Federated Audience Composition のプライバシーとセキュリティの概要 ](https://experienceleague.adobe.com/en/docs/federated-audience-composition/using/start/privacy-security) を参照してください。 Experience Platform製品全般に関する HIPAA コンプライアンスの詳細については、[HIPAA およびAdobe製品とサービスの概要 ](https://www.adobe.com/trust/compliance/hipaa-ready.html) を参照してください。 |

詳しくは、[Federated Audience Composition ドキュメント ](https://experienceleague.adobe.com/ja/docs/federated-audience-composition/using/home) を参照してください。

## [!DNL Privacy Service] {#privacy}

いくつかの法規制や組織の規制により、ユーザーは、要求に応じて、データストアから個人データにアクセスしたり、個人データを削除したりする権利が与えられています。 Adobe Experience Platform [!DNL Privacy Service] は、お客様からのこれらのデータリクエストを管理するのに役立つ RESTful API とユーザーインターフェイスを提供します。 [!DNL Privacy Service] を使用すると、Adobe Experience Cloud アプリケーションから非公開または個人的な顧客データに対するアクセスおよび削除のリクエストを送信でき、法的および組織のプライバシー規制への自動コンプライアンスが容易になります。

**新機能**

| 機能 | 説明 |
|--- | ---|
| テネシー州およびミネソタ州のプライバシー法のサポート | Privacy Serviceは、テネシー州情報保護法（`tipa_tn_usa`）とミネソタ州消費者データ保護法（`mcdpa_mn_usa`）をサポートするようになりました。 これらの新しい州レベルの規制に従って、アクセス要求および削除要求を処理できます。 詳しくは、[ 規制の概要 ](https://experienceleague.adobe.com/en/docs/experience-platform/privacy/regulations/overview) を参照してください。 |

このサービスについて詳しくは、[Privacy Serviceの概要 ](../privacy-service/home.md) を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用した標準 SQL を使用して、Adobe Experience Platform Data Lake でデータをクエリします。 データセットをシームレスに組み合わせ、クエリ結果から新しいデータセットを生成してレポートを強化したり、データサイエンスワークフローを有効にしたり、リアルタイム顧客プロファイルへの取り込みを容易にしたりします。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 高度な統計関数 | **Theta スケッチの交点**：近似の個別カウントおよびセット操作のために Theta スケッチを使用してセットの交点を計算する新しい関数。 **KLL ヒストグラム**:KLL （Kth minimest, L largest, Large items）スケッチを使用して、分位数の推定と分布分析のヒストグラム機能を強化しました。 これらの関数は、Data Distillerのお客様が使用できます。 |
| SQL テンプレート ライブラリ | 一般的なユースケースに対応する SQL テンプレートの包括的なライブラリが利用できるようになりました。 この機能は、頻繁な分析パターンに対するベストプラクティステンプレートを提供し、Data Distillerのお客様が複雑な分析をより効率的に実装できるよう支援することで、クエリの開発を促進します。 |

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| RFM モデリングの例 | Data Distillerのお客様向けに、包括的な最新性、頻度、通貨（RFM）モデリングの例を追加しました。 これには、RFM 手法を使用した顧客セグメント化および価値分析に関する詳細なドキュメントと実装ガイドが含まれます。 |

{style="table-layout:auto"}

[!DNL Query Service] について詳しくは、[[!DNL Query Service]  の概要](../query-service/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| オブジェクト設定の更新の移行 | 初期レプリケーション後、サンドボックス間でオブジェクト設定の反復的な更新を移行できるようになりました。 この機能強化は、サンドボックスの設定全体を再作成することなく、環境全体で設定を更新および伝播する必要がある開発ワークフローをサポートします。 |

{style="table-layout:auto"}

サンドボックスについて詳しくは、[ サンドボックスの概要 ](../sandboxes/home.md) を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Azure Synapse Analytics] の新しい認証タイプのサポート | [!DNL Azure Synapse Analytics] では、既存の接続文字列認証に加えて、サービスプリンシパル認証もサポートするようになりました。 |

**認証に関する重要な更新**

| 更新 | 説明 |
| --- | --- |
| [!DNL Salesforce] Basic Authentication の廃止 | Salesforce CRM およびSalesforce Service Cloud の基本認証は、2026 年 1 月までに廃止されます。 接続を維持するには、OAuth 2.0 認証に移行する必要があります。 この変更は両方のソースコネクタに影響し、セキュリティの向上とSalesforce認証標準への準拠を保証します。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../sources/home.md)を参照してください。
