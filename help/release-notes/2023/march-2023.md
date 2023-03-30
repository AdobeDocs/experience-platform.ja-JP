---
title: Adobe Experience Platform リリースノート 2023年3月
description: Adobe Experience Platform の 2023年3月のリリースノート。
source-git-commit: 582305583aa5869d8cc29f871aeb3fa0ef0b6ab1
workflow-type: tm+mt
source-wordcount: '1724'
ht-degree: 39%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年3月29日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [ダッシュボード](#dashboards)
- [データ収集](#data-collection)
- [データ準備](#data-prep)
- [宛先](#destinations)
- [エクスペリエンスデータモデル](#xdm)
- [クエリサービス](#query-service)
- [Real-Time Customer Data Platform B2B エディション](#b2b)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## ダッシュボード {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを表示できる複数のダッシュボードを提供しています。

**新機能または更新された機能** {#dashboards-new-updated-features}

| 機能 | 説明 |
| --- | --- |
| ユーザー定義ダッシュボード | 次の操作を実行できます。 **属性値のサンプル** 属性を追加する前に、ユーザー定義のダッシュボードウィジェットコンポーザーでウィジェットに属性を追加します。 ウィジェットを作成する際に、個々の属性に対してその属性列のいくつかの値を使用できます。<br>次の操作を実行できます。 **X 軸と Y 軸を交換します。** をクリックします。 これにより、時間を節約し、ウィジェットに属性を追加する際の人間工学に基づくエクスペリエンスを提供します。 これにより、アトリビュートパネルから両方のアトリビュートを再び検索する必要がなくなります。<br> 次の操作を実行できます。 **凡例の位置とタイトルを変更する** ウィジェット内で ウィジェットに凡例を配置した後は、軸ラベルやウィジェットタイトルと同様に、グラフ内の任意の場所に凡例を配置し直したり、凡例のタイトルの名前を変更したりできます。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)をお読みください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| メタコンバージョン API（ベータ版）の新しいクイックスタートワークフロー | データコレクションのホーム画面の「はじめに」の新しいクイックスタートワークフローにアクセスできます。 この [メタ変換 API のクイックスタートワークフロー](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/meta/overview.html?lang=en#quick-start) を使用すれば、わずか数回の簡単な手順で、サーバー側から Meta へと迅速にイベントデータを収集し、転送して広告コンバージョンをおこなうことができます。 |
| Mobile SDK（ベータ版）用の新しいクイックスタートワークフロー | データコレクションのホーム画面の「はじめに」の新しいクイックスタートワークフローにアクセスできます。 この [Mobile SDK のクイックスタートワークフロー](https://developer.adobe.com/client-sdks/documentation/) を使用すると、簡単な手順で Mobile SDK をすばやく実装し、基本的なモバイルイベントを検証できます。 |
| [!DNL Braze] イベント転送拡張機能 | この [[!DNL Braze Track Events API]](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/braze/overview.html) イベント転送拡張機能を使用すると、Adobe Experience Platform Edge Network で取り込んだデータを活用し、に送信できます。 [!DNL Braze] を使用して、サーバー側のイベントの形式で [!DNL Braze] ユーザー追跡 API |
| [!DNL Mixpanel] イベント転送拡張機能 | この [[!DNL Mixpanel Track Events API]](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/braze/overview.html) 拡張機能を使用すると、イベント転送を活用してAdobe Experience Platform Edge Network でイベント情報をキャプチャし、イベント追跡 API を使用して Mixpanel に送信できます。 |

{style="table-layout:auto"}

## データ準備 {#data-prep}

Data Prep を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換、検証をおこなうことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Analyticsデータのフィルタリングの一般的な可用性 | Data Prep 機能を使用して、ルールと条件を適用し、Analytics データをリアルタイム顧客プロファイルに取り込む前にフィルタリングできるようになりました。 詳しくは、 [プロファイル取り込み用の Analytics データのフィルタリング](../../sources/tutorials/ui/create/adobe-applications/analytics.md#filtering-for-profile). |
| URL 文字列のエンコードとデコードに関する新しい関数 | <ul><li>この `get_url_encoded` 関数は、URL を入力として受け取り、特殊文字を ASCII 文字に置き換えたり、エンコードしたりします。</li><li>この `get_url_decoded` 関数は、URL を入力として受け取り、ASCII 文字を特殊文字にデコードします。</li></ul> 詳しくは、 [データ準備関数ガイド](../../data-prep/functions.md). 予約文字とその対応するエンコード済み文字の包括的なリストについては、 [特殊文字](../../data-prep/functions.md#special-characters). |

Data Prep の詳細については、 [データ準備の概要](../../data-prep/home.md).

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先** {#new-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!DNL Adobe Commerce] 接続 GA](../../destinations/catalog/personalization/adobe-commerce.md) | この [!DNL Adobe Commerce] 宛先コネクタ（一般に提供されるようになりました）を使用すると、アクティブ化する 1 つ以上のReal-Time CDPオーディエンスを選択して、 [!DNL Adobe Commerce] 顧客に動的にパーソナライズされたエクスペリエンスを提供するアカウント。 |
| [[!DNL Snap Inc] 接続 GA](../../destinations/catalog/advertising/snap-inc.md) | この [!DNL Snap Inc] 宛先コネクタ（一般提供）を使用すると、マーケターは、Experience Platformで作成されたユーザーセグメントを [!DNL Snapchat Ads] 広告のターゲティングに使用します。 |
| [(API)OracleEloqua 接続](../../destinations/catalog/email-marketing/oracle-eloqua-api.md) | API ベースの接続を使用して [!DNL Oracle Eloqua] キャンペーンの計画と実行を行い、 [!DNL Oracle Eloqua]. |
| [（ベータ版） [!DNL Amazon Ads] 接続](../../destinations/catalog/advertising/amazon-ads.md) | この [!DNL Amazon Ads] Adobe Experience Platformとの統合により、 [!DNL Amazon Ads] 製品 ( [!DNL Amazon DSP (ADSP)]. の使用 [!DNL Amazon Ads] Adobe Experience Platformの宛先では、ユーザーは、ターゲティングとアクティブ化のための広告主オーディエンスを定義できます。 [!DNL Amazon DSP]. |
| [[!DNL Marketo Measure Ultimate] 接続](../../destinations/catalog/adobe/marketo-measure-ultimate.md) | [!DNL Marketo Measure] （以前の Bizible）は、マーケターに、売上高の促進と、会社の投資回収率の最大化に最も効果的なマーケティング活動に関するインサイトを提供します。 宛先により、Adobe Experience Platformから [!DNL Marketo Measure]. カードは次の場所でのみ使用できます： [!DNL Marketo Measure Ultimate] 顧客。 |
| [TikTok接続](../../destinations/catalog/social/tiktok.md) | 広告キャンペーンでターゲティングするためのデータを使用して、TikTok上にカスタムオーディエンスを構築します。 |
| [Zendesk 接続](../../destinations/catalog/crm/zendesk.md) | この宛先を使用して、セグメント内の ID を内の連絡先として作成および更新します [!DNL Zendesk]. |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| 宛先の新しいアクセス制御権限： [[!DNL Activate Segments without Mapping]](../../access-control/home.md#permissions) | 新しい権限により、ユーザーは、 [マッピング手順](../../destinations/ui/activate-batch-profile-destinations.md#mapping). ユーザーは、アクティベーションワークフローでセグメントを追加および削除できますが、マッピングされた属性や ID を追加または削除することはできません。 |

{style="table-layout:auto"}

**修正および機能強化** {#destinations-fixes-and-enhancements}

リアルタイム CDP のファイルベースの宛先で、PGP/GPG 暗号化のバグ修正をリリースしています。 この変更により、現在暗号化を使用している既存のファイルベースの宛先は、以前とは異なる拡張子を持つファイル名を生成します。

- 暗号化を使用する場合の現在の拡張： `filename.csv`
- 暗号化を使用する場合の将来の拡張： `filename.csv.gpg`

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| CSV からスキーマへのレコメンデーション | これで、ローカルファイルをアップロードして、機械学習で生成されたスキーマを作成し、スキーマを手動で作成する必要がなくなりました。 次の [!UICONTROL ソース] workspace、サンプル CSV ファイルのアップロード、Adobeの機械学習アルゴリズムにより、ターゲットフィールドに基づいてスキーマが提案されます。 詳しくは、 [ドキュメント](../../ingestion/tutorials/map-csv/recommendations.md) 詳細は」 |

{style="table-layout:auto"}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。データレイクの任意のデータセットを結合したり、クエリ結果を新しいデータセットとして取得したりすることで、それらのデータセットをレポートやデータサイエンスワークスペースで使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 高速ストアのアクセス制御に基づく属性 | Data Distillerと共に属性ベースのアクセス制御を使用して、高速ストア上のすべてのデータセットに対するアクセス制御を定義します。 これは、ユーザーが作成し、高速ストアに保存されてカスタムダッシュボードを強化するカスタムデータモデルへのアクセスを制御します。 |

{style="table-layout:auto"}

クエリサービスについて詳しくは、[クエリサービスの概要](../../query-service/home.md)を参照してください。

## Real-Time Customer Data Platform B2B エディション {#b2b}

Real-Time Customer Data Platform（Real-Time CDP）上に構築された Real-Time CDP B2B エディションは、B2B サービスモデルで業務を行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| バグ修正 | システム内のプロファイルをより正確に表現するために、Real-time Customer Data Platform B2B Edition のプロファイル総数やアドレス可能なオーディエンス指標に内部プロファイルが含まれなくなりました。 今日から、合計プロファイル数/アドレス可能なオーディエンス指標に 1 回限りの減少が表示される場合があります。 データが消去されたことはありません。これは単にカウントの変更に過ぎません。 ご不明な点はAdobeの担当者にお問い合わせください |

{style="table-layout:auto"}

Real-Time CDP B2B Edition について詳しくは、[Real-Time CDP B2B Edition の概要](../../rtcdp/overview.md)を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| プロファイル指標 | プロファイル指標をより正確に表すために、メンバーシップの分類とチャーン指標は組み合わされ、24 時間にわたって計算されるようになりました。 詳しくは、 [セグメント化 UI ガイド](../../segmentation/ui/overview.md#browse) |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むことができ、Platform サービスを使用してそのデータの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Chatlio] のベータ版の可用性 | この [!DNL Chatlio] ベータ版でのソースの利用が可能になりました。 以下を使用： [!DNL Chatlio] ストリーミングするソース [!DNL Chatlio] イベントデータからExperience Platformへ。 詳しくは、[[!DNL Chatlio] 概要](../../sources/connectors/marketing-automation/chatlio-webhook.md)を参照してください。 |
| [!DNL Customer.io] のベータ版の可用性 | この [!DNL Customer.io] ベータ版でのソースの利用が可能になりました。 以下を使用： [!DNL Customer.io] 顧客イベントデータをExperience Platformにストリーミングするソース。 詳しくは、[[!DNL Customer.io] 概要](../../sources/connectors/marketing-automation/customerio-webhook.md)を参照してください。 |
| [!DNL Pendo] のベータ版の可用性 | この [!DNL Pendo] ベータ版でのソースの利用が可能になりました。 以下を使用： [!DNL Pendo] 製品分析データをExperience Platformにストリーミングするソース。 詳しくは、[[!DNL Pendo] 概要](../../sources/connectors/analytics/pendo-webhook.md)を参照してください。 |
| ドラフトデータフローのサポート | これで、フローサービス API を使用して、データフローをドラフト状態に設定できます。 下書きのデータフローは、後で更新し、新しい情報で公開できます。 詳しくは、 [ソースのデータフローをドラフトとして設定](../../sources/tutorials/api/draft.md). |

{style="table-layout:auto"}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
