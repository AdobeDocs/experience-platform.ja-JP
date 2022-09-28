---
keywords: Experience Platform;ホーム;人気のトピック;ソース;コネクタ;ソースコネクタ;Campaign;Campaign Managed Services
title: Platform UI を使用したAdobe Campaign Managed Cloud Servicesソース接続の作成
description: Platform UI を使用してAdobe Experience PlatformをAdobe Campaign Managed Cloud Servicesに接続する方法を説明します。
exl-id: 067ed558-b239-4845-8c85-3bf9b1d4caed
source-git-commit: b9f032c903da2bdb9f37179b1693119bf7b0029d
workflow-type: tm+mt
source-wordcount: '1149'
ht-degree: 39%

---

# Platform UI を使用したAdobe Campaign Managed Cloud Servicesソース接続の作成

このチュートリアルでは、Adobe Campaign Managed Cloud ServicesデータをAdobe Experience Platformに取り込むためのソース接続を作成する手順を説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Platform を使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを整理する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [サンドボックス](../../../../../sandboxes/home.md)：Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## Adobe Campaign Managed Cloud Servicesを Platform に接続

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。また、検索バーを使用して、表示されるソースを絞り込むこともできます。

以下 **[!UICONTROL Adobe]** カテゴリ、選択 **[!UICONTROL Adobe Campaign Managed Cloud Services]** 次に、 **[!UICONTROL データを追加]**.

![Adobe Campaign Managed Cloud Servicesカードを表示するソースカタログ。](../../../../images/tutorials/create/campaign/catalog.png)

### データの選択 {#select-data}

>[!CONTEXTUALHELP]
>id="platform_sources_campaign_instance"
>title="Adobe Campaign環境インスタンス"
>abstract="使用する Adobe Campaign 環境の名前。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_sources_campaign_mapping"
>title="ターゲットマッピング"
>abstract="ターゲットマッピングは、Campaign によってメッセージの配信に使用される技術的なオブジェクトで、配信の送信に必要なすべての技術的な設定（アドレス、電話番号、オプトインインジケーター、追加の識別子など）が含まれます。"
>text="Learn more in documentation"

>[!CONTEXTUALHELP]
>id="platform_sources_campaign_schema"
>title="スキーマ名"
>abstract="Adobe Campaign データベースで定義されたエンティティの名前。"
>text="Learn more in documentation"

この [!UICONTROL データを選択] 手順が表示され、 [!UICONTROL Adobe Campaignインスタンス], [!UICONTROL ターゲットマッピング]、および [!UICONTROL スキーマ名].

| プロパティ | 説明 |
| --- | --- |
| Adobe Campaignインスタンス | 使用しているAdobe Campaign環境インスタンスの名前。 |
| ターゲットマッピング | メッセージを配信するために Campaign で使用されるテクニカルオブジェクト。配信の送信に必要なすべての技術設定が含まれます。 |
| スキーマ名 | Platform に取り込むスキーマエンティティの名前。 オプションには、配信ログとトラッキングログがあります。 |

![Adobe Campaignインスタンス、ターゲットマッピングおよびスキーマ名を設定できるインターフェイス。](../../../../images/tutorials/create/campaign/select-data.png)

Campaign インスタンスの値、ターゲットマッピング、スキーマ名を指定すると、画面が更新され、スキーマのプレビューとサンプルデータセットが表示されます。 終了したら、「**[!UICONTROL 次へ]**」を選択します。

![スキーマ階層のプレビューとデータセットの例](../../../../images/tutorials/create/campaign/preview.png)

### 既存のデータセットを使用する

この [!UICONTROL データフローの詳細] ページでは、既存のデータセットを使用するか、データフローの新しいデータセットを設定するかを選択できます。

既存のデータセットを使用するには、「 」を選択します。 **[!UICONTROL 既存のデータセット]**. 既存のデータセットは、「[!UICONTROL 詳細検索]」オプションを使用するか、ドロップダウンメニュー内の既存のデータセットのリストをスクロールして取得することができます。

データセットを選択し、データフローの名前と説明（オプション）を入力します。

![既存のデータセットオプションを表示するインターフェイス。](../../../../images/tutorials/create/campaign/existing-dataset.png)

### 新しいデータセットの使用

