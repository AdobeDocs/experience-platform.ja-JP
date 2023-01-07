---
keywords: Experience Platform；ホーム；人気の高いトピック；Adobe Experience Platform；ユーザーガイド；ui ガイド；platform ui ガイド；platform の概要；ダッシュボード；
solution: Experience Platform
title: Experience PlatformUI の概要
description: Adobe Experience Platform
exl-id: 47f9a3fb-731d-4ade-8069-faaa18f224dc
source-git-commit: 5a14eb5938236fa7186d1a27f28cee15fe6558f6
workflow-type: tm+mt
source-wordcount: '1811'
ht-degree: 4%

---

# Adobe Experience Platform UI ガイド

このガイドは、Adobe Experience Platformユーザーインターフェイス (UI) の使用方法の紹介として機能し、様々なコンポーネントの用途を説明し、詳細なドキュメントへのリンクを提供します。

Adobe Experience Platformの詳細については、 [Experience Platformの概要](home.md).

## ホーム画面

Adobe Experience Platformにログインすると、 [!UICONTROL ホーム] ページ ( [指標ダッシュボード](#metrics), [最近のデータ](#recent-data)、および [推奨学習](#recommended-learning) セクション。

![](images/user-guide/homepage.png)

### 指標

指標ダッシュボードには、組織内のデータセット、プロファイル、セグメント、宛先に関する情報を提供するカードが表示されます。

![](images/user-guide/homepage-dashboard.png)

この **[!UICONTROL データセット]** 「 」セクションには、IMS 組織内のデータセットの数が表示されます。 この数は、新しいデータセットが作成されると更新されます。 データセットの詳細については、 [データセットの概要](../catalog/datasets/overview.md).

この **[!UICONTROL プロファイル]** 「 」セクションには、IMS 組織内のプロファイルを持つ人の合計数（プロファイルフラグメントを除く）が表示されます。 この合計ユーザー数は、アドレス可能なオーディエンスの合計を表し、24 時間ごとに更新されます。 プロファイルについて詳しくは、 [リアルタイム顧客プロファイルの概要](../profile/home.md).

この **[!UICONTROL セグメント]** 「 」セクションには、IMS 組織内で作成されたセグメントの合計数が表示されます。 この数は、新しいセグメントが作成されると更新されます。 セグメントについて詳しくは、 [セグメント化サービスの概要](../segmentation/home.md).

この **[!UICONTROL 宛先]** 「 」セクションには、IMS 組織用に作成された宛先の合計数が表示されます。 この数は、新しい宛先が作成されると更新されます。 宛先について詳しくは、 [宛先の概要](../destinations/home.md).

### 最近のデータ

最近使用したデータダッシュボードには、最近作成したデータセット、ソース、セグメント、宛先に関する情報が表示されます。

![](images/user-guide/homepage-recent.png)

この **[!UICONTROL 最近のデータセット]** 「 」セクションには、IMS 組織内で最近作成された 5 つのデータセットが表示されます。 このリストは、新しいデータセットが作成されるたびに更新されます。 リストからデータセットを選択して、指定したデータセットの詳細を表示したり、 **[!UICONTROL すべて表示]** をクリックして、作成したすべてのデータセットのリストを表示します。 データセットの詳細については、 [データセットの概要](../catalog/datasets/overview.md).

この **[!UICONTROL 最近のソース]** 「 」セクションには、IMS 組織内で最近作成された 5 つのソースコネクタの一覧が表示されます。 このリストは、新しいソースコネクタが作成されるたびに更新されます。 リストからソース接続を選択して、指定したコネクタの詳細を表示したり、 **[!UICONTROL すべて表示]** をクリックして、作成されたすべてのソース接続の一覧を表示します。 ソースについて詳しくは、 [ソースの概要](../sources/home.md).

この **[!UICONTROL 最近のセグメント]** 「 」セクションには、IMS 組織内で最近作成された 5 つのセグメント定義が表示されます。 このリストは、新しいセグメント定義が作成されるたびに更新されます。 リストからセグメント定義を選択して、指定したセグメント定義の詳細を表示したり、「 」を選択したりできます **[!UICONTROL すべて表示]** をクリックすると、作成したすべてのセグメント定義のリストが表示されます。 セグメントについて詳しくは、 [セグメント化サービスの概要](../segmentation/home.md).

この **[!UICONTROL 最近の宛先]** 「 」セクションには、IMS 組織内で最近作成された 5 つの宛先が表示されます。 このリストは、新しい宛先が作成されるたびに更新されます。 リストから宛先を選択して、指定した宛先に関する詳細を表示したり、「 」を選択したりできます **[!UICONTROL すべて表示]** をクリックして、作成されたすべての宛先のリストを表示します。 宛先について詳しくは、 [宛先の概要](../destinations/home.md).

### 推奨される学習

この **[!UICONTROL 推奨される学習]** 「 」節では、Adobe Experience Platformの使用を開始する際に役立つドキュメントへのリンクを提供しています。

![](images/user-guide/homepage-recommended.png)

## 上部ナビゲーションバー

Platform UI の上部のナビゲーションバーには、現在サインインしている IMS 組織が表示され、いくつかの重要なコントロールが提供されます。

ナビゲーションバーの左側には、Adobe Experience Platformのロゴが表示されます。 このロゴをいつでも選択すると、Platform UI のホーム画面に戻ります。

![](./images/user-guide/homepage-top-nav-bar.png)

### IMS 組織スイッチャー

上部ナビゲーションバーの右側にある最初の項目は、 **IMS 組織スイッチャー**.

![](./images/user-guide/homepage-ims-org-switcher.png)

アクセス権がある IMS 組織がある場合は、切り替えボタンを選択すると、その組織のドロップダウンメニューが開きます。 別の IMS 組織に切り替えるには、リストに表示されたオプションを選択します。

### アプリケーションの切り替え

上部ナビゲーションの右側にある次の項目は、 **アプリ切り替え**&#x200B;は、 ![アプリ切り替え](./images/user-guide/app-switcher-icon.png) アイコン このアイコンを選択すると、IMS 組織がアクセス権を持つAdobeアプリケーション (Experience Platform、Analytics、Assets など ) を切り替えることができます。

### ヘルプ

アプリケーション切り替えボタンの右側に、 **ヘルプとサポートメニュー**( ![疑問符/ヘルプ](./images/user-guide/help-icon.png) アイコン このアイコンを選択すると、いくつかのヘルプおよびサポートリソースを含むポップオーバーメニューが表示されます。 この **[!UICONTROL ヘルプ]** 「 」タブには、現在表示しているページに関連するドキュメントのリストが表示されます。 この **[!UICONTROL サポート]** 「 」タブを使用すると、サポートチケットとAdobeサポートチームを作成できます。 この **[!UICONTROL フィードバック]** 「 」タブを使用すると、Platform に関するフィードバックをAdobeに送信できます。

![](images/user-guide/homepage-help-clicked.png)

### 通知とお知らせ

内 **通知セクション**( ![ベル/通知とお知らせ](images/user-guide/notification-icon.png) アイコン この **[!UICONTROL 通知]** 「 」タブには、製品に関する重要な情報やその他の関連する更新が表示されます。 **[!UICONTROL お知らせ]** 「 」タブには、サービスメンテナンスに関する情報が表示されます。

### ユーザープロファイル

上部ナビゲーションバーの最後の項目は、 **ユーザー設定**&#x200B;は、 ![ユーザー設定/ユーザープロファイル](images/user-guide/profile-icon.png) アイコン このアイコンを選択して、環境設定を編集するか、ログアウトします。

Platform インターフェイスの明るいテーマと暗いテーマは、名前と電子メールのすぐ下にあるスイッチで切り替えることができます。 目的のテーマを選択します。

![](images/theme.png)

### サンドボックス

上部ナビゲーションバーのすぐ下に、サンドボックスバーがあります。 このバーには、現在 Platform 用に使用しているサンドボックスが表示されます。 サンドボックスの詳細については、 [サンドボックスの概要](../sandboxes/home.md).

## 左側のナビゲーション {#left-nav}

画面の左側のナビゲーションには、Platform UI でサポートされるすべての様々なサービスが表示されます。

メニューアイコンをクリックして、左側のナビゲーションパネルの表示/非表示を切り替えます。

![](images/user-guide/hidemenu.png)

パネルを表示した後で再度クリックすると、ナビゲーションを開いた位置にロックできます。

>[!IMPORTANT]
>
>左側のナビゲーションバーには、アクセス可能な機能のみが表示されます。 以前のバージョンのAdobe Experience Platformでは、使用できない項目は無効になっていました。 表示されないセクションへのアクセス権が必要と思われる場合は、システム管理者に問い合わせてください。

![](images/user-guide/homepage-left.png)

この **[!UICONTROL ホーム]** 「 」セクションを使用して、Platform UI のホームページに戻ることができます。

この **[!UICONTROL ワークフロー]** 「 」セクションには、Platform 内で操作を実行するための複数手順のワークフローのリストが表示されます。 ワークフローについて詳しくは、 [ワークフローの概要](./workflows.md).

### [!UICONTROL 接続]

この **[!UICONTROL ソース]** 「 」セクションでは、ソース接続を作成、更新および削除でき、外部ソースから Platform にデータを取り込むことができます。 ソースについて詳しくは、 [ソースの概要](../sources/home.md).

この **[!UICONTROL 宛先]** 「 」セクションでは、宛先を作成、更新および削除でき、Platform から多数の外部宛先にデータを書き出すことができます。 宛先について詳しくは、 [宛先の概要](../destinations/home.md).

### [!UICONTROL 顧客]

この **[!UICONTROL プロファイル]** 「 」セクションでは、顧客プロファイルを参照し、プロファイル指標を表示し、結合ポリシーを作成および管理し、和集合スキーマを表示できます。 を使用する方法の詳細については、以下を参照してください。 [!UICONTROL プロファイル] セクション、お読みください [[!DNL Profile] ユーザーガイド](../profile/ui/user-guide.md). リアルタイム顧客プロファイルの詳細については、 [リアルタイム顧客プロファイルの概要](../profile/home.md).

この **[!UICONTROL セグメント]** 「 」セクションでは、セグメント定義を作成および管理できます。 を使用する方法の詳細については、以下を参照してください。 [!UICONTROL セグメント] セクション、お読みください [セグメント化ユーザーガイド](../segmentation/ui/overview.md). セグメント化サービスについて詳しくは、 [セグメント化サービスの概要](../segmentation/home.md).

この **[!UICONTROL ID]** 「 」セクションでは、id 名前空間を作成および管理できます。 詳しくは、 [!UICONTROL ID] id 名前空間に関する情報や、Platform UI での id の使用方法に関する節を含む、 [id 名前空間の概要](../identity-service/namespaces.md).

### [!UICONTROL プライバシー]

この **[!UICONTROL ポリシー]** 「 」セクションでは、データ使用ポリシーを作成および管理できます。 ポリシーセクションの使用方法の詳細については、 [データ使用ポリシーユーザガイド](../data-governance/policies/user-guide.md). データ使用ポリシーの詳細については、 [データ使用ポリシーの概要](../data-governance/policies/overview.md).

この **[!UICONTROL リクエスト]** セクションを使用して、プライバシーリクエストを作成および管理できます。 Privacy ServiceUI にアクセスするには、許可リストに加えるが必要です。 リクエストの節の使用方法について詳しくは、 [Privacy Serviceユーザーガイド](../privacy-service/ui/user-guide.md). Privacy Serviceの詳細については、 [Privacy Serviceの概要](../privacy-service/home.md).

### [!UICONTROL データサイエンス]

この **[!UICONTROL ノートブック]** 「 」セクションでは、データの調査、分析、モデリングをおこなえるインタラクティブな開発環境である JupyterLab にアクセスできます。 「ノートブック」セクションの使用方法の詳細については、 [JupyterLab ユーザーガイド](../data-science-workspace/jupyterlab/overview.md). Data Science Workspace について詳しくは、 [Data Science Workspace の概要](../data-science-workspace/home.md)

この **[!UICONTROL モデル]** 「 」セクションでは、機械学習と人工知能を使用して、モデルの作成、開発、トレーニング、調整を行って予測を行うことができます。 モデルの節の詳細については、 [モデルのトレーニングと評価](../data-science-workspace/models-recipes/train-evaluate-model-ui.md).

この **[!UICONTROL サービス]** 「 」セクションでは、スケジュールに沿ったトレーニングとスコアリングに関して公開済みモデルを管理したり、Adobeの Intelligent Services を使用してリアルタイムのパーソナライズされた顧客体験を提供したりできます。 「サービス」の節について詳しくは、 [サービスとしてのモデルの公開のチュートリアル](../data-science-workspace/models-recipes/publish-model-service-ui.md).

### [!UICONTROL データ管理]

この **[!UICONTROL スキーマ]** 「 」セクションでは、Experience Data Model(XDM) スキーマを作成および管理できます。 スキーマの詳細については、 [スキーマの作成](../xdm/tutorials/create-schema-ui.md). XDM について詳しくは、 [XDM システムの概要](../xdm/home.md).

この **[!UICONTROL データセット]** 「 」セクションでは、データセットを作成および管理できます。 データセットの詳細については、 [データセットユーザーガイド](../catalog/datasets/user-guide.md).

この **[!UICONTROL クエリ]** 「 」セクションでは、クエリの作成と管理、Adobe Experience Platform Query Service による SQL クエリのログの記録、 [!DNL PostgreSQL] 資格情報。 クエリの詳細については、 [クエリサービスユーザーガイド](../query-service/ui/overview.md).

この **[!UICONTROL 監視]** 「 」セクションでは、バッチとストリーミングの取り込みを監視できます。 監視について詳しくは、 [データ取得ユーザーガイドの監視](../ingestion/quality/monitor-data-ingestion.md).

### [!UICONTROL 判定]

Adobe Journey Optimizerは、Experience Platform上に構築されたアプリケーションサービスです。 強力な判定テクノロジーを使用して、すべてのタッチポイントにわたる最適なオファーとエクスペリエンスを適切なタイミングで顧客に提供できます。 の操作を含め、Journey Optimizerの詳細を学ぶには [!UICONTROL オファー] および [!UICONTROL アクティビティ] を [Journey Optimizerドキュメント](https://experienceleague.adobe.com/docs/journey-optimizer.html?lang=ja).

### [!UICONTROL 管理]

 Platform ユーザーインターフェイス（UI）には、毎日のスナップショットで取得した、組織のライセンス使用状況に関する重要な情報を表示できるダッシュボードが用意されています。「 」を選択して、このダッシュボードにアクセスします **[!UICONTROL ライセンスの使用]** をクリックします。 ライセンス使用状況ダッシュボードの詳細については、 [ライセンス使用状況ダッシュボードガイド](./license-usage-and-guardrails/license-usage-dashboard.md).

>[!IMPORTANT]
>
>ライセンス使用状況ダッシュボード機能は現在アルファ版で、すべてのユーザーが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

## 次の手順

このガイドを読むことで、Platform UI のホームページと主要なナビゲーション要素が紹介されました。 ユーザーインターフェイスでの作業の詳細については、個々の Platform サービスのドキュメントを参照してください。 このドキュメントへのリンクは、 [左ナビゲーション](#left-nav) このドキュメントの前の節が参照されています。
