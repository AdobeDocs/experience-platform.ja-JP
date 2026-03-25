---
title: Adobe Experience Platform リリースノート（2026年3月）
description: Adobe Experience Platform の 2026年3月のリリースノート。
exl-id: 66b948fd-caa0-4e5e-83dd-3b15b77c09fa
source-git-commit: 6b6a03fb8675ed01dd255f7206b23b05c809f2a6
workflow-type: tm+mt
source-wordcount: '1713'
ht-degree: 13%

---

# Adobe Experience Platform リリースノート

>[!TIP]
>
>他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/releases/latest)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/ja/docs/federated-audience-composition/using/release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/ja/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2026年3月24日（PT）**

Adobe Experience Platformの新機能と既存の機能の更新：

- [詳細なデータライフサイクル管理](#advanced-data-lifecycle-management)
- [Agent Orchestrator](#agent-orchestrator)
- [データストリーム](#datastreams)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [リアルタイム顧客プロファイル](#real-time-customer-profile)
- [セグメント化サービス](#segmentation-service)
- [ソース](#sources)

## 詳細なデータライフサイクル管理 {#advanced-data-lifecycle-management}

Experience Platformには、消費者の記録やデータセットをプログラムによって削除することにより、保存されたデータを管理するためのデータハイジーン機能が揃っています。 UIのデータライフサイクルワークスペースまたはData Hygiene APIへの呼び出しを使用すると、データストアを効果的に管理できます。 これらの機能を使用して、情報が期待どおりに使用され、不正確なデータの修正が必要な場合に更新され、組織のポリシーで必要と判断された場合に削除されるようにします。

| 機能 | 説明 |
| --- | --- |
| マルチデータセットおよびプロファイルのみのレコード削除（APIのみ） | 1つのデータセット ID、データセット IDのコンマ区切りリスト、または`ALL`のリテラル `datasetId`を送信して、1つ、多く、またはすべてのデータセットのIDを削除できます。 また、`targetServices`を`["identity","profile","ajo"]`に設定してプロファイル関連のサービスに削除を制限することもできます。これにより、データレイクは変更されません。この機能は、Data Hygiene API経由でのみ使用できます。 詳細については、[&#x200B; レコードの削除作業指示ガイド &#x200B;](../../hygiene/api/workorder.md)を参照してください。 |

{style="table-layout:auto"}

詳しくは、[高度なデータライフサイクル管理の概要](../../hygiene/home.md)を参照してください。

## Agent Orchestrator {#agent-orchestrator}

Agent Orchestratorを利用して、ワークフローを自動化し、複数のチャネルをまたいで顧客とやり取りする、AIを活用したエージェントを構築およびデプロイします。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [&#x200B; [!DNL Microsoft 365 Copilot]の](https://experienceleague.adobe.com/ja/docs/experience-cloud-ai/experience-cloud-ai/agents/ama-ms)Adobe Marketing Agent | [!DNL Microsoft 365 Copilot]用Adobe Marketing Agentは、Adobeのマーケティングインテリジェンスを、[!DNL Teams]、[!DNL Word]、[!DNL PowerPoint]およびその他[!DNL Microsoft 365] アプリなどの日常的なツールに直接取り込む組み込みエージェントです。 このエージェントを使用すると、施策の計画中にAdobe アプリケーションから信頼できるキャンペーンインサイトを取得したり、オーディエンスを確認したり、他のユーザーと協力してお客様の質問に答えたり、[!DNL Microsoft 365] ワークフローから離れることなく、データに基づいた意思決定を行ったりできます。 |

{style="table-layout:auto"}

詳しくは、[Agent Orchestrator ドキュメント &#x200B;](https://experienceleague.adobe.com/ja/docs/experience-cloud-ai/experience-cloud-ai/agents/agent-orchestrator)を参照してください。

## データストリーム {#datastreams}

データストリームは、Adobe Experience Platform WebおよびMobile SDKとAdobe Experience Platform Edge Network Server APIを実装する際のサーバーサイド設定を表します。 SDKのデータストリーム設定コマンドは、クライアントが操作するすべてのサービスを処理します。

| 機能 | 説明 |
| --- | --- |
| 動的データストリーム設定の一般提供 | 動的データストリーム設定が一般に利用可能になりました。 動的データストリーム設定を使用すると、データストリームに対して有効になっている各サービスに対して、ユーザーが設定できるルールセットを定義できます。このルールセットは、Experience Cloud ソリューションが各タイプのデータを受け取る必要があるルールセットを決定します。 詳しくは、[動的データストリーム設定ガイド &#x200B;](../../datastreams/configure-dynamic-datastream.md)を参照してください。 |

{style="table-layout:auto"}

詳しくは、[&#x200B; データストリームの概要](../../datastreams/overview.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations]は、宛先プラットフォームとの事前定義済みの統合です。 クロスチャネルマーケティング施策、メールキャンペーン、ターゲット広告、その他多くのユースケースで、既知および未知のデータを宛先にアクティベートできます。

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| [Snowflake バッチ &#x200B;](../../destinations/catalog/warehouses/snowflake-batch.md) リージョンセレクター | 検索とドロップダウンを1つのコントロールに統合した新しい検索可能なドロップダウンを使用して、地域をより簡単に見つけることができます。 この更新は3月末までロールアウトされます。 |
| [Snowflake バッチ &#x200B;](../../destinations/catalog/warehouses/snowflake-batch.md)宛先の新しいテーブル構造 | Snowflake アカウントに共有されたテーブルに、個別のオーディエンス名とオーディエンスオリジン列を含む新しい構造が追加されました。 新しいテーブル構造は、今後に設定されるすべての新しい宛先接続に適用されます。 設定した新しい接続では、両方のテーブル構造が作成されます。新しい構造にはV2が先頭に付けられ、古い構造は2026年6月の終わりまで保持され、その後は非推奨になります。 詳しくは、Snowflake Batch ドキュメントの[&#x200B; エクスポート済みデータ &#x200B;](../../destinations/catalog/warehouses/snowflake-batch.md#exported-data) セクションを参照してください。 この更新は3月末までロールアウトされます。 |
| [Adobe Advertising DSP](../../destinations/catalog/advertising/adobe-advertising-dsp-connection.md)接続 | 新しいAdobe Advertising DSP接続は、従来の接続と同じ機能に加えて、追加のIDのサポートを提供します。 新しいコネクタを使用すると、Cookie ベースのIDをAdobe Advertising DSPに書き出すこともできます。 |
| [FreeWheel](../../destinations/catalog/advertising/freewheel.md)接続 | [!DNL Real-Time CDP]人のオーディエンスを毎日のバッチファイルとしてFreeWheelに送信すると、CTV、ビデオ、ディスプレイをまたいでFreeWheelのお得な情報やキャンペーンでターゲットにすることができます。 アクセスについては、Adobe アカウントチームにお問い合わせください。 |
| [The Trade Desk CRM](../../destinations/catalog/advertising/tradedesk-emails.md)および[Pinterest](../../destinations/catalog/advertising/pinterest.md)の外部オーディエンスのサポート | セグメンテーションサービスを超えたオリジンのオーディエンスを、カスタムアップロードオーディエンス（CSVからインポート）、類似オーディエンス、連合オーディエンス、[!DNL Adobe Journey Optimizer]などの他のExperience Platform アプリケーションで作成されたオーディエンスを含む、The Trade Desk CRM、Criteo、およびPinterestにアクティベートできるようになりました。 この更新は3月末までロールアウトされます。 詳しくは、各宛先のカタログページの「[&#x200B; サポートされているオーディエンス &#x200B;](../../destinations/catalog/advertising/criteo.md#supported-audiences)」セクションを参照してください。 |
| カスタムアップロードオーディエンスの制限の増加 | 宛先インスタンスごとに最大20個のカスタムアップロードオーディエンスをアクティブ化できるようになりました。 以前は、この制限は10でした。 詳しくは、[宛先ガードレール &#x200B;](../../destinations/guardrails.md#batch-file-based-activation)を参照してください。 |
| 外部オーディエンスに対する[&#x200B; ファイルの書き出し](../../destinations/ui/export-file-now.md)および[&#x200B; アドホックアクティベーション API](../../destinations/api/ad-hoc-activation-api.md)のサポート | バッチファイルベースの宛先に対してアクティブ化する際に、今すぐファイルを書き出し（UI）とアドホックアクティベーション APIを外部オーディエンス（カスタムアップロード、類似、フェデレーション、他のExperience Platform アプリからのオーディエンスなど）と共に使用できるようになりました。 この更新は3月末までロールアウトされます。 |
| OAuth 2およびmTLSを使用する[HTTP API](../../destinations/catalog/streaming/http-destination.md)の宛先 | 認証エンドポイントが相互TLS （mTLS）を必要とする場合に、OAuth 2を使用するHTTP API宛先を作成および認証できるようになりました。宛先セットアップ中のトークン取得で、mTLSがサポートされるようになりました。 この更新は3月末までロールアウトされます。 |

{style="table-layout:auto"}

**修正点および改善点**

| 修正 | 説明 |
| --- | --- |
| [TikTok](../../destinations/catalog/social/tiktok.md) コネクタの電話番号のハッシュ化 | 宛先カードの設定ミスにより、電話番号のキーオフされたIDがTikTokにアクティベートされない問題を修正しました。 この修正の利点を得るには、新しいアクティベーションフローを設定するか、既存のフローから電話番号マッピングを削除して保存し、もう一度追加します。 |
| [Snowflake ストリーミング &#x200B;](../../destinations/catalog/warehouses/snowflake.md)および[Snowflake バッチ &#x200B;](../../destinations/catalog/warehouses/snowflake-batch.md) アカウント IDの検証 | アカウント ID ステップに正規表現バリデーターが追加されました。 IDを入力すると、組織IDとアカウント IDが正しい形式（ドットで区切られた）であることを確認するために検証されるようになりました。 この更新は3月末までロールアウトされます。 |

{style="table-layout:auto"}

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDMは、Experience Platformに取り込まれるデータに共通の構造と定義（スキーマ）を提供するオープンソース仕様です。 XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

| 機能 | 説明 |
| --- | --- |
| XDM エンティティのアクションと削除サポート | インラインテーブルメニューと詳細ページヘッダーメニューから、スキーマ、クラス、フィールドグループ、データタイプのアクションに直接アクセスできます。 必要な権限を持っている場合は、組織のエンティティがデータセットで使用されておらず、プロファイルに対して有効になっていない場合に、そのエンティティを削除することもできます。 詳しくは、[XDM UI ガイド &#x200B;](../../xdm/ui/explore.md)を参照してください。 |

詳しくは、[XDMの概要](../../xdm/home.md)を参照してください。

## リアルタイム顧客プロファイル {#real-time-customer-profile}

リアルタイムの顧客プロファイルにより、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルからのデータを組み合わせることで、個々の顧客の全体像を把握できます。 プロファイルを利用して顧客データを統合し、顧客とのあらゆるやり取りに関する実用的なタイムスタンプ付きのアカウントを提供します。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| イベント | プロファイルの閲覧時にイベントのルックバック期間を設定できるようになりました。 これにより、指定した期間にプロファイルが関連付けられているイベントを表示できます。 詳しくは、[&#x200B; プロファイル UI ガイド &#x200B;](../../profile/ui/user-guide.md#events)を参照してください。 |

{style="table-layout:auto"}

詳しくは、[[!DNL Real-Time Customer Profile] 概要](../../profile/home.md)を参照してください。

## 実行と操作 {#run-and-operate}

実行ツールと運用ツールを使用して、Experience Platformの実装を調査、トラブルシューティング、最適化します。 スケジュールされたバッチアクティベーションを可視化し、設定の問題を特定して、システムの信頼性を向上させます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [&#x200B; ジョブスケジュール &#x200B;](../../run-and-operate/job-schedules.md)一般提供 | [!DNL Job Schedules]は、取り込みから宛先のアクティベーションまで、データパイプライン全体でスケジュールされたすべてのバッチ処理ジョブの統合ビューを提供します。 実行ステータスを検査し、スケジュールの競合を特定し、設定の問題が業務運営に影響を与える前に診断します。 |
| [&#x200B; ヘルスチェック &#x200B;](../../run-and-operate/health-checks.md)一般提供 | スキーマとIDの設定が不十分な場合、プロファイルの誤った作成、セグメントの選定の失敗、不正確なアクティベーションなど、下流プロセスにおける大きな問題につながります。 <br> ヘルスチェックにより、アプローチが事後対応のトラブルシューティングからプロアクティブで予防的なメンテナンスに移行します。 ヘルスチェックは、サンドボックスで使用されるスキーマとIDを常にスキャンし、調査とトラブルシューティングに使用できる問題の概要を提供します。 |

{style="table-layout:auto"}

詳しくは、[実行と操作の概要](../../run-and-operate/overview.md)、[&#x200B; ジョブスケジュールの調査](../../run-and-operate/job-schedules.md)、および[&#x200B; プラットフォーム UI ガイド &#x200B;](../../landing/ui-guide.md)を参照してください。

## セグメント化サービス {#segmentation-service}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。オーディエンスは、レコードデータ（デモグラフィック情報など）や、顧客と企業とのインタラクションを表す時系列イベントにもとづいておこなうことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 取り込みタイプ | 属性の取り込みタイプを表示できるようになりました。 これにより、データの出所を把握して、より優れたオーディエンスを構築できるようになります。 この機能について詳しくは、[&#x200B; セグメントビルダーガイド &#x200B;](../../segmentation/ui/segment-builder.md)を参照してください。 |
| 概要データ | アカウントおよびピープルベースのオーディエンスの属性の概要データを表示できるようになりました。 アカウントオーディエンスのこの機能について詳しくは、アカウント [&#x200B; オーディエンスビルダーガイド &#x200B;](../../rtcdp/segmentation/audience-builder.md)を参照してください。 人物ベースのオーディエンスのこの機能について詳しくは、[&#x200B; セグメントビルダーガイド &#x200B;](../../segmentation/ui/segment-builder.md)を参照してください。 |

詳しくは、[[!DNL Segmentation Service] 概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用して、外部ストレージシステムやCRM サービスを認証および接続し、取り込み実行の時間を設定し、データ取り込みのスループットを管理します。

**新規または更新されたソース**

| ソース | 説明 |
| --- | --- |
| 新しいIP アドレスを許可リストに加える | GBR9の新しいIP アドレス：Azure上のExperience Platformへのバッチソース接続を正常に行うために許可リストに加えるする必要があるアドレスのリストに、イギリスが追加されました。 詳しくは、[IP アドレスの許可リストに加えるガイド &#x200B;](../../sources/ip-address-allow-list.md#gbr9-united-kingdom)を参照してください。 |
| Change Data Captureの拡張サポート | [!DNL Marketo Engage]、[!DNL Microsoft Dynamics]、[!DNL Salesforce CRM]のソースでChange Data Captureを使用できるようになりました。 |
| [[!DNL Google BigQuery]](../../sources/connectors/databases/bigquery.md)の認証ガイドが改善されました | [!DNL Google BigQuery] ソースの認証ガイドが次の情報で拡張されました： <ul><li>更新トークンに必要なスコープ。</li><li>[!DNL Google] IDに必要なIAM役割。</li><li>`largeResultsDataSetId`の使用に関する追加のガイダンス。</li></ul> |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
