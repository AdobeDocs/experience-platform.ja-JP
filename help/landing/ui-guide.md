---
keywords: Experience Platform；ホーム；人気のあるトピック；Adobe Experience Platform；ユーザーガイド；uiガイド；platform uiガイド；platformの概要；ダッシュボード；
solution: Experience Platform
title: Experience PlatformUIの概要
topic-legacy: ui guide
description: Adobe Experience Platform
exl-id: 47f9a3fb-731d-4ade-8069-faaa18f224dc
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '1755'
ht-degree: 4%

---

# Adobe Experience Platform UIガイド

このガイドは、Adobe Experience Platformユーザーインターフェイス(UI)の使用方法の概要として機能し、様々なコンポーネントの用途を説明し、詳細なドキュメントへのリンクを提供します。

Adobe Experience Platformの詳細については、「[Experience Platformの概要](home.md)」を参照してください。

## ホーム画面

Adobe Experience Platformにログインすると、[!UICONTROL ホーム]ページが表示されます。このページは、[指標ダッシュボード](#metrics)、[最近のデータ](#recent-data)、[推奨学習](#recommended-learning)の各セクションで構成されます。

![](images/user-guide/homepage.png)

### 指標

指標ダッシュボードは、組織内のデータセット、プロファイル、セグメントおよび宛先に関する情報を提供するカードを提供します。

![](images/user-guide/homepage-dashboard.png)

**[!UICONTROL データセット]**&#x200B;セクションには、IMS組織内のデータセットの数が表示されます。 この数は、新しいデータセットが作成されると更新されます。 データセットの詳細については、「[データセットの概要](../catalog/datasets/overview.md)」を参照してください。

「**[!UICONTROL Profiles]**」セクションには、IMS組織内のプロファイルを持つ人の合計数（プロファイルフラグメントを除く）が表示されます。 この合計ユーザー数は、アドレス可能なオーディエンスの合計を表し、24時間ごとに更新されます。 プロファイルについて詳しくは、「[リアルタイム顧客プロファイルの概要](../profile/home.md)」を参照してください。

「**[!UICONTROL セグメント]**」セクションには、IMS組織内で作成されたセグメントの合計数が表示されます。 この数は、新しいセグメントが作成されると更新されます。 セグメントについて詳しくは、「[セグメント化サービスの概要](../segmentation/home.md)」を参照してください。

「**[!UICONTROL 宛先]**」セクションには、IMS組織用に作成された宛先の合計数が表示されます。 この数は、新しい宛先が作成されると更新されます。 宛先について詳しくは、「[宛先の概要](../destinations/home.md)」を参照してください。

### 最近のデータ

最近使用したデータダッシュボードには、最近作成したデータセット、ソース、セグメント、宛先に関する情報が表示されます。

![](images/user-guide/homepage-recent.png)

「**[!UICONTROL 最近のデータセット]**」セクションには、IMS組織内で最も新しく作成された5つのデータセットがリストされます。 このリストは、新しいデータセットが作成されるたびに更新されます。 リストからデータセットを選択して、指定したデータセットの詳細を表示するか、「**[!UICONTROL すべての]**&#x200B;を表示」を選択して、作成したすべてのデータセットのリストを表示します。 データセットの詳細については、「[データセットの概要](../catalog/datasets/overview.md)」を参照してください。

「**[!UICONTROL 最近のソース]**」セクションには、IMS組織内で最も新しく作成された5つのソースコネクタが表示されます。 このリストは、新しいソースコネクタが作成されるたびに更新されます。 リストからソース接続を選択して、指定したコネクタの詳細を表示するか、「**[!UICONTROL すべて表示]**」を選択して、作成したすべてのソース接続のリストを表示できます。 ソースに関する詳細は、[ソースの概要](../sources/home.md)を参照してください。

「**[!UICONTROL 最近のセグメント]**」セクションには、IMS組織内で最も新しく作成された5つのセグメント定義が表示されます。 このリストは、新しいセグメント定義が作成されるたびに更新されます。 リストからセグメント定義を選択して、指定したセグメント定義の詳細を表示するか、「**[!UICONTROL すべて表示]**」を選択して、作成したすべてのセグメント定義のリストを表示します。 セグメントについて詳しくは、「[セグメント化サービスの概要](../segmentation/home.md)」を参照してください。

「**[!UICONTROL 最近の宛先]**」セクションには、IMS組織内で最近作成された5つの宛先が一覧表示されます。 このリストは、新しい宛先が作成されるたびに更新されます。 リストから宛先を選択して、指定した宛先に関する詳細情報を表示するか、「**[!UICONTROL すべて]**&#x200B;を表示」を選択して、作成したすべての宛先のリストを表示できます。 宛先について詳しくは、「[宛先の概要](../destinations/home.md)」を参照してください。

### 推奨学習

「**[!UICONTROL 推奨学習]**」セクションには、Adobe Experience Platformの使用を開始するために役立つドキュメントへのリンクが記載されています。

![](images/user-guide/homepage-recommended.png)

## 上部ナビゲーションバー

Platform UIの上部ナビゲーションバーには、現在サインインしているIMS組織が表示され、いくつかの重要なコントロールが用意されています。

ナビゲーションバーの左側には、Adobe Experience Platformのロゴが表示されます。 これをいつでも選択すると、Platform UIのホーム画面に戻ります。

![](./images/user-guide/homepage-top-nav-bar.png)

### IMS組織の切り替えボタン

上部ナビゲーションバーの右側の最初の項目は、**IMS組織切り替えボタン**&#x200B;です。

![](./images/user-guide/homepage-ims-org.png)

アクセス権があるIMS組織のドロップダウンメニューが表示されます（使用可能な場合）。 リストからそのIMS組織に切り替えるオプションを選択します。

![](./images/user-guide/homepage-ims-org-switcher.png)

### アプリケーションの切り替え

上部ナビゲーションの右側の次の項目は、**アプリケーション切り替えボタン**&#x200B;で、![アプリケーション切り替えボタン](./images/user-guide/app-switcher-icon.png)アイコンで表されます。 このアイコンを選択すると、IMS組織がアクセスできるAdobeアプリケーション(Experience Platform、Analytics、Assetsなど)を切り替えることができます。

### ヘルプ

アプリケーション切り替えボタンの右側には、**ヘルプとサポートメニュー**&#x200B;が表示されます。このメニューは、![疑問符/ヘルプ](./images/user-guide/help-icon.png)アイコンで表されます。 このアイコンを選択すると、いくつかのヘルプおよびサポートリソースを含むポップオーバーメニューが表示されます。 「**[!UICONTROL ヘルプ]**」タブには、現在参照しているページに関連するドキュメントのリストが表示されます。 「**[!UICONTROL サポート]**」タブを使用すると、Adobeサポートチームと共にサポートチケットを作成できます。 「**[!UICONTROL フィードバック]**」タブを使用すると、Platformに関するフィードバックをAdobeに送信できます。

![](images/user-guide/homepage-help-clicked.png)

### 通知とお知らせ

**通知セクション**&#x200B;で、![ベル/通知とお知らせ](images/user-guide/notification-icon.png)アイコンで表されます。 「**[!UICONTROL 通知]**」タブには製品やその他の関連する更新に関する重要な情報が表示され、「**[!UICONTROL お知らせ]**」タブにはサービスメンテナンスに関する情報が表示されます。

### ユーザープロファイル

上部ナビゲーションバーの最後の項目は&#x200B;**ユーザー設定**&#x200B;です。これは、![ユーザー設定/ユーザープロファイル](images/user-guide/profile-icon.png)アイコンで表されます。 環境設定を編集またはログアウトするには、このアイコンを選択します。

### サンドボックス

上部ナビゲーションバーのすぐ下に、サンドボックスバーが表示されます。 このバーには、Platform用に現在使用しているサンドボックスが表示されます。 サンドボックスについて詳しくは、「[サンドボックスの概要](../sandboxes/home.md)」を参照してください。

## 左側のナビゲーション {#left-nav}

画面の左側のナビゲーションには、Platform UIでサポートされるすべての様々なサービスが表示されます。

>[!IMPORTANT]
>
>左側のナビゲーションバーの一部のセクションが、表示されない、または灰色表示になっている場合があります。 これは、これらの機能にアクセスできないためです。 これらの節へのアクセス権が必要な場合は、システム管理者にお問い合わせください。

![](images/user-guide/homepage-left.png)

「**[!UICONTROL ホーム]**」セクションを使用して、Platform UIのホームページに戻ることができます。

**[!UICONTROL ワークフロー]**&#x200B;セクションには、Platform内で操作を実行するための複数手順のワークフローのリストが表示されます。 ワークフローについて詳しくは、[ワークフローの概要](./workflows.md)を参照してください。

### [!UICONTROL 接続]

**[!UICONTROL 「ソース]**」セクションを使用して、ソース接続を作成、更新および削除し、外部ソースからPlatformにデータを取り込むことができます。 ソースに関する詳細は、[ソースの概要](../sources/home.md)を参照してください。

**[!UICONTROL 「宛先]**」セクションを使用すると、宛先を作成、更新および削除でき、Platformから多くの外部宛先にデータを書き出すことができます。 宛先について詳しくは、「[宛先の概要](../destinations/home.md)」を参照してください。

### [!UICONTROL 顧客]

**[!UICONTROL 「Profiles]**」セクションでは、顧客プロファイルの参照、プロファイル指標の表示、結合ポリシーの作成と管理、和集合スキーマの表示をおこなえます。 「[!UICONTROL プロファイル]」の節の使用方法について詳しくは、『[[!DNL Profile] ユーザーガイド](../profile/ui/user-guide.md)』を参照してください。 リアルタイム顧客プロファイルの詳細については、「[リアルタイム顧客プロファイルの概要](../profile/home.md)」を参照してください。

**[!UICONTROL 「セグメント]**」セクションでは、セグメント定義を作成および管理できます。 「[!UICONTROL セグメント]」の節の使用方法について詳しくは、『[セグメント化ユーザーガイド](../segmentation/ui/overview.md)』を参照してください。 セグメント化サービスの詳細については、「[セグメント化サービスの概要](../segmentation/home.md)」を参照してください。

**[!UICONTROL ID]**&#x200B;セクションを使用して、ID名前空間を作成および管理できます。 ID名前空間に関する情報やPlatform UIでのIDの使用方法など、「[!UICONTROL ID]」セクションについて詳しくは、「[ID名前空間の概要](../identity-service/namespaces.md)」を参照してください。

### [!UICONTROL プライバシー]

**[!UICONTROL Policies]**&#x200B;セクションでは、データ使用ポリシーを作成および管理できます。 ポリシーの節の使用について詳しくは、『[データ使用ポリシーユーザガイド](../data-governance/policies/user-guide.md)』を参照してください。 データ使用ポリシーの詳細については、「[データ使用ポリシーの概要](../data-governance/policies/overview.md)」を参照してください。

**[!UICONTROL 「リクエスト]**」セクションでは、プライバシーリクエストを作成および管理できます。 Privacy ServiceUIにアクセスするには、許可リストに加えるが必要です。 「リクエスト」の節の使用方法について詳しくは、『[Privacy Serviceユーザーガイド](../privacy-service/ui/user-guide.md)』を参照してください。 Privacy Serviceの詳細については、「[Privacy Serviceの概要](../privacy-service/home.md)」を参照してください。

### [!UICONTROL データサイエンス]

「**[!UICONTROL ノートブック]**」セクションでは、JupyterLab（データの調査、分析、モデル化をおこなえるインタラクティブな開発環境）にアクセスできます。 「ノートブック」の節の使用に関する詳細については、JupyterLabのユーザーガイド](../data-science-workspace/jupyterlab/overview.md)を参照してください。 [Data Science Workspaceの詳細については、「[Data Science Workspaceの概要](../data-science-workspace/home.md)」を参照してください。

**[!UICONTROL 「モデル]**」セクションを使用すると、機械学習と人工知能を活用して、モデルの作成、開発、トレーニング、調整を行い、予測を行うことができます。 モデルの節の詳細については、[トレーニングとモデル](../data-science-workspace/models-recipes/train-evaluate-model-ui.md)の評価に関するチュートリアルを参照してください。

**[!UICONTROL 「サービス]**」セクションを使用すると、スケジュールに沿ったトレーニングとスコアリングの公開済みモデルを管理したり、Adobeのインテリジェントサービスを活用して、パーソナライズされたリアルタイムの顧客体験を提供できます。 「サービス」の節について詳しくは、「[サービスとしてのモデルの公開](../data-science-workspace/models-recipes/publish-model-service-ui.md)」のチュートリアルを参照してください。

### [!UICONTROL データ管理]

**[!UICONTROL 「スキーマ]**」セクションでは、エクスペリエンスデータモデル(XDM)スキーマを作成および管理できます。 スキーマの詳細については、[スキーマ](../xdm/tutorials/create-schema-ui.md)の作成に関するチュートリアルを参照してください。 XDMについて詳しくは、「[XDMシステムの概要](../xdm/home.md)」を参照してください。

**[!UICONTROL 「データセット]**」セクションでは、データセットを作成および管理できます。 データセットの詳細については、『[データセットユーザガイド](../catalog/datasets/user-guide.md)』を参照してください。

**[!UICONTROL クエリ]**&#x200B;セクションを使用すると、クエリの作成と管理、Adobe Experience PlatformクエリサービスによるSQLクエリのログの作成、PostgreSQL資格情報の表示を行うことができます。 クエリについて詳しくは、『[クエリサービスユーザガイド](../query-service/ui/overview.md)』を参照してください。

「**[!UICONTROL 監視]**」セクションでは、バッチおよびストリーミングの取り込みを監視できます。 監視について詳しくは、『[データ取得ユーザーガイド](../ingestion/quality/monitor-data-ingestion.md)』を参照してください。

### [!UICONTROL 判定]

Offer Decisioning は、Adobe Experience Platform と統合されたアプリケーションサービスです。これにより、 Experience Platform を活用して、あらゆるタッチポイントで最高のオファーとエクスペリエンスを適切なタイミングで顧客に提供できます。[!UICONTROL オファー]と[!UICONTROL アクティビティ]の使用を含む、Offer decisioningについて詳しくは、[Offer decisioningのドキュメント](https://experienceleague.adobe.com/docs/offer-decisioning.html?lang=ja)を参照してください。

### [!UICONTROL 管理]

Platformユーザーインターフェイス(UI)は、毎日のスナップショットでキャプチャされた、組織のライセンス使用状況に関する重要な情報を表示できるダッシュボードを提供します。 これには、ナビゲーションで「**[!UICONTROL ライセンスの使用状況]**」を選択してアクセスできます。 ライセンス使用状況ダッシュボードの詳細については、『[ライセンス使用状況ダッシュボードガイド](license-usage-dashboard.md)』を参照してください。

>[!IMPORTANT]
>
>ライセンス使用状況ダッシュボード機能は、現在アルファ版で、一部のユーザーが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

## 次の手順

このガイドを読むことで、Platform UIのホームページと主要なナビゲーション要素が紹介されました。 ユーザーインターフェイスでの作業について詳しくは、個々のPlatformサービスのドキュメントを参照してください。 このドキュメントへのリンクは、このドキュメントの前の[左側のナビゲーション](#left-nav)の節に記載されています。
