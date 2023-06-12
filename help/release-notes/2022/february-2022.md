---
title: Adobe Experience Platform リリースノート（2022年2月）
description: Adobe Experience Platform の 2022年2月のリリースノートです。
exl-id: ae453f7d-ac75-4cc3-8435-57d25f086cc3
source-git-commit: e2342a8a7d03074ac26fbd129a2e7fd520ccb0c3
workflow-type: tm+mt
source-wordcount: '1019'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年3月7日**

>[!NOTE]
>
>このリリースは、当初の日付の 2月23日から 3月7日に変更されました。

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Dashboards]](#dashboards)
- [[!DNL Data collection]](#data-collection)
- [[!DNL Destinations]](#destinations)
- [[!DNL Identity Service]](#identity)
- [[!DNL Sources]](#sources)

## [!DNL Dashboards] {#dashboards}

Adobe Experience Platform では、複数の [!DNL dashboards] を提供しており、毎日のスナップショットでキャプチャされた、組織のデータに関する重要な情報を表示できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しい標準宛先ウィジェット | 以下の標準ウィジェットを使用すると、宛先に関連する異なる指標を視覚化できます。<ul><li>最近アクティブ化されたセグメント（宛先別）。このウィジェットは、選択した宛先に応じて、最近アクティブ化された上位 5 つのセグメントを降順で示します。</li><li>オーディエンスサイズのトレンド。このウィジェットは、宛先アカウントにマッピングされたセグメントの期間におけるプロファイル数の関係を示します。</li><li>マッピングされていないセグメント（ID 別）。このウィジェットには、特定の宛先と ID に対して、ID 数の降順でランク付けされた上位 5 つのマッピングされていないセグメントが一覧表示されます。</li><li>マッピングされたセグメント（ID 別）。このウィジェットは、上位 5 つのマッピングされたセグメントのリストを表示します。セグメントは、ウィジェットのドロップダウンメニューから選択した宛先 ID に一致するソース ID のそれぞれのカウント数に従って、多い順に並べられます。</li><li>一般的なオーディエンス。このウィジェットには、ページ上部で選択された宛先アカウント全体でアクティブ化された上位 5 つのセグメントと、ウィジェットのドロップダウンで選択された宛先が一覧表示されます。</li></ul> 使用できる標準ウィジェットについて詳しくは、[宛先ダッシュボードドキュメント](https://experienceleague.adobe.com/docs/experience-platform/dashboards/guides/destinations.html?lang=ja#standard-widgets)を参照してください。 |

[!DNL Dashboards] について詳しくは、[[!DNL Dashboards] 概要](../../dashboards/home.md)を参照してください。

## データ収集 {#data-collection}

Platform は、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信します。そこでデータを強化、変換、アドビまたはアドビ以外の宛先への配信を可能にする一連のテクノロジーを提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| データストリーム設定時の UI ワークフローの改善 | データ収集 UI で新しいデータストリームを作成するためのワークフローが新しくなりました。データストリームにサービスを追加するとき、アクセス許可のあるサービスのみがオプションのリストに含まれるようになります。詳しくは、[データストリームの設定](../../edge/datastreams/overview.md)のガイドを参照してください。 |
| データ収集のためのデータ準備 | Adobe Experience Platform Web SDK を使用している場合、データ準備機能を利用して、サーバーサイドのエクスペリエンスデータモデル（XDM）にデータをマッピングできるようになりました。詳しくは、データストリームガイドの[データ収集用のデータ準備](../../edge/datastreams/data-prep.md)に関する節を参照してください。 |
| ファーストパーティデバイス ID | Platform Web SDK を使用して顧客データを収集する際に、独自のデバイス ID を Adobe Experience Platform Edge Network に送信できるようになりました。これにより、サードパーティ cookie の有効期間に関する最近のブラウザー制限の回避策を使用できます。詳しくは、[ファーストパーティデバイス ID](../../edge/identity/first-party-device-ids.md) に関するガイドを参照してください。 |

Platform のデータ収集について詳しくは、[データ収集の概要](../../collection/home.md)を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ----------- | ----------- |
| （ベータ版）Destination SDK によるファイルベースの宛先のサポート | [Destination SDK によるファイルベースの宛先のサポート](../../destinations/destination-sdk/functionality/destination-server/server-specs.md)は、現在、プライベートベータ版で、一部のパートナーおよびお客様にのみ提供されています。機能および関連するドキュメントは、一般リリース前に変更されることがあります。<br><br>機能へのアクセス方法については、アドビアカウント担当者にお問い合わせください。アドビの社内アカウント担当者が Experience Platform 宛先製品およびエンジニアリングチームに連絡して、サポートされるユースケースについて相談します。<br><br> Destination SDK によるファイルベースの宛先のサポートのベータ版フェーズでは、ベータ版パートナーおよびお客様が [Experience Platform Destination SDK](../../destinations/destination-sdk/overview.md) を使用して、以下の機能を利用できるプライベート宛先を作成できます。 <ul><li>Amazon S3、SFTP サーバー、Azure Blob、Azure Data Lake Storage、データランディングゾーンストレージを介して、ファイルベースの（バッチ）宛先を作成。</li><li>デフォルトのファイル書き出しスケジュールおよび頻度オプションを設定。</li><li>書き出された CSV ファイルを書式設定するオプションを設定（区切り文字、エスケープ文字、その他のオプション）。</li><li>カスタムファイルヘッダーを設定および編集する機能。</li><li>ファイルおよびセグメントの書き出しに関するイベント通知を受信する機能。</li><li>追加のファイルタイプを書き出す機能（CSV、TSV、JSON、Parquet など）。</li></ul>  <br>新しい機能の使用を開始するには、[（ベータ版）Destination SDK を使用したファイルベースの宛先の設定](../../destinations/destination-sdk/guides/configure-file-based-destination-instructions.md)を参照してください。Destination SDK を使用して<br><br>プライベートまたは製品化された&#x200B;*ストリーミング*&#x200B;宛先を作成する機能は、既にすべての Experience Platform 顧客およびパートナーが使用できます。詳しくは、[Destination SDK を使用したストリーミング宛先の設定](../../destinations/destination-sdk/guides/configure-destination-instructions.md)方法に関するガイドを参照してください。 |

## [!DNL Identity Service] {#identity}

適切なデジタルエクスペリエンスを提供するには、顧客を完全に理解する必要があります。顧客データが異なる複数のシステムに断片化されており、そのため各顧客が複数の「ID」を持つと考えられる場合、顧客を理解するのはさらに困難になります。

Adobe Experience Platform [!DNL Identity Service] を利用すると、デバイスやシステム間のアイデンティティを橋渡しすることで、顧客と顧客の行動をより把握し、インパクトのあるパーソナルなデジタル体験をリアルタイムで提供できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| `view-identity-graph` の新しい権限 | `view-identity-graph` 権限を使って、組織内のユーザーが ID グラフデータを表示できるかどうかを制御できるようになりました。この権限を持たないユーザーは、UI で ID グラフビューアにアクセスすることや、ID を返す [!DNL Identity Service] API にアクセスすることが禁止されます。権限について詳しくは、[アクセス制御の概要](../../access-control/home.md)を参照してください。 |

[!DNL Identity Service] の一般的な情報について詳しくは、[ID サービスの概要](../../identity-service/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| ベータ版ソースの一般公開（GA） | 以下のソースがベータ版から一般公開（GA）に昇格しました。 <ul><li>[[!DNL Mailchimp Campaigns]](../../sources/connectors/marketing-automation/mailchimp.md)</li><li>[[!DNL Mailchimp Members]](../../sources/connectors/marketing-automation/mailchimp.md)</li><li>[[!DNL Zoho CRM]](../../sources/connectors/crm/zoho.md)</li></ul> |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。