---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2023年10月のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: fc0cb582d74f5ab52410991f65aa14ba05df3f97
workflow-type: tm+mt
source-wordcount: '954'
ht-degree: 42%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年10月25日（PT）**

 Experience Platform の既存の機能に対するアップデート：

- [ダッシュボード](#dashboards)
- [データ収集](#data-collection)
- [宛先](#destinations)
- [サンドボックス](#sandboxes)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## ダッシュボード {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを確認できる複数のダッシュボードを提供しています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 宛先の使用指標 | ライセンス使用状況ダッシュボードに、新しいメータリング指標が追加されました。 The **[!UICONTROL Audience Activationサイズ]** および **[!UICONTROL データ書き出しサイズ]** 指標を使用すると、ライセンス使用権限に関連して Platform から書き出したデータ量を追跡できます。 詳しくは、 [利用可能な指標](../../dashboards/guides/license-usage.md#available-metrics) およびその他のライセンス使用状況指標の説明に関するドキュメントです。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| 拡張機能 | [!DNL Meta] コンバージョン API の機能強化 | 次の 3 つの機能強化がおこなわれました： [メタ変換 API](/help/tags/extensions/server/meta/overview.md) 拡張子： <ul><li>との統合 [[!DNL Meta Business Extension (MBE)]](/help/tags/extensions/server/meta/overview.md#integration-with-meta-business-extension-mbe)：コンバージョン API 統合用のピクセル ID とアクセストークンをAdobeと共有でき、シームレスなログイン操作を作成します。</li><li>との統合 [[!DNL Event Match Quality Score (EMQ)]](/help/tags/extensions/server/meta/overview.md#integration-with-event-quality-match-score-emq)：目的のアクションを完了し、配信された広告にアクションをリンクし直す可能性の高い人に広告を配信できます。</li><li>との統合 [[!DNL LiveRamp (Alpha)]](/help/tags/extensions/server/meta/overview.md#integration-with-liveramp-alpha):PII を直接パートナーやメタと共有する必要がなく、CIP フィールドに LiveRamp の RampID を渡すことができます。 </li></ul> |
| 拡張機能 | [!DNL LinkedIn] コンバージョン API | The [[!DNL LinkedIn] コンバージョン API](../../tags/extensions/server/linkedin/overview.md) 拡張機能を使用すると、Experience PlatformイベントデータをLinkedInに転送することで、LinkedInマーケティングキャンペーンの効果を評価できます。 |
| 秘密鍵 | [!DNL LinkedIn] OAuth 2 Secret | The [[!DNL LinkedIn] OAuth 2 Secret](../../tags/ui/event-forwarding/secrets.md#linkedin-oauth-2) を使用すると、サーバー間インタラクションを次の場所に送信できます： [!DNL LinkedIn] イベント転送で使用できます。 |

データ収集について詳しくは、[データ収集の概要](../../tags/home.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先** {#new-updated-destinations}

| 宛先 | 新規／アップデート | 説明 |
| ----------- |----------------|----------- |
| [[!DNL MoEngage]](/help/destinations/catalog/mobile-engagement/moengage.md) | 新規 | Moengage の宛先を使用して、Adobeデータ（ユーザー属性、セグメントおよびイベント）を MoEngage にリアルタイムで接続し、マッピングします。 その後、顧客はこのデータに基づいて行動し、パーソナライズされたターゲット設定されたエクスペリエンスを提供できます。 |
| [[!DNL Qualtrics Automations]](/help/destinations/catalog/survey/qualtrics-automations.md) | 新規 | Adobe Experience Platformの複数の運用データソースの集計を Qualtrics Experience ID の入力として使用すると、顧客をより深く理解し、目的、感情、エクスペリエンスの推進要因を把握する際に、ターゲットを絞ったアウトリーチを有効にしてギャップを埋めることができます。 |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| （ベータ版）計算フィールドでのハッシュ関数のサポート | に固有の関数に加えて、 [配列の書き出し](../../destinations/ui/export-arrays-calculated-fields.md) または配列の要素から、追加の [ハッシュ関数](../../destinations/ui/export-arrays-calculated-fields.md#hashing-functions) 書き出したファイルの属性をハッシュ化します。 サポートされているハッシュ関数は次のとおりです。 `sha`, `sha256`, `sha512`, `hash`, `md5`, `crc32`. |
| （限定的な GA）特定の宛先に対するアカウントオーディエンスのアクティブ化 | Real-Time CDP B2B のお客様は、 [アカウントオーディエンス](../../segmentation/ui/account-audiences.md) 特定の宛先に送信できます。 この機能の詳細については、 [アカウントオーディエンスの有効化チュートリアル](/help/destinations/ui/activate-account-audiences.md). |

{style="table-layout:auto"}

**修正および機能強化** {#destinations-fixes-and-enhancements}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対処するため、Experience Platformは、1 つの Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展を支援するサンドボックスを提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| サンドボックスツール | サンドボックスツール機能を使用すると、サンドボックス間の設定の精度を向上させ、サンドボックス間でサンドボックス設定をシームレスに書き出し、読み込むことができます。 サンドボックスツール機能を使用すると、異なるオブジェクトを選択してパッケージにエクスポートできます。 詳しくは、 [サンドボックスツール UI ガイド](../../sandboxes/ui/sandbox-tooling.md). |

サンドボックスについて詳しくは、 [サンドボックスの概要](../../sandboxes/home.md) を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| アカウントオーディエンス（限定 GA） | Real-time Customer Data Platform B2B Edition で、アカウントのセグメント化を使用して、ユーザーベースのオーディエンスからアカウントベースのオーディエンスに至る、マーケティングセグメント化エクスペリエンスを完全に簡単かつ洗練化できるようになりました。 この機能の詳細については、 [アカウントオーディエンスの概要](../../segmentation/ui/account-audiences.md). |

セグメント化サービスの詳細については、「[セグメント化サービスの概要](../../segmentation/home.md)」を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| データランディングゾーンの認証を更新しました | 資格情報の表示時に、データランディングゾーンの指定された有効期限が表示されるようになりました。 アプリケーションでトークンを引き続き使用するには、有効期限の前にトークンを更新する必要があります。 指定した有効期限より前に手動でトークンを更新しない場合、次回資格情報を取得する際に自動的に更新され、新しいトークンが提供されます。 詳しくは、 [データランディングゾーンの使用](../../sources/tutorials/ui/create/cloud-storage/data-landing-zone.md). |

{style="table-layout:auto"}

ソースの詳細については、 [ソースの概要](../../sources/home.md).
