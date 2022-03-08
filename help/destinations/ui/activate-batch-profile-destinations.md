---
keywords: プロファイルの宛先のアクティブ化、宛先のアクティブ化、データのアクティブ化電子メールマーケティングの宛先をアクティブ化する。クラウドストレージの宛先をアクティブ化
title: プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化
type: Tutorial
seo-title: Activate audience data to batch profile export destinations
description: セグメントをバッチプロファイルベースの宛先に送信して、Adobe Experience Platformでのオーディエンスデータをアクティブ化する方法を説明します。
seo-description: Learn how to activate the audience data you have in Adobe Experience Platform by sending segments to batch profile-based destinations.
exl-id: 82ca9971-2685-453a-9e45-2001f0337cda
source-git-commit: 6c64e8400c85865aab4e8cfb9e86850562ba97aa
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化

## 概要 {#overview}

この記事では、クラウドストレージや電子メールマーケティングの宛先など、Adobe Experience Platformのバッチプロファイルベースの宛先でオーディエンスデータをアクティブ化するために必要なワークフローについて説明します。

## 前提条件 {#prerequisites}

宛先に対してデータをアクティブ化するには、次の条件を満たしている必要があります。 [宛先に接続されている](./connect-destination.md). まだおこなっていない場合は、 [宛先カタログ](../catalog/overview.md)、サポートされている宛先を参照し、使用する宛先を設定します。

## 宛先を選択 {#select-destination}

1. に移動します。 **[!UICONTROL 接続/宛先]**&#x200B;をクリックし、 **[!UICONTROL カタログ]** タブをクリックします。

   ![「宛先カタログ」タブ](../assets/ui/activate-batch-profile-destinations/catalog-tab.png)

1. 選択 **[!UICONTROL セグメントのアクティブ化]** を選択します。

   ![「セグメントをアクティブ化」ボタン](../assets/ui/activate-batch-profile-destinations/activate-segments-button.png)

1. セグメントのアクティブ化に使用する宛先接続を選択し、「 」を選択します。 **[!UICONTROL 次へ]**.

   ![宛先を選択](../assets/ui/activate-batch-profile-destinations/select-destination.png)

