---
keywords: Experience Platform；ホーム；人気の高いトピック；インバウンドデータのアクティブ化；プロファイルの入力；rtcp の入力；入力された統合プロファイル
solution: Experience Platform
title: インバウンドソースデータをアクティブ化して UI に顧客プロファイルを入力する
topic-legacy: overview
type: Tutorial
description: ソースコネクタからの受信データは、リアルタイム顧客プロファイルデータのエンリッチメントと生成に使用できます。
exl-id: ddd3766a-3f55-4bbc-8358-c578eae2c629
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 22%

---

# 顧客プロファイルに入力するインバウンドソースデータを有効化

ソースコネクタからの受信データは、 [!DNL Real-Time Customer Profile] データ。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)]  システム](../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

さらに、このチュートリアルでは、ソースコネクタを既に作成して設定している必要があります。  UI で様々なコネクタを作成するためのチュートリアルのリストは、 [ソースコネクタの概要](../../home.md).

## を [!DNL Real-Time Customer Profile] データ

顧客プロファイルを強化するには、ターゲットデータセットのソーススキーマが、 [!DNL Real-Time Customer Profile]. 互換性のあるスキーマは、次の要件を満たします。

- スキーマに、ID プロパティとして指定された属性が 1 つ以上あります。
- スキーマに、プライマリ ID として定義された ID プロパティがあります。
- データフロー内のマッピングが存在し、プライマリ ID はターゲット属性です。

「ソース」ワークスペースで、 **[!UICONTROL 参照]** タブをクリックして、ベース接続の一覧を表示します。 表示されたリストで、プロファイルに入力するデータフローを含む接続を見つけます。 接続の詳細にアクセスするには、接続の名前をクリックします。

![](../../images/tutorials/dataflow/cloud-storage/batch/browse.png)

接続の **[!UICONTROL ソースアクティビティ]** 画面が表示され、接続がソースデータを取り込んでいるデータセットが表示されます。 有効にするデータセットの名前をクリックします [!DNL Profile].

![](../../images/tutorials/dataflow/cloud-storage/batch/dataset-dataflow.png)

この **[!UICONTROL データセットアクティビティ]** 画面が表示されます。 この **[!UICONTROL プロパティ]** 列には、データセットの詳細が表示され、 **[!UICONTROL プロファイル]** スイッチして、データセットが準拠するスキーマへのリンクを設定します。 スキーマの名前をクリックして、構成を表示します。

![](../../images/tutorials/dataflow/cloud-storage/batch/select-dataset-schema.png)

この **[!UICONTROL スキーマエディター]** が表示され、中央のキャンバスにスキーマの構造が表示されます。 キャンバス内で、プライマリ ID として設定するフィールドを選択します。 以下 **[!UICONTROL フィールドプロパティ]** 表示されるタブで、 **[!UICONTROL ID]** チェックボックス、 **[!UICONTROL プライマリID]**. 最後に、適切な **[!UICONTROL ID 名前空間]**&#x200B;を選択し、「 **[!UICONTROL 適用]**.

![](../../images/tutorials/dataflow/cloud-storage/batch/set-schema-identity.png)

スキーマの構造の最上位オブジェクト ( **[!UICONTROL スキーマのプロパティ]** 列が表示されます。 のスキーマを有効にする [!DNL Profile] 切り替えて **[!UICONTROL プロファイル]** スイッチ クリック **[!UICONTROL 保存]** 変更を確定します。

![](../../images/tutorials/dataflow/cloud-storage/batch/enable-profile.png)

これで、スキーマが [!DNL Profile]、 **[!UICONTROL データセットアクティビティ]** 画面を表示し、次のデータセットを有効にします。 [!DNL Profile] クリックして **[!UICONTROL プロファイル]** 内で切り替える **[!UICONTROL プロパティ]** 列。

![](../../images/tutorials/dataflow/cloud-storage/batch/enable-dataset-profile.png)

スキーマとデータセットの両方を [!DNL Profile]の場合、そのデータセットに取り込まれたデータも顧客プロファイルに入力されるようになります。

>[!NOTE]
>
>最近有効にしたデータセット内の既存のデータは、 [!DNL Profile].

## 次の手順

このチュートリアルに従うことで、の受信データを正常にアクティブ化できました [!DNL Profile] 母集団。 詳しくは、[[!DNL Real-Time Customer Profile] 概要](../../../profile/home.md)を参照してください。
