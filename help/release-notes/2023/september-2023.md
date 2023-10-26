---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2023年9月のリリースノート。
exl-id: ff7fb0c1-6941-4339-8648-58f9b9e9a91f
source-git-commit: 76ac65730512e589e518095f9496bb309365b0c9
workflow-type: tm+mt
source-wordcount: '2283'
ht-degree: 33%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年9月28日（PT）**

Adobe Experience Platform の新機能：

- [計算属性](#computed-attributes)

 Experience Platform の既存の機能に対するアップデート：

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

計算済み属性を使用すると、直感的な UI でイベントデータを簡単にプロファイル属性に要約し、動作ベースのセグメント化、パーソナライゼーション、アクティベーションを強化できます。 この機能を使用すると、計算済み属性をセルフサービス方式で作成し、管理し、セグメント化、Real-Time CDP宛先、Adobe Journey Optimizerで使用できます。 さらに、計算済み属性は、セグメント化とジャーニーワークフローを簡素化し、関連するエクスペリエンスをシームレスに配信できるようにします。 計算済み属性の詳細については、 [計算済み属性の概要](../../profile/computed-attributes/overview.md).

## アラート {#alerts}

Experience Platform では、様々な Platform アクティビティに関するイベントベースのアラートを登録できます。Platform ユーザーインターフェイスの「[!UICONTROL アラート]」タブを使用して、様々なアラートルールを購読し、UI 内または電子メール通知を通じてアラートメッセージを受け取るように選択できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 「アラート履歴」タブ | アラート [!UICONTROL 履歴] タブに、遅延、開始、成功、失敗を含むすべてのイベントが含まれるようになりました。 詳しくは、 [アラート UI ドキュメント](../../observability/alerts/ui.md) 「履歴」タブの詳細を参照してください。 |

{style="table-layout:auto"}

アラートの詳細については、 [[!DNL Observability Insights] 概要](../../observability/home.md).

## ダッシュボード {#dashboards}

Adobe Experience Platform では、複数の [!DNL dashboards] を提供しており、毎日のスナップショットでキャプチャされた、組織のデータに関する重要な情報を表示できます。

| 機能 | 説明 |
| --- | --- |
| [ライセンス使用状況ダッシュボードの改善](../../dashboards/guides/license-usage.md) | 組織のライセンス使用状況に関するレポートの改善と主要指標のビジュアライゼーションにより、使用許諾契約の管理を維持します。 これらの改善により、購入したすべてのExperience Platform製品のライセンス使用状況指標に対して高い精度を提供します。 |

{style="table-layout:auto"}

ライセンス使用状況ダッシュボードについて詳しくは、 [ライセンス使用状況ダッシュボードの概要](../../dashboards/guides/destinations.md).

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| データストリーム | デバイス参照のサポート | データストリームを設定する際に、収集するデバイス参照情報のレベルを選択できるようになりました。 デバイス参照情報には、ページとのやり取りに使用されるデバイス、ハードウェア、オペレーティングシステム、ブラウザーに関するデータが含まれます。 <br>  ユーザーエージェントやクライアントヒントと共に、デバイス参照情報を収集することはできません。 デバイス情報の収集を選択すると、ユーザーエージェントとクライアントヒントの収集が無効になり、その逆も無効になります。 すべてのデバイス参照情報は、 `xdm:device` フィールドグループを使用します。 詳しくは、 [データ・ストリームの構成](../../datastreams/configure.md#geolocation-device-lookup). |
| 拡張機能 | [!DNL TikTok] web イベント API 拡張機能 | The [[!DNL TikTok] ウェブイベント API](https://exchange.adobe.com/apps/ec/109834/tiktok-web-events-api) 拡張機能を使用すると、 Adobe Experience Platform Edge Network で取得したデータを活用し、に送信できます。 [!DNL TikTok] を使用して、サーバー側のイベントの形式で [!DNL TikTok] ウェブイベント API。 |

{style="table-layout:auto"}

データ収集の詳細については、 [データ収集の概要](../../tags/home.md).

## データガバナンス {#data-governance}

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。これは Experience Platform 内の様々なレベルで重要な役割を果たします。例えば、カタログ化、データ系列、データ使用のラベル付け、データアクセスポリシー、マーケティングアクションのデータに関するアクセス制御などです。

**新機能**

| 機能 | 説明 |
| --- | --- |
| サードパーティデータ用の新しいパートナーエコシステムラベル | サードパーティのエンリッチメントおよび見込み客向けの新しいデータ使用ラベルを利用できます。 詳しくは、 [パートナーエコシステムラベルに関するドキュメント](../../data-governance/labels/reference.md#partner) を参照してください。 |

{style="table-layout:auto"}

データガバナンスについて詳しくは、[データガバナンスの概要](../../data-governance/home.md)を参照してください。

## データハイジーン {#hygiene}

Experience Platform は、消費者レコードとデータセットをプログラムで削除することで、保存されたデータを管理できる、一連のデータ衛生機能を提供します。次のいずれかを使用： [!UICONTROL データのライフサイクル] ワークスペースを使用して、またはデータ衛生 API の呼び出しを通じて、データストアを効果的に管理できます。 これらの機能を使用して、情報が期待どおりに使用され、誤ったデータを修正する必要がある場合は更新され、組織のポリシーで必要と判断される場合は削除されるようにします。

**新機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} | Adobe Experience Platformの高度なデータライフサイクル管理機能（データセットの有効期限とレコードの削除）を使用して、顧客のコミットメントとライセンス契約を満たすために、すべてのデータストアにわたってデータライフサイクルを管理します。<br>データセットの有効期限を自動化すると、データセット全体を削除し、削除するデータセットの日時を設定できます。<br>レコードの削除を使用すると、プライマリ ID をターゲティングして、個々のコンシューマープロファイルを削除できます。 UI を通じて、または CSV/JSON ファイルのアップロードを通じて、プライマリ ID を個別に指定できます。 詳しくは、 [レコードの削除に関するドキュメント](../../hygiene/ui/record-delete.md) 詳細情報 |
| データセット有効期限 | データを最小限に抑え、自動データセット有効期限を使用して、使用許諾契約を常に管理します。 データセット全体を削除し、削除するデータセットの日時を設定することで、データの量を減らします。 詳しくは、 [データセット有効期限に関するドキュメント](../../hygiene/ui/dataset-expiration.md) を参照してください。 |

{style="table-layout:auto"}

Platform のデータハイジーン機能について詳しくは、[データハイジーンの概要](../../hygiene/home.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先** {#new-updated-destinations}

| 宛先 | 新規／アップデート | 説明 |
| ----------- |----------------|----------- |
| [[!DNL LiveRamp - Distribution]](../../destinations/catalog/advertising/liveramp-distribution.md) | 新規 | 以前に転送されたオーディエンスのアクティブ化 [!DNL LiveRamp] モバイル、web、ディスプレイ、接続されたテレビメディアをまたいだプレミアム発行者を対象にしています。 <br> オーディエンスを [!DNL LiveRamp] を通じて説明する [LiveRamp - Onboarding](../../destinations/catalog/advertising/liveramp-onboarding.md) 接続、新しい [[!DNL LiveRamp - Distribution]](../../destinations/catalog/advertising/liveramp-distribution.md) 接続を使用して、ダウンストリームの宛先に対してオーディエンスをアクティブ化できます。 |
| [[!DNL HubSpot]](../../destinations/catalog/crm/hubspot.md) | 新規 | [[!DNL HubSpot]](https://www.hubspot.com) は、マーケティング、セールス、コンテンツ管理、顧客サービスに接続するために必要なすべてのソフトウェア、統合、リソースを含む CRM プラットフォームです。 1 つの CRM プラットフォーム上でデータ、チーム、顧客を接続できます。 |
| [[!DNL Microsoft Dynamics 365]](../../destinations/catalog/crm/microsoft-dynamics-365.md) | 更新済み | のサポートを追加しました。 [!DNL Dynamics 365] カスタムフィールドのカスタムフィールドプレフィックス ( [!DNL Dynamics 365]. 新しい入力フィールド **[!UICONTROL Customization Prefix]**&#x200B;に追加され、 [宛先の詳細を入力](#destination-details) 手順 |
| [[!DNL Experience Cloud Audiences]](../../destinations/catalog/adobe/experience-cloud-audiences.md) | 更新済み | これで、Experience Cloudオーディエンスの宛先が一般的に使用できるようになりました。 この宛先を使用して、Real-Time CDPからAudience ManagerおよびAdobe Analyticsにオーディエンスをアクティブ化します。 オーディエンスをAdobe Analyticsに送信するには、Audience Managerライセンスが必要です。 |

{style="table-layout:auto"}

<!-- 


Add these to release notes as they go out

| [[!DNL Qualtrics]] | New | Use the aggregation of multiple sources of operational data in Adobe Experience Platform as an input in Qualtrics Experience ID to better understand your customers and enable targeted outreach to close the gap when it comes to understanding intent, emotion and experience drivers. | 

-->

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| Real-Time CDPでのデータエクスポート | The [データセットの書き出し](../../destinations/ui/export-datasets.md) の機能が一般的に利用できるようになりました。 詳しくは、 [Experience Platformアプリに基づいて書き出すことができるデータセット](../../destinations/ui/export-datasets.md#datasets-to-export) 購入した後、 [データセットを書き出すためのガードレール](/help/destinations/guardrails.md#dataset-exports). |
| （ベータ版）配列型オブジェクトの書き出しのサポート | プリミティブ値（文字列、整数またはブール値）の配列をフラットスキーマファイルとしてクラウドストレージの宛先に書き出します。 詳しくは、 [ドキュメント](../../destinations/ui/export-arrays-calculated-fields.md). |
| Destination SDKの動的なドロップダウンセレクター | Destination SDKを通じて宛先を作成する場合、 [動的ドロップダウンセレクター](../../destinations/destination-sdk/functionality/destination-configuration/customer-data-fields.md#dynamic-dropdown-selectors) をクリックして、API から取得した値をドロップダウンセレクターのフィールドに入力します。 |

**修正および機能強化** {#destinations-fixes-and-enhancements}

- を利用する [透明性の監視](../../dataflows/ui/monitor-destinations.md#dataflow-runs-for-streaming-destinations) エンタープライズ宛先 ([HTTP API](../../destinations/catalog/streaming/http-destination.md), [Amazon Kinesis](../../destinations/catalog/cloud-storage/amazon-kinesis.md) および [Azure イベントハブ](../../destinations/catalog/cloud-storage/azure-event-hubs.md)) をデータフローの実行レベルで更新し、 [データフローの詳細表示](../../dataflows/ui/monitor-destinations.md#dataflow-run-details-page)を追加しました。
- マッピングされたオーディエンスの名前を [Google Ad Manager](../../destinations/catalog/advertising/google-ad-manager.md), [Google Display &amp; Video 360](../../destinations/catalog/advertising/google-dv360.md)、およびを使用するその他の宛先 [オーディエンス更新テンプレート](../../destinations/destination-sdk/metadata-api/update-audience-template.md)の場合、これらの名前の変更は、宛先の下流で反映されるようになりました。

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| スキーマエディターに追加されたクイックアクション | スキーマエディターのキャンバスに新しいクイックアクションが追加されました。 JSON 構造をコピーしたり、エディターから直接スキーマを削除したりできるようになりました。<br>![スキーマエディターのクイックアクション。](../2023/assets/schema-editor-copy-json.png "「その他」と「 JSON にコピー」が強調表示されたスキーマエディター。"){width="100" zoomable="yes"} |
| カスタム作成者または標準作成者による XDM リソースのフィルタリング | 使用可能なスキーマ、フィールドグループ、データタイプおよびクラスのリストが、作成方法に基づいて事前にフィルタリングされるようになりました。 これにより、リソースを、カスタムで作成されたか、Adobeで作成されたかに基づいてフィルタリングできます。<br>![スキーマワークスペースの標準フィルターとカスタムフィルター。](../2023/assets/standard-and-custom-classes.png "標準フィルターとカスタムフィルターが強調表示されたスキーマワークスペース。"){width="100" zoomable="yes"} <br> 詳しくは、 [リソースの作成と編集に関するドキュメント](../../xdm/ui/resources/classes.md#filter.md) を参照してください。 |

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| スキーマ作成ワークフローの更新 | プロセスを合理化するための新しいスキーマ作成ワークフローが実装されました。 <br> ![新しいスキーマ作成 UI。](../2023/assets/schema-class-options.png "新しいスキーマ詳細セレクターが強調表示されています。"){width="100" zoomable="yes"} <br> 詳しくは、 [スキーマ作成ドキュメント](../../xdm/ui/resources/schemas.md#create) を参照してください。 |

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| データタイプ | [[!UICONTROL 戻る]](https://github.com/adobe/xdm/pull/1773/files) | 発行された RMA(Return Murchandise Authorization)。 |
| データタイプ | [[!UICONTROL 項目を返す]](https://github.com/adobe/xdm/pull/1773/files) | RMA 内の返品品目の情報（返品商品承認）。 |

{style="table-layout:auto"}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明のアップデート |
| --- | --- | --- |
| 拡張機能 | [!UICONTROL AJO エンティティフィールド] | The [[!UICONTROL マルチバリアント用のフラグ]](https://github.com/adobe/xdm/pull/1774/files) がに追加されました [!UICONTROL AJO エンティティフィールド] を使用して、バリアントがマルチバリアントかどうかを識別します。 |
| データタイプ | [!UICONTROL 製品リスト項目] | [[!UICONTROL 項目を返す]](https://github.com/adobe/xdm/pull/1773/files) が追加され、Return Murchandise Authorization 情報が含まれるようになりました。 |
| データタイプ | 注文 | [[!UICONTROL 情報を返す]](https://github.com/adobe/xdm/pull/1773/files) が追加され、発行された RMA(Return Murchandise Authorization) が含まれます。 |

{style="table-layout:auto"}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| ID サービス UI の強化 | Experience PlatformUI の改善されたカスタム名前空間作成ツールを使用して、カスタム名前空間と、対応する ID タイプをより適切に管理できます。 拡張された ID サービス UI には、次の機能が備わっています。 <ul><li>コンテキストエクスペリエンス： ID 名前空間と ID タイプに対する視覚的な手掛かり、明確性、コンテキスト。</li><li>精度：エラー処理の改善。ID 名の重複がなくなりました。</li><li>検出性：製品内ダイアログ内からドキュメントにアクセスできます。</li></ul> 詳しくは、 [カスタム名前空間の作成](../../identity-service/namespaces.md#create-namespaces). |
| ID グラフの制限の変更 | ID グラフの上限が 150 ID から 50 ID に変更されました。 新しい ID がフルグラフに取り込まれると、取り込みタイムスタンプと ID タイプに基づく最も古い ID が削除されます。 Cookie の ID タイプは削除用に優先されます。 実稼動用サンドボックスに次の情報が含まれている場合は、Adobeアカウントチームに連絡して、ID タイプの変更をリクエストしてください。 <ul><li>ユーザー識別子（CRM ID など）を cookie/デバイス id タイプとして設定するカスタム名前空間。</li><li>cookie/device 識別子がクロスデバイス id タイプとして設定されるカスタム名前空間。</li></ul> Adobeエンジニアリングがこれらのリクエストを手動で処理します。 詳しくは、 [ID サービスデータのガードレール](../../identity-service/guardrails.md) そしてガイド [データ管理ライセンス使用権限のベストプラクティス](../../landing/license-usage-and-guardrails/data-management-best-practices.md). |

{style="table-layout:auto"}

ID サービスの詳細については、 [ID サービスの概要](../../identity-service/home.md).

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。任意のデータセットを [!DNL Data Lake] から結合し、クエリの結果を新しいデータセットとして取得することで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| ログのフィルタリング UI の更新 | クエリログのフィルタリングが改善され、監視、管理、トラブルシューティングに関するユーザー生成ログの可視性が向上しました。 様々な設定に基づいてクエリログのリストをフィルタリングできます。 <br> ![クエリログのフィルター設定です。](../2023/assets/log-filter-settings.png "新しいクエリログフィルターがハイライト表示されます。"){width="100" zoomable="yes"}  <br> 詳しくは、 [クエリログドキュメント](../../query-service/ui/query-logs.md#filter-logs) を参照してください。 |
| 複数のクエリエディター UI の更新 | クエリエディターで複数の順次クエリを実行したり、複数のクエリを記述して、すべてのクエリを順次実行したりできるようになりました。 クエリの実行に柔軟性を高めるには、選択したクエリをハイライト表示し、他のクエリとは別に実行する特定のクエリを選択します。 詳しくは、 [クエリエディター UI ガイド](../../query-service/ui/user-guide.md#execute-multiple-sequential-queries) を参照してください。 |

{style="table-layout:auto"}

クエリサービスについて詳しくは、[クエリサービスの概要](../../query-service/home.md)を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| カスタマイズ可能な列 | サイズを変更できる列を使用して、Audience Portal のレイアウトをカスタマイズできるようになりました。 この機能の詳細については、 [セグメント化 UI ガイド](../../segmentation/ui/overview.md#customize). |
| 頻度の分類を更新 | これで、組織内のオーディエンスの更新頻度の分類を表示できます。 この機能の詳細については、 [セグメント化 UI ガイド](../../segmentation/ui/overview.md#browse). |

セグメント化サービスの詳細については、「[セグメント化サービスの概要](../../segmentation/home.md)」を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 次の新しいパラメーターが追加されました： `offset` セルフサービスソースでのページネーション（バッチ SDK） | 次に、 `endConditionName` および `endConditionValue` を使用している場合は `offset` ページネーション。 これらのパラメーターを使用すると、次の HTTP リクエストでページネーションループを終了する条件を指定できます。 詳しくは、 [セルフサービスソース（バッチ SDK）のページネーションガイド](../../sources/sources-sdk/config/sourcespec.md#pagination). |

{style="table-layout:auto"}

ソースの詳細については、 [ソースの概要](../../sources/home.md).