新しいデータセットを使用するには、 **[!UICONTROL 新しいデータセット]** 次に、出力データセット名とオプションの説明を入力します。 次に、「[!UICONTROL 詳細検索]」オプションを使用するか、ドロップダウンメニューで既存のスキーマのリストをスクロールして、マッピングするスキーマを選択します。終了したら、「**[!UICONTROL 次へ]**」を選択します。

![新しい「データセット」オプションを表示するインターフェイス。](../../../../images/tutorials/create/campaign/new-dataset.png)

### アラートの有効化

アラートを有効にすると、データフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を購読および受信します。 アラートについて詳しくは、[UI を使用したソースアラートの購読](../../alerts.md)についてのガイドを参照してください。

データフローへの詳細の入力を終えたら「**[!UICONTROL 次へ]** 」を選択します。

![データフローに対して有効にできる様々なアラートタイプの選択。](../../../../images/tutorials/create/campaign/alerts.png)

### XDM スキーマへのデータフィールドのマッピング

[!UICONTROL マッピング]手順が表示され、ソーススキーマのソースフィールドを、ターゲットスキーマの適切なターゲット XDM フィールドにマッピングするためのインターフェイスが提供されます。

Platform は、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対するインテリジェントなレコメンデーションを提供します。 マッピングルールは、使用例に合わせて手動で調整できます。 必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドを使用した包括的な手順については、 [データ準備 UI ガイド](../../../../../data-prep/ui/mapping.md).

>[!IMPORTANT]
>
>ソースフィールドをターゲット XDM フィールドにマッピングする場合、指定されたプライマリ ID フィールドを適切なターゲット XDM フィールドにマッピングする必要があります。

ソースデータが正常にマッピングされたら、「 」を選択します。 **[!UICONTROL 次へ]**.

![4 つのソースデータフィールドが対応する XDM スキーマフィールドにマッピングされたマッピングツリー。](../../../../images/tutorials/create/campaign/mapping.png)

### データフローのレビュー

**[!UICONTROL レビュー]**&#x200B;手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**:ソースのタイプ、選択したソースファイルの関連パス、およびそのソースファイル内の列数を表示します。
* **[!UICONTROL データセットの割り当てとフィールドのマッピング]**：ソースデータがどのデータセットに取り込まれるかを、そのデータセットが準拠するスキーマを含めて表示します。

データフローをレビューしたら、「 」を選択します。 **[!UICONTROL 完了]** とは、データフローが作成されるまでしばらく時間をかけます。

![接続とデータセットの情報を示すレビューページ。](../../../../images/tutorials/create/campaign/review.png)

### データセットアクティビティの監視

データフローを作成したら、データフローを通じて取り込まれるデータを監視して、取り込まれた率、成功および失敗したバッチに関する情報を確認できます。

データセットアクティビティの表示を開始するには、「 **[!UICONTROL データフロー]** ソースカタログ内。

![データフローヘッダータブが選択されたソースカタログページ](../../../../images/tutorials/create/campaign/dataflows.png)

次に、表示されるデータフローのリストからターゲットデータセットを選択します。

![Adobe Campaign配信ログのターゲットデータセットが選択された既存のデータフローのリスト。](../../../../images/tutorials/create/campaign/target-dataset.png)

「データセットアクティビティ」ページが表示されます。 ここから、取得率、成功したバッチ、失敗したバッチなど、データフローのパフォーマンスに関する情報を確認できます。

また、このページには、データフローのメタデータ記述を更新し、部分取得とエラー診断を有効にし、新しいデータをデータセットに追加するためのインターフェイスも用意されています。

![選択したデータセットの取り込み率を表すグラフを含むインターフェイス。](../../../../images/tutorials/create/campaign/dataset-activity.png)

## 次の手順

このチュートリアルでは、Campaign v8 配信ログとトラッキングログデータを Platform に取り込むためのデータフローを正常に作成しました。 受信データは、[!DNL Real-time Customer Profile] および [!DNL Data Science Workspace] のようなダウンストリームの Platform サービスで使用できるようになりました。詳しくは、次のドキュメントを参照してください。

* [[!DNL Real-time Customer Profile] 概要](../../../../../profile/home.md)
* [[!DNL Data Science Workspace] 概要](../../../../../data-science-workspace/home.md)
