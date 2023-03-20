---
title: Adobe Experience Platform リリースノート（2023年2月）
description: Adobe Experience Platform の 2023年2月のリリースノートです。
source-git-commit: 0935a50527800b255901f8047051c47b45ab33b8
workflow-type: ht
source-wordcount: '1293'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年2月22日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [データ収集](#data-collection)
- [[!DNL Destinations]](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [クエリサービス](#query-service)
- [Real-Time Customer Data Platform B2B エディション](#b2b)
- [ソース](#sources)

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

### Assurance {#assurance}

Adobe Assurance を使用すると、モバイルアプリでデータを収集したりエクスペリエンスを提供したりする方法を検査、配達確認、シミュレートおよび検証できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 公開 API | Adobe Assurance API が利用できるようになりました。 Assurance API は、Mobile SDK で Adobe Assurance 拡張機能を実装すると、ユーザーが自分自身の web およびモバイルアプリをテストしデバッグできるようになる API のコレクションです。 Assurance API について詳しくは、[Assurance API の概要](https://developer.adobe.com/adobe-assurance-public-apis/)を参照してください。 |

{style="table-layout:auto"}

Assurance について詳しくは、[Assurance ドキュメント](https://developer.adobe.com/client-sdks/documentation/platform-assurance/)を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能** {#destinations-new-updated-features}

| 機能 | 説明 |
| ----------- | ----------- |
| [ファイルベース（バッチ）宛先](/help/destinations/destination-types.md#file-based)と統合するための[同意ポリシーの強化](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-enhancement) | <p> プロファイルが同意ポリシーの資格を失った場合、Experience Platform はポリシーの終了をファイルベース宛先にプロアクティブに通知するようになりました。これは、同じ機能がストリーミング宛先について [2023年2月にリリース](/help/release-notes/2023/january-2023.md#destinations-new-updated-functionality)されたのを受けたものです。 </p> <p> <b>メモ</b>：この機能は、**[!UICONTROL Privacy and Security Shield]** および **[!UICONTROL Healthcare Shield]** のお客様のみが使用できます。 </p> |

{style="table-layout:auto"}

**新規ドキュメントまたは更新されたドキュメント** {#destinations-new-updated-documentation}

| ドキュメント | 説明 |
| ----------- | ----------- |
| 宛先の仕組みに関するドキュメント | <p>ユーザーからのよくある質問に基づいて、宛先の仕組みについて説明する次の 3 つの新しい記事を公開しました。</p> <p><ul><li>[宛先での設定可能で一般的な書き出し設定](/help/destinations/how-destinations-work/destinations-configurations.md)</li><li>[様々な宛先タイプのプロファイル書き出し動作](/help/destinations/how-destinations-work/profile-export-behavior.md)</li><li>[宛先アクティブ化ワークフローでの ID の処理](/help/destinations/how-destinations-work/identity-handling.md)</li></p> |

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| UI を介したフィールドの非推奨化 | [データを取り込んだ後でスキーマ内のフィールドを非推奨（廃止予定）にする](../../xdm/tutorials/field-deprecation-ui.md)ことができるようになりました。 XDM フィールドの非推奨化では、フィールドを UI ビューから削除しつつ引き続き使用することができます。 必要に応じて、非推奨フィールドを再度表示できます。また、そのフィールドを参照する任意のセグメント、クエリまたはダウンストリームソリューションは、通常どおりに動作します。 |

{style="table-layout:auto"}

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| クラス | [[!UICONTROL XDM 個人見込み客プロファイル]](https://github.com/adobe/xdm/pull/1669/files) | XDM 個人見込み客プロファイルクラスは、パートナー提供の ID を取り込みます。 |

{style="table-layout:auto"}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| フィールドグループ | [!UICONTROL フリークエンシーキャップの制約] | [!UICONTROL フリークエンシーキャップの制約]フィールドグループが [繰り返しイベントとカスタムイベントをサポートするように更新](https://github.com/adobe/xdm/pull/1641/files)されました。 |
| データタイプ | [!UICONTROL Web リファラー] | Web リファラープロパティが [`xdm:linkName` と `xdm:linkRegion` を含むように更新されました](https://github.com/adobe/xdm/pull/1666/files)。これらはそれぞれ、前のページで選択した HTML 要素の名前と領域です。 |
| フィールドグループ | [!UICONTROL Adobe CJM ExperienceEvent - メッセージインタラクションの詳細] | [[!UICONTROL トラッカー URL] フィールドが ](https://github.com/adobe/xdm/pull/1665/files)[!UICONTROL Adobe CJM ExperienceEvent] に追加されました。このトラッカーは、ユーザーが選択した URL を提供します。 |
| フィールドグループ | [!UICONTROL Adobe CJM ExperienceEvent - メッセージインタラクションの詳細] | [空の `meta:enum` プロパティが](https://github.com/adobe/xdm/pull/1668/files) URL [!UICONTROL トラッキングタイプ]フィールドから削除されました。 |
| データタイプ | [!UICONTROL メディア情報] | [[!UICONTROL メディア情報]データタイプの `videoSegment` プロパティの正規表現パターンが削除されました](https://github.com/adobe/xdm/pull/1667/files)。 |

{style="table-layout:auto"}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。データレイクの任意のデータセットを結合したり、クエリ結果を新しいデータセットとして取得したりすることで、それらのデータセットをレポートやデータサイエンスワークスペースで使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| SQL によるプロファイルでのデータセットの有効化 | [CTAS クエリで LABEL を使用して、データセットを「プロファイル対応」にする](../../query-service/sql/syntax.md#create-table-as-select)か、ALTER を使用して、プロファイルで有効になるように既存のデータセットを更新します。この拡張 SQL コンストラクトを使用すると、リアルタイム顧客プロファイルのビジネスユースケースに対して派生属性をシームレスにサポートできます。 詳しくは、[派生属性のシームレスな SQL フローに関するドキュメント](../../query-service/data-distiller/derived-attributes/seamless-sql-flow.md)を参照してください。 |
| スケジュール済みクエリの監視 | [「スケジュール済みクエリ」タブ](../../query-service/ui/monitor-queries.md)を使用すると、クエリの実行に関する重要な情報を見つけたり、アラートを購読したりできます。 スケジュール詳細のクエリ、ステータス、クエリが失敗した場合のエラーメッセージ／コードを監視できます。 |
| オートコンプリート機能の切り替え | [クエリエディターのオートコンプリート機能を切り替えると](../../query-service/ui/user-guide.md#auto-complete)、特定のメタデータコマンドが削除され、処理時間が短縮されます。この機能では、クエリの記述時に、クエリの SQL キーワード候補とテーブル詳細が自動的に提示されます。 |
| データセットのサンプル | クエリでサンプリングレートを指定し、[データセットサンプルを使用して、均一なランダムサンプルを作成する](../../query-service/essential-concepts/dataset-samples.md)か、特定の条件に基づいて条件付きサンプルを作成します。 |

{style="table-layout:auto"}

クエリサービスについて詳しくは、[クエリサービスの概要](../../query-service/home.md)を参照してください。


## Real-Time Customer Data Platform B2B エディション {#b2b}

Real-Time Customer Data Platform（Real-Time CDP）上に構築された Real-Time CDP B2B エディションは、B2B サービスモデルで業務を行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 関連アカウントサービスの有効化 | 新しい切り替え機能を使用すると、お使いのアカウントで関連アカウントサービスを有効にできます。 詳しくは、[関連アカウントサービスの有効化](../../rtcdp/b2b-ai-ml-services/related-accounts.md#enable)に関するガイドを参照してください。 |

{style="table-layout:auto"}

Real-Time CDP B2B Edition について詳しくは、[Real-Time CDP B2B Edition の概要](../../rtcdp/overview.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むことができ、Platform サービスを使用してそのデータの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Google PubSub] を使用したサブスクリプションレベルアクセスの指定  | 認証時にサブスクリプション ID を指定して、[!DNL Google PubSub] ソースを使用する際に特定のトピックサブスクリプションへのアクセスを定義できるようになりました。詳しくは、[フローサービス API](../../sources/tutorials/api/create/cloud-storage/google-pubsub.md) または [Platform UI](../../sources/tutorials/ui/create/cloud-storage/google-pubsub.md) を使用した [!DNL Google PubSub] 認証のチュートリアルを参照してください。 |
| [!DNL Marketo] からのカスタムアクティビティデータの取り込み | カスタムアクティビティデータを [!DNL Marketo] インスタンスから Experience Platform に取り込めるようになりました。 カスタムアクティビティデータを取り込むには、B2B アクティビティスキーマにカスタムアクティビティフィールドグループを設定し、アクティビティデータセットを使用してデータフローを作成する必要があります。 データフローが完了すると、取り込まれたデータセットには [!DNL Marketo] インスタンスからの標準アクティビティとカスタムアクティビティの両方が含まれています。 その結果、[クエリサービス](../../query-service/home.md)を使用して、Platform のカスタムアクティビティレコードにアクセスできます。 詳しくは、[カスタムアクティビティデータのデータフローの作成](../../sources/tutorials/ui/create/adobe-applications/marketo-custom-activities.md)に関するガイドを参照してください。 |
| [!DNL Marketo] からの未要求アカウントの除外  | 企業データのデータフローを作成する際に、未要求アカウントを取り込みから除外するか取り込みに含めるかを設定できるようになりました。詳しくは、[ソース接続とデータフローの作成： [!DNL Marketo]](../../sources/tutorials/ui/create/adobe-applications/marketo.md) に関するガイドを参照してください。 |

{style="table-layout:auto"}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
