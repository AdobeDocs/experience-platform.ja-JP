---
title: Adobe Experience Platformリリースノート 2022 年 2 月
description: Adobe Experience Platformの 2022 年 2 月のリリースノート。
exl-id: ae453f7d-ac75-4cc3-8435-57d25f086cc3
source-git-commit: 0a01dd2b0d8a1039178e3593475f9a87639ccdcd
workflow-type: tm+mt
source-wordcount: '1019'
ht-degree: 53%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年3月7日（PT）**

>[!NOTE]
>
>今回のリリースは、元の 2 月 23 日から 3 月 7 日に移行しました。

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Dashboards]](#dashboards)
- [[!DNL Data collection]](#data-collection)
- [[!DNL Destinations]](#destinations)
- [[!DNL Identity Service]](#identity)
- [[!DNL Sources]](#sources)

## [!DNL Dashboards] {#dashboards}

Adobe Experience Platform [!DNL dashboards] 毎日のスナップショットでキャプチャされた、組織のデータに関する重要なインサイトを表示できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しい標準の宛先ウィジェット | 次の標準ウィジェットを使用すると、宛先に関連する様々な指標を視覚化できます。<ul><li>最近アクティブ化されたセグメント（宛先別）。 このウィジェットには、最近アクティブ化された上位 5 つのセグメントが、選択した宛先に応じて降順で表示されます。</li><li>オーディエンスサイズのトレンド. このウィジェットは、対象のアカウントにマッピングされたセグメントの一定期間のプロファイル数の関係を示します。</li><li>ID によるマッピングされていないセグメント。 このウィジェットは、特定の宛先と ID に対して ID 数を降順に並べた、マッピングされていない上位 5 つのセグメントをリストします。</li><li>ID によってマッピングされたセグメント。 このウィジェットは、マッピングされた上位 5 つのセグメントをリストします。 セグメントは、ウィジェットのドロップダウンメニューで選択した宛先 ID と一致するソース ID の数に応じて、高い順から低い順に並べられます。</li><li>一般的なオーディエンス。 このウィジェットには、ページ上部で選択された宛先アカウントでアクティブ化された上位 5 つのセグメントと、ウィジェットドロップダウンで選択された宛先のリストが表示されます。</li></ul> 使用可能な標準ウィジェットについて詳しくは、 [宛先ダッシュボードドキュメント。](https://experienceleague.adobe.com/docs/experience-platform/dashboards/guides/destinations.html?lang=en#standard-widgets). |

[!DNL Dashboards] について詳しくは、[[!DNL Dashboards] 概要](../../dashboards/home.md)を参照してください。

## データ収集 {#data-collection}

Platform は、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信します。そこでデータを強化、変換、アドビまたはアドビ以外の宛先への配信を可能にする一連のテクノロジーを提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| データストリーム設定時の UI ワークフローの改善 | データ収集 UI で新しいデータストリームを作成するためのワークフローが新しくなりました。データストリームにサービスを追加するとき、アクセス許可のあるサービスのみがオプションのリストに含まれるようになります。詳しくは、[データストリームの設定](../../edge/datastreams/overview.md)のガイドを参照してください。 |
| データ収集のためのデータ準備 | Adobe Experience Platform Web SDK を使用している場合、データ準備機能を利用して、サーバー側の Experience Data Model（XDM）にデータをマッピングできるようになりました。詳しくは、データストリームガイドの[データ収集用のデータ準備](../../edge/datastreams/data-prep.md)に関する節を参照してください。 |
| ファーストパーティデバイス ID | Platform Web SDK を使用して顧客データを収集する際に、独自のデバイス ID をAdobe Experience Platform Edge Network に送信できるようになりました。これにより、サードパーティ cookie の有効期間に関する最近のブラウザー制限の回避策を使用できます。詳しくは、[ファーストパーティデバイス ID](../../edge/identity/first-party-device-ids.md) に関するガイドを参照してください。 |

Platform のデータ収集について詳しくは、[データ収集概要](../../rtcdp-connections/home.md)を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ----------- | ----------- |
| （ベータ版）ファイルベースの宛先のDestination SDKのサポート | [Destination SDKによるファイルベースの宛先のサポート](../../destinations/destination-sdk/file-based-destination-configuration.md) は現在非公開ベータ版で、一部の数のパートナーや顧客のみが利用できます。 機能と関連ドキュメントは、一般リリースの前に変更される場合があります。<br><br>この機能へのアクセス方法については、Adobeのアカウント担当者にお問い合わせください。 Adobe内部のアカウント担当者は、Experience Platformの宛先の製品およびエンジニアリングチームに連絡して、サポートされる使用例について話し合う必要があります。 <br><br> ファイルベースの宛先のDestination SDKサポートのベータ段階では、ベータパートナーおよびお客様は、 [Experience PlatformDestination SDK](/help/destinations/destination-sdk/overview.md) 非公開の宛先を構築して、次の機能を活用します。 <ul><li>Amazon S3、SFTP サーバー、Azure Blob、Azure Data Lake Storage、Data Landing Zone ストレージを使用して、ファイルベース（バッチ）の宛先を作成します。</li><li>デフォルトのファイル書き出しスケジュールおよび頻度オプションを設定および設定します。</li><li>書き出した CSV ファイルの形式（区切り文字、エスケープ文字、その他のオプション）を設定するオプションを設定および設定します。</li><li>カスタムファイルヘッダーを設定および編集する機能。</li><li>ファイルとセグメントの書き出しに関するイベント通知を受け取る機能。</li><li>CSV、TSV、JSON、Parquet など、追加のファイルタイプを書き出す機能。</li></ul>  <br>新機能の使用を開始するには、 [（ベータ版）Destination SDKを使用してファイルベースの宛先を設定する](../../destinations/destination-sdk/file-based-destination-configuration.md). <br><br> 非公開または製品化されたを作成する機能 *ストリーミング* Destination SDKを使用して宛先を設定する方法は、すべてのExperience Platformの顧客およびパートナーが既に利用できます。 方法に関するガイドを読む [Destination SDKを使用したストリーミング先の設定](/help/destinations/destination-sdk/configure-destination-instructions.md) 」を参照してください。 |

## [!DNL Identity Service] {#identity}

関連性のあるデジタルエクスペリエンスを提供するには、顧客を完全に理解しておく必要があります。顧客データが異なる複数のシステムに断片化されており、そのため各顧客が複数の「ID」を持つと考えられる場合、顧客を理解するのはさらに困難になります。

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