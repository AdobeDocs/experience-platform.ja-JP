---
title: Adobe Experience Platform リリースノート（2023年1月）
description: Adobe Experience Platform の 2023年1月のリリースノートです。
exl-id: 461898ce-5683-4ab1-9167-ac25843a1ff8
source-git-commit: 05a7b73da610a30119b4719ae6b6d85f93cdc2ae
workflow-type: tm+mt
source-wordcount: '2414'
ht-degree: 100%

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

## 人工知能／機械学習サービス {#ai-ml}

人工知能と機械学習サービスは、マーケティングアナリストや実務担当者に対して、顧客体験のユースケースで AI／ML の機能を活用する機能を提供します。これにより、マーケティングアナリストは、データサイエンスの専門知識がなくても、ビジネスレベルの設定を使用して、会社のニーズに固有のモデルを設定できます。

### アトリビューション AI

アトリビューション AI は、コンバージョンイベントにつながるタッチポイントの貢献度を明らかにするために使用します。カスタマージャーニーをまたいだ個別マーケティングタッチポイントのマーケティング効果を、マーケターが定量化する際に役立ちます。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| HIPAA 対応 | Healthcare Shield のお客様は、アトリビューション AI およびその他の特定の Experience Platform ベースのアプリケーションで、保護された医療情報を受信、使用、保守または送信できるようになりました。Healthcare Shield は、HIPAA の対象事業者またはビジネスアソシエイトである医療関係のお客様向けです。詳しくは、[HIPAA およびアドビ製品とサービス](https://www.adobe.com/trust/compliance/hipaa-ready.html)のドキュメントを参照してください |
| 追加のスコアデータセット列の編集 | 既存のモデルを編集する際に、追加のスコアデータセット列（レポート列）を追加または削除できるようになりました。これにより、アトリビューションスコアの柔軟性が拡張され、モデルが既に作成された後で、追加のディメンションに対するインサイトが得られます。詳しくは、[アトリビューション UI ガイド](../../intelligent-services/attribution-ai/user-guide.md)を参照してください。 |

{style="table-layout:auto"}

詳しくは、[AI／ML サービス](../../intelligent-services/attribution-ai/overview.md)の概要を参照してください。

### 顧客 AI

Real-Time Customer Data Platform の顧客 AI は、個々のプロファイルのチャーンやコンバージョンなどのカスタム傾向スコアを大規模に生成するために使用します。これを実現するために、ビジネスニーズを機械学習の問題に変換したり、アルゴリズムを選択したり、トレーニング、またはデプロイする必要はありません。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| HIPAA 対応 | Healthcare Shield のお客様は、Real-Time Customer Data Platform およびその他の特定の Experience Platform ベースのアプリケーションの顧客 AI で、保護された医療情報を受信、使用、保守または送信できるようになりました。 Healthcare Shield は、HIPAA の対象事業者またはビジネスアソシエイトである医療関係のお客様向けです。詳しくは、[HIPAA およびアドビ製品とサービス](https://www.adobe.com/trust/compliance/hipaa-ready.html)のドキュメントを参照してください |

{style="table-layout:auto"}

詳しくは、[AI／ML サービス](../../intelligent-services/customer-ai/overview.md)の概要を参照してください。

## Assurance {#assurance}

Adobe Assurance を使用すると、モバイルアプリでデータを収集したりエクスペリエンスを提供したりする方法を検査、配達確認、シミュレートおよび検証できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 検証エディター | 検証エディターに新しい機能強化が追加されました。 これらの機能強化には、検証列、新しいコード作成ツール、改善された表示が含まれます。 |

{style="table-layout:auto"}

Assurance について詳しくは、[Assurance のドキュメント](https://developer.adobe.com/client-sdks/documentation/platform-assurance/)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しいホーム画面 | データ収集 UI のホームページが更新され、生産性を向上させるための便利なオンボーディング情報やリンクが含まれるようになりました。 これには以下が含まれます。<ol><li>基本を学ぶためのドキュメントと推奨されるワークフロー</li><li>最近のプロパティ、ルールおよびデータ要素</li><li>人気の高い拡張機能</li><li>クイックインストール機能を備えた新しい拡張機能のアップデート</li></ol> |
| イベント転送を使用した [!DNL Google Ads] へのデータの送信 | これで、イベント転送用の [[!DNL Google Ads Enhanced Conversions] API 拡張機能](../../tags/extensions/server/google-ads-enhanced-conversions/overview.md)を [Google Oauth 2 シークレット](../../tags/ui/event-forwarding/secrets.md#google-oauth2)と組み合わせて使用し、サーバーサイドのデータをリアルタイムで安全に [!DNL Google Ads] へと送信できるようになりました。 |

{style="table-layout:auto"}

## 宛先（更新日：2月2日（PT）） {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [（ベータ版）Adobe Experience Cloud Audiences 接続](../../destinations/catalog/adobe/experience-cloud-audiences.md) | [!UICONTROL （ベータ版）Adobe Experience Cloud Audiences 接続]を使用して、Experience Platform から様々な Experience Platform ソリューション（Audience Manager、Analytics、Advertising Cloud、Adobe Campaign、Target、Marketo など）にセグメントを共有します。 |
| [Pega プロファイル接続](../../destinations/catalog/personalization/pega-profile.md) | Adobe Experience Platform の [!DNL Pega Profile Connector] を使用して [!DNL Amazon] S3 ストレージへのライブアウトバウンド接続を作成し、Adobe Experience Platform から CSV ファイルにプロファイルデータを定期的に書き出して、独自の S3 バケットに入れます。[!DNL Pega Customer Decision Hub] では、データジョブをスケジュールして、このプロファイルデータを S3 ストレージから読み込み、[!DNL Pega Customer Decision Hub] プロファイルを更新できます。 |
| [（ベータ版）The Trade Desk CRM EU 接続](../../destinations/catalog/advertising/tradedesk-emails.md) | EUID（European Unified ID）のリリースにより、[宛先カタログ](/help/destinations/catalog/overview.md)に 2 つの [!DNL The Trade Desk - CRM] 宛先が表示されるようになりました。 <ul><li> EU でデータをソースにする場合は、**[!DNL The Trade Desk - CRM (EU)]** の宛先を使用してください。</li><li> APAC または NAMER 地域でデータをソースにする場合は、**[!DNL The Trade Desk - CRM (NAMER & APAC)]** の宛先を使用してください。 </li></ul> |

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| ストリーミング宛先との統合に関する有料メディア同意ポリシーの機能強化 | 有料メディアアクティベーションユースケースの[ストリーミング宛先](/help/destinations/destination-types.md#streaming-destinations)での[同意ポリシーの適用に対する機能強化](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-enhancement)。プロファイルが同意ポリシーの対象として認定されなくなった場合、Experience Platform はポリシーの終了とストリーミング宛先との間で積極的に通信するようになりました。<br> <b>メモ</b>：この機能は、**[!UICONTROL Privacy and Security Shield]** および **[!UICONTROL Healthcare Shield]** のお客様のみが使用できます。 |
| ベータ版クラウドストレージの宛先コネクタの新しい区切り文字オプション | 3 つの新しい区切り文字オプション（コロン `:`、パイプ、セミコロン `;`）が、新しいベータ版クラウドストレージの宛先（[（ベータ版）Amazon S3](/help/destinations/catalog/cloud-storage/amazon-s3.md)、[（ベータ版）Azure Blob](/help/destinations/catalog/cloud-storage/azure-blob.md)、[（ベータ版）Azure Data Lake Storage Gen2](/help/destinations/catalog/cloud-storage/adls-gen2.md)、[（ベータ版）データランディングゾーン](/help/destinations/catalog/cloud-storage/data-landing-zone.md)、[（ベータ版）Google Cloud Storage](/help/destinations/catalog/cloud-storage/google-cloud-storage.md)、[（ベータ版）SFTP](/help/destinations/catalog/cloud-storage/sftp.md)）で使用できるようになりました。<br> ファイルベースの宛先については、サポート対象の[ファイル形式オプション](/help/destinations/ui/batch-destinations-file-formatting-options.md)を参照してください。 |
| [Destination SDK](/help/destinations/destination-sdk/overview.md) の[顧客データフィールド](/help/destinations/destination-sdk/destination-configuration.md#customer-data-fields)設定で使用できる新しいオプションパラメーター | `unique`：ユーザーの組織によって設定されたすべての宛先データフローで値が一意である必要がある顧客データフィールドを作成する必要がある場合は、このパラメーターを使用します。<br> 例えば、[[!UICONTROL カスタムパーソナライゼーション]](/help/destinations/catalog/personalization/custom-personalization.md#parameters)宛先の「**[!UICONTROL 統合エイリアス]**」フィールドは一意である必要があります。つまり、この宛先への 2 つの個別のデータフローがこのフィールドに同じ値を持つことはできません。 |

**修正および機能強化** {#destinations-fixes-and-enhancements}

<table>
    <tr>
        <td><b>修正または機能強化</b></td>
        <td><b>説明</b></td>
    </tr>
    <tr>
        <td>ファイルベースの宛先への書き出し動作の更新（PLAT-123316）</td>
        <td>データファイルをバッチ宛先に書き出す際の<a href="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html?lang=ja#mandatory-attributes">必須属性</a>の動作の問題を修正しました。<br> 以前は、出力ファイルのすべてのレコードに次の両方が含まれていることが確認されていました。 <ol><li><code>mandatoryField</code> 列の null 以外の値および</li><li>他の非必須フィールドの少なくとも 1 つに null 以外の値。</li></ol> 2 番目の条件が削除されました。その結果、次の例に示すように、書き出されたデータファイルでより多くの出力行が表示される場合があります。<br> <b> 2023年1月リリースより前のサンプル動作 </b> <br> 必須フィールド：<code>emailAddress</code> <br> <b>アクティブ化する入力データ</b> <br><table><thead><tr><th>firstName</th><th>メールアドレス</th></tr></thead><tbody><tr><td>John</td><td>john@acme.com</td></tr><tr><td>null</td><td>peter@acme.com</td></tr><tr><td>Jenifer</td><td>jennifer@acme.com</td></tr><tr><td>null</td><td>diana@acme.com</td></tr></tbody></table> <br> <b>アクティベーション出力</b> <br><table><thead><tr><th>firstName</th><th>メールアドレス</th></tr></thead><tbody><tr><td>John</td><td>john@acme.com</td></tr><tr><td>Jenifer</td><td>jennifer@acme.com</td></tr></tbody></table> <br> <b> 2023年1月リリース以降のサンプル動作 </b> <br> <b>アクティベーション出力</b> <br> <table><thead><tr><th>firstName</th><th>メールアドレス</th></tr></thead><tbody><tr><td>John</td><td>john@acme.com</td></tr><tr><td>null</td><td>peter@acme.com</td></tr><tr><td>Jenifer</td><td>jennifer@acme.com</td></tr><tr><td>null</td><td>diana@acme.com</td></tr></tbody></table> </td>
    </tr>
    <tr>
        <td>必要なマッピングおよび重複マッピングの UI および API 検証（PLAT-123316）</td>
        <td>宛先のアクティブ化ワークフローで<a href="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html?lang=ja#mapping">フィールドをマッピング</a>する際に、UI および API で次のように検証が実施されるようになりました。<ul><li><b>必要なマッピング</b>：宛先開発者が必要なマッピングを使用して宛先を設定した場合（<a href="https://experienceleague.adobe.com/docs/experience-platform/destinations/catalog/advertising/google-ad-manager-360-connection.html?lang=ja">Google アドマネージャー 360</a> の宛先など）、データを宛先にアクティブ化するときに、これらの必要なマッピングをユーザーが追加する必要があります。 </li><li><b>重複したマッピング</b>：アクティベーションワークフローのマッピング手順では、ソースフィールドに重複する値を追加できますが、ターゲットフィールドには追加できません。許可されているマッピングと禁止されているマッピングの組み合わせの例については、以下の表を参照してください。 <br><table><thead><tr><th>許可／禁止されています</th><th>ソースフィールド</th><th>ターゲットフィールド</th></tr></thead><tbody><tr><td>許可</td><td><ul><li>email.address</li><li>email.address</li></ul></td><td><ul><li>emailalias1</li><li>email alias2</li></ul></td></tr><tr><td>禁止されています</td><td><ul><li>email.address</li><li>hashed.emails</li></ul></td><td><ul><li>emailalias1</li><li>emailalias1</li></ul></td></tr></tbody></table> </li></ul></td>
    </tr>    
</table>

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライゼーションのために顧客属性を使用したりできます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| スキーマツリーの表示名の改善 | 以前はフィールド名が UI に表示されていましたが、スキーマキャンバス上のスキーマフィールドの表示名がより読みやすくなりました。 |

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| クラス | [[!UICONTROL コンバージョン]](https://github.com/adobe/xdm/blob/master/components/classes/conversion.schema.json) | 通貨コンバージョンなどのコンバージョンデータを追跡するためのクラス。 |
| フィールドグループ | [[!UICONTROL 通貨コンバージョンレートの詳細]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/conversion/currency-conversion-details.schema.json) | 通貨コンバージョンに関連する追加の詳細を取得する、[!UICONTROL コンバージョン]クラスのフィールドグループ。 |
| フィールドグループ | [[!UICONTROL メタデータを使用した同意ポリシーの評価結果のマップ]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-consentResultsv2.schema.json) | 同意ポリシーのエントリと存在に関するメタデータ情報など、複数の同意ポリシーの評価結果に関する詳細の取得。 |

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| データタイプ | [[!UICONTROL 広告の詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/advertisingdetails.schema.json) | `ID` フィールドの名前を `name` に変更し、以前の `name` フィールドは `friendlyName` になりました。 |
| データタイプ | [[!UICONTROL 決定提案の詳細]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/decisioning/proposition-detail.schema.json) | 選択戦略の詳細を取得する `selectionStrategy` フィールドを追加しました。 |
| フィールドグループ | [[!UICONTROL エクスペリエンスイベント - 提案インタラクション]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/decisioning/experienceevent-proposition-interaction.schema.json) | フィールドグループは、[!UICONTROL Journey Step Event] クラスと互換性を持つようになりました。 |
| データタイプ | [[!UICONTROL エラーの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/errordetails.schema.json) | `ID` フィールドの名前を `name` に変更しました。 |
| データタイプ | [[!UICONTROL メディア情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/media.schema.json) | パターンの変更をビデオセグメントプロパティに戻しました。 |
| データタイプ | [[!UICONTROL QoE データの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/qoedatadetails.schema.json) | `droppedFrameCount` フィールドを削除しました。 |
| データタイプ | [[!UICONTROL セッションの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/sessiondetails.schema.json) | `isAuthorized` フィールドの名前を `authorized` に変更し、以前はブール値だった `type` を文字列に更新しました。 |
| データタイプ | [[!UICONTROL 送料]](https://github.com/adobe/xdm/blob/master/components/datatypes/shipping.schema.json) | 新しいフィールド `shipDate`、`trackingNumber`、`trackingURL` を追加しました。 |
| フィールドグループ | [[!UICONTROL AJO エンティティフィールド]](https://github.com/adobe/xdm/blob/master/extensions/adobe/experience/customerJourneyManagement/ajo-entity-mixins.schema.json) | 新しいフィールド `journeyNodeID`、`journeyNodeName`、`journeyModeType` を追加しました。 |
| フィールドグループ | [[!UICONTROL 消費者エクスペリエンスイベント]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/experienceevent-consumer.schema.json) | フィールドグループは、[!UICONTROL Summary Metrics] クラスとも互換性を持つようになりました。 |
| フィールドグループ | [[!UICONTROL 製品トリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/product-triggers.schema.json) | `productTriggers` フィールドを `weather` オブジェクトの下にネストしました。 |
| フィールドグループ | [[!UICONTROL 相対トリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/relative-triggers.schema.json) | `relativeTriggers` フィールドを `weather` オブジェクトの下にネストしました。 |
| フィールドグループ | [[!UICONTROL 重大なトリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/severe-triggers.schema.json) | `severeTriggers` フィールドを `weather` オブジェクトの下にネストしました。 |
| フィールドグループ | [[!UICONTROL 気象トリガー]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/shared/severe-triggers.schema.json) | `weatherTriggers` フィールドを `weather` オブジェクトの下にネストしました。 |
| フィールドグループ | [[!UICONTROL XDM 関連ビジネスアカウント]](https://github.com/adobe/xdm/blob/master/components/fieldgroups/account/related-accounts.schema.json) | フィールドグループが安定しました。 |

{style="table-layout:auto"}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

**今後非推奨（廃止）となる予定** {#deprecation}

セグメントメンバーシップライフサイクルの冗長性を取り除くために、`Existing` ステータスは 2023年3月末に[セグメントメンバーシップマップ](../../xdm/field-groups/profile/segmentation.md)から廃止される予定です。フォローアップのお知らせには、正確な非推奨（廃止予定）日が含まれます。

廃止後、セグメントで認定されたプロファイルは `Realized` として表され、不適格となったプロファイルは引き続き `Exited` として表されます。これにより、`Active` および `Expired` のセグメントステータスを持つファイルベースの送信先と同等になります。

この変更は、[企業の宛先](../../destinations/destination-types.md#streaming-profile-export)（Amazon Kinesis、Azure Event Hubs、HTTP API）を使用していて、`Existing` ステータスに基づいて自動化されたダウンストリームプロセスを導入している場合に影響を与える可能性があります。このような場合は、ダウンストリームの統合を確認してください。特定の時間を超えて新たに認定されたプロファイルを識別することに関心がある場合は、セグメントメンバーシップマップで `Realized` ステータスと `lastQualificationTime` を組み合わせて使用することを検討してください。詳しくは、アドビ担当者にお問い合わせください。

プロファイルデータを操作するためのチュートリアルやベストプラクティスなど、リアルタイム顧客プロファイルについて詳しくは、[リアルタイム顧客プロファイルの概要](../../profile/home.md)を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| セグメントビルダーでの値の一括読み込み | セグメントビルダーで、CSV または TSV ファイルをアップロードするか、コンマ区切りの値を手動で挿入することにより、複数の値の読み込みをサポートするようになりました。詳しくは、[セグメントビルダーガイド](../../segmentation/ui/segment-builder.md#rule-builder-canvas)を参照してください。 |
| 外部オーディエンスのメンバーシップの有効期限 | デフォルトでは、外部オーディエンスのメンバーシップは 30 日間保持されます。これよりも長期間保持するには、オーディエンスデータの取り込み中に `validUntil` フィールドを使用します。 |
| Platform が生成したセグメントメンバーシップの有効期限 | `lastQualificationTime` フィールドに基づき、30 日を超えて `Exited` 状態にあるセグメントメンバーシップは、削除の対象となります。 |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むことができ、Platform サービスを使用してそのデータの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

| 機能 | 説明 |
| --- | --- |
| クラウドストレージソースのサブフォルダーへのユーザーアクセスの許可 | 新しいアカウントを作成する際に、クラウドストレージソースの特定のサブフォルダーへのアクセスを定義できるようになりました。作成したユーザーは、許可されたサブフォルダーのデータにのみアクセスできます。この機能は、[Azure Blob Storage](../../sources/connectors/cloud-storage/blob.md)、[Google Cloud Storage](../../sources/connectors/cloud-storage/google-cloud-storage.md)、[Google PubSub](../../sources/connectors/cloud-storage/google-pubsub.md)、[SFTP](../../sources/connectors/cloud-storage/sftp.md) のクラウドストレージソースで使用できます。 |
| [!DNL SugarCRM] のベータ版の可用性 | [!DNL SugarCRM] ソースを、ベータ版で利用できるようになりました。[!DNL SugarCRM Accounts & Contacts] および [!DNL SugarCRM Events] ソースを使用して、[!DNL SugarCRM] アカウントから Experience Platform にデータを取り込みます。詳しくは、[[!DNL SugarCRM] 概要](../../sources/connectors/crm/sugarcrm.md)を参照してください。 |
