---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ取り込みイベントのサブスクライブ
topic: overview
translation-type: tm+mt
source-git-commit: 4817162fe2b7cbf4ae4c1ed325db2af31da5b5d3
workflow-type: tm+mt
source-wordcount: '768'
ht-degree: 1%

---


# データ取り込み通知

データをAdobe Experience Platformに取り込むプロセスは、複数の手順で構成されます。 プラットフォームに取り込む必要のあるデータファイルを特定したら、取り込みプロセスが開始され、データが正常に取り込まれるか、または失敗するまで各手順が連続して行われます。 インジェスト処理は、 [Adobe Experience Platform Data Ingest API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) 、またはExperience Platformユーザーインターフェイスを使用して開始できます。

プラットフォームに読み込まれたデータは、目的のデータレーク(Data Lake)またはリアルタイム顧客プロファイルデータストアに到達するために、複数の手順を経る必要があります。 各手順では、データの処理、データの検証、データの格納、データの次の手順への渡しを行います。 取り込まれるデータ量に応じて、この処理は時間がかかる場合があり、検証、セマンティック、または処理エラーが原因でプロセスが失敗する可能性が常にあります。 エラーがイベントした場合は、データの問題を修正し、修正したデータファイルを使用してインジェストプロセス全体を再起動する必要があります。

インジェストプロセスの監視を支援するため、Experience Platformでは、プロセスの各手順で公開された一連のイベントに登録し、取り込まれたデータのステータスと発生する可能性のあるエラーを通知できます。

## 使用可能な状態通知イベント

以下に、サブスクライブ可能なデータ取り込みステータス通知のリストを示します。

>[!NOTE] すべてのデータ取り込み通知に対して1つのイベントトピックのみが提供されます。 異なるステータスを区別するために、イベントコードを使用できます。

| プラットフォームサービス | Status | イベントの説明 | イベントコード |
| ---------------- | ------ | ----------------- | ---------- |
| データのランディング | success | 取り込み — バッチが成功しました | ing_load_success |
| データのランディング | 失敗 | 取り込み — バッチが失敗しました | ing_load_failure |
| リアルタイム顧客プロファイル | success | プロファイルサービス — データ読み込みバッチが成功しました | ps_load_success |
| リアルタイム顧客プロファイル | 失敗 | プロファイルサービス — データ読み込みバッチが失敗しました | ps_load_failure |
| 識別グラフ | success | IDグラフ — データ読み込みバッチが成功しました | ig_load_success |
| 識別グラフ | 失敗 | IDグラフ — データの読み込みバッチに失敗しました | ig_load_failure |

## 通知ペイロードスキーマ

データ取り込み通知イベントスキーマは、取り込まれるデータのステータスに関する詳細を提供するフィールドと値を含むExperience Data Model(XDM)スキーマです。 最新の [通知ペイロードスキーマを表示するには、パブリックXDM GitHubリポジトリにアクセスしてください](https://github.com/adobe/xdm/blob/master/schemas/common/notifications/ingestion.schema.json)。

## データ取り込みステータス通知のサブスクライブ

Adobe I/O [イベントを通じて](https://www.adobe.io/apis/experienceplatform/events.html)、Webフックを使用して複数の通知タイプをサブスクライブできます。 Webhookの詳細およびWebhookを使用したAdobe I/Oイベントの登録方法については、『Adobe I/Oイベントの [概要』の「Webhook](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/intro/webhook_docs_intro.md) 」ガイドを参照してください。

### Adobe I/Oコンソールを使用した新しい統合の作成

[Adobe I/O Consoleにサインインし、「](https://console.adobe.io/home) Integrations *（統合）」タブをクリックするか、「Quick Integration(クイック開始)* 」の「Create integration **** （統合を作成）」をクリックします。 「 *統合* 」画面が表示されたら、「 **新しい統合** 」をクリックして新しい統合を作成します。

![新しい統合の作成](../images/quality/subscribe-events/create_integration_start.png)

[新しい統合の *作成* ]画面が表示されます。 「近いリアルタイムイベントを **受信する**」を選択し、「 **続行**」をクリックします。

![ほぼリアルタイムのイベントを受信](../images/quality/subscribe-events/create_integration_receive_events.png)

次の画面には、購読、権限および権限に基づいて、組織が使用できる様々なイベント、製品およびサービスとの統合を作成するオプションが表示されます。 この統合の場合、「エクスペリエンスプラットフォーム **」の下にある「** プラットフォーム通知 **」を選択し、「**&#x200B;続行」をクリックします。

![イベントプロバイダーの選択](../images/quality/subscribe-events/create_integration_select_provider.png)

「 *統合の詳細* 」フォームが表示され、統合の名前と説明、および公開鍵証明書を入力する必要があります。

公開証明書がない場合は、次のコマンドを使用してターミナルで証明書を生成できます。

```shell
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate_pub
```

証明書を生成したら、ファイルを「 **公開鍵証明書** 」ボックスにドラッグ&amp;ドロップするか、「ファイルを **** 選択」をクリックしてファイルディレクトリを参照し、証明書を直接選択します。

証明書を追加すると、「 *イベント登録* 」オプションが表示されます。 「 **イベント登録**」をクリックします。

![統合の詳細](../images/quality/subscribe-events/create_integration_details.png)

[ *イベント登録の詳細* ]ダイアログボックスが展開し、追加のコントロールが表示されます。 ここでは、目的のイベントタイプを選択して、Webフックを登録できます。 イベント登録の名前、WebフックURL *（オプション）*、簡単な説明を入力します。 最後に、登録するイベントタイプ（データ取り込み通知）を選択し、「 **保存**」をクリックします。

![イベントの選択](../images/quality/subscribe-events/create_integration_select_event.png)

## 次の手順

I/O統合を作成すると、その統合に関して受信した通知を表示できます。 イベントをトレースする方法の詳細な手順については、『 [Adobe I/Oイベントのトレース](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/support/tracing.md) 』ガイドを参照してください。
