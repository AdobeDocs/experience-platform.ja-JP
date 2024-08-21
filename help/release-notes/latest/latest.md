---
title: Adobe Experience Platform リリースノート 2024年8月
description: Adobe Experience Platform の 2024年8月のリリースノート。
source-git-commit: d01e16938485f6648cc02ce1674e0e9e84d78147
workflow-type: tm+mt
source-wordcount: '1300'
ht-degree: 29%

---

# Adobe Experience Platform リリースノート

**リリース日：2024年8月20日（PT）**

>[!TIP]
>
>[ サンプルユースケースの概要ドキュメント ](https://experienceleague.adobe.com/en/docs/experience-platform/rtcdp/use-cases/overview) を参照して、Real-Time CDPで実現できる見込み調査や獲得などの様々なユースケースについて確認します。

Experience Platformの既存の機能およびドキュメントのアップデート：

- [属性ベースのアクセス制御](#abac)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [ID サービス](#identity-service)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## 属性ベースのアクセス制御 {#abac}

属性ベースのアクセス制御は、プライバシーを重視するブランドが、ユーザーアクセスをより柔軟に管理できるようにする、Adobe Experience Platformの機能です。 ユーザーの役割に、スキーマフィールドやセグメントなどの個々のオブジェクトを割り当てることができます。 この機能を使用すると、組織内の特定の Platform ユーザーに対する個々のオブジェクトへのアクセスを許可または取り消すことができます。

属性ベースのアクセス制御により、組織の管理者は、すべての Platform ワークフローとリソースにわたって、機密性の高い個人データ（SPD）、個人を特定できる情報（PII）、およびその他のカスタマイズされた種類のデータへのユーザーのアクセスを制御できます。 管理者は、特定のフィールドと、それらのフィールドに対応するデータにのみアクセスできるユーザーの役割を定義できます。

**新機能**

| 機能の更新 | 説明 |
| --- | --- |
| 権限マネージャーの新機能 | [ 権限マネージャー ](../../access-control/abac/permission-manager/overview.md) を使用して、簡単なクエリでレポートを生成できるようになりました。これにより、アクセス管理を理解し、複数のワークフローと精度レベルでアクセス権限を確認する時間を節約できます。 ユーザーおよびロールのレポート作成の詳細については、「[ 権限マネージャーユーザーガイド ](../../access-control/abac/permission-manager/permissions.md)」を参照してください。 ![ 左側のナビゲーションで Permission Manager をハイライト表示した画像Experience Platformーユーザーインターフェイス。](../2024/assets/august/permission-manager-rn.png " ユーザーインターフェイスの権限マネージャー。"){width="250" align="center" zoomable="yes"} |

{style="table-layout:auto"}

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](../../access-control/abac/overview.md)を参照してください。属性ベースのアクセス制御ワークフローの包括的なガイドについては、[属性ベースのアクセス制御エンドツーエンドガイド](../../access-control/abac/end-to-end-guide.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ----------- | ----------- |
| オンデマンドでのファイルのバッチ宛先への書き出しが利用できるようになりました。 | オンデマンドでファイルをバッチ宛先に書き出すオプションを、すべてのお客様が使用できるようになりました。 詳しくは、[ 専用ドキュメント ](../../destinations/ui/export-file-now.md) を参照してください。 |
| [ スケジュール設定ステップ ](../../destinations/ui/activate-batch-profile-destinations.md#scheduling) で、書き出された複数のオーディエンスの書き出しスケジュールを編集します。 | Audience Activation ワークフローのスケジューリング手順から直接複数の書き出しオーディエンスの書き出しスケジュールを編集するオプションを、すべてのお客様が使用できるようになりました。 ![ スケジュール設定手順で「スケジュールを編集」オプションを強調表示したExperience Platformユーザーインターフェイスの画像。](../2024/assets/august/edit-schedule.png " スケジュール設定ステップの「スケジュールを編集」オプション "){width="250" align="center" zoomable="yes"} |
| [ スケジュール設定手順 ](../../destinations/ui/activate-batch-profile-destinations.md#scheduling) で、書き出された複数のオーディエンスのファイル名を編集します。 | Audience Activation ワークフローのスケジューリング手順から直接書き出された複数のファイルの名前を編集するオプションが、すべてのお客様が使用できるようになりました。 ![ スケジュール設定手順で「ファイル名を編集」オプションをハイライト表示したExperience Platformユーザーインターフェイスの画像。](../2024/assets/august/edit-file-name.png " スケジュール設定手順の「ファイル名を編集」オプション "){width="250" align="center" zoomable="yes"}。 |
| [ 宛先の詳細 ](../../destinations/ui/destination-details-page.md#bulk-remove) ページで、データフローから複数のオーディエンスを削除します。 | **[!UICONTROL 宛先の詳細]** ページの既存のデータフローから複数のオーディエンスを削除するオプションが、すべてのお客様が利用できるようになりました。 ![ 宛先の詳細ページの「オーディエンスを削除」オプションをハイライト表示したExperience Platformユーザーインターフェイスの画像。](../2024/assets/august/bulk-remove-audiences.png " 宛先の詳細ページの「オーディエンスを削除」オプション "){width="250" align="center" zoomable="yes"} |
| [ 宛先の詳細 ](../../destinations/ui/destination-details-page.md#bulk-export) ページから、オンデマンドで複数のファイルをバッチ宛先に書き出します。 | **[!UICONTROL 宛先の詳細]** ページからオンデマンドで複数のファイルをバッチ宛先に書き出すオプションを、すべてのお客様が使用できるようになりました。 ![ 宛先の詳細ページの「今すぐファイルを書き出す」オプションをハイライト表示したExperience Platformユーザーインターフェイスの画像。](../2024/assets/august/bulk-export-file-now.png " 宛先の詳細ページの「今すぐファイルを書き出す」オプション "){width="250" align="center" zoomable="yes"} |
| [ 宛先の詳細 ](../../destinations/ui/destination-details-page.md#bulk-edit-file-names) ページから、書き出された複数のオーディエンスのファイル名を編集します。 | 書き出された複数のファイルの名前を **[!UICONTROL 宛先の詳細]** ページから直接編集できるようになりました。 ![ 宛先の詳細ページで「ファイル名を編集」オプションをハイライト表示したExperience Platformユーザーインターフェイスの画像。](../2024/assets/august/edit-file-name-destination-details.png " 宛先の詳細ページの「ファイル名を編集」オプション "){width="250" align="center" zoomable="yes"} |
| [ 宛先の詳細 ](../../destinations/ui/export-datasets.md#remove-dataset) ページで、データフローから複数のデータセットを削除します。 | データフローから複数のデータセットを削除するオプションは、すべてのお客様が利用できるようになりました。 ![ 宛先の詳細ページで「データセットを削除」オプションを強調表示したExperience Platformユーザーインターフェイスの画像。](../2024/assets/august/bulk-remove-datasets.png " 宛先の詳細ページの「データセットを削除」オプション "){width="250" align="center" zoomable="yes"} |

{style="table-layout:auto"}

詳しくは、[ 宛先の概要 ](../../destinations/home.md) を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| ML で支援されるスキーマ作成フロー | 高度な機械学習アルゴリズムを使用して、サンプルデータファイルを分析し、標準フィールドとカスタムフィールドを使用して最適化されたスキーマを自動的に作成します。<br> 主な機能：<br><ul><li>スキーマの迅速な作成：ML 推奨および生成された XDM フィールドを使用して、サンプルデータファイルから直接スキーマを生成します。</li><li>柔軟なスキーマ進化：生成されたスキーマ内のフィールドを簡単に追加または更新します。</li><li>シームレスな統合：スキーマ Ul のコアスキーマ作成フローと完全に統合され、スムーズでまとまりのあるユーザーエクスペリエンスを確保します。</li><li>効率的なレビューと編集：フラットビューエディターを使用してスキーマをすばやく表示および更新し、作成プロセスをより効率的で使いやすくします。</li></ul><br> 詳しくは、[ML-ASSISTED スキーマ作成ワークフローガイド ](../../xdm/ui/ml-assisted-schema-creation.md) を参照してください。 |

{style="table-layout:auto"}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを使用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある、パーソナライズされたデジタル体験をリアルタイムで提供できます。

**ドキュメントの更新**

| 機能 | 説明 |
| --- | --- |
| グラフ設定ガイド | ID グラフリンクルールおよび ID データを使用する際に発生する可能性のある一般的なグラフシナリオについて詳しくは、[ グラフ設定ガイド ](../../identity-service/identity-graph-linking-rules/example-configurations.md) を参照してください。 グラフ設定ガイドでは、単純な 1 人の人物によるグラフシナリオから、複雑で階層的な複数人のグラフのシナリオまで、様々な例を示しています。 また、このガイドを使用して、[ グラフシミュレーション UI](../../identity-service/identity-graph-linking-rules/graph-simulation.md) で入力できるイベントやアルゴリズム設定の例、および特定のグラフシナリオでプライマリ ID を選択する方法の分類を使用することもできます。 |

{style="table-layout:auto"}

ID サービスについて詳しくは、[ID サービスの概要](../../identity-service/home.md)を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 取り込みの詳細 | カスタムアップロードオリジンを使用したオーディエンスの場合、オーディエンスの取り込みの詳細をオーディエンスの詳細ページでより包括的に表示できます。 さらに、スキーマを選択し、ラベル設定に必要な属性を選択することで、ペイロード属性にラベルを適用できます。 取り込みの詳細の節について詳しくは、[ オーディエンスポータルガイド ](../../segmentation/ui/audience-portal.md#ingestion-details) を参照してください。 |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

Experience Platformのソースを使用すると、Adobeアプリケーションまたはサードパーティのデータソースからデータを取り込むことができます。

**ドキュメントの更新**

| ドキュメントの更新 | 説明 |
| --- | --- |
| データフローの更新に関するドキュメントを拡張しました | [UI での既存のソースデータフローの更新 ](../../sources/tutorials/ui/update-dataflows.md) に関するガイドが更新され、既存のデータフローに対して実行できる様々な設定に関する詳細が追加されました。 このガイドも更新され、無効になったデータフローが再度有効になる場合の期待される動作が明確になりました。 |

{style="table-layout:auto"}

詳しくは、[ ソースの概要 ](../../sources/home.md) を参照してください。
