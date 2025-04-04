---
title: Ui を使用した PathFactory アカウントのExperience Platformへの接続
description: UI を通じて PathFactory アカウントをExperience Platformに接続する方法を説明します。
badge: ベータ版
exl-id: 859dd0c1-8c4b-43e3-a87b-84c879460bc0
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '572'
ht-degree: 18%

---

# UI を使用した [!DNL PathFactory] アカウントのExperience Platformへの接続

このチュートリアルでは、[!DNL PathFactory] 訪問者、セッションおよびページビューのデータを UI を通じてAdobe Experience Platformに接続する方法の手順を説明します。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に [!DNL PathFactory] アカウントを持っている場合は、このドキュメントの残りの部分をスキップし、[UI を使用したExperience Platformへのマーケティング自動化データの取り込み ](../../dataflow/marketing-automation.md) に関するチュートリアルに進むことができます。

### 必要な資格情報の収集 {#gather-credentials}

Experience Platformで PathFactory アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| ユーザー名 | PathFactory アカウントのユーザー名。 これは、システム内のアカウントを識別するために不可欠です。 |
| パスワード | PathFactory アカウントに関連付けられたパスワード。 権限のないアクセスを防ぐために、これが安全に保たれていることを確認します。 |
| ドメイン | PathFactory アカウントに関連付けられたドメイン。 これは通常、PathFactory URL 内の一意の ID を参照します。 |
| アクセストークン | システムと PathFactory 間の安全な通信を確保するために、API 認証に使用される一意のトークン。 |
| API エンドポイント | データにアクセスするための特定の API エンドポイント：訪問者、セッションおよびページビュー。 各エンドポイントは、取得可能な様々なデータセットに対応します。 **メモ：** これらは [!DNL PathFactory] で事前定義され、アクセスする予定のデータに固有です。 <ul><li>**訪問者エンドポイント**:`/api/public/v3/data_lake_apis/visitors.json`</li><li>**セッション エンドポイント**: `/api/public/v3/data_lake_apis/sessions.json`</li><li>**ページビューエンドポイント**: `/api/public/v3/data_lake_apis/page_views.json`</li></ul> |

資格情報の保護および使用方法に関するガイダンスと、アクセストークンの取得および更新に関する情報については、[PathFactory サポートセンター ](https://support.pathfactory.com/categories/adobe/) を参照してください。 このリソースでは、資格情報の管理と、効果的で安全な API 統合の確保に関する包括的なガイドを提供します。


## [!DNL PathFactory] アカウントを接続

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 [!UICONTROL  カタログ ] には、Experience Platformでサポートされている様々なソースが表示されます。

カテゴリのリストから適切なカテゴリを選択できます。 検索バーを使用して、特定のソースをフィルタリングすることもできます。

[!UICONTROL  マーケティングオートメーション ] カテゴリで、**[!UICONTROL PathFactory]** を選択してから **[!UICONTROL 設定]** を選択します。

![ ソースカタログと PathFactory ソースが選択されています。](../../../../images/tutorials/create/pathfactory/catalog.png)

**[!UICONTROL PathFactory に接続]** ページが表示されます。 このページでは、新しいアカウントを作成するか、既存のアカウントを使用できます。

### 新規アカウント

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、アカウントの名前、説明（オプション）、[!DNL PathFactory] アカウントに対応する認証資格情報を入力します。

終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![PathFactory の新しいアカウントを認証できる新しいアカウントインターフェイス。](../../../../images/tutorials/create/pathfactory/new.png)

### 既存のアカウント

既存のアカウントがある場合は、「**[!UICONTROL 既存のアカウント]**」を選択し、表示されるリストから使用するアカウントを選択します。

![ 既存の PathFactory アカウントのリストから選択できる既存のアカウントインターフェイス。](../../../../images/tutorials/create/pathfactory/existing.png)

## 次の手順

このチュートリアルでは、[!DNL PathFactory] アカウントとExperience Platformの間の接続を確立しました。 次のチュートリアルに進み、[ マーケティング自動化データをExperience Platformに取り込むためのデータフローの作成 ](../../dataflow/marketing-automation.md) を行いましょう。
