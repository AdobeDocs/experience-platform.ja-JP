---
title: （アルファ） [!DNL LiveRamp SFTP] 接続
description: LiveRamp コネクタを使用して、Adobe Real-time Customer Data Platformから LiveRamp Connect にオーディエンスをオンボーディングする方法を説明します。
hidefromtoc: true
hide: true
source-git-commit: 367ef59f623acc38e636a6cae0c85f186eaccfda
workflow-type: tm+mt
source-wordcount: '1738'
ht-degree: 19%

---


# （アルファ） [!DNL LiveRamp - SFTP] 接続 {#liveramp-destination}

LiveRamp 接続を使用して、Adobe Real-time Customer Data Platformからにオーディエンスをオンボーディングします。 [!DNL LiveRamp Connect].

>[!IMPORTANT]
>
><p>この宛先接続は現在アルファステージにあり、限られたユーザーのみが使用できます。 機能とドキュメントは変更される場合があります。</p>
&gt;<p>この宛先接続の最終バージョンでは、お客様の移行が必要になる場合があります。</p>


## ユースケース {#use-cases}

をいつどのように使用するかをより深く理解するのに役立ちます。 [!DNL LiveRamp SFTP] の宛先について、Adobe Experience Platformのお客様がこの宛先を使用して解決できる使用例を以下に示します。

マーケターは、Adobe Experience Platformからオーディエンスを送信し、ID をにオンボーディングしたい [!DNL LiveRamp Connect] ユーザーを [!DNL CTV] プラットフォーム、使用 [!DNL Ramp ID] 識別子。

## 前提条件 {#prerequisites}

