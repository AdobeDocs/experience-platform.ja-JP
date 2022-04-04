---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；データセットの生成；データセットの生成；データセットの作成；
solution: Experience Platform
title: クエリサービスの結果からデータセットを生成
topic-legacy: queries
type: Tutorial
description: Adobe Experience Platformクエリサービスを使用すると、UI からデータセットを作成できます。 データセットを作成したら、データレイク内の他のデータセットと同様にアクセスし、様々な使用例に使用できます。
exl-id: 6f6c049d-f19f-4161-aeb4-3a01eca7dc75
source-git-commit: 0c2cfe9b0bd839bdf662622283a7563c0417c9a9
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 12%

---

# の結果からデータセットを生成 [!DNL Query Service]

[!DNL Query Service] では、クエリを使用してデータセットを [!DNL Data Lake]. その後、これらのデータセットを他のクエリや他のサービス ( [!DNL Data Science Workspace]、リアルタイム顧客プロファイル、または [!DNL Analysis Workspace].

## Adobe Experience Platformユーザーインターフェイスからデータセットを生成する

Adobe Experience Platformユーザーインターフェイス (UI) からデータセットを作成するには、次の手順に従います。

1. 接続されたクライアントを使用してクエリを作成し、出力を検証します。 を使用してクエリを記述する方法を学ぶには [!DNL Query Editor]、 [!DNL Query Editor] UI ガイド [クエリの記述時](./user-guide.md#writing-queries).

2. Platform UI で、に移動します。 **[!UICONTROL クエリ]** 続いて **[!UICONTROL 参照]** 」タブをクリックし、作成したクエリを選択します。 Platform UI 内で組織用に作成および保存されたクエリを表示する方法の詳細については、 [[!DNL Query Service] 概要](./overview.md#browse).

3. クエリの詳細パネルで、「 」を選択します。 **[!UICONTROL データセットを出力]**.

   ![出力データセットを選択](../images/ui/create-datasets/output-dataset.png)

4. 表示されるダイアログで、LDAP ID の前に付いたデータセット名を入力します。 データセット名は、一意である必要も、SQL セーフである必要もありません。 データセットのテーブル名は、ここで作成したデータセット名に基づいて生成されます。

5. 次に、データセットの説明を [!UICONTROL 説明] フィールドと選択 **[!UICONTROL クエリを実行]**.

   ![クエリを実行](../images/ui/create-datasets/run-query.png)

6. クエリの実行が完了したら、に移動します。 **[!UICONTROL データセット]** をクリックして、作成したデータセットを表示します。 Platform UI 内でデータセットを操作する際に一般的なアクションを実行する方法について詳しくは、 [データセット UI ガイド](../../catalog/datasets/user-guide.md).

データセットを作成した後は、 [!DNL Data Lake] 様々な使用例に使用されます。

>[!NOTE]
>
>実際に実装する場合は、データセットの作成後にデータガバナンスラベルを適用する必要があります。データ使用ラベルをデータセットに適用する方法について詳しくは、 [データ使用ラベルの概要](../../data-governance/labels/overview.md).

## 事前定義済みのデータセットを生成する [!DNL Experience Data Model] スキーマ

SQL 構文を使用して、事前に定義された [!DNL Experience Data Model] (XDM) スキーマ。 でサポートされる構文の詳細 [!DNL Query Service]を読んでください。 [SQL 構文ガイド](../sql/syntax.md#create-table-as-select).

## データセットを出力する

この機能を使用して作成されるデータセットは、SQL 文で定義されているように、出力データの構造と一致するアドホックスキーマを使用して生成されます。一部のダウンストリームサービスには、特定の XDM スキーマを持つデータセットが必要です。 ダウンストリームサービスのデータ形式設定要件を確認してから、クエリを記述してください。

## 次の手順

このドキュメントを読んだ後、 [!DNL Query Service] を使用して、Platform UI からデータセットを生成します。 Platform UI 内でクエリにアクセス、書き込み、実行する方法について詳しくは、 [[!DNL Query Service] UI の概要](./overview.md).
