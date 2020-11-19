---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年8月11日）
doc-type: release notes
last-update: August 10, 2020
author: crhoades, ens28527
translation-type: tm+mt
source-git-commit: 0232acdc64019b9d93888e8137ef9bc8e114779b
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 23%

---


# Adobe Experience Platform リリースノート

**リリース日：2020 年 12 月 8 日**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Destinations]](#destinations)
- [[!DNL Real-time Customer Data Platform]](#rtcdp)
- [[!DNL Sources]](#sources)

## [!DNL Data Science Workspace] {#dsw}

[!DNL Data Science Workspace] 機械学習と人工知能を使用して、データから洞察を引き出します。 Integrated into Adobe Experience Platform, [!DNL Data Science Workspace] helps you make predictions using your content and data assets across Adobe solutions.

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| VMの強化 [!DNL JupyterLab] | 長時間稼働する [!DNL JupyterLab notebook] 仮想マシンの安定性が向上しました。 |

詳細については、『 [!DNL JupyterLab]ユーザガイド [[!DNL JupyterLab] ](../../data-science-workspace/jupyterlab/overview.md)』を参照してください。

## 宛先 {#destinations}

In [Real-time Customer Data Platform](../../rtcdp/overview.md), destinations are pre-built integrations with destination platforms that activate data to those partners in a seamless way.

**新しい宛先**

新しい宛先は、Adobe Experience Platform データをアクティブ化する際に使用できます。詳しくは、以下を参照してください。

| 宛先 | 説明 |
|--- | ---|
| [!DNL Google Customer Match] | Google Customer Matchでは、オンラインおよびオフラインのデータを使用して、Googleが所有し、運営する次のようなプロパティを通じて顧客に連絡し、顧客と再び関与させることができます。 [!DNL Search]、 [!DNL Shopping]、Gmail、YouTubeの3つを追加しました。 <br><br> 宛先カタログの [!DNL Google Customer Match][ページにアクセスし](/help/rtcdp/destinations/google-customer-match-destination.md) 、AdobeReal-time CDPでの宛先とその設定方法の詳細を確認します。 |

**新機能**

| 機能 | 説明 |
|------- | -----------|
| カスタムファイル名エディタ | 電子メールマーケティングの宛先と、エクスポートされたファイルの名前を編集できるクラウドストレージの宛先に関するデータアクティベーションワークフローに更新します。 詳しくは、アクティベーションワークフローの [ 設定手順](/help/rtcdp/destinations/activate-destinations.md#configure) を参照してください。 |
| 推奨属性 | 電子メールマーケティングの宛先および推奨属性を表示するクラウドのストレージ先のデータアクティベーションワークフローを更新し、書き出されたファイルに追加します。 詳しくは、アクティベーションワークフローの「属性を [選択](/help/rtcdp/destinations/activate-destinations.md#select-attributes) 」の手順を参照してください。 |

## [!DNL Real-time Customer Data Platform] {#rtcdp}

Built on Experience Platform, Real-time Customer Data Platform ([!DNL Real-time CDP]) helps companies bring together known and unknown data to activate customer profiles with intelligent decisioning throughout the customer journey. [!DNL Real-time CDP] 複数のエンタープライズデータソースを組み合わせて、顧客プロファイルをリアルタイムで作成します。 これらのプロファイルから作成されたセグメントは、その後、すべてのチャネルとデバイスで1対1のパーソナライズ顧客エクスペリエンスを提供するために、ダウンストリームの宛先に送信できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| （IAB TCF 2.0のサポート） | [!DNL Real-time CDP] は、 [!DNL Transparency & Consent Framework] (IAB)で概要を説明しているように、2.0バージョンの [!DNL Interactive Advertising Bureau] (TCF)に登録されたベンダーになりました。 CMPによって生成された顧客の同意データを受け入れるようにデータ操作やプロファイルスキーマを設定し、下流の宛先に対するセグメントをアクティブ化する際に、顧客の同意を得るための好みを強制できます。 詳細は、Real-time CDPの [IAB TCF 2.0サポートの概要を参照してください](../../rtcdp/privacy/iab/overview.md) 。 |

For more information on [!DNL Real-time CDP], see the [[!DNL Real-time CDP] overview](../../rtcdp/overview.md).

## ソース {#sources}

Adobe Experience Platform can ingest data from external sources while allowing you to structure, label, and enhance that data using [!DNL Platform] services. アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できるようにする RESTful API とインタラクティブな UI を提供します。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| フロー実行の監視 | すべてのフローの実行を監視し、完了ステータス、実行期間、処理されたファイルのリスト、エラー、指標など、各実行の詳細表示を確認できます。 詳細は、 [監視データフロー](../../sources/tutorials/ui/monitor.md) ドキュメントを参照してください。 |
| フロー実行通知 | イベントを登録し、Webフックを登録して、フローの実行に関するステータス、指標、エラーに関するリアルタイム通知を受信できます。 |
| UIカタログの改善 | ソースカタログ画面が更新され、選択したオブジェクトの主なアクションに簡単にアクセスできるようになりました。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。