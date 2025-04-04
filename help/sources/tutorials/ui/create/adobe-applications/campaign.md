---
keywords: Experience Platform;ホーム;人気のトピック;ソース;コネクタ;ソースコネクタ;Campaign;Campaign Managed Services
title: Experience Platform UI を使用したAdobe Campaign Managed Cloud Services ソース接続の作成
description: Experience Platform UI を使用してAdobe Experience PlatformをAdobe Campaign Managed Cloud Servicesに接続する方法について説明します。
exl-id: 067ed558-b239-4845-8c85-3bf9b1d4caed
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1246'
ht-degree: 37%

---

# Experience Platform UI を使用したAdobe Campaign Managed Cloud Services ソース接続の作成

このチュートリアルでは、ソース接続を作成してAdobe Campaign Managed Cloud Services データをAdobe Experience Platformに取り込む手順について説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## Adobe Campaign Managed Cloud ServicesのExperience Platformへの接続

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。また、検索バーを使用して、表示されるソースを絞り込むこともできます。

**[!UICONTROL Adobe アプリケーション]** カテゴリで、「**[!UICONTROL Adobe Campaign Managed Cloud Services」を選択し]** 「**[!UICONTROL データを追加]**」を選択します。

![Adobe Campaign Managed Cloud Servicesカードを表示しているソースカタログ ](../../../../images/tutorials/create/campaign/catalog.png)

### データの選択 {#select-data}

>[!CONTEXTUALHELP]
>id="platform_sources_campaign_instance"
>title="Adobe Campaign 環境インスタンス"
>abstract="使用する Adobe Campaign 環境の名前です。"
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

[!UICONTROL  データの選択 ] 手順が表示され、[!UICONTROL Adobe Campaign インスタンス ]、[!UICONTROL  ターゲットマッピング ]、および [!UICONTROL  スキーマ名 ] を設定するためのインターフェイスが利用できます。

| プロパティ | 説明 |
| --- | --- |
| Adobe Campaign インスタンス | 使用しているAdobe Campaign環境インスタンスの名前。 |
| ターゲットマッピング | メッセージの配信に使用される技術的なオブジェクトで、配信の送信に必要なすべての技術的な設定が含まれます。 |
| スキーマ名 | Experience Platformに取り込むスキーマエンティティの名前。 オプションには、「配信ログ」と「トラッキングログ」があります。 |

![Adobe Campaign インスタンス、ターゲットマッピング、およびスキーマ名を設定できるインターフェイス ](../../../../images/tutorials/create/campaign/select-data.png)。

Campaign インスタンス、ターゲットマッピング、スキーマ名の値を指定したら、画面が更新されて、スキーマのプレビューとサンプルデータセットが表示されます。 終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ スキーマ階層のプレビューとデータセットのサンプル ](../../../../images/tutorials/create/campaign/preview.png)

### 既存のデータセットを使用する

この [!UICONTROL  データフローの詳細 ] ページでは、既存のデータセットを使用するか、データフロー用の新しいデータセットを設定するかを選択できます。

既存のデータセットを使用するには、「**[!UICONTROL 既存のデータセット]**」を選択します。 既存のデータセットは、「[!UICONTROL 詳細検索]」オプションを使用するか、ドロップダウンメニュー内の既存のデータセットのリストをスクロールして取得することができます。

データセットを選択し、データフローの名前と説明（オプション）を入力します。

![ 既存のデータセットオプションを表示するインターフェイス ](../../../../images/tutorials/create/campaign/existing-dataset.png)

### 新しいデータセットの使用

新しいデータセットを使用するには、「**[!UICONTROL 新しいデータセット]**」を選択して、出力データセット名とオプションの説明を入力します。 次に、「[!UICONTROL 詳細検索]」オプションを使用するか、ドロップダウンメニューで既存のスキーマのリストをスクロールして、マッピングするスキーマを選択します。終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ 新しいデータセットオプションを表示するインターフェイス。](../../../../images/tutorials/create/campaign/new-dataset.png)

### アラートの有効化

アラートを有効にすると、データフローのステータスに関する通知を受け取ることができます。リストからアラートを選択すると、データフローのステータスに関する通知を登録して受け取ることができます。 アラートについて詳しくは、[UI を使用したソースアラートの購読](../../alerts.md)についてのガイドを参照してください。

