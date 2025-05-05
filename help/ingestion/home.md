---
title: データ取り込みの概要
description: このドキュメントでは、データをExperience Platformに取り込む 3 つの主な方法を紹介し、詳細については、それぞれの概要ドキュメントへのリンクを示します。
exl-id: c189dd4a-5c59-4189-a18c-a3e45a9ff01d
source-git-commit: 0e484dffa38d454561f9d67c6bea92f426d3515d
workflow-type: tm+mt
source-wordcount: '1157'
ht-degree: 14%

---

# データ取り込みの概要

Adobe Experience Platformでは、データ取得とは、様々なソースからストレージメディアにデータを転送し、そこで組織がアクセス、使用、分析できるようにすることです。 Experience Platformのデータ取り込みは、**ストリーミング取り込み** と **バッチ取り込み** の 2 つの主なカテゴリにグループ化できます。

ストリーミングとバッチの取り込みでは、データをExperience Platformに取り込むために使用できる様々な方法があります。 例えば、様々な **ソース** を使用し、これらのソースに接続してデータをExperience Platformに取り込みます。

データをExperience Platformに取り込む様々な方法の概要については、このドキュメントを参照してください。

<!-- Adobe Experience Platform brings data from multiple sources together in order to help marketers better understand the behavior of their customers. Adobe Experience Platform Data Ingestion represents the multiple methods by which Experience Platform ingests data from these sources, as well as how that data is persisted within the Data Lake for use by downstream Experience Platform services. -->

## ストリーミング取り込み {#streaming}

ストリーミング取得を使用して、クライアントおよびサーバーサイドデバイスから、リアルタイムでExperience Platformにデータを送信できます。 Experience Platformでは、受信したエクスペリエンスデータをストリーミングするためのデータインレットの使用をサポートしています。このデータインレットは、データレイク内のストリーミング対応データセットで保持されます。 データインレットは、収集したデータを自動的に認証するように設定でき、信頼できるソースからのデータであることを確認できます。

詳しくは、[ ストリーミング取得の概要 ](./streaming-ingestion/overview.md) を参照してください。

## バッチ取得 {#batch}

Experience Platformでは、バッチは、一定期間にわたって収集された一連のデータで、1 つのユニットとして一緒に処理されます。 データセットは、バッチで構成されます。 バッチ取り込みを使用して、データをバッチファイルとしてExperience Platformに取り込むことができます。 取り込まれたバッチには、正常に取り込まれたレコードの数と、失敗したレコードの数、および関連するエラーメッセージを示すメタデータが提供されます。

フラットな CSV ファイル（XDM スキーマにマッピングされたもの）や parquet ファイルなど、手動でアップロードされたデータファイルは、この方法を使用して取り込む必要があります。

詳しくは、[ バッチ取得の概要 ](./batch-ingestion/overview.md) を参照してください。

## ソース {#sources}

Experience Platform Sources に接続してデータを取り込むこともできます。 Experience Platformには、接続してデータを取り込むことができる様々なデータソースのカタログが用意されています。 これらのソースには、Adobe Analytics ソースやMarketo Engage ソースなどのネイティブのAdobe アプリケーションを使用できます。 [!DNL Amazon S3] ソースや [!DNL Google Cloud Storage] ソースなど、サードパーティのソースに接続することもできます。

ソースは、クラウドストレージ、データベース、CRM システムなど、様々なカテゴリにグループ化されます。 特定のソースは、バッチまたはストリーミング取得をサポートする場合があります。

ソースを使用すると、様々なデータソースや、様々なユースケースカテゴリからデータを取り込むことができます。 さらに、ソースを介したデータ取り込みでは、外部データソースに対して認証を行い、取り込みスケジュールを設定し、取り込みスループットを管理することができます。

詳しくは、[ ソースの概要 ](../sources/home.md) を参照してください。

### ML 支援スキーマの作成 {#ml-assisted-schema-creation}

新しいデータソースをすばやく統合するために、機械学習アルゴリズムを使用して、サンプルデータからスキーマを生成できるようになりました。 この自動化により、正確なスキーマの作成が簡素化され、エラーが減り、データ収集から分析およびインサイトに至るプロセスが迅速化されます。

