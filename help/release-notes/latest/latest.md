---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: a82381d6133fe793fc0f4be38b6e064684581afb
workflow-type: tm+mt
source-wordcount: '2436'
ht-degree: 95%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年7月27日（PT）**

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

アカウントプロファイルダッシュボードには、マーケティングチャネル全体の複数のソースおよび顧客アカウント情報の保存に現在使用されている様々なシステムからの統合アカウント情報のスナップショットが表示されます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 業界ウィジェット別のアカウント総数 | このウィジェットは、アカウントの合計数を 1 つの指標で表示し、ドーナツグラフを使用して、全体の数に占める各業種の割合を、数に比例したサイズで示します。 |
| アカウントプロファイル追加ウィジェット | このウィジェットは、色分けされた棒グラフを使用して、特定の期間にアカウントに追加されたプロファイルの数と、追加されたプロファイルを構成する様々な業界の割合を示します。 |

{style=&quot;table-layout:auto&quot;}

利用可能な B2B 機能の詳細については、[Real-Time CDP、B2B エディションの概要](../../rtcdp/b2b-overview.md)、または B2B ワークフローの一部としてアカウントプロファイルを作成する方法の詳細については、[エンドツーエンドのチュートリアル](../../rtcdp/b2b-tutorial.md)を参照してください。

アカウントプロファイル関連指標を視覚化するために使用できるウィジェットについて詳しくは、[アカウントプロファイルウィジェットのドキュメント](../../dashboards/guides/account-profiles.md#standard-widgets)を参照してください。

### プロファイルダッシュボード

プロファイルダッシュボードは、組織が Experience Platform のプロファイルストア内に持つ属性（レコード）データのスナップショットを表示します。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| マッピングされたオーディエンスウィジェット | このウィジェットには、プロファイルダッシュボードのドロップダウンで選択した宛先に対してアクティブ化できる、マッピングされたオーディエンスの合計数が表示されます。 |

プロファイルダッシュボードについて詳しくは、[プロファイルダッシュボードの概要](../../dashboards/guides/profiles.md)を参照してください。

### 宛先ダッシュボード

宛先ダッシュボードは、組織が Experience Platform 内で有効にしている宛先のスナップショットを表示します。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| オーディエンスウィジェット | このウィジェットは、プロファイルデータに適用された選択した結合ポリシーに従って、アクティブ化する準備ができているセグメントの合計数を提供します。 |

{style=&quot;table-layout:auto&quot;}

宛先ダッシュボードについて詳しくは、[宛先ダッシュボードの概要](../../dashboards/guides/destinations.md)を参照してください。

## データ収集 {#collection}

Adobe Experience Platform は、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信します。そこでデータを強化、変換、アドビまたはアドビ以外の宛先への配信を可能にする一連のテクノロジーを提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Admin Console による権限管理 | データ収集機能へのアクセスは、Adobe Experience Platform データ収集用カードの Adobe Admin Console で管理されるようになりました。 詳しくは、[データ収集の権限](../../collection/permissions.md)に関するガイドを参照してください。<br><br>また、データストリームの権限も、Adobe Experience Platform のカードの下の Admin Console で管理できるようになり、各ユーザーに対してこれらの権限を手動で設定する以前の方法よりもセキュリティが向上しています。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、「[データ収集の概要](../../collection/home.md)」を参照してください。

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータをマッピング、変換および検証できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Data Prep] レコメンデーションに対する機能強化 | [!DNL Data Prep] レコメンデーションが、より賢く、より高速になりました。新しい検証チェックにより、最も一般的なマッピングエラーを大幅に削減し、価値創出までの時間をさらに短縮します。 |
| ストリーミングアップセットの階層サポート | アップサートをプロファイルにストリーミングする際に、関数 `upsert_array_append` および `upsert_array_replace` を使用して配列とオブジェクトを更新できるようになりました。詳しくは、[[!DNL Data Prep] マッピング関数ガイド](../../data-prep/functions.md)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