この [!DNL LiveRamp - SFTP] 接続を使用してファイルを書き出す [LiveRamp の SFTP](https://docs.liveramp.com/connect/en/upload-a-file-via-liveramp-s-sftp.html) ストレージ。

にデータをExperience Platformから送信する前に [!DNL LiveRamp SFTP]、 [!DNL LiveRamp] 資格情報。 詳しくは、 [!DNL LiveRamp] の担当者を使用して、資格情報を取得します（まだ取得していない場合）。

## サポートされる ID {#supported-identities}

LiveRamp SFTP は、PII ベースの識別子、既知の識別子、カスタム ID などの ID のアクティブ化をサポートします ( 公式の [LiveRamp ドキュメント](https://docs.liveramp.com/connect/en/identity-and-identifier-terms-and-concepts.html#known-identifiers).

内 [マッピング手順](#map) アクティベーションワークフローでは、ターゲットマッピングをカスタム属性として定義する必要があります。

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL セグメントの書き出し]** | セグメント（オーディエンス）のすべてのメンバーを、 [!DNL LiveRamp SFTP] 宛先。 |
| 書き出し頻度 | **[!UICONTROL 日別バッチ]** | セグメントの評価に基づいてExperience Platform内でプロファイルが更新されると、プロファイル (ID) は、宛先プラットフォームの下流にある 1 日 1 回更新されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

**パスワードを使用した SFTP 認証** {#sftp-password}

![パスワードを使用して SFTP を使用して宛先に対して認証する方法を示したサンプルスクリーンショット](../../assets/catalog/advertising/liveramp/liveramp-sftp-password.png)

* **[!UICONTROL ユーザー名]**:ユーザー名 [!DNL LiveRamp SFTP] ストレージの場所。
* **[!UICONTROL パスワード]**:ユーザーのパスワード [!DNL LiveRamp SFTP] ストレージの場所。
* **[!UICONTROL PGP/GPG 暗号化キー]**:必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 以下の画像で、正しく書式設定された暗号化キーの例を参照してください。 暗号化キーを指定する場合は、 **[!UICONTROL 暗号化サブキー ID]** 内 [宛先の詳細](#destination-details) 」セクションに入力します。

   ![UI での正しく書式設定された PGP キーの例を示す画像](../../assets/catalog/advertising/liveramp/pgp-key.png)

**SSH キー認証を使用した SFTP** {#sftp-ssh}

![SSH キーを使用して宛先を認証する方法を示したサンプルスクリーンショット](../../assets/catalog/advertising/liveramp/liveramp-sftp-ssh.png)

* **[!UICONTROL ユーザー名]**:ユーザー名 [!DNL LiveRamp SFTP] ストレージの場所。
* **[!UICONTROL SSH キー]**:プライベート [!DNL SSH] にログインするための鍵 [!DNL LiveRamp SFTP] ストレージの場所。 秘密鍵は [!DNL Base64]-encoded 文字列。パスワードで保護することはできません。

   * 次の手順で [!DNL SSH] キー [!DNL LiveRamp SFTP] サーバーの場合は、 [!DNL LiveRamp]のテクニカルサポートポータルを参照し、公開鍵を提供してください。 詳しくは、 [LiveRamp ドキュメント](https://docs.liveramp.com/connect/en/upload-a-file-via-liveramp-s-sftp.html#upload-with-an-sftp-client).

* **[!UICONTROL PGP/GPG 暗号化キー]**:必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 暗号化キーを指定する場合は、 **[!UICONTROL 暗号化サブキー ID]** 内 [宛先の詳細](#destination-details) 」セクションに入力します。 以下の画像で、正しく書式設定された暗号化キーの例を参照してください。

   ![UI での正しく書式設定された PGP キーの例を示す画像](../../assets/catalog/advertising/liveramp/pgp-key.png)

### 宛先の詳細を入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_subkey"
>title="暗号化サブキー ID"
>abstract="LiveRamp 公開暗号化キーに基づく、暗号化に使用されるサブキー ID。 このフィールドは、認証手順で暗号化キーを指定した場合に必須です。"
>additional-url="https://docs.liveramp.com/connect/en/encrypting-files-for-uploading.html#downloading-the-current-encryption-key" text="サブキー ID の取得方法を説明します"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![宛先の詳細を入力する方法を示す Platform UI のスクリーンショット](../../assets/catalog/advertising/liveramp/liveramp-connection-details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL フォルダーパス]**:パス： [!DNL LiveRamp] `uploads` 書き出したファイルを格納するサブフォルダー。 この `uploads` というプレフィックスが自動的にフォルダーパスに追加されます。
   * 例えば、ファイルをに書き出す場合は、 `uploads/my_export_folder`，入力 `my_export_folder` 内 **[!UICONTROL フォルダーパス]** フィールドに入力します。
* **[!UICONTROL 圧縮形式]**:書き出したファイルにExperience Platformが使用する圧縮タイプを選択します。 次のオプションを使用できます。 **[!UICONTROL GZIP]** または **[!UICONTROL なし]**.
* **[!UICONTROL 暗号化サブキー ID]**:暗号化に使用されるサブキー。 [!DNL LiveRamp] 公開暗号化キー。 このフィールドは、 [認証](#authenticate) 手順 詳しくは、 [!DNL LiveRamp] [暗号化ドキュメント](https://docs.liveramp.com/connect/en/encrypting-files-for-uploading.html#downloading-the-current-encryption-key) サブキー ID の取得方法を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートの詳細については、 [UI を使用した宛先アラートの購読](../../ui/alerts.md).

宛先接続の詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対してセグメントをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]**[に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](/help/destinations/ui/activate-batch-profile-destinations.md) を参照してください。

### スケジュール設定 {#scheduling}

内 [!UICONTROL スケジュール] 手順を実行し、各セグメントの書き出しスケジュールを作成します。設定は次のとおりです。

>[!IMPORTANT]
>
>この宛先に対してアクティブ化されるすべてのセグメントは、次に示すように、まったく同じスケジュールで設定する必要があります。

* **[!UICONTROL ファイル書き出しオプション]**: [!UICONTROL 完全なファイルを書き出し]. [増分ファイルの書き出し](../../ui/activate-batch-profile-destinations.md#export-incremental-files) は現在、 [!DNL LiveRamp] 宛先。
* **[!UICONTROL 頻度]**: [!UICONTROL 毎日]
* 書き出し時間を次の値に設定します。 **[!UICONTROL セグメント評価後]**. スケジュールされたセグメントのエクスポートと [オンデマンドファイルの書き出し](../../ui/export-file-now.md) は現在、 [!DNL LiveRamp] 宛先。
* **[!UICONTROL 日付]**:必要に応じて、書き出しの開始時刻と終了時刻を選択します。

![セグメントのスケジュール設定手順を示した Platform UI のスクリーンショット。](../../assets/catalog/advertising/liveramp/liveramp-segment-scheduling.png)

書き出されたファイル名は、現在、ユーザーが設定できません。 に書き出されたすべてのファイル [!DNL LiveRamp SFTP] の宛先は、次のテンプレートに基づいて自動的に名前が付けられます。

`%ORGANIZATION_NAME%_%DESTINATION%_%DESTINATION_INSTANCE_ID%_%DATETIME%`

![書き出されたファイル名テンプレートを示す Platform UI のスクリーンショット。](../../assets/catalog/advertising/liveramp/liveramp-file-name.png)

例えば、という名前の組織に対してエクスポートされたファイルの名前 [!DNL Luma] 次のようになります。

```json
Luma_LiveRamp_52137231-4a99-442d-804c-39a09ddd005d_20230330_153857.csv
```

### 属性と ID のマッピング {#map}

内 **[!UICONTROL マッピング]** 手順では、プロファイルに書き出す属性と id を選択できます。

>[!IMPORTANT]
>
>この宛先は、アクティベーションフローごとに 1 つのソース ID 名前空間をアクティブ化できます。 複数の ID 名前空間（例： ）を書き出す必要がある場合 `Email` および `Phone`を [別のアクティベーションフローを作成する](../../ui/activate-batch-profile-destinations.md) ID ごとに。

内 **[!UICONTROL マッピング]** ステップ、 **[!UICONTROL ターゲットフィールド]** マッピングは、書き出された CSV ファイルの列ヘッダーの名前を定義します。 書き出したファイルの CSV 列ヘッダーを、任意のわかりやすい名前に変更できます。そのためには、 **[!UICONTROL ターゲットフィールド]**.

1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。画面に新しいマッピング行が表示されます。

   ![Experience PlatformUI 画面マッピング画面を表示](../../assets/catalog/advertising/liveramp/liveramp-add-new-mapping.png)

2. 内 **[!UICONTROL ソースフィールドを選択]** ウィンドウで、 **[!UICONTROL 属性を選択]** カテゴリを選択して、マッピングする XDM 属性を選択するか、 **[!UICONTROL ID 名前空間を選択]** カテゴリを選択し、宛先にマッピングする id を選択します。

   ![Experience PlatformUI のスクリーンショットにソースマッピング画面が表示される。](../../assets/catalog/advertising/liveramp/liveramp-source-mapping.png)

3. 内 **[!UICONTROL ターゲットフィールドを選択]** ウィンドウで、選択したソースフィールドをマッピングする属性名を入力します。 ここで定義した属性名は、書き出された CSV ファイルに列ヘッダーとして反映されます。

   ![Experience PlatformUI 画面にターゲットマッピング画面が表示されます。](../../assets/catalog/advertising/liveramp/liveramp-target-mapping.png)

   また、 **[!UICONTROL ターゲットフィールド]**.

   ![Experience PlatformUI 画面にターゲットマッピング画面が表示されます。](../../assets/catalog/advertising/liveramp/liveramp-target-field.png)

目的のマッピングをすべて追加したら、「 」を選択します。 **[!UICONTROL 次へ]** アクティベーションワークフローを終了します。

## エクスポートされたデータ/データエクスポートの検証 {#exported-data}

データは、 [!DNL LiveRamp SFTP] CSV ファイルとして設定したストレージの場所。

ファイルを [!DNL LiveRamp SFTP] 宛先， Platform は、それぞれに 1 つの CSV ファイルを生成します [結合ポリシー ID](../../../profile/merge-policies/overview.md).

例えば、次のセグメントについて考えてみましょう。

* セグメント A（結合ポリシー 1）
* セグメント B（結合ポリシー 2）
* セグメント C（結合ポリシー 1）
* セグメント D（結合ポリシー 1）

Platform は 2 つの CSV ファイルをに書き出します [!DNL LiveRamp SFTP]:

* セグメント A、C、D を含む 1 つの CSV ファイル
* セグメント B を含む 1 つの CSV ファイル。

書き出された CSV ファイルには、選択した属性と対応するセグメントステータスを持つプロファイルが別々の列に含まれ、属性名とセグメント ID が列ヘッダーとして含まれます。

書き出されたファイルに含まれるプロファイルは、次のセグメント認定ステータスのいずれかに一致します。

* `Active`:プロファイルは現在、セグメントに適合しています。
* `Expired`:プロファイルはセグメントに対して認定されなくなりましたが、過去に認定されています。
* `""`（空の文字列）:プロファイルは、セグメントに対して認定されませんでした。


例えば、書き出された CSV ファイル（1 つを含む） `email` 属性と 3 つのセグメントは次のようになります。

```csv
email,aa2e3d98-974b-4f8b-9507-59f65b6442df,45d4e762-6e57-4f2f-a3e0-2d1893bcdd7f,7729e537-4e42-418e-be3b-dce5e47aaa1e
abc117@testemailabc.com,active,,
abc111@testemailabc.com,,,active
abc102@testemailabc.com,,,active
abc116@testemailabc.com,active,,
abc107@testemailabc.com,active,expired,active
abc101@testemailabc.com,active,active,
```

Platform では各 [結合ポリシー ID](../../../profile/merge-policies/overview.md)の場合は、結合ポリシー ID ごとに個別のデータフロー実行も生成されます。

これは、 **[!UICONTROL アクティブ化された ID]** および **[!UICONTROL 受信したプロファイル]** 指標 [データフロー実行](../../../dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) ページは、セグメントごとに表示されるのではなく、同じ結合ポリシーを使用するセグメントの各グループに対して集計されます。

同じ結合ポリシーを使用するセグメントのグループに対してデータフローの実行が生成されるので、セグメント名は [監視ダッシュボード](../../../dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations).

![Experience PlatformUI の画面に、アクティブ化された指標が表示されます。](../../assets/catalog/advertising/liveramp/liveramp-metrics.png)

## 書き出したデータを LiveRamp にアップロード {#upload-to-liveramp}

データが [!DNL LiveRamp - SFTP] ストレージの [!DNL LiveRamp] プラットフォーム。

からファイルをアップロードする方法の詳細 [!DNL LiveRamp - SFTP] ストレージ [!DNL LiveRamp] オーディエンスについては、次のドキュメントを参照してください。 [最初のファイルをオーディエンスにアップロードする際の考慮事項](https://docs.liveramp.com/connect/en/considerations-when-uploading-the-first-file-to-an-audience.html#considerations-when-uploading-the-first-file-to-an-audience).

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

LiveRamp SFTP ストレージの設定方法について詳しくは、 [公式ドキュメント](https://docs.liveramp.com/connect/en/upload-a-file-via-liveramp-s-sftp.html).