このワークフローについて詳しくは [&#128279;](../xdm/ui/ml-assisted-schema-creation.md) ML-ASSISTED schema creation guide」を参照してください。

## データ準備 {#data-prep}

データ準備は取り込み方法ではありませんが、データ取り込みプロセスの重要な部分です。 データフローを作成してデータをExperience Platformに取り込む前に、データ準備関数を使用して、エクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換および検証を行います。 データ準備は、データ取得プロセス中にExperience Platform ユーザーインターフェイスで「マッピング」手順として表示されます。

詳しくは、[ データ準備の概要 ](../data-prep/home.md) を参照してください。

## ストリーミング取り込み方法 {#streaming-ingestion-methods}

次の表に、ストリーミングデータをExperience Platformに取り込む際に使用できる様々な方法の概要を示します。

<table cellspacing="0" class="Table" style="border-collapse:collapse; width:1123px">
<tbody>
<tr>
<td colspan="4" style="background-color:#308fff; border-bottom:4px solid white; border-left:1px solid white; border-right:1px solid white; border-top:1px solid white; height:31px; vertical-align:top; width:1123px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><strong><span style="color:black">ストリーミングソース</span></strong></span></span></p>
</td>
</tr>
<tr>
<td style="background-color:#969696; border-bottom:1px solid white; border-left:1px solid white; border-right:1px solid white; border-top:none; height:20px; vertical-align:top; width:222px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">メソッド</span></span></span></p>
</td>
<td style="background-color:#969696; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:20px; vertical-align:top; width:401px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">一般的なユースケース</span></span></span></p>
</td>
<td style="background-color:#969696; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:20px; vertical-align:top; width:218px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">プロトコル</span></span></span></p>
</td>
<td style="background-color:#969696; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:20px; vertical-align:top; width:282px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">注意点</span></span></span></p>
</td>
</tr>
<tr>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:1px solid white; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:222px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black"><a href="https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=ja" style="color:#0563c1; text-decoration:underline">Adobe Web/モバイルSDK</a></span></span></span></p>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:401px">
<ul style="list-style-type:square">
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">Web サイトやモバイルアプリからのデータ収集。</span></span></span></li>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">クライアントサイドの収集に推奨される方法。</span></span></span></li>
</ul>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:218px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">プッシュ，HTTP, JSON</span></span></span></p>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:282px">
<ul>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">1 つのSDKを活用して複数のAdobe アプリケーションを実装します。</span></span></span></li>
</ul>
</td>
</tr>
<tr>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:1px solid white; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:222px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black"><a href="https://experienceleague.adobe.com/ja/docs/experience-platform/sources/connectors/streaming/http" style="color:#0563c1; text-decoration:underline">HTTP API コネクタ</a></span></span></span></p>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:401px">
<ul style="list-style-type:square">
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">ストリーミングソース、トランザクション、関連する顧客イベントおよびシグナルからの収集。</span></span></span></li>
</ul>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:218px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">プッシュ、REST API、JSON</span></span></span></p>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:282px">
<ul>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">生データまたは XDM データは、リアルタイムのEdge セグメント化やイベント転送を使用せずに、ハブに直接ストリーミングされます。</span></span></span></li>
</ul>
</td>
</tr>
<tr>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:1px solid white; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:222px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black"><a href="https://experienceleague.adobe.com/docs/experience-platform/edge-network-server-api/data-collection/interactive-data-collection.html?lang=ja" style="color:#0563c1; text-decoration:underline">[!DNL Edge Network] API</a></span></span></span></p>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:401px">
<ul style="list-style-type:square">
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">ストリーミングソース、トランザクション、関連する顧客イベント、およびグローバルに分散した [!DNL Edge Network] からのシグナルからの収集。</span></span></span></li>
</ul>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:218px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">プッシュ、REST API、JSON</span></span></span></p>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:282px">
<ul>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">データは [!DNL Edge Network] を介してストリーミングされます。 Edgeでのリアルタイムのセグメント化とイベント転送のサポート。 </span></span></span></li>
</ul>
</td>
</tr>
<tr>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:1px solid white; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:222px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black"><a href="https://experienceleague.adobe.com/docs/experience-platform/sources/connectors/adobe-applications/analytics.html?lang=ja" style="color:#0563c1; text-decoration:underline">アドビアプリケーション</a></span></span></span></p>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:401px">
<ul style="list-style-type:square">
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">Adobe Analytics、Marketo Engage、Adobe Campaign Managed Services、Adobe Target、Adobe Audience Managerなどのアプリケーションからのデータ取得</span></span></span></li>
</ul>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:218px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">プッシュ、Source コネクタ、API</span></span></span></p>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:282px">
<ul>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">推奨されるアプローチは、従来のアプリケーション SDK を使用する代わりに、Web/Mobile SDKに移行することです。</span></span></span></li>
</ul>
</td>
</tr>
<tr>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:1px solid white; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:222px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black"><a href="https://experienceleague.adobe.com/docs/experience-platform/ingestion/streaming/overview.html?lang=ja" style="color:#0563c1; text-decoration:underline">ストリーミングソース</a></span></span></span></p>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:401px">
<ul style="list-style-type:square">
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">エンタープライズイベントストリームの取り込み。通常、複数のダウンストリームアプリケーションでエンタープライズデータを共有するために使用します。 </span></span></span></li>
</ul>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:218px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">プッシュ、REST API、JSON</span></span></span></p>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:282px">
<ul>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">データは JSON 形式でストリーミングされ、XDM スキーマにマッピングできます。</span></span></span></li>
</ul>
</td>
</tr>
<tr>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:1px solid white; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:222px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black"><a href="https://experienceleague.adobe.com/ja/docs/experience-platform/sources/sdk/streaming-sdk/getting-started" style="color:#0563c1; text-decoration:underline">ストリーミングソースSDK</span></span></span></p>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:401px">
<ul style="list-style-type:square">
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">SDKをストリーミングするセルフサービスソースのセルフサービス機能を使用して、独自のデータソースをExperience Platform ソースカタログに統合します。</span></span></span></li>
</ul>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:218px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">プッシュ，HTTP API, JSON</span></span></span></p>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:39px; vertical-align:top; width:282px">
<ul>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">パートナー統合ストリーミングソースの例としては、Braze、Pendo、RainFocus があります。</span></span></span></li>
</ul>
</td>
</tr>
</tbody>
</table>

## バッチ取り込み方法 {#batch-ingestion-methods}

次の表に、バッチデータをExperience Platformに取り込むために使用できる様々な方法の概要を示します。

<table cellspacing="0" class="Table" style="border-collapse:collapse; width:1105px">
<tbody>
<tr>
<td colspan="4" style="background-color:#308fff; border-bottom:4px solid white; border-left:1px solid white; border-right:1px solid white; border-top:1px solid white; height:37px; vertical-align:top; width:1105px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><strong><span style="color:black">バッチソース</span></strong></span></span></p>
</td>
</tr>
<tr>
<td style="background-color:#969696; border-bottom:1px solid white; border-left:1px solid white; border-right:1px solid white; border-top:none; height:34px; vertical-align:top; width:217px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">メソッド</span></span></span></p>
</td>
<td style="background-color:#969696; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:34px; vertical-align:top; width:397px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">一般的なユースケース</span></span></span></p>
</td>
<td style="background-color:#969696; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:34px; vertical-align:top; width:215px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">プロトコル</span></span></span></p>
</td>
<td style="background-color:#969696; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:34px; vertical-align:top; width:277px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">注意点</span></span></span></p>
</td>
</tr>
<tr>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:1px solid white; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:217px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black"><a href="https://experienceleague.adobe.com/ja/docs/experience-platform/ingestion/batch/overview" style="color:#0563c1; text-decoration:underline">バッチ取得 API</a></span></span></span></p>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:397px">
<ul style="list-style-type:square">
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">エンタープライズ管理キューからの取得。 取り込み前にデータを準備およびフォーマットする必要がある場合は、バッチ取り込みを使用します。</span></span></span></li>
</ul>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:215px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">プッシュ、JSON または Parquet</span></span></span></p>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:277px">
<ul>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">取り込むバッチとファイルを管理する必要があります。</span></span></span></li>
</ul>
</td>
</tr>
<tr>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:1px solid white; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:217px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black"><a href="https://experienceleague.adobe.com/ja/docs/experience-platform/sources/home" style="color:#0563c1; text-decoration:underline">バッチソース</a></span></span></span></p>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:397px">
<ul style="list-style-type:square">
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">クラウドストレージ、CRM、マーケティング自動化アプリケーションからデータを取り込むための一般的なアプローチ。</span></span></span></li>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">大量の履歴データの取り込みに最適です。</span></span></span></li>
</ul>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:215px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">プル、CSV、JSON、Parquet</span></span></span></p>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:277px">
<ul>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">事前設定されたスケジュールされた間隔に基づいてSourceを取得します。</span></span></span></li>
</ul>
</td>
</tr>
<tr>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:1px solid white; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:217px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black"><a href="https://experienceleague.adobe.com/docs/experience-platform/sources/connectors/cloud-storage/data-landing-zone.html?lang=ja" style="color:#0563c1; text-decoration:underline">Data Landing Zone</a></span></span></span></p>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:397px">
<ul style="list-style-type:square">
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">Adobeがプロビジョニングしたクラウドベースのファイルストレージ。 サンドボックスごとに 1 つのデータランディングゾーンコンテナにアクセスできます。</span></span></span></li>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">後でExperience Platformに取り込むために、ファイルをデータランディングゾーンにプッシュします。</span></span></span></li>
</ul>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:215px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">プッシュ，CSV, JSON, Parquet</span></span></span></p>
</td>
<td style="background-color:#e8eeff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:277px">
<ul>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">Experience Platformでは、データランディングゾーンコンテナにアップロードされるすべてのファイルおよびフォルダーに対して、厳密に 7 日間の有効期限が適用されます。 すべてのファイルとフォルダーは、7 日後に削除されます。</span></span></span></li>
</ul>
</tr>
<tr>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:1px solid white; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:217px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black"><a href="https://experienceleague.adobe.com/docs/experience-platform/sources/sdk/overview.html?lang=ja" style="color:#0563c1; text-decoration:underline">バッチソースSDK</a></span></span></span></p>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:397px">
<ul style="list-style-type:square">
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">セルフサービスソースバッチ SDKのセルフサービス機能を使用して、独自のデータソースをExperience Platform ソースカタログに統合します。</span></span></span></li>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">パートナーコネクタまたはエンタープライズコネクタの設定に合わせたワークフローエクスペリエンスに最適です。</span></span></span></li>
</ul>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:215px">
<p><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">プル、REST API、CSV または JSON</span></span></span></p>
</td>
<td style="background-color:#cddbff; border-bottom:1px solid white; border-left:none; border-right:1px solid white; border-top:none; height:62px; vertical-align:top; width:277px">
<ul>
<li><span style="font-size:12pt"><span style="font-family:Calibri,sans-serif"><span style="color:black">パートナー統合バッチソースの例としては、Mailchimp、OneTrust、Zendesk などがあります</span></span></span></li>
</ul>

<p> </p>
</td>
</tr>
</tbody>
</table>

## 次の手順とその他のリソース

このドキュメントでは、[!DNL Experience Platform] での [!DNL Data Ingestion] の様々な側面について簡単に説明しました。各取り込み方法の概要ドキュメントを引き続き参照して、それぞれの機能、ユースケース、ベストプラクティスをよく理解してください。また、次の取り込みの概要ビデオを見ることで、理解を補うこともできます。取り込んだレコードのメタデータを [!DNL Experience Platform] で追跡する方法について詳しくは、[Catalog Service の概要](../catalog/home.md) を参照してください。

>[!WARNING]
>
>次のビデオで使用されている「統合プロファイル」という用語は、今は使われていません。[!DNL "Profile"]または[!DNL "Real-Time Customer Profile"]という用語は、[!DNL Experience Platform] のドキュメントで使用されている正しい用語です。最新の機能については、ドキュメントを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/27106?quality=12&learn=on)
