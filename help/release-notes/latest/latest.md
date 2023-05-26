---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2023年5月のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: fc886dc0d7abb1df76c12edc423bc788b443a788
workflow-type: tm+mt
source-wordcount: '1361'
ht-degree: 30%

---

# Adobe Experience Platform リリースノート

>[!IMPORTANT]
>
>Audience Portal 機能の一般的な可用性に備えて、Adobe Experience Platformでは、システムおよびドキュメント内での「オーディエンス」と「セグメント」の使用を更新しています。
>
>- **対象ユーザ**:共通の特性や行動を共有する人、アカウント、世帯、またはその他のエンティティのセット。
>
>- **セグメント定義**:Adobe Experience Platformでは、ターゲットオーディエンスの主要な特性や動作を記述する際に使用されるルールです。 この用語は、以前は「セグメント」と呼ばれていました。
>
>- **セグメント**:プロファイルをオーディエンスに分割する行為。 「セグメント」という用語は、現在は動詞としてのみ使用されています。
>
>- **セグメント化**:グループ化して一連の結果（オーディエンスなど）を生成するプロファイルの特性を識別し、関連付ける行為。
>
>その結果、Adobe Experience Platform UI 内では、「セグメント」が「オーディエンス」に置き換えられ、この新しいオーディエンス作成および管理方法が反映されます。

