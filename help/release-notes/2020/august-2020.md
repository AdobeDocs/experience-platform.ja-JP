---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年8月11日）
doc-type: release notes
last-update: August 10, 2020
author: crhoades, ens28527
translation-type: tm+mt
source-git-commit: 49c984a60fd699706eec508ec1d786340df40b57
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 24%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 12 月 8 日**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Destinations]](#destinations)
- [[!DNL Real-time Customer Data Platform]](#rtcdp)
- [[!DNL Sources]](#sources)

## [!DNL Data Science Workspace] {#dsw}

[!DNL Data Science Workspace] 機械学習と人工知能を使用して、データから洞察を引き出します。[!DNL Data Science Workspace]は、Adobe Experience Platformに統合され、Adobeソリューション全体でコンテンツやデータを使用して予測を行うのに役立ちます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| [!DNL JupyterLab]でのVMの改善 | 長時間稼働する[!DNL JupyterLab notebook]仮想マシンの安定性が向上しました。 |

[!DNL JupyterLab]の詳細については、[[!DNL JupyterLab] ユーザーガイド](../../data-science-workspace/jupyterlab/overview.md)を参照してください。

## 宛先 {#destinations}

[リアルタイム顧客データプラットフォーム](../../rtcdp/overview.md)では、宛先は、それらのパートナーに対してシームレスにデータをアクティブ化する目的のプラットフォームとの事前に構築された統合です。

**新しい宛先**

新しい宛先は、Adobe Experience Platform データをアクティブ化する際に使用できます。詳しくは、以下を参照してください。

| 宛先 | 説明 |
|--- | ---|
| [!DNL Google Customer Match] | Google Customer Matchでは、オンラインおよびオフラインのデータを使用して、Googleが所有し、運営する次のようなプロパティを通じて顧客に連絡し、顧客と再び関与させることができます。[!DNL Search]、[!DNL Shopping]、Gmail、YouTube。 <br><br> 宛先カタログ内の [!DNL Google Customer Match] [](../../destinations/catalog/advertising/google-customer-match.md) ページを参照し、リアルタイムCDPでの宛先の設定方法の詳細を確認してください。 |

**新機能**

| 機能 | 説明 |
|------- | -----------|
| カスタムファイル名エディタ | 電子メールマーケティングの宛先と、エクスポートされたファイルの名前を編集できるクラウドストレージの宛先に関するデータアクティベーションワークフローに更新します。 詳しくは、アクティベーションワークフローの[設定手順](../../destinations/ui/activate-destinations.md#configure)を参照してください。 |
| 推奨属性 | 電子メールマーケティングの宛先および推奨属性を表示するクラウドのストレージ先のデータアクティベーションワークフローを更新し、書き出されたファイルに追加します。 詳しくは、アクティベーションワークフローの[属性を選択手順](../../destinations/ui/activate-destinations.md#select-attributes)を参照してください。 |

## [!DNL Real-time Customer Data Platform] {#rtcdp}

Experience Platformに基づくリアルタイム顧客データプラットフォーム([!DNL Real-time CDP])は、会社が既知の未知のデータを集め、顧客ジャーニー全体でインテリジェントな判定を行う顧客プロファイルをアクティブにするのに役立ちます。 [!DNL Real-time CDP] 複数のエンタープライズデータソースを組み合わせて、顧客プロファイルをリアルタイムで作成します。これらのプロファイルから作成されたセグメントは、その後、すべてのチャネルとデバイスで1対1のパーソナライズ顧客エクスペリエンスを提供するために、ダウンストリームの宛先に送信できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| （IAB TCF 2.0のサポート） | [!DNL Real-time CDP] は、 [!DNL Transparency & Consent Framework] (IAB)で概要を説明しているように、2.0バージョンの [!DNL Interactive Advertising Bureau] (TCF)に登録されたベンダーになりました。CMPによって生成された顧客の同意データを受け入れるようにデータ操作やプロファイルスキーマを設定し、下流の宛先に対するセグメントをアクティブ化する際に、顧客の同意を得るための好みを強制できます。 |

[!DNL Real-time CDP]について詳しくは、[[!DNL Real-time CDP] 概要](../../rtcdp/overview.md)を参照してください。

## ソース {#sources}

Adobe Experience Platformは外部ソースからデータを取り込みながら、[!DNL Platform]サービスを使ってデータの構造、ラベル付け、拡張を行うことができます。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| フロー実行の監視 | すべてのフローの実行を監視し、完了ステータス、実行期間、処理されたファイルのリスト、エラー、指標など、各実行の詳細表示を確認できます。 詳しくは、[監視データフロー](../../sources/tutorials/ui/monitor.md)ドキュメントを参照してください。 |
| フロー実行通知 | イベントを登録し、Webフックを登録して、フローの実行に関するステータス、指標、エラーに関するリアルタイム通知を受信できます。 |
| UIカタログの改善 | ソースカタログ画面が更新され、選択したオブジェクトの主なアクションに簡単にアクセスできるようになりました。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。