---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2019 年 11 月 18 日
doc-type: release notes
last-update: November 18, 2019
author: crhoades, ens28527
translation-type: tm+mt
source-git-commit: 5f120a716cc3396ef7749463bb6052a8ced2fbb4
workflow-type: tm+mt
source-wordcount: '1878'
ht-degree: 67%

---


# Adobe Experience Platform リリースノート

**リリース日：2019 年 11 月 18 日**

Adobe Experience Platform の新機能：
* [[!DNL Real-time Customer Data Platform]](#rtcdp)
* [[!DNL Destinations]](#destinations)
* [[!DNL Sources]](#sources)

既存の機能の更新：
* [[!DNL Data Science Workspace]](#dsw)
* [[!DNL Experience Data Model (XDM) System]](#xdm)
* [[!DNL Real-time Customer Profile]](#profile)
* [[!DNL Segmentation Service]](#segmentation)

## [!DNL Real-time Customer Data Platform] {#rtcdp}

 Experience Platform をベースに構築されたアドビのリアルタイム顧客データプラットフォーム（リアルタイム CDP）は、企業が既知のデータと未知のデータを統合し、カスタマージャーニーを通じてインテリジェントな判定を行って顧客プロファイルをアクティブ化するのに役立ちます。リアルタイム CDP は、複数の大規模法人データソースを組み合わせて、1 対 1 のパーソナライズされた顧客体験をすべてのチャネルとデバイスにわたって提供するために使用できる、統合されたプロファイルを作成します。

[!DNL Real-time Customer Data Platform] データ管理、ID管理、高度な分類、およびデータ科学のツールが含まれ、プロファイルの構築やオーディエンスの定義ができ、また、厳密なデータ管理ポリシーを適用できる豊富なインサイトの抽出が可能です。

Adobe Experience Cloud とのネイティブな統合に加え、アドビはパートナーの大規模なエコシステムにリンクし、オンサイトやアプリケーション内のパーソナライズ機能から電子メール、有料メディア、コールセンター、接続されたデバイスなど、あらゆるチャネルにわたって、優れた顧客体験を提供できます。

リアルタイム CDP を使用すると、次のことが可能になります。

* 大規模法人の全体から顧客データをストリーミング収集して、顧客を一目で把握する。
* 既知の識別子と未知の識別子に対して、信頼されたガバナンスとプライバシーコントロールを使用して、プロファイルを責任を持って管理する。
* Adobe Sensei による AI や機械学習を活用して、マーケティング担当者向けに構築された実用的なインサイトを生み出し、オーディエンスを拡大縮小する。
* すべてのチャネルと宛先にわたって、パーソナライズされたエクスペリエンスをリアルタイムで提供する。

For more information, see the [Real-time Customer Data Platform documentation](../../rtcdp/overview.md).

**主な特長**

| 機能 | 説明 |
|---|---|
| 宛先 | Pre-built integrations with destination platforms supported by Adobe’s [!DNL Real-time Customer Data Platform] that activate data to those partners in a seamless way. 詳しくは、以下の「[宛先](#destinations)」を参照してください。 |
| ホームページの指標ダッシュボード | Real-time Customer Data Platform(Real-time CDP)ホームページには、プロファイルとセグメントに関する情報を示す指標ダッシュボードが含まれています。 このホームページには、学習教材へのリンクも含まれています。以下の「[リアルタイム顧客データプラットフォームの指標](#real-time-customer-data-platform-metrics)」の節を参照してください。 |
| ソース | アドビソリューション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取り込むことができます。詳しくは、以下の「[ソース](#sources)」の節を参照してください。 |

**[!DNL Real-time Customer Data Platform]指標**

指標ダッシュボードを含むReal-time Customer Data Platform(Real-time CDP)ホームページは、Real-time CDPにログインすると表示されます。

ホームページは、指標カードが表示される場所の 1 つに過ぎません。リアルタイム CDP は、ユーザーの経験を通じて指標カードを提供します。これらの指標は、システム内のデータ、プロファイル、セグメントオーディエンスに関する情報を提供します。

リアルタイム CDP にログインしたときにシステムにデータがない場合、ホームページのダッシュボードは表示されません。この場合、ホームページでは初めて使用するユーザーのための学習教材を提供します。データが収集されると、ダッシュボードが自動的に更新され、そのデータに関する情報が表示されます。

詳しくは、「[リアルタイム顧客データプラットフォームの指標の概要](../../rtcdp/home-page-dashboards.md)」を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobeのリアルタイム顧客データプラットフォームがサポートする目的のプラットフォームとの事前に構築された統合です。この統合により、パートナーに対するデータのアクティブ化がシームレスに行われます。 詳しくは、「[宛先の概要](../../destinations/home.md)」の記事を参照してください。

**使用可能な宛先**

11 月のリリースでは、アドビのリアルタイム顧客データプラットフォームは次の宛先をサポートします。

* 広告: [!DNL Google]
* 電子メールマーケティング：Adobe Campaign, [!DNL Salesforce Marketing Cloud], [!DNL Responsys], [!DNL Oracle Eloqua]

各宛先について詳しくは、「[宛先カタログ](../../destinations/catalog/overview.md)」を参照してください。

**既知の制限事項**

* 初期リリースでは、[アクティベーションフロー](../../destinations/ui/activate-destinations.md#activate-data)（スケジュール手順）のカスタムアクティベーションスケジュールを許可するコントロールは使用できません。
* 現在、宛先設定を編集または削除する方法はありません。この制限を回避するには、[宛先の詳細ページ](../../destinations/ui/destination-details-page.md)の右上にある宛先を有効または無効にします。
* 現在、宛先またはストレージアカウントに接続する際に、アカウントの詳細、パス、または資格情報は検証されません。正しい資格情報を入力していることを確認し、スペルミスや入力ミスをもう一度確認してください。
* 初期リリースでは、資格情報の更新は行われません。アカウントの有効期限が切れるか、更新が必要な場合は、新しい宛先接続を作成し、以前にマッピングしたセグメントを再マッピングする必要があります。

## ソース {#sources}

Adobe Experience Platform can ingest data from external sources while allowing you to structure, label, and enhance that data using [!DNL Platform] services. アドビソリューション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、ストレージシステムと CRM サービスに対する認証、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**主な特長**

| 機能 | 説明 |
| ---------- | ------------ |
| ソース UI | ソース接続を作成、表示、および管理するための新しいユーザーインターフェイス。 |
| CRM コネクタの改良されたワークフロー | New intuitive UI workflow for creating and managing [!DNL Microsoft Dynamics] and [!DNL Salesforce] connectors. |
| クラウドベースのストレージ用のコネクタのサポート | コネクタがクラウドベースのストレージにアクセスできるようになりました。New sources include [!DNL Amazon S3], [!DNL Azure Blob], and FTP/SFTP servers. |

**既知の問題**

* クラウドベースのストレージ用のソースコネクタは、圧縮ファイルの取得をサポートしていません。

ソースについて詳しくは、「[ソースの概要](../../sources/home.md)」を参照してください。

## [!DNL Data Science Workspace] {#dsw}

Adobe Experience Platform [!DNL Data Science Workspace] enables data scientists to seamlessly generate insights from data and content across Adobe applications and third-party systems by building and operationalizing Machine Learning Models. [!DNL Data Science Workspace] は、XDMデータの調査と準備を含む、エンドツーエンドのデータ科学ライフサイクルと緊密に統合され、さらにモデルの開発と運用を強化して、機械学習インサイトを自動的に強化 [!DNL Platform][!DNL Real-time Customer Profile] します。

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| Data access using [!DNL Platform] SDK | Pre-built Recipes and launcher notebooks in [!DNL Python] now use [!DNL Platform] SDK for accessing data. |
| サンドボックスのサポート | ノートブックやレシピを開発用サンドボックスや実稼働用サンドボックスに分離する機能など、今後のサンドボックス機能（現在はベータ版）のサポート。詳しくは、「[サンドボックスの概要](../../sandboxes/home.md)」を参照してください。 |

詳しくは、「[Data Science Workspace　の概要](../../data-science-workspace/home.md)」を参照してください。

## [!DNL Experience Data Model] (XDM)システム {#xdm}

Standardization and interoperability are key concepts behind [!DNL Experience Platform]. [!DNL Experience Data Model] (XDM)は、Adobeに基づいて、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠することで、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| ---------- | ------------ |
| 通知スキーマ | データ取得プロセス中に送信される通知データを表す新しいスキーマ。 |
| Adobe AdCloud DSP スキーマ | Adobe Advertising Cloud ディスプレイ広告（DSP）メタデータを表すための 5 つの新しいスキーマ（配置、キャンペーン、パッケージ、広告主、アカウント）が追加されました。 |
| ExperienceEvent の実装の詳細 Mixin | イベントの収集に使用するソフトウェアに関する情報を保存するための標準フィールドを追加する新しい ExperienceEvent Mixin。 |
| [!DNL Profile Privacy] ミックスイン | New profile mixin that adds fields to accept general out-out and sales/sharing opt-out signals for [!DNL Real-time Customer Profile]. |
| `xdm:alternateDisplayInfo` の形式制限 | `xdm:alternateDisplayInfo` の「タイトル」フィールドと「説明」フィールドは、両方とも検証に合格する文字列である必要があります。 |
| 名前の変更：XDM [!DNL Individual Profile] | The &quot;title&quot; of the &quot;XDM [!DNL Profile]&quot; class has been updated to &quot;XDM [!DNL Individual Profile]&quot;. クラスの公式 `$id` は変更されていません。 |

**既知の問題**

* なし。

To learn more about working with XDM using the [!DNL Schema Registry] API and [!DNL Schema Editor] user interface, please read the [XDM System documentation](../../xdm/home.md).

## [!DNL Real-time Customer Profile] {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。With [!DNL Real-time Customer Profile], you can see a holistic view of each individual customer that combines data from multiple channels, including online, offline, CRM, and third party data. [!DNL Profile] 個別の顧客データを統合表示に統合し、各顧客の操作に関する実用的でタイムスタンプのあるアカウントを提供できます。

| 機能 | 説明 |
| -----------| ---------- |
| Enhancements to [!DNL Profile] lookup | 参照記述子と関連エンティティを使用してプロファイルを検索できるようになりました。 |
| 指定したデータセットのデータのクリーンアップ | Users can now delete data for a given dataset or batch using the [!DNL Profile] System Jobs API. |
| Edge [!DNL Profile] query enhancements | Applications can now query Edge [!DNL Profile] by any of the identities of a given profile. |
| プロジェクションごとの結合ポリシーの設定 | アプリケーションでは、特定の結合ポリシーで管理されるデータのビューを生成するために、プロジェクションごとに結合ポリシーを設定できるようになりました。 |
| 計算済み属性 | 計算済み属性は、他の値、計算、および式に基づいて、フィールドの値を自動的に計算します。計算済み属性は、「受信イベント」、「受信イベントとプロファイルデータ」、または「受信イベント、プロファイルデータ、および履歴イベント」に基づいて、プロファイルレベルで動作し、値（「合計購入」、「ライフタイム値」、「ファネルステータス」など）を集計します。 |

**バグの修正**

* 結合ポリシーの作成ワークフローで使用可能な ID ステッチ戦略のリストを簡素化。

**既知の問題**

* なし。

For more information on [!DNL Real-time Customer Profile], including tutorials and best practices for working with [!DNL Profile] data, please read the [Real-time Customer Profile Overview](../../profile/home.md).

## [!DNL Segmentation Service] {#segmentation}

Adobe Experience Platform [!DNL Segmentation Service] provides a user interface and RESTful API that allows you to build segments and generate audiences from your [!DNL Real-time Customer Profile] data. These segments are centrally configured and maintained on [!DNL Platform], making them readily accessible by any Adobe application.

[!DNL Segmentation Service] 顧客ベース内のマーケティング可能な人々のグループを区別する基準を説明することで、特定のプロファイルのサブセットを定義します。 セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

| 機能 | 説明 |
| -----------| ---------- |
| スケジュールされたセグメント化 | ユーザーは、UI と API を使用して、すべてのセグメントに対して、スケジュールされたセグメント評価を有効にできるようになりました。有効にすると、すべてのセグメントが 1 日に 1 回評価されます。この機能は、オンデマンドのセグメント化機能には影響しません。オンデマンドのセグメント化機能は以前と同じように引き続き機能します。<br/><br/>注意：5つを超えるマージポリシーを持つサンドボックスでは、スケジュール済みセグメント機能を使用でき [!DNL XDM Individual Profile]ません。 |
| ストリーミングセグメント化 | Support for continuous evaluation of segments (streaming segmentation) allows most segment rules to be evaluated as the data is passing into [!DNL Platform]. この機能を使用すると、セグメントメンバーシップは、スケジュールされたセグメント化ジョブを実行しなくても最新の状態になります。複数エンティティの関係を使用するセグメントや強化ペイロードを含むセグメントなど、一部の例外が存在します。 |
| 構成ブロックとしてのセグメント | セグメントビルダー UI を使用してセグメントを作成する場合、以前に定義したセグメントを、追加セグメントの構成ブロックとして使用できるようになりました。 <ul><li>現在のオーディエンスメンバーシップの参照：ユーザーがオーディエンスになったり、オーディエンスでなくなったりしたときに更新されます。</li><li>ロジックのコピー：選択したセグメント定義を新しいセグメントにコピーします。</li></ul> |
| ID 名前空間ごとにセグメントメンバーシップを表示 | セグメントメンバーシップを ID 名前空間（電子メール、ECID、合計数）ごとに表示できるようになりました。 |
| RBAC のサポート | セグメントビルダーでは、基本的なロールベースのアクセス制御と権限がサポートされるようになりました。 |
| Enhanced support for external audience sharing between [!DNL Platform] and Adobe solutions | Users can now bring in external (non-[!DNL Experience Platform]) audience metadata in scenarios where the number of audiences is large or not known a priori. This release includes access to [!DNL Audience Manager] metadata for customers who have provisioned the solution connector. This audience metadata can be used within Segment Builder to create new [!DNL Experience Platform] segments. <br/><br/> さらに、で作成したセグメント [!DNL Experience Platform] は、、などの統合Adobeソリューションで使用できる [!DNL Audience Manager]ようにな [!DNL Target]り [!DNL Ad Cloud]ます。 |

**バグの修正**

* 関係がネストされている場合、複数エンティティのセグメント化でプロファイルが返されない問題を修正しました。
* 除外ロジックが誤った結果を返す問題を修正しました。
* 複数エンティティのフォルダーの読みやすさを改善しました。わかりやすい名前の XDM クラスが表示されるようになりました。
* 同じ XDM フォルダーの複数のコピーが表示される断続的な問題を修正しました。
* 選択した結合ポリシーに対してセグメント予測が使用できない場合に、ユーザーに通知するメッセージが生成されるようになりました。

**既知の問題**

* なし。

To learn more about [!DNL Segmentation Service], please read the [Segmentation Service overview](../../segmentation/home.md).