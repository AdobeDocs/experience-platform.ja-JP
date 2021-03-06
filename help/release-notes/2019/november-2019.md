---
title: Adobe Experience Platformリリースノート 2019 年 11 月
description: Adobe Experience Platform の 2019年11月 のリリースノート。
doc-type: release notes
last-update: November 18, 2019
author: crhoades, ens28527
exl-id: 2c417c56-cc61-4788-b248-d98ea6cf89f0
source-git-commit: ce967ae176fce81aa26d92b3f0ee8be006808657
workflow-type: tm+mt
source-wordcount: '1887'
ht-degree: 70%

---

# Adobe Experience Platform リリースノート

**リリース日：2019 年 11 月 18 日（PT）**

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

[!DNL Real-time Customer Data Platform] には、データガバナンス、id 管理、高度なセグメント化、データサイエンスのツールが含まれており、プロファイルを作成してオーディエンスを定義したり、リッチなインサイトを得たり、厳密なデータガバナンスポリシーを実施したりできます。

Adobe Experience Cloud とのネイティブな統合に加え、アドビはパートナーの大規模なエコシステムにリンクし、オンサイトやアプリケーション内のパーソナライズ機能から電子メール、有料メディア、コールセンター、接続されたデバイスなど、あらゆるチャネルにわたって、優れた顧客体験を提供できます。

リアルタイム CDP を使用すると、次のことが可能になります。

* 大規模法人の全体から顧客データをストリーミング収集して、顧客を一目で把握する。
* 既知の識別子と未知の識別子に対して、信頼されたガバナンスとプライバシーコントロールを使用して、プロファイルを責任を持って管理する。
* Adobe Sensei による AI や機械学習を活用して、マーケティング担当者向けに構築された実用的なインサイトを生み出し、オーディエンスを拡大縮小する。
* すべてのチャネルと宛先にわたって、パーソナライズされたエクスペリエンスをリアルタイムで提供する。

詳しくは、 [Real-time Customer Data Platformドキュメント](../../rtcdp/overview.md).

**主な特長**

