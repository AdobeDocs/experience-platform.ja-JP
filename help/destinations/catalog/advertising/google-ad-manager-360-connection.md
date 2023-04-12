---
title: （ベータ版） [!DNL Google Ad Manager 360] 接続
description: Google Ad Manager 360 は、媒体社がビデオやモバイルアプリを通じて web サイト上の広告の表示を管理できる、Google の広告配信プラットフォームです。
exl-id: 3251145a-3e4d-40aa-b120-d79c8c9c7cae
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 85%

---

# （ベータ版）[!DNL Google Ad Manager 360] 接続

## 概要 {#overview}

[!DNL Google Ad Manager 360] 接続では、[!DNL Google Cloud Storage] を介して、[!DNL Google Ad Manager 360] への [!DNL publisher provided identifiers]（PPID）のバッチアップロードが可能です。

媒体社が提供した ID が Google Ad Manager 360 でどのように機能するかについて詳しくは、[公式 Google ドキュメント](https://support.google.com/admanager/answer/2880055?hl=ja)を参照してください。

>[!IMPORTANT]
>
>この宛先は現在ベータ版で、一部のお客様のみご利用いただけます。[!DNL Google Ad Manager 360] 接続へのアクセスをリクエストするには、アドビ担当者に連絡し、お使いの [!DNL organization ID] を提供します。

[!DNL Google Ad Manager 360] 宛先では、[!DNL CSV] ファイルをお使いの [!DNL Google Cloud Storage] バケットに書き出します。[!DNL CSV] ファイルを書き出したら、お使いの [!DNL Google Ad Manager 360] アカウントに読み込む必要があります。

## 宛先の詳細 {#specifics}

[!DNL Google Ad Manager 360] 宛先に固有の次の詳細事項に注意してください。

* アクティブ化されたオーディエンスは、Google プラットフォームでプログラムにより作成され、CSV ファイルに入力されます。

## サポートされる ID {#supported-identities}

[!DNL This integration] では、以下の表で説明する ID のアクティベーションをサポートしています。

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| PPID | [!DNL Publisher provided ID] | オーディエンスを [!DNL Google Ad Manager 360] に送信するには、このターゲット ID を選択します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | [宛先のアクティベーションワークフロー](/help/destinations/ui/activate-batch-profile-destinations.md#select-attributes)のプロファイル属性の選択画面で選択したように、該当するスキーマフィールド（例：PPID）と共に、セグメントのすべてのメンバーを書き出しています。 |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

### 許可リストへの登録 {#allow-listing}

Platform で最初の [!DNL Google Ad Manager 360] の宛先を設定する前に、許可リストへの登録は必須です。宛先を作成する前に、以下に説明する許可リスト登録プロセスを完了してください。

>[!NOTE]
>
>このルールの例外は、既存の [Audience Manager](https://experienceleague.adobe.com/docs/audience-manager/user-guide/aam-home.html?lang=ja) 顧客。 この Google の宛先への接続を Audience Manager で既に作成している場合は、許可リストへの登録プロセスを再度実行する必要はありません。次の手順に進んでください。

1. 次に示す手順に従います： [Google Ad Manager ドキュメント](https://support.google.com/admanager/answer/3289669?hl=ja) Adobeをリンクされたデータ管理プラットフォーム (DMP) として追加する。
2. 内 [!DNL Google Ad Manager] インターフェイス、に移動します。 **[!UICONTROL 管理者]** > **[!UICONTROL グローバル設定]** > **[!UICONTROL ネットワーク設定]**&#x200B;をクリックし、 **[!UICONTROL API アクセス]** スライダー。


## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/connect-destination.html?lang=ja)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対する認証を行うには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

* **[!UICONTROL アクセスキー ID]**：61 文字の英数字から成る文字列で、Platform に対する [!DNL Google Cloud Storage] アカウントの認証に使用します。
* **[!UICONTROL 秘密アクセスキー]**：Base64 でエンコードされた40 文字の文字列で、Platform に対する [!DNL Google Cloud Storage] アカウントの認証に使用します。

これらの値について詳しくは、[Google Cloud Storage の HMAC キー](https://cloud.google.com/storage/docs/authentication/hmackeys#overview)ガイドを参照してください。独自のアクセスキー ID と秘密アクセスキーを生成する手順については、[[!DNL Google Cloud Storage] ソースの概要](/help/sources/connectors/cloud-storage/google-cloud-storage.md)を参照してください。

### 宛先の詳細を入力 {#destination-details}

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

* **[!UICONTROL 名前]**：この宛先に希望する名前を入力します。
* **[!UICONTROL 説明]**：オプション。例えば、この宛先を使用しているキャンペーンを指定できます。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする宛先フォルダーへのパス。
* **[!UICONTROL バケット名]**：この宛先が使用する [!DNL Google Cloud Storage] バケット名を入力します。
* **[!UICONTROL アカウント ID]**:を入力します。 [!DNL Audience Link ID] から [!DNL Google] アカウント これは、 [!DNL Google Ad Manager] ネットワーク ( [!DNL Network code]) をクリックします。 これは、の下にあります。 **[!UICONTROL 管理者/グローバル設定]** 内 [!DNL Google Ad Manager] インターフェイス。
* **[!UICONTROL アカウントタイプ]**:次の条件に応じて、オプションを選択します。 [!DNL Google] アカウント：
   * [!DNL Google AdX] に `AdX buyer` を使用する
   * [!DNL DoubleClick] for Publishers に `DFP by Google` を使用する

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)についてのガイドを参照してください。

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に対してオーディエンスセグメントをアクティブ化する手順については、 [バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md)を参照してください。

ID マッピングステップでは、次の事前入力済みマッピングが表示されます。

| 事前入力済みマッピング | 説明 |
|---------|----------|
| `ECID` -> `ppid` | ユーザーによる編集が可能な事前入力済みのマッピングは、これだけです。Platform から任意の属性または ID 名前空間を選択し、それを `ppid` にマッピングすることができます。 |
| `metadata.segment.alias` -> `list_id` | Experience Platform セグメント名を Google プラットフォームのセグメント ID にマッピングします。 |
| `iif(${segmentMembership.ups.seg_id.status}=="exited", "1","0")` -> `delete` | 不適格なユーザーをセグメントから削除するタイミングを Google プラットフォームに指示します。 |

これらのマッピングは [!DNL Google Ad Manager 360] で必要であり、すべての [!DNL Google Ad Manager 360] 接続に対してAdobe Experience Platform によって自動的に作成されます。

![Google Ad Manager 360 のマッピングステップを示す UI 画像](../../assets/catalog/advertising/google-ad-manager-360/ad-manager-360-mapping.png)

## 書き出したデータ {#exported-data}

データが正常に書き出されたかどうかを確認するには、[!DNL Google Cloud Storage] バケットを確認し、書き出されたファイルに想定どおりのプロファイル母集団が含まれていることを確認します。

## トラブルシューティング {#troubleshooting}

この宛先の使用中にエラーが発生し、AdobeまたはGoogleに連絡する必要がある場合は、次の ID を手元に置いてください。

AdobeのGoogleアカウント ID は次のとおりです。

* **[!UICONTROL アカウント ID]**:87933855
* **[!UICONTROL 顧客 ID]**:89690775