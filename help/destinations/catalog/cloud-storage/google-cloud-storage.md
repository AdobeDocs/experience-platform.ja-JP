---
title: （ベータ版）Google Cloud Storage 接続
description: Google Cloud Storage に接続し、セグメントをアクティブ化する方法、またはデータセットを書き出す方法について説明します。
exl-id: ab274270-ae8c-4264-ba64-700b118e6435
source-git-commit: a07557ec398631ece0c8af6ec7b32e0e8593e24b
workflow-type: tm+mt
source-wordcount: '908'
ht-degree: 27%

---

# （ベータ版） [!DNL Google Cloud Storage] 接続

>[!IMPORTANT]
>
>この宛先は現在ベータ版で、限られた数のお客様のみが利用できます。 [!DNL Google Cloud Storage] 接続へのアクセスをリクエストするには、アドビ担当者に連絡し、[!DNL Organization ID] を提供します。

## 概要 {#overview}

へのライブアウトバウンド接続を作成する [!DNL Google Cloud Storage] を使用して、Adobe Experience Platformから独自のバケットにデータファイルを定期的に書き出します。

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、該当するスキーマフィールドと共に、 [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳細を表示 [バッチファイルベースの宛先](/help/destinations/destination-types.md#file-based). |

{style=&quot;table-layout:auto&quot;}

## 接続の前提条件の設定 [!DNL Google Cloud Storage] アカウント {#prerequisites}

Platform をに接続するため [!DNL Google Cloud Storage]を使用する場合、最初に [!DNL Google Cloud Storage] アカウント 相互運用性設定にアクセスするには、を開きます。 [!DNL Google Cloud Platform] を選択し、 **[!UICONTROL 設定]** から **[!UICONTROL クラウドストレージ]** 」オプションを使用します。

![クラウドストレージと設定がハイライトされたGoogle Cloud Platform ダッシュボード。](../../../sources/images/tutorials/create/google-cloud-storage/nav.png)

この **[!UICONTROL 設定]** ページが表示されます。 ここから、 [!DNL Google] プロジェクト ID と、 [!DNL Google Cloud Storage] アカウント 相互運用性の設定にアクセスするには、 **[!UICONTROL 相互運用性]** を上部のヘッダーから削除します。

![「 Google Cloud Platform 」ダッシュボードでハイライト表示されている「 Interoperability 」タブ。](../../../sources/images/tutorials/create/google-cloud-storage/project-access.png)

この **[!UICONTROL 相互運用性]** ページには、認証、アクセスキー、およびサービスアカウントに関連付けられたデフォルトプロジェクトに関する情報が含まれています。 サービスアカウントの新しいアクセスキー ID と秘密アクセスキーを生成するには、「 」を選択します。 **[!UICONTROL サービスアカウントのキーの作成]**.

![「 Google Cloud Platform ダッシュボードで強調表示されたサービスアカウント制御のキーを作成」。](../../../sources/images/tutorials/create/google-cloud-storage/interoperability.png)

新しく生成されたアクセスキー ID と秘密アクセスキーを使用して、 [!DNL Google Cloud Storage] アカウントを Platform に送信します。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](/help/destinations/ui/connect-destination.md)の手順に従ってください。宛先設定ワークフローで、以下の 2 つのセクションにリストするフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

* **[!UICONTROL アクセスキー ID]**:61 文字の英数字から成る文字列で、 [!DNL Google Cloud Storage] アカウントを Platform に送信します。 この値の取得方法について詳しくは、 [前提条件](#prerequisites) 」の節を参照してください。
* **[!UICONTROL 秘密アクセスキー]**:ユーザーの認証に使用される 40 文字の base64 エンコードされた文字列 [!DNL Google Cloud Storage] アカウントを Platform に送信します。 この値の取得方法について詳しくは、 [前提条件](#prerequisites) 」の節を参照してください。
* **[!UICONTROL 暗号化キー]**:必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 以下の画像で、正しく書式設定された暗号化キーの例を参照してください。

   ![UI での正しく書式設定された PGP キーの例を示す画像](../../assets/catalog/cloud-storage/sftp/pgp-key.png)

これらの値の詳細については、 [Google Cloud Storage の HMAC キー](https://cloud.google.com/storage/docs/authentication/hmackeys#overview) ガイド。 独自のアクセスキー ID と秘密アクセスキーを生成する手順については、 [[!DNL Google Cloud Storage] ソースの概要](/help/sources/connectors/cloud-storage/google-cloud-storage.md).

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。


* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL バケット名]**:名前を入力 [!DNL Google Cloud Storage] この宛先で使用するバケット。
* **[!UICONTROL フォルダーパス]**:書き出したファイルをホストする保存先フォルダーのパスを入力します。
* **[!UICONTROL ファイルタイプ]**:書き出すファイルに使用する形式Experience Platformを選択します。 選択時に、 [!UICONTROL CSV] オプションを選択する場合は、 [ファイル形式設定オプションの設定](../../ui/batch-destinations-file-formatting-options.md).
* **[!UICONTROL 圧縮形式]**:書き出したファイルにExperience Platformが使用する圧縮タイプを選択します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) を参照してください。

### スケジュール設定

内 **[!UICONTROL スケジュール]** ステップ [書き出しスケジュールの設定](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling) の [!DNL Google Cloud Storage] 宛先と [書き出したファイルの名前を設定する](/help/destinations/ui/activate-batch-profile-destinations.md#file-names).

### 属性と ID のマッピング {#map}

内 **[!UICONTROL マッピング]** 手順では、プロファイルに書き出す属性および id フィールドを選択できます。 また、書き出したファイルのヘッダーを、任意のわかりやすい名前に変更するように選択することもできます。 詳しくは、 [マッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping) （「バッチ保存先 UI のアクティブ化」チュートリアル）。

## （ベータ版）データセットの書き出し {#export-datasets}

この宛先では、データセットの書き出しがサポートされます。 データセットのエクスポートを設定する方法について詳しくは、 [データセットの書き出しチュートリアル](/help/destinations/ui/export-datasets.md).

## データエクスポートの成功を検証 {#exported-data}

データが正常に書き出されたかどうかを確認するには、 [!DNL Google Cloud Storage] バケットを作成し、書き出したファイルに、期待されたプロファイルの母集団が含まれていることを確認します。
