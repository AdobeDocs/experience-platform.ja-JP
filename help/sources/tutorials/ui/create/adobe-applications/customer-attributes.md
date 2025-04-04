---
keywords: Experience Platform；ホーム；人気のトピック；顧客属性
solution: Experience Platform
title: UI での顧客属性Source接続の作成
type: Tutorial
description: UI でソース接続を作成して、顧客属性プロファイルデータをAdobe Experience Platformに取り込む方法を説明します。
exl-id: 66bdab8f-c00e-4ebe-8b8e-f9e12cf86bbe
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '620'
ht-degree: 33%

---

# UI での Customer Attributes ソース接続の作成

このチュートリアルでは、顧客属性プロファイルデータをAdobe Experience Platformに取り込むために、UI でソース接続を作成する手順を説明します。 顧客属性について詳しくは、[ 顧客属性の概要 ](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/attributes.html?lang=ja) を参照してください。

>[!IMPORTANT]
>
>顧客属性ソースは現在、データフローの有効化または無効化をサポートしていません。

## ソース接続の作成

>[!NOTE]
>
>顧客属性プロファイルデータのソース接続が既に確立されている場合は、ソースに接続するオプションは無効になります。

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、[!UICONTROL Sources] ワークスペースにアクセスします。 [!UICONTROL  カタログ ] 画面には、接続を作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

[!UICONTROL Adobe アプリケーション ] カテゴリで、「**[!UICONTROL 顧客属性]**」を選択し、「**[!UICONTROL データを追加]**」を選択します。

![カタログ](../../../../images/tutorials/create/customer-attributes/catalog.png)

### 顧客属性データソースを選択

[!UICONTROL  データを追加 ] 画面には、顧客属性に使用できるすべてのデータソースが一覧表示されます。 顧客属性ソース接続ごとに 1 つのデータセットのみを選択できます。

>[!NOTE]
>
>フィールドグループ、スキーマおよびデータセットは、フロープロビジョニングの一環として、すぐに使用できるように作成されています。 これらは現状のままなので、必要に応じて手動で削除する必要があります。

顧客属性ソースでは、スキーマ進化はサポートされていません。 顧客属性データソースのスキーマ入力を変更すると、Experience Platformと互換性がなくなります。 回避策として、既存の顧客属性データフローを、関連するデータセット、スキーマ、フィールドグループと共に削除した後、更新されたスキーマとデータソースを使用して新しい顧客属性データフローを作成できます。

>[!IMPORTANT]
>
>顧客属性データフローは削除できますが、対応するデータセットはデータフローの削除後も残ります。 データセットを手動で削除する手順については、[ データセットの削除 ](../../../../../catalog/datasets/user-guide.md) に関するガイドを参照してください。

新しい接続を作成するには、リストからデータソースを選択して、「**[!UICONTROL 次へ]**」を選択します。

![add-data](../../../../images/tutorials/create/customer-attributes/add-data.png)

### データフローの詳細を入力

[!UICONTROL  データフローの詳細 ] 手順が表示され、データフローの名前と簡単な説明を入力できます。 このプロセスでは、[!UICONTROL  エラー診断 ]、[!UICONTROL  部分取り込み ]、および [!UICONTROL  アラート ] の設定も指定できます。

[!UICONTROL エラー診断]は、データフローで発生するエラーレコードに対して、詳細なエラーメッセージ生成を有効にします。[!UICONTROL 部分取り込み]では、手動で定義した特定のしきい値に到達するまで、エラーを含むデータを取り込むことができます。詳しくは、[バッチ取り込みの概要](../../../../../ingestion/batch-ingestion/partial.md)を参照してください。

アラートを有効にすると、データフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用したソースアラートの購読](../../alerts.md)についてのガイドを参照してください。

データフローへの詳細の入力を終えたら「**[!UICONTROL 次へ]** 」を選択します。

![dataflow-detail](../../../../images/tutorials/create/customer-attributes/dataflow-detail.png)

### データフローのレビュー

[!UICONTROL レビュー]手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**：ソースのタイプ、選択したソースファイルの関連パスおよびそのソースファイル内の列の数を表示します。
* **[!UICONTROL データセットの割り当てとフィールドのマッピング]**：ソースデータがどのデータセットに取り込まれるかを、そのデータセットが準拠するスキーマを含めて表示します。

![レビュー](../../../../images/tutorials/create/customer-attributes/review.png)

## 次の手順

接続を作成すると、受信データを格納するターゲットスキーマとデータセットが自動的に作成されます。 初回の取り込みが完了したら、[!DNL Real-Time Customer Profile] や [!DNL Segmentation Service] などのダウンストリームのExperience Platform サービスで顧客属性プロファイルデータを使用できるようになります。 詳しくは、次のドキュメントを参照してください。

* [[!DNL Real-Time Customer Profile] 概要](../../../../../profile/home.md)
* [[!DNL Segmentation Service] 概要](../../../../../segmentation/home.md)
