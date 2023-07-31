---
title: LiveRamp — オンボーディング接続
description: LiveRamp コネクタを使用して、Adobe Real-time Customer Data Platform から LiveRamp Connect にオーディエンスをオンボードする方法を説明します。
last-substantial-update: 2023-07-26T00:00:00Z
exl-id: b8ce7ec2-7af9-4d26-b12f-d38c85ba488a
source-git-commit: 3090b8a8eade564190dc32142c3fc71701007337
workflow-type: tm+mt
source-wordcount: '1868'
ht-degree: 70%

---

# [!DNL LiveRamp - Onboarding] 接続 {#liveramp-onboarding}

以下を使用します。 [!DNL LiveRamp - Onboarding] Adobe Real-time Customer Data Platformからにオーディエンスをオンボーディングするための接続 [!DNL LiveRamp Connect].

## ユースケース {#use-cases}

[!DNL LiveRamp - Onboarding] 宛先を使用する方法とタイミングを理解しやすくするために、Adobe Experience Platform のお客様がこの宛先を使用して解決できるユースケースのサンプルを以下に示します。

マーケターは、Adobe Experience Platformからオーディエンスを送信し、ID をにオンボーディングしたい [!DNL LiveRamp Connect] モバイル、オープン Web、ソーシャル、 [!DNL CTV] プラットフォーム、使用 [!DNL Ramp ID] 識別子。

## 前提条件 {#prerequisites}

