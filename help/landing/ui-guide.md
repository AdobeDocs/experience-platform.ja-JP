---
keywords: Experience Platform;home;popular topics;Adobe Experience Platform;user guide;ui guide;platform ui guide;introduction to platform;dashboard;
solution: Experience Platform
title: Adobe Experience PlatformUIガイド
topic: ui guide
description: 'Adobe Experience Platform '
translation-type: tm+mt
source-git-commit: 852792c1288cf7b4815fb0afb742046d7a595da2
workflow-type: tm+mt
source-wordcount: '1737'
ht-degree: 1%

---


# Adobe Experience PlatformUIガイド

このガイドは、Adobe Experience Platformユーザーインターフェイス(UI)の使用方法を紹介し、様々なコンポーネントの用途を説明し、詳細についての詳細ドキュメントへのリンクを提供します。

Adobe Experience Platformの詳細については、 [Experience Platform概要を参照してください](home.md)。

## ホーム画面

Adobe Experience Platformにログインすると、 [!UICONTROL ホーム] ページが開きます。このページは、 [指標ダッシュボード](#metrics)、最近のデータ [、](#recent-data)および [](#recommended-learning) 推奨学習セクションで構成されています。

![](images/user-guide/homepage.png)

### 指標

指標ダッシュボードには、データセット、プロファイル、セグメントおよび組織内の宛先に関する情報を提供するカードが用意されています。

![](images/user-guide/homepage-dashboard.png)

「 **[!UICONTROL Datasets]** 」セクションには、IMS組織内のデータセットの数が表示されます。 新しいデータセットが作成されると、この数値は更新されます。 データセットの詳細については、「 [データセットの概要](../catalog/datasets/overview.md)」を参照してください。

「 **[!UICONTROL プロファイル]** 」セクションには、IMS組織内のプロファイルを持つユーザーの合計数(プロファイルフラグメントを除く)が表示されます。 この合計数はアドレス可能なオーディエンスの合計を表し、24時間に1回更新されます。 プロファイルの詳細については、 [リアルタイム顧客プロファイルの概要を参照してください](../profile/home.md)。

「 **[!UICONTROL セグメント]** 」セクションには、IMS組織内で作成されたセグメントの合計数が表示されます。 この数値は、新しいセグメントが作成されると更新されます。 セグメントについて詳しくは、 [Segmentation Serviceの概要を参照してください](../segmentation/home.md)。

The **[!UICONTROL Destinations]** section shows the total number of destinations created for the IMS Organization. 新しい宛先が作成されると、この番号が更新されます。 宛先について詳しくは、 [宛先の概要を参照してください](../destinations/home.md)。

### 最近のデータ

最新のデータダッシュボードでは、最近作成したデータセット、ソース、セグメント、宛先に関する情報が提供されます。

![](images/user-guide/homepage-recent.png)

「 **[!UICONTROL 最近のデータセット]** 」セクションには、IMS組織内で最も新しく作成された5つのデータセットがリストされます。 このリストは、新しいデータセットが作成されるたびに更新されます。 リストからデータセットを選択し、指定したデータセットの詳細を表示できます。または、「すべて **[!UICONTROL 表示]** 」を選択して、作成されたすべてのデータセットのリストを確認できます。 データセットの詳細については、「 [データセットの概要](../catalog/datasets/overview.md)」を参照してください。

「 **[!UICONTROL 最新のソース]** 」セクションには、IMS組織内で最も新しく作成されたソースコネクタが5つリストされます。 このリストは、新しいソースコネクタが作成されるたびに更新されます。 指定したコネクタの詳細をリストから表示に表示するソース接続を選択するか、作成したすべてのソース接続のリストを表示するには **[!UICONTROL 表示]** 「すべて」を選択します。 ソースの詳細については、「 [ソースの概要](../sources/home.md)」を参照してください。

「 **[!UICONTROL 最近のセグメント]** 」セクションには、IMS組織内で最も新しく作成された5つのセグメント定義がリストされます。 このリストは、新しいセグメント定義が作成されるたびに更新されます。 リストからセグメント定義を選択して、指定したセグメント定義の詳細を表示するか、「 **[!UICONTROL 表示すべて]** 」を選択して、作成したすべてのセグメント定義のリストを確認できます。 セグメントについて詳しくは、 [Segmentation Serviceの概要を参照してください](../segmentation/home.md)。

「 **[!UICONTROL Recent destinations]** 」セクションには、IMS組織内で最も最近作成された5つの宛先がリストされます。 このリストは、新しい宛先が作成されるたびに更新されます。 リストから表示への宛先を選択して、指定した宛先に関する詳細を表示するか、「 **[!UICONTROL 表示すべて]** 」を選択して、作成したすべての宛先のリストを表示できます。 宛先について詳しくは、 [宛先の概要を参照してください](../destinations/home.md)。

### 推奨される学習

「 **[!UICONTROL 推奨学習]** 」セクションには、Adobe Experience Platformを使い始める際に役立つドキュメントへのリンクが記載されています。

![](images/user-guide/homepage-recommended.png)

## 上部ナビゲーションバー

プラットフォームUIの上部ナビゲーションバーには、現在サインインしているIMS組織が表示され、いくつかの重要なコントロールが用意されています。

ナビゲーションバーの左側には、Adobe Experience Platformのロゴが表示されます。 これを選択すると、プラットフォームのUIのホーム画面が表示されます。

![](./images/user-guide/homepage-top-nav-bar.png)

### IMS組織の切り替えボタン

上部ナビゲーションバーの右側の最初の項目は、 **IMS組織の切り替えボタン**&#x200B;です。

![](./images/user-guide/homepage-ims-org.png)

アクセス権を持つIMS組織（利用可能な場合）のドロップダウンメニューが表示されます。 リストからそのIMS組織に切り替えるオプションを選択します。

![](./images/user-guide/homepage-ims-org-switcher.png)

### アプリケーションの切り替え

上部ナビゲーションの右側にある次のアイテムは、 **アプリ切り替えボタン**&#x200B;で、 ![](./images/user-guide/app-switcher-icon.png) アプリ切り替えアイコンで表されます。 このアイコンを選択すると、Experience Platform、分析、アセット、起動など、IMS組織がアクセス権を持つAdobeアプリを切り替えることができます。

### ヘルプ

アプリケーションの切り替えボタンの右側には、 **ヘルプとサポートのメニューが表示されます**。このメニューは ![、](./images/user-guide/help-icon.png) 疑問符/ヘルプアイコンで表されます。 このアイコンを選択すると、ヘルプやサポートのリソースを含むポーバーメニューが表示されます。 「 **[!UICONTROL ヘルプ]** 」タブには、現在参照しているページに関連するドキュメントのリストが表示されます。 「 **[!UICONTROL サポート]** 」タブでは、Adobeサポートチームとサポートチケットを作成できます。 「 **[!UICONTROL Feedback]** 」タブを使用すると、プラットフォームに関するフィードバックをAdobeに送信できます。

![](images/user-guide/homepage-help-clicked.png)

### 通知とお知らせ

「 **通知」セクション**。 ![ベル/通知とお知らせ](images/user-guide/notification-icon.png) アイコンで表されます。 「 **[!UICONTROL 通知]** 」タブには製品やその他の関連する更新に関する重要な情報が表示され、「お知らせ **** 」タブにはサービスのメンテナンスに関する情報が表示されます。

### ユーザープロファイル

上部ナビゲーションバーの最後の項目は **ユーザー設定**&#x200B;です。これは、 ![ユーザー設定/ユーザープロファイル](images/user-guide/profile-icon.png) アイコンで表されます。 環境設定を編集したり、サインアウトしたりするには、このアイコンを選択します。

### サンドボックス

上部ナビゲーションバーのすぐ下に、サンドボックスバーがあります。 このバーは、現在プラットフォームに使用しているサンドボックスを表示します。 サンドボックスについて詳しくは、 [サンドボックスの概要を参照してください](../sandboxes/home.md)。

## 左側のナビゲーション {#left-nav}

画面の左側のナビゲーションは、リストUIでサポートされるすべての様々なサービスをします。

>[!IMPORTANT]
>
>左側のナビゲーションバーにある一部のセクションは、表示されない場合やグレー表示になっている場合があります。 これは、これらの機能にアクセスできないためです。 これらのセクションへのアクセス権を持っていると思われる場合は、システム管理者にお問い合わせください。

![](images/user-guide/homepage-left.png)

「 **[!UICONTROL ホーム]** 」セクションでは、プラットフォームUIのホームページに戻ることができます。

「 **[!UICONTROL ワークフロー]** 」セクションには、プラットフォーム内で操作を実行するための複数手順のワークフローのリストが表示されます。 ワークフローの詳細については、 [ワークフローの概要を参照してください](./workflows.md)。

### [!UICONTROL 接続]

「 **[!UICONTROL ソース]** 」セクションでは、ソース接続を作成、更新、および削除でき、外部ソースのデータをプラットフォームに取り込むことができます。 ソースの詳細については、「 [ソースの概要](../sources/home.md)」を参照してください。

「 **[!UICONTROL 宛先]** 」セクションでは、宛先を作成、更新および削除でき、プラットフォームから多くの外部宛先にデータをエクスポートできます。 宛先について詳しくは、 [宛先の概要を参照してください](../destinations/home.md)。

### [!UICONTROL 顧客]

「 **[!UICONTROL プロファイル]** 」セクションでは、顧客プロファイル、表示プロファイル指標、結合ポリシーの作成と管理、表示和集合スキーマを参照できます。 「 [!UICONTROL プロファイル] 」セクションの使用方法の詳細については、 [[!DNL Profile] ユーザガイドを参照してください](../profile/ui/user-guide.md)。 リアルタイム顧客のプロファイルについて詳しくは、 [リアルタイム顧客プロファイルの概要を参照してください](../profile/home.md)。

「 **[!UICONTROL セグメント]** 」セクションでは、セグメント定義を作成および管理できます。 「 [!UICONTROL セグメント] 」セクションの使用方法の詳細については、「 [セグメント化ユーザーガイド](../segmentation/ui/overview.md)」を参照してください。 Segmentation Serviceについて詳しくは、 [Segmentation Serviceの概要を参照してください](../segmentation/home.md)。

「 **[!UICONTROL ID]** 」セクションでは、ID名前空間を作成および管理できます。 「 [!UICONTROL ID] 」セクションの詳細(ID名前空間に関する情報や、Platform UIでのIDの使用方法など)については、「 [ID名前空間の概要](../identity-service/namespaces.md)」を参照してください。

### [!UICONTROL プライバシー]

「 **[!UICONTROL ポリシー]** 」セクションでは、データ使用ポリシーを作成および管理できます。 「ポリシー」セクションの使用方法の詳細については、『 [データ使用ポリシーユーザーガイド](../data-governance/policies/user-guide.md)』を参照してください。 データ使用ポリシーの詳細については、「 [データ使用ポリシーの概要](../data-governance/policies/overview.md)」を参照してください。

「 **[!UICONTROL リクエスト]** 」セクションでは、プライバシーリクエストを作成および管理できます。 Privacy ServiceUIにアクセスするには、リストに記載されている必要があります。 「リクエスト」セクションの使用方法の詳細については、 [Privacy Serviceユーザーガイドを参照してください](../privacy-service/ui/user-guide.md)。 Privacy Serviceの詳細については、 [Privacy Serviceの概要を参照してください](../privacy-service/home.md)。

### [!UICONTROL データ科学]

「 **[!UICONTROL ノートブック]** 」セクションからJupyterLabにアクセスできます。JupyterLabは、データの調査、分析、モデル化を行う、インタラクティブな開発環境です。 「ノートブック」セクションの使用方法の詳細については、 [JupterLabユーザーガイドを参照してください](../data-science-workspace/jupyterlab/overview.md)。 Data Science Workspaceの詳細については、「 [Data Science Workspaceの概要」を参照してください](../data-science-workspace/home.md)

「 **[!UICONTROL モデル]** 」セクションでは、機械学習と人工知能を活用して、モデルの作成、開発、トレーニング、調整を行い、予測を行うことができます。 モデルに関する詳細は、 [トレーニングとモデルの評価に関するチュートリアルで確認できます](../data-science-workspace/models-recipes/train-evaluate-model-ui.md)。

「 **[!UICONTROL サービス]** 」セクションでは、公開されたモデルをスケジュールされたトレーニングとスコアリング用に管理したり、Adobeのインテリジェントサービスを活用して、リアルタイムでパーソナライズされた顧客体験を提供したりできます。 サービスに関する詳細は、「サービスとしてのモデルの [発行」チュートリアルを参照してください](../data-science-workspace/models-recipes/publish-model-service-ui.md)。

### [!UICONTROL データ管理]

「 **[!UICONTROL スキーマ]** 」セクションでは、Experience Data Model(XDM)スキーマを作成および管理できます。 スキーマの詳細については、スキーマの [作成に関するチュートリアルを参照してください](../xdm/tutorials/create-schema-ui.md)。 More information about XDM can be found in the [XDM System overview](../xdm/home.md).

「 **[!UICONTROL Datasets]** 」セクションでは、データセットの作成と管理を行うことができます。 データセットの詳細については、『 [datasetsユーザガイド](../catalog/datasets/user-guide.md)』を参照してください。

[ **[!UICONTROL クエリ]** ]セクションでは、クエリの作成と管理、Adobe Experience Platformクエリサービスが行うSQLクエリのログ、PostgreSQL資格情報の表示を行うことができます。 クエリの詳細については、『 [クエリサービスユーザガイド](../query-service/ui/overview.md)』を参照してください。

「 **[!UICONTROL 監視]** 」セクションでは、バッチおよびストリーミング取り込みを監視できます。 監視に関する詳細は、『 [監視データ取り込みユーザーガイド](../ingestion/quality/monitor-data-ingestion.md)』を参照してください。

### [!UICONTROL 判定]

Offer Decisioningは、Adobe Experience Platformと統合された申し込みサービスです。 Experience Platformを活用して、すべてのタッチポイントにわたって適切なタイミングで最高のオファーと経験を顧客に提供できます。 [!UICONTROL オファー] や [!UICONTROL アクティビティとの連携など、Offer Decisioningの詳細については、] Offer Decisioningのドキュメントを参照してください [](https://experienceleague.adobe.com/docs/offer-decisioning.html)。

### [!UICONTROL 管理]

UI(Platform User Interface)を使用すると、毎日のスナップショットでキャプチャされる、組織のライセンスの使用に関する重要な情報を表示できるダッシュボードが提供されます。 これには、ナビゲーションで[ **[!UICONTROL ライセンスの使用状況]** ]を選択してアクセスします。 ライセンス使用ダッシュボードの詳細については、『 [ライセンス使用ダッシュボードガイド](license-usage-dashboard.md)』を参照してください。

>[!IMPORTANT]
>
>ライセンス使用ダッシュボード機能は、現在アルファベット順に表示されており、すべてのユーザが使用できるわけではありません。 ドキュメントと機能は変更される場合があります。

## 次の手順

このガイドを読むことで、Platform UIのホームページと主要なナビゲーション要素について紹介します。 ユーザーインターフェイスでの作業に関する詳細は、各プラットフォームサービスのドキュメントを参照してください。 このドキュメントへのリンクは、このドキュメントの前の [左側のナビゲーション](#left-nav) ・セクションに記載されています。