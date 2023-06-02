---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2023年5月のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: bf4c39c311bf206ba9113804e0a2fd5f3610d8dc
workflow-type: tm+mt
source-wordcount: '1993'
ht-degree: 33%

---

# Adobe Experience Platform リリースノート

>[!IMPORTANT]
>
>オーディエンスポータル機能の一般提供に備えて、Adobe Experience Platform では、システムおよびドキュメント内での「オーディエンス」と「セグメント」の使用方法を更新しています。
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

- [ダッシュボード](#dashboards)
- [データ収集](#data-collection)
- [データガバナンス](#data-governance)
- [データ取り込み](#data-ingestion)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [ID サービス](#identity-service)
- [クエリサービス](#query-service)
- [ソース](#sources)

## ダッシュボード {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを確認できる複数のダッシュボードを提供しています。

<!-- 
The [!UICONTROL License Usage] dashboard and individual license data for customers has been updated to resolve previous discrepancies between the license entitlements recorded in the Sales Orders and those originally reported in the Experience Platform [!UICONTROL License Usage] dashboard. The updates of individual license data will happen in phases between June 2023 and June 11, 2023. Your actual usage values remain accurate.<br><br>Experience Platform provides multiple capabilities to manage the usage volume:<br><ul><li>[Review and apply best practices to manage data and license usage](https://experienceleague.adobe.com/docs/experience-platform/landing/license/data-management-best-practices.html)</li><li>Apply filtering rules and conditions to [selectively include or exclude data from ingestion to the Real-Time Customer Profile](https://experienceleague.adobe.com/docs/experience-platform/sources/ui-tutorials/create/adobe-applications/analytics.html#filtering-for-profile) for Analytics data before it is ingested into Profile.</li><li>[Contact Adobe support to apply expiration times for Pseudonymous Profiles.](https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html)</li><li>[Contact Adobe support to enable Experience Event expirations on desired datasets.](https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html)</li><li>Contact your Adobe representative to discuss options to increase your license entitlements.</li></ul>
 

<br><ul><li></li></ul><br><br>

Adobe has corrected erroneous entries in your product's associated TermSheets to match their corresponding [Product Descriptions](https://helpx.adobe.com/legal/product-descriptions.html). Discrepancies in base quantities for the **Average Profile Richness** add-on packs will be corrected on **June 9, 2023**. This will provide an accurate representation of your license usage and ensure contractual compliance for your organization. Note that it can take up to 24-36 hours for the licence usage reports to reflect the update.

As a result of this update, you may notice a one-time drop in your license usage for **Average Profile Richness** and **total consumed storage** metrics. If this update brings you close to your licensed limit there are several measures you can take to mitigate your usage.

-[Apply expiration times for Pseudonymous Profiles](https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html)
-[Contact support in order to enable Experience Event expirations on your required datasets. Adobe support representatives can configure expiration times for all Experience Events that are ingested into a dataset enabled for Real-Time Customer Profile](https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html)
-Apply filtering rules and conditions to [selectively include or exclude data from ingestion to the Real-Time Customer Profile](https://experienceleague.adobe.com/docs/experience-platform/sources/ui-tutorials/create/adobe-applications/analytics.html#filtering-for-profile) for Analytics data before it is ingested into Profile.

Contact your Adobe Support representative to discuss options to reduce your usage or increase your license limits.
 -->

| 機能 | 説明 |
| --- | --- |
| ライセンス権限の修正 | この [!UICONTROL ライセンスの使用状況] お客様のダッシュボードおよび個々のライセンスデータが更新され、販売注文に記録されたライセンスの使用権限と、Experience Platformで最初に報告されたライセンスの使用権限との間の以前の相違が解決されました。 [!UICONTROL ライセンスの使用状況] ダッシュボード。 個々のライセンスデータの更新は、2023 年 6 月から 2023 年 6 月 11 日の間に段階的におこなわれます。 実際の使用状況の値は正確なままです。<br><br>Experience Platformには、使用量を管理する複数の機能が用意されています。<br><ul><li>[データとライセンスの使用状況を管理するためのベストプラクティスを確認し適用する](https://experienceleague.adobe.com/docs/experience-platform/landing/license/data-management-best-practices.html).</li><li>フィルタールールと条件の適用先 [リアルタイム顧客プロファイルへの取り込みからデータを選択的に含める、または除外する](https://experienceleague.adobe.com/docs/experience-platform/sources/ui-tutorials/create/adobe-applications/analytics.html#filtering-for-profile) Analytics データを Profile に取り込む前に、</li><li>[偽名プロファイルの有効期限を適用するには、Adobeサポートにお問い合わせください。](https://experienceleague.adobe.com/docs/experience-platform/profile/pseudonymous-profiles.html)</li><li>[Adobeのサポートに連絡して、目的のデータセットで Experience Event の有効期限を有効にします。](https://experienceleague.adobe.com/docs/experience-platform/profile/event-expirations.html?lang=ja)</li><li>ライセンスの使用権限を増やすためのオプションについては、Adobe担当者にお問い合わせください。</li></ul> |

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

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

## 宛先（更新日：5 月 31 日） {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先** {#new-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| **[[!UICONTROL Mailchimp 関心カテゴリ]](../../destinations/catalog/email-marketing/mailchimp-interest-categories.md)** | **[!UICONTROL Mailchimp]** は、メーリングリストや電子メールマーケティングキャンペーンを使用して連絡先（顧客、顧客、その他の関心のある関係者）を管理し、連絡を取るために企業が使用する、人気のあるマーケティングオートメーションプラットフォームおよび電子メールマーケティングサービスです。 このコネクタを使用して、連絡先を興味と好みに基づいて並べ替えます。 |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| を通じた属性ベースのパーソナライゼーションの一般的な可用性 [Adobe Target](../../destinations/catalog/personalization/adobe-target-connection.md) および [カスタムパーソナライゼーション](../../destinations/catalog/personalization/custom-personalization.md) 宛先。 | プロファイル属性をリアルタイムで活用し、Adobe Targetや他のカスタムパーソナライゼーションの宛先を通じて、1 対 1 の Web およびモバイルパーソナライゼーションをExperience Platformで提供します。 詳しくは、 [専用ドキュメント](../../destinations/ui/activate-edge-personalization-destinations.md) そして [FAQ](/help/destinations/destinations-faq.md#same-next-page-personalization) を参照してください。 |
| Destination SDKは、結合ポリシーに基づいて、エクスポートされたオーディエンスをグループ化するためのサポートを提供します。 | Destination SDKを使用してファイルベースの宛先を作成する場合、結合ポリシーに基づいて、書き出されたオーディエンスの 1 つまたは複数のファイルへのグループ化を設定できるようになりました。 <br><br> さらに、専用のテンプレートマクロを使用して、書き出されたファイル名に結合ポリシー ID と結合ポリシー名を含めることができるようになりました。 <br><br>詳しくは、 [バッチ設定ドキュメント](../../destinations/destination-sdk/functionality/destination-configuration/batch-configuration.md) を参照してください。 `segmentGroupingEnabled` パラメータと新しいファイル名テンプレートマクロ。 |
| ベータクラウドストレージの宛先への書き出しにマニフェストファイルを含める | 6 つのクラウドストレージベータ版の宛先にデータを書き出す際に、書き出し場所、書き出しサイズなどの情報を含むマニフェスト JSON ファイルを含めることができるようになりました。 [（ベータ版）Amazon S3](/help/destinations/catalog/cloud-storage/amazon-s3.md), [（ベータ版）Azure Blob](/help/destinations/catalog/cloud-storage/azure-blob.md), [（ベータ版）Azure Data Lake Storage Gen2](/help/destinations/catalog/cloud-storage/adls-gen2.md), [（ベータ版）データランディングゾーン](/help/destinations/catalog/cloud-storage/data-landing-zone.md), [（ベータ版）Google Cloud Storage](/help/destinations/catalog/cloud-storage/google-cloud-storage.md), [（ベータ版）SFTP](/help/destinations/catalog/cloud-storage/sftp.md). <br><br> 詳しくは、 **[!UICONTROL 宛先の詳細]** 」セクション内にある必要があります。 |

{style="table-layout:auto"}

**修正および機能強化** {#destinations-fixes-and-enhancements}

- （ベータ版）SFTP クラウドストレージの宛先で、ユーザーが Port パラメーターの値をカスタマイズできない問題を修正しました。 を通じて（ベータ版）SFTP の宛先接続を設定する際、値を編集できるようになりました。 [API](/help/destinations/api/activate-segments-file-based-destinations.md) または [UI](/help/destinations/catalog/cloud-storage/sftp.md#authentication-information).

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| フィールドグループ | （複数） | の複数のフィールド [オファー項目](https://github.com/adobe/xdm/pull/1720/files) が更新され、スキーマから二重階層が削除されました。 |
| フィールドグループ | [[!UICONTROL XDM 個人見込み客プロファイル]](https://github.com/adobe/xdm/pull/1721/files) | この `partnerProspect` メタデータタグのオプションが [!UICONTROL XDM 個別見込み客プロファイル] クラス。 |
| データタイプ | （複数） | の複数のフィールドが追加されました [[!UICONTROL メディアの詳細情報]](https://github.com/adobe/xdm/pull/1716/files) データ型。 |
| データタイプ | [[!UICONTROL セッションの詳細情報]](https://github.com/adobe/xdm/pull/1716/files) | リダイレクトが発生したかどうかを示す新しいフィールドが追加されました。 |
| フィールドグループ | [[!UICONTROL Media Analytics インタラクションの詳細]](https://github.com/adobe/xdm/pull/1716/files) | メディアレポートに関連する新しいフィールドが追加されました。 |

{style="table-layout:auto"}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

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
