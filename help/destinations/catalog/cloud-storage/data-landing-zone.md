---
title: データランディングゾーンの宛先
description: データランディングゾーンに接続してセグメントをアクティブ化し、データセットを書き出す方法を説明します。
exl-id: 40b20faa-cce6-41de-81a0-5f15e6c00e64
source-git-commit: a07557ec398631ece0c8af6ec7b32e0e8593e24b
workflow-type: tm+mt
source-wordcount: '987'
ht-degree: 95%

---

# （ベータ版）データランディングゾーンの宛先

>[!IMPORTANT]
>
>この宛先は現在ベータ版で、一部のお客様のみご利用いただけます。[!DNL Data Landing Zone] 接続へのアクセスをリクエストするには、アドビ担当者に連絡し、[!DNL Organization ID] を提供します。


## 概要 {#overview}

[!DNL Data Landing Zone] は Adobe Experience Platform によってプロビジョニングされた [!DNL Azure Blob] ストレージインターフェイスです。安全なクラウドベースのファイルストレージ機能にアクセスして、ファイルを Platform から書き出すことができます。サンドボックスごとに 1 つの [!DNL Data Landing Zone] コンテナに対するアクセス権があります。すべてのコンテナの合計データ量は、Platform 製品およびサービスライセンスで提供される合計データ量に制限されます。Platform とそのアプリケーションサービスのすべての顧客（[!DNL Customer Journey Analytics]、[!DNL Journey Orchestration]、[!DNL Intelligent Services]、および [!DNL Real-time Customer Data Platform]）は、サンドボックスごとに 1 つの [!DNL Data Landing Zone] のコンテナを使用してプロビジョニングされます。[!DNL Azure Storage Explorer] またはコマンドラインインターフェイスを通じて、コンテナに対してファイルの読み取りと書き込みを行うことができます。

[!DNL Data Landing Zone] は SAS ベースの認証をサポートし、そのデータは保存時および転送中は標準 [!DNL Azure Blob] ストレージセキュリティメカニズムで保護されます。SAS ベースの認証を使用すると、パブリックインターネット接続を介して [!DNL Data Landing Zone] コンテナに安全にアクセスできます。ユーザーが [!DNL Data Landing Zone] コンテナにアクセスする場合、ネットワークの変更は必要ありません。つまり、ネットワークに対して許可リストの設定や地域間設定は必要ありません。

Platform では、[!DNL Data Landing Zone] コンテナへアップロードされるすべてのファイルで厳密に 7 日間の有効期間（TTL）が適用されます。すべてのファイルは 7 日後に削除されます。

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性の選択画面で選択したように、該当するスキーマフィールド（例：PPID）と共に、セグメントのすべてのメンバーを書き出しています。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

## [!DNL Data Landing Zone] のコンテンツを管理

[[!DNL Azure Storage Explorer]](https://azure.microsoft.com/ja-jp/features/storage-explorer/) を使用して [!DNL Data Landing Zone] コンテナのコンテンツを管理することができます。

[!DNL Azure Storage Explorer] UI 内で、左側のナビゲーションバーの「接続」アイコンを選択します。**リソースを選択**&#x200B;ウィンドウが開き、接続するオプションが表示されます。**[!DNL Blob container]** を選択し、[!DNL Data Landing Zone] ストレージに接続します。

![select-resource](/help/sources/images/tutorials/create/dlz/select-resource.png)

次に、接続方法として「**共有アクセス署名 URL (SAS)**」を選択し、「**次へ**」をクリックします。

![select-connection-method](/help/sources/images/tutorials/create/dlz/select-connection-method.png)

接続方法を選択した後、**表示名**&#x200B;およびお使いの [!DNL Data Landing Zone] コンテナに対応する&#x200B;**[!DNL Blob]コンテナ SAS URL** を入力します。

>[!IMPORTANT]
>
>データランディングゾーンの資格情報を取得するには、Platform API を使用する必要があります。詳しくは、[データランディングゾーン資格情報の取得](https://experienceleague.adobe.com/docs/experience-platform/sources/api-tutorials/create/cloud-storage/data-landing-zone.html?lang=ja#retrieve-data-landing-zone-credentials)を参照してください。
>
> 資格情報を取得し、書き出したファイルにアクセスするには、上記のページで説明されているすべての HTTP 呼び出しでクエリパラメーター `type=user_drop_zone` を `type=dlz_destination` に置き換える必要があります。

[!DNL Data Landing Zone] SAS URL を入力し、「**次へ**」を選択します。

![enter-connection-info](/help/sources/images/tutorials/create/dlz/enter-connection-info.png)

**概要**&#x200B;ウィンドウが開き、[!DNL Blob] エンドポイントと権限を含む設定の概要が表示されます。準備ができたら、「**接続**」を選択します。

![概要](/help/sources/images/tutorials/create/dlz/summary.png)

接続が成功すると、[!DNL Azure Storage Explorer] UI と [!DNL Data Landing Zone] コンテナが更新されます。

![dlz-user-container](/help/sources/images/tutorials/create/dlz/dlz-user-container.png)

[!DNL Data Landing Zone] コンテナが [!DNL Azure Storage Explorer] に接続され、Experience Platform から [!DNL Data Landing Zone] コンテナへのファイルの書き出しを開始できるようになりました。ファイルを書き出すには、以下の節で説明されているように、Experience Platform UI で [!DNL Data Landing Zone] の宛先への接続を確立する必要があります。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=ja)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

[!DNL Data Landing Zone] は、アドビがプロビジョニングしたストレージであるため、宛先への認証手順を実行する必要はありません。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする宛先フォルダーへのパス。
* **[!UICONTROL ファイルタイプ]**:書き出すファイルに使用する形式Experience Platformを選択します。 選択時に、 [!UICONTROL CSV] オプションを選択する場合は、 [ファイル形式設定オプションの設定](../../ui/batch-destinations-file-formatting-options.md).
* **[!UICONTROL 圧縮形式]**:書き出したファイルにExperience Platformが使用する圧縮タイプを選択します。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先にオーディエンスセグメントを有効化する手順については、[プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](../../ui/activate-batch-profile-destinations.md)を参照してください。

### スケジュール設定

**[!UICONTROL スケジュール設定]**&#x200B;手順では、[!DNL Data Landing Zone] 宛先の[書き出しスケジュールを設定](/help/destinations/ui/activate-batch-profile-destinations.md#scheduling)し、[書き出したファイルの名前を設定](/help/destinations/ui/activate-batch-profile-destinations.md#file-names)することもできます。

### 属性と ID のマッピング {#map}

**[!UICONTROL マッピング]**&#x200B;手順では、プロファイルに書き出す属性および ID フィールドを選択できます。 また、書き出したファイル内のヘッダーを選択して、任意のわかりやすい名前に変更することもできます。詳しくは、「バッチの宛先をアクティベート」UI チュートリアルの[マッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)を参照してください。

## （ベータ版）データセットの書き出し {#export-datasets}

この宛先では、データセットの書き出しをサポートしています。 データセットの書き出し設定方法について詳しくは、[データセットの書き出しチュートリアル](/help/destinations/ui/export-datasets.md)を参照してください。

## データの正常な書き出しの検証 {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Data Landing Zone] ストレージを確認し、書き出されたファイルに想定されるプロファイル母集団が含まれていることを確認してください。
