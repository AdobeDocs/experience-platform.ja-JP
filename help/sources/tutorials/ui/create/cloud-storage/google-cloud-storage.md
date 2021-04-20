---
keywords: Experience Platform；ホーム；人気のあるトピック；Google Cloudストレージ;Google Cloudストレージ;GCS;gcs
solution: Experience Platform
title: UIでのGoogle Cloudストレージソース接続の作成
topic: overview
type: Tutorial
description: Adobe Experience PlatformUIを使用してGoogle Cloudストレージソース接続を作成する方法を説明します。
translation-type: tm+mt
source-git-commit: f6a63ca1e21b3c3f6a55574f31fdf04038b7e5c4
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 8%

---


# UIに[!DNL Google Cloud Storage]ソース接続を作成する

Adobe Experience Platformのソースコネクタは、外部ソースのデータをスケジュールに基づいて取り込む機能を提供します。 このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL Google Cloud Storage] （以下「GCS」と呼びます）ソースコネクタを作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なGCS接続がある場合は、このドキュメントの残りの部分をスキップして、[データフローの設定](../../dataflow/batch/cloud-storage.md)のチュートリアルに進むことができます。

### サポートされているファイル形式

[!DNL Experience Platform] は、外部ストレージから取り込む次のファイル形式をサポートしています。

* 区切り文字区切り値(DSV):DSV形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 DSV形式のファイル内のフィールド・ヘッダーの値は、英数字とアンダースコアのみで構成する必要があります。 一般的なDSVファイルは、今後サポートされる予定です。
* JavaScript Object Notation (JSON):JSON形式のデータファイルは、XDMに準拠している必要があります。
* Apacheパーケット：パーケット形式のデータファイルは、XDMに準拠している必要があります。

### 必要な資格情報の収集

[!DNL Platform]上のGCSデータにアクセスするには、次の値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| アクセスキーID | プラットフォームに対する[!DNL Google Cloud Storage]アカウントの認証に使用する61文字の英数字の文字列。 |
| 秘密アクセスキー | [!DNL Google Cloud Storage]アカウントをプラットフォームに対して認証するために使用する、40文字のベース64エンコードされた文字列。 |

これらの値について詳しくは、[Google CloudストレージのHMACキー](https://cloud.google.com/storage/docs/authentication/hmackeys#overview)ガイドを参照してください。 独自のアクセスキーIDとシークレットアクセスキーを生成する手順については、[[!DNL Google Cloud Storage] 概要](../../../../connectors/cloud-storage/google-cloud-storage.md)を参照してください。

## [!DNL Google Cloud Storage]アカウントに接続

必要な資格情報を収集したら、次の手順に従ってGCSアカウントを[!DNL Platform]にリンクします。

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択して&#x200B;**[!UICONTROL ソース]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には様々なソースが表示され、このソースを使用してアカウントを作成できます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、使用する特定のソースを見つけることもできます。

「**[!UICONTROL Databases]**」カテゴリで、「**[!UICONTROL Google Cloudストレージ]**」を選択します。 このコネクタを初めて使用する場合は、**[!UICONTROL 設定]**&#x200B;を選択します。 それ以外の場合は、**[!UICONTROL 追加data]**&#x200B;を選択して新しいGCSコネクタを作成します。

![カタログ](../../../../images/tutorials/create/google-cloud-storage/catalog.png)

**[!UICONTROL Google Cloudストレージに接続]**&#x200B;ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

### 新しいアカウント

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、名前、オプションの説明およびGCS秘密鍵証明書を入力します。 終了したら、[**[!UICONTROL 接続]**]を選択し、新しい接続が確立されるまでの時間を許可します。

![connect](../../../../images/tutorials/create/google-cloud-storage/connect.png)

### 既存のアカウント

既存のアカウントに接続するには、接続するGCSアカウントを選択し、「**[!UICONTROL 次へ]**」を選択して次に進みます。

![既存の](../../../../images/tutorials/create/google-cloud-storage/existing.png)

## 次の手順

このチュートリアルに従って、GCSアカウントへの接続を確立しました。 次のチュートリアルに進み、[データフローを設定して、クラウドストレージのデータを [!DNL Platform]](../../dataflow/batch/cloud-storage.md)に取り込むことができます。