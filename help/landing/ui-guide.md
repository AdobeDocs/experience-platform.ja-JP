---
keywords: Experience Platform；ホーム；人気のトピック；Adobe Experience Platform；ユーザーガイド；ui ガイド；platform ui ガイド；platform の概要；ダッシュボード；
solution: Experience Platform
title: Experience PlatformUI の概要
description: Adobe Experience Platform
exl-id: 47f9a3fb-731d-4ade-8069-faaa18f224dc
source-git-commit: c2832821ea6f9f630e480c6412ca07af788efd66
workflow-type: tm+mt
source-wordcount: '1795'
ht-degree: 3%

---

# Adobe Experience Platform UI ガイド

このガイドは、Adobe Experience Platform ユーザーインターフェイス（UI）の概要として機能し、様々なコンポーネントの用途を説明し、詳細なドキュメントへのリンクを提供します。

Adobe Experience Platform について詳しくは、[Experience Platform の概要](home.md)を参照してください。

## ホーム画面

Adobe Experience Platformにログインしたら、[!UICONTROL  指標ダッシュボード ]、[ 最近のデータ ](#metrics)、および [ 推奨されるラーニング ](#recent-data) の各セクションで構成される [ ホーム ](#recommended-learning) ページに移動します。

![](images/user-guide/homepage.png)

### 指標

指標ダッシュボードは、組織内のデータセット、プロファイル、セグメントおよび宛先に関する情報を提供するカードを提供します。

![](images/user-guide/homepage-dashboard.png)

「**[!UICONTROL データセット]**」セクションには、組織内のデータセットの数が表示されます。 この番号は、新しいデータセットが作成されると更新されます。 データセットについて詳しくは、[ データセットの概要 ](../catalog/datasets/overview.md) を参照してください。

「**[!UICONTROL プロファイル]**」セクションには、プロファイルフラグメントを除く、組織内でプロファイルを持つユーザーの合計数が表示されます。 このユーザーの合計数は、アドレス可能なオーディエンスの合計を表し、24 時間ごとに更新されます。 プロファイルについて詳しくは、[ リアルタイム顧客プロファイルの概要 ](../profile/home.md) を参照してください。

「**[!UICONTROL セグメント]**」セクションには、組織内で作成されたセグメントの合計数が表示されます。 この番号は、新しいセグメントが作成されると更新されます。 セグメントについて詳しくは、[ セグメント化サービスの概要 ](../segmentation/home.md) を参照してください。

「**[!UICONTROL 宛先]**」セクションには、組織用に作成された宛先の合計数が表示されます。 この番号は、新しい宛先が作成されると更新されます。 宛先について詳しくは、[ 宛先の概要 ](../destinations/home.md) を参照してください。

### 最近のデータ

最近のデータダッシュボードは、最近作成したデータセット、ソース、セグメントおよび宛先に関する情報を提供します。

![](images/user-guide/homepage-recent.png)

「**[!UICONTROL 最近のデータセット]**」セクションには、組織内で最近作成された 5 つのデータセットが一覧表示されます。 このリストは、新しいデータセットが作成されるたびに更新されます。 リストからデータセットを選択して、指定したデータセットの詳細を表示するか、「**[!UICONTROL すべて表示]**」を選択して、作成されたすべてのデータセットのリストを表示できます。 データセットについて詳しくは、[ データセットの概要 ](../catalog/datasets/overview.md) を参照してください。

「**[!UICONTROL 最新のソース]**」セクションには、組織内で最近作成された 5 つのソースコネクタが一覧表示されます。 このリストは、新しいソースコネクタが作成されるたびに更新されます。 リストからソース接続を選択して、指定したコネクタの詳細情報を表示するか、「**[!UICONTROL すべて表示]**」を選択して、作成したすべてのソース接続のリストを表示できます。 ソースについて詳しくは、[ ソースの概要 ](../sources/home.md) を参照してください。

「**[!UICONTROL 最近のセグメント]**」セクションには、組織内で最近作成された 5 つのセグメント定義が一覧表示されます。 このリストは、新しいセグメント定義が作成されるたびに更新されます。 リストからセグメント定義を選択して、指定したセグメント定義の詳細情報を表示するか、「**[!UICONTROL すべて表示]**」を選択して、作成したすべてのセグメント定義のリストを表示できます。 セグメントについて詳しくは、[ セグメント化サービスの概要 ](../segmentation/home.md) を参照してください。

**[!UICONTROL 最近の宛先]** セクションには、組織内で最近作成された 5 つの宛先が一覧表示されます。 このリストは、新しい宛先が作成されるたびに更新されます。 リストから宛先を選択して、指定した宛先に関する詳細を表示するか、「**[!UICONTROL すべて表示]**」を選択して、作成されたすべての宛先のリストを表示できます。 宛先について詳しくは、[ 宛先の概要 ](../destinations/home.md) を参照してください。

### 推奨される学習

**[!UICONTROL 推奨されるラーニング]** のセクションには、Adobe Experience Platformの基本を学ぶのに役立つドキュメントへのリンクが表示されます。

![](images/user-guide/homepage-recommended.png)

## 上部ナビゲーションバー

Platform UI の上部のナビゲーションバーには、現在ログインしている組織が表示され、いくつかの重要なコントロールが用意されています。

ナビゲーションバーの左側には、Adobe Experience Platformのロゴがあります。 このロゴをいつでも選択すると、Platform UI のホーム画面に戻ります。

![](./images/user-guide/homepage-top-nav-bar.png)

### 組織スイッチャー

上部のナビゲーションバーの右側にある最初の項目は **組織スイッチャー** です。

![](./images/user-guide/homepage-ims-org-switcher.png)

スイッチャーを選択すると、アクセス権のある組織（使用可能な場合）のドロップダウンメニューが開きます。 別の組織に切り替えるには、リストに表示されたオプションを選択します。

### アプリケーションの切り替え

上部ナビゲーションの右側の次の項目は **アプリケーションスイッチャー** で、![ アプリケーションスイッチャー ](/help/images/icons/apps.png) アイコンで表されます。 このアイコンを選択すると、Experience Platform、Analytics、Assetsなど、組織がアクセスできるAdobeアプリケーションを切り替えることができます。

### ヘルプ

アプリケーション切り替えボタンの右側には、「**ヘルプとサポート** メニューが表示されます。このメニューは、「![ 疑問符/ヘルプ ](/help/images/icons/help.png)」アイコンで表されます。 このアイコンを選択すると、ポップオーバーメニューが表示され、いくつかのヘルプとサポートリソースが含まれます。 「**[!UICONTROL ヘルプ]**」タブには、現在アクセスしているページに関連するドキュメントのリストが表示されます。 「**[!UICONTROL サポート]**」タブを使用すると、Adobeサポートチームとのサポートチケットを作成できます。 「**[!UICONTROL フィードバック]**」タブを使用すると、Platform に関するフィードバックをAdobeに送信できます。

![](images/user-guide/homepage-help-clicked.png)

### 通知とお知らせ

**ベル/通知とお知らせ** アイコンで表される ![ 通知セクション ](/help/images/icons/bell.png)。 「**[!UICONTROL 通知]**」タブには、製品に関する重要な情報やその他の関連する更新が表示され、「**[!UICONTROL お知らせ]**」タブには、サービスのメンテナンスに関する情報が表示されます。

### ユーザープロファイル

上部のナビゲーションバーの最後の項目は **ユーザー設定** で、![ ユーザー設定/ユーザープロファイル ](images/user-guide/profile-icon.png) アイコンで表されます。 このアイコンを選択して、環境設定を編集したり、ログアウトしたりします。

名前とメールのすぐ下にあるスイッチを使用して、Platform インターフェイスのテーマをライトとダークに切り替えることができます。 好きなテーマを選択します。

![](images/theme.png)

### サンドボックス

上部のナビゲーションバーのすぐ下にサンドボックスバーがあります。 このバーには、Platform に現在使用しているサンドボックスが表示されます。 サンドボックスについて詳しくは、[ サンドボックスの概要 ](../sandboxes/home.md) を参照してください。

## 左側のナビゲーション {#left-nav}

画面左側のナビゲーションには、Platform UI でサポートされる様々なサービスがすべて表示されます。

メニューアイコンをクリックして、左側のナビゲーションパネルの表示/非表示を切り替えます。

![](images/user-guide/hidemenu.png)

パネルを表示した後にもう一度クリックすると、開いた位置でナビゲーションをロックできます。

>[!IMPORTANT]
>
>左側のナビゲーションバーには、アクセス可能な機能のみが表示されます。 以前のバージョンのAdobe Experience Platformでは、使用できない項目が無効になっていました。 表示されないセクションにアクセスする必要があると思われる場合は、システム管理者にお問い合わせください。

![](images/user-guide/homepage-left.png)

「**[!UICONTROL ホーム]**」セクションでは、Platform UI ホームページに戻ることができます。

「**[!UICONTROL ワークフロー]**」セクションには、Platform 内で操作を実行するための複数の手順のワークフローのリストが表示されます。 ワークフローについて詳しくは、[ ワークフローの概要 ](./workflows.md) を参照してください。

### [!UICONTROL 接続]

「**[!UICONTROL ソース]**」セクションでは、ソース接続を作成、更新、削除し、外部ソースから Platform にデータを取り込むことができます。 ソースについて詳しくは、[ ソースの概要 ](../sources/home.md) を参照してください。

「**[!UICONTROL 宛先]**」セクションでは、宛先を作成、更新および削除して、Platform から多くの外部宛先にデータを書き出すことができます。 宛先について詳しくは、[ 宛先の概要 ](../destinations/home.md) を参照してください。

### [!UICONTROL  顧客 ]

「**[!UICONTROL プロファイル]**」セクションでは、顧客プロファイルの参照、プロファイル指標の表示、結合ポリシーの作成と管理、和集合スキーマの表示を行うことができます。 [!UICONTROL  プロファイル ] の使用について詳しくは、[[!DNL Profile]  ユーザーガイド ](../profile/ui/user-guide.md) を参照してください。 リアルタイム顧客プロファイルについて詳しくは、[ リアルタイム顧客プロファイルの概要 ](../profile/home.md) を参照してください。

「**[!UICONTROL オーディエンス]**」セクションでは、セグメント定義を作成および管理できます。 [!UICONTROL  オーディエンス ] の使用について詳しくは、[ セグメント化ユーザーガイド ](../segmentation/ui/overview.md) を参照してください。 セグメント化サービスについて詳しくは、[ セグメント化サービスの概要 ](../segmentation/home.md) を参照してください。

「**[!UICONTROL ID]**」セクションでは、ID 名前空間を作成および管理できます。 ID 名前空間や Platform UI での ID の使用方法に関する情報など、[!UICONTROL ID] の節について詳しくは、[ID 名前空間の概要 ](../identity-service/features/namespaces.md) を参照してください。

### [!UICONTROL プライバシー]

「**[!UICONTROL ポリシー]**」セクションでは、データ使用ポリシーを作成および管理できます。 ポリシーの節の使用について詳しくは、[ データ使用ポリシーユーザーガイド ](../data-governance/policies/user-guide.md) を参照してください。 データ使用ポリシーについて詳しくは、[ データ使用ポリシーの概要 ](../data-governance/policies/overview.md) を参照してください。

「**[!UICONTROL リクエスト]**」セクションでは、プライバシーリクエストを作成および管理できます。 Privacy ServiceUI にアクセスするには、許可リストに加えるする必要があることに注意してください。 リクエストのセクションの使用方法について詳しくは、[Privacy Serviceユーザーガイド ](../privacy-service/ui/user-guide.md) を参照してください。 Privacy Serviceの詳細については、[Privacy Serviceの概要 ](../privacy-service/home.md) を参照してください。

### [!UICONTROL データサイエンス]

「**[!UICONTROL ノートブック]**」セクションでは、データの調査、分析、モデル化を行えるインタラクティブ開発環境である JupyterLab にアクセスできます。 「ノートブック」の節の使用について詳しくは、[JupyterLab ユーザーガイド ](../data-science-workspace/jupyterlab/overview.md) を参照してください。 Data Science Workspaceについて詳しくは、[Data Science Workspaceの概要 ](../data-science-workspace/home.md) を参照してください。

**[!UICONTROL モデル]** セクションでは、機械学習と人工知能を使用して、モデルを作成、開発、トレーニング、調整し、予測を行うことができます。 モデルの節について詳しくは、[ モデルのトレーニングと評価 ](../data-science-workspace/models-recipes/train-evaluate-model-ui.md) に関するチュートリアルを参照してください。

**[!UICONTROL サービス]** セクションでは、スケジュールに沿ったトレーニングやスコアリング用に公開済みモデルを管理したり、Adobeのインテリジェントサービスを使用したりできます。インテリジェントサービスは、パーソナライズされたカスタマーエクスペリエンスをリアルタイムで提供する一連の AI サービスです。 サービスの節について詳しくは、[ サービスとしてのモデルの公開チュートリアル ](../data-science-workspace/models-recipes/publish-model-service-ui.md) を参照してください。

### [!UICONTROL  データ管理 ]

**[!UICONTROL スキーマ]** セクションでは、Experience Data Model （XDM）スキーマを作成および管理できます。 スキーマについて詳しくは、[ スキーマの作成 ](../xdm/tutorials/create-schema-ui.md) のチュートリアルを参照してください。 XDM について詳しくは、[XDM システムの概要 ](../xdm/home.md) を参照してください。

「**[!UICONTROL データセット]**」セクションでは、データセットを作成および管理できます。 データセットについて詳しくは、[ データセットユーザーガイド ](../catalog/datasets/user-guide.md) を参照してください。

「**[!UICONTROL クエリ]**」セクションでは、クエリの作成と管理、Adobe Experience Platform クエリサービスによって作成された SQL クエリのログ記録、[!DNL PostgreSQL] 資格情報の表示を行うことができます。 クエリについて詳しくは、[ クエリサービスユーザーガイド ](../query-service/ui/overview.md) を参照してください。

「**[!UICONTROL 監視]**」セクションでは、バッチ取得とストリーミング取得を監視できます。 監視について詳しくは、[ データ取り込みの監視ユーザーガイド ](../ingestion/quality/monitor-data-ingestion.md) を参照してください。

### [!UICONTROL 判定]

Adobe Journey Optimizerは、Experience Platformを基に構築されたアプリケーションサービスです。 強力な意思決定テクノロジーを使用して、すべてのタッチポイントにわたって適切なタイミングで最高のオファーとエクスペリエンスを顧客に提供できます。 [!UICONTROL  オファー ] および [!UICONTROL  アクティビティの操作など、Journey Optimizerについて詳しくは ]4}Journey Optimizerのドキュメント ](https://experienceleague.adobe.com/docs/journey-optimizer.html?lang=ja) を参照してください。[

### [!UICONTROL 管理]

Platform ユーザーインターフェイス（UI）には、毎日のスナップショットで取得した、組織のライセンス使用状況に関する重要な情報を表示できるダッシュボードが用意されています。 このダッシュボードにアクセスするには、ナビゲーションで **[!UICONTROL ライセンスの使用状況]** を選択します。 ライセンス使用状況ダッシュボードについて詳しくは、[ ライセンス使用状況ダッシュボードガイド ](./license-usage-and-guardrails/license-usage-dashboard.md) を参照してください。

>[!IMPORTANT]
>
>ライセンス使用状況ダッシュボードの機能は現在アルファ版であり、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

## 次の手順

このガイドを読むことで、Platform UI のホームページと主なナビゲーション要素について説明しました。 ユーザーインターフェイスでの作業について詳しくは、個々の Platform サービスのドキュメントを参照してください。 このドキュメントへのリンクは、このドキュメントの前の方にある [ 左側のナビゲーション ](#left-nav) の節に記載されています。
