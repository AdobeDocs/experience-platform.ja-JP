---
title: Ui での Shopify ストリーミング接続とデータフローの作成
description: Experience Platform ユーザーインターフェイスを使用して Shopify ストリーミングソース接続とデータフローを作成する方法について説明します
badge: ベータ版
exl-id: d53f4ab5-8bdc-4647-83d5-ee898abda0f2
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 32%

---

# UI を使用した [!DNL Shopify Streaming] データのソース接続とデータフローの作成

このチュートリアルでは、Experience Platform ユーザーインターフェイスを使用して [!DNL Shopify Streaming] ソース接続とデータフローを作成する手順について説明します。

## はじめに {#getting-started}

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

>[!IMPORTANT]
>
>このチュートリアルでは、[!DNL Shopify Streaming] アカウントの前提条件の設定を完了している必要があります。 アカウントの設定手順については、[[!DNL Shopify Streaming]  概要 &#x200B;](../../../../connectors/ecommerce/shopify-streaming.md) を参照してください。

## [!DNL Shopify Streaming] アカウントを接続

Experience Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、「[!UICONTROL &#x200B; ソース &#x200B;] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

**e コマース** カテゴリで、「[!DNL Shopify Streaming]」を選択し、次に **[!UICONTROL データを追加]** を選択します。

![Experience Platform ソースカタログ &#x200B;](../../../../images/tutorials/create/shopify-streaming/catalog.png)

## データの選択

**[!UICONTROL データを選択]** 手順が表示され、Experience Platformに取り込むデータを選択するためのインターフェイスが表示されます。

* インターフェイスの左側は、アカウント内で利用可能なデータストリームを表示できるブラウザーです。
* インターフェイスの右側の部分では、JSON ファイルから最大 100 行のデータをプレビューできます。

**[!UICONTROL ファイルをアップロード]** を選択して、ローカルシステムから JSON ファイルをアップロードします。 または、アップロードする JSON ファイルを [!UICONTROL &#x200B; ファイルをドラッグ&amp;ドロップ &#x200B;] パネルにドラッグ&amp;ドロップすることもできます。

![&#x200B; ソースワークフローのデータを追加ステップ &#x200B;](../../../../images/tutorials/create/shopify-streaming/select-data.png)

ファイルがアップロードされると、プレビューインターフェイスが更新され、アップロードしたスキーマのプレビューが表示されます。 プレビューインターフェイスを使用すると、ファイルの内容と構造を検査できます。 また、[!UICONTROL &#x200B; フィールドを検索 &#x200B;] ユーティリティを使用して、スキーマ内から特定の項目にアクセスすることもできます。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![&#x200B; ソースワークフローのプレビュー手順 &#x200B;](../../../../images/tutorials/create/shopify-streaming/preview.png)

## データフローの詳細

**データフローの詳細** 手順が表示され、既存のデータセットを使用するか、データフローの新しいデータセットを確立するかのオプションと、データフローの名前と説明を入力する機会が提供されます。 この手順では、プロファイルの取り込み、エラー診断、部分取り込み、アラートの設定も指定できます。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![&#x200B; ソースワークフローのデータフローの詳細手順。](../../../../images/tutorials/create/shopify-streaming/dataflow-detail.png)

## マッピング

[!UICONTROL マッピング]手順が表示され、ソーススキーマのソースフィールドを、ターゲットスキーマの適切なターゲット XDM フィールドにマッピングするためのインターフェイスが提供されます。

Experience Platformは、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対してインテリジェントなレコメンデーションを提供します。 マッピングルールは、ユースケースに合わせて手動で調整できます。 必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドの使用に関する包括的な手順については、[&#x200B; データ準備 UI ガイド &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/data-prep/ui/mapping.html?lang=ja) を参照してください。

ソースデータが正常にマッピングされたら、「**[!UICONTROL 次へ]**」を選択します。

![&#x200B; ソースワークフローのマッピングステップ &#x200B;](../../../../images/tutorials/create/shopify-streaming/mapping.png)

## レビュー

**[!UICONTROL レビュー]**&#x200B;手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

* **[!UICONTROL 接続]**：ソースのタイプ、選択したソースファイルの関連パスおよびそのソースファイル内の列の数を表示します。
* **[!UICONTROL データセットの割り当てとフィールドのマッピング]**：ソースデータがどのデータセットに取り込まれるかを、そのデータセットが準拠するスキーマを含めて表示します。

データフローをレビューしたら、「**[!UICONTROL 終了]**」を選択し、データフローが作成されるまでしばらく待ちます。

![&#x200B; ソースワークフローのレビュー手順。](../../../../images/tutorials/create/shopify-streaming/review.png)

## ストリーミングエンドポイント URL の取得

ストリーミングデータフローを作成したので、ストリーミングエンドポイント URL を取得できるようになりました。 このエンドポイントは、Webhook をサブスクライブするために使用され、ストリーミングソースがExperience Platformと通信できるようになります。

ストリーミングエンドポイントを取得するには、作成したデータフローの [!UICONTROL &#x200B; データフローアクティビティ &#x200B;] ページに移動し、[!UICONTROL &#x200B; プロパティ &#x200B;] パネルの下部からエンドポイントをコピーします。

![&#x200B; データフローアクティビティのストリーミングエンドポイント。](../../../../images/tutorials/create/shopify-streaming/endpoint.png)

## 次の手順

このチュートリアルでは、[!DNL Shopify Streaming] アカウントへのソース接続とデータフローを確立しました。 API を使用して [!DNL Shopify Streaming] アカウントを接続する手順については、[Flow Service API を使用したソース接続とデータフローの作成  [!DNL Shopify]  に関するチュートリアルを参照してください &#x200B;](../../../api/create/ecommerce/shopify-streaming.md)。