**リリース日：2023年5月24日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [データ収集](#data-collection)
- [データガバナンス](#data-governance)
- [データ取り込み](#data-ingestion)
- [宛先](#destinations)
- [ID サービス](#identity-service)
- [クエリサービス](#query-service)
- [ソース](#sources)

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Twitter] コンバージョン API 拡張機能 | この [[!DNL Twitter] コンバージョン API](../../tags/extensions/server/twitter/overview.md) イベント転送拡張機能を使用すると、イベントのコンバージョンに関して、サーバー側でリアルタイムにイベントデータを転送できます。 [!DNL Twitter] コンバージョン API。 |
| データ要素のパスに関する支援 | 内のデータ要素のパスの決定 [Core 拡張機能](../../tags/extensions/client/core/overview.md) 今や以前よりも簡単になった この機能強化により、正しいデータ要素のパスを選択し、書式設定するのに役立つガイド付きフォームが提供されます。 |

{style="table-layout:auto"}

データコレクションの詳細については、 [データコレクションの概要](../../tags/home.md).

## データガバナンス {#data-governance}

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。これは Experience Platform 内の様々なレベルで重要な役割を果たします。例えば、カタログ化、データ系列、データ使用のラベル付け、データアクセスポリシー、マーケティングアクションのデータに関するアクセス制御などです。

**新機能**

| 機能 | 説明 |
| --- | --- |
| データセットフィールドレベルのラベル付けの廃止 | 個々のフィールドにラベルを適用する機能は、データセットからスキーマに移動されました。 これにより、データガバナンス、同意、アクセス制御のために、フィールドラベルの管理を一元化できます。 以前に適用したデータセットのフィールドラベルは、Experience PlatformUI を通じて一時的にサポートされます。 既存のデータセットフィールドラベルは、2024 年 5 月 31 日までにスキーマフィールドラベルに手動で移行する必要があります。 詳しくは、 [データガバナンスのエンドツーエンドガイド](../../data-governance/e2e.md) ラベルの移行の詳細を参照してください。 |

{style="table-layout:auto"}

データガバナンスの詳細については、 [データガバナンスの概要](../../data-governance/home.md).

## データ取得 {#data-ingestion}

Adobe Experience Platform は、あらゆる種類および遅延のデータを取得するための豊富な機能セットを提供します。取り込みは、バッチ API またはストリーミング API、Adobeで作成されたソース、データ統合パートナー、Adobe Experience Platform UI のいずれかを使用しておこなうことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| データ取り込みテンプレートのベータ版の可用性 | データ取り込みテンプレートを使用すると、データアーキテクトとエンジニアは、標準のテンプレートと自動化ツールを使用して、スキーマ、データセットの作成、マッピングルールの設定など、データ取り込みプロセスを高速化できます。 現在、 [[!DNL Marketo Engage]](../../sources/connectors/adobe-applications/marketo/marketo.md), [[!DNL Salesforce]](../../sources/connectors/crm/salesforce.md) および [[!DNL Microsoft Dynamics]](../../sources/connectors/crm/ms-dynamics.md) ソース。 詳しくは、 [UI でのテンプレートの使用](../../sources/tutorials/ui/templates.md). |

データ取り込みの詳細については、 [データ取得の概要](../../ingestion/home.md).

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先** {#new-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| **[[!UICONTROL Mailchimp 関心カテゴリ]](../../destinations/catalog/email-marketing/mailchimp-interest-categories.md)** | **[!UICONTROL Mailchimp]** は、メーリングリストや電子メールマーケティングキャンペーンを使用して連絡先（顧客、顧客、その他の関心のある関係者）を管理し、連絡を取るために企業が使用する、人気のあるマーケティングオートメーションプラットフォームおよび電子メールマーケティングサービスです。 このコネクタを使用して、連絡先を興味と好みに基づいて並べ替えます。 |

{style="table-layout:auto"}

<!--

**New or updated functionality** {#destinations-new-updated-functionality}

| Functionality | Description |
| ----------- | ----------- |
| General availability of attribute-based personalization through the [Adobe Target](../../destinations/catalog/personalization/adobe-target-connection.md) and [Custom personalization](../../destinations/catalog/personalization/custom-personalization.md) destinations. | Leverage profile attributes in real-time to deliver one-to-one web and mobile personalization, via Adobe Target or other custom personalization destinations in Experience Platform. See the [dedicated documentation](../../destinations/ui/activate-edge-personalization-destinations.md) for more details. |
| Destination SDK support for grouping exported audiences based on merge policy. | When building a file-based destination with Destination SDK, you can now configure the grouping of exported audiences into one or multiple files, based on merge policy. <br><br> Additionally, you can now include the merge policy ID and merge policy name in the exported file names, by using the dedicated template macros. <br><br>See the [batch configuration documentation](../../destinations/destination-sdk/functionality/destination-configuration/batch-configuration.md) for more details on how to use the `segmentGroupingEnabled` parameter and the new file name template macros.|

{style="table-layout:auto"}

-->

**修正および機能強化** {#destinations-fixes-and-enhancements}

- （ベータ版）SFTP クラウドストレージの宛先で、ユーザーが Port パラメーターの値をカスタマイズできない問題を修正しました。 を通じて（ベータ版）SFTP の宛先接続を設定する際、値を編集できるようになりました。 [API](/help/destinations/api/activate-segments-file-based-destinations.md) または [UI](/help/destinations/catalog/cloud-storage/sftp.md#authentication-information).

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**機能を更新**

| 機能 | 説明 |
| --- | --- |
| Adobe Experience Cloudアプリケーションでの Partner ID のサポート [!BADGE ベータ版]{type=Informative} | パートナー ID を ID サービスで使用できるようになりました。 パートナー ID は、人物を表すためにデータパートナーが使用する識別子です。 Real-time Customer Data Platformでは、パートナー ID は主に、オーディエンスのアクティベーションの拡張とデータエンリッチメントに使用されます。 パートナー ID は、ID グラフには保存されません。 詳しくは、 [id タイプ](../../identity-service/namespaces.md#identity-types). |

ID サービスの詳細については、 [ID サービスの概要](../../identity-service/home.md)

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL data lake] でデータに対してクエリを実行できます。データレイクの任意のデータセットを結合したり、クエリ結果を新しいデータセットとして取得したりすることで、それらのデータセットをレポートやデータサイエンスワークスペースで使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| ADLS データセットの列レベルの統計を計算 | この `ANALYZE TABLE` コマンドが `COMPUTE STATISTICS` および `SHOW STATISTICS` SQL コマンド ADLS データセットのサブセットまたはそのデータセット内の特定の列に関する統計を計算できるようになりました。 詳しくは、 [データセット統計の計算ガイド](../../query-service/essential-concepts/dataset-statistics.md). |

{style="table-layout:auto"}

クエリサービスの詳細については、 [クエリサービスの概要](../../query-service/home.md).

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むことができ、Platform サービスを使用してそのデータの構造化、ラベル付け、および拡張を行うことができます。Adobeアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| からのデータストリーミングの API サポート [!DNL Snowflake] データベース | これで、 [[!DNL Snowflake] ソース](../../sources/connectors/databases/snowflake-streaming.md) の使用 [!DNL Flow Service] API |
| ドラフトモード用に API サポートを拡張 | これで、 [!DNL Flow Service] API をいつでも使用できます。 以下を使用： `mode=draft` ベース、ソース、ターゲットの接続をドラフトとして保存する状態。 すべてのドラフトエンティティは、後で完了するように再訪問できます。 次のガイドを読む： [設定 [!DNL Flow Service] ドラフト状態のエンティティ](../../sources/tutorials/api/draft.md) を参照してください。 |
| [!DNL Salesforce Marketing Cloud] ソースの一般提供 | この [[!DNL Salesforce Marketing Cloud source] は現在 GA に含まれています](../../sources/connectors/marketing-automation/salesforce-marketing-cloud.md). このソースを使用して、 [!DNL Salesforce Marketing Cloud] データをExperience Platformに送信します。 |
| [!DNL Google Ads] 認証の更新 | 認証時にログイン顧客 ID を指定できるようになりました。 [!DNL Google Ads] 特定のオペレーティング顧客からレポートデータを取得するソースアカウント。 詳しくは、 [[!DNL Google Ads] ソースドキュメント](../../sources/connectors/advertising/ads.md) を参照してください。 |
| [!DNL Google PubSub] 認証の更新 | 次に、 [!DNL Google PubSub] 新しいアカウントを作成する際のソース。 プロジェクトベースの認証を使用してルートレベルのアクセスを許可するか、トピックおよび購読ベースの認証を使用して特定のトピックおよび購読ストリームへのアクセスを制限します。 詳しくは、 [[!DNL Google PubSub] ソースドキュメント](../../sources/connectors/cloud-storage/google-pubsub.md) を参照してください。 |
| 次の新しいページネーションフィールドパラメーター `type=PAGE` セルフサービスソース（バッチ SDK）内 | これで、 `initialPageIndex` および `endPageIndex` ソースを `type=PAGE` バッチ SDK を使用する。 <ul><li>`initialPageIndex`:このパラメーターを使用すると、ページネーションが開始するページ番号を定義できます。 </li><li>`endPageIndex`:このパラメーターを使用すると、終了条件を設定し、ページネーションを停止できます。</li></ul> これらの新しいパラメーターの詳細については、 [セルフサービスソースバッチ SDK ドキュメント](../../sources/sources-sdk/config/sourcespec.md#page). |
| ドラフトモードの UI のサポート | ユーザーインターフェイスを使用して、ソースワークフロー中に進行状況を一時停止し、保存できるようになりました。 次を選択できます。 **[!UICONTROL ドラフトとして保存]** ワークフローのデータフローの詳細、マッピング、スケジュール手順の間、後で完了するためにデータフローをドラフトとして保存する。 次のガイドを読む： [UI でのドラフトとしてのデータフローの保存](../../sources/tutorials/ui/draft.md) を参照してください。 |

{style="table-layout:auto"}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。