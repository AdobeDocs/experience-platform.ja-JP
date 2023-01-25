---
title: Adobe Experience Platformリリースノート 2023 年 1 月
description: Adobe Experience Platformの 2023 年 1 月のリリースノート。
source-git-commit: 68e5baac9012a33d179f8ebff23deda7a8efd26b
workflow-type: tm+mt
source-wordcount: '1371'
ht-degree: 38%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年1月25 日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [Assurance](#assurance)
- [データ収集](#data-collection)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [リアルタイム顧客プロファイル](#profile)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## Assurance {#assurance}

Adobe保証を使用すると、モバイルアプリでデータを収集したりエクスペリエンスを提供したりする方法を調査、配達確認、シミュレーションおよび検証できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 検証エディター | 検証エディターに新しい機能強化が追加されました。 これらの機能強化には、検証列、新しいコード作成ツール、改善された表示が含まれます。 |

{style=&quot;table-layout:auto&quot;}

アシュランスの詳細については、 [アシュランスドキュメント](https://developer.adobe.com/client-sdks/documentation/platform-assurance/).

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しいホーム画面 | データ収集 UI のホームページが更新され、生産性を向上させるための便利なオンボーディング情報やリンクが含まれるようになりました。 これには以下が含まれます。<ol><li>ドキュメントおよび推奨ワークフローを参照してください</li><li>最近使用したプロパティ、ルール、およびデータ要素</li><li>一般的な拡張機能</li><li>クイックインストール機能による新しい拡張機能の更新</li></ol> |
| データ送信先 [!DNL Google Ads] イベント転送の使用 | これで、 [[!DNL Google Ads Enhanced Conversions] API 拡張機能](../../tags/extensions/server/google-ads-enhanced-conversions/overview.md) イベント転送の場合は、 [Google OAUTH 2 の秘密鍵](../../tags/ui/event-forwarding/secrets.md#google-oauth2)（サーバー側のデータをに安全に送信するため） [!DNL Google Ads] リアルタイムで。 |

{style=&quot;table-layout:auto&quot;}

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 文字列フィールドの推奨値を無効にする | 次の操作を実行できます。 [文字列フィールドの個々の推奨値を無効にする](../../xdm/ui/fields/enum.md) 内 [!UICONTROL スキーマ] ワークスペース（標準コンポーネントのワークスペースを含む） この機能は、推奨値を持つフィールドでのみ使用でき、列挙制約ではサポートされません。 |

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| クラス | [[!UICONTROL コンバージョン]](https://github.com/adobe/xdm/blob/master/components/classes/conversion.schema.json) | 通貨コンバージョンなどのコンバージョンデータを追跡するためのクラス。 |
| フィールドグループ | [[!UICONTROL 通貨換算レートの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/conversion/currency-conversion-details.schema.json) | のフィールドグループ [!UICONTROL コンバージョン] クラスを使用して、通貨換算に関連する追加の詳細を取得します。 |
| フィールドグループ | [[!UICONTROL 同意ポリシーの評価結果は、メタデータにマッピングされます]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-consentResultsv2.schema.jsonn) | 同意ポリシーのエントリに関するメタデータ情報や存在する情報など、複数の同意ポリシーの評価結果の詳細をキャプチャします。 |

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| データタイプ | [[!UICONTROL 広告の詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/advertisingdetails.schema.json) | この `ID` フィールド名は「 」に変更されました。 `name`、および前の `name` フィールドは現在 `friendlyName`. |
| データタイプ | [[!UICONTROL 決定提案の詳細]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/decisioning/proposition-detail.schema.json) | 追加された `selectionStrategy` 選択戦略の詳細をキャプチャするフィールド。 |
| フィールドグループ | [[!UICONTROL エクスペリエンスイベント — 提案インタラクション]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/decisioning/experienceevent-proposition-interaction.schema.json) | フィールドグループは、 [!UICONTROL ジャーニーステップイベント] クラス。 |
| データタイプ | [[!UICONTROL エラーの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/errordetails.schema.json) | この `ID` フィールド名は「 」に変更されました。 `name`. |
| データタイプ | [[!UICONTROL メディア情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/media.schema.json) | パターンの変更をビデオセグメントプロパティに戻しました。 |
| データタイプ | [[!UICONTROL QoE データの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/qoedatadetails.schema.json) | 削除された `droppedFrameCount` フィールドに入力します。 |
| データタイプ | [[!UICONTROL セッションの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/sessiondetails.schema.json) | 名前を変更した `isAuthorized` ～に向かって `authorized`を更新し、 `type` を文字列（以前はブール値）に設定します。 |
| データタイプ | [[!UICONTROL 送料]](https://github.com/adobe/xdm/blob/master/components/datatypes/shipping.schema.json) | 次の新しいフィールドが追加されました。 `shipDate`, `trackingNumber`、および `trackingURL`. |
| フィールドグループ | [[!UICONTROL AJO エンティティフィールド]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/ajo-entity-mixins.schema.json) | 次の新しいフィールドが追加されました。 `journeyNodeID`, `journeyNodeName`、および `journeyModeType`. |
| フィールドグループ | [[!UICONTROL 消費者エクスペリエンスイベント]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/experienceevent-consumer.schema.json) | フィールドグループも [!UICONTROL 概要指標] クラス。 |
| フィールドグループ | [[!UICONTROL 製品トリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/product-triggers.schema.json) | この `productTriggers` フィールドは現在、 `weather` オブジェクト。 |
| フィールドグループ | [[!UICONTROL 相対トリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/relative-triggers.schema.json) | この `relativeTriggers` フィールドは現在、 `weather` オブジェクト。 |
| フィールドグループ | [[!UICONTROL 重大なトリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/severe-triggers.schema.json) | この `severeTriggers` フィールドは現在、 `weather` オブジェクト。 |
| フィールドグループ | [[!UICONTROL 気象トリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/severe-triggers.schema.json) | この `weatherTriggers` フィールドは現在、 `weather` オブジェクト。 |
| フィールドグループ | [[!UICONTROL XDM 関連ビジネスアカウント]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/account/related-accounts.schema.json) | フィールドグループが安定しました。 |

{style=&quot;table-layout:auto&quot;}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。リアルタイム顧客プロファイルを使用すると、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。 プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

**今後の廃止** {#deprecation}

セグメントメンバーシップのライフサイクルの冗長性を取り除くには、 `Existing` のステータスは次の場所で非推奨となります： [セグメントメンバーシップマップ](../../xdm/field-groups/profile/segmentation.md) 2023 年 3 月末。 フォローアップのお知らせには、正確な廃止日が含まれます。

廃止後、セグメントで認定されたプロファイルは、次のように表示されます `Realized` 不適格なプロファイルは、引き続き次のように表されます。 `Exited`. これにより、 `Active` および `Expired` セグメントのステータス。

この変更は、 [企業の宛先](../../destinations/destination-types.md#streaming-profile-export) (Amazon Kinesis、Azure Event Hubs、HTTP API) を使用し、 `Existing` ステータス。 その場合は、ダウンストリーム統合を確認してください。 特定の期間を超えて新しく認定されたプロファイルを識別したい場合は、 `Realized` ステータスと `lastQualificationTime` を選択します。 詳しくは、Adobe担当者にお問い合わせください。

プロファイルデータを操作するためのチュートリアルやベストプラクティスなど、リアルタイム顧客プロファイルの詳細については、まず [リアルタイム顧客プロファイルの概要](../../profile/home.md).

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| Platform が生成したセグメントメンバーシップの有効期限 | 次に属する任意のセグメントメンバーシップ `Exited` ～に基づいて 30 日以上の州 `lastQualificationTime` フィールドは削除されます。 |
| 外部オーディエンスのメンバーシップの有効期限 | デフォルトでは、外部オーディエンスのメンバーシップは 30 日間保持されます。 長期間保持するには、 `validUntil` フィールドに値を入力します。 |

{style=&quot;table-layout:auto&quot;}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platformは、外部ソースからデータを取り込むことができ、Platform Services を使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

| 機能 | 説明 |
| --- | --- |
| クラウドストレージソースのサブフォルダーへのユーザーアクセスを許可する | 新しいアカウントを作成する際に、クラウドストレージソースの特定のサブフォルダーへのアクセスを定義できるようになりました。 作成したユーザーは、許可されたサブフォルダーのデータにのみアクセスできます。 この機能は、次のクラウドストレージソースで使用できます。 [Azure Blob ストレージ](../../sources/connectors/cloud-storage/blob.md), [Google Cloud Storage](../../sources/connectors/cloud-storage/google-cloud-storage.md), [Google PubSub](../../sources/connectors/cloud-storage/google-pubsub.md)、および [SFTP](../../sources/connectors/cloud-storage/sftp.md). |
| ベータ版の可用性 [!DNL SugarCRM] | [!DNL SugarCRM] ソースがベータ版で利用できるようになりました。 以下を使用： [!DNL SugarCRM Accounts & Contacts] そして [!DNL SugarCRM Events] ソースからデータを取り込む [!DNL SugarCRM] アカウントからExperience Platformへ。 詳しくは、 [[!DNL SugarCRM] 概要](../../sources/connectors/crm/sugarcrm.md). |