1. 次のセクションに移動： [セグメントを選択](#select-segments).

## セグメントを選択 {#select-segments}

セグメント名の左側にあるチェックボックスを使用して、宛先に対してアクティブ化するセグメントを選択し、「 」を選択します。 **[!UICONTROL 次へ]**.

![セグメントを選択](../assets/ui/activate-batch-profile-destinations/select-segments.png)


## セグメントの書き出しをスケジュール {#scheduling}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_schedule"
>title="スケジュール"
>abstract="鉛筆アイコンを使用して、ファイルのエクスポートタイプ（完全なファイルまたは増分ファイル）とエクスポート頻度を設定します。"
>additional-url="https://www.adobe.com/go/destinations-profile-batch-en" text="詳しくは、ドキュメントを参照してください。"

[!DNL Adobe Experience Platform] 電子メールマーケティングおよびクラウドストレージの宛先のデータを [!DNL CSV] ファイル。 内 **[!UICONTROL スケジュール]** ページでは、書き出す各セグメントのスケジュールとファイル名を設定できます。 スケジュールの設定は必須ですが、ファイル名の設定はオプションです。

>[!IMPORTANT]
> 
>[!DNL Adobe Experience Platform] は、1 ファイルあたり 500 万件のレコード（行）でエクスポートファイルを自動的に分割します。 各行は 1 つのプロファイルを表します。
>
>次のように、分割ファイル名には、ファイルが大きなエクスポートの一部であることを示す数字が付加されます。 `filename.csv`, `filename_2.csv`, `filename_3.csv`.

を選択します。 **[!UICONTROL スケジュールを作成]** ボタンをクリックします。

![「スケジュールを作成」ボタン](../assets/ui/activate-batch-profile-destinations/create-schedule-button.png)

### 完全なファイルを書き出し {#export-full-files}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_exportoptions"
>title="ファイル書き出しオプション"
>abstract="選択 **完全なファイルを書き出し** を使用して、セグメントに該当するすべてのプロファイルの完全なスナップショットを書き出します。 選択 **増分ファイルの書き出し** ：前回のエクスポート以降にセグメントの対象として認定されたプロファイルのみをエクスポートします。 <br> 最初の増分ファイルの書き出しには、セグメントに適合するすべてのプロファイルが含まれ、バックフィルとして機能します。 今後の増分ファイルには、最初の増分ファイルエクスポート以降にセグメントで認定されたプロファイルのみが含まれます。"
>additional-url="https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations.html?lang=en#export-incremental-files" text="増分ファイルの書き出し"

選択 **[!UICONTROL 完全なファイルを書き出し]** ：選択したセグメントのすべてのプロファイル認定の完全なスナップショットを含むファイルのエクスポートをトリガーします。

![完全なファイルを書き出し](../assets/ui/activate-batch-profile-destinations/export-full-files.png)

1. 以下を使用： **[!UICONTROL 頻度]** 書き出し頻度を選択するセレクター：

   * **[!UICONTROL 1 回]**:1 回のオンデマンドフルファイル書き出しをスケジュールします。
   * **[!UICONTROL 毎日]**:指定した時刻に、毎日 1 回、ファイルの完全なエクスポートをスケジュールします。

1. 以下を使用： **[!UICONTROL 時間]** 時刻を選択するセレクター ( ) [!DNL UTC] 形式を指定します。

   >[!IMPORTANT]
   >
   >内部Experience Platform処理の設定方法により、最初の増分処理またはフルファイル書き出しにすべてのバックフィルデータが含まれていない場合があります。 <br> <br> フルファイルと増分ファイルの両方で最新の完全なバックフィルデータ書き出しを確実におこなうには、Adobeでは、次の日の午後 12 時 (GMT) 以降に最初のファイル書き出し時間を設定することをお勧めします。 この制限は、今後のリリースで対処される予定です。

1. 以下を使用： **[!UICONTROL 日付]** セレクターを使用して、エクスポートを実行する日または間隔を選択します。 毎日のエクスポートの場合、ベストプラクティスは、開始日と終了日を、ダウンストリームプラットフォームのキャンペーンの期間に合わせて設定することです。

   >[!IMPORTANT]
   >
   > エクスポート間隔を選択する場合、その間隔の最終日はエクスポートに含まれません。 例えば、1 月 4 日から 11 日の間隔を選択した場合、最後のファイルエクスポートは 1 月 10 日に実行されます。

1. 選択 **[!UICONTROL 作成]** スケジュールを保存します。


### 増分ファイルの書き出し {#export-incremental-files}

選択 **[!UICONTROL 増分ファイルの書き出し]** 最初のファイルが、選択したセグメントに対するすべてのプロファイル認定の完全なスナップショットであるエクスポートをトリガーし、それ以降のファイルは、前回のエクスポート以降の増分プロファイル認定です。

>[!IMPORTANT]
>
>最初に書き出された増分ファイルには、セグメントの対象として認定されるすべてのプロファイルが含まれ、バックフィルとして機能します。

![増分ファイルの書き出し](../assets/ui/activate-batch-profile-destinations/export-incremental-files.png)

1. 以下を使用： **[!UICONTROL 頻度]** 書き出し頻度を選択するセレクター：

   * **[!UICONTROL 毎日]**:増分ファイルの書き出しを、指定した時刻に毎日 1 回スケジュールします。
   * **[!UICONTROL 毎時]**:3、6、8、12 時間ごとに増分ファイルのエクスポートをスケジュールします。

1. 以下を使用： **[!UICONTROL 時間]** 時刻を選択するセレクター ( ) [!DNL UTC] 形式を指定します。

   >[!IMPORTANT]
   >
   >内部Experience Platform処理の設定方法により、最初の増分処理またはフルファイル書き出しにすべてのバックフィルデータが含まれていない場合があります。 <br> <br> フルファイルと増分ファイルの両方で最新の完全なバックフィルデータ書き出しを確実におこなうには、Adobeでは、次の日の午後 12 時 (GMT) 以降に最初のファイル書き出し時間を設定することをお勧めします。 この制限は、今後のリリースで対処される予定です。

1. 以下を使用： **[!UICONTROL 日付]** セレクターを使用して、エクスポートを実行する間隔を選択します。 ベストプラクティスは、開始日と終了日を、ダウンストリームプラットフォームのキャンペーンの期間に合わせて設定することです。

   >[!IMPORTANT]
   >
   >期間の最終日はエクスポートに含まれません。 例えば、1 月 4 日から 11 日の間隔を選択した場合、最後のファイルエクスポートは 1 月 10 日に実行されます。

1. 選択 **[!UICONTROL 作成]** スケジュールを保存します。

### ファイル名の設定 {#file-names}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_filename"
>title="ファイル名を設定"
>abstract="ファイルベースの宛先の場合、セグメントごとに一意のファイル名が生成されます。 ファイル名エディターを使用して、一意のファイル名を作成および編集するか、デフォルトの名前のままにします。"

デフォルトのファイル名は、宛先名、セグメント ID、日時インジケーターで構成されます。 例えば、エクスポートされたファイル名を編集して、異なるキャンペーンを区別したり、データのエクスポート時間をファイルに追加したりできます。

鉛筆アイコンを選択してモーダルウィンドウを開き、ファイル名を編集します。 ファイル名は 255 文字までに制限されています。

![ファイル名を設定](../assets/ui/activate-batch-profile-destinations/configure-name.png)

ファイル名エディターで、別のコンポーネントを選択してファイル名に追加できます。

![ファイル名のオプションを編集](../assets/ui/activate-batch-profile-destinations/activate-workflow-configure-step-2.png)

宛先名とセグメント ID をファイル名から削除することはできません。 これらに加えて、次を追加できます。

* **[!UICONTROL セグメント名]**:ファイル名にセグメント名を追加できます。
* **[!UICONTROL 日時]**:追加する項目を選択 `MMDDYYYY_HHMMSS` 形式または Unix の 10 桁のタイムスタンプ。 増分エクスポートごとに動的ファイル名を生成する場合は、次のオプションのいずれかを選択します。
* **[!UICONTROL カスタムテキスト]**:ファイル名にカスタムテキストを追加します。

選択 **[!UICONTROL 変更を適用]** をクリックして選択を確定します。

>[!IMPORTANT]
> 
>選択しない場合、 **[!UICONTROL 日時]** コンポーネントの場合、ファイル名は静的になり、新しく書き出されたファイルは、保存場所にある以前のファイルを書き出しごとに上書きします。 ストレージの場所から電子メールマーケティングプラットフォームに繰り返しインポートジョブを実行する場合は、このオプションをお勧めします。

すべてのセグメントの設定が完了したら、「 」を選択します。 **[!UICONTROL 次へ]** をクリックして続行します。

## プロファイル属性を選択 {#select-attributes}

プロファイルベースの宛先の場合、ターゲット宛先に送信するプロファイル属性を選択する必要があります。


1. 内 **[!UICONTROL 属性を選択]** ページ、選択 **[!UICONTROL 新しいフィールドを追加]**.

   ![新しいマッピングを追加](../assets/ui/activate-batch-profile-destinations/add-new-field.png)

1. 右側の矢印を選択します。 **[!UICONTROL スキーマフィールド]** エントリ。

   ![ソースフィールドを選択](../assets/ui/activate-batch-profile-destinations/select-target-field.png)

1. 内 **[!UICONTROL フィールドを選択]** ページで、宛先に送信する XDM 属性を選択してから、「 」を選択します **[!UICONTROL 選択]**.

   ![ソースフィールドページを選択](../assets/ui/activate-batch-profile-destinations/target-field-page.png)

1. さらにマッピングを追加するには、手順 1 ～ 3 を繰り返します。

>[!NOTE]
>
> Adobe Experience Platformは、スキーマの 4 つの推奨される、一般的に使用される属性を事前に選択します。 `person.name.firstName`, `person.name.lastName`, `personalEmail.address`, `segmentMembership.status`.

ファイルの書き出しは、 `segmentMembership.status` が選択されている場合：
* この `segmentMembership.status` フィールドが選択され、書き出されたファイルは次を含む **[!UICONTROL アクティブ]** 最初のフルスナップショットのメンバーと **[!UICONTROL アクティブ]** および **[!UICONTROL 期限切れ]** 以降の増分エクスポートのメンバー
* この `segmentMembership.status` フィールドが選択されていない場合、書き出されたファイルは次のみを含みます **[!UICONTROL アクティブ]** 最初のフルスナップショットと、その後の増分エクスポートのメンバー。

![推奨属性](../assets/ui/activate-batch-profile-destinations/mandatory-deduplication.png)

### 必須の属性 {#mandatory-attributes}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_mandatorykey"
>title="必須の属性について"
>abstract="書き出されたすべてのプロファイルに含める XDM スキーマ属性を選択します。 必須キーのないプロファイルは、宛先に書き出されません。 必須キーを選択しない場合、属性に関係なく、すべての認定プロファイルがエクスポートされます。"
>additional-url="http://www.adobe.com/go/destinations-mandatory-attributes-en" text="詳しくは、ドキュメントを参照してください。"

必須属性は、選択した属性がすべてのプロファイルレコードに含まれるようにする、ユーザーが有効にしたチェックボックスです。 例：書き出されるすべてのプロファイルには、電子メールアドレスが含まれ&#x200B;ます。

属性を必須としてマークして、 [!DNL Platform] は、特定の属性を含むプロファイルのみを書き出します。 その結果、追加のフィルタリング形式として使用できます。 属性を必須としてマークすることは **not** 必須

必須属性を選択しない場合、属性に関係なく、すべての認定済みプロファイルがエクスポートされます。

属性の 1 つを [一意の ID](../../destinations/catalog/email-marketing/overview.md#identity) スキーマから。 必須属性について詳しくは、 [電子メールマーケティングの宛先](../../destinations/catalog/email-marketing/overview.md#identity) ドキュメント。

### 重複排除キー {#deduplication-keys}

>[!CONTEXTUALHELP]
>id="platform_destinations_activate_deduplicationkey"
>title="重複排除キーについて"
>abstract="重複排除キーを選択することで、エクスポートファイル内の同じプロファイルの複数のレコードを排除します。 重複排除キーとして 1 つの名前空間または最大 2 つの XDM スキーマ属性を選択します。 重複排除キーを選択しないと、エクスポートファイルでプロファイルエントリが重複する場合があります。"
>additional-url="http://www.adobe.com/go/destinations-deduplication-keys-en" text="詳しくは、ドキュメントを参照してください。"

重複排除キーは、ユーザーがプロファイルの重複排除を希望する ID を決定する、ユーザー定義のプライマリキー&#x200B;です。

重複排除キーを使用すると、同じプロファイルの複数のレコードが 1 つのエクスポートファイルに含まれる可能性がなくなります。

で重複排除キーを使用する方法は 3 つあります。 [!DNL Platform]:

* 単一の ID 名前空間をとして使用 [!UICONTROL 重複排除キー]
* 単一のプロファイル属性を [!DNL XDM] as a のプロファイル [!UICONTROL 重複排除キー]
* 1 つの [!DNL XDM] 複合キーとしてのプロファイル

>[!IMPORTANT]
>
> 単一の ID 名前空間を宛先に書き出すことができ、名前空間は重複排除キーとして自動的に設定されます。 宛先への複数の名前空間の送信はサポートされていません。
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

### 重複排除の使用例 1:重複排除なし {#deduplication-use-case-1}

重複排除を使用しない場合、エクスポートファイルには次のエントリが含まれます。

| personalEmail | firstName | lastName |
|---|---|---|
| johndoe@example.com | John | Doe |
| johndoe@example.com | John | D |


### 重複排除の使用例 2:id 名前空間に基づく重複排除 {#deduplication-use-case-2}

での重複排除 [!DNL Email] 名前空間の場合、書き出しファイルには次のエントリが含まれます。 プロファイル B はセグメントに適合する最新のものなので、書き出されるのは 1 つだけです。

| メール* | personalEmail | firstName | lastName |
|---|---|---|---|
| johndoe_1@example.com | johndoe@example.com | John | D |
| johndoe_2@example.com | johndoe@example.com | John | D |

### 重複排除の使用例 3:単一のプロファイル属性に基づく重複排除 {#deduplication-use-case-3}

での重複排除 `personal Email` 属性を指定した場合、エクスポートファイルには次のエントリが含まれます。 プロファイル B はセグメントに適合する最新のものなので、書き出されるのは 1 つだけです。

| personalEmail* | firstName | lastName |
|---|---|---|
| johndoe@example.com | John | D |


### 重複排除の使用例 4:2 つのプロファイル属性に基づく重複排除 {#deduplication-use-case-4}

複合キーによる重複排除の想定 `personalEmail + lastName`を指定した場合、エクスポートファイルには次のエントリが含まれます。

| personalEmail* | lastName* | firstName |
|---|---|---|
| johndoe@example.com | D | John |
| johndoe@example.com | Doe | John |


Adobeは、などの ID 名前空間を選択することをお勧めします [!DNL CRM ID] または電子メールアドレスを重複排除キーとして指定して、すべてのプロファイルレコードを一意に識別できるようにします。

>[!NOTE]
> 
>データセット内の特定のフィールドに（データセット全体ではなく）データ使用ラベルが適用されている場合、アクティベーション時にこれらのフィールドレベルのラベルが適用されます。
>
>* これらのフィールドは、セグメント定義で使用されます。
>* フィールドは、ターゲット先の予測属性として設定されます。
>
> 例えば、 `person.name.firstName` には、宛先のマーケティングアクションと競合する特定のデータ使用ラベルがあり、レビュー手順にデータ使用ポリシー違反が表示されます。 詳しくは、 [Adobe Experience Platformのデータガバナンス](../../rtcdp/privacy/data-governance-overview.md#destinations).

## レビュー {#review}

「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>この手順では、Adobe Experience Platformはデータ使用ポリシーの違反を確認します。 次に、ポリシーに違反した場合の例を示します。 違反を解決するまで、セグメントのアクティベーションワークフローを完了することはできません。 ポリシー違反の解決方法については、 [ポリシーの適用](../../rtcdp/privacy/data-governance-overview.md#enforcement) （データガバナンスに関するドキュメントの節）。

![データポリシー違反](../assets/common/data-policy-violation.png)

ポリシー違反が検出されていない場合は、「 」を選択します。 **[!UICONTROL 完了]** をクリックして選択を確定し、宛先へのデータの送信を開始します。

![レビュー](../assets/ui/activate-batch-profile-destinations/review.png)

## セグメントのアクティベーションを検証 {#verify}


電子メールマーケティングの宛先とクラウドストレージの宛先の場合、Adobe Experience Platformは `.csv` ファイルを指定したストレージの場所に保存します。 新しいファイルはストレージの場所に毎日作成されます。デフォルトのファイル形式は次のとおりです。
`<destinationName>_segment<segmentID>_<timestamp-yyyymmddhhmmss>.csv`

3 日連続で受け取るファイルは次のようになります。

```console
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200409052200.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200410061130.csv
```

これらのファイルがストレージの場所に存在すれば、アクティベーションは成功しています。書き出されたファイルの構造を理解するには、次の手順を実行します。 [サンプルの.csv ファイルをダウンロード](../assets/common/sample_export_file_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv). このサンプルファイルには、プロファイル属性が含まれています `person.firstname`, `person.lastname`, `person.gender`, `person.birthyear`、および `personalEmail.address`.
