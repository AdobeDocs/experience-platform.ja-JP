---
title: Adobe Experience Platform リリースノート 2022年10月
description: Adobe Experience Platform の 2022年10月のリリースノート。
exl-id: 61ef2472-5e79-433f-9f60-b1245f619b42
source-git-commit: 260ba98f920c8006ab3ed7fb2519a8c1720916c8
workflow-type: tm+mt
source-wordcount: '1159'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年10月26日（PT）**

- [顧客管理キー](#cmk)
- [データ収集](#data-collection)
- [宛先](#destinations)
- [エクスペリエンスデータモデル](#xdm)
- [クエリサービス](#query-service)

## 顧客管理キー {#cmk}

Adobe Experience Platform に保存されるすべてのデータは、システムレベルのキーを使用して、保存時に暗号化されます。Platform 上に作成されたアプリケーションを使用している場合は、代わりに独自の暗号化キーを使用するように選択できるようになり、データのセキュリティをより詳細に制御できます。

機能について詳しくは、[顧客管理キー](../../landing/governance-privacy-security/customer-managed-keys/overview.md)の概要を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| データストリームの機密データ処理 | データストリームでは、複数の Platform テクノロジーを活用して、HIPAA（Health Insurance Portability and Accountability Act）などの規制で強化される機密データを適切に処理するようになりました。詳しくは、[データストリームでの機密データの処理](../../datastreams/overview.md#sensitive)を参照してください。 |
|  イベント転送用の [!DNL Splunk] 拡張機能 | [イベント転送](../../tags/ui/event-forwarding/overview.md)拡張機能を使用して、[!DNL Splunk] にデータを送信できるようになりました。詳しくは、[[!DNL Splunk] 拡張機能の概要](../../tags/extensions/server/splunk/overview.md)を参照してください。 |
|  イベント転送用の [!DNL Zendesk] 拡張機能 | [イベント転送](../../tags/ui/event-forwarding/overview.md)拡張機能を使用して、[!DNL Zendesk] にデータを送信できるようになりました。詳しくは、[[!DNL Zendesk] 拡張機能の概要](../../tags/extensions/server/zendesk/overview.md)を参照してください。 |

{style="table-layout:auto"}

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| （ベータ版）データセットの書き出し | [ベータ版の機能であるデータセットの書き出し](/help/destinations/ui/export-datasets.md)を利用すると、最初に生成したデータ（[Real-Time Customer Data Platform の製品説明](https://helpx.adobe.com/jp/legal/product-descriptions/real-time-customer-data-platform-b2c-edition-prime-and-ultimate-packages.html)で定義済み）を Adobe Experience Platform から宛先ユーザーインターフェイスを介して顧客独自の外部システムに書き出すことができます。これにより、ノーコード／ローコードのワークフローを使用して、6 つのクラウドストレージの宛先（以下の表に示す）に Experience Platform からデータを書き出して、分析およびコンプライアンスのユースケースを実行できます。 |
| （ベータ版）ファイル書き出し機能の強化 | Experience Platform からファイルを書き出す際に、以下の強化されたカスタマイズ機能を利用できるようになりました。<br><ul><li>追加された[ファイル命名オプション](/help/destinations/ui/activate-batch-profile-destinations.md#file-names)。</li><li>書き出されたファイルにカスタムファイルヘッダーを設定する機能（[マッピングステップの改善](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)による）</li><li>[書き出し対象の CSV データファイルの形式をカスタマイズする機能](/help/destinations/ui/batch-destinations-file-formatting-options.md)。</li></ul> <br> この機能は、以下の表に示す 6 つの新しいベータ版クラウドストレージカードでサポートされています。 |

{style="table-layout:auto"}

**新規宛先または更新された宛先** {#new-or-updated-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!DNL Line]](../../destinations/catalog/mobile-engagement/line.md) | LINE は、人物、サービスおよび情報をつなぎ、チャットアプリからエンターテインメント、ソーシャルおよび日々のアクティビティのハブに成長した人気のコミュニケーションプラットフォームです。 |
| [[!DNL Microsoft Dynamics 365]](../../destinations/catalog/crm/microsoft-dynamics-365.md) | Microsoft Dynamics 365 は、クラウドベースのビジネスアプリケーションプラットフォームです。エンタープライズリソースプランニング（ERP）と顧客関係管理（CRM）を生産性アプリケーションおよび AI ツールと共に組み合わせて、よりスムーズで管理が強化されたエンドツーエンドの運用、成長の可能性の向上およびコスト削減を実現します。 |
| [[!DNL (Beta) Adobe Commerce]](../../destinations/catalog/personalization/adobe-commerce.md) | [!DNL (Beta) Adobe Commerce] 宛先コネクタを使用すると、[!DNL Adobe Commerce] アカウントに対してアクティブ化する Real-Time CDP セグメントを 1 つ以上選択して、買い物客向けにパーソナライズされた動的なエクスペリエンスを提供できます。[!DNL Adobe Commerce] 内でこれらの Real-Time CDP セグメントを選択して、「2 つ買うと 1 つ無料」などの、買い物かご内の独自のオファーをパーソナライズできます。また、ヒーローバナーを表示したり、プロモーションオファーを通じて製品の価格を変更したりすることもできます。これらはすべて Adobe Real-Time CDP セグメント用にカスタマイズされています。 |
| [[!DNL (Beta) Azure Data Lake Storage Gen2]](../../destinations/catalog/cloud-storage/adls-gen2.md) | [!DNL Azure Data Lake Storage Gen2] へのライブアウトバウンド接続を作成して、Adobe Experience Platform から独自のストレージの場所に定期的にデータファイルを書き出します。この新しいベータ版の宛先は、拡張されたファイル書き出し機能を提供し、データセットの書き出しをサポートします。 |
| [[!DNL (Beta) Data Landing Zone]](../../destinations/catalog/cloud-storage/data-landing-zone.md) | [!DNL Data Landing Zone] は、Adobe Experience Platform によってプロビジョニングされる [!DNL Azure Blob] ストレージインターフェイスであり、Platform からファイルを書き出すための安全なクラウドベースのファイルストレージ機能へのアクセスを許可します。この新しいベータ版の宛先は、拡張されたファイル書き出し機能を提供し、データセットの書き出しをサポートします。 |
| [[!DNL (Beta) Google Cloud Storage]](../../destinations/catalog/cloud-storage/google-cloud-storage.md) | [!DNL Google Cloud Storage] へのライブアウトバウンド接続を作成して、Adobe Experience Platform から独自のバケットに定期的にデータファイルを書き出します。この新しいベータ版の宛先は、拡張されたファイル書き出し機能を提供し、データセットの書き出しをサポートします。 |
| [[!DNL (Beta) Amazon S3]](../../destinations/catalog/cloud-storage/amazon-s3.md#changelog) | ベータ版の参加者には、宛先カタログに 2 つの [!DNL Amazon S3] 宛先カードが並んで表示されるようになりました。新しいベータ版の宛先は、拡張されたファイル書き出し機能を提供し、データセットの書き出しをサポートします。 |
| [[!DNL (Beta) Azure Blob]](../../destinations/catalog/cloud-storage/azure-blob.md#changelog) | ベータ版の参加者には、宛先カタログに 2 つの [!DNL Azure Blob] 宛先カードが並んで表示されるようになりました。新しいベータ版の宛先は、拡張されたファイル書き出し機能を提供し、データセットの書き出しをサポートします。 |
| [[!DNL (Beta) SFTP]](../../destinations/catalog/cloud-storage/sftp.md#changelog) | ベータ版の参加者には、宛先カタログに 2 つの [!DNL SFTP] 宛先カードが並んで表示されるようになりました。新しいベータ版の宛先は、拡張されたファイル書き出し機能を提供し、データセットの書き出しをサポートします。 |

{style="table-layout:auto"}

**新規ドキュメントまたは更新されたドキュメント**

| ドキュメント | 説明 |
| ----------- | ----------- |
| [宛先ガードレール](../../destinations/guardrails.md) | このページでは、アクティベーション動作に関するデフォルトの使用方法とレート制限について説明します。 |

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| データタイプ | [[!UICONTROL セッションの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/sessiondetails.schema.json) | `authorized` フィールドをブーリアン型から文字列型に更新しました。`season` および `episode` は、整数から文字列に変更されました。 |
| データタイプ | [[!UICONTROL 広告の詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/advertisingdetails.schema.json) | `name` の名前は `friendlyName` に変更されました。`ID` の名前は `name` に変更されました。 |
| データタイプ | [[!UICONTROL エラーの詳細情報]](https://github.com/adobe/xdm/blob/master/components/datatypes/errordetails.schema.json) | `ID` の名前は `name` に変更されました。 |

{style="table-layout:auto"}

Platform の XDM について詳しくは、[XDM システムの概要](../../xdm/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。任意のデータセットを [!DNL Data Lake] から結合し、クエリの結果を新しいデータセットとして取得することで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| Platform UI によるクエリの監視 | クエリサービスの「[!UICONTROL 予定クエリ]」タブでは、UI を使用して、すべてのクエリジョブのステータスをより明確に表示できます。「[!UICONTROL 予定クエリ]」タブから、エラーメッセージや失敗した場合のコードなど、クエリ実行のステータスに関する重要な情報を確認できるようになりました。これらのクエリのステータスに基づいて、UI を使用して、アラートを購読することもできます。この機能について詳しくは、[クエリドキュメントの監視](../../query-service/ui/monitor-queries.md)を参照してください。 |
| クエリ高速化レポートインサイトデータモデル | Data Distiller SKU の一部として、クエリ高速化ストアを使用すると、データから重要なインサイトを得るために必要な時間と処理能力を削減できます。クエリ高速化ストアを使用すると、カスタムデータモデルを作成したり、既存の Adobe Real-time Customer Data Platform データモデルを拡張したりして、レポートインサイトとそのビジュアライゼーションを改善できます。この機能について詳しくは、[クエリ高速化ストアレポートインサイトのドキュメント](../../query-service/data-distiller/query-accelerated-store/reporting-insights-data-model.md)を参照してください。 |

{style="table-layout:auto"}

クエリサービスについて詳しくは、[クエリサービスの概要](../../query-service/home.md)を参照してください。
Adobe Experience Platform の新機能：

