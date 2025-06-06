---
keywords: Experience Platform;ホーム;人気のトピック;クエリサービス;データセットの生成;データセットの作成;
solution: Experience Platform
title: クエリ結果からの出力データセットの生成
type: Tutorial
description: Adobe Experience Platform クエリサービスを使用すると、UI からデータセットを作成できます。データセットを作成したら、データレイク内の他のデータセットと同様にアクセスしたり、様々なユースケースに使用したりできます。
exl-id: 6f6c049d-f19f-4161-aeb4-3a01eca7dc75
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 63%

---

# クエリ結果からの出力データセットの生成

[!DNL Query Service] では、クエリを使用して [!DNL Data Lake] にデータセットを生成できます。その後、これらのデータセットをさらなるクエリの入力として使用したり、[!DNL Data Science Workspace]、リアルタイム顧客プロファイル、[!DNL Analysis Workspace] などの他のサービスで使用したりできます。

## Adobe Experience Platform ユーザーインターフェイスからのデータセットの生成

Adobe Experience Platform ユーザーインターフェイス（UI）からデータセットを作成するには、次の手順に従います。

1. 接続したクライアントを使用してクエリを作成し、出力を検証します。[!DNL Query Editor] を使用してクエリを記述する方法については、[!DNL Query Editor] UI ガイドで[クエリの記述](./user-guide.md#writing-queries)を参照してください。

2. Experience Platform UI で、「**[!UICONTROL クエリ]**」に移動したあと「**[!UICONTROL テンプレート]**」タブに移動し、作成したクエリを選択します。 Experience Platform UI 内で組織用に作成および保存されたクエリを表示する方法について詳しくは、[[!DNL Query Service]  概要 ](./overview.md#browse) を参照してください。

3. クエリの詳細パネルで、「**[!UICONTROL CTAS として実行]**」を選択します。

   ![ 「選択 [!UICONTROL CTAS として実行 &#x200B;]」がハイライト表示されたクエリワークスペース [!UICONTROL &#x200B; 「テンプレート &#x200B;]」タブ ](../images/ui/create-datasets/run-as-ctas.png)

4. 表示されるダイアログで、先頭に LDAP ID が追加されたデータセット名を入力します。 データセット名は、一意である必要も、SQL セーフである必要もありません。 なお、データセットのテーブル名は、ここで作成したデータセット名に基づいて生成されます。

5. 次に、データセットの説明を「[!UICONTROL &#x200B; 説明 &#x200B;]」フィールドに入力し、「**[!UICONTROL CTAS として実行]**」を選択します。

   ![ データセットの詳細と [!UICONTROL CTAS として実行 &#x200B;] がハイライト表示されたデータセットを出力ダイアログ ](../images/ui/create-datasets/run-query.png)

6. クエリの実行が完了したら、**[!UICONTROL データセット]**&#x200B;に移動して、作成したデータセットを表示します。 Experience Platform UI 内でデータセットを操作する際に一般的なアクションを実行する方法について詳しくは、[ データセット UI ガイド ](../../catalog/datasets/user-guide.md) を参照してください。

データセットを作成したら、[!DNL Data Lake] 内の他のデータセットと同様にアクセスしたり、様々なユースケースに使用したりできます。

>[!NOTE]
>
>実際に実装する場合は、データセットの作成後にデータガバナンスラベルを適用する必要があります。データ使用ラベルをデータセットに適用する方法について詳しくは、[データ使用ラベルの概要](../../data-governance/labels/overview.md)を参照してください。

## 事前に定義された [!DNL Experience Data Model] スキーマを使用したデータセットの生成

SQL 構文を使用して、事前定義済みの [!DNL Experience Data Model]（XDM）スキーマでデータセットを生成します。 [!DNL Query Service] でサポートされている構文について詳しくは、[SQL 構文ガイド](../sql/syntax.md#create-table-as-select)を参照してください。

## データセットを出力する

この機能を使用して作成されるデータセットは、SQL 文で定義されているように、出力データの構造と一致するアドホックスキーマを使用して生成されます。一部のダウンストリームサービスには、特定の XDM スキーマを持つデータセットが必要です。ダウンストリームサービスのデータ形式設定要件を確認してから、クエリを記述してください。

## 次の手順

このドキュメントでは、[!DNL Query Service] を使用してExperience Platform UI からデータセットを生成する方法を説明しました。 Experience Platform UI 内でのクエリへのアクセス、クエリの記述、クエリの実行の方法について詳しくは、[[!DNL Query Service] UI の概要 ](./overview.md) を参照してください。
