---
title: Adobe Experience Platformリリースノート 2020 年 8 月
description: Adobe Experience Platformの 2020 年 8 月のリリースノート。
doc-type: release notes
last-update: August 10, 2020
author: crhoades, ens28527
exl-id: 9347147f-e830-4487-aa12-f56723abb3c8
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 29%

---

# Adobe Experience Platform リリースノート

**リリース日：2020年8月12日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Data Science Workspace]](#dsw)
- [[!DNL Destinations]](#destinations)
- [[!DNL Real-Time Customer Data Platform]](#rtcdp)
- [[!DNL Sources]](#sources)

## [!DNL Data Science Workspace] {#dsw}

 [!DNL Data Science Workspace] は機械学習と人工知能を使用して、データからインサイトを引き出します。Adobe Experience Platform に統合された [!DNL Data Science Workspace] は、アドビソリューションをまたいでコンテンツやデータアセットを使用し、予測をおこなうのに役立ちます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| の VM の改善点 [!DNL JupyterLab] | 長時間実行の安定性を向上 [!DNL JupyterLab notebook] 仮想マシン。 |

詳しくは、 [!DNL JupyterLab]詳しくは、 [[!DNL JupyterLab] ユーザーガイド](../../data-science-workspace/jupyterlab/overview.md).

## 宛先 {#destinations}

In [Real-time Customer Data Platform](../../rtcdp/overview.md)の宛先は、宛先プラットフォームとの事前に構築された統合であり、これらのパートナーに対するデータをシームレスにアクティブ化します。

**新しい宛先**

新しい宛先は、Adobe Experience Platform データをアクティブ化する際に使用できます。詳しくは、以下を参照してください。

| 宛先 | 説明 |
|--- | ---|
| [!DNL Google Customer Match] | Google Customer Match を使用すると、オンラインとオフラインのデータを使用して、次のようなGoogleの所有および運用するプロパティをまたいで、顧客にリーチし、再び関与させることができます。 [!DNL Search], [!DNL Shopping]、Gmail、YouTube。 <br><br> 次にアクセス： [!DNL Google Customer Match] [ページ](../../destinations/catalog/advertising/google-customer-match.md) 宛先カタログのを参照してください。 |

**新機能**

| 機能 | 説明 |
|------- | -----------|
| カスタムファイル名エディター | 書き出されたファイルの名前を編集できる、電子メールマーケティングの宛先とクラウドストレージの宛先のデータアクティベーションワークフローに対して更新します。 詳しくは、 [ ステップの設定](../../destinations/ui/activate-batch-profile-destinations.md) （アクティベーションワークフロー内） |
| 推奨属性 | 書き出されたファイルに追加するための推奨属性を表示する、電子メールマーケティングの宛先およびクラウドストレージの宛先のデータアクティベーションワークフローに更新します。 詳しくは、 [属性を選択ステップ](../../destinations/ui/activate-batch-profile-destinations.md) （アクティベーションワークフロー内） |

## [!DNL Real-Time Customer Data Platform] {#rtcdp}

Experience Platform上に構築、Real-time Customer Data Platform([!DNL Real-Time CDP]) は、企業が既知のデータと不明なデータを統合し、カスタマージャーニー全体を通じてインテリジェントな判定をおこなって、顧客プロファイルをアクティブ化するのに役立ちます。 [!DNL Real-Time CDP] は、複数の企業データソースを組み合わせて、リアルタイムで顧客プロファイルを作成します。 その後、これらのプロファイルから作成されたセグメントをダウンストリームの宛先に送信して、すべてのチャネルとデバイスにわたって 1 対 1 でパーソナライズされた顧客体験を提供できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| IAB TCF 2.0 のサポート | [!DNL Real-Time CDP] は、現在は、2.0 バージョンの [!DNL Transparency & Consent Framework] (TCF)。 [!DNL Interactive Advertising Bureau] (IAB) を参照してください。 CMP で生成される顧客の同意データを受け入れるようにデータ操作とプロファイルスキーマを設定し、ダウンストリームの宛先に対してセグメントをアクティブ化する際に、顧客の同意設定を適用できます。 |

詳しくは、 [!DNL Real-Time CDP]を参照し、 [[!DNL Real-Time CDP] 概要](../../rtcdp/overview.md).

## ソース {#sources}

Adobe Experience Platformで外部ソースからデータを取り込みながら、を使用してデータの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| フロー実行の監視 | すべてのフロー実行を監視し、完了ステータス、実行期間、処理されたファイルのリスト、エラー、指標など、各実行の詳細ビューを確認できます。 詳しくは、 [データフローの監視](../../sources/tutorials/ui/monitor.md) ドキュメントを参照してください。 |
| フロー実行通知 | イベントをサブスクライブし、Web フックを登録して、フロー実行に関するステータス、指標、エラーに関するリアルタイム通知を受け取ることができます。 |
| UI カタログの改善 | ソースカタログ画面を更新して、選択したオブジェクトの主なアクションに簡単にアクセスできるようにしました。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
