---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: c4cd691eeae9e27dd7616dc19672dc5d08b8cec7
workflow-type: tm+mt
source-wordcount: '2327'
ht-degree: 31%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 7 月 27 日**

Adobe Experience Platform の既存の機能に対するアップデート：

- [ダッシュボード](#dashboards)
- [データ収集](#data-collection)
- [[!DNL Data Prep]](#data-prep)
- [[!DNL Destinations]](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [Real-time Customer Data Platform B2B エディション](#b2b)
- [リアルタイム顧客プロファイル](#profile)
- [ソース](#sources)

## ダッシュボード {#dashboards}

Adobe Experience Platform は、毎日のスナップショットで取得した、組織のデータに関する重要な情報を表示できる複数の [!DNL dashboards] を提供しています。

### アカウントプロファイルダッシュボード

アカウントプロファイルダッシュボードには、マーケティングチャネル全体の複数のソースおよび顧客アカウント情報の保存に現在使用している様々なシステムからの統合アカウント情報のスナップショットが表示されます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 業界ウィジェット別の合計アカウント数 | このウィジェットは、1 つの指標のアカウントの合計数を表示し、ドーナツグラフを使用して、全体の数を構成する業種の数の比例サイズを示します。 |
| アカウントプロファイル追加ウィジェット | このウィジェットは、色分けされた棒グラフを使用して、特定の期間にアカウントに追加されたプロファイルの数と、追加されたプロファイルを構成する様々な業界の割合を示します。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [リアルタイム CDP、B2B エディションの概要](../../rtcdp/b2b-overview.md) 使用可能な B2B 機能の詳細、または [エンドツーエンドのチュートリアル](../../rtcdp/b2b-tutorial.md) B2B ワークフローの一部としてアカウントプロファイルを作成する方法の詳細を説明します。

アカウントプロファイル関連指標を視覚化するために使用できるウィジェットについて詳しくは、 [アカウントプロファイルウィジェットドキュメント](../../dashboards/guides/account-profiles.md#standard-widgets).

### プロファイルダッシュボード

プロファイルダッシュボードは、組織が Experience Platform のプロファイルストア内に持つ属性（レコード）データのスナップショットを表示します。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| マッピングされたオーディエンスウィジェット | このウィジェットには、プロファイルダッシュボードドロップダウンで選択した宛先に対してアクティブ化できる、マッピングされたオーディエンスの合計数が表示されます。 |

プロファイルダッシュボードについて詳しくは、 [プロファイルダッシュボードの概要](../../dashboards/guides/profiles.md).

### 宛先ダッシュボード

宛先ダッシュボードは、組織が Experience Platform 内で有効にしている宛先のスナップショットを表示します。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| オーディエンスウィジェット | このウィジェットは、プロファイルデータに適用された選択した結合ポリシーに従って、アクティブ化する準備ができたセグメントの合計数を提供します。 |

{style=&quot;table-layout:auto&quot;}

宛先ダッシュボードについて詳しくは、 [宛先ダッシュボードの概要](../../dashboards/guides/destinations.md).

## データ収集 {#collection}

Adobe Experience Platform は、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信します。そこでデータを強化、変換、アドビまたはアドビ以外の宛先への配信を可能にする一連のテクノロジーを提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Admin Consoleによる権限管理 | データ収集機能へのアクセスは、Adobe Experience Platformデータ収集用カードのAdobe Admin Consoleで管理されるようになりました。 詳しくは、 [データ収集権限](../../collection/permissions.md) を参照してください。<br><br>また、Adobe Experience Platformのカードの下のAdmin Consoleでもデータストリームの権限を管理できるようになり、以前の方法で各ユーザーに対して手動でこれらの権限を設定した場合のセキュリティが向上しました。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、「[データ収集の概要](../../collection/home.md)」を参照してください。

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータをマッピング、変換および検証できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| の機能強化 [!DNL Data Prep] Recommendations | [!DNL Data Prep] Recommendationsは今、より賢く、より速くなっています。 新しい検証チェックにより、最も一般的なマッピングエラーが大幅に減少し、価値創出までの時間が大幅に短縮されます。 |
| ストリーミングアップセットの階層サポート | 関数を使用できるようになりました `upsert_array_append` および `upsert_array_replace` を使用して、アップサートをプロファイルにストリーミングする際に配列とオブジェクトを更新する。 詳しくは、 [[!DNL Data Prep] マッピング関数ガイド](../../data-prep/functions.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、以下を参照してください。 [!DNL Data Prep]を参照し、 [[!DNL Data Prep] 概要](../../data-prep/home.md).

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ----------- | ----------- |
| [今すぐファイルを書き出す（ベータ版）](../../destinations/ui/export-file-now.md) | 以前にスケジュールされたセグメントの現在の書き出しスケジュールを中断することなく、完全なファイルを書き出す。 このエクスポートは、以前にスケジュールされたエクスポートに加えておこなわれ、セグメントのエクスポート頻度は変更されません。 <br> ファイルのエクスポートが直ちにトリガーされ、Experience Platformのセグメント化の実行から最新の結果が取得されます。 <br> <br>この機能へのアクセスについては、Adobe担当者にお問い合わせください。 |

{style=&quot;table-layout:auto&quot;}

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [Marketo V2](../../destinations/catalog/adobe/marketo-engage.md) | Marketo Engage先を更新すると、自動化を使用して静的リストの作成プロセスを合理化し、ユーザーがリードに追加のフィールドを取り込めるようになります。 以下のMarketo V2 の機能強化の詳細を参照してください。 <br><ul><li>内 **[!UICONTROL セグメントをスケジュール]** アクティベーションワークフローの手順のMarketo V1 では、手動で **マッピング ID** データをMarketoに正常に書き出すために使用します。 この手動の手順は、Marketo V2 では不要になりました。</li><li>内 **[!UICONTROL マッピング]** アクティベーションワークフローの手順で、Marketo V1 で、Marketoで XDM フィールドを 3 つのターゲットフィールドにのみマッピングできました。 `firstName`, `lastName`、および `companyName`. Marketo V2 リリースで、Marketoで XDM フィールドを他の多数のフィールドにマッピングできるようになりました。 詳しくは、 [Marketo V2 でサポートされる属性](../../destinations/catalog/adobe/marketo-engage.md#supported-attributes).  </li></ul> |
| [Pega Customer Decision Hub](../../destinations/catalog/personalization/pega.md) | Pega Customer Decision Hub のAdobe Experience Platformのプロファイル属性とセグメントメンバーシップ情報を、アダプティブモデルの述語として使用し、次に最適な判定を行うのに役立ちます。 |
| [(API)SalesforceMarketing Cloud](../../destinations/catalog/email-marketing/salesforce-marketing-cloud-exact-target.md) | この宛先を使用すると、マーケターは、Experience Platformで作成したユーザーセグメントを Snapchat 広告にインポートし、広告のターゲティングに使用できます。 |
| [Salesforce CRM](../../destinations/catalog/crm/salesforce.md) | SalesforceMarketing Cloudの連絡先情報を更新し、Experience Platformのプロファイルとセグメント情報を表示 |
| [(ベータ) [!DNL Snap Inc.]](../../destinations/catalog/advertising/snap-inc.md) | この宛先を使用すると、マーケターは、Experience Platformで作成したユーザーセグメントを Snapchat 広告にインポートし、広告のターゲティングに使用できます。 <br><br>この宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。 |
| [（ベータ版） [!DNL Trade Desk] - CRM 接続](../../destinations/catalog/advertising/tradedesk-emails.md) | 用途 [!DNL The Trade Desk] 自分のに対してプロファイルをアクティブ化するための CRM 宛先 [!DNL Trade Desk] CRM データに基づくオーディエンスのターゲティングおよび抑制のアカウント。 <br><br>この宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。 |

{style=&quot;table-layout:auto&quot;}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 医療業界のデータモデル | 標準的な医療データモデルを導入し、デジタル獲得の増加、プログラム登録の向上、薬剤情報の促進に関連する 5 つの一般的な業界ユースケースをサポートしました。 概要については、 [医療データモデル](../../xdm/schema/industries/healthcare.md) これらの使用例と、それらをサポートする標準の XDM コンポーネントについて詳しくは、を参照してください。<br><br>新しい業界フィルターが [!UICONTROL スキーマ] カスタムスキーマを構築する際に、医療関連のコンポーネントを参照するのに役立つ UI。 |

{style=&quot;table-layout:auto&quot;}

**新しい XDM コンポーネント**

>[!WARNING]
>
>次の表に示す新しい XDM コンポーネントは、試行用で、現在テスト中です。 これらのコンポーネントは、安定化する前に、（必要に応じて）大幅な変更で更新されることが予想されます。 それに応じて開発作業を計画してください。

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| クラス | [[!UICONTROL 天気]](https://github.com/adobe/xdm/blob/master/components/classes/weather.schema.json) | 気象データのキャプチャに使用されるレコードベースのクラス。 |
| フィールドグループ | [[!UICONTROL 現在の天気]](https://github.com/adobe/xdm/blob/master/components/classes/weather.schema.json) | のフィールドグループ [!UICONTROL XDM ExperienceEvent] および [!UICONTROL 天気] 郵便番号の現在の天気条件を取得するクラスです。 |
| フィールドグループ | [[!UICONTROL 予測天気]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/forecasted-weather.schema.json) | のフィールドグループ [!UICONTROL XDM ExperienceEvent] および [!UICONTROL 天気] 郵便番号の予測気象条件をキャプチャするために使用するクラスです。 |
| フィールドグループ | [[!UICONTROL 製品トリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/product-triggers.schema.json) | のフィールドグループ [!UICONTROL XDM ExperienceEvent] および [!UICONTROL 天気] クラス。消費者の行動を促す気象条件を活用する、製品固有のトリガーをキャプチャするために使用します。 |
| フィールドグループ | [[!UICONTROL 相対トリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/relative-triggers.schema.json) | のフィールドグループ [!UICONTROL XDM ExperienceEvent] および [!UICONTROL 天気] クラス。消費者の行動を促すという既知の気象トリガーを活用する相対的な環境をキャプチャするために使用します。 |
| フィールドグループ | [[!UICONTROL 重大なトリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/severe-triggers.schema.json) | のフィールドグループ [!UICONTROL XDM ExperienceEvent] および [!UICONTROL 天気] クラス。消費者の行動を促すと知られている厳しい気象条件を活用するトリガーをキャプチャするために使用します。 |
| フィールドグループ | [[!UICONTROL 気象トリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/weather-triggers.schema.json) | のフィールドグループ [!UICONTROL XDM ExperienceEvent] および [!UICONTROL 天気] クラス。消費者の行動を促すと知られている気象条件を活用する一般的なトリガーをキャプチャするために使用します。 |
| フィールドグループ | [[!UICONTROL MediaCollection インタラクションの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-media-collection.schema.json) | のフィールドグループ [!UICONTROL XDM ExperienceEvent] メディアインタラクションに関する詳細をキャプチャするクラス。 |
| フィールドグループ | [[!UICONTROL MediaReporting インタラクションの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-media-reporting.schema.json) | のフィールドグループ [!UICONTROL XDM ExperienceEvent] メディアレポートとのインタラクションに関する詳細をキャプチャするクラス。 |
| データタイプ | [[!UICONTROL 広告の詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/advertisingdetails.schema.json) | 広告アセットに関する詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL 広告ポッドの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/advertisingpoddetails.schema.json) | 単一の広告ブレーク内で再生される複数の広告のシーケンスである、広告ポッドに関する詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL チャプターの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/chapterdetails.schema.json) | ビデオコンテンツのチャプターまたはセグメントに関する詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL エラーの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/errordetails.schema.json) | ビデオ再生エラーに関する詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL プレーヤーイベントの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/playereventdetails.schema.json) | 再生ヘッドの位置やセッション ID など、ビデオプレーヤーに関するイベント関連の詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL プレーヤーステートデータ情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/playerstatedata.schema.json) | ビデオプレーヤーの状態関連の詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL データの詳細情報の問い合わせ]](https://github.com/adobe/xdm/blob/master/components/datatypes/qoedatadetails.schema.json) | ビデオ再生イベントに関する Quality-of-Experience(QoE) の詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL セッションの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/sessiondetails.schema.json) | ビデオ再生イベントに関するセッションの詳細をキャプチャします。 |

{style=&quot;table-layout:auto&quot;}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## Real-time Customer Data Platform B2B エディション {#b2b}

Real-time Customer Data Platform（Real-time CDP）上に構築された Real-time CDP B2B Edition は、B2B サービスを行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| リード — アカウントマッチング | リード — アカウントの照合により、既知の担当者プロファイルをアカウントプロファイルに参加させることができます。 その後、アカウントやオポチュニティなどの B2B コンテキストでデータをセグメント化し、ターゲットに設定できます。 毎日実行されるジョブは、決定論的および確率論的要因の両方を使用して、どのアカウントにもまだ関連付けられていないユーザープロファイルを最も一致したアカウントに一致させます。 その後、セグメント定義にそのような一致を含めるかどうかを決定できます |

詳しくは、 [アカウントマッチングにつながる](../../rtcdp/b2b-ai-ml-services/lead-to-account-matching.md).

プロファイルエンリッチメントの監視方法に関するガイドについては、 [UI でのプロファイルエンリッチメントの監視](../../dataflows/ui/b2b/monitor-profile-enrichment.md).

リードとアカウントのマッチングの設定方法については、 [アカウントプロファイル UI ガイド](../../rtcdp/account/../accounts/account-profile-ui-guide.md?lang=en#configure-lead-to-account-matching).

Real-time CDP B2B Edition の詳細については、 [リアルタイム CDP B2B の概要](../../rtcdp/overview.md).

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

| 機能 | 説明 |
| ------- | ----------- |
| 孤立したプロファイルエッジ属性のクリーンアップ（限られたリリース） | 組織がこの機能にアクセスできる場合、プロファイルサービスは、ユーザーアクティビティ領域の残りのエッジ属性を毎日削除して、システム内のプロファイルをより正確に表示できるようになりました。 このクリーンアップは、特定のプロファイルのすべてのプロファイルフラグメントが削除された後に発生し、が含まれるデータセットから結合されるプロファイルに影響を与えます `com_adobe_aep_profile_region_dataset` が true とマークされている。 このリリース以前の残りのエッジ属性フラグメントがこれらの指標に含まれていたので、ライセンス使用状況ダッシュボードの「アドレス可能なオーディエンス」指標にドロップが表示されたり、プロファイルダッシュボードの「プロファイル数」指標にドロップが表示されたりします。 |

{style=&quot;table-layout:auto&quot;}

プロファイルデータを操作するためのチュートリアルやベストプラクティスなど、リアルタイム顧客プロファイルの詳細については、[リアルタイム顧客プロファイルの概要](../../profile/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| の一般リリース [!DNL Azure Data Explorer] ソース | AzureData Explorerソースを使用して、 [!DNL Azure] インスタンスからExperience Platformへ。 詳しくは、[[!DNL Azure Data Explorer] ソースの概要](../../sources/connectors/databases/data-explorer.md)を参照してください。 |
| の一般公開 [!DNL Generic OData] ソース | 以下を使用： [!DNL Generic OData] ソース：オープンデータプロトコルをサポートするシステムからExperience Platformにリソースを取り込みます。 詳しくは、[[!DNL Generic OData] ソースの概要](../../sources/connectors/protocols/odata.md)を参照してください。 |
| のソースファイルプロパティの自動検出のサポート [!DNL Data Landing Zone] Experience PlatformUI | この [!DNL Data Landing Zone] ソースで、Experience PlatformUI を使用する際のファイルプロパティの自動検出がサポートされるようになりました。 詳しくは、[ [!DNL Data Landing Zone]  ソース接続の作成](../../sources/tutorials/ui/create/cloud-storage/data-landing-zone.md)のドキュメントを参照してください。 |

{style=&quot;table-layout:auto&quot;}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
