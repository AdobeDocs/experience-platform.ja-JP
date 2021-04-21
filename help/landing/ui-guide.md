---
keywords: Experience Platform；ホーム；人気の高いトピック；Adobe Experience Platform；ユーザーガイド；uiガイド；プラットフォームuiガイド；プラットフォームの紹介；ダッシュボード;
solution: Experience Platform
title: Experience PlatformUIの概要
topic-legacy: ui guide
description: Adobe Experience Platform
exl-id: 47f9a3fb-731d-4ade-8069-faaa18f224dc
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1755'
ht-degree: 3%

---

# Adobe Experience PlatformUIガイド

このガイドは、Adobe Experience Platformユーザーインターフェイス(UI)の使用方法を紹介し、様々なコンポーネントの用途を説明し、詳細についての詳細ドキュメントへのリンクを提供します。

Adobe Experience Platformの詳細については、[Experience Platformの概要](home.md)を参照してください。

## ホーム画面

Adobe Experience Platformにログインした後、[!UICONTROL ホーム]ページに移動します。このページは、[指標ダッシュボード](#metrics)、[最近のデータ](#recent-data)、[推奨学習](#recommended-learning)の各セクションで構成されています。

![](images/user-guide/homepage.png)

### 指標

指標ダッシュボードには、データセット、プロファイル、セグメントおよび組織内の宛先に関する情報を提供するカードが用意されています。

![](images/user-guide/homepage-dashboard.png)

**[!UICONTROL Datasets]**&#x200B;セクションには、IMS組織内のデータセットの数が表示されます。 新しいデータセットが作成されると、この数値は更新されます。 データセットに関する詳細は、[データセットの概要](../catalog/datasets/overview.md)を参照してください。

**[!UICONTROL プロファイル]**&#x200B;セクションには、IMS組織内のプロファイルを持つ人の合計数が表示されます。プロファイルフラグメントは除きます。 この合計数はアドレス可能なオーディエンスの合計を表し、24時間に1回更新されます。 プロファイルに関する詳細は、[リアルタイム顧客プロファイルの概要](../profile/home.md)を参照してください。

「**[!UICONTROL セグメント]**」セクションには、IMS組織内で作成されたセグメントの合計数が表示されます。 この数値は、新しいセグメントが作成されると更新されます。 セグメントについて詳しくは、[Segmentation Serviceの概要](../segmentation/home.md)を参照してください。

**[!UICONTROL Destinations]**&#x200B;セクションには、IMS組織で作成された宛先の合計数が表示されます。 新しい宛先が作成されると、この番号が更新されます。 宛先に関する詳細は、[宛先の概要](../destinations/home.md)を参照してください。

### 最近のデータ

最新のデータダッシュボードでは、最近作成したデータセット、ソース、セグメント、宛先に関する情報が提供されます。

![](images/user-guide/homepage-recent.png)

**[!UICONTROL 最近のデータセット]**&#x200B;セクションには、IMS組織内で最も新しく作成された5つのデータセットがリストされます。 このリストは、新しいデータセットが作成されるたびに更新されます。 リストからデータセットを選択して、指定したデータセットの詳細を表示するか、**[!UICONTROL 表示all]**&#x200B;を選択して、作成されたすべてのデータセットのリストを確認できます。 データセットに関する詳細は、[データセットの概要](../catalog/datasets/overview.md)を参照してください。

「**[!UICONTROL 最近のソース]**」セクションには、IMS組織内で最も新しく作成されたソースコネクタ5つがリストされます。 このリストは、新しいソースコネクタが作成されるたびに更新されます。 リストからソース接続を選択して、指定した表示の詳細を参照できます。または、**[!UICONTROL 表示all]**&#x200B;を選択して、作成されたすべてのソース接続のリストを確認できます。 ソースに関する詳細は、[ソースの概要](../sources/home.md)を参照してください。

**[!UICONTROL 最近のセグメント]**&#x200B;セクションには、IMS組織内で最も新しく作成された5つのセグメント定義がリストされます。 このリストは、新しいセグメント定義が作成されるたびに更新されます。 リストからセグメント定義を選択して、指定したセグメント定義の詳細を表示するか、「**[!UICONTROL 表示すべて]**」を選択して、作成したすべてのセグメント定義のリストを確認できます。 セグメントについて詳しくは、[Segmentation Serviceの概要](../segmentation/home.md)を参照してください。

**[!UICONTROL Recent destinations]**&#x200B;セクションには、IMS組織内で最も新しく作成された宛先5つがリストされます。 このリストは、新しい宛先が作成されるたびに更新されます。 指定した宛先に関する詳細をリストから表示に表示する宛先を選択するか、作成したすべての宛先のリストを表示するには、**[!UICONTROL 表示all]**&#x200B;を選択します。 宛先に関する詳細は、[宛先の概要](../destinations/home.md)を参照してください。

### 推奨される学習

「**[!UICONTROL 推奨される学習]**」セクションには、Adobe Experience Platformを使い始める際に役立つドキュメントへのリンクが記載されています。

![](images/user-guide/homepage-recommended.png)

## 上部ナビゲーションバー

プラットフォームUIの上部ナビゲーションバーには、現在サインインしているIMS組織が表示され、いくつかの重要なコントロールが用意されています。

ナビゲーションバーの左側には、Adobe Experience Platformのロゴが表示されます。 これを選択すると、プラットフォームのUIのホーム画面が表示されます。

![](./images/user-guide/homepage-top-nav-bar.png)

### IMS組織の切り替えボタン

上部ナビゲーションバーの右側の最初の項目は、**IMS組織の切り替えボタン**&#x200B;です。

![](./images/user-guide/homepage-ims-org.png)

アクセス権を持つIMS組織（利用可能な場合）のドロップダウンメニューが表示されます。 リストからそのIMS組織に切り替えるオプションを選択します。

![](./images/user-guide/homepage-ims-org-switcher.png)

### アプリケーションの切り替え

上部ナビゲーションの右側の次のアイテムは、**アプリケーション切り替えボタン**&#x200B;で、![アプリケーション切り替えボタン](./images/user-guide/app-switcher-icon.png)で表されます。 このアイコンを選択すると、Experience Platform、分析、アセット、起動など、IMS組織がアクセス権を持つAdobeアプリを切り替えることができます。

### ヘルプ

アプリケーション切り替えボタンの右側には、**ヘルプとサポートメニュー**&#x200B;が表示されます。これは、![疑問符/help](./images/user-guide/help-icon.png)アイコンで表されます。 このアイコンを選択すると、ヘルプやサポートのリソースを含むポーバーメニューが表示されます。 「**[!UICONTROL ヘルプ]**」タブには、現在閲覧中のページに関連するドキュメントのリストが表示されます。 「**[!UICONTROL サポート]**」タブを使用すると、Adobeサポートチームとサポートチケットを作成できます。 **[!UICONTROL 「Feedback]**」タブを使用すると、プラットフォームに関するフィードバックをAdobeに送信できます。

![](images/user-guide/homepage-help-clicked.png)

### 通知とお知らせ

**通知セクション**&#x200B;内。![ベル/通知とお知らせ](images/user-guide/notification-icon.png)アイコンで表されます。 「**[!UICONTROL 通知]**」タブには製品やその他の関連する更新に関する重要な情報が表示され、「**[!UICONTROL お知らせ]**」タブにはサービスメンテナンスに関する情報が表示されます。

### ユーザープロファイル

上部ナビゲーションバーの最後の項目は、**ユーザー設定**&#x200B;です。これは、![ユーザー設定/ユーザープロファイル](images/user-guide/profile-icon.png)アイコンで表されます。 環境設定を編集したり、サインアウトしたりするには、このアイコンを選択します。

### サンドボックス

上部ナビゲーションバーのすぐ下に、サンドボックスバーがあります。 このバーは、現在プラットフォームに使用しているサンドボックスを表示します。 サンドボックスについて詳しくは、[サンドボックスの概要](../sandboxes/home.md)を参照してください。

## 左側のナビゲーション{#left-nav}

画面の左側のナビゲーションは、リストUIでサポートされるすべての様々なサービスをします。

>[!IMPORTANT]
>
>左側のナビゲーションバーにある一部のセクションは、表示されない場合やグレー表示になっている場合があります。 これは、これらの機能にアクセスできないためです。 これらのセクションへのアクセス権を持っていると思われる場合は、システム管理者にお問い合わせください。

![](images/user-guide/homepage-left.png)

「**[!UICONTROL ホーム]**」セクションを使用すると、プラットフォームUIのホームページに戻ることができます。

**[!UICONTROL ワークフロー]**&#x200B;セクションは、プラットフォーム内で操作を実行するための複数手順のワークフローのリストを示しています。 ワークフローに関する詳細は、[ワークフローの概要](./workflows.md)を参照してください。

### [!UICONTROL 接続]

**[!UICONTROL ソース]**&#x200B;セクションでは、ソース接続を作成、更新、および削除でき、外部ソースのデータをプラットフォームに取り込むことができます。 ソースに関する詳細は、[ソースの概要](../sources/home.md)を参照してください。

**[!UICONTROL Destinations]**&#x200B;セクションでは、宛先を作成、更新および削除でき、プラットフォームから多くの外部宛先にデータをエクスポートできます。 宛先に関する詳細は、[宛先の概要](../destinations/home.md)を参照してください。

### [!UICONTROL 顧客]

**[!UICONTROL プロファイル]**&#x200B;セクションでは、顧客プロファイル、表示プロファイル指標、結合ポリシーの作成と管理、表示和集合スキーマを参照できます。 [!UICONTROL プロファイル]の使い方の詳細については、[[!DNL Profile] ユーザーガイド](../profile/ui/user-guide.md)をお読みください。 リアルタイム顧客のプロファイルについて詳しくは、[リアルタイム顧客プロファイルの概要](../profile/home.md)を参照してください。

「**[!UICONTROL セグメント]**」セクションでは、セグメント定義を作成および管理できます。 [!UICONTROL セグメント]の使用について詳しくは、[セグメント化ユーザーガイド](../segmentation/ui/overview.md)を参照してください。 Segmentation Serviceの詳細については、[Segmentation Service overview](../segmentation/home.md)を参照してください。

「**[!UICONTROL ID]**」セクションでは、ID名前空間を作成および管理できます。 「ID]」セクションの詳細(ID名前空間に関する情報や、プラットフォームUIでのIDの使用方法など)については、[!UICONTROL ID名前空間の概要](../identity-service/namespaces.md)を参照してください。[

### [!UICONTROL プライバシー]

「**[!UICONTROL ポリシー]**」セクションでは、データ使用ポリシーを作成および管理できます。 「ポリシー」セクションの使用方法の詳細については、[データ使用ポリシーユーザーガイド](../data-governance/policies/user-guide.md)を参照してください。 データ使用ポリシーの詳細については、[データ使用ポリシーの概要](../data-governance/policies/overview.md)を参照してください。

「**[!UICONTROL リクエスト]**」セクションでは、プライバシーリクエストを作成および管理できます。 Privacy ServiceUIにアクセスするには、許可リストに加えるUIが必要です。 「リクエスト」の節の使い方について詳しくは、[Privacy Serviceユーザーガイド](../privacy-service/ui/user-guide.md)をお読みください。 Privacy Serviceに関する詳細は、[Privacy Serviceの概要](../privacy-service/home.md)を参照してください。

### [!UICONTROL データサイエンス]

**[!UICONTROL ノートブック]**&#x200B;セクションは、データの調査、分析、モデル化を可能にするインタラクティブな開発環境であるJupterLabへのアクセスを提供します。 「ノートブック」セクションの使い方の詳細については、[JupyterLabユーザーガイド](../data-science-workspace/jupyterlab/overview.md)を参照してください。 Data Science Workspaceの詳細については、[データサイエンスワークスペースの概要](../data-science-workspace/home.md)を参照してください

**[!UICONTROL モデル]**&#x200B;セクションでは、機械学習と人工知能を活用して、モデルの作成、開発、トレーニング、調整を行い、予測を行うことができます。 「モデル」セクションの詳細については、[トレーニングのチュートリアルで、モデル](../data-science-workspace/models-recipes/train-evaluate-model-ui.md)を評価してください。

「**[!UICONTROL サービス]**」セクションでは、公開されたモデルをスケジュール済みのトレーニングとスコアリング用に管理したり、Adobeのインテリジェントサービスを利用して、リアルタイムでパーソナライズされた顧客体験を提供できます。 サービスに関する詳細は、「[サービスとしてのモデルの公開](../data-science-workspace/models-recipes/publish-model-service-ui.md)」を参照してください。

### [!UICONTROL データ管理]

「**[!UICONTROL スキーマ]**」セクションでは、エクスペリエンスデータモデル(XDM)スキーマを作成および管理できます。 スキーマの詳細については、[スキーマ](../xdm/tutorials/create-schema-ui.md)の作成のチュートリアルを参照してください。 XDMの詳細は[XDMシステム概要](../xdm/home.md)を参照してください。

「**[!UICONTROL データセット]**」セクションでは、データセットの作成と管理が可能です。 データセットに関する詳細は、『[datasetsユーザーガイド](../catalog/datasets/user-guide.md)』を参照してください。

**[!UICONTROL クエリ]**&#x200B;セクションを使用すると、クエリの作成と管理、Adobe Experience Platformクエリサービスが行うSQLクエリのログ、PostgreSQL資格情報の表示を行うことができます。 クエリの詳細については、『クエリサービスユーザガイド](../query-service/ui/overview.md)』を参照してください。[

**[!UICONTROL 監視]**&#x200B;セクションでは、バッチとストリーミング取り込みを監視できます。 監視に関する詳細は、『[監視データ取り込みユーザーガイド](../ingestion/quality/monitor-data-ingestion.md)』を参照してください。

### [!UICONTROL 判定]

Offer Decisioning は、Adobe Experience Platform と統合されたアプリケーションサービスです。これにより、 Experience Platform を活用して、あらゆるタッチポイントで最高のオファーとエクスペリエンスを適切なタイミングで顧客に提供できます。[!UICONTROL オファー]と[!UICONTROL アクティビティ]の使用など、Offer decisioningに関する詳細は、[Offer decisioningドキュメント](https://experienceleague.adobe.com/docs/offer-decisioning.html)を参照してください。

### [!UICONTROL 管理]

UI(Platform User Interface)を使用すると、毎日のスナップショットでキャプチャされる、組織のライセンスの使用に関する重要な情報を表示できるダッシュボードが提供されます。 これにアクセスするには、ナビゲーションで&#x200B;**[!UICONTROL ライセンスの使用状況]**&#x200B;を選択します。 ライセンス使用ダッシュボードの詳細については、[ライセンス使用ダッシュボードガイド](license-usage-dashboard.md)を参照してください。

>[!IMPORTANT]
>
>ライセンス使用ダッシュボード機能は、現在アルファベット順に表示されており、すべてのユーザが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

## 次の手順

このガイドを読むことで、Platform UIのホームページと主要なナビゲーション要素について紹介します。 ユーザーインターフェイスでの作業に関する詳細は、各プラットフォームサービスのドキュメントを参照してください。 このドキュメントへのリンクは、このドキュメントの前の[左側のナビゲーション](#left-nav)のセクションにあります。
