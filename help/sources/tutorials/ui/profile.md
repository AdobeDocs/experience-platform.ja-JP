---
keywords: Experience Platform；ホーム；人気のあるトピック；受信データのアクティブ化；プロファイルの入力；rtcp の入力；統合プロファイル
solution: Experience Platform
title: 受信ソースデータのアクティブ化による UI での顧客プロファイルへの入力
topic-legacy: overview
type: Tutorial
description: ソースコネクタからの受信データは、リアルタイム顧客プロファイルデータのエンリッチメントと入力に使用できます。
exl-id: ddd3766a-3f55-4bbc-8358-c578eae2c629
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 16%

---

# 顧客プロファイルに入力するインバウンドソースデータの有効化

ソースコネクタからの受信データは、[!DNL Real-time Customer Profile] データのエンリッチメントと入力に使用できます。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../xdm/home.md):顧客体験データを整理する際に使用す [!DNL Experience Platform] る標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

さらに、このチュートリアルでは、ソースコネクタを既に作成して設定している必要があります。  UI で異なるコネクタを作成するためのチュートリアルのリストは、[ ソースコネクタの概要 ](../../home.md) に記載されています。

## [!DNL Real-time Customer Profile] データの入力

顧客プロファイルを強化するには、[!DNL Real-time Customer Profile] での使用に対してターゲットデータセットのソーススキーマとの互換性が必要です。 互換性のあるスキーマは、次の要件を満たします。

- スキーマに、ID プロパティとして指定された属性が 1 つ以上あります。
- スキーマに、プライマリ ID として定義された ID プロパティがあります。
- データフロー内のマッピングが存在し、プライマリ ID はターゲット属性です。

「ソース」ワークスペース内で、「**[!UICONTROL 参照]**」タブをクリックして、ベース接続をリストします。 表示されたリストで、プロファイルに入力するデータフローを含む接続を探します。 接続の名前をクリックして、詳細にアクセスします。

![](../../images/tutorials/dataflow/cloud-storage/batch/browse.png)

接続の **[!UICONTROL ソースアクティビティ]** 画面が開き、接続がソースデータを取り込むデータセットが表示されます。 [!DNL Profile] に対して有効にするデータセットの名前をクリックします。

![](../../images/tutorials/dataflow/cloud-storage/batch/dataset-dataflow.png)

**[!UICONTROL データセットアクティビティ]** 画面が表示されます。 画面の右側の **[!UICONTROL プロパティ]** 列には、データセットの詳細が表示され、**[!UICONTROL プロファイル]** スイッチと、データセットが準拠するスキーマへのリンクが含まれます。 スキーマの名前をクリックして、構成を表示します。

![](../../images/tutorials/dataflow/cloud-storage/batch/select-dataset-schema.png)

**[!UICONTROL スキーマエディタ]** が表示され、中央のキャンバスにスキーマの構造が表示されます。 キャンバス内で、プライマリ ID として設定するフィールドを選択します。 表示される「**[!UICONTROL フィールドのプロパティ]**」タブで、「**[!UICONTROL ID]**」チェックボックスを選択し、「**[!UICONTROL プライマリID]**」を選択します。 最後に、適切な **[!UICONTROL ID 名前空間]** を選択し、**[!UICONTROL 「適用]**」をクリックします。

![](../../images/tutorials/dataflow/cloud-storage/batch/set-schema-identity.png)

スキーマの構造の最上位のオブジェクトをクリックすると、「**[!UICONTROL スキーマのプロパティ]**」列が表示されます。 **[!UICONTROL プロファイル]** スイッチを切り替えて、[!DNL Profile] のスキーマを有効にします。 「**[!UICONTROL 保存]**」をクリックして変更を確定します。

![](../../images/tutorials/dataflow/cloud-storage/batch/enable-profile.png)

スキーマが [!DNL Profile] に対して有効になったら、**[!UICONTROL データセットアクティビティ]** 画面に戻り、**[!UICONTROL プロパティ]** 列内の **[!UICONTROL プロファイル]** 切り替えをクリックして、[!DNL Profile] のデータセットを有効にします。

![](../../images/tutorials/dataflow/cloud-storage/batch/enable-dataset-profile.png)

[!DNL Profile] に対してスキーマとデータセットの両方を有効にすると、そのデータセットに取り込まれたデータも顧客プロファイルに取り込まれるようになります。

>[!NOTE]
>
>最近有効にしたデータセット内の既存のデータは、[!DNL Profile] によって消費されません。

## 次の手順

このチュートリアルでは、[!DNL Profile] 母集団の受信データを正常にアクティブ化しました。 詳しくは、[[!DNL Real-time Customer Profile]  概要 ](../../../profile/home.md) を参照してください。
