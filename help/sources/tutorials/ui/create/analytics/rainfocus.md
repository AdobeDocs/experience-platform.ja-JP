---
title: UI を使用して RainFocus アカウントをExperience Platformに接続する
description: UI を使用して RainFocus アカウントをExperience Platformに接続する方法を説明します。
badge: ベータ版
exl-id: a349e37e-9f2c-47ff-8360-ccbe578dce27
source-git-commit: b4334b4f73428f94f5a7e5088f98e2459afcaf3c
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 29%

---

# 接続する [!DNL RainFocus] UI を使用してExperience Platformにアカウント

>[!NOTE]
>
>[!DNL RainFocus] ソースはベータ版です。詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

このチュートリアルでは、 [!DNL RainFocus] アカウントを使用して、イベント管理および analytics データをAdobe Experience Platformにストリーミングします。

>[!IMPORTANT]
>
>このソースコネクタとドキュメントページは、 [!DNL RainFocus] チーム。 お問い合わせや更新のご依頼は、clientcare から直接お問い合わせください。<span>@rainfocus.comまたは [[!DNL RainFocus] ヘルプセンター](https://help.rainfocus.com/hc/en-us)

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

### 前提条件

接続する前に [!DNL RainFocus] アカウントからExperience Platformに移行する場合は、まず、前提条件となるタスクを実行する必要があります。

* [必要な資格情報の収集](../../../../connectors/analytics/rainfocus.md#gather-required-credentials)
* [XDM スキーマの作成と ID フィールドの定義](../../../../connectors/analytics/rainfocus.md#create-an-xdm-schema-and-define-the-identity-field)
* [RainFocus での統合プロファイルの作成](../../../../connectors/analytics/rainfocus.md#create-an-integration-profile-in-rainfocus)

前提条件の設定が完了したら、次の手順に進むことができます。

## RainFocus アカウントをExperience Platformに接続

Platform UI で、「 」を選択します。 **[!UICONTROL ソース]** 左側のナビゲーションバーから、ソースワークスペースにアクセスします。 *[!UICONTROL カタログ]*&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

の下 *[!UICONTROL Analytics]* カテゴリ、選択 **[!UICONTROL RainFocus エクスペリエンス]**&#x200B;を選択し、 **[!UICONTROL データを追加]**.

![RainFocus ソースが選択されたExperience PlatformUI のソースカタログ。](/help/sources/images/tutorials/create/rainfocus/rainfocus_sources-rf.png)

## データの選択

「データの選択」手順が表示され、Experience Platformに取り込むデータを選択するためのインターフェイスが提供されます。

* インターフェイスの左側には、アカウント内で使用可能なデータストリームを表示できるブラウザーがあります。
* インターフェイスの右側では、JSON ファイルから最大 100 行のデータをプレビューできます。

選択 **[!UICONTROL ファイルをアップロード]** をクリックして、ローカルシステムから JSON ファイルをアップロードします。 または、アップロードする JSON ファイルを「ファイルをドラッグ&amp;ドロップ」パネルにドラッグ&amp;ドロップします。

からダウンロードしたサンプル JSON ペイロードをアップロードします。 **RainFocus**.

![ソースワークフローの「データを選択」ステップ。](/help/sources/images/tutorials/create/rainfocus/rainfocus_source-json-upload.png)

ファイルがアップロードされると、プレビューインターフェイスが更新され、アップロードしたスキーマのプレビューが表示されます。 プレビューインターフェイスを使用すると、ファイルの内容と構造を検査できます。 検索フィールドユーティリティを使用して、スキーマ内の特定の項目にアクセスすることもできます。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ソースワークフローのデータのプレビューステップ。](/help/sources/images/tutorials/create/rainfocus/rainfocus_source-json-preview.png)

## データフローの詳細

The **データフローの詳細** 手順が表示され、既存のデータセットを使用するか、データフローの新しいデータセットを確立するか、およびデータフローの名前と説明を指定する機会が提供されます。 この手順では、プロファイルの取り込み、エラー診断、部分取り込み、アラートの設定も指定できます。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ソースワークフローのデータフローの詳細手順。](/help/sources/images/tutorials/create/rainfocus/rainfocus_source-dataflow-setup.png)

## マッピング {#mapping}

マッピング手順が表示され、ソーススキーマのソースフィールドを、ターゲットスキーマの適切なターゲット XDM フィールドにマッピングするためのインターフェイスが提供されます。

Experience Platformは、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対するインテリジェントなレコメンデーションを提供します。 マッピングルールは、ユースケースに合わせて手動で調整できます。必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドを使用した包括的な手順については、 [データ準備 UI ガイド](../../../../../data-prep/ui/mapping.md).

ソースデータが正常にマッピングされたら、「 」を選択します。 **[!UICONTROL 次へ]**.

![ソースワークフローのマッピング手順です。](/help/sources/images/tutorials/create/rainfocus/rainfocus_source-mappings.png)

## レビュー

**レビュー**&#x200B;手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

* **接続**：ソースのタイプ、選択したソースファイルの関連パスおよびそのソースファイル内の列の数を表示します。
* **データセットの割り当てとフィールドのマッピング**：ソースデータがどのデータセットに取り込まれるかを、そのデータセットが準拠するスキーマを含めて表示します。

データフローをレビューしたら、「**終了**」を選択し、データフローが作成されるまでしばらく待ちます。

![ソースワークフローのレビューステップ。](/help/sources/images/tutorials/create/rainfocus/rainfocus_source-compelete.png)

## ストリーミングエンドポイント URL を取得する {#get-your-streaming-endpoint-url}

ストリーミングデータフローを作成したら、ストリーミングエンドポイント URL を取得できます。 このエンドポイントは、Webhook を購読するために使用され、ストリーミングソースとExperience Platformが通信できます。

ストリーミングエンドポイントを取得するには、 *[!UICONTROL データフローアクティビティ]* 作成したデータフローのページで、エンドポイントをの下部からコピーします。 *[!UICONTROL プロパティ]* パネル。

![ソースワークスペースのデータフローアクティビティページ（ストリーミングエンドポイント URL が強調表示されています）。](/help/sources/images/tutorials/create/rainfocus/rainfocus_source-dataflow-api.png)

## RainFocus での統合プロファイルのアクティブ化

データフローが完了し、ストリーミングエンドポイント URL を取得したら、 [!DNL Integration Profile] in [!DNL RainFocus].

* にログインします。 [[!DNL RainFocus] platform](https://app.rainfocus.com). プライマリナビゲーションで、「 」を選択します。 **[!DNL Libraries]** および **[!DNL Integration Profiles]**
* を開きます。 [!DNL Integration Profile] 以前に [前提条件](../../../../connectors/analytics/rainfocus.md#create-an-integration-profile-in-rainfocus).
* を貼り付けます。 **データフロー ID** および **ストリーミングエンドポイント** データフローからコピーしたExperience Platformを選択し、 **保存**

## 次の手順

このチュートリアルに従うことで、 [!DNL RainFocus] ソースに含まれます。これにより、イベント管理および分析データをExperience Platformにストリーミングできます。

## その他のリソース

以下のドキュメントは、 [!DNL RainFocus] ソース。

* [RainFocus ヘルプセンター](https://help.rainfocus.com/hc/en-us)
* [Adobe Developer Portal でのAdobe サービスアカウント (JWT) の作成](https://developer.adobe.com/developer-console/docs/guides/authentication/ServiceAccountIntegration/)
* [API でのスキーマの作成](../../../../../xdm/tutorials/create-schema-api.md)
* [UI でのスキーマの作成](../../../../../xdm/tutorials/create-schema-ui.md)
* [UI での ID フィールドの定義](https://experienceleague.adobe.com/docs/experience-platform/xdm/ui/fields/identity.html)
