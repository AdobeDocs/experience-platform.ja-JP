---
keywords: Experience Platform；ホーム；人気のトピック；Adobe Experience Platform；ユーザーガイド；ui ガイド；platform ui ガイド；platform の概要；ダッシュボード；
solution: Experience Platform
title: Experience PlatformUI の概要
description: Adobe Experience Platform
exl-id: 47f9a3fb-731d-4ade-8069-faaa18f224dc
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '1795'
ht-degree: 3%

---

# Adobe Experience Platform UI ガイド

このガイドは、Adobe Experience Platform ユーザーインターフェイス（UI）の概要として機能し、様々なコンポーネントの用途を説明し、詳細なドキュメントへのリンクを提供します。

Adobe Experience Platform について詳しくは、[Experience Platform の概要](home.md)を参照してください。

## ホーム画面

Adobe Experience Platformにログインした後、を使用します [!UICONTROL ホーム] ページで構成されています。 [指標ダッシュボード](#metrics), [最近のデータ](#recent-data)、および [recommended learning](#recommended-learning) セクション。

![](images/user-guide/homepage.png)

### 指標

指標ダッシュボードは、組織内のデータセット、プロファイル、セグメントおよび宛先に関する情報を提供するカードを提供します。

![](images/user-guide/homepage-dashboard.png)

この **[!UICONTROL データセット]** 「」セクションには、組織内のデータセットの数が表示されます。 この番号は、新しいデータセットが作成されると更新されます。 データセットの詳細については、次を参照してください。 [データセットの概要](../catalog/datasets/overview.md).

この **[!UICONTROL プロファイル]** 「」セクションには、プロファイルフラグメントを除く、組織内にプロファイルを持つユーザーの合計数が表示されます。 このユーザーの合計数は、アドレス可能なオーディエンスの合計を表し、24 時間ごとに更新されます。 プロファイルの詳細については、を参照してください。 [リアルタイム顧客プロファイルの概要](../profile/home.md).

この **[!UICONTROL セグメント]** 「」セクションには、組織内で作成されたセグメントの合計数が表示されます。 この番号は、新しいセグメントが作成されると更新されます。 セグメントの詳細については、を参照してください。 [セグメント化サービスの概要](../segmentation/home.md).

この **[!UICONTROL 宛先]** セクションには、組織用に作成された宛先の合計数が表示されます。 この番号は、新しい宛先が作成されると更新されます。 宛先について詳しくは、以下を参照してください。 [宛先の概要](../destinations/home.md).

### 最近のデータ

最近のデータダッシュボードは、最近作成したデータセット、ソース、セグメントおよび宛先に関する情報を提供します。

![](images/user-guide/homepage-recent.png)

この **[!UICONTROL 最近のデータセット]** 「」セクションには、組織内で最近作成された 5 つのデータセットが一覧表示されます。 このリストは、新しいデータセットが作成されるたびに更新されます。 リストからデータセットを選択して、指定したデータセットの詳細を表示したり、を選択したりできます **[!UICONTROL すべて表示]** 作成されたすべてのデータセットのリストを表示する場合。 データセットの詳細については、次を参照してください。 [データセットの概要](../catalog/datasets/overview.md).

この **[!UICONTROL 最近のソース]** セクションには、組織内で最近作成された 5 つのソースコネクタが一覧表示されます。 このリストは、新しいソースコネクタが作成されるたびに更新されます。 リストからソース接続を選択して、指定したコネクタの詳細情報を表示したり、を選択したりできます **[!UICONTROL すべて表示]** 作成されたすべてのソース接続のリストを表示する ソースについて詳しくは、次を参照してください [ソースの概要](../sources/home.md).

この **[!UICONTROL 最近のセグメント]** 「」セクションには、組織内で最近作成された 5 つのセグメント定義が一覧表示されます。 このリストは、新しいセグメント定義が作成されるたびに更新されます。 リストからセグメント定義を選択して、指定したセグメント定義の詳細情報を表示したり、を選択したりできます **[!UICONTROL すべて表示]** 作成されたすべてのセグメント定義のリストを表示します。 セグメントの詳細については、を参照してください。 [セグメント化サービスの概要](../segmentation/home.md).

この **[!UICONTROL 最近の宛先]** セクションには、組織内で最近作成された 5 つの宛先が一覧表示されます。 このリストは、新しい宛先が作成されるたびに更新されます。 リストから宛先を選択して、指定した宛先の詳細を表示したり、を選択したりできます **[!UICONTROL すべて表示]** 作成されたすべての宛先のリストを表示する 宛先について詳しくは、以下を参照してください。 [宛先の概要](../destinations/home.md).

### 推奨される学習

この **[!UICONTROL おすすめのラーニング]** の節では、Adobe Experience Platformの基本を学ぶのに役立つドキュメントへのリンクを示しています。

![](images/user-guide/homepage-recommended.png)

## 上部ナビゲーションバー

Platform UI の上部のナビゲーションバーには、現在ログインしている組織が表示され、いくつかの重要なコントロールが用意されています。

ナビゲーションバーの左側には、Adobe Experience Platformのロゴがあります。 このロゴをいつでも選択すると、Platform UI のホーム画面に戻ります。

![](./images/user-guide/homepage-top-nav-bar.png)

### 組織スイッチャー

上部のナビゲーションバーの右側に表示される最初の項目は、です。 **組織スイッチャー**.

![](./images/user-guide/homepage-ims-org-switcher.png)

スイッチャーを選択すると、アクセス権のある組織（使用可能な場合）のドロップダウンメニューが開きます。 別の組織に切り替えるには、リストに表示されたオプションを選択します。

### アプリケーションの切り替え

上部のナビゲーションの右側にある次の項目は、 **アプリケーションスイッチャー**。で表されます。 ![アプリケーションスイッチャー](./images/user-guide/app-switcher-icon.png) アイコン。 このアイコンを選択すると、Experience Platform、Analytics、Assetsなど、組織がアクセスできるAdobeアプリケーションを切り替えることができます。

### ヘルプ

アプリケーション切り替えボタンの右側には、 **ヘルプとサポート メニュー**。は、で表されます。 ![疑問符/ヘルプ](./images/user-guide/help-icon.png) アイコン。 このアイコンを選択すると、ポップオーバーメニューが表示され、いくつかのヘルプとサポートリソースが含まれます。 この **[!UICONTROL ヘルプ]** タブには、現在表示しているページに関連するドキュメントのリストが表示されます。 この **[!UICONTROL サポート]** タブを使用すると、Adobeサポートチームとのサポートチケットを作成できます。 この **[!UICONTROL Feedback]** タブを使用すると、Platform に関するフィードバックをAdobeに送信できます。

![](images/user-guide/homepage-help-clicked.png)

### 通知とお知らせ

が含まれる **通知セクション**。は、で表されます。 ![ベル/通知とお知らせ](images/user-guide/notification-icon.png) アイコン。 この **[!UICONTROL 通知]** タブには、製品に関する重要な情報やその他の関連する更新が表示され、 **[!UICONTROL お知らせ]** 「」タブには、サービスメンテナンスに関する情報が表示されます。

### ユーザープロファイル

上部のナビゲーションバーの最終的な項目は、 **ユーザー設定**。で表されます。 ![ユーザー設定/ユーザープロファイル](images/user-guide/profile-icon.png) アイコン。 このアイコンを選択して、環境設定を編集したり、ログアウトしたりします。

名前とメールのすぐ下にあるスイッチを使用して、Platform インターフェイスのテーマをライトとダークに切り替えることができます。 好きなテーマを選択します。

![](images/theme.png)

### サンドボックス

上部のナビゲーションバーのすぐ下にサンドボックスバーがあります。 このバーには、Platform に現在使用しているサンドボックスが表示されます。 サンドボックスの詳細については、次を参照してください。 [サンドボックスの概要](../sandboxes/home.md).

## 左側のナビゲーション {#left-nav}

画面左側のナビゲーションには、Platform UI でサポートされる様々なサービスがすべて表示されます。

メニューアイコンをクリックして、左側のナビゲーションパネルの表示/非表示を切り替えます。

![](images/user-guide/hidemenu.png)

パネルを表示した後にもう一度クリックすると、開いた位置でナビゲーションをロックできます。

>[!IMPORTANT]
>
>左側のナビゲーションバーには、アクセス可能な機能のみが表示されます。 以前のバージョンのAdobe Experience Platformでは、使用できない項目が無効になっていました。 表示されないセクションにアクセスする必要があると思われる場合は、システム管理者にお問い合わせください。

![](images/user-guide/homepage-left.png)

この **[!UICONTROL ホーム]** 「」セクションを使用すると、Platform UI ホームページに戻ることができます。

この **[!UICONTROL ワークフロー]** 「」セクションには、Platform 内で操作を実行するための複数の手順のワークフローのリストが表示されます。 ワークフローについて詳しくは、以下を参照してください。 [ワークフローの概要](./workflows.md).

### [!UICONTROL 接続]

この **[!UICONTROL ソース]** セクションでは、ソース接続を作成、更新、削除して、外部ソースから Platform にデータを取り込むことができます。 ソースについて詳しくは、次を参照してください [ソースの概要](../sources/home.md).

この **[!UICONTROL 宛先]** セクションでは、宛先を作成、更新、削除して、Platform から多くの外部宛先にデータを書き出すことができます。 宛先について詳しくは、以下を参照してください。 [宛先の概要](../destinations/home.md).

### [!UICONTROL 顧客]

この **[!UICONTROL プロファイル]** セクションでは、顧客プロファイルの参照、プロファイル指標の表示、結合ポリシーの作成と管理、和集合スキーマの表示を行うことができます。 の使用について詳しくは、 [!UICONTROL プロファイル] を参照してください。 [[!DNL Profile] ユーザーガイド](../profile/ui/user-guide.md). リアルタイム顧客プロファイルの詳細については、を参照してください。 [リアルタイム顧客プロファイルの概要](../profile/home.md).

この **[!UICONTROL オーディエンス]** セクションでは、セグメント定義を作成および管理できます。 の使用について詳しくは、 [!UICONTROL オーディエンス] を参照してください。 [セグメント化ユーザーガイド](../segmentation/ui/overview.md). セグメント化サービスの詳細については、次を参照してください。 [セグメント化サービスの概要](../segmentation/home.md).

この **[!UICONTROL ID]** セクションでは、ID 名前空間を作成および管理できます。 について [!UICONTROL ID] id 名前空間と Platform UI での ID の使用方法に関する情報を含む節については、を参照してください。 [id 名前空間の概要](../identity-service/features/namespaces.md).

### [!UICONTROL プライバシー]

この **[!UICONTROL ポリシー]** セクションでは、データ使用ポリシーを作成および管理できます。 「ポリシー」セクションの使用方法について詳しくは、を参照してください。 [データ使用ポリシーユーザーガイド](../data-governance/policies/user-guide.md). データ使用ポリシーの詳細については、を参照してください。 [データ使用ポリシーの概要](../data-governance/policies/overview.md).

この **[!UICONTROL リクエスト]** セクションでは、プライバシーリクエストを作成および管理できます。 Privacy ServiceUI にアクセスするには、許可リストに加えるする必要があることに注意してください。 リクエストの節の使用について詳しくは、を参照してください。 [Privacy Serviceユーザーガイド](../privacy-service/ui/user-guide.md). Privacy Serviceについて詳しくは、を参照してください。 [Privacy Serviceの概要](../privacy-service/home.md).

### [!UICONTROL データサイエンス]

この **[!UICONTROL ノートブック]** の節では、データの調査、分析、モデル化を可能にするインタラクティブな開発環境である JupyterLab へのアクセスについて説明します。 ノートブックの使用について詳しくは、 [JupyterLab ユーザーガイド](../data-science-workspace/jupyterlab/overview.md). Data Science Workspaceについて詳しくは、以下を参照してください。 [Data Science Workspaceの概要](../data-science-workspace/home.md)

この **[!UICONTROL モデル]** セクションでは、機械学習と人工知能を使用して、モデルを作成、開発、トレーニング、調整し、予測を行うことができます。 モデルの節について詳しくは、のチュートリアルを参照してください。 [モデルのトレーニングと評価](../data-science-workspace/models-recipes/train-evaluate-model-ui.md).

この **[!UICONTROL サービス]** セクションでは、スケジュールに沿ったトレーニングやスコアリング用に公開済みモデルを管理したり、Adobeのインテリジェントサービスを使用したりできます。インテリジェントサービスは、パーソナライズされたカスタマーエクスペリエンスをリアルタイムで提供する一連の AI サービスです。 サービスの節について詳しくは、次を参照してください。 [サービスとしてのモデルの公開のチュートリアル](../data-science-workspace/models-recipes/publish-model-service-ui.md).

### [!UICONTROL データ管理]

この **[!UICONTROL スキーマ]** セクションでは、エクスペリエンスデータモデル（XDM）スキーマを作成および管理できます。 スキーマについて詳しくは、のチュートリアルを参照してください。 [スキーマの作成](../xdm/tutorials/create-schema-ui.md). XDM について詳しくは、を参照してください。 [XDM システムの概要](../xdm/home.md).

この **[!UICONTROL データセット]** 「」セクションでは、データセットを作成および管理できます。 データセットの詳細については、次を参照してください。 [データセットユーザーガイド](../catalog/datasets/user-guide.md).

この **[!UICONTROL クエリ]** セクションでは、クエリを作成および管理し、Adobe Experience Platform クエリサービスによって作成された SQL クエリをログに記録し、 [!DNL PostgreSQL] 資格情報。 クエリの詳細については、を参照してください。 [クエリサービスユーザーガイド](../query-service/ui/overview.md).

この **[!UICONTROL 監視]** セクションでは、バッチおよびストリーミングの取り込みを監視できます。 監視の詳細については、を参照してください。 [データ取り込みの監視ユーザーガイド](../ingestion/quality/monitor-data-ingestion.md).

### [!UICONTROL 判定]

Adobe Journey Optimizerは、Experience Platformを基に構築されたアプリケーションサービスです。 強力な意思決定テクノロジーを使用して、すべてのタッチポイントにわたって適切なタイミングで最高のオファーとエクスペリエンスを顧客に提供できます。 の操作など、Journey Optimizerの詳細について説明します [!UICONTROL オファー] および [!UICONTROL アクティビティ] を訪問 [Journey Optimizer ドキュメント](https://experienceleague.adobe.com/docs/journey-optimizer.html?lang=ja).

### [!UICONTROL 管理]

Platform ユーザーインターフェイス（UI）には、毎日のスナップショットで取得した、組織のライセンス使用状況に関する重要な情報を表示できるダッシュボードが用意されています。 次を選択して、このダッシュボードにアクセスします： **[!UICONTROL ライセンス使用状況]** をクリックします。 ライセンス使用状況ダッシュボードについて詳しくは、を参照してください。 [ライセンス使用状況ダッシュボードガイド](./license-usage-and-guardrails/license-usage-dashboard.md).

>[!IMPORTANT]
>
>ライセンス使用状況ダッシュボードの機能は現在アルファ版であり、すべてのユーザーが利用できるわけではありません。 ドキュメントと機能は変更される場合があります。

## 次の手順

このガイドを読むことで、Platform UI のホームページと主なナビゲーション要素について説明しました。 ユーザーインターフェイスでの作業について詳しくは、個々の Platform サービスのドキュメントを参照してください。 このドキュメントへのリンクは、 [左側のナビゲーション](#left-nav) このドキュメントの前半で見つかった節です。