[!DNL LiveRamp - Onboarding] 接続では、[LiveRamp の SFTP](https://docs.liveramp.com/connect/en/upload-a-file-via-liveramp-s-sftp.html) ストレージを使用してファイルを書き出します。

Experience Platform から [!DNL LiveRamp - Onboarding] にデータを送信するには、まず [!DNL LiveRamp] 資格情報が必要です。資格情報がまだない場合は、[!DNL LiveRamp] の担当者に連絡して資格情報を取得してください。

## サポートされている ID {#supported-identities}

[!DNL LiveRamp - Onboarding] では、公式の [LiveRamp ドキュメント](https://docs.liveramp.com/connect/en/identity-and-identifier-terms-and-concepts.html#known-identifiers)に記載されている、PII ベースの識別子、既知の識別子、カスタム ID などの ID のアクティブ化をサポートしています。

アクティブ化ワークフローの[マッピングステップ](#map)では、ターゲットマッピングをカスタム属性として定義する必要があります。

## サポートされるオーディエンス {#supported-audiences}

この節では、この宛先に書き出すことができるすべてのオーディエンスについて説明します。

すべての宛先は、Experience Platformを通じて生成されたオーディエンスのアクティブ化をサポートします [セグメント化サービス](../../../segmentation/home.md).

また、この宛先では、以下の表で説明するオーディエンスのアクティブ化もサポートされます。

| オーディエンスタイプ | 説明 |
---------|----------|
| カスタムアップロード | オーディエンス [インポート済み](../../../segmentation/ui/overview.md#importing-an-audience) を CSV ファイルからExperience Platformに追加します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| 項目 | タイプ | メモ |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL オーディエンスの書き出し]** | オーディエンスのすべてのメンバーを、 [!DNL LiveRamp - Onboarding] 宛先。 |
| 書き出し頻度 | **[!UICONTROL 日別バッチ]** | オーディエンスの評価に基づいてExperience Platform内でプロファイルが更新されると、プロファイル (ID) は、宛先プラットフォームの下流にある 1 日 1 回更新されます。 詳しくは、[バッチ（ファイルベース）宛先](/help/destinations/destination-types.md#file-based)を参照してください。 |

{style="table-layout:auto"}

## 宛先への接続 {#connect}

>[!IMPORTANT]
> 
>宛先に接続するには、**[!UICONTROL 宛先の管理]** [アクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[宛先設定のチュートリアル](../../ui/connect-destination.md)の手順に従ってください。宛先の設定ワークフローで、以下の 2 つのセクションにリストされているフィールドに入力します。

### 宛先に対する認証 {#authenticate}

宛先に対して認証するには、必須フィールドに入力し、「**[!UICONTROL 宛先に接続]**」を選択します。

**パスワードを使用した SFTP 認証** {#sftp-password}

![パスワードによる SFTP を使用して宛先に対する認証を行う方法を示すサンプルスクリーンショット](../../assets/catalog/advertising/liveramp-onboarding/liveramp-sftp-password.png)

* **[!UICONTROL ユーザー名]**：[!DNL LiveRamp - Onboarding] ストレージの場所のユーザー名。
* **[!UICONTROL パスワード]**：[!DNL LiveRamp - Onboarding] ストレージの場所のパスワード。
* **[!UICONTROL PGP／GPG 暗号化キー]**：必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。正しい形式の暗号化キーの例については、以下の画像を参照してください。暗号化キーを入力する場合は、「[宛先の詳細](#destination-details)」セクションで&#x200B;**[!UICONTROL 暗号化サブキー ID]** も入力する必要があります。

  ![UI での正しい形式の PGP キーの例を示す画像](../../assets/catalog/advertising/liveramp-onboarding/pgp-key.png)

**SSH キー認証を使用した SFTP** {#sftp-ssh}

![SSH キーを使用して宛先に対する認証を行う方法を示すサンプルスクリーンショット](../../assets/catalog/advertising/liveramp-onboarding/liveramp-sftp-ssh.png)

* **[!UICONTROL ユーザー名]**：[!DNL LiveRamp - Onboarding] ストレージの場所のユーザー名。
* **[!UICONTROL SSH キー]**：[!DNL LiveRamp - Onboarding] ストレージの場所へのログインに使用する [!DNL SSH] 秘密鍵。この秘密鍵は、[!DNL Base64] でエンコードされた文字列の形式にする必要があり、パスワードで保護しないでください。

   * [!DNL SSH] キーを [!DNL LiveRamp - Onboarding] サーバーに接続するには、[!DNL LiveRamp] のテクニカルサポートポータルを通じてチケットを送信し、公開鍵を入力する必要があります。詳しくは、[LiveRamp ドキュメント](https://docs.liveramp.com/connect/en/upload-a-file-via-liveramp-s-sftp.html#upload-with-an-sftp-client)を参照してください。

* **[!UICONTROL PGP／GPG 暗号化キー]**：必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。暗号化キーを入力する場合は、「[宛先の詳細](#destination-details)」セクションで&#x200B;**[!UICONTROL 暗号化サブキー ID]** も入力する必要があります。正しい形式の暗号化キーの例については、以下の画像を参照してください。

  ![UI での正しい形式の PGP キーの例を示す画像](../../assets/catalog/advertising/liveramp-onboarding/pgp-key.png)

### 宛先の詳細の入力 {#destination-details}

>[!CONTEXTUALHELP]
>id="platform_destinations_liveramp_subkey"
>title="暗号化サブキー ID"
>abstract="LiveRamp 公開暗号化キーに基づく、暗号化に使用するサブキー ID。認証手順で暗号化キーを入力した場合、このフィールドは必須です。"
>additional-url="https://docs.liveramp.com/connect/en/encrypting-files-for-uploading.html#downloading-the-current-encryption-key" text="サブキー ID の取得方法を説明します"

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![宛先の詳細を入力する方法を示す Platform UI のスクリーンショット](../../assets/catalog/advertising/liveramp-onboarding/liveramp-connection-details.png)

* **[!UICONTROL 名前]**：今後この宛先を認識するための名前。
* **[!UICONTROL 説明]**：今後この宛先を識別するのに役立つ説明。
* **[!UICONTROL フォルダーパス]**：書き出したファイルをホストする [!DNL LiveRamp] `uploads` サブフォルダーへのパス。`uploads` プレフィックスがフォルダーパスに自動的に追加されます。[!DNL LiveRamp] 他の既存のフィードとは別にファイルを保存し、すべての自動処理がスムーズに実行されるように、 Adobe Real-Time CDPからの配信用に専用のサブフォルダーを作成することをお勧めします。
   * 例えば、ファイルを `uploads/my_export_folder` に書き出す場合は、「**[!UICONTROL フォルダーパス]**」フィールドに `my_export_folder` と入力します。
* **[!UICONTROL 圧縮形式]**：書き出したファイルに Experience Platform で使用する圧縮タイプを選択します。使用可能なオプションは、**[!UICONTROL GZIP]** または&#x200B;**[!UICONTROL なし]**&#x200B;です。
* **[!UICONTROL 暗号化サブキー ID]**：[!DNL LiveRamp] 公開暗号化キーに基づく、暗号化に使用するサブキー。[認証](#authenticate)手順で暗号化キーを入力した場合、このフィールドは必須です。サブキー ID の取得方法については、[!DNL LiveRamp] [暗号化ドキュメント](https://docs.liveramp.com/connect/en/encrypting-files-for-uploading.html#downloading-the-current-encryption-key)を参照してください。

### アラートの有効化 {#enable-alerts}

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読](../../ui/alerts.md)に関するガイドを参照してください。

宛先接続への詳細の入力を終えたら「**[!UICONTROL 次へ]**」を選択します。

## この宛先に対するオーディエンスをアクティブ化 {#activate}

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

読み取り [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](/help/destinations/ui/activate-batch-profile-destinations.md) を参照してください。

### スケジュール設定 {#scheduling}

Adobe Analytics の [!UICONTROL スケジュール] 次の設定で、各オーディエンスの書き出しスケジュールを作成します。

>[!IMPORTANT]
>
>この宛先に対してアクティブ化されるすべてのオーディエンスは、以下に示すように、同じスケジュールで設定する必要があります。

* **[!UICONTROL ファイル書き出しオプション]**：[!UICONTROL 完全ファイルを書き出し]。[増分ファイル書き出し](../../ui/activate-batch-profile-destinations.md#export-incremental-files)は現在、[!DNL LiveRamp] 宛先ではサポートされていません。
* **[!UICONTROL 頻度]**：[!UICONTROL 毎日]
* 書き出し時間を&#x200B;**[!UICONTROL セグメント評価後]**&#x200B;に設定します。スケジュールされたオーディエンスのエクスポートおよび [オンデマンドファイルの書き出し](../../ui/export-file-now.md) は、現在、 [!DNL LiveRamp] 宛先。
* **[!UICONTROL 日付]**：希望する書き出し開始時刻および終了時刻を選択します。

![オーディエンスのスケジュール設定手順を示した Platform UI のスクリーンショット。](../../assets/catalog/advertising/liveramp-onboarding/liveramp_scheduling_screenshot.png)

書き出すファイルの名前は現在、ユーザーが設定することはできません。[!DNL LiveRamp - Onboarding] 宛先に書き出すすべてのファイルは、次のテンプレートに基づいて自動的に名前が付けられます。

`%ORGANIZATION_NAME%_%DESTINATION%_%DESTINATION_INSTANCE_ID%_%DATETIME%`

![書き出すファイルの名前テンプレートを示す Platform UI のスクリーンショット。](../../assets/catalog/advertising/liveramp-onboarding/liveramp-file-name.png)

例えば、[!DNL Luma] という名前の組織の場合、書き出すファイルの名前は次のようになります。

```json
Luma_LiveRamp_52137231-4a99-442d-804c-39a09ddd005d_20230330_153857.csv
```

### 属性と ID のマッピング {#map}

**[!UICONTROL マッピング]**&#x200B;ステップでは、書き出すプロファイル属性および ID を選択できます。

>[!IMPORTANT]
>
>この宛先では、アクティブ化フローごとに 1 つのソース ID 名前空間のアクティブ化をサポートしています。`Email` や `Phone` など、複数の ID 名前空間を書き出す必要がある場合は、ID ごとに[個別のアクティブ化フローを作成](../../ui/activate-batch-profile-destinations.md)する必要があります。

**[!UICONTROL マッピング]**&#x200B;ステップでは、書き出された CSV ファイルの列ヘッダーの名前が&#x200B;**[!UICONTROL ターゲットフィールド]**&#x200B;マッピングで定義されます。**[!UICONTROL ターゲットフィールド]**&#x200B;のカスタム名を指定することで、書き出すファイルの CSV 列ヘッダーを任意のわかりやすい名前に変更できます。

>[!IMPORTANT]
>
>への最初のファイル配信後にターゲットフィールドに加えられた変更 [!DNL LiveRamp]、 [!DNL LiveRamp] アカウントチームまたは [LiveRamp サポートにチケットを送信](https://docs.liveramp.com/connect/en/considerations-when-uploading-the-first-file-to-an-audience.html#creating-a-support-case) 変更が自動化プロセスに反映されるようにします。

1. **[!UICONTROL マッピング]**&#x200B;手順で、「**[!UICONTROL 新しいマッピングを追加]**」を選択します。画面に新しいマッピング行が表示されます。

   ![マッピング画面を示す Experience Platform UI のスクリーンショット。](../../assets/catalog/advertising/liveramp-onboarding/liveramp-add-new-mapping.png)

2. **[!UICONTROL ソースフィールドを選択]**&#x200B;ウィンドウで、**[!UICONTROL 属性を選択]**&#x200B;カテゴリを選択し、マッピングする XDM 属性を選択するか、**[!UICONTROL ID 名前空間を選択]**&#x200B;カテゴリを選択して、宛先にマッピングする ID を選択します。

   ![ソースマッピング画面を示す Experience Platform UI のスクリーンショット。](../../assets/catalog/advertising/liveramp-onboarding/liveramp-source-mapping.png)

3. **[!UICONTROL ターゲットフィールドを選択]**&#x200B;ウィンドウで、選択したソースフィールドのマッピング先となる属性名を入力します。ここで定義した属性名が、書き出された CSV ファイルに列ヘッダーとして反映されます。

   ![ターゲットマッピング画面を示す Experience Platform UI のスクリーンショット。](../../assets/catalog/advertising/liveramp-onboarding/liveramp-target-mapping.png)

   また、**[!UICONTROL ターゲットフィールド]**&#x200B;に直接入力して属性名を入力することもできます。

   ![ターゲットマッピング画面を示す Experience Platform UI のスクリーンショット。](../../assets/catalog/advertising/liveramp-onboarding/liveramp-target-field.png)

必要なマッピングをすべて追加したら、「**[!UICONTROL 次へ]**」を選択してアクティブ化ワークフローを終了します。

## 書き出されたデータ／データ書き出しの検証 {#exported-data}

データは、設定した [!DNL LiveRamp - Onboarding] ストレージの場所に CSV ファイルとして書き出されます。

ファイルを [!DNL LiveRamp - Onboarding] 宛先に書き出す場合、Platform では[結合ポリシー ID](../../../profile/merge-policies/overview.md) ごとに 1 つの CSV ファイルを生成します。

例えば、次のオーディエンスについて考えてみましょう。

* オーディエンス A（結合ポリシー 1）
* オーディエンス B（結合ポリシー 2）
* オーディエンス C（結合ポリシー 1）
* オーディエンス D（結合ポリシー 1）

Platform では、次の 2 つの CSV ファイルを [!DNL LiveRamp - Onboarding] に書き出します。

* オーディエンス A、C、D を含む 1 つの CSV ファイル。
* オーディエンス B を含む 1 つの CSV ファイル。

書き出された CSV ファイルには、選択した属性と対応するオーディエンスステータスを持つプロファイルが、別々の列に、属性名と共に含まれています。 `audience_namespace:audience_ID` は、次の例に示すように、列ヘッダーとしてペア化されます。

`ATTRIBUTE_NAME, AUDIENCE_NAMESPACE_1:AUDIENCE_ID_1, AUDIENCE_NAMESPACE_2:AUDIENCE_ID_2,..., AUDIENCE_NAMESPACE_X:AUDIENCE_ID_X`

書き出されたファイルに含まれるプロファイルは、次のオーディエンス資格ステータスのいずれかに一致します。

* `Active`：プロファイルは現在、オーディエンスの資格を満たしています。
* `Expired`：プロファイルはオーディエンスの資格を失っていますが、過去に認定されています。
* `""`（空の文字列）：プロファイルは、オーディエンスに対して認定されませんでした。

例えば、書き出された CSV ファイル（1 つを含む） `email` 属性。Experience Platformからの 2 つのオーディエンス [セグメント化サービス](../../../segmentation/home.md)と 1 つ [インポート済み](../../../segmentation/ui/overview.md#importing-an-audience) 外部オーディエンスは次のようになります。

```csv
email,ups:aa2e3d98-974b-4f8b-9507-59f65b6442df,ups:45d4e762-6e57-4f2f-a3e0-2d1893bcdd7f,CustomerAudienceUpload:7729e537-4e42-418e-be3b-dce5e47aaa1e
abc117@testemailabc.com,active,,
abc111@testemailabc.com,,,active
abc102@testemailabc.com,,,active
abc116@testemailabc.com,active,,
abc107@testemailabc.com,active,expired,active
abc101@testemailabc.com,active,active,
```

上記の例では、 `ups:aa2e3d98-974b-4f8b-9507-59f65b6442df` および `ups:45d4e762-6e57-4f2f-a3e0-2d1893bcdd7f` の節では、セグメント化サービスからのオーディエンスについて説明し、 `CustomerAudienceUpload:7729e537-4e42-418e-be3b-dce5e47aaa1e` は、Platform as a にインポートされたオーディエンスを表します [カスタムアップロード](../../../segmentation/ui/overview.md#importing-an-audience).

Platform では[結合ポリシー ID](../../../profile/merge-policies/overview.md) ごとに 1 つの CSV ファイルを生成するので、結合ポリシー ID ごとに個別のデータフロー実行も生成します。

これは、 **[!UICONTROL アクティブ化された ID]** および **[!UICONTROL 受信したプロファイル]** 指標 [データフロー実行](../../../dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations) ページは、各オーディエンスに対して表示されるのではなく、同じ結合ポリシーを使用するオーディエンスの各グループに対して集計されます。

同じ結合ポリシーを使用するオーディエンスのグループに対してデータフローの実行が生成されるので、オーディエンス名は [監視ダッシュボード](../../../dataflows/ui/monitor-destinations.md#dataflow-runs-for-batch-destinations).

![「アクティブ化された ID」指標を示す Experience Platform UI のスクリーンショット。](../../assets/catalog/advertising/liveramp-onboarding/liveramp-metrics.png)

## 書き出されたデータの LiveRamp へのアップロード {#upload-to-liveramp}

データが [!DNL LiveRamp - Onboarding] ストレージに正常に書き出されたら、[!DNL LiveRamp] プラットフォームにデータをアップロードする必要があります。

ファイルを [!DNL LiveRamp - Onboarding] ストレージから [!DNL LiveRamp] オーディエンスにアップロードする方法について詳しくは、[最初のファイルをオーディエンスにアップロードする際の考慮事項](https://docs.liveramp.com/connect/en/considerations-when-uploading-the-first-file-to-an-audience.html#considerations-when-uploading-the-first-file-to-an-audience)のドキュメントを参照してください。

## データの使用とガバナンス {#data-usage-governance}

[!DNL Adobe Experience Platform] のすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。[!DNL Adobe Experience Platform] がどのように データガバナンスを実施するかについて詳しくは、[データガバナンスの概要](/help/data-governance/home.md)を参照してください。

## その他のリソース {#additional-resources}

の設定方法の詳細については、 [!DNL LiveRamp - Onboarding] ストレージ、「 [公式文書](https://docs.liveramp.com/connect/en/upload-a-file-via-liveramp-s-sftp.html).
