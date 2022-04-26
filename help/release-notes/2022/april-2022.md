---
title: Adobe Experience Platformリリースノート 2022 年 4 月
description: Adobe Experience Platformの 2022 年 4 月のリリースノート。
source-git-commit: 4bbf7642a456f36ea0fe7fc1c8d68ad37351ff4c
workflow-type: tm+mt
source-wordcount: '907'
ht-degree: 12%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 4 月 27 日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [エクスペリエンスデータモデル（XDM）](#xdm)

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platformに取り込まれるデータの共通の構造と定義（スキーマ）を提供するオープンソース仕様です。 XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| スキーマの個々の標準フィールドを追加または削除する | スキーマエディターの UI で、標準フィールドグループの一部をスキーマに追加できるようになり、カスタムリソースを最初から作成しなくても、含めるように選択したフィールドの柔軟性が向上しました。<br><br>また、スキーマ構造内で直接アドホックカスタムフィールドを定義し、事前にフィールドグループを作成または編集しなくても、新しいカスタムフィールドグループまたは既存のカスタムフィールドグループに割り当てることができるようになりました。<br><br>詳しくは、 [UI でのスキーマの作成と編集](../../xdm/ui/resources/schemas.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

**新しい XDM コンポーネント**

| コンポーネントの種類 | 名前 | 説明 |
| --- | --- | --- |
| グローバルスキーマ | [[!UICONTROL データ衛生操作リクエスト]](https://github.com/adobe/xdm/blob/master/schemas/hygiene/aep-hygiene-ops-record.schema.json) | 指定したデータセットまたはサンドボックス内のレコードを削除または変更するためのデータクレンジング要求の詳細をキャプチャします。 |
| 記述子 | [[!UICONTROL 時系列の精度記述子]](https://github.com/adobe/xdm/blob/master/schemas/descriptors/time-series/descriptorTimeSeriesGranularity.schema.json) | 時系列データと概要データの精度を示します。 スキーマに適用される場合、スキーマの `timestamp` フィールドは、この精度の期間での最初のタイムスタンプです。 |
| クラス | [[!UICONTROL XDM 概要指標]](https://github.com/adobe/xdm/blob/master/components/classes/summary_metrics.schema.json) | SQL SELECT と GROUP BY の結果など、グループ化ディメンションと共に事前に要約された指標を提供します。 |
| フィールドグループ | [[!UICONTROL 同意ポリシーの評価結果マップ]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-site-search.schema.json) | 個人の同意ポリシーの評価結果をキャプチャします。 |
| フィールドグループ | [[!UICONTROL サイト検索]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-site-search.schema.json) | 検索クエリ、フィルタリング、並べ替えなど、サイト検索関連の情報をキャプチャします。 |
| フィールドグループ | [[!UICONTROL リードのマージ]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/events/merge-leads.schema.json) | 2 つ以上のリードがマージされるイベントの詳細をキャプチャします。 |
| フィールドグループ | [[!UICONTROL 送信済み電子メール]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/events/emailsent.schema.json) | 電子メールが受信者に送信されるイベントの詳細をキャプチャします。 |
| フィールドグループ | [[!UICONTROL フィールドのステッチ]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-stitching.schema.json) | イベントの ID ステッチプロセスで計算された値をキャプチャします。 |
| フィールドグループ | [[!UICONTROL 監査用のセカンダリ受信者の詳細]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/secondary-recipient-detail.schema.json) | 監査のセカンダリ受信者の詳細を取得するAdobe Journey Optimizerのフィールドグループです。 |
| フィールドグループ | [[!UICONTROL XDM ビジネスアカウント人物関係の詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/account-person/account-person-details.schema.json) | アカウントと人物の関係に関連する詳細をキャプチャします。 |
| フィールドグループ | [[!UICONTROL アカウント担当者の詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/account-person/account-person-details.schema.json) | アカウントと人物の関係に関連する詳細をキャプチャします。 |
| データタイプ | [[!UICONTROL 買い物かご]](https://github.com/adobe/xdm/blob/master/components/datatypes/cart.schema.json) | e コマースの買い物かごに関する情報をキャプチャします。 |
| データタイプ | [[!UICONTROL 送料]](https://github.com/adobe/xdm/blob/master/components/datatypes/shipping.schema.json) | 1 つ以上の製品の配送先情報をキャプチャします。 |
| データタイプ | [[!UICONTROL サイト検索]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-site-search.schema.json) | サイト検索アクティビティの情報をキャプチャします。 |
| 拡張機能 (Workfront) | [[!UICONTROL オペレーショナルタスク属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/opTask.schema.json) | 操作タスクに関連する詳細をキャプチャします。 |
| 拡張機能 (Workfront) | [[!UICONTROL 作業Portfolio属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/portfolio.schema.json) | 作業ポートフォリオに関連する詳細をキャプチャします。 |
| 拡張機能 (Workfront) | [[!UICONTROL 作業プログラム属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/program.schema.json) | 作業プログラムに関連する詳細をキャプチャします。 |
| 拡張機能 (Workfront) | [[!UICONTROL 作業プロジェクト属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/project.schema.json) | 作業プロジェクトに関連する詳細をキャプチャします。 |

{style=&quot;table-layout:auto&quot;}

**XDM コンポーネントの更新**

| コンポーネントの種類 | 名前 | 説明のアップデート |
| --- | --- | --- |
| グローバルスキーマ | [[!UICONTROL 宛先]](https://github.com/adobe/xdm/blob/master/schemas/destinations/destination.schema.json) | 次の新しい列挙値 `destinationCategory`. |
| 記述子 | [[!UICONTROL わかりやすい名前記述子]](https://github.com/adobe/xdm/blob/master/schemas/descriptors/display/alternateDisplayInfo.schema.json) | 推奨値 (`meta:enum`) は標準フィールドからは不要です。 |
| フィールドグループ | [[!UICONTROL ユーザーログインプロセス]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/experience-event/experienceevent-user-login-details.schema.json) | `createProfile` フィールドが追加されました。 |
| データタイプ | [[!UICONTROL コマース]](https://github.com/adobe/xdm/blob/master/components/datatypes/marketing/commerce.schema.json) | 買い物かご関連のフィールドがいくつか追加されました。 |
| データタイプ | [[!UICONTROL 製品リスト項目]](https://github.com/adobe/xdm/blob/master/components/datatypes/productlistitem.schema.json) | 選択したオプションと割引額に対して追加される新しいフィールド。 |
| 拡張機能（インテリジェントサービス） | [[!UICONTROL Intelligent Services JourneyAI Send Time Optimization]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/intelligentServices/profile-journeyai-sendtimeoptimization.schema.json) | 送信時スコアのストレージ形式を最適化します。 |
| 拡張機能 (Workfront) | [[!UICONTROL Workfront Change イベント]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/changeevent.schema.json) | 複数のフィールドが `workfront:customData` カスタムフォームフィールドのフィールド。 |
| 拡張機能 (Workfront) | [[!UICONTROL 作業タスクの属性]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/task.schema.json) | 複数のフィールドが追加されました。 |
| 拡張機能 (Workfront) | [[!UICONTROL 作業用オブジェクト]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/workfront/workobject.schema.json) | 親オブジェクトタイプとカスタムフォームフィールド用の新しいフィールドです。 |

{style=&quot;table-layout:auto&quot;}

Platform での XDM について詳しくは、 [XDM システムの概要](../../xdm/home.md).
