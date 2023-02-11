---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platformの 2023 年 1 月のリリースノート。
source-git-commit: 6388c72aa0be8f5f91efaaa6a0edd22f3eb99de8
workflow-type: tm+mt
source-wordcount: '2431'
ht-degree: 28%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年1月25 日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Artificial Intelligence and Machine Learning Services]](#ai/ml-services)
- [Assurance](#assurance)
- [データ収集](#data-collection)
- [[!DNL Destinations]](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [リアルタイム顧客プロファイル](#profile)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## 人工知能/機械学習サービス {#ai-ml}

人工知能と機械学習サービスは、マーケティングアナリストや実践者に対して、顧客体験の使用事例で AI/ML の機能を活用する機能を提供します。 これにより、マーケティングアナリストは、ビジネスレベルの設定を使用する会社のニーズに固有のデータサイエンスの専門知識を必要とせずに、予測を設定できます。

### アトリビューション AI

Attribution AIは、コンバージョンイベントにつながるタッチポイントにクレジットを関連付けるために使用されます。 これは、マーケターが、カスタマージャーニーをまたいだ個別マーケティングタッチポイントのマーケティング効果を、マーケターが、定量化する際に役立ちます。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| HIPAA 対応 | Healthcare Shield のお客様は、保護された医療情報をAttribution AIや他の特定のExperience Platform・ベースのアプリケーションで受信、使用、保守または送信できるようになりました。 ヘルスケアシールドは、HIPAA に基づく被保険者またはビジネス関連のお客様向けです。 詳しくは、 [HIPAA およびAdobe製品とサービス](https://www.adobe.com/trust/compliance/hipaa-ready.html) |
| 追加のスコアデータセット列の編集 | 既存のモデルを編集する際に、追加のスコアデータセット列（レポート列）を追加または削除できるようになりました。 これにより、アトリビューションスコアの柔軟性が拡張され、モデルが既に作成された後で、追加のディメンションに対するインサイトが得られます。 詳しくは、 [Attribution UI ガイド](../../intelligent-services/attribution-ai/user-guide.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [AI/ML サービス](../../intelligent-services/attribution-ai/overview.md) の概要を参照してください。

### 顧客 AI

Real-time Customer Data Platformの顧客 AI は、個々のプロファイルのカスタム傾向スコア（チャーンやコンバージョンなど）を大規模に生成するために使用します。 これを実現するために、ビジネスニーズを機械学習の問題に変換したり、アルゴリズムを選択したり、トレーニング、またはデプロイする必要はありません。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| HIPAA 対応 | Healthcare Shield のお客様は、Real-time Customer Data Platformや他の特定のExperience Platform・ベースのアプリケーションの顧客 AI で、保護された医療情報を受信、使用、保守または送信できるようになりました。 ヘルスケアシールドは、HIPAA に基づく被保険者またはビジネス関連のお客様向けです。 詳しくは、 [HIPAA およびAdobe製品とサービス](https://www.adobe.com/trust/compliance/hipaa-ready.html) |

{style=&quot;table-layout:auto&quot;}

詳しくは、 [AI/ML サービス](../../intelligent-services/customer-ai/overview.md) の概要を参照してください。

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

## 宛先（更新日：2 月 2 日） {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [（ベータ版）Adobe Experience Cloud Audiences 接続](../../destinations/catalog/adobe/experience-cloud-audiences.md) | 以下を使用： [!UICONTROL （ベータ版）Adobe Experience Cloud Audiences] Experience Platformから様々なExperience Platformソリューション (Audience Manager、Analytics、Advertising Cloud、Adobe Campaign、Target、Marketoなど ) にセグメントを共有するための接続 |
| [Pega プロファイル接続](../../destinations/catalog/personalization/pega-profile.md) | 以下を使用： [!DNL Pega Profile Connector] Adobe Experience Platformで、 [!DNL Amazon] S3 ストレージを使用し、プロファイルデータをAdobe Experience Platformから独自の S3 バケットに定期的に CSV ファイルに書き出します。 In [!DNL Pega Customer Decision Hub]を使用すると、データジョブをスケジュールして、このプロファイルデータを S3 ストレージからインポートし、 [!DNL Pega Customer Decision Hub] プロファイル。 |
| [（ベータ版）トレードデスク CRM EU 接続](../../destinations/catalog/advertising/tradedesk-emails.md) | EUID（ヨーロッパの統合 ID）のリリースに伴い、次の 2 つが表示されるようになりました [!DNL The Trade Desk - CRM] の宛先 [宛先カタログ](/help/destinations/catalog/overview.md). <ul><li> EU でデータをソースする場合は、 **[!DNL The Trade Desk - CRM (EU)]** 宛先。</li><li> APAC または NAMER 地域のデータをソースにする場合は、 **[!DNL The Trade Desk - CRM (NAMER & APAC)]** 宛先。 </li></ul> |

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| ストリーミング宛先との統合に関する有料メディア同意ポリシーの強化 | An [同意政策の実施の強化](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-enhancement) オン [ストリーミング先](/help/destinations/destination-types.md#streaming-destinations) 有料メディアアクティベーションの使用例の場合 プロファイルが同意ポリシーの対象として認定されなくなった場合、Experience Platformはポリシーの終了とストリーミング先との間で積極的に通信するようになりました。 <br> <b>注意</b>:この機能は、 **[!UICONTROL プライバシーとセキュリティシールド]**、および **[!UICONTROL 医療用盾]**. |
| ベータクラウドストレージの宛先コネクタの新しい区切り文字オプション | 3 つの新しい区切り文字オプション（コロン） `:`，パイプ，セミコロン `;`) が新しいベータクラウドストレージの宛先で使用できるようになりました。 [（ベータ版）Amazon S3](/help/destinations/catalog/cloud-storage/amazon-s3.md), [（ベータ版）Azure Blob](/help/destinations/catalog/cloud-storage/azure-blob.md), [（ベータ版）Azure Data Lake Storage Gen2](/help/destinations/catalog/cloud-storage/adls-gen2.md), [（ベータ版）データランディングゾーン](/help/destinations/catalog/cloud-storage/data-landing-zone.md), [（ベータ版）Google Cloud Storage](/help/destinations/catalog/cloud-storage/google-cloud-storage.md), [（ベータ版）SFTP](/help/destinations/catalog/cloud-storage/sftp.md). <br> サポート対象の [ファイル形式オプション](/help/destinations/ui/batch-destinations-file-formatting-options.md) （ファイルベースの宛先の場合） |
| で使用できる新しいオプションパラメーター [顧客データフィールド](/help/destinations/destination-sdk/destination-configuration.md#customer-data-fields) の設定 [Destination SDK](/help/destinations/destination-sdk/overview.md) | `unique`:ユーザーの組織で設定されたすべての宛先データフローで一意の値を持つ顧客データフィールドを作成する必要がある場合は、このパラメータを使用します。 <br> 例えば、 **[!UICONTROL 統合エイリアス]** フィールド [[!UICONTROL カスタムパーソナライゼーション]](/help/destinations/catalog/personalization/custom-personalization.md#parameters) の宛先は一意である必要があります。つまり、この宛先への 2 つの異なるデータフローが、このフィールドに同じ値を持つことはできません。 |

**修正点および機能強化** {#destinations-fixes-and-enhancements}

<table>
    <tr>
        <td><b>修正または機能強化</b></td>
        <td><b>説明</b></td>
    </tr>
    <tr>
        <td>ファイルベースの宛先への書き出し動作の更新 (PLAT-123316)</td>
        <td>の動作の問題を修正しました <a href="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html?lang=en#mandatory-attributes">必須属性</a> データファイルをバッチ保存先に書き出す際に使用します。 <br> 以前は、出力ファイル内のすべてのレコードに次の両方が含まれていることを検証していました。 <ol><li>NULL 以外の <code>mandatoryField</code> 列と</li><li>他の非必須フィールドの少なくとも 1 つに null 以外の値を指定します。</li></ol> 2 つ目の条件が削除されました。 その結果、次の例に示すように、書き出されたデータファイルに、より多くの出力行が表示される場合があります。<br> <b> 2023 年 1 月リリースより前のサンプル動作 </b> <br> 必須フィールド： <code>emailAddress</code> <br> <b>アクティブ化するデータを入力</b> <br><table><thead><tr><th>firstName</th><th>emailAddress</th></tr></thead><tbody><tr><td>John</td><td>john@acme.com</td></tr><tr><td>null</td><td>peter@acme.com</td></tr><tr><td>ジェニファー</td><td>jennifer@acme.com</td></tr><tr><td>null</td><td>diana@acme.com</td></tr></tbody></table> <br> <b>有効化の出力</b> <br><table><thead><tr><th>firstName</th><th>emailAddress</th></tr></thead><tbody><tr><td>John</td><td>john@acme.com</td></tr><tr><td>ジェニファー</td><td>jennifer@acme.com</td></tr></tbody></table> <br> <b> 2023 年 1 月リリース以降のサンプル動作 </b> <br> <b>有効化の出力</b> <br> <table><thead><tr><th>firstName</th><th>emailAddress</th></tr></thead><tbody><tr><td>John</td><td>john@acme.com</td></tr><tr><td>null</td><td>peter@acme.com</td></tr><tr><td>ジェニファー</td><td>jennifer@acme.com</td></tr><tr><td>null</td><td>diana@acme.com</td></tr></tbody></table> </td>
    </tr>
    <tr>
        <td>必要なマッピングおよび重複マッピングの UI および API 検証 (PLAT-123316)</td>
        <td>UI と API で、検証が次のように、 <a href="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html?lang=en#mapping">フィールドのマッピング</a> 「宛先のアクティブ化」ワークフローで、次の操作をおこないます。<ul><li><b>必須マッピング</b>:宛先の開発者が、必要なマッピングを使用して宛先を設定した場合 ( 例： <a href="https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/advertising/google-ad-manager-360-connection.html?lang=en">Google Ad Manager 360</a> 宛先 ) の場合、宛先へのデータをアクティブ化する際に、これらの必要なマッピングをユーザーが追加する必要があります。 </li><li><b>マッピングを複製</b>:アクティベーションワークフローのマッピング手順で、重複する値をソースフィールドに追加できますが、ターゲットフィールドには追加できません。 許可されているマッピングと禁止されているマッピングの組み合わせの例については、以下の表を参照してください。 <br><table><thead><tr><th>許可/禁止</th><th>ソースフィールド</th><th>ターゲットフィールド</th></tr></thead><tbody><tr><td>許可</td><td><ul><li>email.address</li><li>email.address</li></ul></td><td><ul><li>emailalias1</li><li>メールのエイリアス 2</li></ul></td></tr><tr><td>Forbidden</td><td><ul><li>email.address</li><li>hashed.emails</li></ul></td><td><ul><li>emailalias1</li><li>emailalias1</li></ul></td></tr></tbody></table> </li></ul></td>
    </tr>    
</table>

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| スキーマツリーの表示名の改善 | 以前は、フィールド名は UI に表示されていましたが、現在は、スキーマキャンバス上のスキーマフィールドの表示名が読みやすくなっています。 |

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
| セグメントビルダーでの一括値インポート | セグメントビルダーで、CSV または TSV ファイルをアップロードするか、コンマ区切りの値を手動で挿入することで、複数の値のインポートがサポートされるようになりました。 詳しくは、 [セグメントビルダーガイド](../../segmentation/ui/segment-builder.md#rule-builder-canvas). |
| 外部オーディエンスのメンバーシップの有効期限 | デフォルトでは、外部オーディエンスのメンバーシップは 30 日間保持されます。 長期間保持するには、 `validUntil` フィールドに値を入力します。 |
| Platform が生成したセグメントメンバーシップの有効期限 | 次に属する任意のセグメントメンバーシップ `Exited` ～に基づいて 30 日以上の州 `lastQualificationTime` フィールドは削除されます。 |

{style=&quot;table-layout:auto&quot;}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platformは、外部ソースからデータを取り込むことができ、Platform Services を使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。 アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

| 機能 | 説明 |
| --- | --- |
| クラウドストレージソースのサブフォルダーへのユーザーアクセスを許可する | 新しいアカウントを作成する際に、クラウドストレージソースの特定のサブフォルダーへのアクセスを定義できるようになりました。 作成したユーザーは、許可されたサブフォルダーのデータにのみアクセスできます。 この機能は、次のクラウドストレージソースで使用できます。 [Azure Blob ストレージ](../../sources/connectors/cloud-storage/blob.md), [Google Cloud Storage](../../sources/connectors/cloud-storage/google-cloud-storage.md), [Google PubSub](../../sources/connectors/cloud-storage/google-pubsub.md)、および [SFTP](../../sources/connectors/cloud-storage/sftp.md). |
| ベータ版の可用性 [!DNL SugarCRM] | [!DNL SugarCRM] ソースは、ベータ版で利用できるようになりました。 以下を使用： [!DNL SugarCRM Accounts & Contacts] そして [!DNL SugarCRM Events] ソースからデータを取り込む [!DNL SugarCRM] アカウントからExperience Platformへ。 詳しくは、 [[!DNL SugarCRM] 概要](../../sources/connectors/crm/sugarcrm.md). |