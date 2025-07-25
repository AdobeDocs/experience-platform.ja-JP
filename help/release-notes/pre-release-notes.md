---
title: Experience Platformのプレリリースノート
description: Adobe Experience Platformの最新のリリースノートのプレビュー。
hide: true
hidefromtoc: true
exl-id: f2c41dc8-9255-4570-b459-4f9fc28ee58b
source-git-commit: 7e91181f71b84fdaf04a39e003cbbd415827e282
workflow-type: tm+mt
source-wordcount: '1387'
ht-degree: 17%

---

# Adobe Experience Platformのプレリリースノート

>[!IMPORTANT]
>
>このドキュメントは、今月のリリースノートの **プレビュー** として提供することを目的としています。 リリース項目は変更される場合があり、最終リリースで追加または削除される場合があります。

>[!TIP]
>
>その他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/releases/pre-release-notes)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/ja/docs/federated-audience-composition/using/e-release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/ja/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2025年7月29日（PT）**

Adobe Experience Platformの既存の機能に対する新機能とアップデート：

- [宛先](#destinations)
- [データ取り込み](#ingestion)
- [クエリサービス](#query-service)
- [Real-Time CDP B2B エディション](#b2b)
- [サンドボックス](#sandboxes)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| Marketo宛先カードの統合 | Marketo V2 とMarketo Engage Person Sync の宛先カードは、1 つの統合された宛先カードに統合されました。 この統合により、宛先の選択プロセスが簡略化され、Marketo統合をより効率的に行うことができます。 |

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| エッジ宛先用の強化されたデータストリーム情報 | Adobe Targetとカスタム Personalizationの宛先の右側パネルの情報が改善され、データストリーム名が表示されるようになりました。これにより、関連するデータストリーム設定がよりわかりやすく表示され、既存のデータフローを確認する際の混乱が軽減されます。 宛先設定画面の **[!UICONTROL データストリーム ID]** セレクターが **[!UICONTROL データストリーム]** に更新され、ユーザーインターフェイスがわかりやすくなりました。 |
| 宛先選択でのマーケティングアクションの表示 | マーケティングアクションが宛先 **[!UICONTROL 参照]** タブの右側のパネルおよび **[!UICONTROL データフロー実行]** ページに表示されるようになり、表示ページに移動しなくてもマーケティングアクションの変更をすぐに確認できるようになりました。 この改善により、宛先の設定中にマーケティングアクション設定を簡単に検証できるようになり、ユーザーエクスペリエンスが向上します。 |
| （限定ベータ版）宛先のマーケティングアクションの編集 | 既存の宛先のマーケティングアクションを編集できるようになりました。 この機能は限定的なベータ版です。 アクセスをリクエストするには、Adobe担当者にお問い合わせください。 |
| （限定的なベータ版）宛先の編集 | これで、作成後に宛先設定を編集できます。 この機能は限定的なベータ版です。 アクセスをリクエストするには、Adobe担当者にお問い合わせください。 |
| 宛先接続のアカウント名と説明 | 宛先に接続する際にアカウント名と説明を追加できるようになり、複数のアカウントを持つ宛先をより適切に管理できます。 |

**修正点**

| 問題 | 説明 |
| --- | --- |
| カテゴリスクロール機能 | 宛先とソースカタログのカテゴリ サイドメニューが、マウスポインターを置くと正しくスクロールされず、宛先カテゴリを参照するユーザーのナビゲーションの使いやすさが向上する問題を修正しました。 |

詳しくは、[ 宛先の概要 ](../destinations/home.md) を参照してください。

## データ取り込み {#ingestion}

Experience Platformは、様々なソースからのバッチデータ取り込みとストリーミングデータ取り込みの両方をサポートする包括的なデータ取り込みフレームワークを提供します。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングプロファイル取り込みの監視のサポート | ストリーミングプロファイル取り込みのリアルタイム監視が使用できるようになり、スループット、待ち時間、データ品質指標に対する透明性が提供されます。 これにより、プロアクティブなアラートと実用的なインサイトがサポートされ、データエンジニアが容量違反と取り込みの問題を特定するのに役立ちます。 |

詳しくは、[ データ取り込みの概要 ](../ingestion/home.md) を参照してください。

## クエリサービス {#query-service}

Adobe Experience Platform クエリサービスは、プラットフォーム全体でのデータ分析と調査に対応する堅牢な SQL インターフェイスを提供します。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| セッション管理の強化 | Data Distillerのセッション管理機能が強化され、開発および実稼動環境全体でユーザーセッションをより詳細に制御し、パフォーマンスのモニタリングを強化できるようになりました。 |
| 有効期限のない資格情報のパスワード文字制限のサポート | Data Distillerでは、特定の文字制限を持つ、有効期限のない資格情報をサポートするようになりました。 パスワードには数字、小文字、大文字、特殊文字が少なくとも 1 文字必要ですが、ドル記号（$）はサポートされていません。 推奨される特殊文字には `!, @, #, ^, or &` があります。 |
| 環境全体にわたるパフォーマンスの一貫性の向上 | Data Distillerのパフォーマンスが開発用サンドボックスと実稼動用サンドボックスの間で一貫するようになり、どちらの環境でも同様のバックエンドリソースが使用できるようになりました。 計算時間は、データ量と、処理時に使用可能なバックエンド計算リソースによって異なる場合があります。 |

詳しくは、[ クエリサービスの概要 ](../query-service/home.md) を参照してください。

## Real-Time CDP B2B エディション {#b2b}

Real-Time CDP B2B editionは、包括的な B2B 顧客データ管理機能を提供します。これにより、組織は統合された顧客プロファイルを作成し、高度な B2B オーディエンスを作成して、様々なマーケティングチャネルにわたってデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| B2B アーキテクチャのアップグレード | Experience Platformは、B2B 属性を持つマルチエンティティオーディエンスを大幅に改善する新しい B2B アーキテクチャにアップグレードしています。 このアップグレードにより、結合ポリシーのサポートが統合され、オーディエンス数の精度が向上し、エンティティ解決機能が向上します。 |
| 複数エンティティオーディエンスの結合ポリシーの統合 | B2B 属性を持つマルチエンティティオーディエンスでは、複数の結合ポリシーをサポートするのではなく、単一の結合ポリシー（デフォルトの結合ポリシー）のみをサポートするようになりました。 この変更により、一貫したオーディエンス構成が保証され、結合ロジックの管理が簡素化されます。 |
| アカウントオーディエンス制約の更新 | アカウントオーディエンスには、エクスペリエンスイベントの 30 日間のルックバックウィンドウ、カスタムエンティティの制限、`inSegment` スタムイベントの使用制限という、以前の制約がなくなりました。 これらの更新により、複雑な B2B オーディエンス定義を柔軟に作成できるようになります。 |
| B2B エンティティのオーディエンス数の拡張 | アカウントや商談などの B2B エンティティを持つオーディエンスのオーディエンスサイズの予測が、リアルタイムのセグメント化の結果に基づいて正確になりました。 この改善により、B2B の複雑な関係に関連するオーディエンスに対して、より正確で信頼性の高い予測が提供されます。 |
| オーディエンスメンバーシップのアカウントスナップショット | オーディエンスメンバーシップの詳細がスナップショット書き出しのアカウントエンティティに含まれるようになり、アカウントレベルのオーディエンスステータス、タイムスタンプ、メンバーシップ指標にアクセスできるようになりました。 これにより、プロファイル（人物）とアカウントのセグメント化モデルの間に機能パリティが生じます。 |
| マルチエンティティオーディエンス用のサンドボックスツールの変更 | 移行前に書き出された B2B エンティティとエクスペリエンスイベントを含んだマルチエンティティオーディエンスの読み込みは、サポートされなくなりました。 これらのオーディエンスは読み込みの検証に失敗し、新しいアーキテクチャに自動的に変換することはできません。 ターゲットサンドボックスにインポートする前に、移行後にオーディエンスを再エクスポートする必要があります。 |
| B2B Entity API の廃止 | B2B エンティティ（アカウント、商談、アカウントと人物の関係、商談と人物の関係、キャンペーン、キャンペーンメンバー、マーケティングリスト、マーケティングリストメンバー）の API を使用したオーディエンスの作成は、非推奨（廃止予定）になりました。 さらに、これらの B2B エンティティに対するプロファイルアクセス API のルックアップ操作と削除操作も非推奨（廃止予定）になりました。 |
| エンティティ解決の ID 名前空間の更新 | アカウントエンティティと商談エンティティでは、特定の ID 名前空間を使用した時間優先ベースの結合が使用されるようになりました（アカウントの場合は `b2b_account`、商談の場合は `b2b_opportunity`）。 他のすべてのエンティティは、時間の優先順位に基づくマージを使用して、プライマリ ID の重複がマージされて統合されます。 |

詳しくは、[Real-Time CDP B2B editionの概要 ](../rtcdp/b2b-overview.md) を参照してください。

## サンドボックス {#sandboxes}

Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| マルチエンティティオーディエンスの読み込みの変更 | サンドボックスツールが更新され、新しい B2B アーキテクチャのアップグレードがサポートされるようになりました。 B2B エンティティとエクスペリエンスイベントを含んだマルチエンティティオーディエンスは、サンドボックスツールを使用してターゲットサンドボックスにインポートする前に、アーキテクチャのアップグレード後に再度書き出す必要があります。 アップグレード前のバージョンをインポートすると、検証に失敗します。 |

サンドボックスについて詳しくは、[ サンドボックスの概要 ](../sandboxes/home.md) を参照してください。

## セグメント化サービス {#segmentation-service}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。オーディエンスは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて設定できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 外部オーディエンス API | 外部オーディエンス API を使用すると、外部で生成されたオーディエンスをプログラムでAdobe Experience Platformに読み込むことができます。 |

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新しいソース**

| ソース | 説明 |
| --- | --- |
| [!DNL Didomi] （ストリーミングSDK）のサポート | [!DNL Didomi] ソースコネクタを使用すると、[!DNL Didomi] のプラットフォームから同意管理データを取り込み、プライバシー規制や同意ベースのマーケティング戦略への準拠をサポートできます。 |

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 選択したソースでの change data capture のサポート | ソースコネクタを使用して、増分取り込みの変更データキャプチャを有効にするデータフローを作成できるようになりました。 この機能を使用すると、お客様はデータの種類を変更して増分取り込みを行い、データの鮮度を向上させ、処理オーバーヘッドを削減できます。 |
| [!DNL Salesforce] でのレコードのソフト削除のサポート | [!DNL Salesforce] ソースでは、オプションの `includeDeletedObjects` パラメーターを使用して、ソフト削除されたレコードを含めることができるようになりました。 true に設定した場合、お客様は、ソフト削除されたレコードを [!DNL Salesforce] クエリに含め、これらのレコードをExperience Platformに取り込むことができます。 |

詳しくは、[ソースの概要](../sources/home.md)を参照してください。
