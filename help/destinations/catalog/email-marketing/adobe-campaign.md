---
keywords: メール；メール；メール；メールの宛先；adobe campaign;campaign
title: Adobe Campaign 接続
description: Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。
exl-id: 0de91738-8f56-41f5-8745-9b14b15db76a
source-git-commit: c35b43654d31f0f112258e577a1bb95e72f0a971
workflow-type: tm+mt
source-wordcount: '906'
ht-degree: 38%

---

# Adobe Campaign 接続

## 概要 {#overview}

Adobe Campaign は、オンラインおよびオフラインのすべてのチャネルにまたがるキャンペーンをカスタマイズし、実施するのに役立つソリューションセットです。詳しくは [Campaign Classicの基本を学ぶ ](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/starting-with-adobe-campaign/about-adobe-campaign-classic.html) を参照してください。

オーディエンスデータをAdobe Campaignに送信するには、まずAdobe Experience Platformで [ 宛先に接続 ](#connect-destination) してから、ストレージの場所からAdobe Campaignに [ データの読み込みを設定 ](#import-data-into-campaign) する必要があります。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| オーディエンスオリジン | サポートあり | 説明 |
| ---------|----------|----------|
| [!DNL Segmentation Service] | ✓ | Experience Platform[ セグメント化サービス ](../../../segmentation/home.md) を通じて生成されたオーディエンス。 |
| カスタムアップロード | ✓ | CSV ファイルから Experience Platform に[読み込まれた](../../../segmentation/ui/audience-portal.md#import-audience)オーディエンス。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性選択画面で選択した目的のスキーマフィールド（例：メールアドレス、電話番号、姓）と共に、セグメントのすべてのメンバーを書き出します。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## IP アドレスの許可リスト {#allow-list}

SFTP ストレージを使用してメールマーケティングの宛先を設定する場合は、特定の IP 範囲をAdobe許可リストに加えるに追加することをお勧めします。

AdobeIP を許可リストに追加する必要がある場合は ](../cloud-storage/ip-address-allow-list.md)[IP アドレスの SFTP 宛先の許可リスト」を参照してください。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[ アクセス制御の概要 ](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。

Adobe Campaignは、次の接続タイプをサポートしています。

* **[!UICONTROL Amazon S3]**
* **[!UICONTROL パスワードを使用した SFTP]**
* **[!UICONTROL SSH キーを使用した SFTP]**
* **[!UICONTROL Azure Blob]**

Adobe Campaignにデータを送信する場合は、[!DNL Amazon S3] または [!DNL Azure Blob] を使用することをお勧めします。

### 接続パラメーター {#parameters}

この宛先を[設定](../../ui/connect-destination.md)するとき、次の情報を指定する必要があります。

* **[!UICONTROL Amazon S3]** 接続の場合は、[!UICONTROL  アクセスキー ID] および [!UICONTROL  シークレットアクセスキー ] を指定する必要があります。
* **[!UICONTROL パスワード付き SFTP]** 接続の場合、[!UICONTROL  ドメイン ]、[!UICONTROL  ポート ]、[!UICONTROL  ユーザー名 ]、[!UICONTROL  パスワード ] を指定する必要があります。
* SSH キーを使用した **[!UICONTROL SFTP]** 接続の場合は、[!UICONTROL  ドメイン ]、[!UICONTROL  ポート ]、[!UICONTROL  ユーザー名 ]、[!UICONTROL SSH キー ] を指定する必要があります。
* **[!UICONTROL Azure Blob]** 接続の場合は、接続文字列を指定する必要があります。
* 必要に応じて、RSA 形式の公開鍵を添付して、「**[!UICONTROL キー]**」セクションで書き出したファイルに PGP/GPG による暗号化を追加できます。 公開鍵は、[!DNL Base64] でエンコードされた文字列として記述する必要があります。
* **[!UICONTROL 名前]**：宛先の名前を選択します。
* **[!UICONTROL 説明]**：宛先の説明を入力します。
* **[!UICONTROL バケット名]**：*S3 接続用*。書き出しデータを CSV ファイルとして保存 [!DNL Platform] る S3 バケットの場所を入力します。
* **[!UICONTROL フォルダーパス]**：書き出しデータを CSV ファイルとして保存 [!DNL Platform] るストレージの場所のパスを指定します。
* **[!UICONTROL コンテナ]**: *Blob 接続用*。 フォルダーパスが存在する Blob を保持するコンテナ。
* **[!UICONTROL ファイル形式]**: **CSV** を選択して、CSV ファイルをストレージの場所に書き出します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>* データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。
>* *ID* を書き出すには、**[!UICONTROL ID グラフの表示]**[ アクセス制御権限 ](/help/access-control/home.md#permissions) が必要です。<br> ![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}


この宛先に対してオーディエンスをアクティブ化する手順については、[ プロファイル書き出しのバッチ宛先に対するオーディエンスデータのアクティブ化 ](../../ui/activate-batch-profile-destinations.md) を参照してください。

### 宛先属性 {#destination-attributes}

この宛先に対してオーディエンスをアクティブ化する場合、Adobeでは [ 和集合スキーマ ](../../../profile/home.md#profile-fragments-and-union-schemas) から一意の ID を選択することをお勧めします。 宛先に書き出す一意の ID およびその他の XDM フィールドを選択します。詳しくは、[ メールマーケティングの宛先に対してオーディエンスをアクティブ化する際のベストプラクティス ](overview.md#best-practices) を参照してください。

## 書き出したデータ {#exported-data}

[!DNL Adobe Campaign] 宛先の場合、[!DNL Platform] は、指定されたストレージの場所に `.csv` ファイルを作成します。ファイルについて詳しくは、Audience Activation チュートリアルの [Audience Activation の検証 ](../../ui/activate-batch-profile-destinations.md#verify) の節を参照してください。

## Adobe Campaignへのデータの読み込みの設定 {#import-data-into-campaign}

>[!IMPORTANT]
>
>* この統合を実行する際は、Adobe Campaignの契約に従って、[!DNL SFTP] のストレージ制限、データベースストレージ制限、アクティブなプロファイル制限に注意してください。
>* [!DNL Campaign] ワークフローを使用して、書き出されたセグメントをAdobe Campaignでスケジュール、読み込み、マッピングする必要があります。 Adobe Campaign Classic ドキュメントの [ 繰り返し読み込みの設定 ](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/use-cases/data-management/recurring-import-workflow.html?lang=ja) およびAdobe Campaign Standard ドキュメントの [ データ管理アクティビティについて ](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/data-management-activities/about-data-management-activities.html) を参照してください。
>* Adobe Campaignにデータを送信する場合は、[!DNL Amazon S3] または [!DNL Azure Blob] を使用することをお勧めします。

[!DNL Platform] を [!DNL Amazon S3] または [!DNL Azure Blob] ストレージに接続した後、ストレージの場所からAdobe Campaignへのデータの読み込みを設定する必要があります。 これを実現する方法については、次のAdobe Campaign ドキュメントページを参照してください。
* [ データのインポートとエクスポートの概要 ](https://experienceleague.adobe.com/docs/campaign-classic/using/getting-started/importing-and-exporting-data/get-started-data-import-export.html?lang=ja) および [ データの読み込み（ファイル） ](https://experienceleague.adobe.com/docs/campaign-classic/using/automating-with-workflows/action-activities/data-loading--file-.html?lang=ja) （Adobe Campaign Classic ドキュメント）。
* [ プロセスとデータ管理の基本を学ぶ ](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/get-started-workflows.html) および [ ファイルを読み込む ](https://experienceleague.adobe.com/docs/campaign-standard/using/managing-processes-and-data/data-management-activities/load-file.html)Adobe Campaign Standard ドキュメント。
