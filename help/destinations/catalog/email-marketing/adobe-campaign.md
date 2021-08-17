---
keywords: Eメール；Eメール；Eメール；Eメールの宛先；Adobe Campaign；キャンペーン
title: Adobe Campaign接続
description: Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。
exl-id: 0de91738-8f56-41f5-8745-9b14b15db76a
source-git-commit: 15ea3ab9370541c35b874414a8753e8812eea9c6
workflow-type: tm+mt
source-wordcount: '728'
ht-degree: 20%

---

# Adobe Campaign接続

## 概要 {#overview}

Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。詳しくは、[Campaign Classic](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/starting-with-adobe-campaign/about-adobe-campaign-classic.html)の使用を開始するを参照してください。

セグメントデータをAdobe Campaignに送信するには、まずAdobe Experience Platformで宛先](#connect-destination)に接続し、次にストレージの場所からAdobe Campaignにデータインポート](#import-data-into-campaign)を設定する必要があります。[[

## 書き出しタイプ {#export-type}

**プロファイルベースの**  — セグメントのすべてのメンバーを、目的のスキーマフィールド(例：電子メールアドレス、電話番号、姓)。宛先のアクティベーションワークフ **[!UICONTROL ロー]** の「属性を選 [択」手順で選択します](../../ui/activate-destinations.md#select-attributes)。

## IPアドレス許可リスト {#allow-list}

SFTPストレージを使用した電子メールマーケティングの宛先を設定する場合、Adobeでは、特定のIP範囲を許可リストに追加することをお勧めします。

許可リストにAdobeIPを追加する必要がある場合は、[クラウドストレージの宛先](../cloud-storage/ip-address-allow-list.md)のIPアドレス許可リストを参照してください。

## 宛先に接続 {#connect}

この宛先に接続するには、[宛先の設定に関するチュートリアル](../../ui/connect-destination.md)で説明されている手順に従います。

Adobe Campaignは、次の接続タイプをサポートしています。

* **[!UICONTROL Amazon S3]**
* **[!UICONTROL SFTP（パスワード）]**
* **[!UICONTROL SFTP（SSHキー）]**
* **[!UICONTROL Azure BLOB]**

Adobe Campaignにデータを送信する方法は、[!DNL Amazon S3]または[!DNL Azure Blob]を使用することをお勧めします。

### 接続パラメーター {#parameters}

[この宛先を設定](../../ui/connect-destination.md)する際に、次の情報を指定する必要があります。

* **[!UICONTROL Amazon S3]**&#x200B;接続の場合、[!UICONTROL アクセスキーID]と[!UICONTROL シークレットアクセスキー]を指定する必要があります。
* **[!UICONTROL SFTPとパスワード]**&#x200B;の接続の場合、[!UICONTROL ドメイン]、[!UICONTROL ポート]、[!UICONTROL ユーザー名]、[!UICONTROL パスワード]を指定する必要があります。
* **[!UICONTROL SFTP(SSHキー]**&#x200B;接続の場合、[!UICONTROL ドメイン]、[!UICONTROL ポート]、[!UICONTROL ユーザー名]、および[!UICONTROL SSHキー]を指定する必要があります。
* **[!UICONTROL Azure Blob]**&#x200B;接続の場合は、接続文字列を指定する必要があります。
* 必要に応じて、RSA形式の公開鍵を添付し、**[!UICONTROL Key]**&#x200B;セクションで、PGP/GPGを使用した暗号化を書き出したファイルに追加できます。 公開鍵は、[!DNL Base64]エンコードされた文字列として書き込む必要があります。
* **[!UICONTROL 名前]**：宛先の名前を選択します。
* **[!UICONTROL 説明]**：宛先の説明を入力します。
* **[!UICONTROL バケット名]**：*S3 接続用*。[!DNL Platform]が書き出しデータをCSVまたはタブ区切りファイルとして格納するS3バケットの場所を入力します。
* **[!UICONTROL フォルダーパス]**[!DNL Platform]： が書き出しデータを CSV またはタブ区切りファイルとして格納するストレージの場所へのパスを指定します。
* **[!UICONTROL コンテナ]**: *BLOB接続の場合*。フォルダーパスが含まれるBLOBを格納するコンテナです。
* **[!UICONTROL ファイル形式]**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。

## この宛先へのセグメントのアクティブ化 {#activate}

宛先に対するオーディエンスセグメントをアクティブ化する手順については、[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md)を参照してください。

## 宛先属性 {#destination-attributes}

[この宛先に対してセグメント](../../ui/activate-destinations.md)をアクティブ化する場合は、[和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の識別子を選択することをAdobeにお勧めします。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、[書き出したファイルの宛先属性として使用するスキーマフィールドの選択](./overview.md#destination-attributes)を参照してください。

## エクスポートされたデータ {#exported-data}

[!DNL Adobe Campaign]の宛先の場合、[!DNL Platform]は、指定したストレージの場所にタブ区切りの`.csv`ファイルを作成します。 ファイルについて詳しくは、セグメントのアクティベーションに関するチュートリアルの「電子メールマーケティングの宛先とクラウドストレージの宛先](../../ui/activate-destinations.md#esp-and-cloud-storage) 」を参照してください。[

## Adobe Campaign へのデータインポートの設定 {#import-data-into-campaign}

>[!IMPORTANT]
>
>* この統合を実行する際は、Adobe Campaign契約に従って、ストレージの制限[!DNL SFTP]、データベースのストレージの制限、アクティブなプロファイルの制限に注意してください。
>* [!DNL Campaign]ワークフローを使用して、Adobe Campaignで書き出したセグメントのスケジュール、読み込み、マッピングをおこなう必要があります。 Adobe Campaign Classicのドキュメントの「[繰り返しインポートの設定](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/recurring-import-workflow.html?lang=ja)」およびAdobe Campaign Standardのドキュメントの「[データ管理アクティビティについて](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/data-management-activities/about-data-management-activities.html)」を参照してください。
>* Adobe Campaignにデータを送信する方法は、[!DNL Amazon S3]または[!DNL Azure Blob]を使用することをお勧めします。


[!DNL Platform]を[!DNL Amazon S3]または[!DNL Azure Blob]ストレージに接続した後、ストレージの場所からAdobe Campaignへのデータインポートを設定する必要があります。 これをおこなう方法については、次のAdobe Campaignドキュメントページを参照してください。
* [Adobe Campaign Classicのドキュメントの「データのインポ](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/importing-and-exporting-data/get-started-data-import-export.html?lang=ja) ートと [エクスポート（ファイル）](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/action-activities/data-loading--file-.html) 」を参照してください。
* [Adobe Campaign Standardのドキュメントで、プロセスとデータ管](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/get-started-workflows.html) 理の概要と [フ](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/data-management-activities/load-file.html) ァイルの読み込みを始めます。
