---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 受信ソースデータをアクティブ化して顧客プロファイルを入力します
topic: overview
translation-type: tm+mt
source-git-commit: d3c725c4760acb3857a67d0d30b24732c963a030
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---


# 受信ソースデータをアクティブ化して顧客プロファイルを入力します

ソースコネクタから受信するデータは、データの富化と埋め込みに使用でき [!DNL Real-time Customer Profile] ます。

## はじめに

このチュートリアルでは、次のAdobe Experience Platformのコンポーネントについて十分に理解している必要があります。

- [Experience Data Model(XDM)System](../../../xdm/home.md): 顧客体験データを [!DNL Experience Platform] 整理するための標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成における主な原則とベストプラクティスが含まれます。
   - [スキーマエディタのチュートリアル](../../../xdm/tutorials/create-schema-ui.md): スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [リアルタイム顧客プロファイル](../../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

また、このチュートリアルでは、ソースコネクタを既に作成し、設定している必要があります。  UIで異なるコネクタを作成するためのチュートリアルのリストは、 [ソースコネクタの概要](../../home.md)。

## データの入力 [!DNL Real-time Customer Profile]

顧客のプロファイルを強化するには、ターゲットデータセットのソーススキーマがでの使用に対して互換性がある必要があり [!DNL Real-time Customer Profile]ます。 互換性のあるスキーマは、次の要件を満たします。

- スキーマに、identityプロパティとして指定された属性が1つ以上あります。
- スキーマには、プライマリIDとして定義されたIDプロパティがあります。
- データフロー内のマッピングは、プライマリIDがターゲット属性である場合に存在します。

「ソース」ワークスペース内で、「 **[!UICONTROL 参照]** 」タブをクリックして、ベース接続をリストします。 表示されたリストで、プロファイルに入力するデータフローを含む接続を探します。 接続の名前をクリックして詳細を表示します。

![](../../images/tutorials/dataflow/cloud-storage/batch/browse.png)

接続の *[!UICONTROL ソースアクティビティ]* 画面が表示され、接続がソースデータを取り込むデータセットが表示されます。 有効にするデータセットの名前をクリックし [!DNL Profile]ます。

![](../../images/tutorials/dataflow/cloud-storage/batch/dataset-dataflow.png)

[ *[!UICONTROL データセットアクティビティ]* ]画面が表示されます。 画面の右側の *[!UICONTROL プロパティ]* 列には、データセットの詳細が表示され、 **** プロファイルスイッチとデータセットが属するスキーマへのリンクが含まれています。 スキーマの名前をクリックして、構成を表示します。

![](../../images/tutorials/dataflow/cloud-storage/batch/select-dataset-schema.png)

*[!UICONTROL スキーマエディタ]* (Editor)が表示され、中央のキャンバスにスキーマの構造が表示されます。 キャンバス内で、プライマリIDとして設定するフィールドを選択します。 表示される「 *[!UICONTROL フィールドプロパティ]* 」タブで、「 **[!UICONTROL ID]** 」チェックボックスを選択し、 **[!UICONTROL プライマリID]**&#x200B;を選択します。 最後に、適切な **[!UICONTROL ID名前空間を選択し]**、「 **[!UICONTROL Apply]**」をクリックします。

![](../../images/tutorials/dataflow/cloud-storage/batch/set-schema-identity.png)

スキーマの構造の最上位のオブジェクトをクリックすると、 *[!UICONTROL スキーマプロパティ]* 列が表示されます。 プロファイルスイッチを切り替え [!DNL Profile] て、のスキーマを有効にし **[!UICONTROL ます]** 。 「 **[!UICONTROL 保存]** 」をクリックして変更を確定します。

![](../../images/tutorials/dataflow/cloud-storage/batch/enable-profile.png)

スキーマが有効になったら、データセットアクティビティ [!DNL Profile]画面に戻り、「プ *[!UICONTROL ロパティ]* 」列内でプロファイル [!DNL Profile] の切り替えをクリックして、データセットを有効に ****** します。

![](../../images/tutorials/dataflow/cloud-storage/batch/enable-dataset-profile.png)

スキーマとデータセットの両方を有効にした状態で [!DNL Profile]、そのデータセットに取り込まれたデータも顧客のプロファイルに割り当てられるようになりました。

>[!NOTE] 最近有効にしたデータセット内の既存のデータは、 [!DNL Profile]

## 次の手順

このチュートリアルに従うと、 [!DNL Profile] 母集団の受信データを正常にアクティブ化できます。 詳しくは、 [リアルタイム顧客プロファイルの概要を参照してください](../../../profile/home.md)。