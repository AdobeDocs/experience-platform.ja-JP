---
title: Ui を使用して PathFactory アカウントをExperience Platformに接続する
description: UI を使用して PathFactory アカウントをExperience Platformに接続する方法を説明します。
last-substantial-update: 2024-04-30T00:00:00Z
hide: true
hidefromtoc: true
badge: ベータ版
source-git-commit: 18f6c253aec6815cf84272cbce340a9aa7ed8ab9
workflow-type: tm+mt
source-wordcount: '570'
ht-degree: 21%

---

# を接続 [!DNL PathFactory] ui を通じてExperience Platformにアカウントを割り当てる

このチュートリアルでは、を接続する手順を説明します [!DNL PathFactory] UI を通じてAdobe Experience Platformに送信される訪問者数、セッション数およびページビュー数。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既にがある場合 [!DNL PathFactory] アカウントの場合は、このドキュメントの残りの部分をスキップし、に関するチュートリアルに進むことができます。 [ui を使用したマーケティング自動化データのExperience Platformへの取り込み](../../dataflow/marketing-automation.md).

### 必要な資格情報の収集 {#gather-credentials}

Platform で PathFactory アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| ユーザー名 | PathFactory アカウントのユーザー名。 これは、システム内のアカウントを識別するために不可欠です。 |
| パスワード | PathFactory アカウントに関連付けられたパスワード。 権限のないアクセスを防ぐために、これが安全に保たれていることを確認します。 |
| ドメイン | PathFactory アカウントに関連付けられたドメイン。 これは通常、PathFactory URL 内の一意の ID を参照します。 |
| アクセストークン | システムと PathFactory 間の安全な通信を確保するために、API 認証に使用される一意のトークン。 |
| API エンドポイント | データにアクセスするための特定の API エンドポイント：訪問者、セッションおよびページビュー。 各エンドポイントは、取得可能な様々なデータセットに対応します。 **注意：** これらは、次の方法で事前定義されています [!DNL PathFactory] およびは、アクセスしようとしているデータに固有です。 <ul><li>**訪問者エンドポイント**: `/api/public/v3/data_lake_apis/visitors.json`</li><li>**セッションエンドポイント**: `/api/public/v3/data_lake_apis/sessions.json`</li><li>**ページビューエンドポイント**: `/api/public/v3/data_lake_apis/page_views.json`</li></ul> |

資格情報の保護および使用方法に関するガイダンスと、アクセストークンの取得および更新に関するガイダンスについては、を参照してください [PathFactory サポート・センター](https://support.pathfactory.com/categories/adobe/). このリソースでは、資格情報の管理と、効果的で安全な API 統合の確保に関する包括的なガイドを提供します。


## [!DNL PathFactory] アカウントを接続

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して、[!UICONTROL ソース]ワークスペースにアクセスします。この [!UICONTROL カタログ] Experience Platformでサポートされている様々なソースを表示します。

カテゴリのリストから適切なカテゴリを選択できます。 検索バーを使用して、特定のソースをフィルタリングすることもできます。

の下 [!UICONTROL マーケティングの自動化] カテゴリ、選択 **[!UICONTROL PathFactory]** を選択してから、 **[!UICONTROL の設定]**.

![PathFactory ソースが選択されているソースカタログ](../../../../images/tutorials/create/pathfactory/catalog.png)

この **[!UICONTROL PathFactory への接続]** ページが表示されます。 このページでは、新しいアカウントを作成するか、既存のアカウントを使用できます。

### 新規アカウント

新しいアカウントを作成するには、を選択します **[!UICONTROL 新しいアカウント]** アカウントの名前、説明（オプション）、に対応する認証資格情報を指定します [!DNL PathFactory] アカウント。

終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![PathFactory の新しいアカウントを認証できる新しいアカウントインターフェイス。](../../../../images/tutorials/create/pathfactory/new.png)

### 既存のアカウント

既存のアカウントがある場合は、 **[!UICONTROL 既存のアカウント]** 次に、表示されるリストから使用するアカウントを選択します。

![既存の PathFactory アカウントのリストから選択できる既存のアカウント インターフェイス。](../../../../images/tutorials/create/pathfactory/existing.png)

## 次の手順

このチュートリアルでは、との間の接続を確立しました [!DNL PathFactory] アカウントとExperience Platform。 次のチュートリアルに進むことができます。 [マーケティング自動化データをExperience Platformに取り込むデータフローの作成](../../dataflow/marketing-automation.md).
