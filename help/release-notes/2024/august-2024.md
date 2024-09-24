---
title: Adobe Experience Platform リリースノート 2024年8月
description: Adobe Experience Platform の 2024年8月のリリースノート。
exl-id: 153891e9-fd82-4894-a047-c8d82f214fef
source-git-commit: 4fecb47084a522b4eb9808dc317e0d70e7ef42c6
workflow-type: ht
source-wordcount: '1562'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2024年8月20日（PT）**

>[!TIP]
>
>組織が Real-time CDP を使用して実現できる見込み客の調査、獲得などの様々なユースケースについて詳しくは、[サンプルユースケースの概要ドキュメント](https://experienceleague.adobe.com/ja/docs/experience-platform/rtcdp/use-cases/overview)を参照してください。

Experience Platform の既存の機能とドキュメントに対するアップデート：

- [属性ベースのアクセス制御](#abac)
- [データ取り込み](#data-ingestion)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [ID サービス](#identity-service)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## 属性ベースのアクセス制御 {#abac}

属性ベースのアクセス制御は、プライバシーを重視するブランドが、ユーザーアクセスをより柔軟に管理できるようにする、Adobe Experience Platform の機能です。ユーザーの役割に、スキーマフィールドやセグメントなどの個々のオブジェクトを割り当てることができます。 この機能を使用すると、組織内の特定の Platform ユーザーに対する個々のオブジェクトへのアクセスを許可または取り消すことができます。

属性ベースのアクセス制御により、組織の管理者は、すべてのプラットフォームワークフローとリソースにわたって、機密性の高い個人データ（SPD）、個人を特定できる情報（PII）、およびその他のカスタマイズされた種類のデータへのユーザーのアクセスを制御できます。管理者は、特定のフィールドと、それらのフィールドに対応するデータにのみアクセスできるユーザーの役割を定義できます。

**新機能**

| 機能アップデート | 説明 |
| --- | --- |
| 権限マネージャーの新機能 | [権限マネージャー](../../access-control/abac/permission-manager/overview.md)を使用して、簡単なクエリでレポートを生成できるようになりました。これにより、アクセス管理を理解し、複数のワークフローと精度レベルにまたがるアクセス権限の検証にかかる時間を節約できます。ユーザーと役割のレポート作成について詳しくは、[権限マネージャーユーザーガイド](../../access-control/abac/permission-manager/permissions.md)を参照してください。![左側のナビゲーションで権限マネージャーがハイライト表示された Experience Platform ユーザーインターフェイスの画像。](assets/august/permission-manager-rn.png "ユーザーインターフェイスの権限マネージャー。"){width="250" align="center" zoomable="yes"} |

{style="table-layout:auto"}

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](../../access-control/abac/overview.md)を参照してください。属性ベースのアクセス制御ワークフローの包括的なガイドについては、[属性ベースのアクセス制御エンドツーエンドガイド](../../access-control/abac/end-to-end-guide.md)を参照してください。

## データ取得（更新日：8月23日（PT）） {#data-ingestion}

Adobe Experience Platform は、あらゆるタイプ、あらゆる待ち時間のデータを取り込める、豊富な機能セットを提供します。取得は、Batch API または Streaming API、アドビの組み込みソース、データ統合パートナー、Adobe Experience Platform UI を使用して行うことができます。

**バッチデータ取り込みにおける日付形式の処理の更新**

このリリースでは、バッチデータ取り込みにおける&#x200B;*日付形式の処理*&#x200B;に関する問題に対処しています。以前は、クライアントによって `Date` として挿入された日付フィールドが、`DateTime` 形式に変換されていました。つまり、タイムゾーンがフィールドに自動的に追加され、`Date` 形式を好む、または必要とするユーザーにとって困難が生じていました。今後、`Date` タイプのフィールドにタイムゾーンは自動的に追加されなくなります。この更新により、書き出されたデータの形式が、顧客のリクエストに応じて、そのフィールドのプロファイルで表される形式と一致するようになります。

リリース前の `Date` フィールド：`"birthDate": "2018-01-12T00:00:00Z"`
リリース後の `Date` フィールド：`"birthDate": "2018-01-12"`

詳しくは、[バッチ取り込み](/help/ingestion/batch-ingestion/overview.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先** {#new-updated-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| [Braze](/help/destinations/catalog/mobile-engagement/braze.md) | [!UICONTROL Braze] では、ダッシュボードと REST エンドポイントの多数の異なるインスタンスを管理します。[!UICONTROL Braze] のお客様は、プロビジョニング先のインスタンスに基づいて、正しい REST エンドポイントを使用する必要があります。このリリースでは、[!UICONTROL Braze] に接続する際に選択できる新しい US-07 エンドポイントが追加されました。 |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| オンデマンドでのファイルのバッチ宛先への書き出しが一般提供されるようになりました。 | オンデマンドでのファイルのバッチ宛先への書き出しを行うオプションを、すべてのお客様が使用できるようになりました。詳しくは、[専用のドキュメント](../../destinations/ui/export-file-now.md)を参照してください。 |
| [スケジュール設定ステップ](../../destinations/ui/activate-batch-profile-destinations.md#scheduling)で、書き出された複数のオーディエンスの書き出しスケジュールを編集します。 | オーディエンスアクティベーションワークフローのスケジュール設定ステップから複数の書き出されたオーディエンスの書き出しスケジュールを直接編集するオプションを、すべてのお客様が使用できるようになりました。![スケジュール設定ステップの「スケジュールを編集」オプションがハイライト表示された Experience Platform ユーザーインターフェイスの画像。](assets/august/edit-schedule.png "スケジュール設定ステップの「スケジュールを編集」オプション。"){width="250" align="center" zoomable="yes"} |
| [スケジュール設定ステップ](../../destinations/ui/activate-batch-profile-destinations.md#scheduling)で、書き出された複数のオーディエンスのファイル名を編集します。 | オーディエンスアクティベーションワークフローのスケジュール設定ステップから複数の書き出されたファイルの名前を直接編集するオプションを、すべてのお客様が使用できるようになりました。![スケジュール設定ステップの「ファイル名を編集」オプションがハイライト表示された Experience Platform ユーザーインターフェイスの画像。](assets/august/edit-file-name.png "スケジュール設定ステップの「ファイル名を編集」オプション。"){width="250" align="center" zoomable="yes"} |
| [宛先の詳細](../../destinations/ui/destination-details-page.md#bulk-remove)ページで、データフローから複数のオーディエンスを削除します。 | **[!UICONTROL 宛先の詳細]**&#x200B;ページで、既存のデータフローから複数のオーディエンスを削除するオプションを、すべてのお客様が使用できるようになりました。![宛先の詳細ページの「オーディエンスを削除」オプションがハイライト表示された Experience Platform ユーザーインターフェイスの画像。](assets/august/bulk-remove-audiences.png "宛先の詳細ページの「オーディエンスを削除」オプション。"){width="250" align="center" zoomable="yes"} |
| [宛先の詳細](../../destinations/ui/destination-details-page.md#bulk-export)ページで、複数のファイルをオンデマンドでバッチ宛先に書き出します。 | **[!UICONTROL 宛先の詳細]**&#x200B;ページで、複数のファイルをオンデマンドでバッチ宛先に書き出すオプションを、すべてのお客様が使用できるようになりました。![宛先の詳細ページの「ファイルを今すぐ書き出し」オプションがハイライト表示された Experience Platform ユーザーインターフェイスの画像。](assets/august/bulk-export-file-now.png "宛先の詳細ページの「ファイルを今すぐ書き出し」オプション。"){width="250" align="center" zoomable="yes"} |
| [宛先の詳細](../../destinations/ui/destination-details-page.md#bulk-edit-file-names)ページで、書き出された複数のオーディエンスのファイル名を編集します。 | **[!UICONTROL 宛先の詳細]**&#x200B;ページで、書き出された複数のファイルの名前を直接編集できるようになりました。![宛先の詳細ページの「ファイル名を編集」オプションがハイライト表示された Experience Platform ユーザーインターフェイスの画像。](assets/august/edit-file-name-destination-details.png "宛先の詳細ページの「ファイル名を編集」オプション。"){width="250" align="center" zoomable="yes"} |
| [宛先の詳細](../../destinations/ui/export-datasets.md#remove-dataset)ページで、データフローから複数のデータセットを削除します。 | データフローから複数のデータセットを削除するオプションを、すべてのお客様が使用できるようになりました。![宛先の詳細ページの「データセットを削除」オプションがハイライト表示された、Experience Platform ユーザーインターフェイスの画像。](assets/august/bulk-remove-datasets.png "宛先の詳細ページの「データセットを削除」オプション。"){width="250" align="center" zoomable="yes"} |

{style="table-layout:auto"}

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| ML 支援型スキーマ作成フロー | 高度な機械学習アルゴリズムを使用して、サンプルデータファイルを分析し、標準フィールドとカスタムフィールドを使用して最適化されたスキーマを自動的に作成します。<br>主な機能：<br><ul><li>迅速なスキーマ作成：ML 推奨および生成された XDM フィールドを使用して、サンプルデータファイルからスキーマを直接生成します。</li><li>スキーマが柔軟に進化：生成されたスキーマ内のフィールドを、容易に追加または更新します。</li><li>シームレスな統合：スキーマ Ul のコアスキーマ作成フローと完全に統合され、スムーズで一貫性のあるユーザーエクスペリエンスを確保します。</li><li>効率的なレビューと編集：フラットビューエディターを使用してスキーマをすばやく表示および更新できるので、作成プロセスがより効率的で使いやすくなります。</li></ul><br>詳しくは、[ML 支援型スキーマ作成ワークフローガイド](../../xdm/ui/ml-assisted-schema-creation.md)を参照してください。 |

{style="table-layout:auto"}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを使用すると、デバイスやシステム間で ID を紐付けることで、顧客とその行動の全体像を把握し、インパクトのある、個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**更新されたドキュメント**

| 機能 | 説明 |
| --- | --- |
| グラフ設定ガイド | ID グラフのリンクルールと ID データの操作中に発生する可能性のある、一般的なグラフシナリオについて詳しくは、[グラフ設定ガイド](../../identity-service/identity-graph-linking-rules/example-configurations.md)を参照してください。グラフ設定ガイドでは、単一ユーザーのシンプルなグラフシナリオから、複雑で階層的な複数ユーザーのグラフシナリオまで、様々な例について説明しています。また、このガイドでは、[グラフシミュレーション UI](../../identity-service/identity-graph-linking-rules/graph-simulation.md) に入力できるイベントやアルゴリズム設定の例と、特定のグラフシナリオでプライマリ ID が選択される方法の分類についても説明しています。 |

{style="table-layout:auto"}

ID サービスについて詳しくは、[ID サービスの概要](../../identity-service/home.md)を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 取り込みの詳細 | カスタムのアップロード元を使用するオーディエンスの場合、オーディエンスの詳細ページ内でオーディエンスの取り込みの詳細をより包括的に表示できます。また、スキーマを選択し、ラベル付けする目的の属性を選択することで、ペイロード属性にラベルを適用できます。取り込みの詳細セクションについて詳しくは、[オーディエンスポータルガイド](../../segmentation/ui/audience-portal.md#ingestion-details)を参照してください。 |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

Experience Platform のソースを使用して、Adobe アプリケーションまたはサードパーティのデータソースからデータを取り込みます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Analytics ソースコネクタに対するアップデート | Analytics ソースコネクタはアドビが完全に管理しているので、データセットアクティビティページには、バッチに関する情報が表示されません。取り込まれたレコードに関する指標を確認することで、データのフローを監視できます。詳しくは、[Analytics データのソース接続](../../sources/tutorials/ui/create/adobe-applications/analytics.md)の作成に関するガイドを参照してください。 |

**更新されたドキュメント**

| 更新されたドキュメント | 説明 |
| --- | --- |
| データフローの更新に関するドキュメントを拡張 | [UI での既存のソースデータフローの更新](../../sources/tutorials/ui/update-dataflows.md)に関するガイドが更新され、既存のデータフローに対して実行できる様々な設定に関する詳細情報が提供されるようになりました。また、ガイドは、無効になったデータフローが再度有効になる場合の予想される動作を明確にするためにも更新されました。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
