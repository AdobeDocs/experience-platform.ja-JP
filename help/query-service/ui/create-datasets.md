---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；クエリサービス；データセットの生成；データセットの生成；データセットの作成；
solution: Experience Platform
title: クエリサービスの結果からデータセットを生成
topic-legacy: queries
type: Tutorial
description: Adobe Experience Platformクエリサービスを使用すると、UI からデータセットを作成できます。 データセットを作成した後は、データレイク内の他のデータセットと同様にアクセスし、様々な使用例に使用できます。
exl-id: 6f6c049d-f19f-4161-aeb4-3a01eca7dc75
source-git-commit: d2f19cc97082f75e66cf38e54b5bdb89482930ed
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 40%

---

# クエリサービスの結果からデータセットを生成する

[!DNL Query Service] の真の力は、クエリを使用して [!DNL Data Lake] 内のデータセットを生成し、他のクエリや [!DNL Data Science Workspace]、[!DNL Real-time Customer Profile]、[!DNL Analysis Workspace] などの他のサービスで使用する場合に明らかになります。

[!DNL Query Service] では、UI からデータセットを作成できます。次の手順に従います。

1. 接続されたクライアントを使用してクエリを記述し、出力を検証します。
2. [!DNL Platform] UI にログインし、「クエリ」に移動します。
3. リストでクエリを探し、その行にカーソルを合わせます。
4. 「**[!UICONTROL データセットを作成]**」を選択します。 ![画像](../images/ui/create-datasets/output-dataset.png)
5. LDAP ID の後にデータセット名を入力します（一意である必要も、SQL セーフである必要もありません。システムは、ここで指定した名前に基づいて「テーブル名」を生成します）。
6. データセットの説明を入力し、「**[!UICONTROL クエリを実行]**」を選択します。![画像](../images/ui/create-datasets/run-query.png)
7. クエリの完了を確認し、データセットのリストページに移動して、作成したデータセットを確認します。

データセットを作成した後は、[!DNL Data Lake] 内の他のデータセットと同様にアクセスし、様々な使用例に使用できます。

>[!NOTE]
>
>実際に実装する場合は、データセットの作成後に [!DNL Data Governance] ラベルを適用する必要があります。

## 事前定義済みの [!DNL Experience Data Model] スキーマを使用してデータセットを生成する

事前に定義された [!DNL Experience Data Model] (XDM) スキーマを使用してデータセットを生成するには、SQL 構文を使用する必要があります。 使用する構文について詳しくは、[SQL 構文のガイド](../sql/syntax.md#create-table-as-select)を参照してください。

## データセットを出力する

この機能を使用して作成されるデータセットは、SQL 文で定義されているように、出力データの構造と一致するアドホックスキーマを使用して生成されます。一部のダウンストリームサービスでは、特定の [!DNL Experience Data Model](XDM) スキーマを持つデータセットが必要です。 ダウンストリームサービスのデータ形式設定要件を確認してから、クエリを記述してください。
