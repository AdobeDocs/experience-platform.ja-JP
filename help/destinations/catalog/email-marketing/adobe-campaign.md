---
keywords: E メール；E メール；E メールの宛先；Adobe Campaign;Campaign
title: Adobe Campaign接続
description: Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。
exl-id: 0de91738-8f56-41f5-8745-9b14b15db76a
source-git-commit: b0d6e02c67f2a62971332acb224c7422ea467e6c
workflow-type: tm+mt
source-wordcount: '713'
ht-degree: 16%

---

# Adobe Campaign接続

## 概要 {#overview}

Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。詳しくは、 [使用の手引きCampaign Classic](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/starting-with-adobe-campaign/about-adobe-campaign-classic.html?lang=ja) を参照してください。

セグメントデータをAdobe Campaignに送信するには、まず [宛先に接続](#connect-destination) Adobe Experience Platformで [データインポートの設定](#import-data-into-campaign) ストレージの場所からAdobe Campaignへ

## 書き出しタイプ {#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：電子メールアドレス、電話番号、姓 )。 **[!UICONTROL 属性を選択]** 手順 [オーディエンスのアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes).

## IP アドレスの許可リスト {#allow-list}

SFTP ストレージを使用した電子メールマーケティングの宛先を設定する場合、Adobeでは、特定の IP 範囲を許可リストに追加することをお勧めします。

参照： [クラウドストレージの宛先の IP アドレス許可リスト](../cloud-storage/ip-address-allow-list.md) 許可リストにAdobeIP を追加する必要がある場合。

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

Adobe Campaignは、次の接続タイプをサポートしています。

* **[!UICONTROL Amazon S3]**
* **[!UICONTROL パスワード付き SFTP]**
* **[!UICONTROL SSH キーを使用した SFTP]**
* **[!UICONTROL Azure BLOB]**

データをAdobe Campaignに送信する推奨される方法は、次のとおりです [!DNL Amazon S3] または [!DNL Azure Blob].

### 接続パラメーター {#parameters}

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* の場合 **[!UICONTROL Amazon S3]** 接続、 [!UICONTROL アクセスキー ID] および [!UICONTROL 秘密アクセスキー].
* の場合 **[!UICONTROL パスワード付き SFTP]** 接続、 [!UICONTROL ドメイン], [!UICONTROL ポート], [!UICONTROL ユーザー名]、および [!UICONTROL パスワード].
* の場合 **[!UICONTROL SSH キーを使用した SFTP]** 接続、 [!UICONTROL ドメイン], [!UICONTROL ポート], [!UICONTROL ユーザー名]、および [!UICONTROL SSH キー].
* の場合 **[!UICONTROL Azure Blob]** 接続する場合は、接続文字列を指定する必要があります。
* 必要に応じて、RSA 形式の公開鍵を添付し、PGP/GPG を使用して、 **[!UICONTROL キー]** 」セクションに入力します。 公開鍵は、 [!DNL Base64] エンコードされた文字列。
* **[!UICONTROL 名前]**：宛先の名前を選択します。
* **[!UICONTROL 説明]**：宛先の説明を入力します。
* **[!UICONTROL バケット名]**：*S3 接続用*。S3 バケットの場所を入力します。ここで、 [!DNL Platform] は、書き出しデータを CSV ファイルとして保存します。
* **[!UICONTROL フォルダーパス]**:ストレージの場所に次のパスを指定します。 [!DNL Platform] は、書き出しデータを CSV ファイルとして保存します。
* **[!UICONTROL コンテナ]**: *BLOB 接続の場合*. フォルダーパスが含まれる BLOB を格納するコンテナです。
* **[!UICONTROL ファイル形式]**:選択 **CSV** を使用して、CSV ファイルをストレージの場所に書き出します。

## この宛先へのセグメントのアクティブ化 {#activate}

詳しくは、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) を参照してください。

### 宛先属性 {#destination-attributes}

この宛先に対してセグメントをアクティブ化する場合は、Adobeでは、 [和集合スキーマ](../../../profile/home.md#profile-fragments-and-union-schemas). 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、 [電子メールマーケティングの宛先に対してオーディエンスをアクティブ化する際のベストプラクティス](overview.md#best-practices).

## 書き出されたデータ {#exported-data}

の場合 [!DNL Adobe Campaign] 宛先、 [!DNL Platform] を作成 `.csv` ファイルを指定したストレージの場所に保存します。 ファイルの詳細については、 [セグメントのアクティベーションを検証](../../ui/activate-batch-profile-destinations.md#verify) （セグメントのアクティベーションに関するチュートリアル）。

## Adobe Campaign へのデータインポートの設定 {#import-data-into-campaign}

>[!IMPORTANT]
>
>* 次の点に注意してください。 [!DNL SFTP] この統合を実行する際に、Adobe Campaign契約に従って、ストレージの制限、データベースのストレージの制限、アクティブなプロファイルの制限。
>* Adobe Campaignで、 [!DNL Campaign] ワークフロー。 参照： [繰り返し発生するインポートの設定](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/recurring-import-workflow.html?lang=ja) Adobe Campaign Classicのドキュメントおよび [データ管理アクティビティについて](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/data-management-activities/about-data-management-activities.html) (Adobe Campaign Standardドキュメント )
>* データをAdobe Campaignに送信する推奨される方法は、次のとおりです [!DNL Amazon S3] または [!DNL Azure Blob].


接続後 [!DNL Platform] を [!DNL Amazon S3] または [!DNL Azure Blob] ストレージの場合は、ストレージの場所からAdobe Campaignへのデータインポートを設定する必要があります。 これをおこなう方法については、次のAdobe Campaignドキュメントページを参照してください。
* [データのインポートとエクスポートの概要](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/importing-and-exporting-data/get-started-data-import-export.html?lang=ja) および [データ読み込み（ファイル）](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/action-activities/data-loading--file-.html) (Adobe Campaign Classicドキュメント ) を参照してください。
* [プロセスとデータ管理の概要](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/get-started-workflows.html) および [ファイルを読み込み](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/data-management-activities/load-file.html) (Adobe Campaign Standardドキュメント ) を参照してください。
