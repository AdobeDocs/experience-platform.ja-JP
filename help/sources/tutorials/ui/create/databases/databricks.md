---
title: Ui を使用した Databricks のExperience Platformへの接続
description: ユーザーインターフェイスを使用して Databricks をExperience Platformに接続する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
badgeBeta: label="ベータ版" type="Informative"
exl-id: 877e22c0-cb77-45bb-88c9-54fdde2d6905
source-git-commit: 96e395e3b3d977d7eb04c400f6fd290977bf1101
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 8%

---

# UI で [!DNL Databricks] をExperience Platformに接続する

>[!AVAILABILITY]
>
>* Real-Time CDP Ultimateを購入したユーザーは、ソースカタログで [!DNL Databricks] ソースを利用できます。
>
>* [!DNL Databricks] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、ソースの概要の [&#x200B; 利用条件 &#x200B;](../../../../home.md#terms-and-conditions) を参照してください。

このガイドでは、UI のソースワークスペースを使用して [!DNL Databricks] アカウントをAdobe Experience Platformに接続する方法について説明します。

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [&#x200B; ソース &#x200B;](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### 必要な資格情報の収集

[!DNL Databricks] をExperience Platformに接続するには、次の資格情報の値を指定します。

| 資格情報 | 説明 |
| --- | --- |
| ドメイン | [!DNL Databricks] ワークスペースの URL。 例：`https://adb-1234567890123456.7.azuredatabricks.net`。 |
| クラスター ID | [!DNL Databricks] 内のクラスターの ID。 このクラスターは、既に既存のクラスターであり、インタラクティブなクラスターである必要があります。 |
| アクセストークン | [!DNL Databricks] アカウントを認証するアクセストークン。 [!DNL Databricks] ワークスペースを使用してアクセストークンを生成できます。 |
| データベース | delta lake 内のデータベースの名前。 |

詳しくは、[[!DNL Databricks] 概要](../../../../connectors/databases/databricks.md)を参照してください。

## ソースカタログのナビゲート

Experience Platformの UI で、左側のナビゲーションから **[!UICONTROL Sources]** を選択し、*[!UICONTROL Sources]* ワークスペースにアクセスします。 カテゴリを選択するか、検索バーを使用してソースを検索します。

[!DNL Databricks] に接続するには、[*[!UICONTROL データベース]*] カテゴリに移動し、&lbrack;**[!UICONTROL Azure Databricks]** ソース カードを選択してから [**[!UICONTROL 設定]**] を選択します。

>[!TIP]
>
>ソースカタログ内のソースは、特定のソースがまだ認証済みのアカウントを持っていない場合に「**[!UICONTROL 設定]**」オプションを表示します。 認証済みアカウントを作成すると、このオプションは **[!UICONTROL データを追加]** に変わります。

![Azure Databricks ソースカードが選択されているソースカタログ &#x200B;](../../../../images/tutorials/create/databricks/catalog.png)

### 既存のアカウントを使用

既存のアカウントを使用するには、「**[!UICONTROL 既存のアカウント]**」を選択して、使用する [!DNL Azure Databricks] アカウントを選択します。

![&#x200B; ソースワークフローの既存のアカウントインターフェイスで「既存のアカウント」が選択されている様子。](../../../../images/tutorials/create/databricks/existing.png)

### 新しいアカウントを作成

新しいアカウントを作成するには、「**[!UICONTROL 新しいアカウント]**」を選択し、名前を入力して、オプションでアカウントの説明を追加します。 次に、次の認証資格情報の値を指定します。

* ドメイン
* クラスター ID
* アクセストークン
* データベース

![&#x200B; アカウント名とオプションの説明が表示された、ソースワークフローの新しいアカウントインターフェイス。](../../../../images/tutorials/create/databricks/new.png)

さらに、[!UICONTROL &#x200B; ステージング SAS URI] 資格情報をコピーして、[!DNL Azure Databricks] 環境に貼り付ける必要があります。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、接続が確立されるまでしばらく待ちます。

![SAS URI ステージング資格情報。](../../../../images/tutorials/create/databricks/sas-uri.png)

## データのデータフロー [!DNL Azure Databricks] 作成

[!DNL Azure Databricks] アカウントに正常に接続したので、次は [&#x200B; データフローを作成し、データベースからExperience Platformにデータを取り込む &#x200B;](../../dataflow/databases.md) ことができます。
