---
keywords: Experience Platform；ホーム；人気のあるトピック；受信データのアクティブ化；プロファイルの入力；rtcp；入力された統合プロファイル
solution: Experience Platform
title: UIで顧客プロファイルを入力するためのインバウンドソースデータのアクティブ化
topic-legacy: overview
type: Tutorial
description: ソースコネクタから受信するデータは、リアルタイム顧客プロファイルデータの強化と埋め込みに使用できます。
exl-id: ddd3766a-3f55-4bbc-8358-c578eae2c629
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 14%

---

# 受信ソースデータをアクティブ化して顧客プロファイルを入力します

ソースコネクタからの受信データは、[!DNL Real-time Customer Profile]データを豊かにし、埋め込むために使用できます。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 編成する際に使用される標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

また、このチュートリアルでは、ソースコネクタを既に作成し、設定している必要があります。  UIで異なるコネクタを作成するためのチュートリアルのリストは、[ソースコネクタの概要](../../home.md)を参照してください。

## [!DNL Real-time Customer Profile]データを入力

顧客のプロファイルを強化するには、ターゲットデータセットのソーススキーマが[!DNL Real-time Customer Profile]で使用するために互換性がある必要があります。 互換性のあるスキーマは、次の要件を満たします。

- スキーマに、ID プロパティとして指定された属性が 1 つ以上あります。
- スキーマに、プライマリ ID として定義された ID プロパティがあります。
- データフロー内のマッピングは、プライマリIDがターゲット属性である場合に存在します。

「ソース」ワークスペース内で、「**[!UICONTROL 参照]**」タブをクリックして、ベース接続をリストします。 表示されたリストで、プロファイルに入力するデータフローを含む接続を探します。 接続の名前をクリックして詳細を表示します。

![](../../images/tutorials/dataflow/cloud-storage/batch/browse.png)

接続の&#x200B;**[!UICONTROL ソースアクティビティ]**&#x200B;画面が開き、接続がソースデータを取り込むデータセットが表示されます。 [!DNL Profile]に対して有効にするデータセットの名前をクリックします。

![](../../images/tutorials/dataflow/cloud-storage/batch/dataset-dataflow.png)

**[!UICONTROL データセットアクティビティ]**&#x200B;画面が表示されます。 画面の右側の&#x200B;**[!UICONTROL プロパティ]**&#x200B;列には、データセットの詳細が表示され、**[!UICONTROL プロファイル]**&#x200B;スイッチと、データセットが準拠するスキーマへのリンクが含まれています。 スキーマの名前をクリックして、構成を表示します。

![](../../images/tutorials/dataflow/cloud-storage/batch/select-dataset-schema.png)

**[!UICONTROL スキーマエディター]**&#x200B;が表示され、中央のキャンバスにスキーマの構造が表示されます。 キャンバス内で、プライマリIDとして設定するフィールドを選択します。 表示される&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;タブで、**[!UICONTROL 「ID」]**&#x200B;チェックボックスを選択し、**[!UICONTROL プライマリID]**&#x200B;を選択します。 最後に、適切な&#x200B;**[!UICONTROL ID名前空間]**&#x200B;を選択し、**[!UICONTROL 「Apply]**」をクリックします。

![](../../images/tutorials/dataflow/cloud-storage/batch/set-schema-identity.png)

スキーマの構造の最上位のオブジェクトをクリックすると、**[!UICONTROL スキーマプロパティ]**&#x200B;列が表示されます。 **[!UICONTROL プロファイル]**&#x200B;スイッチを切り替えて、[!DNL Profile]のスキーマを有効にします。 「**[!UICONTROL 保存]**」をクリックして変更を終了します。

![](../../images/tutorials/dataflow/cloud-storage/batch/enable-profile.png)

スキーマが[!DNL Profile]に対して有効になったら、**[!UICONTROL データセットアクティビティ]**&#x200B;画面に戻り、**[!UICONTROL プロパティ]**&#x200B;列内の&#x200B;**[!UICONTROL プロファイル]**&#x200B;をクリックして[!DNL Profile]のデータセットを有効にします。

![](../../images/tutorials/dataflow/cloud-storage/batch/enable-dataset-profile.png)

[!DNL Profile]に対してスキーマとデータセットの両方を有効にすると、そのデータセットに取り込まれたデータも顧客のプロファイルに反映されます。

>[!NOTE]
>
>最近有効にしたデータセット内の既存のデータは、[!DNL Profile]で消費されません。

## 次の手順

このチュートリアルに従うと、[!DNL Profile]母集団の受信データを正常にアクティブ化できます。 詳しくは、[[!DNL Real-time Customer Profile]  の概要](../../../profile/home.md)を参照してください。
