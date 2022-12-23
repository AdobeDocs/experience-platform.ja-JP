---
keywords: プロファイルの宛先のアクティベート;宛先のアクティベート;データのアクティベート;メールマーケティングの宛先アクティベート;クラウドストレージの宛先をアクティベート
title: プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化
type: Tutorial
description: セグメントをバッチプロファイルベースの宛先に送信して、Adobe Experience Platform でのオーディエンスデータをアクティベートする方法を説明します。
exl-id: 82ca9971-2685-453a-9e45-2001f0337cda
source-git-commit: 79fc200f4e56699013b1ba3f91f5e383cea77e2a
workflow-type: ht
source-wordcount: '3411'
ht-degree: 100%

---

# プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の管理]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]**&#x200B;および&#x200B;**[!UICONTROL セグメントの表示]** [に対するアクセス制御権限](/help/access-control/home.md#permissions)が必要です。詳しくは、[アクセス制御の概要](/help/access-control/ui/overview.md)または製品管理者に問い合わせて、必要な権限を取得してください。
>
>改善されたファイル書き出し機能のベータプログラムに参加している一部のお客様には、[新しいベータ版のクラウドストレージの宛先](/help/release-notes/2022/october-2022.md#destinations)へのアクティベーションワークフローの一部として、新しい&#x200B;**[!UICONTROL マッピング]**&#x200B;手順が表示されます。また、[既知の制限事項](#known-limitations)をリリースの一部として追加しました。

## 概要 {#overview}

この記事では、クラウドストレージやメールマーケティングの宛先など、Adobe Experience Platform のバッチプロファイルベースの宛先でオーディエンスデータをアクティベートするために必要なワークフローについて説明します。

## 前提条件 {#prerequisites}

宛先へのデータをアクティベートするには、正常に[宛先に接続する](./connect-destination.md)必要があります。まだ接続していない場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照し、使用する宛先を設定します。

## 宛先の選択 {#select-destination}

1. **[!UICONTROL 接続／宛先]**&#x200B;に移動し、「**[!UICONTROL カタログ]**」タブを選択します。

   ![「宛先のカタログ」タブへのアクセス方法をハイライト表示した画像](../assets/ui/activate-batch-profile-destinations/catalog-tab.png)

1. 以下に示す画像のように、セグメントをアクティベートする宛先に対応するカードで「**[!UICONTROL セグメントのアクティベート]**」を選択します。

   ![「セグメントをアクティベート」ボタンをハイライトした画像](../assets/ui/activate-batch-profile-destinations/activate-segments-button.png)

1. セグメントをアクティベートするために使用する宛先接続を選択し、「**[!UICONTROL 次へ]**」を選択します。

   ![セグメントをアクティベートする 1 つまたは複数の宛先の選択方法を強調表示した画像](../assets/ui/activate-batch-profile-destinations/select-destination.png)

1. 次のセクションの「[セグメントを選択](#select-segments)」に移動します。

## セグメントを選択 {#select-segments}

セグメント名の左側にあるチェックボックスを使用して、宛先に対してアクティベートするセグメントを選択し、「**[!UICONTROL 次へ]**」を選択します。

![アクティベートする 1 つまたは複数のセグメントの選択方法を強調表示した画像](../assets/ui/activate-batch-profile-destinations/select-segments.png)


## セグメントの書き出しをスケジュールする {#scheduling}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_schedule"
>title="スケジュール"
>abstract="鉛筆アイコンを使用して、ファイルの書き出しタイプ（完全なファイルまたは増分ファイル）と書き出し頻度を設定します。"

[!DNL Adobe Experience Platform] は、メールマーケティングおよびクラウドストレージの宛先のデータを [!DNL CSV] ファイルの形式で書き出します。**[!UICONTROL スケジュール]**&#x200B;ページでは、書き出す各セグメントのスケジュールとファイル名を設定できます。スケジュールの設定は必須ですが、ファイル名の設定はオプションです。

>[!IMPORTANT]
> 
>[!DNL Adobe Experience Platform] は、書き出しファイルを1 ファイルあたり 500 万件のレコード（行）で自動的に分割します。各行は 1 つのプロファイルを表します。
>
>`filename.csv`、`filename_2.csv`、`filename_3.csv` のように、分割ファイル名には、ファイルが大きな書き出しの一部であることを示す数字が付加されます。

宛先に送信するセグメントに対応する&#x200B;**[!UICONTROL スケジュールを作成]**&#x200B;ボタンを選択します。

![「スケジュールを作成」ボタンを強調表示した画像](../assets/ui/activate-batch-profile-destinations/create-schedule-button.png)

### 完全ファイルを書き出し {#export-full-files}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_exportoptions"
>title="ファイルの書き出しオプション"
>abstract="「**完全ファイルを書き出し**」を選択して、セグメントに該当するすべてのプロファイルの完全なスナップショットを書き出します。「**増分ファイルの書き出し**」を選択して、前回の書き出し以降にセグメントの対象として認定されたプロファイルのみを書き出します。<br> 最初の増分ファイルの書き出しには、セグメントに適合するすべてのプロファイルが含まれ、バックフィルとして機能します。今後の増分ファイルには、最初の増分ファイル書き出し以降にセグメントで認定されたプロファイルのみが含まれます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html?lang=ja#export-incremental-files" text="増分ファイルの書き出し"

>[!CONTEXTUALHELP]
>id="platform_destinations_activationchaining_aftersegmentevaluation"
>title="セグメント評価後にアクティベート"
>abstract="アクティベーションは、毎日のセグメント化ジョブが完了した直後に実行されます。 これにより、最新のプロファイルが確実に書き出されます。"

>[!CONTEXTUALHELP]
>id="platform_destinations_activationchaining_scheduled"
>title="スケジュールされたアクティベーション"
>abstract="アクティベーションが特定の時刻に実行されます。"

「**[!UICONTROL 完全ファイルを書き出し]**」を選択して、選択したセグメントに対する、すべてのプロファイル選定の完全なスナップショットを含むファイルの書き出しをトリガーします。

![「完全なファイルを書き出し」切り替えを選択した UI の画像。](../assets/ui/activate-batch-profile-destinations/export-full-files.png)

1. 「**[!UICONTROL 頻度]**」セレクターを使用して、書き出しの頻度を選択します。

   * **[!UICONTROL 1 回]**：オンデマンドによる 1 回限りの完全ファイルの書き出しをスケジュールします。
   * **[!UICONTROL 毎日]**：指定した時刻に、毎日 1 回、完全ファイルの書き出しをスケジュールします。

1. 「**[!UICONTROL 時間]**」切り替えスイッチを使用して、書き出しをセグメント評価直後に実行するか、指定した時間にスケジュールに沿って実行するかを選択します。 「**[!UICONTROL スケジュール済み]**」オプションを選択すると、セレクターを使用して書き出しを実行する時刻を [!DNL UTC] 形式で選択できます。

   >[!NOTE]
   >
   >以下に説明する「**[!UICONTROL セグメントの評価後]**」オプションは、現在、一部のベータ版のお客様のみが利用できます。

   「**[!UICONTROL セグメントの評価後]**」オプションを使用して、毎日の Platform バッチセグメント化ジョブが完了した直後にアクティベーションジョブをを実行します。これにより、アクティベーションジョブが実行されると、最新のプロファイルが確実に宛先に書き出されます。

   <!-- Batch segmentation currently runs at {{insert time of day}} and lasts for an average {{x hours}}. Adobe reserves the right to modify this schedule. -->

   ![バッチ宛先のアクティベーションフローでの「セグメントの評価後」オプションを強調表示した画像。](../assets/ui/activate-batch-profile-destinations/after-segment-evaluation-option.png)
「**[!UICONTROL スケジュール済み]**」オプションを使用して、特定の時間にアクティベーションジョブを実行します。 これにより、Experience Platform プロファイルデータは毎日同じ時刻に書き出されます。アクティベーションジョブが開始される前にバッチセグメント化ジョブが完了しているかどうかに応じて、書き出すプロファイルが最新ではない場合があります。

   ![バッチ宛先のアクティベーションフローの「スケジュール済み」オプションが強調表示され、時間セレクターが表示されている画像。](../assets/ui/activate-batch-profile-destinations/scheduled-option.png)

   >[!IMPORTANT]
   >
   >内部の Experience Platform 処理の設定方法により、最初の増分ファイルまたは完全ファイルの書き出しにすべてのバックフィルデータが含まれない場合があります。<br> <br> 完全ファイルと増分ファイルの両方で最新の完全なバックフィルデータの書き出しを確実に行うには、Adobe では、次の日の午後12時（GMT）以降に初回のファイル書き出し時間を設定することをお勧めします。この制限は、今後のリリースで対処される予定です。

1. 「**[!UICONTROL 日付]**」セレクターを使用して、書き出しを実行する日または間隔を選択します。日常の書き出しでのベストプラクティスは、開始日と終了日を、ダウンストリームプラットフォームのキャンペーンの期間に合わせて設定することです。

   >[!IMPORTANT]
   >
   > 書き出し間隔を選択する場合、その間隔の最終日は書き出しに含まれません。例えば、1月4日から 11日までの間隔を選択した場合、最後のファイルエクスポートは 1月10日に実行されます。

1. 「**[!UICONTROL 作成]**」を選択して、スケジュールを保存します。

### 増分ファイルの書き出し {#export-incremental-files}

「**[!UICONTROL 増分ファイルのエクスポート]**」を選択して、エクスポートをトリガーします。最初のファイルは、選択したセグメントに対するすべてのプロファイル資格の完全なスナップショットであり、それ以降のファイルは、前回のエクスポート以降の増分プロファイル資格です。

>[!IMPORTANT]
>
>最初にエクスポートされた増分ファイルには、セグメントの対象となるすべてのプロファイルが含まれ、バックフィルとして機能します。

![「増分ファイルの書き出し」切替スイッチが選択された UI の画像。](../assets/ui/activate-batch-profile-destinations/export-incremental-files.png)

1. **[!UICONTROL 頻度]**&#x200B;セレクターを使用して、エクスポートする頻度を選択します。

   * **[!UICONTROL 毎日]**：増分ファイルのエクスポートを、毎日 1 回指定した時刻にスケジュールします。
   * **[!UICONTROL 毎時]**：増分ファイルのエクスポートを、3 時間、6 時間、8 時間、または 12 時間ごとにスケジュールします。

1. **[!UICONTROL 時間]**&#x200B;セレクターを使用して、エクスポートを実行する時刻を [!DNL UTC] 形式で指定します。

   >[!IMPORTANT]
   >
   >内部の Experience Platform 処理の設定方法により、最初の増分ファイルまたは完全ファイルの書き出しにすべてのバックフィルデータが含まれない場合があります。<br> <br> 完全ファイルと増分ファイルの両方で最新の完全なバックフィルデータの書き出しを確実に行うには、Adobe では、次の日の午後12時（GMT）以降に初回のファイル書き出し時間を設定することをお勧めします。この制限は、今後のリリースで対処される予定です。

1. **[!UICONTROL 日付]**&#x200B;セレクターを使用して、エクスポートを実行する間隔を選択します。ベストプラクティスは、開始日と終了日を、ダウンストリームプラットフォームのキャンペーンの期間に合わせて設定することです。

   >[!IMPORTANT]
   >
   >間隔の最終日はエクスポートに含まれません。例えば、1月4日から 11日までの間隔を選択した場合、最後のファイルエクスポートは 1月10日に実行されます。

1. 「**[!UICONTROL 作成]**」を選択して、スケジュールを保存します。

### ファイル名の設定 {#file-names}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_filename"
>title="ファイル名の設定"
>abstract="ファイルベースの宛先の場合、セグメントごとに一意のファイル名が生成されます。ファイル名エディターを使用して、一意のファイル名を作成および編集するか、デフォルトの名前のままにします。"

ほとんどの宛先では、デフォルトのファイル名は、宛先名、セグメント ID、日時インジケーターで構成されます。例えば、書き出したファイル名を編集して、異なるキャンペーンを区別したり、データの書き出し時間をファイルに追加したりできます。一部の宛先開発者は、別のデフォルトのファイル名追加オプションを宛先に表示するように選択する場合があります。

鉛筆アイコンを選択してモーダルウィンドウを開き、ファイル名を編集します。ファイル名は 255 文字までに制限されています。

>[!NOTE]
>
>次の画像は [!DNL Amazon S3] の宛先のファイル名を編集する方法を示していますが、プロセスはすべてのバッチ宛先 （SFTP、[!DNL Azure Blob Storage] または [!DNL Google Cloud Storage] など）で同じです。

![ファイル名の設定に使用される鉛筆アイコンをハイライト表示する画像。](../assets/ui/activate-batch-profile-destinations/configure-name.png)

ファイル名エディターで、別のコンポーネントを選択してファイル名に追加できます。

![使用可能なすべてのファイル名オプションを示す画像。](../assets/ui/activate-batch-profile-destinations/activate-workflow-configure-step-2.png)

宛先名とセグメント ID をファイル名から削除することはできません。これらに加えて、次を追加できます。

| ファイル名オプション | 説明 |
|---------|----------|
| **[!UICONTROL セグメント名]** | 書き出されたセグメントの名前。 |
| **[!UICONTROL 日時]** | ファイルを生成した時刻のタイムスタンプを、`MMDDYYYY_HHMMSS` 形式で追加するか、Unix の 10 桁で追加するかを選択します。増分書き出しのたびにファイル名を動的に生成したい場合は、次のオプションのいずれかを選択します。 |
| **[!UICONTROL カスタムテキスト]** | ファイル名に追加する任意のカスタムテキスト。 |
| **[!UICONTROL 宛先 ID]** | セグメントの書き出しに使用する宛先データフローの ID。<br> **メモ**：このファイル名追加オプションは、改善されたファイル書き出し機能ベータプログラムに参加しているベータ版のお客様のみが利用できます。ベータ版プログラムへのアクセスを希望する場合は、アドビ担当者またはカスタマーケアにお問い合わせください。 |
| **[!UICONTROL 宛先名]** | セグメントの書き出しに使用する宛先データフローの名前。<br> **メモ**：このファイル名追加オプションは、改善されたファイル書き出し機能ベータプログラムに参加しているベータ版のお客様のみが利用できます。ベータ版プログラムへのアクセスを希望する場合は、アドビ担当者またはカスタマーケアにお問い合わせください。 |
| **[!UICONTROL 組織名]** | Experience Platform 内の組織名。<br> **メモ**：このファイル名追加オプションは、改善されたファイル書き出し機能ベータプログラムに参加しているベータ版のお客様のみが利用できます。ベータ版プログラムへのアクセスを希望する場合は、アドビ担当者またはカスタマーケアにお問い合わせください。 |
| **[!UICONTROL サンドボックス名]** | セグメントの書き出しに使用するサンドボックスの ID。<br> **メモ**：このファイル名追加オプションは、改善されたファイル書き出し機能ベータプログラムに参加している、ベータ版のお客様のみが利用できます。ベータ版プログラムへのアクセスを希望する場合は、アドビ担当者またはカスタマーケアにお問い合わせください。 |

{style=&quot;table-layout:auto&quot;}

**[!UICONTROL 変更を適用]**&#x200B;を選択して、確定します。

>[!IMPORTANT]
> 
>**[!UICONTROL 日時]**&#x200B;コンポーネントを選択しない場合、ファイル名は固定され、新しくエクスポートされたファイルは、エクスポートのたびに保存場所にある以前のファイルを上書きします。ストレージの場所からメールマーケティングプラットフォームへのインポートジョブを繰り返し実行する場合は、このオプションをお勧めします。

すべてのセグメントの設定が完了したら「**[!UICONTROL 次へ]** 」を選択して続行します。

## プロファイル属性の選択 {#select-attributes}

プロファイルベースの宛先の場合、ターゲット宛先に送信するプロファイル属性を選択する必要があります。

1. **[!UICONTROL 属性を選択]**&#x200B;ページで「**[!UICONTROL 新しいフィールドを追加]**」を選択します。

   ![「新しいフィールドを追加」ボタンを強調表示した画像。](../assets/ui/activate-batch-profile-destinations/add-new-field.png)

1. 「**[!UICONTROL スキーマフィールド]**」エントリの右側の矢印を選択します。

   ![ソースフィールドの選択方法をハイライト表示した画像。](../assets/ui/activate-batch-profile-destinations/select-target-field.png)

1. **[!UICONTROL フィールドを選択]**&#x200B;ページで、宛先に送信する XDM 属性を選択してから「**[!UICONTROL 選択]**」を選択します。

   ![ソースフィールドとして使用可能な様々なフィールドを示す画像。](../assets/ui/activate-batch-profile-destinations/target-field-page.png)

1. さらにマッピングを追加するには、手順 1～3 を繰り返します。

>[!NOTE]
>
> Adobe Experience Platform は、スキーマから推奨される一般的に使用される属性 4 つ（`person.name.firstName`、`person.name.lastName`、`personalEmail.address`、`segmentMembership.status`）を事前に選択します。

>[!IMPORTANT]
>
>既知の制限により、現在、**[!UICONTROL フィールドを選択]**&#x200B;ウィンドウを使用して、`segmentMembership.status` をファイル書き出しに追加できません。 代わりに、手動で値 `xdm: segmentMembership.status` をスキーマフィールドに貼り付ける必要があります（下図を参照）。
>
>![アクティベーションワークフローのマッピング手順でセグメントメンバーシップの回避策を示す画面の録画。](/help/destinations/assets/ui/activate-batch-profile-destinations/segment-membership.gif)

ファイルのエクスポートは、`segmentMembership.status` が選択されているかどうかによって、次のように異なります。
* `segmentMembership.status` フィールドを選択した場合、エクスポートされたファイルには、最初の完全スナップショットでは&#x200B;**[!UICONTROL アクティブ]**&#x200B;メンバーが含まれ、その後の増分エクスポートでは&#x200B;**[!UICONTROL アクティブ]**&#x200B;および&#x200B;**[!UICONTROL 期限切れ]**&#x200B;のメンバーが含まれます。
* `segmentMembership.status` フィールドを選択しない場合、エクスポートされたファイルには、最初の完全スナップショットとその後の増分エクスポートで、**[!UICONTROL アクティブ]**&#x200B;メンバーのみが含まれます。

![セグメントアクティベーションワークフローのマッピング手順で事前入力された推奨属性を示す画像。](../assets/ui/activate-batch-profile-destinations/mandatory-deduplication.png)

### 必須の属性 {#mandatory-attributes}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_mandatorykey"
>title="必須の属性について"
>abstract="書き出されたすべてのプロファイルに含める XDM スキーマ属性を選択します。必須キーのないプロファイルは、宛先に書き出されません。必須キーを選択しない場合、属性に関係なく、すべての対象プロファイルを書き出します。"

必須属性は、選択した属性がすべてのプロファイルレコードに含まれるようにする、ユーザーが有効にしたチェックボックスです。例：書き出されるすべてのプロファイルには、メールアドレスが含まれます。

[!DNL Platform] が特定の属性を含むプロファイルのみを書き出すよう、属性を必須としてマークすることができます。その結果、追加のフィルタリング形式として使用できます。属性を必須としてマークする必要は&#x200B;**ありません**。

必須属性を選択しない場合、属性に関係なく、すべての対象プロファイルを書き出します。

属性の 1 つをスキーマの[一意の ID](../../destinations/catalog/email-marketing/overview.md#identity) にすることをお勧めします。必須属性について詳しくは、[メールマーケティングの宛先](../../destinations/catalog/email-marketing/overview.md#identity)ドキュメントの ID の節を参照してください。

### 重複排除キー {#deduplication-keys}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_deduplicationkey"
>title="重複排除キーについて"
>abstract="重複排除キーを選択することで、書き出しファイル内にある、同一プロファイルの複数のレコードを排除します。重複排除キーとして、1 つの名前空間または最大 2 つの XDM スキーマ属性を選択できます。重複排除キーを選択しないと、書き出しファイルでプロファイルエントリの重複が発生する場合があります。"

重複排除キーは、ユーザーがプロファイルの重複排除する ID を決定する、ユーザー定義のプライマリキーです。

重複排除キーを使用すると、同一プロファイルの複数のレコードが 1 つの書き出しファイルに含まれる可能性がなくなります。

[!DNL Platform] で重複排除キーを使用する方法は 3 つあります。

* [!UICONTROL 重複排除キー]として、単一の ID 名前空間を使用する
* [!UICONTROL 重複排除キー]として、[!DNL XDM] プロファイルから単一のプロファイル属性を使用する
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
        "id": "johndoe_1@example.com"
      },
      {
        "id": "johndoe_2@example.com"
      }
    ]
  },
  "segmentMembership": {
    "ups": {
      "fa5c4622-6847-4199-8dd4-8b7c7c7ed1d6": {
        "status": "existing",
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
        "id": "johndoe_1@example.com"
      },
      {
        "id": "johndoe_2@example.com"
      }
    ]
  },
  "segmentMembership": {
    "ups": {
      "fa5c4622-6847-4199-8dd4-8b7c7c7ed1d6": {
        "status": "existing",
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

[!DNL Email] 名前空間の重複排除を指定した場合、書き出しファイルには次のエントリが含まれます。プロファイル B はセグメントに適合する最新のプロファイルなので、唯一書き出されます。

| メール* | personalEmail | firstName | lastName |
|---|---|---|---|
| johndoe_1@example.com | johndoe@example.com | John | D |
| johndoe_2@example.com | johndoe@example.com | John | D |

### 重複排除のユースケース 3：単一のプロファイル属性に基づく重複排除 {#deduplication-use-case-3}

`personal Email` 属性による重複排除を仮定した場合、エクスポートファイルには次のエントリが含まれます。プロファイル B はセグメントに適合する最新のプロファイルなので、唯一書き出されます。

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

>[!NOTE]
> 
>（データセット全体ではなく）データセット内の特定のフィールドのみに適用されたデータ使用ラベルがある場合、アクティベーション時のこれらのフィールドレベルラベルの適用は、次の条件で行われます。
>
>* これらのフィールドは、セグメント定義で使用されます。
>* フィールドは、ターゲット先の予測属性として設定されます。
>
> 例えば、フィールド `person.name.firstName` に宛先のマーケティングアクションと競合する特定のデータ使用ラベルがある場合、レビュー手順でデータ使用ポリシー違反が表示されます。詳しくは、[Adobe Experience Platform でのデータガバナンス](../../rtcdp/privacy/data-governance-overview.md#destinations)を参照してください。

## （ベータ版）マッピング {#mapping}

>[!IMPORTANT]
> 
>ベータ版の一部のお客様は、改善された&#x200B;**[!UICONTROL マッピング]**&#x200B;手順を表示できます。この手順は、先程説明した[プロファイル属性を選択](#select-attributes)に変わるものです。この新しい&#x200B;**[!UICONTROL マッピング]**&#x200B;手順では、書き出したファイルのヘッダーを編集して目的のカスタム名にすることができます。
> 
> 機能とドキュメントは変更される場合があります。このベータ版プログラムへのアクセスを希望する場合は、アドビ担当者またはカスタマーケアにお問い合わせください。

この手順では、ターゲットの宛先に書き出すファイルに追加する、プロファイル属性を選択する必要があります。 書き出すプロファイル属性と ID を選択するには：

1. **[!UICONTROL マッピング]**&#x200B;ページで「**[!UICONTROL 新しいフィールドを追加]**」を選択します。

   ![マッピングワークフローで強調表示されている新しいフィールドコントロールを追加します。](../assets/ui/activate-batch-profile-destinations/add-new-field-mapping.png)

1. **[!UICONTROL ソースフィールド]**&#x200B;エントリの右側の矢印を選択します。

   ![マッピングワークフローでハイライト表示されているソースフィールドコントロールを選択します。](../assets/ui/activate-batch-profile-destinations/select-source-field.png)

1. **[!UICONTROL ソースフィールドを選択]**&#x200B;ページで、宛先へ書き出したファイルに含めるプロファイル属性と ID を選択してから、「**[!UICONTROL 選択]**」を選択します。

   >[!TIP]
   > 
   >次の画像に示すように、検索フィールドを使用して、選択を絞り込むことができます。

   ![宛先に書き出すことができるプロファイル属性を示す、モーダルウィンドウ。](../assets/ui/activate-batch-profile-destinations/select-source-field-modal.png)


1. 書き出し用に選択したフィールドがマッピングビューに表示されます。 必要に応じて、書き出したファイルのヘッダーの名前を編集できます。 それには、ターゲットフィールドでアイコンを選択します。

   ![宛先に書き出すことができるプロファイル属性を示す、モーダルウィンドウ。](../assets/ui/activate-batch-profile-destinations/mapping-step-select-target-field.png)

1. **[!UICONTROL ターゲットフィールドを選択]**&#x200B;ページで、書き出したファイルでヘッダーに目的の名前を入力し、「**[!UICONTROL 選択]**」をクリックします。

   ![ヘッダーに対して入力されたわかりやすい名前を示すモーダルウィンドウ。](../assets/ui/activate-batch-profile-destinations/select-target-field-mapping.png)

1. 書き出し用に選択したフィールドがマッピングビューに表示され、編集したヘッダーが書き出したファイルに表示されます。

   ![宛先に書き出すことができるプロファイル属性を示すモーダルウィンドウ。](../assets/ui/activate-batch-profile-destinations/select-target-field-updated.png)

1. （オプション）書き出されたフィールドを[必須キー](#mandatory-keys)または [重複排除キー](#deduplication-keys)のどちらにするかを選択できます。

   ![宛先に書き出すことができるプロファイル属性を示すモーダルウィンドウ。](../assets/ui/activate-batch-profile-destinations/select-mandatory-deduplication-key.png)

1. 書き出すフィールドをさらに追加するには、上記の手順を繰り返します。

### 既知の制限事項 {#known-limitations}

新しい&#x200B;**[!UICONTROL マッピング]**&#x200B;ページには次の既知の制限事項があります。

#### マッピングワークフローでは、セグメントメンバーシップの属性を選択できません

既知の制限により、現在、**[!UICONTROL フィールドを選択]**&#x200B;ウィンドウを使用してファイルの書き出しに `segmentMembership.status` を追加することはできません。 代わりに、値 `xdm: segmentMembership.status` をスキーマフィールドに手動で貼り付ける必要があります（下図を参照）。

![アクティベーションワークフローのマッピング手順でセグメントメンバーシップの回避策を示す画面記録。](/help/destinations/assets/ui/activate-batch-profile-destinations/segment-membership-mapping-step.gif)

ファイルのエクスポートは、`segmentMembership.status` が選択されているかどうかによって、次のように異なります。
* `segmentMembership.status` フィールドを選択した場合、エクスポートされたファイルには、最初の完全スナップショットでは&#x200B;**[!UICONTROL アクティブ]**&#x200B;メンバーが含まれ、その後の増分エクスポートでは&#x200B;**[!UICONTROL アクティブ]**&#x200B;および&#x200B;**[!UICONTROL 期限切れ]**&#x200B;のメンバーが含まれます。
* `segmentMembership.status` フィールドを選択しない場合、エクスポートされたファイルには、最初の完全スナップショットとその後の増分エクスポートで、**[!UICONTROL アクティブ]**&#x200B;メンバーのみが含まれます。

#### ID 名前空間は現在、書き出し用に選択できません

以下の画像に示すように、ID 名前空間を書き出し用に選択する機能は、現在サポートされていません。 書き出し用の ID 名前空間を選択すると、**[!UICONTROL レビュー]**&#x200B;手順でエラーが発生します。

![ID の書き出しを示す、サポートされていないマッピング](/help/destinations/assets/ui/activate-batch-profile-destinations/unsupported-identity-mapping.png)

ベータ版の間に書き出したファイルに ID 名前空間を追加する必要がある場合の一時的な回避策として、次のいずれかを実行できます。
* 書き出しに ID 名前空間を含めるデータフローに、従来のクラウドストレージの宛先を使用する
* ID を属性として Experience Platform にアップロードし、クラウドストレージの宛先に書き出します。

## レビュー {#review}

「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>このステップでは、Adobe Experience Platform はデータ使用ポリシーの違反がないかを確認します。ポリシーに違反した場合の例を次に示します。違反を解決するまで、セグメントのアクティベーションワークフローを完了することはできません。ポリシー違反の解決方法については、データガバナンスに関するドキュメントの[ポリシーの適用](../../rtcdp/privacy/data-governance-overview.md#enforcement)を参照してください。

![データポリシー違反の例を示す画像。](../assets/common/data-policy-violation.png)

ポリシー違反が検出されていない場合は、「**[!UICONTROL 完了]**」を選択して選択内容を確定し、宛先へのデータの送信を開始します。

![セグメントアクティベーションワークフローのレビュー画面を示す画像。](../assets/ui/activate-batch-profile-destinations/review.png)

## セグメントのアクティベーションを検証 {#verify}

メールマーケティングの宛先とクラウドストレージの宛先の場合、Adobe Experience Platform は、指定されたストレージの場所に `.csv` ファイルを作成します。ワークフローで設定したスケジュールに従って、ストレージの場所に新しいファイルが作成されます。デフォルトのファイル形式は `<destinationName>_segment<segmentID>_<timestamp-yyyymmddhhmmss>.csv` です。

例えば、書き出し頻度を毎日に選択した場合、連続した 3 日間に受け取るファイルは次のようになります。

```console
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200409052200.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200410061130.csv
```

これらのファイルがストレージの場所に存在すれば、アクティベーションは成功しています。エクスポートされるファイルの構造を理解するには、[サンプルの .csv ファイルをダウンロード](../assets/common/sample_export_file_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv)することができます。このサンプルファイルには、プロファイル属性 `person.firstname`、`person.lastname`、`person.gender`、`person.birthyear` および `personalEmail.address` が含まれています。
