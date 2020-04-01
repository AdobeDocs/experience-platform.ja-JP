---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ取り込みイベント
topic: overview
translation-type: tm+mt
source-git-commit: 4817162fe2b7cbf4ae4c1ed325db2af31da5b5d3

---


# データ収集通知

データをAdobe Experience Platformに取り込むプロセスは、複数の手順で構成されます。 プラットフォームに取り込む必要のあるデータファイルを特定すると、取り込みプロセスが開始され、データが正常に取り込まれるか、または失敗するまで各手順が連続して実行されます。 インジェスト処理は、 [Adobe Experience Platform Data Ingestion APIを使用するか、Experience Platformのユーザーインターフェイスを使用して](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) 、開始することができます。

プラットフォームに読み込まれたデータは、目的のデータレークまたはリアルタイム顧客データストアに到達するために、複数の手順を踏む必要があります。 各手順では、データの処理、データの検証、データの保存を行い、データを次の手順に渡します。 取り込まれるデータの量に応じて、この処理に時間がかかる場合があり、検証、セマンティック、または処理エラーが原因でプロセスが失敗する可能性が常にあります。 障害がイベントした場合は、データの問題を修正し、修正したデータファイルを使用して取り込みプロセス全体を再開する必要があります。

インジェストプロセスの監視を支援するため、Experience Platformでは、プロセスの各手順で公開された一連のイベントをサブスクライブし、取り込んだデータのステータスや発生した可能性のあるエラーを通知できます。

## 使用可能なステータス通知イベント

以下に、サブスクライブできるリスト取り込みステータス通知を示します。

>[!NOTE] すべてのデータ取り込み通知に対して1つのイベントトピックのみが提供されています。 異なるステータスを区別するために、イベントコードを使用できます。

| プラットフォームサービス | Status | イベントの説明 | イベントコード |
| ---------------- | ------ | ----------------- | ---------- |
| データランディング | success | 取り込み — バッチが成功しました | ing_load_success |
| データランディング | 失敗 | 取り込み — バッチが失敗しました | ing_load_failure |
| リアルタイム顧客プロファイル | success | プロファイルサービス — データの読み込みバッチが成功しました | ps_load_success |
| リアルタイム顧客プロファイル | 失敗 | プロファイルサービス — データの読み込みバッチが失敗しました | ps_load_failure |
| アイデンティティグラフ | success | IDグラフ — データの読み込みバッチが成功しました | ig_load_success |
| アイデンティティグラフ | 失敗 | IDグラフ — データの読み込みバッチが失敗しました | ig_load_failure |

## 通知ペイロードスキーマ

データ取り込み通知イベントスキーマは、取り込まれるデータのステータスに関する詳細を提供するフィールドと値を含むExperience Data Model(XDM)スキーマです。 最新の通知ペイロードの表示を行うには、パブリックXDM GitHubレポートを [参照してください](https://github.com/adobe/xdm/blob/master/schemas/common/notifications/ingestion.schema.json)。

## データ取り込みステータス通知のサブスクライブ

Adobe I/O [イベントを通じ](https://www.adobe.io/apis/experienceplatform/events.html)、Webフックを使用して複数の通知タイプをサブスクライブできます。 Webhookの詳細およびWebhookを使用したAdobe I/Oイベントのサブスクライブ方法については、『Adobe I/Oイベントの概要』 [Webhooksガイドを参照してください](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/intro/webhook_docs_intro.md) 。

### Adobe I/Oコンソールを使用した新しい統合の作成

[Adobe I/O Consoleにサインインし、「統合」タブをクリックするか、「クイッ](https://console.adobe.io/home) ク開始 ****** 」の「統合を作成」をクリックします。 統合画面が表 *示されたら* 、「新しい統 **合」をクリックして** 、新しい統合を作成します。

![新しい統合の作成](../images/quality/subscribe-events/create_integration_start.png)

[新しい *統合の作成* ]画面が表示されます。 「ニア **リアルタイムイベントを受信**」を選択し、「続行」をクリ **ックします**。

![ほぼリアルタイムのイベント](../images/quality/subscribe-events/create_integration_receive_events.png)

次の画面には、購読、権限、権限に基づいて、組織が使用できる様々なイベント、製品、サービスを使用した統合を作成するオプションが表示されます。 この統合の場合、「エクスペリエンスプ **ラットフォーム** 」で「プラットフォーム通知」を選択し、「続行」をクリ **ックしま**&#x200B;す。

![イベントプロバイダ](../images/quality/subscribe-events/create_integration_select_provider.png)

統合 *の詳細* ・フォームが表示され、統合の名前と説明、および公開鍵証明書を入力する必要があります。

公開証明書がない場合は、次のコマンドを使用してターミナルで証明書を生成できます。

```shell
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout private.key -out certificate_pub
```

証明書を生成したら、「 **Public keys certificates** 」ボックスにファイルをドラッグ&amp;ドロップするか、「 **Select a File** 」をクリックしてファイルディレクトリを参照し、証明書を直接選択します。

証明書を追加すると、「登録」オプ *ションが* イベントされます。 「 **追加イベント登録」をクリックします**。

![統合の詳細](../images/quality/subscribe-events/create_integration_details.png)

[ *イベント登録の詳細* ]ダイアログが展開し、追加のコントロールが表示されます。 ここでは、目的のオプションを選択し、Webイベントタイプを登録できます。 イベント登録の名前、WebフックURL *（オプション）*、簡単な説明を入力します。 最後に、購読するイベントタイプ（データ取り込み通知）を選択し、「保存」をクリック **します**。

![選択イベント](../images/quality/subscribe-events/create_integration_select_event.png)

## 次の手順

I/O統合を作成したら、その統合に関する受信通知を表示できます。 イベントのトレース方 [法の詳細については、『](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/support/tracing.md) Adobe I/Oイベントのトレース』ガイドを参照してください。
