---
title: Adobe Experience Platform リリースノート 2020年8月
description: Adobe Experience Platform の 2020年8月のリリースノート。
doc-type: release notes
last-update: August 10, 2020
author: crhoades, ens28527
exl-id: 9347147f-e830-4487-aa12-f56723abb3c8
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '576'
ht-degree: 38%

---

# Adobe Experience Platform リリースノート

**リリース日：2020年8月12日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Destinations]](#destinations)
- [[!DNL Real-Time Customer Data Platform]](#rtcdp)
- [[!DNL Sources]](#sources)

## [!DNL Data Science Workspace] {#dsw}

[!DNL Data Science Workspace] は機械学習と人工知能を使用して、データからインサイトを引き出します。 Adobe Experience Platform に統合された [!DNL Data Science Workspace] は、アドビソリューションをまたいでコンテンツやデータアセットを使用し、予測をおこなうのに役立ちます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| [!DNL JupyterLab] での VM の改善 | 長時間実行される [!DNL JupyterLab notebook] 仮想マシンの安定性が向上しました。 |

[!DNL JupyterLab] の詳細については、[[!DNL JupyterLab]  ユーザーガイド ](../../data-science-workspace/jupyterlab/overview.md) を参照してください。

## 宛先 {#destinations}

[Real-time Customer Data Platform](../../rtcdp/overview.md) では、宛先は、宛先プラットフォームとの事前定義済みの統合であり、シームレスな方法でこれらのパートナーにデータをアクティブ化します。

**新しい宛先**

新しい宛先は、Adobe Experience Platform データをアクティブ化する際に使用できます。詳しくは、以下を参照してください。

| 宛先 | 説明 |
|--- | ---|
| [!DNL Google Customer Match] | Google カスタマーマッチを使用すると、[!DNL Search]、[!DNL Shopping]、Gmail、YouTubeなど、Googleが所有および運営するプロパティをまたいでオンラインおよびオフラインのデータを使用して、顧客にリーチし、再びエンゲージできます。 宛先 <br><br>Real-Time CDPでの設定方法について詳しくは、宛先カタログの [!DNL Google Customer Match] [ ページ ](../../destinations/catalog/advertising/google-customer-match.md) を参照してください。 |

**新機能**

| 機能 | 説明 |
|------- | -----------|
| カスタムのファイル名エディター | メールマーケティングの宛先とクラウドストレージの宛先のデータアクティベーションワークフローを更新し、書き出したファイルの名前を編集できるようにします。 詳しくは、アクティベーションワークフローの [ 設定ステップ ](../../destinations/ui/activate-batch-profile-destinations.md) を参照してください。 |
| 推奨される属性 | 書き出されたファイルに追加する推奨属性を表示する、メールマーケティングの宛先とクラウドストレージの宛先のデータアクティベーションワークフローを更新します。 詳しくは、アクティベーションワークフローの [ 属性を選択ステップ ](../../destinations/ui/activate-batch-profile-destinations.md) を参照してください。 |

## [!DNL Real-Time Customer Data Platform] {#rtcdp}

Experience Platform 上に構築された Real-time Customer Data Platform（[!DNL Real-Time CDP]）は、企業が既知および未知のデータを統合し、カスタマージャーニーを通じてインテリジェントな意思決定により顧客プロファイルをアクティブ化するのに役立ちます。[!DNL Real-Time CDP] では、複数のエンタープライズデータソースを組み合わせて、リアルタイムで顧客プロファイルを作成します。これらのプロファイルから構築されたセグメントをダウンストリームの宛先に送信し、すべてのチャネルとデバイスにわたって 1 対 1 のパーソナライズされた顧客体験を提供できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| IAB TCF 2.0 のサポート | [!DNL Interactive Advertising Bureau] （IAB）で概要が説明されているように、[!DNL Real-Time CDP] は [!DNL Transparency & Consent Framework] （TCF）の 2.0 バージョンの登録済みベンダーになりました。 CMP が生成した顧客同意データを受け入れるようにデータ操作とプロファイルスキーマを設定し、ダウンストリーム宛先に対してセグメントをアクティブ化する際に顧客の同意環境設定を適用できます。 |

[!DNL Real-Time CDP] について詳しくは、[[!DNL Real-Time CDP] 概要](../../rtcdp/overview.md)を参照してください。

## ソース {#sources}

Adobe Experience Platformでは、外部ソースからデータを取り込むときに、[!DNL Platform] サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を提供します。 これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| フロー実行監視 | ユーザーは、すべてのフロー実行を監視し、完了ステータス、実行期間、処理されたファイルのリスト、エラー、指標など、各実行の詳細を表示できます。 詳しくは、[ データフローの監視 ](../../sources/tutorials/ui/monitor.md) ドキュメントを参照してください。 |
| フロー実行通知 | ユーザーはイベントを購読し、Webhook を登録して、フロー実行に関するステータス、指標およびエラーについてリアルタイム通知を受け取ることができます。 |
| UI カタログの改善 | ソースカタログ画面が更新され、選択したオブジェクトのプライマリアクションに簡単にアクセスできるようになりました。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
