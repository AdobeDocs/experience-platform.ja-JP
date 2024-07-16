---
keywords: Experience Platform；ホーム；人気のトピック；受信データのアクティブ化；プロファイルの設定；rtcp の設定；入力された統合プロファイル
solution: Experience Platform
title: UI に顧客プロファイルを設定するためのインバウンド Source データの有効化
type: Tutorial
description: ソースコネクタからのインバウンドデータを使用して、リアルタイム顧客プロファイルデータのエンリッチメントと入力に役立てることができます。
exl-id: ddd3766a-3f55-4bbc-8358-c578eae2c629
source-git-commit: ed92bdcd965dc13ab83649aad87eddf53f7afd60
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 22%

---

# 顧客プロファイルに入力する受信ソースデータの有効化

ソースコネクタからの受信データを使用して、[!DNL Real-Time Customer Profile] データのエンリッチメントと入力を行うことができます。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)]  システム](../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディターのチュートリアル](../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-Time Customer Profile]](../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

また、このチュートリアルでは、ソースコネクタを既に作成および設定している必要があります。  UI で様々なコネクタを作成するためのチュートリアルのリストは、[ ソースコネクタの概要 ](../../home.md) を参照してください。

## [!DNL Real-Time Customer Profile] データの入力

顧客プロファイルを強化するには、ターゲットデータセットのソーススキーマが [!DNL Real-Time Customer Profile] で使用できるように互換性がある必要があります。 互換性のあるスキーマは、次の要件を満たします。

- スキーマに、ID プロパティとして指定された属性が 1 つ以上あります。
- スキーマに、プライマリ ID として定義された ID プロパティがあります。
- プライマリ ID がターゲット属性である、データフロー内のマッピングが存在します。

ソースワークスペース内で、「**[!UICONTROL 参照]**」タブをクリックして、ベース接続を一覧表示します。 表示されたリストで、プロファイルに入力するデータフローを含む接続を見つけます。 接続の名前をクリックして、詳細にアクセスします。

![](../../images/tutorials/dataflow/cloud-storage/batch/browse.png)

接続の **[!UICONTROL Source アクティビティ]** 画面が表示され、接続がソースデータを取り込むデータセットが表示されます。 [!DNL Profile] に対して有効にするデータセットの名前をクリックします。

![](../../images/tutorials/dataflow/cloud-storage/batch/dataset-dataflow.png)

**[!UICONTROL データセットアクティビティ]** 画面が表示されます。 画面の右側にある **[!UICONTROL プロパティ]** 列には、データセットの詳細が表示され、**[!UICONTROL プロファイル]** スイッチと、データセットが準拠するスキーマへのリンクが含まれます。 スキーマの名前をクリックして、その構成を表示します。

![](../../images/tutorials/dataflow/cloud-storage/batch/select-dataset-schema.png)

**[!UICONTROL スキーマエディター]** が表示され、中央のキャンバスにスキーマの構造が表示されます。 キャンバス内で、プライマリ ID として設定するフィールドを選択します。 表示された **[!UICONTROL フィールドプロパティ]** タブで、「**[!UICONTROL ID]**」チェックボックスをオンにし、「**[!UICONTROL プライマリ ID]**」をクリックします。 最後に、適切な **[!UICONTROL ID 名前空間]** を選択し、「**[!UICONTROL 適用]**」をクリックします。

![](../../images/tutorials/dataflow/cloud-storage/batch/set-schema-identity.png)

スキーマの構造の最上位オブジェクトをクリックすると、「**[!UICONTROL スキーマプロパティ]**」列が表示されます。 **[!UICONTROL プロファイル]** スイッチを切り替えて、[!DNL Profile] のスキーマを有効にします。 「**[!UICONTROL 保存]**」をクリックして、変更を最終決定します。

![](../../images/tutorials/dataflow/cloud-storage/batch/enable-profile.png)

スキーマが [!DNL Profile] に対して有効になったので、**[!UICONTROL データセットアクティビティ]** 画面に戻り、**[!UICONTROL プロパティ]** 列内の ]**プロファイル**[!UICONTROL  トグルをクリックして、データセットを [!DNL Profile] に対して有効にします。

![](../../images/tutorials/dataflow/cloud-storage/batch/enable-dataset-profile.png)

スキーマとデータセットの両方が [!DNL Profile] に対して有効になっている場合、そのデータセットに取り込まれたデータは、顧客プロファイルにも入力されるようになりました。

>[!NOTE]
>
>最近有効にしたデータセット内の既存データは、[!DNL Profile] では使用されません。

## 次の手順

このチュートリアルでは、母集団の受信データを正常にアクティブ化 [!DNL Profile] ました。 詳しくは、[[!DNL Real-Time Customer Profile] 概要](../../../profile/home.md)を参照してください。
