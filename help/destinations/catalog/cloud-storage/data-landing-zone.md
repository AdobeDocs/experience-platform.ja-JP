---
title: データランディングゾーンの宛先
description: データランディングゾーンに接続してセグメントをアクティブ化し、データセットを書き出す方法を説明します。
exl-id: 40b20faa-cce6-41de-81a0-5f15e6c00e64
source-git-commit: a07557ec398631ece0c8af6ec7b32e0e8593e24b
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 25%

---

# （ベータ版）データランディングゾーンの宛先

>[!IMPORTANT]
>
>この宛先は現在ベータ版で、限られた数のお客様のみが利用できます。 [!DNL Data Landing Zone] 接続へのアクセスをリクエストするには、アドビ担当者に連絡し、[!DNL Organization ID] を提供します。


## 概要 {#overview}

[!DNL Data Landing Zone] は [!DNL Azure Blob] Adobe Experience Platformによってプロビジョニングされたストレージインターフェイス。Platform からファイルをエクスポートするための、セキュリティで保護されたクラウドベースのファイルストレージ機能にアクセスできます。 1 つに対するアクセス権があります [!DNL Data Landing Zone] サンドボックスごとのコンテナおよびすべてのコンテナの合計データ量は、Platform 製品およびサービスライセンスで提供される合計データ量に制限されます。 Platform とそのアプリケーションサービスのすべてのお客様 ( [!DNL Customer Journey Analytics], [!DNL Journey Orchestration], [!DNL Intelligent Services]、および [!DNL Real-time Customer Data Platform] は、1 つの [!DNL Data Landing Zone] サンドボックスごとのコンテナ を通じて、コンテナに対してファイルの読み取りと書き込みをおこなうことができます [!DNL Azure Storage Explorer] またはコマンドラインインターフェイスを使用します。

[!DNL Data Landing Zone] は SAS ベースの認証をサポートし、そのデータは標準で保護されています [!DNL Azure Blob] 保存時および移動時の保管セキュリティメカニズム SAS ベースの認証を使用すると、 [!DNL Data Landing Zone] コンテナを使用して、公開インターネット接続を介して接続できます。 ユーザーが [!DNL Data Landing Zone] コンテナを使用する場合は、ネットワークに対して許可リストや地域間の設定を設定する必要はありません。

Platform では、 [!DNL Data Landing Zone] コンテナ。 すべてのファイルは 7 日後に削除されます。

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、該当するスキーマフィールド（PPID など）と共に、 [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳細を表示 [バッチファイルベースの宛先](/help/destinations/destination-types.md#file-based). |

{style=&quot;table-layout:auto&quot;}

## コンテンツを管理 [!DNL Data Landing Zone]

以下を使用できます。 [[!DNL Azure Storage Explorer]](https://azure.microsoft.com/en-us/features/storage-explorer/) コンテンツを管理するには [!DNL Data Landing Zone] コンテナ。

内 [!DNL Azure Storage Explorer] UI で、左側のナビゲーションバーの接続アイコンを選択します。 この **リソースを選択** ウィンドウが開き、接続するオプションが表示されます。 選択 **[!DNL Blob container]** を [!DNL Data Landing Zone] ストレージ。

![select-resource](/help/sources/images/tutorials/create/dlz/select-resource.png)

次に、 **共有アクセス署名 URL (SAS)** を選択し、「 」を選択します。 **次へ**.

![select-connection-method](/help/sources/images/tutorials/create/dlz/select-connection-method.png)

接続方法を選択した後、 **表示名** そして **[!DNL Blob]コンテナ SAS URL** は、 [!DNL Data Landing Zone] コンテナ。

>[!IMPORTANT]
>
>データランディングゾーンの資格情報を取得するには、Platform API を使用する必要があります。 詳しくは、 [データランディングゾーン資格情報の取得](https://experienceleague.adobe.com/docs/experience-platform/sources/api-tutorials/create/cloud-storage/data-landing-zone.html?lang=en#retrieve-data-landing-zone-credentials).
>
> 資格情報を取得して書き出したファイルにアクセスするには、クエリーパラメーターを置き換える必要があります `type=user_drop_zone` と `type=dlz_destination` 上記のページで説明されているすべての HTTP 呼び出し。

次を指定： [!DNL Data Landing Zone] SAS URL を選択し、 **次へ**.

![enter-connection-info](/help/sources/images/tutorials/create/dlz/enter-connection-info.png)

この **概要** ウィンドウが開き、設定の概要 ( [!DNL Blob] エンドポイントと権限。 準備が整ったら、「 」を選択します。 **接続**.

![概要](/help/sources/images/tutorials/create/dlz/summary.png)

接続が成功すると、次の情報が更新されます： [!DNL Azure Storage Explorer] UI と [!DNL Data Landing Zone] コンテナ。

![dlz-user-container](/help/sources/images/tutorials/create/dlz/dlz-user-container.png)

を [!DNL Data Landing Zone] ～に接続された容器 [!DNL Azure Storage Explorer]を使用して、Experience Platformから [!DNL Data Landing Zone] コンテナ。 ファイルを書き出すには、 [!DNL Data Landing Zone] の宛先を指定します（以下の節で説明）。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html)の手順に従ってください。宛先設定ワークフローで、以下の 2 つのセクションにリストするフィールドに入力します。

### 宛先に対する認証 {#authenticate}

理由： [!DNL Data Landing Zone] は、Adobeがプロビジョニングしたストレージです。宛先への認証手順を実行する必要はありません。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。


* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
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

内 **[!UICONTROL スケジュール]** ステップ [書き出しスケジュールの設定](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling) の [!DNL Data Landing Zone] 宛先と [書き出したファイルの名前を設定する](/help/destinations/ui/activate-batch-profile-destinations.md#file-names).

### 属性と ID のマッピング {#map}

内 **[!UICONTROL マッピング]** 手順では、プロファイルに書き出す属性および id フィールドを選択できます。 また、書き出したファイルのヘッダーを、任意のわかりやすい名前に変更するように選択することもできます。 詳しくは、 [マッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping) （「バッチ保存先 UI のアクティブ化」チュートリアル）。

## （ベータ版）データセットの書き出し {#export-datasets}

この宛先では、データセットの書き出しがサポートされます。 データセットのエクスポートを設定する方法について詳しくは、 [データセットの書き出しチュートリアル](/help/destinations/ui/export-datasets.md).

## データエクスポートの成功を検証 {#exported-data}

データが正常に書き出されたかどうかを確認するには、 [!DNL Data Landing Zone] ストレージに保存し、書き出したファイルに、期待されたプロファイルの母集団が含まれていることを確認します。
