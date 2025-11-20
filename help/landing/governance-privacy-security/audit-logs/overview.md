---
title: 監査ログ概要
description: 監査ログを使用して、Adobe Experience Platform で誰が何のアクションを実行したかを確認する方法を説明します。
role: Admin,Developer
feature: Audits
exl-id: 00baf615-5b71-4e0a-b82a-ca0ce8566e7f
source-git-commit: d6575e44339ea41740fa18af07ce5b893f331488
workflow-type: tm+mt
source-wordcount: '1579'
ht-degree: 31%

---

# 監査ログ {#audit-logs}

>[!CONTEXTUALHELP]
>id="platform_audits_privacyconsole_actions"
>title="上位のアクション"
>abstract="このウィジェットには、選択した期間内に Experience Platform で実行されたアクションの上位のタイプが表示されます。Experience Platform で記録されたアクションの完全なリストを表示するには、左側のナビゲーションの「**監査**」を選択します。"

>[!CONTEXTUALHELP]
>id="platform_audits_privacyconsole_users"
>title="上位のユーザー"
>abstract="このウィジェットには、選択した期間内に Experience Platform で最も多くアクションを実行したユーザーが表示されます。Experience Platform で記録されたアクションの完全なリストを表示するには、左側のナビゲーションの「**監査**」を選択します。"

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_audits_description"
>title="Experience Platform でのユーザーアクティビティの監視"
>abstract="<h2>説明</h2><p>監査ログの形式で、様々な Experience Platform サービスと機能のユーザーアクティビティを監視できます。これらのログは、<b>誰</b>が<b>いつ</b>、<b>どの</b>アクションを実行したかを記録する監査記録を形成します。監査ログは、Experience Platform に関する問題のトラブルシューティングに役立ち、企業のデータ管理ポリシーおよび規制要件に効果的に準拠するのに役立ちます。</p>"

システムで実行されるアクティビティの透明性と可視性を高めるために、Adobe Experience Platformでは、様々なサービスや機能に関するユーザーアクティビティを「監査ログ」の形式で監査できます。 これらのログは、Experience Platformに関する問題のトラブルシューティングに役立つ監査証跡を形成し、企業のデータ管理ポリシーおよび規制要件に効果的に準拠するのに役立ちます。

基本的な意味では、監査ログは&#x200B;**&#x200B;**&#x200B;誰が&#x200B;**何を**&#x200B;アクションを実行したか、および&#x200B;**いつ**&#x200B;かを示します。ログに記録される各アクションには、アクションタイプ、日時、アクションを実行したユーザーの電子メール ID、アクションタイプに関連する追加属性を示すメタデータが含まれます。

ユーザーがアクションを実行すると、2 種類の監査イベントが記録されます。 コアイベントは、アクション（[!UICONTROL allow] または [!UICONTROL deny]）の認証結果をキャプチャし、拡張イベントは、実行結果（[!UICONTROL success] または [!UICONTROL failure]）をキャプチャします。 複数の拡張イベントを同じコアイベントにリンクできます。 例えば、宛先をアクティブ化すると、コアイベントは [!UICONTROL Destination Update] アクションの認証を記録し、拡張イベントは複数の [!UICONTROL Segment Activate] アクションを記録します。

>[!NOTE]
>
> **Role** リソース内のアクション **ユーザーの追加** および **ユーザーの削除** のメタデータには、アクションを実行したユーザーのメール ID は含まれません。 代わりに、ログにはシステムで生成されたメール ID （system@adobe.com）が表示されます。

このドキュメントでは、UI または API での表示方法や管理方法など、Experience Platformの監査ログについて説明します。

## 監査ログで記録されるイベントタイプ {#category}

次の表に、監査ログでリソースが記録されるアクションの概要を示します。

