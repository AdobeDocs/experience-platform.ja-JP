---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: クエリ結果からデータセットを生成する
topic: queries
translation-type: tm+mt
source-git-commit: 3b710e7a20975880376f7e434ea4d79c01fa0ce5
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---


# クエリ結果からデータセットを生成する

の真の力 [!DNL Query Service] は、クエリを使用してデータセットを生成し、より多くのクエリへの入力として、または [!DNL Data Lake] 、 [!DNL Data Science Workspace]、などの他のサービスで使用する場合に明らかになり [!DNL Real-time Customer Profile][!DNL Analysis Workspace]ます。

[!DNL Query Service] UIからデータセットを作成できます。 次の手順に従います。

1. 接続されたクライアントを使用してクエリを書き込み、出力を検証します。
2. UIにログインし、 [!DNL Platform] クエリに移動します。
3. リスト内のクエリを見つけ、行の上にカーソルを置きます。
4. 「データセット **[!UICONTROL を作成]**」をクリックします。 ![画像](../images/queries/create-datasets/click-create-dataset.png)
5. LDAP IDの前に付けたデータセット名を入力します（一意である必要もSQLセーフでない場合もあります）。 システムは、ここで与えられた名前に基づいて「テーブル名」を生成します)。
6. データセットの説明を入力し、「 **[!UICONTROL クエリを実行]**」をクリックします。![画像](../images/queries/create-datasets/run-query.png)
7. クエリの完了を確認し、データセットリストページに移動して、先ほど作成したデータセットを確認します。

作成したデータセットは、内の他のデータセットと同様にアクセスでき、様々な使用例 [!DNL Data Lake] に使用できます。

>[!NOTE]
>
>実稼動環境での導入では、データセットの作成後に [!DNL Data Governance] ラベルを適用する必要があります。

## 事前定義された [!DNL Experience Data Model] スキーマを使用したデータセットの生成

事前定義 [!DNL Experience Data Model] (XDM)スキーマを使用してデータセットを生成するには、SQL構文を使用する必要があります。 使用する構文の詳細については、『 [SQL構文』ガイドを参照してください](../sql/syntax.md#create-table-as-select)。

## 出力データセット

この機能を使用して作成されたデータセットは、SQLステートメントで定義された出力データの構造と一致するアドホックスキーマで生成されます。 一部のダウンストリームサービスでは、特定の [!DNL Experience Data Model] (XDM)スキーマを持つデータセットが必要です。 クエリを書き込む前に、ダウンストリームサービスのデータフォーマット要件を確認します。