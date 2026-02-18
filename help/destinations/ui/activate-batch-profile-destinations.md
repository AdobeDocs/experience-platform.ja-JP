---
title: バッチプロファイルの書き出し宛先に対してオーディエンスをアクティブ化する
type: Tutorial
description: Adobe Experience Platformのオーディエンスをバッチプロファイルベースの宛先に送信してアクティブ化する方法を説明します。
exl-id: 82ca9971-2685-453a-9e45-2001f0337cda
source-git-commit: 99bac2ea71003b678a25b3afc10a68d36472bfbc
workflow-type: tm+mt
source-wordcount: '4578'
ht-degree: 39%

---


# バッチプロファイルの書き出し宛先に対してオーディエンスをアクティブ化する

>[!IMPORTANT]
> 
>* オーディエンスをアクティブ化し、ワークフローの [&#x200B; マッピングステップ &#x200B;](#mapping) を有効にするには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Destinations]**、**[!UICONTROL View Profiles]** および **[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。
>* ワークフローの [&#x200B; マッピングステップ &#x200B;](#mapping) を実行せずにオーディエンスをアクティブ化するには、**[!UICONTROL View Destinations]**、**[!UICONTROL Activate Segment without Mapping]**、**[!UICONTROL View Profiles]** および **[!UICONTROL View Segments]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。
>* *ID* を書き出すには、**[!UICONTROL View Identity Graph]** [&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。<br> ![&#x200B; 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択します。](/help/destinations/assets/overview/export-identities-to-destination.png " 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 "){width="100" zoomable="yes"}
> 
> 詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。

## 概要 {#overview}

この記事では、クラウドストレージやメールマーケティングの宛先など、バッチプロファイルファイルベースの宛先に対してAdobe Experience Platformでオーディエンスをアクティブ化するために必要なワークフローについて説明します。

## 前提条件 {#prerequisites}

宛先へのオーディエンスをアクティブ化するには、正常に [&#x200B; 宛先に接続 &#x200B;](./connect-destination.md) されている必要があります。 まだ接続していない場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照し、使用する宛先を設定します。

## 書き出しでサポートされているファイル形式 {#supported-file-formats-export}

オーディエンスを書き出す場合、次のファイル形式がサポートされます。

* CSV
* JSON
* PARQUET

CSV ファイルの書き出しでは、書き出したファイルの構造をより柔軟に指定できます。 詳しくは、[CSV ファイルのファイル形式設定 &#x200B;](/help/destinations/ui/batch-destinations-file-formatting-options.md#file-configuration) を参照してください。

[&#x200B; ファイルベースの宛先への接続を作成 &#x200B;](/help/destinations/ui/connect-destination.md) する際に、書き出すファイル形式を選択します。

## 宛先の選択 {#select-destination}

1. **[!UICONTROL Connections > Destinations]** に移動して、「**[!UICONTROL Catalog]**」タブを選択します。

   ![&#x200B; 「宛先のカタログ」タブへのアクセス方法をハイライト表示した画像。](../assets/ui/activate-batch-profile-destinations/catalog-tab.png)

1. 次の画像に示すように、オーディエンスをアクティベートする宛先に対応するカードで **[!UICONTROL Activate audiences]** を選択します。

   ![&#x200B; カタログページでハイライト表示されているオーディエンスコントロールをアクティブ化 &#x200B;](../assets/ui/activate-batch-profile-destinations/activate-audiences-button.png)

1. オーディエンスをアクティベートするために使用する宛先接続を選択し、「**[!UICONTROL Next]**」を選択します。

   ![&#x200B; オーディエンスをアクティブ化する 1 つまたは複数の宛先を選択するためにハイライト表示されたチェックボックス。](../assets/ui/activate-batch-profile-destinations/select-destination.png)

1. 次の節の「オーディエンスを選択 [&#x200B; に移動し &#x200B;](#select-audiences) す。

## オーディエンスを選択 {#select-audiences}

宛先に対してアクティブ化するオーディエンスを選択するには、オーディエンス名の左側にあるチェックボックスを使用したあと、「**[!UICONTROL Next]**」を選択します。

接触チャネルに応じて、複数のタイプのオーディエンスから選択できます。

* **[!UICONTROL Segmentation Service]**: Segmentation Service によってExperience Platform内で生成されたオーディエンス。 詳しくは、[&#x200B; セグメント化ドキュメント &#x200B;](../../segmentation/ui/overview.md) を参照してください。
* **[!UICONTROL Custom upload]**:Experience Platform以外で生成され、CSV ファイルとしてExperience Platformにアップロードされたオーディエンス。 外部オーディエンスについて詳しくは、[&#x200B; オーディエンスの読み込み &#x200B;](../../segmentation/ui/audience-portal.md#import-audience) に関するドキュメントを参照してください。 **[!UICONTROL Custom uploads]** のオーディエンスからオーディエンスを選択すると、「[&#x200B; エンリッチメント属性を選択 &#x200B;](#select-enrichment-attributes) 手順が自動的に有効になります。
* その他のタイプのオーディエンス。他のAdobe ソリューション（[!DNL Audience Manager] など）から派生します。

>[!IMPORTANT]
>
>バッチファイルベースの宛先に対してカスタムアップロードオーディエンスをアクティブ化する場合、データフローでアクティブ化できるオーディエンスは 10 個に制限されています。

![&#x200B; アクティブ化する 1 つ以上のオーディエンスを選択する際に表示されるチェックボックス。](../assets/ui/activate-batch-profile-destinations/select-audiences.png)

>[!TIP]
>
>既存のアクティベーションフローからオーディエンスを削除するには、**[!UICONTROL Activation data]** のページを使用します。 詳しくは、[&#x200B; アクティベーションフローから複数のオーディエンスを削除する &#x200B;](../ui/destination-details-page.md#bulk-remove) 方法に関する節を参照してください。

## オーディエンスの書き出しのスケジュール {#scheduling}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_schedule"
>title="スケジュール"
>abstract="鉛筆アイコンを使用して、ファイルの書き出しタイプ（完全なファイルまたは増分ファイル）と書き出し頻度を設定します。"

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_schedule_weekly_messaging"
>title="週次書き出し"
>abstract="<sup>*</sup> 開始日を選択すると、選択した終了日まで、その週のその日にその後の書き出しが実行されます。"

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_schedule_monthly_messaging"
>title="月次書き出し"
>abstract="<sup>*</sup> 開始日を選択すると、選択した終了日まで、その月のその日にその後の書き出しが実行されます。月の日数が 30 日または 31 日に満たない場合、月の最終日に書き出しが行われます。"

[!DNL Adobe Experience Platform] は、メールマーケティングおよびクラウドストレージの宛先のデータを [&#x200B; 様々なファイルタイプ &#x200B;](#supported-file-formats-export) として書き出します。 **[!UICONTROL Scheduling]** ページでは、書き出す各オーディエンスのスケジュールとファイル名を設定できます。

Experience Platformでは、各ファイル書き出しのデフォルトのスケジュールを自動的に設定します。 各スケジュールの横にある鉛筆アイコンを選択し、カスタムスケジュールを定義することで、必要に応じてデフォルトのスケジュールを変更できます。

![&#x200B; スケジュール設定ステップでハイライト表示されたスケジュール管理を編集 &#x200B;](../assets/ui/activate-batch-profile-destinations/edit-default-schedule.png)

複数のスケジュールを同時に編集するには、画面の左側にあるチェックボックスを使用してオーディエンスを選択してから、「**[!UICONTROL Edit schedule]**」を選択します。 設定したスケジュールは、選択したオーディエンスの書き出されたすべてのファイルに適用されます。

![&#x200B; 選択した複数のオーディエンスのスケジュールを編集オプションを示すExperience Platform ユーザーインターフェイスの画像 &#x200B;](../assets/ui/activate-batch-profile-destinations/edit-schedule.png)

>[!TIP]
>
>既存のアクティベーションフローのオーディエンスアクティベーションスケジュールは、**[!UICONTROL Activation data]** のページから編集できます。 詳しくは、[&#x200B; アクティブ化スケジュールの一括編集 &#x200B;](../ui/destination-details-page.md#bulk-edit-schedule) に関するドキュメントを参照してください。

>[!IMPORTANT]
>
>[!DNL Adobe Experience Platform] は、書き出しファイルを1 ファイルあたり 500 万件のレコード（行）で自動的に分割します。各行は 1 つのプロファイルを表します。
>
>`filename.csv`、`filename_2.csv`、`filename_3.csv` のように、分割ファイル名には、ファイルが大きな書き出しの一部であることを示す数字が付加されます。

### 完全ファイルを書き出し {#export-full-files}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_exportoptions"
>title="ファイルの書き出しオプション"
>abstract="「**完全ファイルを書き出し**」を選択して、オーディエンスに該当するすべてのプロファイルの完全なスナップショットを書き出します。「**増分ファイルの書き出し**」を選択して、前回の書き出し以降にオーディエンスの対象として認定されたプロファイルのみを書き出します。<br> 最初の増分ファイルの書き出しには、オーディエンスに該当するすべてのプロファイルが含まれ、バックフィルとして機能します。それ以後の増分ファイルには、最初の増分ファイル書き出し以降にオーディエンスに該当したプロファイルのみが含まれます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html?lang=ja#export-incremental-files" text="増分ファイルの書き出し"

>[!CONTEXTUALHELP]
>id="platform_destinations_activationchaining_aftersegmentevaluation"
>title="オーディエンス評価後にアクティベート"
>abstract="<p>アクティベーションは、毎日のセグメント化ジョブが完了した直後に実行されます。 これにより、最新のプロファイルが確実に書き出されます。</p><p>オーディエンスの評価後にプロファイルを書き出すオプションは、週次および月次の書き出し頻度では使用<i>できません</i>。</p>"

>[!CONTEXTUALHELP]
>id="platform_destinations_activationchaining_scheduled"
>title="スケジュールされたアクティベーション"
>abstract="アクティベーションが特定の時刻に実行されます。"

「**[!UICONTROL Export full files]**」を選択すると、選択したオーディエンスに対するすべてのプロファイル選定の完全なスナップショットを含むファイルの書き出しがトリガーになります。

![&#x200B; 「完全なファイルを書き出し」切替スイッチが選択されています。](../assets/ui/activate-batch-profile-destinations/export-full-files.png)

1. **[!UICONTROL Frequency]** セレクターを使用して、書き出しの頻度を選択します。

   * **[!UICONTROL Once]**：オンデマンドによる 1 回限りの完全ファイルの書き出しをスケジュールします。
   * **[!UICONTROL Daily]**：指定した時刻に、毎日 1 回、完全ファイルの書き出しをスケジュールします。
   * **[!UICONTROL Weekly]**：開始日を選択すると、選択した終了日まで、その週の当日に後続のエクスポートが発生します。
   * **[!UICONTROL Monthly]**：開始日を選択すると、選択した終了日まで、その月の日付に後続のエクスポートが発生します。 月の日数が 30 日または 31 日に満たない場合、月の最終日に書き出しが行われます。

   >[!NOTE]
   >
   > 現在、毎週および毎月のスケジュールオプションは、次のファイルベースのクラウドストレージの宛先に対してのみ、および [&#x200B; 人物オーディエンス &#x200B;](../../segmentation/types/overview.md#people-audience) および [&#x200B; 見込み客オーディエンス &#x200B;](../../segmentation/types/overview.md#prospect-audience) をアクティブ化する場合にのみサポートされています。
   > 
   > * [Amazon S3](../catalog/cloud-storage/amazon-s3.md)
   > * [Azure Blob Storage](../catalog/cloud-storage/azure-blob.md)
   > * [Data Landing Zone](../catalog/cloud-storage/data-landing-zone.md)
   > * [Google Cloud Storage](../catalog/cloud-storage/google-cloud-storage.md)
   > * [SFTP](../catalog/cloud-storage/sftp.md)
   > 
   > 毎週および毎月のスケジュールオプションは、他の宛先タイプでは使用できません。

2. **[!UICONTROL Time]** の切り替えスイッチを使用して、書き出しをオーディエンス評価直後に実行するか、指定した時間にスケジュールに沿って実行するかを選択します。 「**[!UICONTROL Scheduled]**」オプションを選択すると、セレクターを使用して書き出しを実行する時刻を [!DNL UTC] 形式で選択できます。

   **[!UICONTROL After segment evaluation]** オプションを使用して、毎日のExperience Platform バッチセグメント化ジョブが完了した直後にアクティベーションジョブをを実行します。 このオプションを使用すると、アクティベーションジョブが実行されたときに、最新のプロファイルが確実に宛先に書き出されます。 これにより、オーディエンスが、アクションに基づいて 1 日に複数回書き出される可能性があります。

   >[!IMPORTANT]
   >
   >セグメント評価後にアクティベートするように既に設定されているオーディエンスに対して [&#x200B; 柔軟なオーディエンス評価 &#x200B;](../../segmentation/ui/audience-portal.md#flexible-audience-evaluation) を実行した場合、オーディエンスは、以前の日別アクティベーションジョブに関係なく、柔軟なオーディエンス評価ジョブが終了するとすぐにアクティベートされます。 これにより、オーディエンスが、アクションに基づいて 1 日に複数回エクスポートされる可能性があります。

   <!-- Batch segmentation currently runs at {{insert time of day}} and lasts for an average {{x hours}}. Adobe reserves the right to modify this schedule. -->

   ![バッチ宛先のアクティベーションフローでの「セグメントの評価後」オプションを強調表示した画像。](../assets/ui/activate-batch-profile-destinations/after-segment-evaluation-option.png)
**[!UICONTROL Scheduled]** オプションを使用して、特定の時間にアクティベーションジョブを実行します。 このオプションを選択すると、Experience Platform プロファイルデータが毎日同じ時間に書き出されます。 ただし、アクティベーションジョブが開始される前にバッチセグメント化ジョブが完了しているかどうかに応じて、書き出すプロファイルが最新でない場合があります。

   ![バッチ宛先のアクティベーションフローの「スケジュール済み」オプションが強調表示され、時間セレクターが表示されている画像。](../assets/ui/activate-batch-profile-destinations/scheduled-option.png)

   過去 24 時間以内に作成され、[&#x200B; バッチセグメント化 &#x200B;](../../segmentation/methods/batch-segmentation.md) によって評価されたオーディエンスをマッピングする場合、毎日の書き出しスケジュールを、できるだけ早く、次の日から開始するように設定します。 これにより、毎日のバッチ評価ジョブが最初に実行され、完全なオーディエンスデータを書き出します。

   エクスポートスケジュールを設定する場合は、アクティベーションフローが完了してから少なくとも **1 時間** 開始時刻を設定します。 オーディエンスのアクティベーションがシステム全体に反映されるまで、最大 1 時間かかる場合があります。 アクティブ化から 1 時間以内に書き出しを実行するようにスケジュールすると、スケジュールされた書き出しが実行されない場合があります。

3. **[!UICONTROL Date]** セレクターを使用して、書き出しを実行する日または間隔を選択します。 日常の書き出しでのベストプラクティスは、開始日と終了日を、ダウンストリームプラットフォームのキャンペーンの期間に合わせて設定することです。

   >[!IMPORTANT]
   >
   > 書き出し間隔を選択する場合、その間隔の最終日は書き出しに含まれません。例えば、1 月 4 日から 11 日までの間隔を選択した場合、最後のファイルエクスポートは 1 月 10 日に実行されます。

4. 「**[!UICONTROL Create]**」を選択して、スケジュールを保存します。

### 増分ファイルの書き出し

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_something"
>title="ファイル名の設定"
>abstract="ファイルベースの宛先の場合、オーディエンスごとに一意のファイル名が生成されます。ファイル名エディターを使用して、一意のファイル名を作成および編集するか、デフォルトの名前のままにします。"

「**[!UICONTROL Export incremental files]**」を選択して、エクスポートをトリガーします。最初のファイルは、選択したオーディエンスに対するすべてのプロファイル選定の完全なスナップショットであり、それ以降のファイルは、前回のエクスポート以降の増分プロファイル選定です。

>[!IMPORTANT]
>
>最初にエクスポートされた増分ファイルには、オーディエンスの対象となるすべてのプロファイルが含まれ、バックフィルとして機能します。

![&#x200B; 「増分ファイルの書き出し」切替スイッチが選択されている &#x200B;](../assets/ui/activate-batch-profile-destinations/export-incremental-files.png)。

1. **[!UICONTROL Frequency]** セレクターを使用して、書き出しの頻度を選択します。

   * **[!UICONTROL Daily]**：増分ファイルの書き出しを、毎日 1 回指定した時刻にスケジュールします。
   * **[!UICONTROL Hourly]**：増分ファイルの書き出しを、3 時間、6 時間、8 時間または 12 時間ごとにスケジュールします。


2. **[!UICONTROL Time]** セレクターを使用して、ファイルが書き出され [!DNL UTC] 時刻を形式を指定します。

3. **[!UICONTROL Date]** セレクターを使用して、書き出しが行われる間隔を選択します。 ベストプラクティスは、開始日と終了日を、ダウンストリームプラットフォームのキャンペーンの期間に合わせて設定することです。

   >[!IMPORTANT]
   >
   >間隔の最終日はエクスポートに含まれません。例えば、1 月 4 日から 11 日までの間隔を選択した場合、最後のファイルエクスポートは 1 月 10 日に実行されます。

4. 「**[!UICONTROL Create]**」を選択して、スケジュールを保存します。

### ファイル名の設定 {#configure-file-names}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_filename"
>title="ファイル名の設定"
>abstract="ファイルベースの宛先の場合、オーディエンスごとに一意のファイル名が生成されます。ファイル名エディターを使用して、一意のファイル名を作成および編集するか、デフォルトの名前のままにします。"

ほとんどの宛先では、デフォルトのファイル名は、宛先名、オーディエンス ID、日時インジケーターで構成されます。 例えば、書き出したファイル名を編集して、異なるキャンペーンを区別したり、データの書き出し時間をファイルに追加したりできます。一部の宛先開発者は、別のデフォルトのファイル名追加オプションを宛先に表示するように選択する場合があります。

モーダルウィンドウを開き、ファイル名を編集するには、鉛筆アイコンを選択します。 ファイル名は 255 文字までに制限されています。

>[!NOTE]
>
>次の画像は [!DNL Amazon S3] の宛先のファイル名を編集する方法を示していますが、プロセスはすべてのバッチ宛先 （SFTP、[!DNL Azure Blob Storage] または [!DNL Google Cloud Storage] など）で同じです。

![ファイル名の設定に使用される鉛筆アイコンをハイライト表示する画像。](../assets/ui/activate-batch-profile-destinations/configure-name.png)

ファイル名エディターで、別のコンポーネントを選択してファイル名に追加できます。

![使用可能なすべてのファイル名オプションを示す画像。](../assets/ui/activate-batch-profile-destinations/activate-workflow-configure-step-2.png)

宛先名とオーディエンス ID をファイル名から削除することはできません。 これらのオプションに加えて、次のオプションを追加できます。

| ファイル名オプション | 説明 |
|---------|----------|
| **[!UICONTROL Audience name]** | 書き出されたオーディエンスの名前。 |
| **[!UICONTROL Date and time]** | ファイルを生成した時刻のタイムスタンプを、`MMDDYYYY_HHMMSS` 形式で追加するか、UNIX の 10 桁で追加するかを選択します。 増分書き出しのたびにファイル名を動的に生成したい場合は、次のオプションのいずれかを選択します。 |
| **[!UICONTROL Custom text]** | ファイル名に追加する任意のカスタムテキスト。 |
| **[!UICONTROL Destination ID]** | オーディエンスの書き出しに使用する宛先データフローの ID。 |
| **[!UICONTROL Destination name]** | オーディエンスの書き出しに使用する宛先データフローの名前。 |
| **[!UICONTROL Organization name]** | Experience Platform内の組織名。 |
| **[!UICONTROL Sandbox name]** | オーディエンスの書き出しに使用するサンドボックスの ID。 |

{style="table-layout:auto"}

複数のファイル名を同時に編集するには、画面の左側にあるチェックボックスを使用してオーディエンスを選択してから、「オーディ **[!UICONTROL Edit file name]** ンス」を選択します。 設定するファイル名オプションは、選択したオーディエンスの書き出されたすべてのファイルに適用されます。

![&#x200B; 選択した複数のオーディエンスのファイル名を編集オプションを示すExperience Platform ユーザーインターフェイスの画像 &#x200B;](../assets/ui/activate-batch-profile-destinations/edit-file-name.png)

「**[!UICONTROL Apply changes]**」を選択して、選択を確定します。

>[!IMPORTANT]
> 
>**[!UICONTROL Date and Time]** コンポーネントを選択しない場合、ファイル名は固定され、新しく書き出されたファイルは、書き出しのたびに保存場所にある以前のファイルを上書きします。 ストレージの場所からメールマーケティングプラットフォームへのインポートジョブを繰り返し実行する場合は、このオプションをお勧めします。

すべてのオーディエンスの設定が完了したら、「**[!UICONTROL Next]**」を選択して続行します。

## マッピング {#mapping}

この手順では、ターゲットの宛先に書き出すファイルに追加する、プロファイル属性を選択する必要があります。 書き出すプロファイル属性と ID を選択するには：

1. **[!UICONTROL Mapping]** ページで「**[!UICONTROL Add new mapping]**」を選択します。

   ![マッピングワークフローで強調表示されている新しいフィールドコントロールを追加します。](../assets/ui/activate-batch-profile-destinations/add-new-field-mapping.png)

1. **[!UICONTROL Source field]** エントリの右側の矢印を選択します。

   ![マッピングワークフローでハイライト表示されているソースフィールドコントロールを選択します。](../assets/ui/activate-batch-profile-destinations/select-source-field.png)

1. **[!UICONTROL Select source field]** ページで、宛先へ書き出したファイルに含めるプロファイル属性と ID を選択してから、「**[!UICONTROL Select]**」を選択します。

   >[!TIP]
   > 
   >次の画像に示すように、検索フィールドを使用して、選択を絞り込むことができます。

   **[!UICONTROL Show only fields with data]** 切替スイッチを使用すると、値が入力されたスキーマフィールドのみを表示できます。 デフォルトでは、入力されたスキーマフィールドのみが表示されます。

   ![宛先に書き出すことができるプロファイル属性を示す、モーダルウィンドウ。](../assets/ui/activate-batch-profile-destinations/select-source-field-modal.png)

   **[!UICONTROL Show display names for fields]** トグルを使用して、スキーマフィールド名の代わりに、フィールドのわかりやすい名前を表示します。

   ![&#x200B; 表示名の切り替えを表示するソースフィールドを選択 &#x200B;](../assets/ui/activate-batch-profile-destinations/show-display-names.gif)

1. 書き出し用に選択したフィールドがマッピングビューに表示されます。 必要に応じて、書き出したファイルのヘッダーの名前を編集できます。 それには、ターゲットフィールドでアイコンを選択します。

   >[!NOTE]
   >
   >書き出されたファイルのフィールド名では、ドット（`.`）はサポートされていません。 フィールド名にドット（`person.name.firstName` など）が含まれる場合、書き出された列名の各ドットはアンダースコア（`_`）に置き換えられます。 例えば、`person.name.firstName` は書き出されたファイルで `person_name_firstName` になります。

   ![宛先に書き出すことができるプロファイル属性を示す、モーダルウィンドウ。](../assets/ui/activate-batch-profile-destinations/mapping-step-select-target-field.png)

1. **[!UICONTROL Select target field]** ページで、書き出したファイルでヘッダーに目的の名前を入力し、「**[!UICONTROL Select]**」を選択します。

   ![ヘッダーに対して入力されたわかりやすい名前を示すモーダルウィンドウ。](../assets/ui/activate-batch-profile-destinations/select-target-field-mapping.png)

1. 書き出し用に選択したフィールドがマッピングビューに表示され、編集したヘッダーが書き出したファイルに表示されます。

   ![宛先に書き出すことができるプロファイル属性を示すモーダルウィンドウ。](../assets/ui/activate-batch-profile-destinations/select-target-field-updated.png)

1. （オプション） UI でマッピングされたフィールドの順序は、書き出された CSV ファイルの列の順序で（上から下に）反映されます。一番上の行は CSV ファイルの左端の列です。 以下に示すように、マッピングされた行をドラッグ&amp;ドロップすることで、マッピングされたフィールドを任意の方法で並べ替えることができます。

   >[!NOTE]
   >
   >この機能はベータ版で、一部のお客様のみご利用いただけます。 この機能へのアクセスをリクエストするには、Adobe担当者にお問い合わせください。

   ![&#x200B; ドラッグ&amp;ドロップでマッピングフィールドの並べ替えを示す録画 &#x200B;](../assets/ui/activate-batch-profile-destinations/reorder-fields.gif)

1. （オプション）書き出されたフィールドを[必須キー](#mandatory-keys)または [重複排除キー](#deduplication-keys)のどちらにするかを選択できます。

   ![宛先に書き出すことができるプロファイル属性を示すモーダルウィンドウ。](../assets/ui/activate-batch-profile-destinations/select-mandatory-deduplication-key.png)

1. 書き出すフィールドをさらに追加するには、上記の手順を繰り返します。

### 必須の属性 {#mandatory-attributes}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_mandatorykey"
>title="必須の属性について"
>abstract="書き出されたすべてのプロファイルに含める XDM スキーマ属性を選択します。必須キーのないプロファイルは、宛先に書き出されません。必須キーを選択しない場合、属性に関係なく、すべての該当プロファイルを書き出します。"

必須属性は、選択した属性がすべてのプロファイルレコードに含まれるようにする、ユーザーが有効にしたチェックボックスです。例：書き出されるすべてのプロファイルには、メールアドレスが含まれます。

[!DNL Experience Platform] が特定の属性を含むプロファイルのみを書き出すよう、属性を必須としてマークすることができます。その結果、追加のフィルタリング形式として使用できます。属性を必須としてマークする必要は&#x200B;**ありません**。

必須属性を選択しない場合、属性に関係なく、すべての対象プロファイルを書き出します。

属性の 1 つをスキーマの[一意の ID](../../destinations/catalog/email-marketing/overview.md#identity) にすることをお勧めします。必須属性について詳しくは、[メールマーケティングの宛先](../../destinations/catalog/email-marketing/overview.md#identity)ドキュメントの ID の節を参照してください。

### 重複排除キー {#deduplication-keys}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_deduplicationkey"
>title="重複排除キーについて"
>abstract="重複排除キーを選択することで、書き出しファイル内にある、同一プロファイルの複数のレコードを排除します。重複排除キーとして、1 つの名前空間または最大 2 つの XDM スキーマ属性を選択できます。重複排除キーを選択しないと、書き出しファイルでプロファイルエントリの重複が発生する場合があります。"

重複排除キーは、ユーザーがプロファイルの重複排除する ID を決定する、ユーザー定義のプライマリキーです。

重複排除キーを使用すると、同一プロファイルの複数のレコードが 1 つの書き出しファイルに含まれる可能性がなくなります。

[!DNL Experience Platform] で重複排除キーを使用する方法は 3 つあります。

* [!UICONTROL deduplication key] として単一の ID 名前空間を使用
* [!DNL XDM] として、[!UICONTROL deduplication key] プロファイルから単一のプロファイル属性を使用する
* 複合キーとして、[!DNL XDM] プロファイルから 2 つのプロファイル属性の組み合わせを使用する

>[!IMPORTANT]
>
> 単一の ID 名前空間を宛先に書き出すことができ、名前空間は自動的に重複排除キーとして設定されます。複数の名前空間を宛先に送信することはサポートされていません。
> 
> ID 名前空間とプロファイル属性の組み合わせを重複排除キーとして使用することはできません。

### 重複排除の例 {#deduplication-example}

次の例は、選択した重複排除キーに応じた重複排除の仕組みを示しています。

次の 2 つのプロファイルについて考えてみましょう。

**プロファイル A**

```json
{
  "identityMap": {
    "Email": [
      {
        "id": "johndoe@example.com"
      },
      {
        "id": "doejohn_1@example.com"
      }
    ]
  },
  "segmentMembership": {
    "ups": {
      "fa5c4622-6847-4199-8dd4-8b7c7c7ed1d6": {
        "status": "realized",
        "lastQualificationTime": "2021-03-10 10:03:08"
      }
    }
  },
  "person": {
    "name": {
      "lastName": "Doe",
      "firstName": "John"
    }
  },
  "personalEmail": {
    "address": "johndoe@example.com"
  }
}
```

**プロファイル B**

```json
{
  "identityMap": {
    "Email": [
      {
        "id": "johndoe@example.com"
      },
      {
        "id": "doejohn_2@example.com"
      }
    ]
  },
  "segmentMembership": {
    "ups": {
      "fa5c4622-6847-4199-8dd4-8b7c7c7ed1d6": {
        "status": "realized",
        "lastQualificationTime": "2021-04-10 11:33:28"
      }
    }
  },
  "person": {
    "name": {
      "lastName": "D",
      "firstName": "John"
    }
  },
  "personalEmail": {
    "address": "johndoe@example.com"
  }
}
```

### 重複排除の使用例 1：重複排除なし {#deduplication-use-case-1}

重複排除を使用しない場合、書き出しファイルには次のエントリが含まれます。

| personalEmail | firstName | lastName |
|---|---|---|
| johndoe@example.com | John | Doe |
| johndoe@example.com | John | D |


### 重複排除の使用例 2：ID 名前空間に基づく重複排除 {#deduplication-use-case-2}

[!DNL Email] 名前空間の重複排除を指定した場合、書き出しファイルには次のエントリが含まれます。プロファイル B はオーディエンスに適合する最新のプロファイルなので、唯一書き出されます。

| メール* | personalEmail | firstName | lastName |
|---|---|---|---|
| johndoe@example.com | johndoe@example.com | John | D |
| doejohn_2@example.com | johndoe@example.com | John | D |

### 重複排除のユースケース 3：単一のプロファイル属性に基づく重複排除 {#deduplication-use-case-3}

`personal Email` 属性による重複排除を仮定した場合、エクスポートファイルには次のエントリが含まれます。プロファイル B はオーディエンスに適合する最新のプロファイルなので、唯一書き出されます。

| personalEmail* | firstName | lastName |
|---|---|---|
| johndoe@example.com | John | D |


### 重複排除のユースケース 4：2 つのプロファイル属性に基づく重複排除 {#deduplication-use-case-4}

複合キー `personalEmail + lastName` による重複排除を仮定した場合、エクスポートファイルには次のエントリが含まれます。

| personalEmail* | lastName* | firstName |
|---|---|---|
| johndoe@example.com | D | John |
| johndoe@example.com | Doe | John |

[!DNL CRM ID] やメールアドレスなどの ID 名前空間を重複排除キーとして選択して、すべてのプロファイルレコードが必ず一意に識別されるようにすることをお勧めします。

### 同じタイムスタンプを持つプロファイルの重複排除動作 {#deduplication-same-timestamp}

ファイルベースの宛先にプロファイルを書き出す場合、重複排除では、複数のプロファイルが同じ重複排除キーと同じ参照タイムスタンプを共有している場合に、1 つのプロファイルのみが書き出されるようにします。 このタイムスタンプは、プロファイルのオーディエンスメンバーシップまたは ID グラフが最後に更新された瞬間を表します。 プロファイルの更新および書き出し方法について詳しくは、[&#x200B; プロファイルの書き出し動作 &#x200B;](https://experienceleague.adobe.com/en/docs/experience-platform/destinations/how-destinations-work/profile-export-behavior#what-determines-a-data-export-and-what-is-included-in-the-export-2) ドキュメントを参照してください。

#### 主な考慮事項

* **決定論的選択**：複数のプロファイルの重複排除キーと参照タイムスタンプが同じ場合、重複排除ロジックは、選択した他の列（配列、マップ、オブジェクトなどの複雑なタイプを除く）の値を並べ替えて、書き出すプロファイルを決定します。 ソートされた値は辞書順に評価され、最初のプロファイルが選択されます。

* **サンプルシナリオ**

次のデータについて考えてみます。重複排除キーは `Email` の列です。

| メール* | first_name | last_name | タイムスタンプ |
|---|---|---|---|  
| `test1@test.com` | John | モリス | 2024-10-12T09:50 |
| `test1@test.com` | John | Doe | 2024-10-12T09:50 |
| `test2@test.com` | Frank | Smith | 2024-10-12T09:50 |

{style="table-layout:auto"}

重複排除後、書き出しファイルには次の内容が含まれます。

| メール* | first_name | last_name | タイムスタンプ |
|---|---|---|---|  
| `test1@test.com` | John | Doe | 2024-10-12T09:50 |
| `test2@test.com` | Frank | Smith | 2024-10-12T09:50 |

{style="table-layout:auto"}

**説明**:`test1@test.com` の場合、両方のプロファイルで同じ重複排除キーとタイムスタンプが共有されます。 アルゴリズムは、`first_name` と `last_name` の列の値を辞書順に並べ替えます。 名が同じであるため、タイは `last_name` 列を使用して解決されます。ここで、「Doe」は「Morris」の前にあります。

**信頼性の向上**：この更新された重複排除プロセスにより、同じ座標を持つ連続する実行で常に同じ結果が得られ、一貫性が向上します。

### 計算フィールドを使用したデータ変換の実行 {#calculated-fields}

[&#x200B; 計算フィールド &#x200B;](/help/destinations/ui/data-transformations-calculated-fields.md) コントロールを使用して、ファイルベースの宛先に書き出されたデータに対して様々なデータ変換を実行できます。

### 既知の制限事項 {#known-limitations}

新しい **[!UICONTROL Mapping]** ページには次の既知の制限事項があります。

#### マッピングワークフローでは、オーディエンスメンバーシップの属性を選択できません

既知の制限により、現在、**[!UICONTROL Select field]** ウィンドウを使用してファイルの書き出しに `segmentMembership.seg_namespace.seg_id.status` を追加することはできません。 代わりに、手動で値 `xdm: segmentMembership.seg_namespace.seg_id.status` をスキーマフィールドに貼り付ける必要があります（下図を参照）。

![&#x200B; アクティベーションワークフローのマッピング手順でオーディエンスメンバーシップの回避策を示す画面録画。](../assets/ui/activate-batch-profile-destinations/segment-membership-mapping-step.gif)


>[!NOTE]
>
>クラウドストレージの宛先の場合、デフォルトでマッピングに追加される属性は次のとおりです。
>
>* `segmentMembership.seg_namespace.seg_id.status`
>* `segmentMembership.seg_namespace.seg_id.lastQualificationTime`

ファイルのエクスポートは、`segmentMembership.seg_namespace.seg_id.status` が選択されているかどうかによって、次のように異なります。

* `segmentMembership.seg_namespace.seg_id.status` フィールドを選択した場合、エクスポートされたファイルには、最初の完全スナップショットでは **[!UICONTROL Active]** メンバーが含まれ、その後の増分エクスポートでは新しく **[!UICONTROL Active]** 成および **[!UICONTROL Expired]** メンバーが含まれます。
* `segmentMembership.seg_namespace.seg_id.status` フィールドを選択しない場合、エクスポートされたファイルには、最初の完全スナップショットとその後の増分エクスポートで、**[!UICONTROL Active]** メンバーのみが含まれます。

詳しくは、[&#x200B; ファイルベースの宛先のプロファイル書き出し動作 &#x200B;](/help/destinations/how-destinations-work/profile-export-behavior.md#file-based-destinations) を参照してください。

#### ID 名前空間は現在、書き出し用に選択できません

以下の画像に示すように、ID 名前空間を書き出し用に選択する機能は、現在サポートされていません。 書き出し用の ID 名前空間を選択すると、**[!UICONTROL Review]** の手順でエラーが発生します。

![ID の書き出しを示す、サポートされていないマッピング。](../assets/ui/activate-batch-profile-destinations/unsupported-identity-mapping.png)

ベータ版の間に書き出したファイルに ID 名前空間を追加する必要がある場合の一時的な回避策として、次のいずれかを実行できます。

* 書き出しに ID 名前空間を含めるデータフローに、従来のクラウドストレージの宛先を使用する
* ID を属性として Experience Platform にアップロードし、クラウドストレージの宛先に書き出します。

## プロファイル属性の選択 {#select-attributes}

>[!IMPORTANT]
> 
>カタログ内のすべてのクラウドストレージの宛先では [[!UICONTROL Mapping] 改善された &#x200B;](#mapping) ールステップを表示できます。これは、この節で説明する **[!UICONTROL Select attributes]** のステップに代わるものです。
>
>この **[!UICONTROL Select attributes]** ステップは、Adobe Campaign、Oracle Responsys、Oracle Eloqua およびSalesforce Marketing Cloudのメールマーケティングの宛先に対しても引き続き表示されます。

プロファイルベースの宛先の場合、ターゲット宛先に送信するプロファイル属性を選択する必要があります。

1. **[!UICONTROL Select attributes]** ページで「**[!UICONTROL Add new field]**」を選択します。

   ![「新しいフィールドを追加」ボタンを強調表示した画像。](../assets/ui/activate-batch-profile-destinations/add-new-field.png)

2. **[!UICONTROL Schema field]** エントリの右側の矢印を選択します。

   ![ソースフィールドの選択方法をハイライト表示した画像。](../assets/ui/activate-batch-profile-destinations/select-source-field.png)

3. **[!UICONTROL Select field]** ページで、宛先に送信する XDM 属性または ID 名前空間を選択してから「**[!UICONTROL Select]**」を選択します。

   ![ソースフィールドとして使用可能な様々なフィールドを示す画像。](../assets/ui/activate-batch-profile-destinations/target-field-page.png)

4. さらにマッピングを追加するには、手順 1～3 を繰り返します。

>[!NOTE]
>
> Adobe Experience Platform は、スキーマから推奨される一般的に使用される属性 4 つ（`person.name.firstName`、`person.name.lastName`、`personalEmail.address`、`segmentMembership.seg_namespace.seg_id.status`）を事前に選択します。

![Audience Activation ワークフローのマッピング手順で事前入力された推奨属性を示す画像 &#x200B;](../assets/ui/activate-batch-profile-destinations/prefilled-fields.png)

>[!IMPORTANT]
>
>既知の制限により、現在、**[!UICONTROL Select field]** ウィンドウを使用してファイルの書き出しに `segmentMembership.seg_namespace.seg_id.status` を追加することはできません。 代わりに、手動で値 `xdm: segmentMembership.seg_namespace.seg_id.status` をスキーマフィールドに貼り付ける必要があります（下図を参照）。
>
>![&#x200B; アクティベーションワークフローのマッピング手順でオーディエンスメンバーシップの回避策を示す画面録画。](../assets/ui/activate-batch-profile-destinations/segment-membership.gif)

ファイルのエクスポートは、`segmentMembership.seg_namespace.seg_id.status` が選択されているかどうかによって、次のように異なります。

* `segmentMembership.seg_namespace.seg_id.status` フィールドを選択した場合、エクスポートされたファイルには、最初の完全スナップショットでは **[!UICONTROL Active]** メンバーが含まれ、その後の増分エクスポートでは **[!UICONTROL Active]** メンバーと **[!UICONTROL Expired]** メンバーが含まれます。
* `segmentMembership.seg_namespace.seg_id.status` フィールドを選択しない場合、エクスポートされたファイルには、最初の完全スナップショットとその後の増分エクスポートで、**[!UICONTROL Active]** メンバーのみが含まれます。

## エンリッチメント属性を選択 {#select-enrichment-attributes}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_exclude_enrichment_attributes"
>title="エンリッチメント属性の除外"
>abstract="このオプションを有効にすると、選択したカスタムアップロードオーディエンスのプロファイルを宛先に書き出すことはできますが、その属性はすべて除外されます。"

>[!IMPORTANT]
>
>この手順は、**[!UICONTROL Custom upload]** オーディエンスの選択 [&#x200B; 手順で &#x200B;](#select-audiences) オーディエンスを選択した場合にのみ表示されます。

エンリッチメント属性は、Experience Platform as **[!UICONTROL Custom uploads]** に取り込まれた、カスタムでアップロードされたオーディエンスに対応します。 この手順では、選択した外部オーディエンスごとに、宛先に書き出す属性を選択できます。

![&#x200B; エンリッチメント属性選択ステップを示す UI 画像。](../assets/ui/activate-batch-profile-destinations/select-enrichment-attributes-step.png)

各外部オーディエンスのエンリッチメント属性を選択するには、次の手順に従います。

1. **[!UICONTROL Enrichment attributes]** 列で、「![&#x200B; 編集ボタン &#x200B;](/help/images/icons/edit.png) （編集）」ボタンを選択します。
1. 「**[!UICONTROL Add enrichment attribute]**」を選択します。新しい空のスキーマフィールドが表示されます。
   ![&#x200B; エンリッチメント属性モーダル画面を示す UI 画像。](../assets/ui/activate-batch-profile-destinations/add-enrichment-attribute.png)
1. 空のフィールドの右側にあるボタンを選択して、フィールド選択画面を開きます。
1. オーディエンス用に書き出す属性を選択します。
   ![&#x200B; エンリッチメント属性リストを示す UI 画像。](../assets/ui/activate-batch-profile-destinations/select-enrichment-attributes.png)
1. 書き出す属性をすべて追加したら、「**[!UICONTROL Save and close]**」を選択します。
1. 外部オーディエンスごとに、これらの手順を繰り返します。

属性を書き出さずに宛先に対して外部オーディエンスをアクティブ化する場合は、「**[!UICONTROL Exclude enrichment attributes]**」切替スイッチを有効にします。 このオプションでは、外部オーディエンスからプロファイルが書き出されますが、対応する属性は宛先に送信されません。

![&#x200B; 「エンリッチメント属性を除外」切替スイッチを示す UI 画像。](../assets/ui/activate-batch-profile-destinations/exclude-enrichment-attributes.png)

**[!UICONTROL Next]** を選択して、[&#x200B; レビュー &#x200B;](#review) ステップに移動します。

## レビュー {#review}

>[!NOTE]
> 
>（データセット全体ではなく）データセット内の特定のフィールドのみに適用されたデータ使用ラベルがある場合、アクティベーション時のこれらのフィールドレベルラベルの適用は、次の条件で行われます。
>
>* フィールドは、オーディエンス定義で使用されます。
>* フィールドは、ターゲット先の予測属性として設定されます。
>
> 例えば、フィールド `person.name.firstName` に宛先のマーケティングアクションと競合する特定のデータ使用ラベルがある場合、レビュー手順でデータ使用ポリシー違反が表示されます。詳しくは、[Adobe Experience Platform でのデータガバナンス](../../rtcdp/privacy/data-governance-overview.md#destinations)を参照してください。

**[!UICONTROL Review]** のページには、選択内容の概要が表示されます。 **[!UICONTROL Cancel]** を選択してフローを中断する **[!UICONTROL Back]**、設定を変更する、または **[!UICONTROL Finish]** を選択して選択内容を確認し、宛先へのデータの送信を開始します。

![&#x200B; レビュー手順に表示される選択の概要。](../assets/ui/activate-batch-profile-destinations/review.png)

### 同意ポリシーの評価 {#consent-policy-evaluation}

>[!CONTEXTUALHELP]
>id="platform_governance_policies_viewApplicableConsentPolicies"
>title="適用可能な同意ポリシーを表示"
>abstract="お客様の組織で **Adobe Healthcare Shield** または **Adobe Privacy &amp; Security Shield** を購入した場合、**[!UICONTROL View applicable consent policies]** を選択すると、どの同意ポリシーが適用され、その結果、いくつのプロファイルがアクティベーションに含まれるかを確認することができます。 会社が前述の SKU へのアクセス権を持っていない場合は、このコントロールを無効になります。"

お客様の組織で **Adobe Healthcare Shield** または **Adobe Privacy &amp; Security Shield** を購入した場合、**[!UICONTROL View applicable consent policies]** を選択すると、どの同意ポリシーが適用され、その結果、いくつのプロファイルがアクティベーションに含まれるかを確認することができます。 詳しくは、[&#x200B; 同意ポリシーの評価 &#x200B;](/help/data-governance/enforcement/auto-enforcement.md#consent-policy-evaluation) を参照してください。

### データ使用ポリシーのチェック {#data-usage-policy-checks}

**[!UICONTROL Review]** の手順では、Experience Platformはデータ使用ポリシーの違反もチェックします。 ポリシーに違反した場合の例を次に示します。違反を解決するまで、Audience Activation ワークフローを完了することはできません。 ポリシー違反の解決方法については、データガバナンスに関するドキュメントの [&#x200B; データ使用ポリシー違反 &#x200B;](/help/data-governance/enforcement/auto-enforcement.md#data-usage-violation) を参照してください。

![&#x200B; アクティブ化ワークフローに示すデータポリシー違反の例。](../assets/common/data-policy-violation.png)

### オーディエンスのフィルタリング {#filter-audiences}

また、この手順では、ページで使用可能なフィルターを使用して、このワークフローの一環としてスケジュールまたはマッピングが更新されたオーディエンスのみを表示できます。 また、表示するテーブル列を切り替えることもできます。

![&#x200B; レビューステップで使用可能なオーディエンスフィルターを示す画面録画。](../assets/ui/activate-batch-profile-destinations/filter-audiences-batch-review.gif)

選択内容に満足し、ポリシー違反が検出されていない場合は、「**[!UICONTROL Finish]**」を選択して選択内容を確定し、宛先へのデータの送信を開始します。

## Audience Activation の検証 {#verify}

オーディエンスをクラウドストレージの宛先に書き出す場合、Adobe Experience Platformは、指定されたストレージの場所に `.csv`、`.json` または `.parquet` ファイルを作成します。 ワークフローで設定したスケジュールに従って、ストレージの場所に新しいファイルが作成されます。デフォルトのファイル形式を以下に示しますが、[&#x200B; ファイル名のコンポーネントを編集 &#x200B;](#configure-file-names) できます。
`<destinationName>_segment<segmentID>_<timestamp-yyyymmddhhmmss>.csv`

例えば、書き出し頻度を毎日に選択した場合、連続した 3 日間に受け取るファイルは次のようになります。

```console
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200409052200.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200410061130.csv
```

これらのファイルがストレージの場所に存在すれば、アクティベーションは成功しています。エクスポートされるファイルの構造を理解するには、[サンプルの .csv ファイルをダウンロード](../assets/common/sample_export_file_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv)することができます。このサンプルファイルには、プロファイル属性 `person.firstname`、`person.lastname`、`person.gender`、`person.birthyear` および `personalEmail.address` が含まれています。
