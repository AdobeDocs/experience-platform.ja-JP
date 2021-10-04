---
keywords: Azure Blob;BLOB 宛先；s3;Azure BLOB 宛先
title: Azure BLOB 接続
description: Azure BLOB ストレージへのライブアウトバウンド接続を作成して、タブ区切りのデータファイルまたは CSV データファイルをAdobe Experience Platformから定期的に書き出します。
exl-id: 8099849b-e3d2-48a5-902a-ca5a5ec88207
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '400'
ht-degree: 14%

---

# [!DNL Azure Blob] 接続

## 概要 {#overview}

[!DNL Azure Blob] ( 以下、と呼ばれま [!DNL Blob]す ) は、Microsoft が提供するクラウド向けオブジェクトストレージソリューションです。このチュートリアルでは、[!DNL Platform] ユーザーインターフェイスを使用して [!DNL Blob] の宛先を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../xdm/home.md):顧客体験データを整理する際に使用する標準化されたExperience Platformフレームワーク。
   * [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../xdm/tutorials/create-schema-ui.md):スキーマエディターの UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Blob] の宛先がある場合は、このドキュメントの残りの部分をスキップして、[ 宛先へのセグメントのアクティブ化 ](../../ui/activate-batch-profile-destinations.md) に関するチュートリアルに進んでください。

## サポートされているファイル形式 {#file-formats}

[!DNL Experience Platform] は、次のファイル形式の書き出し先をサポートしていま [!DNL Blob]す。

* 区切り文字区切り値 (DSV):DSV 形式のデータ・ファイルのサポートは、現在、コンマ区切り値に制限されています。 一般的な DSV ファイルのサポートは、今後提供される予定です。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL 接続文字列]**:BLOB ストレージ内のデータにアクセスするには、接続文字列が必要です。[!DNL Blob] 接続文字列パターンは次の文字列で始まります。`DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`.
   * [!DNL Blob] 接続文字列の設定の詳細については、Microsoft のドキュメントの「[Azure ストレージアカウントの接続文字列の設定 ](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string#configure-a-connection-string-for-an-azure-storage-account)」を参照してください。

* 必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、[!DNL Base64] エンコードされた文字列として書き込む必要があります。
* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**:書き出したファイルをホストする宛先フォルダーのパスを入力します。
* **[!UICONTROL コンテナ]**:この宛先で使用す [!DNL Azure Blob Storage] るコンテナの名前を入力します。

必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、[!DNL Base64] エンコードされた文字列として書き込む必要があります。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ プロファイルの一括書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-batch-profile-destinations.md) を参照してください。
