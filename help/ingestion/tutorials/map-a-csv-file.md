---
keywords: Experience Platform;home;popular topics;map csv;map csv file;map csv file to xdm;map csv to xdm;ui guide;
solution: Experience Platform
title: XDM スキーマへの CSV ファイルのマッピング
topic: tutorial
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platformのユーザーインターフェイスを使用してCSVファイルをXDMスキーマにマップする方法について説明します。
translation-type: tm+mt
source-git-commit: 7adf18e4251f377fee586c8a0f23b89acd75afca
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 25%

---


# XDM スキーマへの CSV ファイルのマッピング

In order to ingest CSV data into [!DNL Adobe Experience Platform], the data must be mapped to an [!DNL Experience Data Model] (XDM) schema. This tutorial covers how to map a CSV file to an XDM schema using the [!DNL Platform] user interface.

また、このチュートリアルの付録では、[マッピング関数](#mapping-functions)の使用に関する詳細情報を提供します。

## はじめに

This tutorial requires a working understanding of the following components of [!DNL Platform]:

- [[!DNL Experience Data Model (XDM System)]](../../xdm/home.md):顧客体験データを [!DNL Platform] 整理する際に使用される標準化されたフレームワーク。
- [[!DNLバッチインジェスト]](../batch-ingestion/overview.md):ユーザーが指定したデータファイルからデータを [!DNL Platform] 取り込む方法。

また、このチュートリアルでは、CSV データの取り込み先のデータセットを既に作成している必要があります。UI でデータセットを作成する手順については、[データ取得のチュートリアル](./ingest-batch-data.md)を参照してください。

## 宛先の選択

[!DNL [Adobe Experience Platform]](https://platform.adobe.com) にログインし、左のナビゲーションバーから **[!UICONTROL ワークフローを選択して]** ワークフロー **** ワークスペースにアクセスします。

**[!UICONTROL ワークフロー]** 画面の「 **[!UICONTROL Data ingestion]** 」セクションで「 **[!UICONTROL CSVをXDMスキーマにマップ」を選択し、「]** Launch **** Launch」を選択します。

![](../images/tutorials/map-a-csv-file/workflows.png)

The **[!UICONTROL Map CSV to XDM schema]** workflow appears, starting on the **[!UICONTROL Destination]** step. 取り込む受信データのデータセットを選択します。 既存のデータセットを使用することも、新しいデータセットを作成することもできます。

**既存のデータセットを使用する**

CSVデータを既存のデータセットに取り込むには、「既存のデータセット **[!UICONTROL を使用]**」を選択します。 検索関数を使用して既存のデータセットを取得するか、パネル内の既存のデータセットのリストをスクロールして取得できます。

![](../images/tutorials/map-a-csv-file/use-existing-dataset.png)

CSVデータを新しいデータセットに取り込むには、「新しいデータセットを **[!UICONTROL 作成]** 」を選択し、表示されるフィールドにデータセットの名前と説明を入力します。 検索機能を使用するか、提供されるスキーマのリストをスクロールして、スキーマを選択します。 「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![](../images/tutorials/map-a-csv-file/create-new-dataset.png)

## データの追加

「**[!UICONTROL データ追加]**」手順が表示されます。CSVファイルを用意されているスペースにドラッグ&amp;ドロップするか、「ファイルを **[!UICONTROL 選択]** 」を選択してCSVファイルを手動で入力します。

![](../images/tutorials/map-a-csv-file/add-data.png)

The **[!UICONTROL Sample data]** section appears once the file is uploaded, showing the first ten rows of data. Once you have confirmed that the data has uploaded as expected, select **[!UICONTROL Next]**.

![](../images/tutorials/map-a-csv-file/sample-data.png)

## XDM スキーマフィールドへの CSV フィールドのマッピング

「**[!UICONTROL マッピング]**」手順が表示されます。CSV ファイルの列は「**[!UICONTROL ソースフィールド]**」の下にリストされ、対応する XDM スキーマフィールドが「**[!UICONTROL ターゲットフィールド]**」の下にリストされます。未選択のターゲットフィールドは赤で囲まれます。フィルターフィールドオプションを使用して、使用可能なソースフィールドのリストを絞り込むことができます。

>[!TIP]
>
>[!DNL Platform] 選択したターゲットスキーマまたはデータセットに基づいて、自動マップされたフィールドに高度な推奨機能を提供します。 使用事例に合わせて手動でマッピングルールを調整できます。

CSV列をXDMフィールドにマップするには、列の対応するターゲットフィールドの横にあるスキーマアイコンを選択します。

![](../images/tutorials/map-a-csv-file/mapping.png)

**[!UICONTROL スキーマフィールドの選択]**&#x200B;ウィンドウが表示されます。ここで、XDM スキーマの構造をナビゲートし、CSV 列のマッピング先のフィールドを探します。XDM フィールドをクリックして選択してから、「**[!UICONTROL 選択]**」をクリックします。

![](../images/tutorials/map-a-csv-file/select-schema-field.png)

マッピングされていない残りのソースフィールドの手順を完了すると、 **[!UICONTROL マッピング]** 画面が再表示され、選択したXDMフィールドが **[!UICONTROL ターゲットフィールドの下に表示されます]**。

![](../images/tutorials/map-a-csv-file/field-mapped.png)

フィールドをマッピングする際に、入力ソースフィールドに基づいて値を計算する関数を含めることもできます。詳しくは、付録の「[マッピング関数](#mapping-functions)」の節を参照してください。

### 追加計算フィールド

計算済みフィールドでは、入力スキーマーの属性に基づいて値を作成できます。 これらの値をターゲットスキーマの属性に割り当て、名前と説明を指定して参照しやすくすることができます。

先に進むには、 **[!UICONTROL 追加]** 計算フィールドボタンを選択します。

![](../images/tutorials/map-a-csv-file/add-calculate-field.png)

計算済みフィールド **[!UICONTROL を作成]** パネルが表示されます。 左側のダイアログボックスには、計算フィールドでサポートされるフィールド、関数、演算子が含まれています。 いずれかのタブを選択して、式エディタに関数、フィールドまたは演算子を追加する開始を行います。

![](../images/tutorials/map-a-csv-file/create-calculated-fields.png)

| タブ | 説明 |
| --------- | ----------- |
| フィールド | 「フィールド」タブのリストフィールドと属性は、ソーススキーマで使用できます。 |
| 関数 | 「関数」タブには、データの変換に使用できる関数がリストされます。 |
| 演算子 | 「operators」タブには、データの変換に使用できる演算子がリストされます。 |

中央の式エディターを使用して、手動でフィールド、関数および演算子を追加できます。 式の作成を開始するエディタを選択します。

![](../images/tutorials/map-a-csv-file/expression-editor.png)

「 **[!UICONTROL 保存]** 」を選択して続行します。

マッピング画面が再び開き、新しく作成したソースフィールドが表示されます。 対応するターゲットフィールドを適用し、「 **[!UICONTROL 完了]** 」を選択してマッピングを完了します。

![](../images/tutorials/map-a-csv-file/new-field.png)

## データフローの監視

CSVファイルをマッピングして作成したら、ファイルを介して取り込まれるデータを監視できます。 データフローの監視の詳細については、「ストリーミングデータフローの [監視に関するチュートリアル](../../ingestion/quality/monitor-data-flows.md)」を参照してください。

## マッピング関数の使用

関数を使用するには、「**[!UICONTROL ソースフィールド]**」の下に適切な構文で関数を入力します。

例えば、**市区町村**&#x200B;と&#x200B;**国**&#x200B;の CSV フィールドを連結し、**市区町村** XDM フィールドに割り当てるには、ソースフィールドを「`concat(city, ", ", county)`」と設定します。

![](../images/tutorials/map-a-csv-file/mapping-function.png)

XDMフィールドへの列のマッピングについて詳しくは、データ準備（マッパー）関数の [使用に関するガイドを参照してください](../../data-prep/functions.md)。

## 次の手順

By following this tutorial, you have successfully mapped a flat CSV file to an XDM schema and ingested it into [!DNL Platform]. このデータは、などのダウンストリーム [!DNL Platform] サービスで使用できるようになり [!DNL Real-time Customer Profile]ました。 詳細については、 [[!DNLリアルタイム顧客プロファイル]の概要を参照してください](../../profile/home.md) 。
