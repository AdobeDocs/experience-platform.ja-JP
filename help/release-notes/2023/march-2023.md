---
title: Adobe Experience Platform リリースノート（2023年3月）
description: Adobe Experience Platform の 2023年3月のリリースノート。
exl-id: 3f4d764a-77cd-4e4a-ae11-e97a23006a53
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '2020'
ht-degree: 82%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年3月29日（PT）**

Adobe Experience Platform の既存の機能のアップデート：

- [ダッシュボード](#dashboards)
- [データ収集](#data-collection)
- [データ準備](#data-prep)
- [宛先](#destinations)
- [エクスペリエンスデータモデル](#xdm)
- [クエリサービス](#query-service)
- [Real-Time Customer Data Platform B2B 版](#b2b)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## ダッシュボード {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを確認できる複数のダッシュボードを提供しています。

**新機能または更新された機能** {#dashboards-new-updated-features}

| 機能 | 説明 |
| --- | --- |
| ユーザー定義ダッシュボード | ユーザー定義ダッシュボードウィジェットコンポーザーでウィジェットに属性を追加する前に、**属性値をサンプリング**&#x200B;できるようになりました。ウィジェットを作成する際に、その属性列から得られるいくつかのサンプル値を個々の属性に使用できます。<br>軸スワップボタンを使用して、ウィジェットの **X 軸と Y 軸をスワップ**&#x200B;できるようになりました。これにより、ウィジェットに属性を追加する際の時間が節約され、エクスペリエンスもより人間工学的になります。この結果、属性パネルから両方の属性を再度見つける必要がなくなります。<br>ウィジェット内の&#x200B;**凡例の場所とタイトルを変更**&#x200B;できるようになりました。凡例がウィジェットに表示された後、その凡例をグラフ内の任意の場所に再配置できます。また、軸ラベルやウィジェットタイトルと同様に、凡例のタイトルの名前を変更することもできます。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Meta Conversions API (Beta) の新しいクイックスタートワークフロー | データ収集のホーム画面から「はじめに」の下にある新しいクイックスタートワークフローにアクセスします。[Meta Conversions API のクイックスタートワークフロー](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/meta/overview.html#quick-start?lang=ja)により、お客様はイベントデータを迅速に収集し、広告コンバージョンのためにわずか数回の簡単な手順で Meta にサーバーサイド転送できます。 |
| Mobile SDK（ベータ版）用の新しいクイックスタートワークフロー | データ収集のホーム画面から「はじめに」の下にある新しいクイックスタートワークフローにアクセスします。[Mobile SDK 用のクイックスタートワークフロー](https://developer.adobe.com/client-sdks/documentation/)を使用すると、Mobile SDK を迅速に実装し、基本的なモバイルイベントをわずか数回の簡単な手順で検証できます。 |
| [!DNL Braze] イベント転送拡張機能 | [[!DNL Braze Track Events API]](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/braze/overview.html?lang=ja) イベント転送拡張機能を使用すると、Adobe Experience Platform Edge Network で取得したデータを活用したり、[!DNL Braze] User Track API を使用してサーバーサイドイベントの形式で [!DNL Braze] に送信したりできます。 |
| [!DNL Epsilon] イベント転送拡張機能 | [[!DNL Epsilon Events API]](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/overview.html)拡張機能を使用すると、イベント転送を活用してAdobe Experience Platform Edge Networkのイベント情報を取得し、[!DNL Epsilon] Event APIを使用して[!DNL Epsilon]に送信できます。 |
| [!DNL Mixpanel] イベント転送拡張機能 | [[!DNL Mixpanel Track Events API]](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/braze/overview.html?lang=ja) 拡張機能を使用すると、お客様は、イベント転送を活用して、Adobe Experience Platform Edge Network で取得したイベント情報を、Track Events API を使用して Mixpanel に送信できます。 |

{style="table-layout:auto"}

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換および検証を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Analytics データに対するフィルタリングの一般提供 | データ準備機能を使用してルールと条件を適用し、Analytics データをリアルタイム顧客プロファイルに取り込む前にフィルタリングできるようになりました。詳しくは、[プロファイル取り込み用の Analytics データのフィルタリング](../../sources/tutorials/ui/create/adobe-applications/analytics.md#filtering-for-profile)に関するガイドを参照してください。 |
| URL 文字列のエンコードおよびデコード用の新しい関数 | <ul><li>`get_url_encoded` 関数は、URL を入力として受け取り、特殊文字を ASCII 文字に置換つまりエンコードします。</li><li>`get_url_decoded` 関数は、URL を入力として受け取り、ASCII 文字を特殊文字にデコードします。</li></ul> 詳しくは、[データ準備関数ガイド](../../data-prep/functions.md)を参照してください。予約文字とそれらに対応するエンコードされた文字の包括的なリストについては、[特殊文字](../../data-prep/functions.md#special-characters)に関するガイドを参照してください。 |

データ準備について詳しくは、[データ準備の概要](../../data-prep/home.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先** {#new-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!DNL Adobe Commerce] 接続の GA](../../destinations/catalog/personalization/adobe-commerce.md) | （一般提供されるようになった）[!DNL Adobe Commerce] 宛先コネクタを使用すると、[!DNL Adobe Commerce] アカウントに対してアクティブ化する Real-Time CDP オーディエンスを 1 つ以上選択して、パーソナライズした動的なエクスペリエンスを買い物客に提供できます。 |
| [[!DNL Snap Inc] 接続の GA](../../destinations/catalog/advertising/snap-inc.md) | （一般提供されるようになった）[!DNL Snap Inc] 宛先コネクタを使用すると、マーケターは、Experience Platform で作成したユーザーセグメントを [!DNL Snapchat Ads] に読み込んで、広告のターゲティングに使用できます。 |
| [（API）Oracle Eloqua 接続](../../destinations/catalog/email-marketing/oracle-eloqua-api.md) | [!DNL Oracle Eloqua] への API ベースの接続を使用すると、キャンペーンを計画および実行しながら、パーソナライズしたカスタマーエクスペリエンスを [!DNL Oracle Eloqua] で見込み客に提供することができます。 |
| [（ベータ版） [!DNL Amazon Ads] 接続](../../destinations/catalog/advertising/amazon-ads.md) | [!DNL Amazon Ads] と Adobe Experience Platform の統合により、[!DNL Amazon DSP (ADSP)] などの [!DNL Amazon Ads] 製品へのターンキー統合が可能になります。Adobe Experience Platform で [!DNL Amazon Ads] 宛先を使用すると、ターゲティングとアクティブ化のための広告主オーディエンスを [!DNL Amazon DSP] で定義できます。 |
| [[!DNL Marketo Measure Ultimate] 接続](../../destinations/catalog/adobe/marketo-measure-ultimate.md) | [!DNL Marketo Measure]（旧称 Bizible）を使用すると、マーケターは、会社の収益を増やし投資回収率を最大限に高めるのに最も効果的なマーケティング活動に関するインサイトを得ることができます。宛先により、Adobe Experience Platform から [!DNL Marketo Measure] への B2B データフローが可能になります。カードは、[!DNL Marketo Measure Ultimate] のお客様のみが使用できます。 |
| [TikTok 接続](../../destinations/catalog/social/tiktok.md) | お持ちのデータを使用して TikTok でカスタムオーディエンスを作成し、広告キャンペーンのターゲティングを行えます。 |
| [Zendesk 接続](../../destinations/catalog/crm/zendesk.md) | この宛先を使用すると、セグメント内の ID を [!DNL Zendesk] 内の連絡先として作成および更新できます。 |

{style="table-layout:auto"}

**新しい機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| 宛先の新しいアクセス制御権限：[[!DNL Activate Segments without Mapping]](../../access-control/home.md#permissions) | この新しい権限により、ユーザーは、[マッピングステップ](../../destinations/ui/activate-batch-profile-destinations.md#mapping)を非表示にして、セグメントを既存の宛先に対してアクティブ化できます。ユーザーは、アクティブ化ワークフローでセグメントを追加および削除できますが、マッピングされた属性や ID を追加または削除することはできません。 |

{style="table-layout:auto"}

**修正と機能強化** {#destinations-fixes-and-enhancements}

Real-Time CDPのファイルベースの宛先におけるPGP/GPG暗号化に関するバグ修正をリリースします。 この変更により、暗号化を現在使用している既存のファイルベース宛先では、以前とは異なる拡張子を持つファイル名を生成します。

- 暗号化を使用する場合の現在の拡張子：`filename.csv`
- 暗号化を使用する場合の今後の拡張子：`filename.csv.gpg`

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| CSV からのスキーマ生成に関するレコメンデーション | ローカルファイルをアップロードして、機械学習で生成されたスキーマを作成できるようになりました。これにより、手動でスキーマを作成する必要がなくなります。[!UICONTROL Sources] ワークスペースから、サンプルのCSV ファイルをアップロードすると、Adobe マシンラーニングアルゴリズムがターゲットフィールドに基づいたスキーマを提案します。 詳しくは、[ドキュメント](../../ingestion/tutorials/map-csv/recommendations.md)を参照してください。 |

{style="table-layout:auto"}

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| クラス | [[!UICONTROL Offer Item]](https://github.com/adobe/xdm/pull/1678/files) | Offerを表すクラス。 |
| クラス | [[!UICONTROL Decision Item]](https://github.com/adobe/xdm/pull/1678/files) | 決定の対象となる項目。 決定プロセスの出力は、1つ以上の決定項目です。 |
| クラス | [[!UICONTROL Media Session Server Timeout]](https://github.com/adobe/xdm/pull/1676/files) | これは、ユーザーの最後の既知のインタラクションからセッションが閉じられた瞬間までの間に経過した時間（秒単位）を示します。 |
| フィールドグループ | [[!UICONTROL XDM Profile Computed Attributes]](https://github.com/adobe/xdm/pull/1686/files) | これにより、社内Adobe サービスの計算属性が顧客データに追加されます。 これは、顧客がデータを取り込むために使用すべきではありません。 |
| データタイプ | [[!UICONTROL Refund Item]](https://github.com/adobe/xdm/pull/1685/files) | 払い戻しが注文に関連付けられているかどうかを示し、払い戻しのタイプ、金額、および関連する通貨を定義します。 |
| データタイプ | [[!UICONTROL Category data]](https://github.com/adobe/xdm/pull/1677/files) | この新しいデータタイプは、製品のカテゴリを表します。 |
| スキーマ | [[!UICONTROL Adobe Target Classification Fields]](https://github.com/adobe/xdm/pull/1682/files) | Target Classification データセット用に新しいXDM スキーマが作成されました。 Target アクティビティとエクスペリエンスを分類する一連のメタデータフィールドが含まれています。 |

{style="table-layout:auto"}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| フィールドグループ | [[!UICONTROL Content Component Details]](https://github.com/adobe/xdm/pull/1674/files) | `uri-reference`が[!UICONTROL Content Component Details]から削除されました |
| フィールドグループ | [[!UICONTROL AJO Entity tags]](https://github.com/adobe/xdm/pull/1672/files) | ジャーニーまたはCampaignに対応するAJO エンティティ タグを[!UICONTROL AJO Entity Fields]に追加しました |
| フィールドグループ | （複数） | [[!UICONTROL Journey Orchestration Step Event Common Fields]](https://github.com/adobe/xdm/pull/1671/files)に複数のフィールドを追加しました |
| フィールドグループ | （複数） | [[!UICONTROL Media Reporting]](https://github.com/adobe/xdm/pull/1670/files)の複数のXDM イベントタイプを追加しました。 |
| フィールドグループ | [!UICONTROL Workfront Change Event] | `Full Record`と`Accessor Employee Ids`のフィールドグループが追加されました。 |
| データタイプ | [[!UICONTROL Product list item]](https://github.com/adobe/xdm/pull/1685/files) | 項目に返金された金額を示すために、[!UICONTROL Refund Amount]が追加されました（ある場合）。 |
| データタイプ | [[!UICONTROL Order &#x200B;]](https://github.com/adobe/xdm/pull/1685/files) | この注文の返金リストに[!UICONTROL Refunds List]が追加されました。 |
| データタイプ | [[!UICONTROL Product List Item &#x200B;]](https://github.com/adobe/xdm/pull/1677/files) | 本製品のカテゴリーデータのリストに、製品カテゴリを追加しました。 |
| データタイプ | [!UICONTROL Session details information] | `pev3`がレポートに使用されるメディアストリームのタイプを示す[文字列フィールドを追加しました](https://github.com/adobe/xdm/pull/1676/files)。 また、`pccr` プロパティは、リダイレクトが発生したかどうかを示します。 |
| データタイプ | [!UICONTROL Requisition List] | [要求リストのプロパティ &#x200B;](https://github.com/adobe/xdm/pull/1675/files)を提供します。 名前、ID、説明が含まれます。 |
| データタイプ | [!UICONTROL Commerce] | [Commerce データ型が更新され](https://github.com/adobe/xdm/pull/1675/files)が、`requisitionListOpens`、`requisitionListAdds`、`requisitionListRemovals`および`requisitionList`が含まれるようになりました。 |

{style="table-layout:auto"}

Experience PlatformのXDMについて詳しくは、[XDM System overview](../../xdm/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。データレイクの任意のデータセットを結合したり、クエリ結果を新しいデータセットとして取得したりすることで、それらのデータセットをレポートやデータサイエンスワークスペースで使用したり、リアルタイム顧客プロファイルへの取り込みが可能になります。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 高速化ストアに対する属性ベースのアクセス制御 | Data Distiller で属性ベースのアクセス制御を使用すると、高速化ストア上のすべてのデータセットに対するアクセス制御を定義できます。これにより、ユーザーが作成して高速化ストアに保存されたカスタムデータモデルへのアクセスが制御されて、カスタムダッシュボードが強化されます。 |

{style="table-layout:auto"}

クエリサービスについて詳しくは、[クエリサービスの概要](../../query-service/home.md)を参照してください。

## Real-Time Customer Data Platform B2B エディション {#b2b}

Real-Time Customer Data Platform（Real-Time CDP）上に構築された Real-Time CDP B2B エディションは、B2B サービスモデルで業務を行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| バグ修正 | システム内のプロファイルをより正確に表すために、Real-Time Customer Data Platform B2B 版の合計プロファイル数やアドレス可能なオーディエンス指標に内部プロファイルが含まれなくなりました。本日以降、合計プロファイル数／アドレス可能なオーディエンス指標が一時的に低下する可能性があります。データが消去されてはいません。これは単にカウントの変更にすぎません。ご不明な点がございましたら、アドビの担当者にお問い合わせください。 |

{style="table-layout:auto"}

Real-Time CDP B2B Edition について詳しくは、[Real-Time CDP B2B Edition の概要](../../rtcdp/overview.md)を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| プロファイル指標 | プロファイル指標をより正確に表すために、メンバーシップの分類とチャーン指標が結合され、24 時間にわたって計算されるようになりました。詳しくは、[セグメント化 UI ガイド](../../segmentation/ui/overview.md#browse)を参照してください。 |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platformでは、外部ソースからデータを取り込み、Experience Platform サービスを使用してデータを構造化、ラベル付け、強化することができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Chatlio] のベータ版の提供 | [!DNL Chatlio] ソースのベータ版が利用可能になりました。[!DNL Chatlio] ソースを使用すると、[!DNL Chatlio] イベントデータを Experience Platform にストリーミングできます。詳しくは、[[!DNL Chatlio] 概要](../../sources/connectors/marketing-automation/chatlio-webhook.md)を参照してください。 |
| [!DNL Customer.io] のベータ版の提供 | [!DNL Customer.io] ソースのベータ版が利用可能になりました。[!DNL Customer.io] ソースを使用すると、顧客イベントデータを Experience Platform にストリーミングできます。詳しくは、[[!DNL Customer.io] 概要](../../sources/connectors/marketing-automation/customerio-webhook.md)を参照してください。 |
| [!DNL Pendo] のベータ版の提供 | [!DNL Pendo] ソースのベータ版が利用可能になりました。[!DNL Pendo] ソースを使用すると、製品分析データを Experience Platform にストリーミングできます。詳しくは、[[!DNL Pendo] 概要](../../sources/connectors/analytics/pendo-webhook.md)を参照してください。 |
| ドラフトデータフローのサポート | Flow Service API を使用して、データフローをドラフト状態に設定できるようになりました。ドラフトに設定したデータフローは、後で更新して新しい情報と共に公開できます。詳しくは、[ソースデータフローのドラフト設定](../../sources/tutorials/api/draft.md)に関するガイドを参照してください。 |

{style="table-layout:auto"}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