[!DNL Data Prep] について詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ----------- | ----------- |
| [今すぐファイルを書き出す（ベータ版）](../../destinations/ui/export-file-now.md) | 以前にスケジュールされたセグメントの現在の書き出しスケジュールを中断することなく、完全なファイルを書き出します。 この書き出しは、以前にスケジュールされた書き出しに加えて行われ、セグメントの書き出し頻度は変更されません。 <br> ファイルの書き出しが直ちにトリガーされ、Experience Platform のセグメント化の実行から最新の結果が取得されます。<br> <br>この機能へのアクセス権については、アドビ担当者にお問い合わせください。 |

{style=&quot;table-layout:auto&quot;}

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [Marketo V2](../../destinations/catalog/adobe/marketo-engage.md) | Marketo Engage の宛先を更新すると、自動化を使用して静的リストの作成プロセスを合理化し、ユーザーがリードに追加のフィールドを取り込めるようになります。 以下の Marketo V2 の機能強化の詳細を参照してください。<br><ul><li>Marketo V1 では、アクティベーションワークフローの&#x200B;**[!UICONTROL セグメントをスケジュール]**&#x200B;手順で、**マッピング ID** を手動で追加して、Marketo にデータを正常に書き出す必要がありました。この手動の手順は、Marketo V2 では不要になりました。</li><li>Marketo V1 では、アクティベーションワークフローの&#x200B;**[!UICONTROL マッピング]**&#x200B;手順で、XDM フィールドを Marketo の 3 つのターゲットフィールド（`firstName`、`lastName`、および `companyName`）にのみマッピングできました。  Marketo V2 リリースで、XDM フィールドを Marketo の多数のフィールドにマッピングできるようになりました。 詳しくは、[Marketo V2 でサポートされる属性](../../destinations/catalog/adobe/marketo-engage.md#supported-attributes)をご覧ください。  </li></ul> |
| [Pega Customer Decision Hub](../../destinations/catalog/personalization/pega.md) | Pega Customer Decision Hub の Adobe Experience Platform のプロファイル属性とセグメントメンバーシップ情報を適応モデルの予測因子として使用し、次善のアクションの意思決定に役立てます。 |
| [（API）Salesforce Marketing Cloud](../../destinations/catalog/email-marketing/salesforce-marketing-cloud-exact-target.md) | この宛先を使用すると、マーケターは、Experience Platform で作成したユーザーセグメントを Snapchat 広告にインポートし、広告のターゲティングに使用できます。 |
| [Salesforce CRM](../../destinations/catalog/crm/salesforce.md) | Experience Platform のプロファイルとセグメント情報を使用して、Salesforce Marketing Cloud の連絡先情報を更新する |
| [（ベータ版） [!DNL Snap Inc.]](../../destinations/catalog/advertising/snap-inc.md) | この宛先を使用すると、マーケターは、Experience Platform で作成したユーザーセグメントを Snapchat 広告にインポートし、広告のターゲティングに使用できます。 <br><br>この宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。 |
| [（ベータ版） [!DNL Trade Desk] - CRM 接続](../../destinations/catalog/advertising/tradedesk-emails.md) | [!DNL The Trade Desk] CRM 宛先を使用して、CRM データに基づくオーディエンスのターゲティングおよび抑制のために、プロファイルを [!DNL Trade Desk] アカウントにアクティベートします。<br><br>この宛先は現在ベータ版です。 ドキュメントと機能は変更される場合があります。 |

{style=&quot;table-layout:auto&quot;}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| 医療業界のデータモデル | 標準的な医療データモデルを導入し、デジタル獲得の増加、プログラム登録の向上、薬剤情報の促進に関連する 5 つの一般的な業界ユースケースをサポートします。 これらの使用例とそれらをサポートする標準 XDM コンポーネントの詳細については、[医療データモデル](../../xdm/schema/industries/healthcare.md)の概要を参照してください。<br><br>[!UICONTROL スキーマ] UI に新しい業界フィルターが追加され、カスタムスキーマを構築するときに医療関連のコンポーネントを参照できるようになりました。 |

{style=&quot;table-layout:auto&quot;}

**新しい XDM コンポーネント**

