---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UI での Google Cloud ストレージソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: 41fe3e5b2a830c3182b46b3e0873b1672a1f1b03
workflow-type: tm+mt
source-wordcount: '540'
ht-degree: 16%

---


# Create a [!DNL Google Cloud Storage] source connector in the UI

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、ユー [!DNL Google Cloud Storage] ザインターフェイスを使用してソースコネクタ [!DNL Platform] （以下「GCS」と呼ばれます）を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [エクスペリエンスデータモデルl（XDM）システム](../../../../../xdm/home.md)：Experience Platform が顧客体験データを整理するための標準化されたフレームワークです。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [リアルタイム顧客プロファイル](../../../../../profile/home.md)：複数のソースから集約されたデータに基づいて、統合されたリアルタイムのコンシューマープロファイルを提供します。

GCSベースの接続が既にある場合は、このドキュメントの残りの部分をスキップし、データフローの [設定に関するチュートリアルに進むことができます](../../dataflow/batch/cloud-storage.md)。

### サポートされているファイル形式

[!DNL Experience Platform] は、外部ストレージから取り込む次のファイル形式をサポートしています。

* 区切り文字区切り値(DSV):DSV形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 DSV形式のファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 一般的なDSVファイルは、今後サポートされる予定です。
* JavaScript Object Notation (JSON):JSON形式のデータファイルは、XDMに準拠している必要があります。
* Apacheパーケット：パーケット形式のデータファイルは、XDMに準拠している必要があります。

### 必要な資格情報の収集

上のGCSデータにアクセスするに [!DNL Platform]は、有効なGCS **アクセスキーID** と ****&#x200B;シークレットを指定する必要があります。 これらの値の取得方法について詳しくは、の <a href="https://cloud.google.com/docs/authentication/production" target="_blank">サーバー間認証ガイド</a> を参照してくだ [!DNL Google Cloud]さい。

## GCSアカウントの接続

必要な資格情報を収集したら、次の手順に従って、接続先の新しいGCSアカウントを作成でき [!DNL Platform]ます。

[Adobe Experience Platform](https://platform.adobe.com) にログインし、左のナビゲーションバーで **[!UICONTROL 「ソース]** 」を選択して「 *[!UICONTROL ソース]* 」ワークスペースにアクセスします。 「 *[!UICONTROL カタログ]* 」画面には様々なソースが表示され、このソースを使用してインバウンドアカウントを作成できます。各ソースには、関連付けられた既存のアカウントおよびデータフローの数が表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「 *[!UICONTROL Databases]* 」 **[!UICONTROL カテゴリで、「]** Google Cloudストレージ **[!UICONTROL 」を選択し、次に「]** データ」を選択して、新しいGCSコネクタを作成します。

![カタログ](../../../../images/tutorials/create/google-cloud-storage/catalog.png)

「Google Cloud *[!UICONTROL ストレージに]* 接続」ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「 **[!UICONTROL 新規アカウント]**」を選択します。 表示される入力フォームで、接続に名前、オプションの説明およびGCS秘密鍵証明書を入力します。 完了したら、[ **[!UICONTROL 接続]** ]を選択し、新しいアカウントが確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/google-cloud-storage/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するGCSアカウントを選択し、「 **[!UICONTROL 次へ]** 」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/google-cloud-storage/existing.png)

## 次の手順

このチュートリアルに従って、GCSアカウントへの接続を確立しました。 次のチュートリアルに進み、クラウドストレージのデータをプラットフォームに取り込むようにデータフローを [設定できるようになりました](../../dataflow/batch/cloud-storage.md)。