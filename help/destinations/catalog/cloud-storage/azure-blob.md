---
keywords: Azure Blob;Blob宛先；s3;azure BLOB宛先
title: Azure Blob接続
description: Azure Blobストレージへのライブ送信接続を作成して、タブ区切りデータファイルまたはCSVデータファイルをAdobe Experience Platformから定期的にエクスポートします。
translation-type: tm+mt
source-git-commit: 7d579d85d427c45f39d000288ed883c7ffd003bf
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 7%

---


# [!DNL Azure Blob] connection

[!DNL Azure Blob] (以下「[!DNL Blob]」と呼ばれる)は、Microsoftが提供するクラウド向けのオブジェクトストレージソリューションです。このチュートリアルでは、[!DNL Platform]ユーザーインターフェイスを使用して[!DNL Blob]宛先を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

- [[!DNL Experience Data Model (XDM)] システム](../../../xdm/home.md):Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   - [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   - [スキーマエディタのチュートリアル](../../../xdm/tutorials/create-schema-ui.md):スキーマエディターのUIを使用してカスタムスキーマを作成する方法を説明します。
- [[!DNL Real-time Customer Profile]](../../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

既に有効なBlobの宛先がある場合は、このドキュメントの残りの部分をスキップして、[宛先](../../ui/activate-destinations.md)へのセグメントのアクティブ化に関するチュートリアルに進むことができます。

## サポートされているファイル形式 {#file-formats}

[!DNL Experience Platform] は、次の書き出し先のファイル形式をサポートしてい [!DNL Blob]ます。

- 区切り文字区切り値(DSV):DSV形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 一般的なDSVファイルは、今後サポートされる予定です。 サポートされるファイルの詳細については、[アクティベート先](../../ui/activate-destinations.md#esp-and-cloud-storage)のチュートリアルのクラウドストレージの節を参照してください。

## BLOBアカウントに接続{#connect-destination}

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL 宛先]**」を選択して、**[!UICONTROL 宛先]**&#x200B;ワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、アカウントを作成するための様々なリンク先が表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索オプションを使用して、操作対象の特定の宛先を見つけることもできます。

**[!UICONTROL クラウドストレージ]**&#x200B;カテゴリの下で、**[!UICONTROL Azure Blobストレージ]**&#x200B;を選択し、**[!UICONTROL 構成]**&#x200B;を選択します。

![カタログ](../../assets/catalog/cloud-storage/blob/catalog.png)

>[!NOTE]
>
>この宛先との接続が既に存在する場合は、宛先カードに「**[!UICONTROL Activate]**」ボタンが表示されます。 「**[!UICONTROL アクティブ化]**」と「**[!UICONTROL 設定]**」の違いについて詳しくは、保存先のワークスペースドキュメントの「[カタログ](../../ui/destinations-workspace.md#catalog)」の節を参照してください。

**[!UICONTROL Azure Blobストレージ]**&#x200B;に接続ページが表示されます。 このページでは、新しい秘密鍵証明書または既存の秘密鍵証明書を使用できます。

## 新しいアカウント{#new-account}

新しい資格情報を使用する場合は、「**[!UICONTROL 新しいアカウント]**」を選択します。 表示される入力フォームで、接続文字列を指定します。 接続文字列は、Blobストレージのデータにアクセスするために必要です。 [!DNL Blob]接続文字列のパターン開始:`DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`.

[!DNL Blob]接続文字列の構成の詳細については、Microsoftのドキュメントの[Azureストレージアカウントの接続文字列を構成する](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string#configure-a-connection-string-for-an-azure-storage-account)を参照してください。

必要に応じて、RSA形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 この公開鍵&#x200B;**は、Base64エンコードされた文字列として書かれる必要があります。**

![新しいアカウント](../../assets/catalog/cloud-storage/blob/new.png)

## 既存のアカウント{#existing-account}

既存のアカウントに接続するには、接続する[!DNL Blob]アカウントを選択し、**次へ**&#x200B;を選択して次に進みます。

![既存のアカウント](../../assets/catalog/cloud-storage/blob/existing.png)

## 認証 {#authentication}

**認証**&#x200B;ページが表示されます。 表示される入力フォームで、ファイルの名前、オプションの説明、フォルダーパス、コンテナーを入力します。

この手順では、この宛先に適用する&#x200B;**[!UICONTROL マーケティングアクション]**&#x200B;を選択することもできます。 マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。 Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

終了したら、「**[!UICONTROL 宛先を作成]**」を選択します。

![認証](../../assets/catalog/cloud-storage/blob/authentication.png)

## 次の手順 {#activate-segments}

このチュートリアルに従うと、[!DNL Blob]アカウントへの接続が確立されます。 次のチュートリアルに進み、[目的の](../../ui/activate-destinations.md)にセグメントをアクティブにすることができます。
