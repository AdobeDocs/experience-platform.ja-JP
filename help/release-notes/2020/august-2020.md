---
title: Adobe Experience Platform リリースノート
description: Experience Platformリリースノート（2020年8月11日）
doc-type: release notes
last-update: August 10, 2020
author: crhoades, ens28527
exl-id: 9347147f-e830-4487-aa12-f56723abb3c8
source-git-commit: a619227de30513bb06a22ce7b4f2fc13847c1ab6
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 29%

---

# Adobe Experience Platform リリースノート

**リリース日：2020 年 12 月 8 日**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Destinations]](#destinations)
- [[!DNL Real-time Customer Data Platform]](#rtcdp)
- [[!DNL Sources]](#sources)

## [!DNL Data Science Workspace] {#dsw}

 [!DNL Data Science Workspace] は機械学習と人工知能を使用して、データからインサイトを引き出します。Adobe Experience Platform に統合された [!DNL Data Science Workspace] は、アドビソリューションをまたいでコンテンツやデータアセットを使用し、予測をおこなうのに役立ちます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| [!DNL JupyterLab]でのVMの改善 | [!DNL JupyterLab notebook]仮想マシンの長時間実行の安定性が向上しました。 |

[!DNL JupyterLab]の詳細については、[[!DNL JupyterLab] ユーザーガイド](../../data-science-workspace/jupyterlab/overview.md)を参照してください。

## 宛先 {#destinations}

[リアルタイム顧客データプラットフォーム](../../rtcdp/overview.md)では、宛先は、宛先プラットフォームとの事前に構築された統合であり、これらのパートナーに対してシームレスにデータをアクティブ化します。

**新しい宛先**

新しい宛先は、Adobe Experience Platform データをアクティブ化する際に使用できます。詳しくは、以下を参照してください。

| 宛先 | 説明 |
|--- | ---|
| [!DNL Google Customer Match] | Googleカスタマーマッチを使用すると、オンラインおよびオフラインのデータを使用して、Googleが所有および運用する次のようなプロパティをまたいで顧客にリーチし、再び関与させることができます。[!DNL Search]、[!DNL Shopping]、Gmail、YouTube。 <br><br> 宛先の詳細 [!DNL Google Customer Match] [](../../destinations/catalog/advertising/google-customer-match.md) と、リアルタイムCDPでの設定方法については、宛先カタログのページを参照してください。 |

**新機能**

| 機能 | 説明 |
|------- | -----------|
| カスタムファイル名エディター | 書き出されたファイルの名前を編集できる、電子メールマーケティングの宛先とクラウドストレージの宛先のデータアクティベーションワークフローに更新します。 詳しくは、アクティベーションワークフローの[設定手順](../../destinations/ui/activate-batch-profile-destinations.md)を参照してください。 |
| 推奨属性 | 書き出されたファイルに追加する推奨属性を表示する、電子メールマーケティングの宛先とクラウドストレージの宛先のデータアクティベーションワークフローに更新します。 詳しくは、アクティベーションワークフローの[属性の選択の手順](../../destinations/ui/activate-batch-profile-destinations.md)を参照してください。 |

## [!DNL Real-time Customer Data Platform] {#rtcdp}

リアルタイム顧客データプラットフォーム([!DNL Real-time CDP])は、Experience Platformに基づいて構築され、既知のデータと不明なデータを統合して、カスタマージャーニー全体を通じてインテリジェントな判定をおこない、顧客プロファイルをアクティブ化します。 [!DNL Real-time CDP] は、複数のエンタープライズデータソースを組み合わせて、リアルタイムで顧客プロファイルを作成します。その後、これらのプロファイルから作成されたセグメントをダウンストリームの宛先に送信して、すべてのチャネルとデバイスにわたって、1対1でパーソナライズされた顧客体験を提供できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| IAB TCF 2.0のサポート | [!DNL Real-time CDP] は、(IAB)で概要を説明しているように、(TCF)の2.0バ [!DNL Transparency & Consent Framework] ージョンの登録ベンダーにな [!DNL Interactive Advertising Bureau] りました。CMPで生成される顧客の同意データを受け入れるようにデータ操作とプロファイルスキーマを設定し、ダウンストリームの宛先に対してセグメントをアクティブ化する際に顧客の同意設定を適用できます。 |

[!DNL Real-time CDP]について詳しくは、[[!DNL Real-time CDP] 概要](../../rtcdp/overview.md)を参照してください。

## ソース {#sources}

Adobe Experience Platformでは、外部ソースからデータを取り込みながら、[!DNL Platform]サービスを使用してデータの構造化、ラベル付け、拡張をおこなうことができます。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| フロー実行の監視 | すべてのフロー実行を監視し、完了ステータス、実行期間、処理されたファイルのリスト、エラー、指標など、各実行の詳細を確認できます。 詳しくは、[データフローの監視](../../sources/tutorials/ui/monitor.md)ドキュメントを参照してください。 |
| フロー実行の通知 | ユーザーは、イベントをサブスクライブし、Webフックを登録して、フロー実行に関するステータス、指標、エラーに関するリアルタイム通知を受け取ることができます。 |
| UIカタログの改善 | ソースカタログ画面を更新し、選択したオブジェクトの主なアクションに容易にアクセスできるようにしました。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
