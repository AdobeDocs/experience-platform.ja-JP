---
keywords: 電子メール；電子メール；電子メール；電子メールの送信先；アドビのキャンペーン;キャンペーン
title: Adobe Campaign接続
description: Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。
translation-type: tm+mt
source-git-commit: 6e7ecfdc0b2cbf6f07e6b2220ec163289511375e
workflow-type: tm+mt
source-wordcount: '713'
ht-degree: 37%

---


# Adobe Campaign接続

Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。詳しくは、「[Adobe Campaign Classic について](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/starting-with-adobe-campaign/about-adobe-campaign-classic.html)」を参照してください。

セグメントデータをAdobe Campaignに送信するには、まずAdobe Experience Platformの宛先](#connect-destination)に[接続し、次に[ストレージの場所からAdobe Campaignへのデータインポート](#import-data-into-campaign)を設定する必要があります。

## エクスポートの種類{#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](../../ui/activate-destinations.md#select-attributes)。

## 宛先の接続 {#connect-destination}

**[!UICONTROL 接続]**/**[!UICONTROL 宛先]**&#x200B;で、「Adobe Campaign」を選択し、「宛先&#x200B;]**に接続」を選択します。**[!UICONTROL 

![Adobe Campaign への接続](../../assets/catalog/email-marketing/adobe-campaign/catalog.png)

「宛先の接続」ワークフローで、ストレージの場所の&#x200B;**[!UICONTROL 接続タイプ]**&#x200B;を選択します。Adobe Campaignの場合は、**[!UICONTROL AmazonS3]**、**[!UICONTROL SFTP with Password]**、**[!UICONTROL SFTP with SSH Key]**、**[!UICONTROL Azure Blob]**&#x200B;のいずれかを選択できます。 接続タイプに応じて以下の情報を入力し、「**[!UICONTROL 接続]**」を選択します。

![Campaign ウィザードの設定](../../assets/catalog/email-marketing/adobe-campaign/connection-type.png)

- **[!UICONTROL AmazonS3 接続]**&#x200B;の場合、アクセスキー ID とシークレットアクセスキーを指定する必要があります。
- **[!UICONTROL SFTP（パスワード）]** で接続する場合は、ドメイン、ポート、ユーザー名、パスワードを指定する必要があります。
- **[!UICONTROL SFTP（SSH キー）]** で接続する場合は、ドメイン、ポート、ユーザー名、SSH キーを指定する必要があります。
- **[!UICONTROL Azure Blob]**&#x200B;接続の場合は、接続文字列を指定する必要があります。

必要に応じて、RSA形式の公開鍵を添付し、PGP/GPGを使用した暗号化を&#x200B;**[!UICONTROL Key]**&#x200B;セクションのエクスポートファイルに追加することができます。 この公開鍵&#x200B;**は、Base64エンコードされた文字列として書かれる必要があります。**

![Campaign 情報の入力](../../assets/catalog/email-marketing/adobe-campaign/account-info.png)

「**[!UICONTROL 基本情報]**」で、目的の宛先に関する情報を次のように入力します。
- **[!UICONTROL 名前]**：宛先の名前を選択します。
- **[!UICONTROL 説明]**：宛先の説明を入力します。
- **[!UICONTROL バケット名]**：*S3 接続用*。S3バケットの場所を入力します。プラットフォームでエクスポートデータがCSVまたはタブ区切りファイルとしてデポジットされます。
- **[!UICONTROL Folder Path]**:ストレージー上の場所に、PlatformがエクスポートデータをCSVまたはタブ区切りファイルとしてデポジットするパスを指定します。
- **[!UICONTROL コンテナ]**: *BLOB接続の場合*。フォルダーパスが含まれるBLOBを保持するコンテナです。
- **[!UICONTROL ファイル形式]**：**CSV** または **TAB_DELIMITED**。ストレージの場所に書き出すファイル形式を選択します。
- **[!UICONTROL マーケティングアクション]**:マーケティングアクションは、データをエクスポート先にエクスポートする意図を示します。Adobe定義のマーケティングアクションから選択するか、独自のマーケティングアクションを作成することができます。 マーケティング活動の詳細については、「[Adobe Experience Platform](../../../data-governance/policies/overview.md)のデータガバナンス」ページを参照してください。 Adobe定義の個々のマーケティングアクションについて詳しくは、[データ使用ポリシーの概要](../../../data-governance/policies/overview.md)を参照してください。

![Campaign の基本情報](../../assets/catalog/email-marketing/adobe-campaign/basic-information.png)

上記のフィールドに入力した後、「**[!UICONTROL 作成]**」をクリックします。これで宛先が接続され、宛先への[セグメントをアクティブ化](../../ui/activate-destinations.md)できます。

## セグメントのアクティブ化 {#activate-segments}

セグメントのアクティベーションワークフローについて詳しくは、「[宛先へのプロファイルとセグメントのアクティブ化](../../ui/activate-destinations.md)」を参照してください。

## 宛先属性 {#destination-attributes}

Adobe Campaign の宛先に対して[セグメントをアクティブ化する](../../ui/activate-destinations.md)場合は、[ユニオンスキーマー](../../../profile/home.md#profile-fragments-and-union-schemas)から一意の ID を選択することをお勧めします。宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、電子メールマーケティング宛先ドキュメントの「書き出したファイル](./overview.md#destination-attributes)で、宛先属性として使用するスキーマフィールドの選択」を参照してください。[

## エクスポートされたデータ{#exported-data}

[!DNL Adobe Campaign]宛先の場合、Platformは、指定したストレージーの場所にタブ区切りの`.txt`ファイルまたは`.csv`ファイルを作成します。 ファイルについて詳しくは、セグメントアクティベーションチュートリアルの「電子メールマーケティングの宛先とクラウドストレージの宛先」[を参照してください。](../../ui/activate-destinations.md#esp-and-cloud-storage)

## Adobe Campaign へのデータインポートの設定 {#import-data-into-campaign}

>[!IMPORTANT]
>
>- この統合を実行する際は、Adobe Campaign契約に従って、SFTPストレージの制限、データベースストレージの制限およびアクティブなプロファイルの制限に注意してください。
>- [!DNL Campaign]ワークフローーを使用して、Adobe Campaignーでエクスポートしたセグメントのスケジュール、インポート、およびマッピングを行う必要があります。 Adobe Campaignドキュメントの[定期的なインポートの設定](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/general-operation/importing-data.html#automating-with-workflows)を参照してください。



プラットフォームを[!DNL Amazon S3]またはSFTPストレージに接続した後、ストレージの場所からAdobe Campaignへのデータインポートを設定する必要があります。 これを達成する方法については、Adobe Campaignマニュアルの[データの読み込み](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/general-operation/importing-data.html)を参照してください。