データフローへの詳細の入力を終えたら「**[!UICONTROL 次へ]** 」を選択します。

![ データフローに対して有効にできる様々なアラートタイプの選択。](../../../../images/tutorials/create/campaign/alerts.png)

### XDM スキーマへのデータフィールドのマッピング

[!UICONTROL マッピング]手順が表示され、ソーススキーマのソースフィールドを、ターゲットスキーマの適切なターゲット XDM フィールドにマッピングするためのインターフェイスが提供されます。

Experience Platformは、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対してインテリジェントなレコメンデーションを提供します。 マッピングルールは、ユースケースに合わせて手動で調整できます。 必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドの使用に関する包括的な手順については、[ データ準備 UI ガイド ](../../../../../data-prep/ui/mapping.md) を参照してください。

>[!IMPORTANT]
>
>ソースフィールドをターゲット XDM フィールドにマッピングする場合は、指定したプライマリ ID フィールドを適切なターゲット XDM フィールドにマッピングする必要があります。
>
>オーディエンスごとに、最大 20 個のフィールドを追加してAdobe Campaignにマッピングできます。 この制限は、Campaign エクスプローラーの管理/プラットフォーム/オプション フォルダーで `NmsCdp_Aep_Sources_Max_Columns` オプションの値を更新することで変更できます。

ソースデータが正常にマッピングされたら、「**[!UICONTROL 次へ]**」を選択します。

![ 対応する XDM スキーマフィールドにマッピングされた 4 つのソースデータフィールドを含むマッピングツリー。](../../../../images/tutorials/create/campaign/mapping.png)

### データフローのレビュー

**[!UICONTROL レビュー]**&#x200B;手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**：ソースのタイプ、選択したソースファイルの関連パスおよびそのソースファイル内の列の数を表示します。
* **[!UICONTROL データセットの割り当てとフィールドのマッピング]**：ソースデータがどのデータセットに取り込まれるかを、そのデータセットが準拠するスキーマを含めて表示します。

データフローをレビューしたら、「**[!UICONTROL 終了]**」を選択し、データフローが作成されるまでしばらく待ちます。

![ 接続とデータセットの情報を表示するレビューページ ](../../../../images/tutorials/create/campaign/review.png)

### データセットアクティビティの監視

データフローを作成したら、そのデータフローを通じて取り込まれるデータを監視し、取り込まれた割合や成功したバッチおよび失敗したバッチに関する情報を確認できます。

データセットアクティビティの表示を開始するには、ソースカタログで **[!UICONTROL データフロー]** を選択します。

![ データフローヘッダータブが選択されたソースカタログページ ](../../../../images/tutorials/create/campaign/dataflows.png)

次に、表示されるデータフローのリストからターゲットデータセットを選択します。

![Adobe Campaign配信ログターゲットデータセットが選択された既存のデータフローのリスト。](../../../../images/tutorials/create/campaign/target-dataset.png)

データセットアクティビティ ページが表示されます。 ここから、取り込み率、成功したバッチ、失敗したバッチなど、データフローのパフォーマンスに関する情報を確認できます。

また、このページには、データフローのメタデータの説明を更新したり、部分取り込みやエラー診断を有効にしたり、データセットに新しいデータを追加したりするためのインターフェイスも用意されています。

![ 選択したデータセットの取り込み率を表すグラフを備えたインターフェイス。](../../../../images/tutorials/create/campaign/dataset-activity.png)


>[!IMPORTANT]
>
>古いイベントログをAdobe Campaign Managed Cloud Services ソースでバックフィルすることはできません。 バックフィルが必要な場合は、カスタムワークフローまたはカスタム実装を使用して、Amazon S3 または Azure Blob にデータを書き出すか、Amazon S3 または Azure Blob からAdobe Experience Platform データセットにデータを書き出します。


## 次の手順

このチュートリアルでは、Campaign v8 の配信ログとトラッキングログのデータをExperience Platformに取り込むデータフローを正常に作成しました。 これで、[!DNL Real-Time Customer Profile] や [!DNL Data Science Workspace] などのダウンストリームのExperience Platform サービスで受信データを使用できるようになりました。 詳しくは、次のドキュメントを参照してください。

* [[!DNL Real-Time Customer Profile] 概要](../../../../../profile/home.md)
* [[!DNL Data Science Workspace] 概要](../../../../../data-science-workspace/home.md)
