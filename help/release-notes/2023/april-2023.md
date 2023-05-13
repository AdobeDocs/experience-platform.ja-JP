---
title: Adobe Experience Platform リリースノート 2023年4月
description: Adobe Experience Platform の 2023年4月のリリースノート。
exl-id: 8b8fa810-d301-43c1-98df-10d3903f3147
source-git-commit: c95d2ab1a6f104c18c491d3a533ee2c304a0aa68
workflow-type: tm+mt
source-wordcount: '2095'
ht-degree: 92%

---

# Adobe Experience Platform リリースノート

>[!IMPORTANT]
>
>2023 年 5 月 15 日以降、 `Existing` セグメントメンバーシップのライフサイクルでの冗長性を削除するために、セグメントメンバーシップマップでステータスが非推奨となります。 この変更後、セグメントで認定されたプロファイルは、 `Realized` 不適格なプロファイルは、引き続き次のように表されます。 `Exited`. この変更の詳細については、 [セグメント化サービスセクション](#segmentation).

**リリース日：2023年4月26日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [ダッシュボード](#dashboards)
- [データ準備](#data-prep)
- [データ収集](#data-collection)
- [宛先](#destinations)
- [エクスペリエンスデータモデル](#xdm)
- [Real-Time Customer Data Platform](#rtcdp)
- [リアルタイム顧客プロファイル](#profile)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## ダッシュボード {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを確認できる複数のダッシュボードを提供しています。

**新機能または更新された機能** {#dashboards-new-updated-features}

| 機能 | 説明 |
| --- | --- |
| ユーザー定義ダッシュボード | ウィジェットのインサイトから&#x200B;**履歴データをフィルタリング**&#x200B;し、最近のデータまたはカスタム分析期間のいずれかを使用できるようになりました。詳しくは、[ユーザー定義ダッシュボードガイド](../../dashboards/user-defined-dashboards.md#filter-historical-data)を参照してください。<br>また、**既存のウィジェットを複製**&#x200B;できるようになりました。複製をカスタマイズしてその属性を編集することで、新しい一意のウィジェットを作成する際に最初からやり直す必要がなくなります。詳しくは、[ウィジェット複製ガイド](../../dashboards/user-defined-dashboards.md#duplicate-a-widget)を参照してください。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換および検証を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 非実稼動サンドボックスでの Adobe Analytics のバックフィル期間の更新 | 非実稼動サンドボックスでの Adobe Analytics のバックフィル期間が 3 か月に短縮されました。実稼動用サンドボックスのバックフィルは、13 か月で同じままです。この変更は新しいフローにのみ適用され、既存のフローには影響しません。詳しくは、[Adobe Analytics の概要](../../sources/connectors/adobe-applications/analytics.md)を参照してください。 |
| FPID 文字列を ECID に変換する新しいマッパー関数 | `fpid_to_ecid` 関数を使用すると、FPID 文字列を ECID に変換し、Experience Platform および Experience Cloud アプリケーションで使用できるようになります。詳しくは、[データ準備関数ガイド](../../data-prep/functions.md)を参照してください。 |

{style="table-layout:auto"}

データ準備について詳しくは、[データ準備の概要](../../data-prep/home.md)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| データストリームの IP アドレスの不明化 | [データストリーム設定 UI](../../edge/datastreams/configure.md) で、部分的または完全なデータストリームレベルの IP の不明化オプションを定義できるようになりました。<br><br>データストリームレベルの IP の不明化の設定は、Adobe Target および Audience Manager で設定した IP の不明化よりも優先されます。<br><br>Adobe Analytics に送信されるデータは、データストリームレベルの [!UICONTROL IP の不明化]の設定の影響を受けません。Adobe Analytics では現在、不明化されていない IP アドレスを受信します。Analytics で不明化された IP アドレスを受信できるようにするには、Adobe Analytics で IP の不明化を個別に設定する必要があります。この動作は、今後のリリースで更新される予定です。<br><br> IP の不明化とその設定方法について詳しくは、[データストリーム設定ドキュメント](../../edge/datastreams/configure.md#advanced-options)を参照してください。 |
| [データストリーム設定の上書き](../../edge/datastreams/overrides.md) | イベントデータセット、Target プロパティトークン、ID 同期コンテナ、Analytics レポートスイートなどの特定の設定を上書きするために使用できる、データストリームの追加の設定オプションを定義できるようになりました。<br><br>データストリーム設定の上書きは、次の 2 つの手順で行います。 <ol><li>最初に、[データストリーム設定ページ](../../edge/datastreams/configure.md)でデータストリーム設定の上書きを定義する必要があります。</li><li>次に、Web SDK コマンドまたは Web SDK [タグ拡張機能](../../edge/extension/web-sdk-extension-configuration.md)を使用して、上書きを Edge Network に送信する必要があります。</li></ol> |
| OAuth JWT 秘密鍵 | [OAuth JWT 秘密鍵](https://experienceleague.adobe.com/docs/experience-platform/tags/event-forwarding/secrets.html?lang=ja)では、アドビおよび Google サービストークンを使用して、イベント転送でのサーバー間インタラクションをサポートできます。 |
| [!DNL Pinterest Conversions API] 拡張機能 | [[!DNL Pinterest Conversions API]](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/pinterest/overview.html?lang=ja) イベント転送拡張機能を使用すると、Adobe Experience Platform Edge Network で取得したデータを活用したり、[!DNL Pinterest Conversions API] を使用してサーバーサイドイベントの形式で [!DNL Pinterest] に送信したりできます。 |

{style="table-layout:auto"}

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先** {#new-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!DNL Salesforce Marketing Cloud Account Engagement] 接続](../../destinations/catalog/email-marketing/salesforce-marketing-cloud-account-engagement.md) | Salesforce Marketing Cloud Account Engagement（旧 Pardot）の宛先を使用して、リードをキャプチャ、追跡、スコアリング、評価を行います。この宛先は、複数の部門や意思決定者が関与し、より長い販売サイクルと意思決定サイクルが必要な B2B ユースケースに使用します。 |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| [!DNL Custom Personalization] と [!DNL Adobe Commerce] の宛先のデータフロー監視 | <p> [Adobe Commerce](/help/destinations/catalog/personalization/adobe-commerce.md)、[カスタムパーソナライゼーション](../../destinations/catalog/personalization/custom-personalization.md)、[属性を含むカスタムパーソナライゼーション](../../destinations/catalog/personalization/custom-personalization.md)接続のアクティベーション指標を確認できるようになりました。 </p> <p>![Adobe Commerce の画像](/help/destinations/assets/common/adobe-commerce-metrics.png "Adobe Commerce の指標"){width="100" zoomable="yes"}</p>  詳しくは、[宛先ワークスペースでのデータフローの監視](../../dataflows/ui/monitor-destinations.md#monitor-dataflows-in-the-destinations-workspace)を参照してください。 |
| [!DNL Google Ad Manager] と [!DNL Google Ad Manager 360] の宛先の新しい「**[!UICONTROL セグメント名にセグメント ID を追加]**」フィールド | <p>[[!DNL Google Ad Manager]](/help/destinations/catalog/advertising/google-ad-manager.md#parameters) のセグメント名と、[[!DNL Google Ad Manager 360]](/help/destinations/catalog/advertising/google-ad-manager-360-connection.md#destination-details) に Experience Platform のセグメント ID を `Segment Name (Segment ID)` のように含めることができるようになりました。</p><p>![セグメント ID を追加の画像](/help/destinations/assets/common/append-segment-id-to-segment-name.png "新しい「セグメント名にセグメント ID を追加」フィールド "){width="100" zoomable="yes"}</p> |
| スケジュールされたオーディエンスのバックフィル | <p>[[!DNL Google Display & Video 360]](/help/destinations/catalog/advertising/google-dv360.md#specifics) の宛先では、セグメントが最初に宛先接続にマッピングされてから 24～48 時間後に、宛先へのオーディエンスバックフィルのアクティベーションが行われるようにスケジュールされています。この更新は、データの取り込みまで 24 時間待機するという Google のポリシーに対応するものであり、Real-Time CDP と [!DNL Google Display & Video 360] の間の一致率が向上します。</p> <p>これは、この宛先にのみ適用されるバックエンド設定であり、UI でお客様が設定可能なスケジュールオプションとは無関係です。</p> |

{style="table-layout:auto"}

**修正および機能強化** {#destinations-fixes-and-enhancements}

- ファイルベースの宛先書き出しのレポート指標を&#x200B;**除外した ID** の問題を修正しました。お客様は、アクティブ化された書き出しから書き出されたすべての ID を期待どおりに受信していました。ただし、UI の&#x200B;**除外された ID** レポート指標では、書き出されないはずの ID が誤ってカウントされたので、多数の除外された ID を誤って表示していました。(PLAT-149774)
- アクティベーションワークフローの&#x200B;**スケジュール**&#x200B;手順の問題を修正しました。マッピング ID が必要な宛先の場合、お客様は、既存の宛先接続に追加されたセグメントのマッピング ID を追加できませんでした。(PLAT-148808)

<!--
- We have fixed an issue with the beta SFTP destination where the port number was previously hardcoded to 22. The port is now configurable for this destination. 

-->

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 表示名の切り替え | スキーマエディターでは、元のフィールド名と、さらに人間が読み取り可能な表示名との間で変更する切替スイッチを提供するようになりました。<br>![表示名の切り替えがハイライト表示されているスキーマエディター。](../../xdm/images/ui/resources/schemas/display-name-toggle.png "スキーマエディターの表示名の切り替え"){width="100" zoomable="yes"}<br>この柔軟性により、フィールドの検出性が向上し、スキーマを編集できます。標準フィールドグループの表示名はシステムで生成されますが、必要に応じて UI からカスタマイズすることもできます。詳しくは、[表示名の切り替えドキュメント](https://experienceleague.adobe.com/docs/experience-platform/xdm/ui/resources/schemas.html?lang=ja#display-name-toggle)を参照してください。 |

{style="table-layout:auto"}

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| スキーマ | [[!UICONTROL Adobe Target 分類フィールド]](https://github.com/adobe/xdm/pull/1719/files) | Target のアクティビティとエクスペリエンスを分類する一連のメタデータフィールドを含む、Target 分類データセット用の新しい XDM スキーマ。 |

{style="table-layout:auto"}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| フィールドグループ | [[!UICONTROL Adobe 統合プロファイルサービスアカウント和集合拡張機能]](https://github.com/adobe/xdm/pull/1696/files) | ユーザーがアカウント和集合にセグメントメンバーシップを追加できるようにするリアルタイム顧客プロファイルのアカウント拡張フィールドグループを追加しました。 |
| スキーマ | [[!UICONTROL 計算属性システムスキーマ]](https://github.com/adobe/xdm/pull/1696/files) | リアルタイム顧客プロファイルで使用される計算属性フィールドグループは、システムの読み取り専用グローバルスキーマに更新されました。 |
| フィールドグループ | 複数 | [[!UICONTROL 時系列スキーマ]](https://github.com/adobe/xdm/pull/1718/files)のフィールドとして複数のイベントを追加しました。 |
| フィールドグループ | プロファイルのロイヤルティの詳細 | 「プログラム名」から「アップグレード日」に `xdm:upgradeDate` の[タイトルを修正しました](https://github.com/adobe/xdm/pull/1717/files)。 |
| フィールドグループ | 複数 | [[!UICONTROL 決定項目]](https://github.com/adobe/xdm/pull/1714/files)の複数のフィールドが更新され、重複のネストされた階層が削除されました。 |

{style="table-layout:auto"}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## Real-Time Customer Data Platform

Experience Platform 上に構築された Real-time Customer Data Platform（[!DNL Real-Time CDP]）は、企業が既知および未知のデータを統合し、カスタマージャーニーを通じてインテリジェントな意思決定により顧客プロファイルをアクティブ化するのに役立ちます。[!DNL Real-Time CDP] では、複数のエンタープライズデータソースを組み合わせて、リアルタイムで顧客プロファイルを作成します。これらのプロファイルから構築されたセグメントをダウンストリームの宛先に送信し、すべてのチャネルとデバイスにわたって 1 対 1 のパーソナライズされた顧客体験を提供できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| Real-Time CDP ホームページの強化 | [Real-Time CDP ホームページ](https://experience.adobe.com)の強化により、外観が更新され、パフォーマンスが向上しました。ホームページは権限に応じた状態になり、アクセス権のある機能に関連するウィジェットが表示されます。詳しくは、[Real-Time CDP ホームページダッシュボードの概要](../../rtcdp/home-page-dashboards.md)を参照してください。 |
| 自己識別サーベイ | 自己識別サーベイは、Adobe Experience Platform UI ホームページで提供されている短いアンケートです。自己識別サーベイを使用して、Experience Platform の個人プロファイルを作成し、選択内容に基づいて調整されたガイドラインを受信します。詳しくは、[自己識別サーベイの概要](../../landing/self-identification.md)を参照してください。 |

[!DNL Real-Time CDP] について詳しくは、[[!DNL Real-Time CDP] 概要](../../rtcdp/overview.md)を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 偽名プロファイルデータの有効期限 | 偽名プロファイルデータの有効期限が一般に利用できるようになりました。このリリースでは、有効にすると、Experience Platform インスタンスから古い偽名プロファイルが継続的に削除されます。この機能と偽名プロファイルについて詳しくは、[偽名プロファイルデータの有効期限ガイド](../../profile/pseudonymous-profiles.md)を参照してください。 |

{style="table-layout:auto"}

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| セグメントメンバーシップマップ | 2023 年 5 月 15 日 (PT) に発表された前の発表に対するフォローアップとして、 `Existing` セグメントメンバーシップのライフサイクルでの冗長性を削除するために、セグメントメンバーシップマップでステータスが非推奨となります。 この変更後、セグメントで認定されたプロファイルは、 `Realized` 不適格なプロファイルは、引き続き次のように表されます。 `Exited`.<br/><br/> この変更は、 [企業の宛先](../../destinations/destination-types.md#streaming-profile-export) (Amazon Kinesis、Azure Event Hubs、HTTP API) の `Existing` ステータス。 該当する場合は、ダウンストリーム統合を確認してください。 特定の時間を超えて新たに認定されたプロファイルを識別することに関心がある場合は、セグメントメンバーシップマップで `Realized` ステータスと `lastQualificationTime` を組み合わせて使用することを検討してください。詳しくは、アドビ担当者にお問い合わせください。 |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むことができ、Platform サービスを使用してそのデータの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Microsoft Dynamics、Salesforce CRM および Salesforce Marketing Cloud の行レベルデータをフィルタリングするための API サポート | 論理演算子と比較演算子を使用すると、Microsoft Dynamics、Salesforce CRM および Salesforce Marketing Cloud ソースの行レベルデータをフィルタリングできます。[API を使用したソースのデータのフィルタリング](../../sources/tutorials/api/filter.md)に関するガイドを参照してください。 |
| Shopify ストリーミングのベータ版の提供 | [Shopify ストリーミングソース](../../sources/connectors/ecommerce/shopify-streaming.md)のベータ版が利用可能になりました。Shopify ストリーミングソースを使用すると、Shopify パートナーアカウントから Experience Platform にデータをストリーミングできます。 |
| OneTrust 統合の一般提供 | [OneTrust 統合ソース](../../sources/connectors/consent-and-preferences/onetrust.md)は一般提供（GA）版になりました。OneTrust 統合ソースを使用すると、OneTrust 統合アカウントから Experience Platform に同意と環境設定のデータを取り込むことができます。 |
| Oracle Service Cloud の一般提供 | [Oracle Service Cloud ソース](../../sources/connectors/customer-success/oracle-service-cloud.md)は一般提供（GA）版になりました。Oracle Service Cloud ソースを使用すると、Oracle Service Cloud データを Experience Platform に取り込むことができます。 |

{style="table-layout:auto"}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。