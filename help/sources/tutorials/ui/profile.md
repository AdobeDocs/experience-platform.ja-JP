---
keywords: Experience Platform;home;popular topics;activate inbound data;populate profile;populate rtcp;populated unified profile
solution: Experience Platform
title: 受信ソースデータをアクティブ化して顧客プロファイルを入力します
topic: overview
type: Tutorial
description: ソースコネクタから受信するデータは、リアルタイム顧客プロファイルデータの強化と埋め込みに使用できます。
translation-type: tm+mt
source-git-commit: f86f7483e7e78edf106ddd34dc825389dadae26a
workflow-type: tm+mt
source-wordcount: '489'
ht-degree: 15%

---


# 受信ソースデータをアクティブ化して顧客プロファイルを入力します

ソースコネクタから受信するデータは、データの富化と埋め込みに使用でき [!DNL Real-time Customer Profile] ます。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

また、このチュートリアルでは、ソースコネクタを既に作成し、設定している必要があります。  UIで異なるコネクタを作成するためのチュートリアルのリストは、 [ソースコネクタの概要](../../home.md)。

## データの入力 [!DNL Real-time Customer Profile]

顧客のプロファイルを強化するには、ターゲットデータセットのソーススキーマがでの使用に対して互換性がある必要があり [!DNL Real-time Customer Profile]ます。 互換性のあるスキーマは、次の要件を満たします。

- スキーマに、ID プロパティとして指定された属性が 1 つ以上あります。
- スキーマに、プライマリ ID として定義された ID プロパティがあります。
- データフロー内のマッピングは、プライマリIDがターゲット属性である場合に存在します。

「ソース」ワークスペース内で、「 **[!UICONTROL 参照]** 」タブをクリックして、ベース接続をリストします。 表示されたリストで、プロファイルに入力するデータフローを含む接続を探します。 接続の名前をクリックして詳細を表示します。

![](../../images/tutorials/dataflow/cloud-storage/batch/browse.png)

接続の **[!UICONTROL ソースアクティビティ]** 画面が表示され、接続がソースデータを取り込むデータセットが表示されます。 有効にするデータセットの名前をクリックし [!DNL Profile]ます。

![](../../images/tutorials/dataflow/cloud-storage/batch/dataset-dataflow.png)

[ **[!UICONTROL データセットアクティビティ]** ]画面が表示されます。 画面の右側の **[!UICONTROL プロパティ]** 列には、データセットの詳細が表示され、 **** プロファイルスイッチとデータセットが属するスキーマへのリンクが含まれています。 スキーマの名前をクリックして、構成を表示します。

![](../../images/tutorials/dataflow/cloud-storage/batch/select-dataset-schema.png)

**[!UICONTROL スキーマエディタ]** (Editor)が表示され、中央のキャンバスにスキーマの構造が表示されます。 キャンバス内で、プライマリIDとして設定するフィールドを選択します。 表示される「 **[!UICONTROL フィールドプロパティ]** 」タブで、「 **[!UICONTROL ID]** 」チェックボックスを選択し、 **[!UICONTROL プライマリID]**&#x200B;を選択します。 最後に、適切な **[!UICONTROL ID名前空間を選択し]**、「 **[!UICONTROL Apply]**」をクリックします。

![](../../images/tutorials/dataflow/cloud-storage/batch/set-schema-identity.png)

スキーマの構造の最上位のオブジェクトをクリックすると、 **[!UICONTROL スキーマプロパティ]** 列が表示されます。 プロファイルスイッチを切り替え [!DNL Profile] て、のスキーマを有効にし **[!UICONTROL ます]** 。 「 **[!UICONTROL 保存]** 」をクリックして変更を確定します。

![](../../images/tutorials/dataflow/cloud-storage/batch/enable-profile.png)

スキーマが有効になったら、データセットアクティビティ [!DNL Profile]画面に戻り、「プ **[!UICONTROL ロパティ]** 」列内でプロファイル [!DNL Profile] の切り替えをクリックして、データセットを有効に ******** します。

![](../../images/tutorials/dataflow/cloud-storage/batch/enable-dataset-profile.png)

スキーマとデータセットの両方を有効にした状態で [!DNL Profile]、そのデータセットに取り込まれたデータも顧客のプロファイルに割り当てられるようになりました。

>[!NOTE]
>
>最近有効にしたデータセット内の既存のデータは、で消費されません [!DNL Profile]。

## 次の手順

このチュートリアルに従うと、 [!DNL Profile] 母集団の受信データを正常にアクティブ化できます。 詳しくは、[[!DNL Real-time Customer Profile]  の概要](../../../profile/home.md)を参照してください。