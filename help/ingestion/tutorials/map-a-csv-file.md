---
keywords: Experience Platform；ホーム；人気のあるトピック；CSVのマップ；CSVファイルのマップ；CSVファイルのxdmへのマップ；CSVのxdmへのマップ；uiガイド；
solution: Experience Platform
title: CSVファイルのXDMスキーマへのマップ
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Adobe Experience Platformのユーザーインターフェイスを使用してCSVファイルをXDMスキーマにマップする方法について説明します。
exl-id: 15f55562-269d-421d-ad3a-5c10fb8f109c
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '882'
ht-degree: 14%

---

# XDM スキーマへの CSV ファイルのマッピング

CSVデータを[!DNL Adobe Experience Platform]に取り込むには、データを[!DNL Experience Data Model] (XDM)スキーマにマップする必要があります。 このチュートリアルでは、CSVファイルを[!DNL Platform]ユーザーインターフェイスを使用してXDMスキーマにマップする方法について説明します。

また、このチュートリアルの付録では、[マッピング関数](#mapping-functions)の使用に関する詳細情報を提供します。

## はじめに

このチュートリアルでは、[!DNL Platform]の次のコンポーネントについて十分に理解している必要があります。

- [[!DNL Experience Data Model (XDM System)]](../../xdm/home.md)：顧客体験データを編成する際に [!DNL Platform] に使用される標準化されたフレームワーク。
- [[!DNL Batch ingestion]](../batch-ingestion/overview.md):ユーザーが指定したデータファイルからデータを [!DNL Platform] 取り込む方法。

また、このチュートリアルでは、CSV データの取り込み先のデータセットを既に作成している必要があります。UI でデータセットを作成する手順については、[データ取得のチュートリアル](./ingest-batch-data.md)を参照してください。

## 宛先の選択

[[!DNL Adobe Experience Platform]](https://platform.adobe.com)にログインし、左のナビゲーションバーで&#x200B;**[!UICONTROL ワークフロー]**&#x200B;を選択して、**[!UICONTROL ワークフロー]**&#x200B;ワークスペースにアクセスします。

**[!UICONTROL ワークフロー]**&#x200B;画面の「**[!UICONTROL データ収集]**」セクションで「**[!UICONTROL CSVをXDMスキーマ]**&#x200B;にマップ」を選択し、「**[!UICONTROL 起動]**」を選択します。

![](../images/tutorials/map-a-csv-file/workflows.png)

**[!UICONTROL Map CSV to XDMスキーマ]**&#x200B;ワークフローが表示され、**[!UICONTROL 宛先]**&#x200B;の手順から開始します。 取り込む受信データのデータセットを選択します。 既存のデータセットを使用することも、新しいデータセットを作成することもできます。

**既存のデータセットを使用する**

CSVデータを既存のデータセットに取り込むには、「**[!UICONTROL 既存のデータセットを使用]**」を選択します。 検索関数を使用して既存のデータセットを取得するか、パネル内の既存のデータセットのリストをスクロールして取得できます。

![](../images/tutorials/map-a-csv-file/use-existing-dataset.png)

CSVデータを新しいデータセットに取り込むには、**[!UICONTROL 新しいデータセットを作成]**&#x200B;を選択し、与えられたフィールドにデータセットの名前と説明を入力します。 検索機能を使用するか、提供されるスキーマのリストをスクロールして、スキーマを選択します。 「**[!UICONTROL 次へ]**」を選択して次に進みます。

![](../images/tutorials/map-a-csv-file/create-new-dataset.png)

## データの追加

「**[!UICONTROL データ追加]**」手順が表示されます。CSVファイルを用意されているスペースにドラッグ&amp;ドロップするか、「**[!UICONTROL ファイルを選択]**」を選択してCSVファイルを手動で入力します。

![](../images/tutorials/map-a-csv-file/add-data.png)

ファイルがアップロードされると&#x200B;**[!UICONTROL サンプルデータ]**&#x200B;セクションが表示され、最初の10行のデータが表示されます。 データが期待どおりにアップロードされたことを確認したら、「**[!UICONTROL 次へ]**」を選択します。

![](../images/tutorials/map-a-csv-file/sample-data.png)

## XDM スキーマフィールドへの CSV フィールドのマッピング

「**[!UICONTROL マッピング]**」手順が表示されます。CSV ファイルの列は「**[!UICONTROL ソースフィールド]**」の下にリストされ、対応する XDM スキーマフィールドが「**[!UICONTROL ターゲットフィールド]**」の下にリストされます。

[!DNL Platform] 選択したターゲットスキーマまたはデータセットに基づいて、自動マップされるフィールドに対して高度なレコメンデーションを自動的に提供します。使用事例に合わせて手動でマッピングルールを調整できます。

![](../images/tutorials/map-a-csv-file/mapping-with-suggestions.png)

すべての自動生成マッピング値を受け入れるには、「[!UICONTROL すべてのターゲットフィールドを受け入れる]」というチェックボックスを選択します。

![](../images/tutorials/map-a-csv-file/filled-mapping-with-suggestions.png)

ソーススキーマで複数のレコメンデーションが使用できる場合があります。 この場合、マッピングカードに最も目立つレコメンデーションが表示され、その後に使用可能な追加のレコメンデーション数を含む青い円が表示されます。 電球アイコンを選択すると、追加の推奨のリストが表示されます。 代わりにマップ先のレコメンデーションの横にあるチェックボックスをオンにして、代替レコメンデーションの1つを選択できます。

![](../images/tutorials/map-a-csv-file/multiple-recommendations.png)

または、ソーススキーマをターゲットスキーマに手動でマップすることもできます。 マップするソーススキーマの上にカーソルを置き、プラスアイコンを選択します。

![](../images/tutorials/map-a-csv-file/mapping-with-suggestions-and-buttons.png)

**[!UICONTROL ソースをターゲットフィールド]**&#x200B;にマップするポーバーが表示されます。 ここから、マッピングするフィールドを選択し、次に&#x200B;**[!UICONTROL 保存]**&#x200B;を実行して新しいマッピングを追加します。

![](../images/tutorials/map-a-csv-file/manual-mapping.png)

マッピングの1つを削除する場合は、そのマッピングの上にマウスポインターを置いて、マイナスアイコンを選択します。

### 追加計算フィールド

計算済みフィールドでは、入力スキーマーの属性に基づいて値を作成できます。 これらの値をターゲットスキーマの属性に割り当て、名前と説明を指定して、参照しやすくすることができます。

先に進むには、**[!UICONTROL 追加計算済みフィールド]**&#x200B;ボタンを選択します。

![](../images/tutorials/map-a-csv-file/add-calculated-field.png)

「**[!UICONTROL 計算済みフィールドを作成]**」パネルが表示されます。 左側のダイアログボックスには、計算フィールドでサポートされるフィールド、関数、演算子が含まれています。 いずれかのタブを選択して、式エディタに関数、フィールドまたは演算子を追加する開始を行います。

![](../images/tutorials/map-a-csv-file/create-calculated-fields.png)

| タブ | 説明 |
| --------- | ----------- |
| フィールド | 「フィールド」タブのリストフィールドと属性は、ソーススキーマで使用できます。 |
| 関数 | 「関数」タブには、データの変換に使用できる関数がリストされます。 計算フィールド内で使用できる関数の詳細については、[データ準備（マッパー）関数](../../data-prep/functions.md)のガイドを参照してください。 |
| 演算子 | 「operators」タブには、データの変換に使用できる演算子がリストされます。 |

中央の式エディターを使用して、手動でフィールド、関数および演算子を追加できます。 式の作成を開始するエディタを選択します。

![](../images/tutorials/map-a-csv-file/create-calculated-field.png)

「**[!UICONTROL 保存]**」を選択して続行します。

マッピング画面が再び開き、新しく作成したソースフィールドが表示されます。 対応するターゲットフィールドを適用し、「**[!UICONTROL 完了]**」を選択してマッピングを完了します。

![](../images/tutorials/map-a-csv-file/new-calculated-field.png)

## データ取得の監視

CSVファイルをマッピングして作成したら、ファイルを介して取り込まれるデータを監視できます。 データ取り込みの監視について詳しくは、[データ取り込みの監視](../../ingestion/quality/monitor-data-ingestion.md)のチュートリアルを参照してください。

## 次の手順

このチュートリアルに従うと、フラットなCSVファイルをXDMスキーマに正常にマッピングし、[!DNL Platform]に取り込むことができます。 このデータは、現在は[!DNL Real-time Customer Profile]などのダウンストリーム[!DNL Platform]サービスで使用できます。 詳しくは、[[!DNL Real-time Customer Profile]](../../profile/home.md)の概要を参照してください。