>[!WARNING]
>
>次の表に示す新しい XDM コンポーネントは、試行用で、現在テスト中です。 これらのコンポーネントは、安定化するまで、（必要に応じて）大幅な変更で更新されることが予想されます。 それに応じて開発作業を計画してください。

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| クラス | [[!UICONTROL 天気]](https://github.com/adobe/xdm/blob/master/components/classes/weather.schema.json) | 気象データのキャプチャに使用されるレコードベースのクラス。 |
| フィールドグループ | [[!UICONTROL 現在の天気]](https://github.com/adobe/xdm/blob/master/components/classes/weather.schema.json) | [!UICONTROL XDM ExperienceEvent] および[!UICONTROL 天気]クラスのフィールドグループで、郵便番号の現在の気象条件をキャプチャするために使用します。 |
| フィールドグループ | [[!UICONTROL 予測された天気]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/forecasted-weather.schema.json) | [!UICONTROL XDM ExperienceEvent] および[!UICONTROL 天気]クラスのフィールドグループで、 郵便番号の予測気象条件をキャプチャするために使用します。 |
| フィールドグループ | [[!UICONTROL 製品トリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/product-triggers.schema.json) | [!UICONTROL XDM ExperienceEvent] および[!UICONTROL 天気]クラスのフィールドグループで、消費者の行動を促すという既知の気象条件を活用する、製品固有のトリガーをキャプチャするために使用します。 |
| フィールドグループ | [[!UICONTROL 相対トリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/relative-triggers.schema.json) | [!UICONTROL XDM ExperienceEvent] および[!UICONTROL 天気]クラスのフィールドグループで、消費者の行動を促すという既知の気象条件を活用する、相対トリガーをキャプチャするために使用します。 |
| フィールドグループ | [[!UICONTROL 重大なトリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/severe-triggers.schema.json) | [!UICONTROL XDM ExperienceEvent] および[!UICONTROL 天気]クラスのフィールドグループ。消費者の行動を促すことがわかっている厳しい気象条件を活用するトリガーをキャプチャするために使用します。 |
| フィールドグループ | [[!UICONTROL 気象トリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/weather-triggers.schema.json) | [!UICONTROL XDM ExperienceEvent] および [!UICONTROL 天気]クラスのフィールドグループ。消費者の行動を促すことがわかっている既知の気象条件を活用する一、般的なトリガーをキャプチャするために使用します。 |
| フィールドグループ | [[!UICONTROL MediaCollection インタラクションの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-media-collection.schema.json) | メディアインタラクションに関する詳細をキャプチャする [!UICONTROL XDM ExperienceEvent] クラスのフィールドグループ。 |
| フィールドグループ | [[!UICONTROL MediaReporting インタラクションの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-media-reporting.schema.json) | [!UICONTROL XDM ExperienceEvent] クラスのフィールドグループで、メディアレポートとのインタラクションに関する詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL 広告の詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/advertisingdetails.schema.json) | 広告アセットに関する詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL 広告ポッドの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/advertisingpoddetails.schema.json) | 単一の広告ブレーク内で連続再生される複数の広告のシーケンスである、広告ポッドに関する詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL チャプターの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/chapterdetails.schema.json) | ビデオコンテンツのチャプターまたはセグメントに関する詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL エラーの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/errordetails.schema.json) | ビデオ再生エラーに関する詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL プレーヤーイベントの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/playereventdetails.schema.json) | 再生ヘッドの位置やセッション ID など、ビデオプレーヤーに関するイベント関連の詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL プレーヤーの状態データ情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/playerstatedata.schema.json) | ビデオプレーヤーの状態に関する詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL QoE データの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/qoedatadetails.schema.json) | ビデオ再生イベントに関する品質エクスペリエンス（QoE）の詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL セッションの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/sessiondetails.schema.json) | ビデオ再生イベントに関するセッションの詳細をキャプチャします。 |

{style=&quot;table-layout:auto&quot;}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## Real-time Customer Data Platform B2B エディション {#b2b}

