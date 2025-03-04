---
title: Adobe Experience Platform リリースノート 2023年9月
description: Adobe Experience Platform の 2023年9月のリリースノート。
exl-id: ff7fb0c1-6941-4339-8648-58f9b9e9a91f
source-git-commit: 2d640b282feb783694276c69366b1fccadddfd78
workflow-type: tm+mt
source-wordcount: '2265'
ht-degree: 31%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年9月28日（PT）**

Adobe Experience Platform の新機能：

- [計算属性](#computed-attributes)

Experience Platformの既存の機能に対するアップデート：

- [アラート](#alerts)
- [ダッシュボード](#dashboards)
- [データ収集](#data-collection)
- [データガバナンス](#data-governance)
- [データハイジーン](#hygiene)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [ID サービス](#identity-service)
- [クエリサービス](#query-service)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## 計算属性 {#computed-attributes}

計算済み属性を使用すると、直感的な UI を使用してイベントデータをプロファイル属性に簡単に要約し、動作ベースのセグメント化、パーソナライゼーションおよびアクティブ化を強化できます。 この機能を使用すると、計算済み属性をセルフサービス方式で作成および管理し、セグメント化、Real-Time CDPの宛先、Adobe Journey Optimizerで使用できます。 さらに、計算済み属性は、セグメント化とジャーニーワークフローを簡素化し、関連性の高いエクスペリエンスをシームレスに提供できるようにします。 計算属性について詳しくは、[ 計算属性の概要 ](../../profile/computed-attributes/overview.md) を参照してください。

## アラート {#alerts}

Experience Platform では、様々な Platform アクティビティに関するイベントベースのアラートを登録できます。Platform ユーザーインターフェイスの「[!UICONTROL アラート]」タブを使用して、様々なアラートルールを購読し、UI 内または電子メール通知を通じてアラートメッセージを受け取るように選択できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 「アラートの履歴」タブ | 「アラート [!UICONTROL  履歴 ]」タブに、遅延、開始、成功、失敗を含むすべてのイベントが含まれるようになりました。 「履歴」タブについて詳しくは、[ アラート UI ドキュメント ](../../observability/alerts/ui.md) を参照してください。 |

{style="table-layout:auto"}

アラートについて詳しくは、[[!DNL Observability Insights]  概要 ](../../observability/home.md) を参照してください。

## ダッシュボード {#dashboards}

Adobe Experience Platform では、複数の [!DNL dashboards] を提供しており、毎日のスナップショットでキャプチャされた、組織のデータに関する重要な情報を表示できます。

| 機能 | 説明 |
| --- | --- |
| [ ライセンス使用状況ダッシュボードの改善 ](../../dashboards/guides/license-usage.md) | 組織のライセンス使用状況に関するレポートおよび主要指標のビジュアライゼーションを改善して、ライセンス契約の管理を維持します。 これらの改善点により、購入したすべてのExperience Platform製品のライセンス使用状況指標に対して高い精度が提供されます。 |

{style="table-layout:auto"}

ライセンス使用状況ダッシュボードについて詳しくは、[ ライセンス使用状況ダッシュボードの概要 ](../../dashboards/guides/destinations.md) を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| データストリーム | デバイス検索のサポート | データストリームを設定する際に、収集するデバイス参照情報のレベルを選択できるようになりました。 デバイス参照情報には、ページとのやり取りに使用するデバイス、ハードウェア、オペレーティングシステム、ブラウザーに関するデータが含まれています。 <br> デバイス参照情報は、ユーザーエージェントおよびクライアントヒントと共に収集できません。 デバイス情報の収集を選択すると、ユーザーエージェントとクライアントヒントの収集が無効になります。逆も同様です。 すべてのデバイス参照情報は、`xdm:device` フィールドグループに保存されます。 詳しくは、[ データストリームの設定 ](../../datastreams/configure.md#geolocation-device-lookup) に関するドキュメントを参照してください。 |
| 拡張機能 | [!DNL TikTok] Web イベント API 拡張機能 | [[!DNL TikTok] Web Events API](https://exchange.adobe.com/apps/ec/109834/tiktok-web-events-api) 拡張機能を使用すると、Adobe Experience Platform Edge Networkで取得したデータを活用したり、[!DNL TikTok] Web Events API を使用してサーバーサイドイベントの形式で [!DNL TikTok] に送信したりできます。 |

{style="table-layout:auto"}

データ収集について詳しくは、[ データ収集の概要 ](../../tags/home.md) を参照してください。

## データガバナンス {#data-governance}

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。これは Experience Platform 内の様々なレベルで重要な役割を果たします。例えば、カタログ化、データ系列、データ使用のラベル付け、データアクセスポリシー、マーケティングアクションのデータに関するアクセス制御などです。

**新機能**

| 機能 | 説明 |
| --- | --- |
| サードパーティデータ用の新しいパートナーエコシステムラベル | サードパーティのエンリッチメントと見込み客向けの新しいデータ使用ラベルを利用できます。 詳しくは、[ パートナーエコシステムラベルに関するドキュメント ](../../data-governance/labels/reference.md#partner) を参照してください。 |

{style="table-layout:auto"}

データガバナンスについて詳しくは、[データガバナンスの概要](../../data-governance/home.md)を参照してください。

## データハイジーン {#hygiene}

Experience Platform は、消費者レコードとデータセットをプログラムで削除することで、保存されたデータを管理できる、一連のデータ衛生機能を提供します。UI の [!UICONTROL  データライフサイクル ] ワークスペース、または Data Hygiene API への呼び出しを使用して、データストアを効果的に管理できます。 これらの機能を使用して、情報が期待どおりに使用され、必要な場合は不適切なデータの修正が更新され、組織のポリシーで必要と判断された場合は削除されるようにします。

**新機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} レコード削除（限定リリース） | Adobe Experience Platformの高度なデータライフサイクル管理機能：データセットの有効期限とレコード削除の自動化により、お客様のコミットメントと使用許諾契約を満たすために、すべてのデータストアにわたるデータライフサイクルを管理します。<br> データセットの自動有効期限を使用すると、データセット全体を削除し、削除するデータセットの日時を設定できます。<br> レコード削除を使用すると、プライマリ ID をターゲット設定することで、個々の消費者プロファイルを削除できます。 UI または CSV/JSON ファイルのアップロードを使用して、プライマリ ID を個別に指定できます。 詳しくは、[ レコード削除のドキュメント ](../../hygiene/ui/record-delete.md) を参照してください |
| データセット有効期限 | データセットの自動有効期限を使用すると、データを最小限に抑え、使用許諾契約を常に制御できます。 データセット全体を削除し、削除するデータセットの日時を設定することで、データ量を削減します。 詳しくは、[ データセット有効期限のドキュメント ](../../hygiene/ui/dataset-expiration.md) を参照してください。 |

{style="table-layout:auto"}

Platform のデータハイジーン機能について詳しくは、[データハイジーンの概要](../../hygiene/home.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先** {#new-updated-destinations}

| 宛先 | 新規／アップデート | 説明 |
| ----------- |----------------|----------- |
| [[!DNL LiveRamp - Distribution]](../../destinations/catalog/advertising/liveramp-distribution.md) | 新規 | モバイル、web、ディスプレイ、コネクテッドの各 TV メディアで、[!DNL LiveRamp] にオンボーディング済みのオーディエンスをアクティブ化し、プレミアム パブリッシャーに配信します。 <br> [LiveRamp - オンボーディング ](../../destinations/catalog/advertising/liveramp-onboarding.md) 接続を通じてオーディエンスを [!DNL LiveRamp] アカウントにオンボーディングした後、新しい [[!DNL LiveRamp - Distribution]](../../destinations/catalog/advertising/liveramp-distribution.md) 接続を使用して、ダウンストリームの宛先に対してオーディエンスをアクティブ化します。 |
| [[!DNL HubSpot]](../../destinations/catalog/crm/hubspot.md) | 新規 | [[!DNL HubSpot]](https://www.hubspot.com) は、マーケティング、セールス、コンテンツ管理、カスタマーサービスを結び付けるために必要なすべてのソフトウェア、統合、リソースを備えた CRM プラットフォームです。 データ、チーム、顧客を 1 つの CRM プラットフォームに接続できます。 |
| [[!DNL Microsoft Dynamics 365]](../../destinations/catalog/crm/microsoft-dynamics-365.md) | 更新済み | [!DNL Dynamics 365] のデフォルトソリューション内 [!DNL Dynamics 365] 作成されなかったカスタムフィールドのカスタムフィールドプレフィックスに対するサポートを追加しました。 **[!UICONTROL 宛先の詳細の入力]** 手順に、新しい入力フィールド [Customization Prefix](#destination-details) が追加されました。 |
| [[!DNL Experience Cloud Audiences]](../../destinations/catalog/adobe/experience-cloud-audiences.md) | 更新済み | Experience Cloud オーディエンスの宛先が一般公開されました。 この宛先を使用して、Real-Time CDPからAudience ManagerおよびAdobe Analyticsにオーディエンスをアクティブ化します。 オーディエンスをAdobe Analyticsに送信するには、Audience Manager ライセンスが必要です。 |

{style="table-layout:auto"}

<!-- 


Add these to release notes as they go out

| [[!DNL Qualtrics]] | New | Use the aggregation of multiple sources of operational data in Adobe Experience Platform as an input in Qualtrics Experience ID to better understand your customers and enable targeted outreach to close the gap when it comes to understanding intent, emotion and experience drivers. | 

-->

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| Real-Time CDPでのデータの書き出し | [ データセットの書き出し ](../../destinations/ui/export-datasets.md) 機能が一般公開されました。 [ 購入したExperience Platform アプリに基づいて書き出すことができるデータセット ](../../destinations/ui/export-datasets.md#datasets-to-export) を確認し、[ データセットを書き出すためのガードレール ](/help/destinations/guardrails.md#dataset-exports) を確認します。 |
| （Beta）配列型オブジェクトの書き出しのサポート | プリミティブ値（文字列、整数またはブール値）の配列をフラットスキーマファイルとしてクラウドストレージ宛先に書き出します。 機能について詳しくは、[ ドキュメント ](../../destinations/ui/export-arrays-maps-objects.md) を参照してください。 |
| Destination SDKの動的ドロップダウンセレクター | Destination SDKを使用して宛先を作成する際に、[ 動的ドロップダウンセレクター ](../../destinations/destination-sdk/functionality/destination-configuration/customer-data-fields.md#dynamic-dropdown-selectors) を使用して、ドロップダウンセレクターのフィールドに、API から取得した値を入力できるようになりました。 |

**修正および機能強化** {#destinations-fixes-and-enhancements}

- データフロー実行レベルでエンタープライズ宛先（[HTTP API](../../destinations/catalog/streaming/http-destination.md)、[Amazon Kinesis](../../destinations/catalog/cloud-storage/amazon-kinesis.md) および [Azure Event Hubs](../../destinations/catalog/cloud-storage/azure-event-hubs.md)）で使用できる [ モニタリング透明性 ](../../dataflows/ui/monitor-destinations.md#dataflow-runs-for-streaming-destinations) を利用して、[ データフローの詳細ビュー ](../../dataflows/ui/monitor-destinations.md#dataflow-run-details-page) でアクティブ化指標とステータスを監視し、エラーコードやトラブルシューティング用メッセージを介して追加情報を提供します。
- [Google Ad Manager](../../destinations/catalog/advertising/google-ad-manager.md)、[Google ディスプレイおよびビデオ 360](../../destinations/catalog/advertising/google-dv360.md)、および [ オーディエンス更新テンプレート ](../../destinations/destination-sdk/metadata-api/update-audience-template.md) を使用するその他の宛先にマッピングされたオーディエンスの名前を更新すると、これらの名前の変更は宛先のダウンストリームに反映されるようになりました。

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| スキーマエディターに追加されたクイックアクション | スキーマエディターのキャンバスに新しいクイックアクションが追加されました。 エディターから直接 JSON 構造をコピーしたり、スキーマを削除したりできるようになりました。<br>![ スキーマエディターのクイックアクション。](../2023/assets/schema-editor-copy-json.png " その他および「JSON にコピー」がハイライト表示されたスキーマエディター "){width="100" zoomable="yes"} |
| カスタムまたは標準の作成者による XDM リソースのフィルタリング | 使用可能なスキーマ、フィールドグループ、データタイプおよびクラスのリストが、作成方法に基づいて事前にフィルタリングされるようになりました。 これにより、カスタムビルドされたか、Adobeで作成されたかに基づいてリソースをフィルタリングできます。<br>![ スキーマ ワークスペースの標準フィルターとカスタムフィルター。](../2023/assets/standard-and-custom-classes.png " 標準フィルターとカスタムフィルターがハイライト表示されたスキーマワークスペース。"){width="100" zoomable="yes"} <br> しくは、[ リソースの作成と編集 ](../../xdm/ui/resources/classes.md#filter.md) ドキュメントを参照してください。 |

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 更新されたスキーマ作成ワークフロー | 新しいスキーマ作成ワークフローが実装され、プロセスが合理化されました。<br> ![ 新しいスキーマ作成 UI](../2023/assets/schema-class-options.png " ハイライト表示された新しいスキーマの詳細セレクター。詳 "){width="100" zoomable="yes"} くは、[ スキーマ作成ドキュメント ](../../xdm/ui/resources/schemas.md#create) を参照してくださ <br>。 |

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| データタイプ | [[!UICONTROL  戻る ]](https://github.com/adobe/xdm/pull/1773/files) | 発行された RMA （返品承認）。 |
| データタイプ | [[!UICONTROL  返品品目 ]](https://github.com/adobe/xdm/pull/1773/files) | 返品品目の RMA 内の情報（返品承認）。 |

{style="table-layout:auto"}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明のアップデート |
| --- | --- | --- |
| 拡張機能 | [!UICONTROL AJO エンティティフィールド ] | [[!UICONTROL  マルチバリアントのフラグ ]](https://github.com/adobe/xdm/pull/1774/files) が [!UICONTROL AJO エンティティフィールド ] に追加され、バリアントがマルチバリアントかどうかを識別できるようになりました。 |
| データタイプ | [!UICONTROL  商品リスト項目 ] | 返品承認情報を含むように [[!UICONTROL  返品品目 ]](https://github.com/adobe/xdm/pull/1773/files) が追加されました。 |
| データタイプ | 注文 | [[!UICONTROL  返品情報 ]](https://github.com/adobe/xdm/pull/1773/files) は、発行された RMA （返品商品承認）を含むように追加されました。 |

{style="table-layout:auto"}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| ID サービス UI の機能強化 | Experience Platform UI の改善されたカスタム名前空間作成ツールを使用して、カスタム名前空間とそれに対応する ID タイプをより適切に管理します。 拡張 ID サービス UI には、次の機能が用意されています。 <ul><li>コンテキストエクスペリエンス：ID 名前空間と ID タイプの概要に関する視覚的な手がかり、明確さ、コンテキスト。</li><li>精度：ID 名の重複をなくし、エラー処理を向上しました。</li><li>検出性：製品内ダイアログ内からドキュメントにアクセスできます。</li></ul> 詳しくは、[ カスタム名前空間の作成 ](../../identity-service/features/namespaces.md#create-namespaces) に関するガイドを参照してください。 |
| ID グラフの制限の変更 | ID グラフの制限は、150 個の ID から 50 個の ID に変更されました。 新しい ID がフルグラフに取り込まれると、取り込みタイムスタンプと ID タイプに基づく最も古い ID が削除されます。 cookie の ID タイプは削除に対して優先順位付けされます。 実稼動サンドボックスに次の ID が含まれる場合は、Adobe アカウントチームに連絡して、ID タイプの変更をリクエストしてください。 <ul><li>ユーザー識別子（CRM ID など）が cookie/デバイス ID タイプとして設定されるカスタム名前空間。</li><li>cookie とデバイスの識別子がクロスデバイス id タイプとして設定されるカスタム名前空間。</li></ul> これらのリクエストは、Adobe エンジニアリングによって手動で処理されます。 詳しくは、[ID サービスデータのガードレール ](../../identity-service/guardrails.md) および [ データ管理ライセンス使用権限のベストプラクティス ](../../landing/license-usage-and-guardrails/data-management-best-practices.md) に関するガイドを参照してください。 |

{style="table-layout:auto"}

ID サービスについて詳しくは、[ID サービスの概要 ](../../identity-service/home.md) を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。任意のデータセットを [!DNL Data Lake] から結合し、クエリの結果を新しいデータセットとして取得することで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| ログフィルタリング用 UI の更新 | クエリログのフィルタリングの強化により、監視、管理、トラブルシューティングのためのユーザー生成ログの可視性が向上しました。 様々な設定に基づいて、クエリログのリストをフィルタリングできます。<br> ![ クエリログのフィルター設定。](../2023/assets/log-filter-settings.png " 新しいクエリログフィルターがハイライト表示されます。"){width="100" zoomable="yes"} しくは、[ クエリログのドキュメント ](../../query-service/ui/query-logs.md#filter-logs) を参照してくださ <br>。 |
| 複数のクエリエディターの UI の更新 | クエリエディターで複数の順次クエリを実行したり、複数のクエリを記述してすべてのクエリを順番に実行したりできるようになりました。 クエリの実行をより柔軟に行うには、選択したクエリをハイライト表示し、その特定のクエリを他のクエリとは別に実行するように選択します。 詳しくは、[ クエリエディター UI ガイド ](../../query-service/ui/user-guide.md#execute-multiple-sequential-queries) を参照してください。 |

{style="table-layout:auto"}

クエリサービスについて詳しくは、[クエリサービスの概要](../../query-service/home.md)を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| カスタマイズ可能な列 | サイズを変更できる列を使用して、オーディエンスポータルのレイアウトをカスタマイズできるようになりました。 この機能について詳しくは、[ オーディエンスポータルの概要 ](../../segmentation/ui/audience-portal.md#customize) を参照してください。 |
| 更新頻度の分類 | 組織のオーディエンスの更新頻度の分類を表示できるようになりました。 この機能について詳しくは、[ セグメント化 UI ガイド ](../../segmentation/ui/overview.md#browse) を参照してください。 |

セグメント化サービスの詳細については、「[セグメント化サービスの概要](../../segmentation/home.md)」を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| セルフサービスソースでの `offset` ページネーションの新しいパラメーター（バッチ SDK） | ページネーションを使用する際に、ソースの `endConditionName` と `endConditionValue` を指定できるように `offset` りました。 これらのパラメーターを使用すると、次の HTTP リクエストでページネーションループを終了させる条件を指定できます。 詳しくは、[ セルフサービスソースのページネーションガイド（バッチ SDK） ](../../sources/sources-sdk/config/sourcespec.md#pagination) を参照してください。 |

{style="table-layout:auto"}

ソースについて詳しくは、[ ソースの概要 ](../../sources/home.md) を参照してください。
