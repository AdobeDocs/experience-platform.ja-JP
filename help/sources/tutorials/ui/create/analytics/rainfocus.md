---
title: UI を使用した RainFocus アカウントのExperience Platformへの接続
description: UI を使用して RainFocus アカウントをExperience Platformに接続する方法を説明します。
badge: ベータ版
exl-id: a349e37e-9f2c-47ff-8360-ccbe578dce27
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '989'
ht-degree: 26%

---

# UI を使用した [!DNL RainFocus] アカウントのExperience Platformへの接続

>[!NOTE]
>
>[!DNL RainFocus] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、[ ソースの概要 ](../../../../home.md#terms-and-conditions) を参照してください。

このチュートリアルでは、[!DNL RainFocus] アカウントを連携し、イベント管理と Analytics データをAdobe Experience Platformにストリーミングする方法について説明します。

>[!IMPORTANT]
>
>このソースコネクタとドキュメントページは、[!DNL RainFocus] チームが作成および管理します。 お問い合わせや更新のリクエストについては、clientcare<span>@rainfocus.comまで直接ご連絡いただくか、[[!DNL RainFocus]  ヘルプセンター ](https://help.rainfocus.com/hc/en-us) をご覧ください。

## はじめに

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

### 前提条件

[!DNL RainFocus] アカウントをExperience Platformに接続する前に、まず次の前提条件のタスクを実行する必要があります。

* [必要な資格情報の収集](../../../../connectors/analytics/rainfocus.md#gather-required-credentials)
* [XDM スキーマを作成し、ID フィールドを定義します](../../../../connectors/analytics/rainfocus.md#create-an-xdm-schema-and-define-the-identity-field)
* [RainFocus での統合プロファイルの作成](../../../../connectors/analytics/rainfocus.md#create-an-integration-profile-in-rainfocus)

前提条件の設定が完了したら、以下に示す手順に進むことができます。

## RainFocus アカウントのExperience Platformへの接続

Experience Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、ソース ワークスペースにアクセスします。 *[!UICONTROL カタログ]*&#x200B;画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

*[!UICONTROL Analytics]* カテゴリ内で、「**[!UICONTROL RainFocus Experience]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![RainFocus ソースが選択されているExperience Platform UI のソースカタログ ](/help/sources/images/tutorials/create/rainfocus/rainfocus_sources-rf.png)

## データの選択

データを選択手順が表示され、Experience Platformに取り込むデータを選択するためのインターフェイスが表示されます。

* インターフェイスの左側は、アカウント内で利用可能なデータストリームを表示できるブラウザーです。
* インターフェイスの右側の部分では、JSON ファイルから最大 100 行のデータをプレビューできます。

**[!UICONTROL ファイルをアップロード]** を選択して、ローカルシステムから JSON ファイルをアップロードします。 または、アップロードする JSON ファイルをファイルをドラッグ&amp;ドロップ パネルにドラッグ&amp;ドロップすることもできます。

**RainFocus** からダウンロードしたサンプル JSON ペイロードをアップロードします。

![ ソースワークフローのデータを選択ステップ ](/help/sources/images/tutorials/create/rainfocus/rainfocus_source-json-upload.png)

ファイルがアップロードされると、プレビューインターフェイスが更新され、アップロードしたスキーマのプレビューが表示されます。 プレビューインターフェイスを使用すると、ファイルの内容と構造を検査できます。 また、検索フィールドユーティリティを使用して、スキーマ内から特定の項目にアクセスすることもできます。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ ソースワークフローのデータプレビュー手順。](/help/sources/images/tutorials/create/rainfocus/rainfocus_source-json-preview.png)

## データフローの詳細

**データフローの詳細** 手順が表示され、既存のデータセットを使用するか、データフローの新しいデータセットを確立するかのオプションと、データフローの名前と説明を入力する機会が提供されます。 この手順では、プロファイルの取り込み、エラー診断、部分取り込み、アラートの設定も指定できます。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ ソースワークフローのデータフローの詳細手順。](/help/sources/images/tutorials/create/rainfocus/rainfocus_source-dataflow-setup.png)

## マッピング {#mapping}

マッピング手順が表示され、ソーススキーマのソースフィールドを、ターゲットスキーマの適切なターゲット XDM フィールドにマッピングするためのインターフェイスが提供されます。

Experience Platformは、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対してインテリジェントなレコメンデーションを提供します。 マッピングルールは、ユースケースに合わせて手動で調整できます。 必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドの使用に関する包括的な手順については、[ データ準備 UI ガイド ](../../../../../data-prep/ui/mapping.md) を参照してください。

ソースデータが正常にマッピングされたら、「**[!UICONTROL 次へ]**」を選択します。

![ ソースワークフローのマッピングステップ ](/help/sources/images/tutorials/create/rainfocus/rainfocus_source-mappings.png)

## レビュー

**レビュー**&#x200B;手順が表示され、新しいデータフローを作成する前に確認できます。詳細は、次のカテゴリに分類されます。

* **接続**：ソースのタイプ、選択したソースファイルの関連パスおよびそのソースファイル内の列の数を表示します。
* **データセットの割り当てとフィールドのマッピング**：ソースデータがどのデータセットに取り込まれるかを、そのデータセットが準拠するスキーマを含めて表示します。

データフローをレビューしたら、「**終了**」を選択し、データフローが作成されるまでしばらく待ちます。

![ ソースワークフローのレビュー手順。](/help/sources/images/tutorials/create/rainfocus/rainfocus_source-compelete.png)

## ストリーミングエンドポイント URL の取得 {#get-your-streaming-endpoint-url}

ストリーミングデータフローを作成したので、ストリーミングエンドポイント URL を取得できるようになりました。 このエンドポイントは、Webhook をサブスクライブするために使用され、ストリーミングソースがExperience Platformと通信できるようになります。

ストリーミングエンドポイントを取得するには、作成したデータフローの *[!UICONTROL データフローアクティビティ]* ページに移動し、*[!UICONTROL プロパティ]* パネルの下部からエンドポイントをコピーします。

![ ストリーミングエンドポイント URL がハイライト表示されたソースワークスペースのデータフローアクティビティページ ](/help/sources/images/tutorials/create/rainfocus/rainfocus_source-dataflow-api.png)

## RainFocus での統合プロファイルのアクティブ化

データフローが完了し、ストリーミングエンドポイント URL を取得したら、[!DNL RainFocus] で [!DNL Integration Profile] をアクティブ化できます。

* [[!DNL RainFocus] platform](https://app.rainfocus.com) にログインします。 プライマリナビゲーションで、「**[!DNL Libraries]**」と「**[!DNL Integration Profiles]**」を選択します。
* [ 前提条件 ](../../../../connectors/analytics/rainfocus.md#create-an-integration-profile-in-rainfocus) の一部として、前の手順で作成した [!DNL Integration Profile] を開きます。
* Experience Platformのデータフローからコピーした **データフロー ID** と **ストリーミングエンドポイント** を貼り付けて、「**保存**」を選択します。

## 次の手順

このチュートリアルでは、[!DNL RainFocus] ソースの接続を確立し、イベント管理および分析データをExperience Platformにストリーミングできるようにします。

## その他のリソース

次のドキュメントでは、[!DNL RainFocus] ソースに関するニュアンスについて詳しく説明します。

* [RainFocus ヘルプセンター ](https://help.rainfocus.com/hc/en-us)
* [Adobe Developer ポータルでのAdobe サービスアカウント（JWT）の作成 ](https://developer.adobe.com/developer-console/docs/guides/authentication/ServiceAccountIntegration/)
* [API でのスキーマの作成](../../../../../xdm/tutorials/create-schema-api.md)
* [UI でのスキーマの作成](../../../../../xdm/tutorials/create-schema-ui.md)
* [UI での ID フィールドの定義 ](https://experienceleague.adobe.com/docs/experience-platform/xdm/ui/fields/identity.html)
