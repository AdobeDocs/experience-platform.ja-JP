---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2020 年 11 月 11 日
doc-type: release notes
last-update: November 10, 2020
author: crhoades, ens25212
translation-type: tm+mt
source-git-commit: 5f184de8c20001f7d9a78dab17130ccadb918dfb
workflow-type: tm+mt
source-wordcount: '2049'
ht-degree: 26%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 11 月 11 日**

Adobe Experience Platform の新機能：

- [Adobe Experience PlatformData Lake移行](#migration)
- [[!DNL Access control]](#access-control)
- [[!DNL Offer Decisioning]](#offer-decisioning)
- [[!DNL Sandboxes]](#sandboxes)

既存の機能の更新：

- [[!DNL Data Prep]](#data-prep)
- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Destinations] サービス](#destinations)
- [[!DNL Intelligent Services]](#intelligent-services)
- [[!DNL Real-time Customer Profile]](#profile)
- [[!DNL Sources]](#sources)

## Adobe Experience PlatformData Lake移行 {#migration}

AdobeがData LakeをGen1からGen2に移行する間、ユーザーはData Lakeから読み取ることができますが、Data Lakeに書き込むすべての機能に影響が及びます。 Adobeは、移行の影響について詳しく説明し、特定のIMS組織の移行日時を確認するために、システム管理者に連絡します。

詳細については、『 [Data Lake移行ガイド](../../landing/adls2-gen2-migration.md)』を参照してください。

## [!DNL Access control] {#access-control}

[!DNL Experience Platform] は、[Adobe Admin Console](https://adminconsole.adobe.com) 製品プロファイルを活用し、ユーザーを権限とサンドボックスにリンクします。権限は、データモデリング、プロファイル管理、サンドボックス管理など、さまざまな Platform 機能へのアクセスを制御します。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| 権限 | In the [!DNL Admin Console], the  tab within a [!DNL Platform] product profile allows you customize which [!DNL Platform] capabilities are available for the users attached to that profile. 使用できる権限カテゴリは次のとおりです。 **[!UICONTROL Data Modeling,]****[!UICONTROL データ管理]**, **[!UICONTROL プロファイル管理]**, **[!UICONTROL Identity Management]**, **[!UICONTROL 管理，DD]**************************&#x200B;クエリMonitoring Data Monitoring Data Monitoring Data Monitoring Data Monitoring Data Monitoring Data Monitoring Data Mo Moda Mo Moder Modeling, Mo Moding Moding MongngS,, |
| サンドボックスへのアクセス |  製品プロファイル内の「**[!UICONTROL 権限]**」タブでは、特定のサンドボックスへのアクセス権をユーザーに付与できます。[!DNL Platform]詳しくは、以下の「[サンドボックス](#sandboxes)」の節を参照してください。 |

詳しくは、「[アクセス制御の概要](../../access-control/home.md)」を参照してください。

## [!DNL Offer Decisioning] {#offer-decisioning}

[!DNL Offer Decisioning] は、と統合されたApplication Serviceで [!DNL Experience Platform]す。 これにより、あらゆるタッチポイント [!DNL Platform] にわたり、最適なオファーと経験を適切なタイミングで顧客に提供できます。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| オファーライブラリの一元化 | オファーを構成する様々な要素を作成および管理し、その規則と制約を定義するインターフェイス。 |
| オファー決定エンジン | オファー判断エンジンは、オファーライブラリと共に [!DNL Platform] データと [!DNL Real-time Customer Profiles]を活用して、オファーを配信する適切な時期、顧客とチャネルを選択します。 |

詳しくは、ドキュメントを参照してくだ [[!DNL Offer Decisioning]](https://experienceleague.adobe.com/docs/offer-decisioning/using/offer-decisioning-home.html?lang=en) さい。

## [!DNL Sandboxes] {#sandboxes}

[!DNL Experience Platform] は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。In order to address this need, [!DNL Experience Platform] provides sandboxes which partition a single [!DNL Platform] instance into separate virtual environments to help develop and evolve digital experience applications.

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| 実稼働用サンドボックス | [!DNL Experience Platform] は、1 つの実稼動用サンドボックスを提供します。このサンドボックスは、削除またはリセットすることはできません。使用可能なサンドボックスの総数（実稼働環境用と非実稼働環境用）は、取得したライセンスによって決まります。 |
| 非実稼働用サンドボックス | Multiple non-production sandboxes can be created for a single [!DNL Platform] instance, allowing you to test features, run experiments, and make custom configurations without impacting your production sandbox. |
| サンドボックス切り替えボタン | In the [!DNL Experience Platform] user interface, the sandbox switcher in the top-left corner of the screen allows you to switch between available sandboxes through a dropdown menu. サンドボックス切り替えボタンは、検索機能も提供します。この機能を使用すると、使用可能なサンドボックスをフィルタリングできます。 |
| `x-sandbox-name` ヘッダー | All calls to [!DNL Experience Platform] APIs must now include the new `x-sandbox-name` header, whose value references the `name` attribute of the sandbox the operation will take place in. |

詳しくは、[サンドボックスの概要](../../sandboxes/home.md)を参照してください。

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] データエンジニアがエクスペリエンスデータモデル(XDM)との間でデータのマッピング、変換、検証を行えるようにします。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 反復演算 | [!DNL Data Prep] マッパーで、階層に対する反復操作をサポートするようになりました。 |
| マッパー関数 | [!DNL Data Prep] マッパーでは、ソースからターゲットXDMに属性を **コピーできなくなりました** 。 |

For more information, please see the [[!DNL Data Prep] overview](../../data-prep/home.md).

## Data Science ワークスペース {#dsw}

Data Science Workspaceは、機械学習と人工知能を使用して、データからインサイトを作成します。 Adobe Experience Platform に統合された Data Science Workspace は、アドビソリューションでコンテンツやデータアセットを使用して予測をおこなうことを支援します。Data Science Workspaceでこれを実現する方法の1つは、を使用することで [!DNL JupyterLab]す。 [!DNL JupyterLab] は、のWebベースのユーザーインターフェイスで、Adobe Experience Platform [[!DNL Project Jupyter]](https://jupyter.org/) と緊密に統合されています。 It provides an interactive development environment for data scientists to work with [!DNL Jupyter] notebooks, code, and data.

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| [!DNL JupyterLab] レシピビルダーテンプレート | ノートブックからレシピ要件への対応、およびバージョンの更新。 [!DNL Python] ML Runtimeの基本イメージは、3.6.7および [!DNL Python][!DNL Conda] 環境専用に更新されました。 |

詳細については、Jupyterノートブックを使用したレシピの [作成に関するドキュメントをお読みください](../../data-science-workspace/jupyterlab/create-a-recipe.md)。

## [!DNL Destinations] サービス {#destinations}

[アドビのリアルタイム顧客データプラットフォーム](../../rtcdp/overview.md) において、宛先は宛先プラットフォームとの事前に構築された統合であり、これらのパートナーに対してシームレスにデータを活用します。

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| Microsoft Bing | Microsoft Bingの宛先は、Microsoftディスプレイ広告全体でターゲットを設定したデジタルキャンペーンの再ターゲット化とオーディエンスを実行するのに役立ちます。 |
| The Trade Desk | トレードデスクは、ディスプレイ、ビデオ、モバイル在庫のソースを対象としたデジタルキャンペーンのリターゲティングとオーディエンスを、広告購入者が実行するセルフサービスプラットフォームです。 |

<!-- | Braze | Braze is a comprehensive customer engagement platform that power relevant and memorable experiences between customers and the brands they love. |  -->

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 必須フィールド | ユーザーは、必須フィールドをマークできます。必須フィールドを含むフィールドのみが書き出されるようにします。 |

<!-- | File scheduling | For both email based and cloud storage destinations, users can create a one-time export or create daily snapshots. |
| File encryption | For file based destinations, users can now add encryption to their exported files. | -->

詳しくは、『[宛先の概要](../../rtcdp/destinations/destinations-overview.md)』を参照してください。

## インテリジェントサービス {#intelligent-services}

インテリジェントサービスは、マーケティングアナリストや実践者に対して、顧客体験の使用事例で人工知能と機械学習の機能を活用する機能を提供します。これにより、マーケティングアナリストは、データ科学の専門知識を必要とせずに、ビジネスレベルの設定を使用して、会社のニーズに固有の予測を設定できます。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| コンシューマーエクスペリエンスイベント(CEE)データセット | CEEデータセットの作成時に、スキーマエディタでデータセットへのIDフィールドの追加がサポートされるようになりました。 Attribution AIAIとお客様AIは、イベントの結合に主要なIDを使用します。 |

詳しくは、『Intelligent Servicesデータ準備ガイド [』の「データセットへのIDフィールドの](../../intelligent-services/data-preparation.md#add-identity-fields-to-the-dataset) 追加」の節をお読みください。

### Attribution AI

Attribution AI はインテリジェントサービスの一部で、顧客とのやり取りの影響と増分的な効果を指定した成果に照らして計算する、マルチチャネルのアルゴリズムアトリビューションサービスです。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| データソースリンク | 元のデータセットソースへのリンクは、選択したサービスインスタンスの右側のレールから表示したり、ナビゲーションしたりできます。 |
| インスタンス名の編集 | 既存のAttribution AIインスタンスの名前を変更できるようになりました。 |
| クローンインスタンス | 現在選択されているサービスインスタンスの設定をコピーし、変更を許可します。 |
| インスタンス設定パラメーターの変更 | 既存のAttribution AIインスタンスがまだスコアリングを開始していない場合、その設定を変更できるようになりました。 |
| 1点 | Attribution AIインスタンスでアドホックモデルスコアリングをトリガーできるようになりました。 |
| 列を通る | 生の出力スコアファイルに追加される列を設定して、BIツール表示にディメンションを追加できるようになりました。 |
| インスタンスのアクティベーションとアクティベーション解除 | Attribution AIインスタンスの予定モデルトレーニングとスコアリングをアクティブ化および非アクティブ化できるようになりました。 |
| エンタイトルメントの追跡 | アカウントが消費したアトリビューションインサイトの合計数は、インスタンスを作成コンテナで確認できます。 |
| 位置によるタッチポイントの分類 | コンバージョンパスの位置によるタッチポイントの分析を提供する新しいインサイトグラフです。 |
| トップコンバージョンパス | 「パス分析ー」タブに新しいインサイトグラフが表示されます。 グラフには上位5つのコンバージョンパスのリストが含まれ、最もコンバージョン率の高いマーケティングチャネルのタッチポイントのシーケンスを示しています。 |
| タッチポイントの効果 | モデルがタッチポイントの効果を測定する際に最も重要な3つの変数について、深い洞察を提供します。 変数は、タッチされた正と負のパス、タッチポイントの効率およびタッチポイントのボリュームの比率です。 |

For more information, please read the [Attribution AI overview](../../intelligent-services/attribution-ai/overview.md).

### 顧客 AI

マーケティング担当者は、インテリジェントサービスの一部である顧客 AI を使用して、顧客予測と説明を個人レベルで生成することができます。顧客 AI は、影響力のある要因の助けを借りて、顧客が何をする可能性があるかとその理由を知ることができます。さらに、マーケターは、顧客 AI の予測と洞察を活用して、最も適切なオファーとメッセージを提供することで、顧客のエクスペリエンスをパーソナライズできます。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| データソースリンク | 元のデータセットソースへのリンクは、選択したサービスインスタンスの右側のレールから表示したり、ナビゲーションしたりできます。 |
| インスタンス名の編集 | 既存の顧客AIインスタンスの名前を変更できます。 |
| インスタンス設定パラメーターの変更 | 既存の顧客AIインスタンスがまだスコアリングを開始していない場合に、その設定を変更できるようになりました。 |
| クローンインスタンス | 現在選択されているサービスインスタンスの設定をコピーし、変更を許可します。 |
| エンタイトルメントの追跡 | アカウントの顧客AIがスコアするプロファイルの合計量は、インスタンスを作成コンテナで確認できます。 |
| 予測目標 | 「発生する」か「発生しない」かを予測する新しいオプションにより、予測目標を作成する柔軟性が向上しました。 さらに、複数のイベントを使用した場合に、イベントの「すべて」が発生するか、「いずれか」が発生するかを予測するオプションが追加されました。 |
| 影響力のある要因のドリルダウン | 傾向に最も影響を与える要因のバケツに、ドリルダウンが含まれるようになりました。 ドリルダウンは、傾向バケット内の最も影響力のある要因のそれぞれに関する値のより深いレベルの概要です。 |

For more information, please read the [Customer AI overview](../../intelligent-services/customer-ai/overview.md).

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。[!DNL Profile] 個別の顧客データを統合表示に統合し、各顧客の操作に関する実用的でタイムスタンプのあるアカウントを提供できます。

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| マージポリシーワークフローの更新 | プラットフォームは、統合ポリシー設定を新しい段階的なワークフローにアップグレードしました。 このワークフローにより、複数のプロファイルデータセットのデータフラグメントをまとめ、データセット間でのデータの結合方法を優先度を設定して、個々のデータの包括的な表示を作成できます。 適切な結合方法（タイムスタンプが並べ替えられたものか、データセットの優先順位）を選択し、ExperienceEventデータセットをプロファイルデータセットに追加することで、選択したXDM個々のプロファイルデータセットを結合できます。 |
| 和集合スキーマ表示 | Experience PlatformUIでは、和集合スキーマに貢献するすべてのスキーマとデータセットに関する情報、およびIDや関係フィールドなどの表面キー属性を、より簡単に見つけることができます。 これらの更新により、トラブルシューティングおよび検証機能が向上し、プロファイルが正しく設定され、IDが正しく関連付けられ、データが正常に取り込まれたことを確認できます。 |

For more information on Real-time Customer Profile, including tutorials and best practices for working with [!DNL Profile] data, please read the [Real-time Customer Profile overview](../../profile/home.md).

## [!DNL Sources] {#sources}

Adobe Experience Platform can ingest data from external sources while allowing you to structure, label, and enhance that data using [!DNL Platform] services. アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新しいソース**
|機能 |説明 |
| — | — |
| [!DNL Shopify] | [!DNL Shopify] APIまたはUIを使用してに接続でき [!DNL Experience Platform][!DNL Flow Service] るようになりました。 See the [Shopify connector overview](../../sources/connectors/ecommerce/shopify.md) for more information. |

**主な特長**

| 機能 | 説明 |
| ------- | ----------- |
| 接続情報の更新 | APIとUIを使用して、既存のバッチ接続の名前、説明、資格情報を更新できるようになり [!DNL Flow Service] ました。 詳しくは、Flow Service APIを使用した [接続の](../../sources/tutorials/api/update.md) 更新に関するチュートリアルおよびUIを使用したアカウントの詳細の [編集を参照してください](../../sources/tutorials/ui/monitor.md)。 |
| 接続の削除 | エラーを含むバッチ接続や不要になったバッチ接続を、 [!DNL Flow Service] APIとUIを使用して削除できるようになりました。 詳しくは、Flow Service APIを使用した接続の [削除およびUIを使用したアカウントの](../../sources/tutorials/api/delete.md) 削除に関するチュートリアルを参照してください [](../../sources/tutorials/ui/delete-accounts.md)。 |
| ストリーミングソースでのマッピングのAPIのサポート | APIを使用して、ストリーミングソースのマッピング機能を実行できるようになりました。 |
| クラウドストレージソースのカスタム区切り文字のAPIサポート | クラウドストレージソースを使用して、CSV以外の区切り形式のファイルを収集できるようになりました。 タブ、カンマ、パイプ、セミコロン、ハッシュなど、任意の1つの列区切り文字を使用して、任意の形式のフラットファイルを収集できます。 値を指定しない場合、デフォルトでコンマが使用されます。 |
| Adobe Audience ManagerコネクタのSandboxサポート | Audience Managerコネクタは、サンドボックス対応になりました。 ユーザーは、コネクタが選択したSandbox（実稼動用以外のサンドボックスを含む）にAudience Managerデータセットをルーティングできるようにできます。 設定は、IMS組織ごとに1つのサンドボックスに制限されます。 |
| UXの強化 | ソースカタログからファイルベースの取り込みにアクセスできるようになりました。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。