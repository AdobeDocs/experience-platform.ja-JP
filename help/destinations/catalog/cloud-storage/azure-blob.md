---
keywords: Azure Blob;BLOB 宛先；s3;azure BLOB 宛先
title: Azure BLOB 接続
description: Azure Blob ストレージへのライブアウトバウンド接続を作成し、CSV データファイルを定期的にAdobe Experience Platformから書き出します。
exl-id: 8099849b-e3d2-48a5-902a-ca5a5ec88207
source-git-commit: b4810dfef7b0d437744ca14a32bd4f5746e8d002
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 14%

---

# [!DNL Azure Blob] 接続

## 概要 {#overview}

[!DNL Azure Blob] ( 以下「 [!DNL Blob]) は、Microsoftのクラウド用オブジェクトストレージソリューションです。 このチュートリアルでは、 [!DNL Blob] 宛先を [!DNL Platform] ユーザーインターフェイス。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)] システム](../../../xdm/home.md):Experience Platformが顧客体験データを整理する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../xdm/tutorials/create-schema-ui.md):スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-time Customer Profile]](../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

既に有効な [!DNL Blob] の宛先については、このドキュメントの残りの部分をスキップし、次のチュートリアルに進んでください。 [宛先へのセグメントのアクティブ化](../../ui/activate-batch-profile-destinations.md).

## サポートされているファイル形式 {#file-formats}

[!DNL Experience Platform] は、次のファイル形式の書き出し先をサポートしています。 [!DNL Blob]:

* 区切り文字区切り値 (DSV):DSV 形式のデータ・ファイルのサポートは、現在、コンマ区切りの値に制限されています。 一般的な DSV ファイルのサポートは、今後提供される予定です。

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **[!UICONTROL 接続文字列]**:接続文字列は、BLOB ストレージのデータにアクセスするために必要です。 この [!DNL Blob] 接続文字列のパターンが次の値で始まる： `DefaultEndpointsProtocol=https;AccountName={ACCOUNT_NAME};AccountKey={ACCOUNT_KEY}`.
   * 設定に関する詳細 [!DNL Blob] 接続文字列： [Azure ストレージアカウントの接続文字列の構成](https://docs.microsoft.com/en-us/azure/storage/common/storage-configure-connection-string#configure-a-connection-string-for-an-azure-storage-account) (Microsoftドキュメント ) を参照してください。

* 必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、 [!DNL Base64] エンコードされた文字列。
* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**:書き出したファイルをホストする保存先フォルダーのパスを入力します。
* **[!UICONTROL コンテナ]**:名前を入力 [!DNL Azure Blob Storage] この宛先で使用するコンテナ。

必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、 [!DNL Base64] エンコードされた文字列。

## この宛先へのセグメントのアクティブ化 {#activate}

詳しくは、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) を参照してください。
