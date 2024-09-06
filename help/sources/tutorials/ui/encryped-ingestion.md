---
title: ソース UI のWorkspaceでの暗号化されたデータの取り込み
description: ソース UI ワークスペースで暗号化されたデータを取り込む方法を説明します。
badge: ベータ版
hide: true
hidefromtoc: true
exl-id: 34aaf9b6-5c39-404b-a70a-5553a4db9cdb
source-git-commit: b4545943abbb68d36a64935feb4466d075331504
workflow-type: tm+mt
source-wordcount: '617'
ht-degree: 18%

---

# ソース UI での暗号化されたデータの取り込み

>[!AVAILABILITY]
>
>ソース UI での暗号化されたデータ取り込みのサポートはベータ版であり、組織では使用できない場合があります。 機能とドキュメントは変更される場合があります。

クラウドストレージバッチソースを使用して、暗号化されたデータファイルとフォルダーをAdobe Experience Platformに取り込むことができます。 暗号化されたデータの取り込みでは、非対称暗号化メカニズムを利用して、バッチデータを Experience Platform に安全に転送できます。 現在のところ、サポートされている非対称暗号化メカニズムは PGP と GPG です。

この機能は、次のソースで使用できます。

* [Amazon S3]
* [Azure Blob]
* [Azure Data Lake Storage Gen2]
* [Azure ファイルストレージ ]
* [Data Landing Zone]
* [FTP]
* [Google Cloud Storage]
* [HDFS]
* [Oracle オブジェクト ストレージ ]
* [SFTP]

このガイドでは、UI を使用してクラウドストレージバッチソースで暗号化されたデータを取り込む方法について説明します。

## 基本を学ぶ

UI で暗号化されたデータの取り込みを使用する前に、次のExperience Platformの機能と概念を理解しておくと役に立ちます。

* [ ソース ](../../home.md):Experience Platformのソースを使用して、Adobeアプリケーションまたはサードパーティのデータソースからデータを取り込みます。
* [ データフロー ](../../../dataflows/home.md)：データフローは、Experience Platform間でデータを移動するデータジョブを表します。 ソースワークスペースを使用すると、特定のソースからExperience Platformにデータを取り込むデータフローを作成できます。
* [ サンドボックス ](../../../sandboxes/home.md):Experience Platform内のサンドボックスを使用して、開発インスタンス間に仮想パーティションを作成し、Experience Platformまたは実稼動専用の環境を作成します。

### 概要の概要

1. Experience PlatformUI のソースワークスペースを使用して、暗号化キーペアを作成します。 オプションで、署名検証キーペアを作成して、暗号化されたデータに追加のセキュリティレイヤーを提供することもできます。
2. 公開鍵を使用してデータを暗号化します。
3. 暗号化されたデータをクラウドストレージプロバイダーに配置します。 この手順では、ソースデータをエクスペリエンスデータモデル（XDM）スキーマにマッピングするための参照として使用できるサンプルファイルが用意されていることも確認する必要があります。
4. ソース接続を作成して、暗号化されたデータをExperience Platformに取り込みます。
5. ソース接続を作成する際に、データの暗号化に使用した公開鍵に対応するキー ID を指定します。 署名検証キーペアのメカニズムも使用した場合は、暗号化されたデータに対応する署名検証キー ID を指定する必要があります。
6. データフロー作成手順に進みます。

## 暗号化キーペアの作成 {#create-an-encryption-key-pair}

>[!CONTEXTUALHELP]
>id="platform_sources_encrypted_encryptionKeyId"
>title="暗号化キー ID"
>abstract="ソースデータの暗号化に使用した暗号化キーに対応する暗号化キー ID を指定します。"

* Platform UI でソースワークスペースに移動し、上部のヘッダーから [!UICONTROL  キーペア ] を選択します。
* 組織内の既存の暗号化キーペアのリストを表示するページが表示されます。 このページでは、特定のキーのタイトル、ID、タイプ、暗号化アルゴリズム、有効期限およびステータスに関する情報が提供されます。 新しいキーペアを作成するには、「**[!UICONTROL キーを作成]**」を選択します。
* 次に、作成するキーの種類を選択します。 暗号化キーを作成するには、「**[!UICONTROL 暗号化キー]**」を選択し、暗号化キーのタイトルとパスフレーズを入力します。 パスフレーズは、暗号化キーを保護する追加のレイヤーです。 作成時に、Experience Platform は、公開鍵とは別のセキュアなコンテナにパスフレーズを保存します。 空白以外の文字列をパスフレーズとして指定する必要があります。

### 署名検証キーの作成 {#create-a-sign-verification-key}

>[!CONTEXTUALHELP]
>id="platform_sources_encrypted_signVerificationKeyId"
>title="署名検証キー ID"
>abstract="署名済みの暗号化されたソースデータに対応する署名検証キー ID を指定します。"

## 暗号化されたデータの取り込み {#ingest-encrypted-data}

>[!CONTEXTUALHELP]
>id="platform_sources_encrypted_isFileEncrypted"
>title="ファイルは暗号化されていますか？"
>abstract="既に暗号化されているファイルを取り込む場合は、この切替スイッチを選択します。"

>[!CONTEXTUALHELP]
>id="platform_sources_encrypted_sampleFile"
>title="サンプルファイルの選択"
>abstract="マッピングを作成するには、暗号化されたデータを取り込む際にサンプルファイルを取り込む必要があります。"


<!-- 
## Outline

Sections:

* Create public key
* Create customer key
* Create sources flow to ingest encrypted data
  * File ingestion
  * Folder ingestion
* Updated encrypted flow

* Select [!UICONTROL Key Pairs] from the header in the sources UI workspace.
  * You are taken to the [!UICONTROL Key Pairs] page:
    * Select **[!UICONTROL Encryption key]** for list of key pairs that you have created and managed.
    * Select **[!UICONTROL Customer key]** for a list of key pairs that your customers have created and managed.
* Key Pair functions:
  * Select **[!UICONTROL Key details]** to view key details.
  * Select **[!UICONTROL Delete]** to delete.
* Select [!UICONTROL Create key] to create either an encryption key or a customer key

## Questions and clarifications

* Public key vs. customer key
* Verify E2E:
  * Create keys (encryption key or customer key)
  * Use these keys to encrypt your data
  * Place your encrypted data in your cloud storage (Amazon S3 or Google Cloud Storage)
  * Ingest that encrypted data to Experience Platform by creating a source connection
    * Select the encrypted source data
    * Enable "Is the file encrypted"
    * Select/upload sample file for mapping
    * Use the encryption key name that corresponds with the key used to encrypt the source data
      * If the data was encrypted using customer key, provide the sign verification key.
  * Proceed with source connection creation flow -->
