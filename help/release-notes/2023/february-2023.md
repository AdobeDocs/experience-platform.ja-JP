---
title: Adobe Experience Platformリリースノート 2023 年 2 月
description: Adobe Experience Platformの 2023 年 2 月のリリースノート。
source-git-commit: 72ae96f72bfffe376fec5c0e1dcf79406cb86a26
workflow-type: tm+mt
source-wordcount: '1172'
ht-degree: 37%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年2月22日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Destinations]](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [クエリサービス](#query-service)
- [Real-Time Customer Data Platform B2B エディション](#b2b)
- [ソース](#sources)

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能** {#destinations-new-updated-features}

| 機能 | 説明 |
| ----------- | ----------- |
| [同意ポリシーの強化](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-enhancement) （との統合用） [ファイルベース（バッチ）の宛先](/help/destinations/destination-types.md#file-based) | <p> プロファイルが同意ポリシーの対象として認定されなくなった場合、Experience Platformはポリシーの終了をファイルベースの宛先に積極的に通信するようになりました。 これは、 [2023 年 2 月リリース](/help/release-notes/2023/january-2023.md#destinations-new-updated-functionality) の機能は、ストリーミング先に対しても同じです。 </p> <p> <b>注意</b>:この機能は、 **[!UICONTROL プライバシーとセキュリティシールド]**、および **[!UICONTROL 医療用盾]**. </p> |

{style=&quot;table-layout:auto&quot;}

**新規ドキュメントまたは更新されたドキュメント** {#destinations-new-updated-documentation}

| ドキュメント | 説明 |
| ----------- | ----------- |
| 宛先の仕組みドキュメント | <p>ユーザーからのよくある質問に基づき、宛先の動作に関する 3 つの新しい説明記事を公開しました。</p> <p><ul><li>[宛先での設定可能で一般的な書き出し設定](/help/destinations/how-destinations-work/destinations-configurations.md)</li><li>[様々な宛先タイプに対するプロファイル書き出し動作](/help/destinations/how-destinations-work/profile-export-behavior.md)</li><li>[宛先アクティベーションワークフローでの ID 処理](/help/destinations/how-destinations-work/identity-handling.md)</li></p> |

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| UI を通じたフィールドの廃止 | 次の操作を実行できます。 [データの取り込み後にスキーマのフィールドを非推奨にする](../../xdm/tutorials/field-deprecation-ui.md). XDM フィールドの廃止により、UI ビューからフィールドを削除しながら、使用するためにフィールドを保持できます。 廃止されたフィールドを必要に応じて再度表示できます。また、そのフィールドを参照するセグメント、クエリ、ダウンストリームソリューションは、通常どおりに実行されます。 |

{style=&quot;table-layout:auto&quot;}

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| クラス | [[!UICONTROL XDM 個別見込み客プロファイル]](https://github.com/adobe/xdm/pull/1669/files) | XDM Individual Prospect Profile クラスは、パートナーが提供する ID を取り込みます。 |

{style=&quot;table-layout:auto&quot;}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| フィールドグループ | [!UICONTROL 頻度キャップの制約] | この [!UICONTROL 頻度キャップの制約] フィールドグループが [繰り返しイベントとカスタムイベントをサポートするように更新](https://github.com/adobe/xdm/pull/1641/files). |
| データタイプ | [!UICONTROL Web リファラー] | Web リファラープロパティが [次を含めるように更新しました： `xdm:linkName` および `xdm:linkRegion`](https://github.com/adobe/xdm/pull/1666/files). それぞれ、前のページで選択したHTML要素の名前と領域です。 |
| フィールドグループ | [!UICONTROL Adobe CJM ExperienceEvent - メッセージインタラクションの詳細] | [この [!UICONTROL トラッカー URL] フィールドが追加されました](https://github.com/adobe/xdm/pull/1665/files) から [!UICONTROL AdobeCJM ExperienceEvent]. このトラッカーは、ユーザーが選択した URL を提供します。 |
| フィールドグループ | [!UICONTROL AdobeCJM ExperienceEvent — メッセージインタラクションの詳細] | [空の `meta:enum` プロパティが削除されました](https://github.com/adobe/xdm/pull/1668/files) URL から [!UICONTROL トラッキングタイプ] フィールドに入力します。 |
| データタイプ | [!UICONTROL メディア情報] | [の正規表現パターン `videoSegment` プロパティ [!UICONTROL メディア情報] データ型が削除されました](https://github.com/adobe/xdm/pull/1667/files). |

{style=&quot;table-layout:auto&quot;}

Platform での XDM について詳しくは、 [XDM システムの概要](../../xdm/home.md). &#x200B;

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。データレイクの任意のデータセットを結合し、クエリ結果を新しいデータセットとして取り込んで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| SQL を使用したプロファイルのデータセットの有効化 | [CTAS クエリで LABEL を使用して、データセット「プロファイルが有効」にする](../../query-service/sql/syntax.md#create-table-as-select)または ALTER を使用して、既存のデータセットを更新し、プロファイルに対して有効にします。 この拡張 SQL 構成を使用して、リアルタイム顧客プロファイルビジネスの使用例に対して派生属性をシームレスにサポートできます。 詳しくは、 [派生属性ドキュメントのシームレスな SQL フロー](../../query-service/data-distiller/derived-attributes/seamless-sql-flow.md) を参照してください。 |
| スケジュールクエリの監視 | 以下を使用： [「予定クエリ」タブ](../../query-service/ui/monitor-queries.md) クエリの実行に関する重要な情報を見つけ、アラートを購読します。 クエリを監視して、スケジュールの詳細、ステータス、エラーメッセージ/コードが失敗した場合に表示されます。 |
| オートコンプリート機能を切り替え | 特定のメタデータコマンドを削除し、次の方法で処理時間を短縮 [クエリエディターのオートコンプリート機能の切り替え](../../query-service/ui/user-guide.md#auto-complete). この機能では、クエリの記述時に、SQL キーワードの候補と表の詳細が自動的に提示されます。 |
| データセットのサンプル | クエリでサンプリングレートを指定し、 [データセットサンプルを使用して、均一なランダムサンプルを作成する](../../query-service/essential-concepts/dataset-samples.md)または、特定の条件に基づく条件付きサンプルを作成します。 |

{style=&quot;table-layout:auto&quot;}

クエリサービスについて詳しくは、[クエリサービスの概要](../../query-service/home.md)を参照してください。


## Real-Time Customer Data Platform B2B エディション {#b2b}

Real-Time Customer Data Platform（Real-Time CDP）上に構築された Real-Time CDP B2B エディションは、B2B サービスモデルで業務を行っているマーケター向けに設計されています。複数のソースからのデータをまとめて、人物とアカウントプロファイルの単一のビューに結合します。この統合されたデータにより、マーケターは特定のオーディエンスを正確にターゲットにして、利用可能なすべてのチャネルでそれらのオーディエンスを惹き付けることができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 関連アカウントサービスを有効にする | 新しい切り替え機能を使用すると、お使いのアカウントで関連アカウントサービスを有効にできます。 詳しくは、 [関連アカウントサービスの有効化](../../rtcdp/b2b-ai-ml-services/related-accounts.md#enable). |

{style=&quot;table-layout:auto&quot;}

Real-Time CDP B2B Edition の詳細については、 [Real-Time CDP B2B Edition の概要](../../rtcdp/overview.md).

## ソース {#sources}

Adobe Experience Platformは、外部ソースからデータを取り込むことができ、Platform Services を使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 次を使用してサブスクリプションレベルのアクセスを指定 [!DNL Google PubSub] | を使用する際に、特定のトピック購読へのアクセスを定義できるようになりました。 [!DNL Google PubSub] 認証時に購読 ID を指定してソースを作成する。 詳しくは、 [!DNL Google PubSub] 認証チュートリアル [フローサービス API の使用](../../sources/tutorials/api/create/cloud-storage/google-pubsub.md) または [Platform UI](../../sources/tutorials/ui/create/cloud-storage/google-pubsub.md). |
| カスタムアクティビティデータの取り込み元 [!DNL Marketo] | これで、 [!DNL Marketo] インスタンスからExperience Platformへ。 カスタムアクティビティデータを取り込むには、B2B アクティビティスキーマでカスタムアクティビティフィールドグループを設定し、アクティビティデータセットを使用してデータフローを作成する必要があります。 データフローが完了すると、取り込まれたデータセットには、 [!DNL Marketo] インスタンス。 その後、 [クエリサービス](../../query-service/home.md) をクリックして、Platform のカスタムアクティビティレコードにアクセスします。 詳しくは、 [カスタムアクティビティデータのデータフローの作成](../../sources/tutorials/ui/create/adobe-applications/marketo-custom-activities.md). |
| 次の場所から未要求のアカウントを除外 [!DNL Marketo] | 会社データのデータフローを作成する際に、取得から要求されていないアカウントを除外するか、取得から含めるかを設定できるようになりました。 詳しくは、 [のソース接続とデータフローの作成 [!DNL Marketo]](../../sources/tutorials/ui/create/adobe-applications/marketo.md). |

{style=&quot;table-layout:auto&quot;}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
