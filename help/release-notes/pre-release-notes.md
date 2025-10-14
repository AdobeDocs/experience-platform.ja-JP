---
title: Experience Platformのプレリリースノート
description: Adobe Experience Platformの最新のリリースノートのプレビュー。
exl-id: f2c41dc8-9255-4570-b459-4f9fc28ee58b
source-git-commit: 9cf809f8fd6e424b4dcd800c3d554e4eb0e337dc
workflow-type: tm+mt
source-wordcount: '944'
ht-degree: 16%

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

**リリース日：2025 年 10 月**

Adobe Experience Platformの既存の機能に対する新機能とアップデート：

- [アラート](#alerts)
- [宛先](#destinations)
- [セグメント化サービス](#segmentation-service)
- [ソース](#sources)

## アラート {#alerts}

Experience Platformでは、様々なExperience Platform アクティビティに関するイベントベースのアラートを登録できます。 Experience Platform ユーザーインターフェイスの「[!UICONTROL &#x200B; アラート &#x200B;]」タブを使用して、様々なアラートルールを購読し、UI 内またはメール通知を通じてアラートメッセージを受け取るように選択できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 宛先の失敗率アラート | 宛先に対して新しいアラートが追加されました：**宛先の失敗率がしきい値を超えています**。 このアラートは、データのアクティベーション中に失敗したレコードの数が許可されているしきい値を超えた場合に通知するので、アクティベーションの問題に迅速に対応できます。 |

{style="table-layout:auto"}

アラートについて詳しくは、[[!DNL Observability Insights]  概要 &#x200B;](../observability/home.md) を参照してください。

## 宛先 {#destinations}

Experience Platformから [!DNL Destinations] データの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| [!DNL AdForm] | この宛先を使用して、Adobe Real-Time CDP オーディエンスを [!DNL AdForm] に送信し、Experience Cloud ID （ECID）と [!DNL AdForm] の ID Fusion に基づいてアクティブ化します。 [!DNL AdForm] の ID Fusion は、Experience Cloud ID （ECID）に基づいてファーストパーティオーディエンスをアクティブ化できる ID 解決サービスです。 |
| `Amazon Ads` | `firstName`、`lastName`、`street`、`city`、`state`、`zip`、`country` などの個人識別子のサポートを追加しました。 これらのフィールドをターゲット ID としてマッピングすると、オーディエンスの一致率を向上させることができます。 |

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL AES256] 宛先での [!DNL Amazon S3] サーバーサイド暗号化のサポート | [!DNL Amazon S3] の宛先では、サーバーサイドの暗号化 [!DNL AES256] サポートするようになり、書き出したデータのセキュリティを強化します。 この暗号化方法は、[!DNL Amazon S3] 宛先接続を設定または更新する際に設定できます。この方法では、業界標準の [!DNL AES256] 暗号化アルゴリズムを使用して、保存時にデータが暗号化されます。 詳しくは、[[!DNL Amazon]  ドキュメント &#x200B;](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingEncryption.html) を参照してください。 |
| [&#x200B; オーディエンスレベルの監視をサポートする新しい宛先 &#x200B;](../dataflows/ui/monitor-destinations.md#audience-level-view) | 以下の宛先で、オーディエンスレベルの監視がサポートされるようになりました。 <ul><li>[!DNL Airship Tags]</li><li>（API） [!DNL Salesforce Marketing Cloud]</li><li>[!DNL Marketo Engage]</li><li>[!DNL Microsoft Bing]</li><li>（V1） [!DNL Pega CDH Realtime Audience]</li><li>（V2） [!DNL Pega CDH Realtime Audience]</li><li>[!DNL Salesforce Marketing Cloud] Account Engagement</li><li>[!DNL The Trade Desk]</li></ul> |
| データセット書き出しガードレールの修正 | データセット書き出しガードレールの修正が実装されました。 以前は、XDM エクスペリエンスイベントスキーマに基づい _タイムスタンプ列を含むが_ 含まない）一部のデータセットがエクスペリエンスイベントデータセットとして誤って扱われ、書き出しが 365 日のルックバックウィンドウに制限されていました。 ドキュメント化された 365 日間のルックバックガードレールは、エクスペリエンスイベントデータセットにのみ適用されるようになりました。 XDM エクスペリエンスイベントスキーマ以外のスキーマを使用するデータセットは、100 億レコードのガードレールで管理されるようになりました。 一部のお客様では、データセットの書き出し数が増加し、365 日間のルックバックウィンドウで誤って失敗する場合があります。 これにより、ルックバックウィンドウが長い予測ワークフローのデータセットを書き出すことができます。 詳しくは、[&#x200B; データセット書き出しガードレール &#x200B;](../destinations/guardrails.md#dataset-exports) を参照してください。 |
| エンタープライズ宛先に関するオーディエンスレベルのレポートの強化 | エンタープライズの宛先に関するオーディエンスレベルのレポートロジックの改善。 このリリース以降、選択した宛先に関連するオーディエンスのみを含んだ、より正確なオーディエンスレポート番号が表示されるようになります。 この監視の調整により、データフローでマッピングされたオーディエンスのみがレポートに含まれるようになり、実際のデータのアクティブ化に関するインサイトが明確になります。 これは、アクティブ化されるデータの量には影響しません。純粋に、レポートの精度を向上させるための監視機能の強化にすぎません。 |

詳しくは、[&#x200B; 宛先の概要 &#x200B;](../destinations/home.md) を参照してください。

## セグメント化サービス {#segmentation-service}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。オーディエンスは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて設定できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| ストリーミングセグメント化の監視 | ストリーミングセグメント化のリアルタイム監視は、サンドボックス、データセット、オーディエンスレベルでの評価率、待ち時間、データ品質指標に対する透明性を提供します。 これにより、プロアクティブなアラートと実用的なインサイトがサポートされ、データエンジニアが容量違反と取り込みの問題を特定するのに役立ちます。 モニタリング指標には、評価率、P95 取得待ち時間、受信、評価、失敗およびスキップされたレコードが含まれます。 データセット別の表示およびオーディエンス別の表示機能は、最終的に選定された新しいプロファイルと選定されなかったプロファイルを包括的に可視化します。 |

詳しくは、[[!DNL Segmentation Service] 概要](../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新規または更新されたソース**

| ソース | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} [!DNL Talon.one] ロイヤルティデータのソース | [!DNL Talon.One] のソースを使用して、ロイヤルティデータのバッチ取り込みとExperience Platformへのストリーミングを行います。 コネクタは、プロファイルデータ、トランザクションデータ、ロイヤルティデータ（獲得したポイント、交換されたポイント、期限切れのポイント、階層データを含む）のストリーミングをサポートしています。 |

**更新されたソース**

| ソース | 説明 |
| --- | --- |
| [!DNL Google Ads] ソースの一般提供（API のみ） | [!DNL Google Ads] ソースの API バージョンが一般提供されるようになりました。 API に関するドキュメントを更新し、最新バージョンが `v21` 新されたことを反映しました。また、Experience Platformでは v19 以降のすべてのバージョンをサポートしています。 UI バージョンはベータ版のままで、1 回限りの取り込みをサポートします。 増分データ取り込みを使用するには、API ルートを使用します。 |
| [!DNL Azure Event Hubs] 仮想ネットワークのサポート | Adobeは、Azure Event Hubs への仮想ネットワーク接続を明示的にサポートするようになり、パブリックネットワークではなくプライベートネットワーク経由でのデータ転送が可能になりました。 お客様は、*Experience Platform VNet を許可リストに加えるして、Azure プライベートバックボーンを通じて Event Hubs トラフィックを非公開でルーティングし、データ取得ワークフローのセキュリティとコンプライアンスを強化できます。 |

詳しくは、[ソースの概要](../sources/home.md)を参照してください。
