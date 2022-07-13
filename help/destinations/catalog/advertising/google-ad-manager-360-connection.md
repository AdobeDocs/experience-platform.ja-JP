---
title: （ベータ版） [!DNL Google Ad Manager 360] 接続
description: Google Ad Manager 360 は、Googleの広告サービングプラットフォームで、パブリッシャーがビデオやモバイルアプリを通じて、Web サイト上の広告の表示を管理する手段を提供します。
exl-id: 3251145a-3e4d-40aa-b120-d79c8c9c7cae
source-git-commit: 4f57574bc17f43406df800358c7320372eb197d0
workflow-type: tm+mt
source-wordcount: '888'
ht-degree: 12%

---

# （ベータ版） [!DNL Google Ad Manager 360] 接続

## 概要 {#overview}

この [!DNL Google Ad Manager 360] 接続では、バッチアップロードが有効になります [!DNL publisher provided identifiers] (PPID) を次に移動： [!DNL Google Ad Manager 360]，経由 [!DNL Google Cloud Storage].

パブリッシャーが提供した ID がGoogle Ad Manager 360 でどのように機能するかについて詳しくは、 [公式Googleドキュメント](https://support.google.com/admanager/answer/2880055?hl=en).

>[!IMPORTANT]
>
>この宛先は現在ベータ版で、限られた数のお客様のみが利用できます。 [!DNL Google Ad Manager 360] 接続へのアクセスをリクエストするには、アドビ担当者に連絡し、[!DNL IMS Organization ID] を提供します。

この [!DNL Google Ad Manager 360] 宛先の書き出し [!DNL CSV] の [!DNL Google Cloud Storage] バケット。 を書き出したら、 [!DNL CSV] ファイルを [!DNL Google Ad Manager 360] アカウント

## 宛先の詳細 {#specifics}

次に示す詳細は、 [!DNL Google Ad Manager 360] 宛先。

* アクティブ化されたオーディエンスは、Googleプラットフォームでプログラムを使用して作成され、CSV ファイルに入力されます。

## サポートされる ID {#supported-identities}

[!DNL This integration] では、以下の表で説明する id のアクティブ化をサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| PPID | [!DNL Publisher provided ID] | オーディエンスを送信する先のこのターゲット ID を選択します [!DNL Google Ad Manager 360] |

{style=&quot;table-layout:auto&quot;}

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、該当するスキーマフィールド（PPID など）と共に、 [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳細を表示 [バッチファイルベースの宛先](/help/destinations/destination-types.md#file-based). |

{style=&quot;table-layout:auto&quot;}

## 前提条件 {#prerequisites}

### 許可リストへの登録 {#allow-listing}

>[!NOTE]
>
>許可リストは、最初の [!DNL Google Ad Manager] の宛先を設定します。 以下に説明する許可リストプロセスが次の方法で完了していることを確認してください： [!DNL Google] 宛先を作成する前に、をクリックします。

>[!IMPORTANT]
>
>Googleは、外部の Audience Management プラットフォームをGoogle Ad Manager 360 に接続するプロセスを簡略化しました。 これで、セルフサービス方式でGoogle Ad Manager 360 にリンクするプロセスを完了できます。 読み取り [データ管理プラットフォームからのセグメント](https://support.google.com/admanager/answer/3289669?hl=en) (Googleドキュメント ) を参照してください。 以下に示す ID を手元に用意してください。

* **アカウント ID**:AdobeのGoogleアカウント ID。 アカウント ID :87933855.
* **顧客 ID**:Adobeの顧客アカウント ID とGoogle。 顧客 ID :89690775.
* **ネットワークコード**:これがあなたの [!DNL Google Ad Manager] ネットワーク識別子（の下） **[!UICONTROL 管理者/グローバル設定]** (Googleインターフェイスの ) または（URL の）を参照してください。
* **オーディエンスリンク ID**:これは、 [!DNL Google Ad Manager] ネットワーク ( [!DNL Network code]) は、の下にもあります。 **[!UICONTROL 管理者/グローバル設定]** Googleインターフェイスの
* アカウントの種類。Google DFP または AdX 購入者。

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、 **[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html)の手順に従ってください。宛先設定ワークフローで、以下の 2 つのセクションにリストするフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先を認証するには、必須フィールドに入力し、「 」を選択します。 **[!UICONTROL 宛先に接続]**.

* **[!UICONTROL アクセスキー ID]**:61 文字の英数字から成る文字列で、 [!DNL Google Cloud Storage] アカウントを Platform に送信します。
* **[!UICONTROL 秘密アクセスキー]**:ユーザーの認証に使用される 40 文字のベース 64 エンコードされた文字列 [!DNL Google Cloud Storage] アカウントを Platform に送信します。

これらの値について詳しくは、 [Google Cloud Storage の HMAC キー](https://cloud.google.com/storage/docs/authentication/hmackeys#overview) ガイド。 独自のアクセスキー ID と秘密アクセスキーを生成する手順については、 [[!DNL Google Cloud Storage] ソースの概要](/help/sources/connectors/cloud-storage/google-cloud-storage.md).

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。 UI でフィールドの横にアスタリスクが表示される場合は、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先の名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL バケット名]**:名前を入力 [!DNL Google Cloud Storage] この宛先で使用するバケット。
* **[!UICONTROL フォルダーパス]**:書き出したファイルをホストする保存先フォルダーのパスを入力します。

### アラートの有効化 {#enable-alerts}

アラートを有効にして、宛先へのデータフローのステータスに関する通知を受け取ることができます。 リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の指定が完了したら、 **[!UICONTROL 次へ]**.

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

詳しくは、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) を参照してください。

ID マッピング手順では、次の事前入力済みマッピングを確認できます。

| 事前入力済みのマッピング | 説明 |
|---------|----------|
| `ECID` -> `ppid` | これは、ユーザーが編集可能な事前入力済みのマッピングのみです。 Platform から任意の属性または ID 名前空間を選択し、にマッピングできます。 `ppid`. |
| `metadata.segment.alias` -> `list_id` | Experience Platformセグメント名をGoogleプラットフォームのセグメント ID にマッピングします。 |
| `iif(${segmentMembership.ups.seg_id.status}=="exited", "1","0")` -> `delete` | 不適格なユーザーをセグメントから削除するタイミングをGoogleプラットフォームに伝えます。 |

これらのマッピングは、 [!DNL Google Ad Manager 360] すべての [!DNL Google Ad Manager 360] 接続。

![Google Ad Manager 360 のマッピング手順を示す UI 画像。](../../assets/catalog/advertising/google-ad-manager-360/ad-manager-360-mapping.png)

## 書き出したデータ {#exported-data}

データが正常に書き出されたかどうかを確認するには、 [!DNL Google Cloud Storage] バケットを作成し、書き出したファイルに、期待されたプロファイルの母集団が含まれていることを確認します。
