---
keywords: E メール；E メール；E メールの宛先；Adobe Campaign;Campaign
title: Adobe Campaign接続
description: Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。
exl-id: 0de91738-8f56-41f5-8745-9b14b15db76a
source-git-commit: 3aac1e7c7fe838201368379da8504efc8e316e1c
workflow-type: tm+mt
source-wordcount: '722'
ht-degree: 21%

---

# Adobe Campaign接続

## 概要 {#overview}

Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。詳しくは、[Campaign Classic](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/starting-with-adobe-campaign/about-adobe-campaign-classic.html?lang=ja) の使用を開始するを参照してください。

セグメントデータをAdobe Campaignに送信するには、まずAdobe Experience Platformで宛先 ](#connect-destination) に接続し、次にストレージの場所からAdobe Campaignに [ データインポート ](#import-data-into-campaign) を設定する必要があります。[

## 書き出しタイプ {#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：電子メールアドレス、電話番号、姓 )。オーディエンスのアクティベーションワ **[!UICONTROL ークフ]** ローの「属性を選 [択」手順で選択します](../../ui/activate-batch-profile-destinations.md#select-attributes)。

## IP アドレス許可リスト {#allow-list}

SFTP ストレージを使用した電子メールマーケティングの宛先を設定する場合、Adobeでは、特定の IP 範囲を許可リストに追加することをお勧めします。

許可リストにAdobeIP を追加する必要がある場合は、[ クラウドストレージの宛先 ](../cloud-storage/ip-address-allow-list.md) の IP アドレス許可リストを参照してください。

## 宛先に接続 {#connect}

この宛先に接続するには、[ 宛先の設定に関するチュートリアル ](../../ui/connect-destination.md) で説明されている手順に従います。

Adobe Campaignは、次の接続タイプをサポートしています。

* **[!UICONTROL Amazon S3]**
* **[!UICONTROL SFTP（パスワード）]**
* **[!UICONTROL SFTP（SSH キー）]**
* **[!UICONTROL Azure BLOB]**

Adobe Campaignにデータを送信する方法は、[!DNL Amazon S3] または [!DNL Azure Blob] を使用することをお勧めします。

### 接続パラメーター {#parameters}

[ この宛先を設定 ](../../ui/connect-destination.md) する際は、次の情報を指定する必要があります。

* **[!UICONTROL Amazon S3]** 接続の場合、[!UICONTROL  アクセスキー ID] と [!UICONTROL  シークレットアクセスキー ] を指定する必要があります。
* **[!UICONTROL パスワード]** を使用した SFTP 接続の場合、[!UICONTROL  ドメイン ]、[!UICONTROL  ポート ]、[!UICONTROL  ユーザー名 ]、[!UICONTROL  パスワード ] を指定する必要があります。
* **[!UICONTROL SFTP（SSH キー]** 接続）の場合は、[!UICONTROL  ドメイン ]、[!UICONTROL  ポート ]、[!UICONTROL  ユーザー名 ]、および [!UICONTROL SSH キー ] を指定する必要があります。
* **[!UICONTROL Azure Blob]** 接続の場合は、接続文字列を指定する必要があります。
* 必要に応じて、RSA 形式の公開鍵を添付し、**[!UICONTROL Key]** セクションで、PGP/GPG を使用した暗号化を書き出したファイルに追加できます。 公開鍵は、[!DNL Base64] エンコードされた文字列として書き込む必要があります。
* **[!UICONTROL 名前]**：宛先の名前を選択します。
* **[!UICONTROL 説明]**：宛先の説明を入力します。
* **[!UICONTROL バケット名]**：*S3 接続用*。[!DNL Platform] が書き出しデータを CSV またはタブ区切りファイルとして格納する S3 バケットの場所を入力します。
* **[!UICONTROL フォルダーパス]**[!DNL Platform]： が書き出しデータを CSV またはタブ区切りファイルとして格納するストレージの場所へのパスを指定します。
* **[!UICONTROL コンテナ]**: *BLOB 接続の場合*。フォルダーパスが含まれる BLOB を格納するコンテナです。
* **[!UICONTROL ファイル形式]**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。

## この宛先へのセグメントのアクティブ化 {#activate}

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、[ プロファイルの一括書き出し先へのオーディエンスデータのアクティブ化 ](../../ui/activate-batch-profile-destinations.md) を参照してください。

### 宛先属性 {#destination-attributes}

この宛先に対してセグメントをアクティブ化する場合は、[ 和集合スキーマ ](../../../profile/home.md#profile-fragments-and-union-schemas) から一意の識別子を選択することをAdobeにお勧めします。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、電子メールマーケティングの宛先にオーディエンスをアクティブ化する際のベストプラクティス [ を参照してください。](overview.md#best-practices)

## エクスポートされたデータ {#exported-data}

[!DNL Adobe Campaign] の宛先の場合、[!DNL Platform] は指定したストレージの場所にタブ区切りの `.csv` ファイルを作成します。 ファイルの詳細については、『セグメントのアクティベーションのチュートリアル』の [ セグメントのアクティベーションの検証 ](../../ui/activate-batch-profile-destinations.md#verify) を参照してください。

## Adobe Campaign へのデータインポートの設定 {#import-data-into-campaign}

>[!IMPORTANT]
>
>* この統合を実行する際は、Adobe Campaign契約に従って、ストレージの制限 [!DNL SFTP]、データベースのストレージの制限、アクティブなプロファイルの制限に注意してください。
>* [!DNL Campaign] ワークフローを使用して、Adobe Campaignで書き出したセグメントのスケジュール、読み込み、マッピングをおこなう必要があります。 Adobe Campaign Classicのドキュメントの「[ 繰り返しインポートの設定 ](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/recurring-import-workflow.html?lang=ja)」およびAdobe Campaign Standardのドキュメントの「[ データ管理アクティビティについて ](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/data-management-activities/about-data-management-activities.html)」を参照してください。
>* Adobe Campaignにデータを送信する方法は、[!DNL Amazon S3] または [!DNL Azure Blob] を使用することをお勧めします。


[!DNL Platform] を [!DNL Amazon S3] または [!DNL Azure Blob] ストレージに接続した後、ストレージの場所からAdobe Campaignへのデータインポートを設定する必要があります。 これをおこなう方法については、次のAdobe Campaignドキュメントページを参照してください。
* [Adobe Campaign Classicのドキュメントで、データの読み込](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/importing-and-exporting-data/get-started-data-import-export.html?lang=ja) みと書 [き出し（ファイル）](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/action-activities/data-loading--file-.html) を使い始めます。
* [Adobe Campaign Standardのドキュメントで、プロセスとデータ管](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/get-started-workflows.html) 理の概 [要と](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/data-management-activities/load-file.html) ファイルの読み込みを参照してください。