| リソース | アクション |
| --- | --- |
| [アクセス制御ポリシー (属性ベースのアクセス制御)](../../../access-control/home.md) | <ul><li> の作成</li><li>更新</li><li>削除</li></ul> |
| [&#x200B; アカウント （Adobe） &#x200B;](../../../sources/connectors/tutorials/ui/../../../tutorials/ui/update.md) | <ul><li> の作成</li><li>更新</li><li>削除</li></ul> |
| [Attribution AIインスタンス](../../../intelligent-services/attribution-ai/overview.md) | <ul><li> の作成</li><li>更新</li><li>削除</li><li>有効にする</li><li>無効にする</li></ul> |
| [監査ログ](../../../landing/governance-privacy-security/audit-logs/overview.md) | <ul><li>書き出し</li></ul> |
| [クラス](../../../xdm/schema/composition.md#class) | <ul><li> の作成</li><li>更新</li><li>削除</li></ul> |
| 計算された属性 | <ul><li> の作成</li><li>更新</li><li>削除</li></ul> |
| [顧客 AI インスタンス](../../../intelligent-services/customer-ai/overview.md) | <ul><li> の作成</li><li>更新</li><li>削除</li><li>有効にする</li><li>無効にする</li></ul> |
| [データセット](../../../catalog/datasets/overview.md) | <ul><li> の作成</li><li>更新</li><li>削除</li><li>[Real-時間 Customer プロフィール の有効化](../../../profile/home.md)</li><li>プロファイルを無効にする</li><li>データの追加</li><li>バッチを削除</li></ul> |
| [データストリーム](../../../datastreams/overview.md) | <ul><li> の作成</li><li>更新</li><li>削除</li><li>有効にする</li><li>無効にする</li><li>[編集マッピング](../../../datastreams/data-prep.md)</li></ul> |
| [データタイプ](../../../xdm/schema/composition.md#data-type) | <ul><li> の作成</li><li>更新</li><li>削除</li></ul> |
| [宛先](../../../destinations/home.md) | <ul><li> の作成</li><li>更新</li><li>削除</li><li>有効にする</li><li>無効にする</li><li>データセットをアクティブ化</li><li>データセットを削除</li><li>プロファイルをアクティブ化</li><li>プロファイルを削除</li></ul> |
| [&#x200B; フィールドグループ &#x200B;](../../../xdm/schema/composition.md#field-group) | <ul><li> の作成</li><li>更新</li><li>削除</li></ul> |
| [ID グラフ &#x200B;](../../../identity-service/features/identity-graph-viewer.md) | <ul><li>表示</li></ul> |
| [ID 名前空間 &#x200B;](../../../identity-service/features/namespaces.md) | <ul><li> の作成</li><li>更新</li></ul> |
| [ポリシーを結合](../../../profile/merge-policies/overview.md) | <ul><li> の作成</li><li>更新</li><li>削除</li></ul> |
| [製品プロファイル](../../../access-control/home.md) | <ul><li> の作成</li><li>更新</li><li>削除</li></ul> |
| [クエリ](../../../query-service/ui/overview.md) | <ul><li>実行</li></ul> |
| [&#x200B; クエリテンプレート &#x200B;](../../../query-service/ui/overview.md) | <ul><li> の作成</li><li>更新</li><li>削除</li></ul> |
| [&#x200B; 役割（属性ベースのアクセス制御） &#x200B;](../../../access-control/home.md) | <ul><li> の作成</li><li>更新</li><li>削除</li><li>ユーザーを追加</li><li>ユーザーを削除</li></ul> |
| [サンドボックス](../../../sandboxes/home.md) | <ul><li> の作成</li><li>更新</li><li>リセット</li><li>削除</li></ul> |
| [予定クエリ](../../../query-service/ui/overview.md) | <ul><li> の作成</li><li>更新</li><li>削除</li></ul> |
| [スキーマ](../../../xdm/schema/composition.md) | <ul><li> の作成</li><li>更新</li><li>削除</li><li>プロファイルを有効にする</li></ul> |
| [セグメント](../../../segmentation/home.md) | <ul><li> の作成</li><li>削除</li><li>セグメントをアクティブ化</li><li>セグメントを削除</li></ul> |
| [Sourceのデータフロー &#x200B;](../../../sources/connectors/tutorials/ui/../../../tutorials/ui/update.md) | <ul><li> の作成</li><li>更新</li><li>削除</li><li>有効にする</li><li>無効にする</li><li>データセットをアクティベート</li><li>データセットを削除</li><li>プロファイル無効化</li><li>プロファイルを削除</li></ul> |
| [&#x200B; 作業指示 &#x200B;](../../../hygiene/home.md) | <ul><li> の作成</li></ul> |

## 監査ログへのアクセス

組織に対してこの機能が有効になっている場合、アクティビティが発生すると監査ログが自動的に収集されます。 ログ収集を手動で有効にする必要はありません。

監査ログを表示および書き出すには、**[!UICONTROL View User Activity Log]** のアクセス制御権限（[!UICONTROL Data Governance] カテゴリに表示）が付与されている必要があります。 Experience Platform機能の個々の権限を管理する方法については、[&#x200B; アクセス制御ドキュメント &#x200B;](../../../access-control/home.md) を参照してください。

## UI での監査ログの管理 {#managing-audit-logs-in-the-ui}

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_audits_instructions"
>title="手順"
>abstract="<ul><li>左側のナビゲーションの「<b>監査</b>」を選択します。監査ワークスペースには、記録されたログのリストが表示されます。デフォルトでは、最新のログから古いログの順に並べ替えられています。</li>   <li> メモ：監査ログは、365 日間保持され、その後システムから削除されます。したがって、遡ることができる期間は最大 365 日までです。365 日より前のデータを振り返る必要がある場合は、社内ポリシーの要件を満たすために定期的にログを書き出す必要があります。 </li><li>リストからイベントを選択して、その詳細を右側のパネルに表示します。 </li><li>ファネルアイコンを選択して、結果を絞り込むのに役立つフィルターコントロールのリストを表示します。選択した各種フィルターに関係なく、最新 1000 件のレコードのみが表示されます。 </li><li>監査ログの現在のリストを書き出すには、「**ログをダウンロード**」を選択します。</li><li>この機能に関する詳しいヘルプについては、Experience League の<a href="https://experienceleague.adobe.com/docs/experience-platform/landing/governance-privacy-security/audit-logs/overview.html?lang=ja">監査ログの概要</a>を参照してください。</li></ul>"

Experience Platform UI の **[!UICONTROL Audits]** Workspace 内で、様々なExperience Platform機能の監査ログを表示できます。 ワークスペースには、記録されたログのリストが表示されます。デフォルトでは、最新のログから古いログの順に並べ替えられています。

![&#x200B; 左側のメニューの監査をハイライト表示した監査ダッシュボード。](../../images/audit-logs/audits.png)

監査ログは、365 日間保持され、その後システムから削除されます。 365 日を超えるデータが必要な場合は、社内ポリシーの要件を満たすために定期的にログを書き出す必要があります。

監査ログの要求方法によって、アクセスできる期間とレコード数が変わります。 [&#x200B; ログの書き出し &#x200B;](#export-audit-logs) を使用すると、365 日（90 日間隔）を上限とする最大 10,000 個の監査ログ（コアまたはエンハンス）に戻すことができます。Experience Platformの [&#x200B; アクティビティログ UI](#filter-audit-logs) には、過去 90 日を上限とする最大 1,000 個のコアイベントが表示され、それぞれに対応するエンハンスドイベントが含まれます。

リストからイベントを選択して、その詳細を右側のパネルに表示します。

![&#x200B; イベントの詳細パネルがハイライト表示された監査ダッシュボードの「アクティビティログ」タブ。](../../images/audit-logs/select-event.png)

### 監査ログのフィルタリング

ファネルアイコン（![フィルターアイコン](/help/images/icons/filter.png)）を選択し、フィルターコントロールのリストを表示して、結果を絞り込みます。

>[!NOTE]
>
>Experience Platform UIには、適用されたフィルターに関係なく、過去 90 日間から最大 1000 個のコアイベントのみが表示され、それぞれに対応する拡張イベントが含まれます。 それを超えるログ(最大 365 日間)が必要な場合は、監査ログを [エクスポート](#export-audit-logs)する必要があります。

![フィルタリングされたアクティビティログがハイライト表示された監査ダッシュボード。](../../images/audit-logs/filters.png)

UIの監査イベントでは、次のフィルターを使用できます。

| フィルター | 説明 |
| --- | --- |
| [!UICONTROL Category] | ドロップダウンメニューを使用して、表示された結果を [カテゴリ](#category)でフィルタリングします。 |
| [!UICONTROL Action] | アクションごとにフィルターします。 各サービスで使用できるアクションは、上のリソース表で確認できます。 |
| [!UICONTROL User] | ユーザーでフィルタリングする完全なユーザー ID(例: `johndoe@acme.com`)を入力します。 |
| [!UICONTROL Status] | フィルター 監査イベント (成功、失敗、許可、または [アクセス制御](../../../access-control/home.md) アクセス許可がないために拒否されたイベント)。 実行されたアクションの場合、コアイベントには [!UICONTROL Allow] または [!UICONTROL Deny]が表示されます。 コアイベントが [!UICONTROL Allow]されると、 **[!UICONTROL Success]** または **[!UICONTROL Failure]**&#x200B;を示す 1 つ以上の拡張イベントがアタッチされている可能性があります。 たとえば、アクションが成功すると、コアイベントに [!UICONTROL Allow] が表示され、アタッチされた拡張イベントに [!UICONTROL Success] が表示されます。 |
| [!UICONTROL Date] | 開始日や終了日を選択して、結果をフィルターする日付範囲を定義します。 データ は、90 日間のルックバック期間 (2021-12-15 から 2022-03-15 など) でエクスポートできます。 これはイベントタイプによって異なる場合があります。 |

フィルターを削除するには、問題のフィルターのピルアイコンの「X」を選択するか、 **[!UICONTROL Clear all]** を選択してすべてのフィルターを削除します。

![明確なフィルターが強調表示された監査ダッシュボード。](../../images/audit-logs/clear-filters.png)

返される監査ログデータには、選択したフィルター条件に一致するすべてのクエリに関する次の情報が含まれます。

| 列の名前 | 説明 |
|---|---|
| [!UICONTROL Timestamp] | `month/day/year hour:minute AM/PM`形式で実行されたアクションの正確な日時。 |
| [!UICONTROL Asset Name] | [!UICONTROL Asset Name]フィールドの値は、フィルターとして選択したカテゴリによって異なります。 |
| [!UICONTROL Category] | このフィールドは、フィルターのドロップダウンで選択されているカテゴリと一致します。 |
| [!UICONTROL Action] | 使用できるアクションは、フィルターとして選択したカテゴリによって異なります。 |
| [!UICONTROL User] | このフィールドには、クエリを実行したユーザー ID が表示されます。 |

![&#x200B; フィルタリングされたアクティビティログがハイライト表示された監査ダッシュボード。](../../images/audit-logs/filtered.png)

### 監査ログの書き出し {#export-audit-logs}

監査ログの現在のリストをエクスポートするには、「**[!UICONTROL Download log]**」を選択します。

>[!NOTE]
>
>ログは、過去 365 日までの 90 日間隔で要求できます。 ただし、1 回のエクスポート中に返すことができるログの最大量は、10,000 件の監査イベント (コアまたは拡張) です。

![[!UICONTROL Download log]が強調表示された監査ダッシュボード。](../../images/audit-logs/download.png)

表示されるダイアログで、好みの形式( **[!UICONTROL CSV]** または **[!UICONTROL JSON]**)を選択し、[ **[!UICONTROL Download]**] を選択します。 ブラウザー によって生成されたファイルがダウンロードされ、コンピューターに保存されます。

![[!UICONTROL Download]がハイライトされたファイル形式選択ダイアログ。](../../images/audit-logs/select-download-format.png)

## アラートの有効化 {#enable-alerts}

監査アラートを有効にして、次のルールの通知を受信できます。

* 作成オーディエンス
* オーディエンスの更新
* オーディエンス削除
* データセット作成
* データセットの更新
* データセット削除
* スキーマ作成
* スキーマの更新
* スキーマの削除

リストから目的のアラートを選択して、通知を受信するように登録します。 アラートについて詳しくは、[UI を使用したアラートの購読 &#x200B;](../../../observability/alerts/ui.md) についてのガイドを参照してください。

## API での監査ログの管理

UI で実行できるすべてのアクションは、API 呼び出しを使用して実行することもできます。 詳しくは、[API リファレンスドキュメント &#x200B;](https://www.adobe.io/experience-platform-apis/references/audit-query/) を参照してください。

## Adobe Admin Consoleの監査ログの管理

Adobe Admin Consoleでアクティビティの監査ログを管理する方法については、次の [&#x200B; ドキュメント &#x200B;](https://helpx.adobe.com/jp/enterprise/using/audit-logs.html) を参照してください。

## 次の手順とその他のリソース

このガイドでは、Experience Platformでの監査ログの管理方法について説明しました。 Experience Platform アクティビティの監視方法について詳しくは、[Observability Insights](../../../observability/home.md) および [&#x200B; データ取得の監視 &#x200B;](../../../ingestion/quality/monitor-data-ingestion.md) に関するドキュメントを参照してください。

Experience Platformの監査ログの理解を深めるために、次のビデオを視聴してください。

>[!VIDEO](https://video.tv.adobe.com/v/341450?quality=12&learn=on)