| 機能 | 説明 |
|---|---|
| 宛先 | Adobeの [!DNL Real-time Customer Data Platform] を使用することで、パートナーに対してシームレスにデータをアクティブ化できます。 詳しくは、以下の「[宛先](#destinations)」を参照してください。 |
| ホームページの指標ダッシュボード | Real-time Customer Data Platform(Real-time CDP) のホームページには、プロファイルとセグメントに関する情報を表示する指標ダッシュボードが含まれています。 このホームページには、学習教材へのリンクも含まれています。以下の「[リアルタイム顧客データプラットフォームの指標](#real-time-customer-data-platform-metrics)」の節を参照してください。 |
| ソース | アドビソリューション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取り込むことができます。詳しくは、以下の「[ソース](#sources)」の節を参照してください。 |

**[!DNL Real-time Customer Data Platform]指標**

リアルタイム CDP にログインすると、指標ダッシュボードを含むReal-time Customer Data Platform（リアルタイム CDP）ホームページが表示されます。

ホームページは、指標カードが表示される場所の 1 つに過ぎません。リアルタイム CDP は、ユーザーの経験を通じて指標カードを提供します。これらの指標は、システム内のデータ、プロファイル、セグメントオーディエンスに関する情報を提供します。

リアルタイム CDP にログインしたときにシステムにデータがない場合、ホームページのダッシュボードは表示されません。この場合、ホームページでは初めて使用するユーザーのための学習教材を提供します。データが収集されると、ダッシュボードが自動的に更新され、そのデータに関する情報が表示されます。

詳しくは、「[リアルタイム顧客データプラットフォームの指標の概要](../../rtcdp/home-page-dashboards.md)」を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、AdobeのReal-time Customer Data Platformでサポートされる宛先プラットフォームとの事前定義済みの統合で、これらのパートナーに対するデータをシームレスにアクティブ化します。 詳しくは、「[宛先の概要](../../destinations/home.md)」の記事を参照してください。

**使用可能な宛先**

11 月のリリースでは、アドビのリアルタイム顧客データプラットフォームは次の宛先をサポートします。

* 広告: [!DNL Google]
* 電子メールマーケティング：Adobe Campaign [!DNL Salesforce Marketing Cloud], [!DNL Responsys], [!DNL Oracle Eloqua]

各宛先について詳しくは、「[宛先カタログ](../../destinations/catalog/overview.md)」を参照してください。

**既知の制限事項**

* 初期リリースでは、アクティベーションフロー（スケジュール手順）のカスタムアクティベーションスケジュールを許可するコントロールは使用できません。
* 現在、宛先設定を編集または削除する方法はありません。この制限を回避するには、[宛先の詳細ページ](../../destinations/ui/destination-details-page.md)の右上にある宛先を有効または無効にします。
* 現在、宛先またはストレージアカウントに接続する際に、アカウントの詳細、パス、または資格情報は検証されません。正しい資格情報を入力していることを確認し、スペルミスや入力ミスをもう一度確認してください。
* 初期リリースでは、資格情報の更新は行われません。アカウントの有効期限が切れるか、更新が必要な場合は、新しい宛先接続を作成し、以前にマッピングしたセグメントを再マッピングする必要があります。

## ソース {#sources}

Adobe Experience Platformで外部ソースからデータを取り込みながら、を使用してデータの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。 アドビソリューション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、ストレージシステムと CRM サービスに対する認証、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**主な特長**

| 機能 | 説明 |
| ---------- | ------------ |
| ソース UI | ソース接続を作成、表示、および管理するための新しいユーザーインターフェイス。 |
| CRM コネクタの改良されたワークフロー | 作成と管理のための直感的な新しい UI ワークフロー [!DNL Microsoft Dynamics] および [!DNL Salesforce] コネクタ。 |
| クラウドベースのストレージ用のコネクタのサポート | コネクタがクラウドベースのストレージにアクセスできるようになりました。新しいソースには以下が含まれます。 [!DNL Amazon S3], [!DNL Azure Blob]、および FTP/SFTP サーバー。 |

**既知の問題**

* クラウドベースのストレージ用のソースコネクタは、圧縮ファイルの取得をサポートしていません。

ソースについて詳しくは、「[ソースの概要](../../sources/home.md)」を参照してください。

## [!DNL Data Science Workspace] {#dsw}

Adobe Experience Platform [!DNL Data Science Workspace] を使用すると、Adobeサイエンティストは機械学習モデルを構築し運用することで、データやコンテンツから、データやサードパーティのシステムをまたいでシームレスにインサイトを生み出すことができます。 [!DNL Data Science Workspace] は、と緊密に統合されています [!DNL Platform] とは、XDM データの調査と準備、次にモデルの開発と運用を含め、エンドツーエンドのデータサイエンスライフサイクルを強化し、自動的にエンリッチメントをおこなうためのモデルの開発と運用を可能にします。 [!DNL Real-time Customer Profile] と機械学習インサイト

**新機能**

| 機能 | 説明 |
| -----------| ---------- |
| を使用したデータアクセス [!DNL Platform] SDK | の事前定義済みレシピとランチャーノートブック [!DNL Python] 現在は [!DNL Platform] データにアクセスするための SDK。 |
| サンドボックスのサポート | ノートブックやレシピを開発用サンドボックスや実稼働用サンドボックスに分離する機能など、今後のサンドボックス機能（現在はベータ版）のサポート。詳しくは、「[サンドボックスの概要](../../sandboxes/home.md)」を参照してください。 |

詳しくは、「[Data Science Workspace　の概要](../../data-science-workspace/home.md)」を参照してください。

## [!DNL Experience Data Model]（XDM）システム {#xdm}

標準化と相互運用性は、[!DNL Experience Platform] の背後にある重要な概念です。アドビが推進する [!DNL Experience Data Model]（XDM）は、顧客体験データを標準化し、顧客体験管理のスキーマを定義する取り組みです。

XDM はパブリックに文書化された仕様であり、デジタルエクスペリエンスのパワーを向上させるために設計されています。Adobe Experience Platform 上のサービスと通信するすべてのアプリケーションに共通の構造と定義を提供します。XDM 標準規格に準拠することで、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| ---------- | ------------ |
| 通知スキーマ | データ取得プロセス中に送信される通知データを表す新しいスキーマ。 |
| Adobe AdCloud DSP スキーマ | Adobe Advertising Cloud ディスプレイ広告（DSP）メタデータを表すための 5 つの新しいスキーマ（配置、キャンペーン、パッケージ、広告主、アカウント）が追加されました。 |
| ExperienceEvent 実装の詳細スキーマフィールドグループ | イベントの収集に使用するソフトウェアに関する情報を保存するための標準フィールドを追加する新しい ExperienceEvent フィールドグループ。 |
| [!DNL Profile Privacy] フィールドグループ | の一般的なアウト/販売/共有のオプトアウトシグナルを受け入れるフィールドを追加する新しいプロファイルフィールドグループ [!DNL Real-time Customer Profile]. |
| `xdm:alternateDisplayInfo` の形式制限 | `xdm:alternateDisplayInfo` の「タイトル」フィールドと「説明」フィールドは、両方とも検証に合格する文字列である必要があります。 |
| 名前の変更：XDM [!DNL Individual Profile] | 「XDM」の「タイトル」 [!DNL Profile]「 」クラスが「XDM」に更新されました [!DNL Individual Profile]&quot;. クラスの公式 `$id` は変更されていません。 |

**既知の問題**

* なし。

を使用した XDM の操作について詳しくは、以下を参照してください。 [!DNL Schema Registry] API および [!DNL Schema Editor] ユーザーインターフェイス、 [XDM システムドキュメント](../../xdm/home.md).

## [!DNL Real-time Customer Profile] {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。を使用 [!DNL Real-time Customer Profile]を使用すると、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。 [!DNL Profile] では、様々な顧客データを統合ビューに統合し、顧客インタラクションごとに実用的なタイムスタンプ付きの説明を提供できます。

| 機能 | 説明 |
| -----------| ---------- |
| の機能強化 [!DNL Profile] 参照 | 参照記述子と関連エンティティを使用してプロファイルを検索できるようになりました。 |
| 指定したデータセットのデータのクリーンアップ | ユーザーは、 [!DNL Profile] システムジョブ API |
| Edge [!DNL Profile] クエリの強化 | アプリケーションが Edge に対してクエリを実行できるようになりました [!DNL Profile] 特定のプロファイルの ID に基づいて |
| プロジェクションごとの結合ポリシーの設定 | アプリケーションでは、特定の結合ポリシーで管理されるデータのビューを生成するために、プロジェクションごとに結合ポリシーを設定できるようになりました。 |
| 計算済み属性 | 計算済み属性は、他の値、計算、および式に基づいて、フィールドの値を自動的に計算します。計算済み属性は、「受信イベント」、「受信イベントとプロファイルデータ」、または「受信イベント、プロファイルデータ、および履歴イベント」に基づいて、プロファイルレベルで動作し、値（「合計購入」、「ライフタイム値」、「ファネルステータス」など）を集計します。 |

**バグの修正**

* 結合ポリシーの作成ワークフローで使用可能な ID ステッチ戦略のリストを簡素化。

**既知の問題**

* なし。

詳しくは、 [!DNL Real-time Customer Profile]を操作するためのチュートリアルやベストプラクティスを含む [!DNL Profile] データを読んでください [リアルタイム顧客プロファイルの概要](../../profile/home.md).

## [!DNL Segmentation Service] {#segmentation}

Adobe Experience Platform [!DNL Segmentation Service] は、セグメントを作成し、[!DNL Real-time Customer Profile] データからオーディエンスを生成できるユーザーインターフェイスおよび RESTful API を提供します。これらのセグメントは、[!DNL Platform] 上で一元的に設定および管理され、アドビのアプリケーションから簡単にアクセスできます。

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

| 機能 | 説明 |
| -----------| ---------- |
| スケジュールされたセグメント化 | ユーザーは、UI と API を使用して、すべてのセグメントに対して、スケジュールされたセグメント評価を有効にできるようになりました。有効にすると、すべてのセグメントが 1 日に 1 回評価されます。この機能は、オンデマンドのセグメント化機能には影響しません。オンデマンドのセグメント化機能は以前と同じように引き続き機能します。<br/><br/>注意：スケジュールされたセグメント化機能は、 [!DNL XDM Individual Profile]. |
| ストリーミングセグメント化 | セグメントの継続的な評価（ストリーミングセグメント化）のサポートにより、データがに渡される際に、ほとんどのセグメントルールを評価できます。 [!DNL Platform]. この機能を使用すると、セグメントメンバーシップは、スケジュールされたセグメント化ジョブを実行しなくても最新の状態になります。複数エンティティの関係を使用するセグメントや強化ペイロードを含むセグメントなど、一部の例外が存在します。 |
| 構成ブロックとしてのセグメント | セグメントビルダー UI を使用してセグメントを作成する場合、以前に定義したセグメントを、追加セグメントの構成ブロックとして使用できるようになりました。 <ul><li>現在のオーディエンスメンバーシップの参照：ユーザーがオーディエンスになったり、オーディエンスでなくなったりしたときに更新されます。</li><li>ロジックのコピー：選択したセグメント定義を新しいセグメントにコピーします。</li></ul> |
| ID 名前空間ごとにセグメントメンバーシップを表示 | セグメントメンバーシップを ID 名前空間（電子メール、ECID、合計数）ごとに表示できるようになりました。 |
| RBAC のサポート | セグメントビルダーでは、基本的なロールベースのアクセス制御と権限がサポートされるようになりました。 |
| 間での外部オーディエンス共有のサポートが強化されました [!DNL Platform] とAdobe | ユーザーは、外部 ([!DNL Experience Platform]) オーディエンスメタデータ（オーディエンス数が多い、または事前にわかっていないシナリオ）に関する情報です。 このリリースには、 [!DNL Audience Manager] ソリューションコネクタをプロビジョニングした顧客のメタデータ。 このオーディエンスメタデータは、セグメントビルダー内で新しい [!DNL Experience Platform] セグメント。 <br/><br/> また、 [!DNL Experience Platform] は、次を含む統合Adobeソリューションで使用できるようになりました。 [!DNL Audience Manager], [!DNL Target]、および [!DNL Ad Cloud]. |

**バグの修正**

* 関係がネストされている場合、複数エンティティのセグメント化でプロファイルが返されない問題を修正しました。
* 除外ロジックが誤った結果を返す問題を修正しました。
* 複数エンティティのフォルダーの読みやすさを改善しました。わかりやすい名前の XDM クラスが表示されるようになりました。
* 同じ XDM フォルダーの複数のコピーが表示される断続的な問題を修正しました。
* 選択した結合ポリシーに対してセグメント予測が使用できない場合に、ユーザーに通知するメッセージが生成されるようになりました。

**既知の問題**

* なし。

詳しくは、以下を参照してください。 [!DNL Segmentation Service]を読んでください。 [セグメント化サービスの概要](../../segmentation/home.md).
