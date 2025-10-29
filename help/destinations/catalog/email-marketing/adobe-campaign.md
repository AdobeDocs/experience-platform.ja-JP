---
keywords: メール；メール；メール；メールの宛先；adobe campaign;campaign
title: Adobe Campaign 接続
description: Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。
exl-id: 0de91738-8f56-41f5-8745-9b14b15db76a
source-git-commit: 1b507e9846a74b7ac2d046c89fd7c27a818035ba
workflow-type: tm+mt
source-wordcount: '841'
ht-degree: 35%

---

# Adobe Campaign 接続

## 概要 {#overview}

Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。詳しくは、[Campaign Classicの基本を学ぶ &#x200B;](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/starting-with-adobe-campaign/about-adobe-campaign-classic.html) を参照してください。

オーディエンスデータをAdobe Campaignに送信するには、まずAdobe Experience Platformで [&#x200B; 宛先に接続 &#x200B;](#connect-destination) してから、ストレージの場所からAdobe Campaignに [&#x200B; データの読み込みを設定 &#x200B;](#import-data-into-campaign) する必要があります。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
| ---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform [&#x200B; セグメント化サービス &#x200B;](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
|---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL Profile-based]** | [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL Batch]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## IP アドレスの許可リスト {#allow-list}

SFTP ストレージを使用してメールマーケティングの宛先を設定する場合、Adobeでは、特定の IP 範囲を許可リストに追加することをお勧めします。

許可リストにAdobe IP を追加する必要がある場合は [&#128279;](../cloud-storage/ip-address-allow-list.md)IP アドレス^SFTP 宛先の許可リスト^を参照してください。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL Manage Destinations]** アクセス制御権限 [&#x200B; が必要 &#x200B;](/help/access-control/home.md#permissions) す。 詳しくは、[&#x200B; アクセス制御の概要 &#x200B;](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

Adobe Campaignは、次の接続タイプをサポートしています。

* **[!UICONTROL Amazon S3]**
* **[!UICONTROL SFTP with Password]**
* **[!UICONTROL SFTP with SSH Key]**
* **[!UICONTROL Azure Blob]**

Adobe Campaignにデータを送信する場合は、[!DNL Amazon S3] または [!DNL Azure Blob] を使用することをお勧めします。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Amazon S3]** 接続の場合、[!UICONTROL Access Key ID] と [!UICONTROL Secret Access Key] を指定する必要があります。
* **[!UICONTROL SFTP with Password]** 接続の場合、[!UICONTROL Domain]、[!UICONTROL Port]、[!UICONTROL Username] および [!UICONTROL Password] を指定する必要があります。
* **[!UICONTROL SFTP with SSH Key]** 接続の場合、[!UICONTROL Domain]、[!UICONTROL Port]、[!UICONTROL Username] および [!UICONTROL SSH Key] を指定する必要があります。
* **[!UICONTROL Azure Blob]** 接続の場合は、接続文字列を指定する必要があります。
* 必要に応じて、RSA 形式の公開鍵を添付して、**[!UICONTROL Key]** セクションで書き出したファイルに PGP/GPG による暗号化を追加できます。 公開鍵は、[!DNL Base64] でエンコードされた文字列として記述する必要があります。
* **[!UICONTROL Name]**：宛先に関連する名前を選択します。
* **[!UICONTROL Description]**：宛先の説明を入力します。
* **[!UICONTROL Bucket Name]**: *S3 接続用*。 書き出しデータを CSV ファイルとして保存 [!DNL Experience Platform] る S3 バケットの場所を入力します。
* **[!UICONTROL Folder Path]**：書き出しデータを CSV ファイルとして保存 [!DNL Experience Platform] るストレージの場所のパスを指定します。
* **[!UICONTROL Container]**: *Blob 接続用*。 フォルダーパスが存在する Blob を保持するコンテナ。
* **[!UICONTROL File Format]**: **CSV** を選択して、CSV ファイルをストレージの場所に書き出します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL Next]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]**、**[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}


この宛先に対してオーディエンスをアクティブ化する手順については、[&#x200B; プロファイル書き出しのバッチ宛先に対するオーディエンスデータのアクティブ化 &#x200B;](../../ui/activate-batch-profile-destinations.md) を参照してください。

### 宛先属性 {#destination-attributes}

この宛先に対してオーディエンスをアクティブ化する場合、Adobeでは [&#x200B; 和集合スキーマ &#x200B;](../../../profile/home.md#profile-fragments-and-union-schemas) から一意の ID を選択することをお勧めします。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、[&#x200B; メールマーケティングの宛先に対してオーディエンスをアクティブ化する際のベストプラクティス &#x200B;](overview.md#best-practices) を参照してください。

## 書き出したデータ {#exported-data}

[!DNL Adobe Campaign] 宛先の場合、[!DNL Experience Platform] は、指定されたストレージの場所に `.csv` ファイルを作成します。ファイルについて詳しくは、Audience Activation チュートリアルの [Audience Activation の検証 &#x200B;](../../ui/activate-batch-profile-destinations.md#verify) の節を参照してください。

## Adobe Campaignへのデータの読み込みの設定 {#import-data-into-campaign}

>[!IMPORTANT]
>
>* この統合を実行する際は、Adobe Campaignの契約に従って、[!DNL SFTP] のストレージ制限、データベースストレージ制限、アクティブなプロファイル制限に注意してください。
>* [!DNL Campaign] ワークフローを使用して、書き出されたセグメントをAdobe Campaignでスケジュール、読み込み、マッピングする必要があります。 Adobe Campaign Classic ドキュメントの [&#x200B; 繰り返し読み込みの設定 &#x200B;](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/recurring-import-workflow.html?lang=ja) およびAdobe Campaign Standard ドキュメントの [&#x200B; データ管理アクティビティについて &#x200B;](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/data-management-activities/about-data-management-activities.html?lang=ja) を参照してください。
>* Adobe Campaignにデータを送信する場合は、[!DNL Amazon S3] または [!DNL Azure Blob] を使用することをお勧めします。

[!DNL Experience Platform] を [!DNL Amazon S3] または [!DNL Azure Blob] ストレージに接続した後、ストレージの場所からAdobe Campaignへのデータの読み込みを設定する必要があります。 これを実現する方法については、次のAdobe Campaign ドキュメントページを参照してください。

* [&#x200B; データのインポートとエクスポートの概要 &#x200B;](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/importing-and-exporting-data/get-started-data-import-export.html?lang=ja) および [&#x200B; データの読み込み（ファイル） &#x200B;](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/action-activities/data-loading--file-.html?lang=ja) （Adobe Campaign Classic ドキュメント）。
* [&#x200B; プロセスとデータ管理の基本を学ぶ &#x200B;](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/get-started-workflows.html?lang=ja) および [&#x200B; ファイルを読み込む &#x200B;](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/data-management-activities/load-file.html?lang=ja)Adobe Campaign Standard ドキュメント。