Real-time Customer Data Platform（Real-time CDP）上に構築された Real-time CDP B2B Edition は、B2B サービスを行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| リードとアカウントのマッチング | リードとアカウントのマッチングにより、既知のユーザープロファイルをアカウントプロファイルに結合できます。次に、アカウントやオポチュニティなどの B2B コンテキストでデータをセグメント化し、ターゲットに設定できます。 毎日実行されるジョブでは、決定論的要因と確率論的要因の両方を使用して、どのアカウントにもまだ関連付けられていないユーザープロファイルを、最も一致するアカウントに一致させます。 その後、セグメント定義にそのような一致を含めるかどうかを決定できます。<br><br>詳しくは、[リードとアカウントのマッチング](../../rtcdp/b2b-ai-ml-services/lead-to-account-matching.md)のドキュメントを参照してください。リードとアカウントのマッチングの設定方法については、[アカウントプロファイル UI ガイド](../../rtcdp/account/../accounts/account-profile-ui-guide.md?lang=en#configure-lead-to-account-matching)を参照してください。</li> |
| 予測リードとアカウントスコアリング | 予測リードとアカウントスコアリングでは、ツリーベースの（ランダムフォレスト/グラデーションブースト）機械学習手法を使用します。この手法では、オポチュニティステージのコンバージョンイベントの学習と予測、アカウントレベルでの個人アクティビティの集計が行われます。 また、最も影響力のある要因は、集計と単位レベルの両方で利用でき、B2B マーケターがスコアの原因となった要素をより深く理解できるようになります。 <br><br>詳しくは、 [予測リードとアカウントスコアリング](../../rtcdp/b2b-ai-ml-services/predictive-lead-and-account-scoring.md). スコアの管理方法について詳しくは、 [Real-time Customer Data Platform, B2B Edition での予測リードとアカウントスコアリングの管理](../../rtcdp/b2b-ai-ml-services/manage-predictive-lead-and-account-scoring.md) |

プロファイルエンリッチメントの監視方法については、[UI でのプロファイルエンリッチメントの監視](../../dataflows/ui/b2b/monitor-profile-enrichment.md)を参照してください。Real-Time CDP B2B Edition について詳しくは、[Real-time Customer Data Platform の概要](../../rtcdp/overview.md)を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

| 機能 | 説明 |
| ------- | ----------- |
| 孤立したプロファイルエッジ属性のクリーンアップ（限定リリース） | 組織がこの機能にアクセスできる場合、プロファイルサービスは、ユーザーアクティビティ領域の残りのエッジ属性を毎日削除して、システム内のプロファイルをより正確に表示できるようになりました。 このクリーンアップは、特定のプロファイルのすべてのプロファイルフラグメントが削除された後に発生し、`com_adobe_aep_profile_region_dataset` が true とマークされているデータセットから結合されるプロファイルに影響を与えます。このリリース以前の残りのエッジ属性フラグメントはこの指標に含まれていたため、クリーンアップによってライセンス使用状況ダッシュボードの「アドレス可能なオーディエンス」指標が低下したり、プロファイルダッシュボードの「プロファイル数」指標が低下したりする場合があります。 |

{style=&quot;table-layout:auto&quot;}

プロファイルデータを操作するためのチュートリアルやベストプラクティスなど、リアルタイム顧客プロファイルの詳細については、[リアルタイム顧客プロファイルの概要](../../profile/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Azure Data Explorer] ソースの一般提供 | Azure Data Explorer ソースを使用して、[!DNL Azure] インスタンスから Experience Platform にデータを取り込みます。詳しくは、[[!DNL Azure Data Explorer] ソースの概要](../../sources/connectors/databases/data-explorer.md)を参照してください。 |
| [!DNL Generic OData] ソースの一般提供 | [!DNL Generic OData] ソースを使用して、オープンデータプロトコルをサポートするシステムから Experience Platform にリソースを取り込みます。 詳しくは、[[!DNL Generic OData] ソースの概要](../../sources/connectors/protocols/odata.md)を参照してください。 |
| Experience Platform UI での [!DNL Data Landing Zone] のソースファイルプロパティの自動検出のサポート | [!DNL Data Landing Zone] ソースは、Experience Platform UI 使用時のファイルプロパティの自動検出をサポートするようになりました。詳しくは、[ [!DNL Data Landing Zone]  ソース接続の作成](../../sources/tutorials/ui/create/cloud-storage/data-landing-zone.md)のドキュメントを参照してください。 |

{style=&quot;table-layout:auto&quot;}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
