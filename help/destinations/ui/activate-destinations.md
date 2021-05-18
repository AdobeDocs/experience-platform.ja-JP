---
keywords: 宛先をアクティブ化；宛先をアクティブ化；データをアクティブ化
title: 宛先へのプロファイルとセグメントのアクティブ化
type: Tutorial
seo-title: 宛先へのプロファイルとセグメントのアクティブ化
description: セグメントを宛先にマッピングして、Adobe Experience Platformでのデータをアクティブ化します。 これをおこなうには、次の手順に従います。
seo-description: セグメントを宛先にマッピングして、Adobe Experience Platformでのデータをアクティブ化します。 これをおこなうには、次の手順に従います。
exl-id: c3792046-ffa8-4851-918f-98ced8b8a835
source-git-commit: 70be44e919070df910d618af4507b600ad51123c
workflow-type: tm+mt
source-wordcount: '2566'
ht-degree: 12%

---

# 宛先へのプロファイルとセグメントのアクティブ化

## 概要 {#overview}

セグメントを宛先にマッピングして、[!DNL Adobe Experience Platform]にあるデータをアクティブにします。 これをおこなうには、次の手順に従います。

## 前提条件 {#prerequisites}

宛先へのデータをアクティブ化するには、[宛先が接続されている](./connect-destination.md)必要があります。まだの場合は、[宛先カタログ](../catalog/overview.md)に移動し 、サポートされている宛先を参照して、1 つ以上の宛先を設定します。

## データのアクティブ化 {#activate-data}

アクティベーションワークフローの手順は、宛先のタイプによって若干異なります。 すべてのタイプの宛先に対する完全なワークフローを以下に示します。

## {#select-destination}へのデータのアクティブ化先を選択

適用先：すべての宛先

Adobe Experience Platformユーザーインターフェイスで、**[!UICONTROL 宛先]**/**[!UICONTROL 参照]**&#x200B;に移動し、次の図に示すように、セグメントをアクティブにする宛先に対応する&#x200B;**[!UICONTROL アクティブ化]**&#x200B;ボタンをクリックします。

![行き先にアクティブ化](../assets/ui/activate-destinations/browse-tab-activate.png)

次のセクションの手順に従って、アクティブにするセグメントを選択します。

## [!UICONTROL セグメントの選択] 手順  {#select-segments}

適用先：すべての宛先

![セグメントの選択手順](../assets/ui/activate-destinations/select-segments-icon.png)

**[!UICONTROL 宛先]**&#x200B;をアクティブ化ワークフローの&#x200B;**[!UICONTROL セグメントを選択]**&#x200B;ページで、宛先にアクティブ化する1つ以上のセグメントを選択します。 「**[!UICONTROL 次へ]**」を選択して、次の手順に進みます。

![segments-to-destination](../assets/ui/activate-destinations/email-select-segments.png)

## [!UICONTROL ID] マッピング手順  {#identity-mapping}

適用先：ソーシャルリンク先とGoogle Customer Matchの広告先

![IDマッピング手順](../assets/ui/activate-destinations/identity-mapping-icon.png)

ソーシャル宛先の場合は、ソース属性またはID名前空間を選択して、宛先のターゲットIDとしてマップする必要があります。

## 例：[!DNL Facebook Custom Audience] {#example-facebook}でのオーディエンスデータのアクティブ化

以下は、[!DNL Facebook]でオーディエンスデータをアクティブ化する際の正しいIDマッピングの例です。

ソースフィールドの選択：

* 使用している電子メールアドレスがハッシュ化されていない場合は、`Email`名前空間をソースIDとして選択します。
* [!DNL Facebook] [電子メールハッシュ要件](../catalog/social/facebook.md#email-hashing-requirements)に従って、データインジェスト時に顧客の電子メールアドレスを[!DNL Platform]にハッシュした場合は、`Email_LC_SHA256`名前空間をソースIDとして選択します。
* データがハッシュ化されていない電話番号で構成されている場合は、`PHONE_E.164`名前空間をソースIDとして選択します。 [!DNL Platform] は、 [!DNL Facebook] 要件に合わせて電話番号をハッシュします。
* [!DNL Facebook] [電話番号ハッシュ要件](../catalog/social/facebook.md#phone-number-hashing-requirements)に従って、[!DNL Platform]にデータ取り込む際の電話番号をハッシュする場合は、`Phone_SHA256`名前空間をソースIDとして選択します。
* データが[!DNL Apple]デバイスIDで構成されている場合は、`IDFA`名前空間をソースIDとして選択します。
* データが[!DNL Android]デバイスIDで構成されている場合は、`GAID`名前空間をソースIDとして選択します。
* データが他の種類の識別子で構成されている場合は、`Custom`名前空間をソースIDとして選択します。

ターゲットフィールドの選択：

* ソース名前空間が`Email`または`Email_LC_SHA256`の場合は、`Email_LC_SHA256`名前空間をターゲットIDとして選択します。
* ソース名前空間が`PHONE_E.164`または`Phone_SHA256`の場合は、`Phone_SHA256`名前空間をターゲットIDとして選択します。
* ソース名前空間が`IDFA`または`GAID`の場合は、`IDFA`または`GAID`名前空間をターゲットIDとして選択します。
* ソース名前空間がカスタム名前空間の場合は、`Extern_ID`をターゲットIDとして選択します。

![IDマッピング](../assets/ui/activate-destinations/identity-mapping.png)

非ハッシュ化された名前空間のデータは、アクティベーション時に[!DNL Platform]によって自動的にハッシュされます。

属性ソースデータは自動的にハッシュされません。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティベーション上のデータを自動的にハッシュ化します。
[!DNL Platform]![IDマッピング変換](../assets/ui/activate-destinations/identity-mapping-transformation.png)

 

## 例：[!DNL Google Customer Match] {#example-gcm}でのオーディエンスデータのアクティブ化

これは、[!DNL Google Customer Match]でオーディエンスデータをアクティブ化する際の正しいIDマッピングの例です。

ソースフィールドの選択：

* 使用している電子メールアドレスがハッシュ化されていない場合は、`Email`名前空間をソースIDとして選択します。
* [!DNL Google Customer Match] [電子メールハッシュ要件](../catalog/social/../advertising/google-customer-match.md)に従って、データインジェスト時に顧客の電子メールアドレスを[!DNL Platform]にハッシュした場合は、`Email_LC_SHA256`名前空間をソースIDとして選択します。
* データがハッシュ化されていない電話番号で構成されている場合は、`PHONE_E.164`名前空間をソースIDとして選択します。 [!DNL Platform] は、 [!DNL Google Customer Match] 要件に合わせて電話番号をハッシュします。
* [!DNL Facebook] [電話番号ハッシュ要件](../catalog/social/../advertising/google-customer-match.md)に従って、[!DNL Platform]にデータ取り込む際の電話番号をハッシュする場合は、`Phone_SHA256_E.164`名前空間をソースIDとして選択します。
* データが[!DNL Apple]デバイスIDで構成されている場合は、`IDFA`名前空間をソースIDとして選択します。
* データが[!DNL Android]デバイスIDで構成されている場合は、`GAID`名前空間をソースIDとして選択します。
* データが他の種類の識別子で構成されている場合は、`Custom`名前空間をソースIDとして選択します。

ターゲットフィールドの選択：

* ソース名前空間が`Email`または`Email_LC_SHA256`の場合は、`Email_LC_SHA256`名前空間をターゲットIDとして選択します。
* ソース名前空間が`PHONE_E.164`または`Phone_SHA256_E.164`の場合は、`Phone_SHA256_E.164`名前空間をターゲットIDとして選択します。
* ソース名前空間が`IDFA`または`GAID`の場合は、`IDFA`または`GAID`名前空間をターゲットIDとして選択します。
* ソース名前空間がカスタム名前空間の場合は、`User_ID`をターゲットIDとして選択します。

![IDマッピング](../assets/ui/activate-destinations/identity-mapping-gcm.png)

非ハッシュ化された名前空間のデータは、アクティベーション時に[!DNL Platform]によって自動的にハッシュされます。

属性ソースデータは自動的にハッシュされません。 ソースフィールドにハッシュ化されていない属性が含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティベーション上のデータを自動的にハッシュ化します。
[!DNL Platform]![IDマッピング変換](../assets/ui/activate-destinations/identity-mapping-gcm-transformation.png)

## **** スケジュールの手順 {#scheduling}

適用先：電子メールマーケティングの宛先とクラウドストレージの宛先

![スケジュール手順](../assets/ui/activate-destinations/scheduling-icon.png)

[!DNL Adobe Experience Platform] 電子メールマーケティングおよびクラウドストレージの宛先のデータを [!DNL CSV] ファイルの形式でエクスポートします。**[!UICONTROL スケジュール]**&#x200B;の手順では、エクスポートする各セグメントのスケジュールとファイル名を設定できます。 スケジュールの設定は必須ですが、ファイル名の設定はオプションです。

>[!IMPORTANT]
> 
>[!DNL Adobe Experience Platform] 1ファイルあたり500万件のレコード（行）でエクスポートファイルを自動的に分割します。各行は1つのプロファイルを表します。
>
>分割ファイル名には、そのファイルが大きな書き出しの一部であることを示す番号が付加されます。次に例を示します。`filename.csv`、`filename_2.csv`、`filename_3.csv`。

送信先に送信するセグメントに対応する「**[!UICONTROL スケジュール]**&#x200B;を作成」ボタンを選択します。

![スケジュールを作成ボタン](../assets/ui/activate-destinations/create-schedule-button.png)

### フルファイルのエクスポート{#export-full-files}

[**[!UICONTROL フルファイルをエクスポート]**]を選択すると、エクスポートしたファイルに、そのセグメントに該当するすべてのプロファイルの完全なスナップショットが含まれます。

![フルファイルの書き出し](../assets/ui/activate-destinations/export-full-files.png)

1. **[!UICONTROL 頻度]**&#x200B;セレクターを使用して、1回限り（**[!UICONTROL 1回]**）または&#x200B;**[!UICONTROL 毎日]**&#x200B;のエクスポートを選択します。 フルファイルを&#x200B;**[!UICONTROL 毎日]**&#x200B;エクスポートすると、毎日開始日から終了日の午前12:00（米国東部標準時の午後7:00）に、ファイルがエクスポートされます。
2. **[!UICONTROL 時間]**&#x200B;セレクターを使用して、エクスポートを実行する時刻を[!DNL UTC]形式で選択します。 ファイル&#x200B;**[!UICONTROL 毎日]**&#x200B;をエクスポートすると、毎日、開始日から選択した時刻の終了日まで、ファイルがエクスポートされます。

   >[!IMPORTANT]
   >
   >特定の時間帯にファイルをエクスポートするオプションは、現在ベータ版であり、一部のお客様のみ利用できます。

3. **[!UICONTROL 日付]**&#x200B;セレクターを使用して、エクスポートを実行する日または間隔を選択します。
4. 「**[!UICONTROL 作成]**」を選択して、スケジュールを保存します。

### 増分ファイルのエクスポート{#export-incremental-files}

「**[!UICONTROL 増分ファイルをエクスポート]**」を選択すると、エクスポートしたファイルに、最後のエクスポート以降にそのセグメントに適したプロファイルのみが含まれます。

>[!IMPORTANT]
>
>最初にエクスポートされた増分ファイルには、セグメントに該当するすべてのプロファイルが含まれ、バックフィルとして機能します。

![増分ファイルの書き出し](../assets/ui/activate-destinations/export-incremental-files.png)

1. **[!UICONTROL 頻度]**&#x200B;セレクターを使用して、**[!UICONTROL 毎日]**&#x200B;または&#x200B;**[!UICONTROL 時間別]**&#x200B;のエクスポートを選択します。 増分ファイル&#x200B;**[!UICONTROL 日別]**&#x200B;は、開始日から終了日の午後12:00 UTC（米国東部標準時の7:00 AM）に、毎日ファイルをエクスポートします。
   * 「**[!UICONTROL 時間別]**」を選択する場合は、**[!UICONTROL 各]**&#x200B;セレクターを使用して、**[!UICONTROL 3]**、**[!UICONTROL 6]**、**[!UICONTROL 8]**、**[!UICONTROL 12]**&#x200B;時間のオプションから選択します。

      >[!IMPORTANT]
      >
      >3、6、8、12時間ごとにインクリメンタルファイルを書き出すオプションは、現在ベータ版であり、お客様の数によってのみご利用いただけます。 ベータ版以外のお客様は、増分ファイルを1日1回エクスポートできます。

2. **[!UICONTROL 時間]**&#x200B;セレクターを使用して、エクスポートを実行する時刻を[!DNL UTC]形式で選択します。

   >[!IMPORTANT]
   >
   >エクスポートの時刻を選択するオプションは、選択した数の顧客のみが使用できます。 ベータ版以外のお客様は、増分ファイルを日1回、米国時(UTC)の午後12:00（米国東部標準時の午前7:00）にエクスポートできます。

3. **[!UICONTROL 日付]**&#x200B;セレクターを使用して、エクスポートを実行する日または間隔を選択します。
4. 「**[!UICONTROL 作成]**」を選択して、スケジュールを保存します。

### ファイル名を構成{#file-names}

デフォルトのファイル名は、宛先名、セグメントID、日時インジケーターで構成されます。 例えば、異なるキャンペーンを区別したり、ファイルにデータの書き出し時間を付けたりするために、書き出したファイル名を編集できます。

鉛筆アイコンを選択してモーダルウィンドウを開き、ファイル名を編集します。 ファイル名は255文字までに制限されます。

![ファイル名の設定](../assets/ui/activate-destinations/configure-name.png)

ファイル名エディターで、別のコンポーネントを選択してファイル名に追加できます。

![ファイル名の編集オプション](../assets/ui/activate-destinations/activate-workflow-configure-step-2.png)

宛先名とセグメントIDはファイル名から削除できません。 これらに加えて、次を追加できます。

* **[!UICONTROL セグメント名]**:ファイル名にセグメント名を追加できます。
* **[!UICONTROL 日時]**:フ `MMDDYYYY_HHMMSS` ォーマットを追加するか、ファイルが生成された時刻の10桁のタイムスタンプをUnixに追加するかを選択します。ファイルに、増分書き出しのたびに動的なファイル名を生成する場合は、次のいずれかのオプションを選択します。
* **[!UICONTROL カスタムテキスト]**:ファイル名追加にカスタムテキストを追加します。

「**[!UICONTROL 変更を適用]**」を選択して、選択を確定します。

>[!IMPORTANT]
> 
>**[!UICONTROL 日時]**&#x200B;コンポーネントを選択しない場合、ファイル名は静的になり、新しく書き出されたファイルによってストレージー上の以前のファイルが上書きされ、各書き出しで上書きされます。 ストレージの場所から電子メールマーケティングプラットフォームに定期的にインポートジョブを実行する場合は、このオプションをお勧めします。

すべてのセグメントの設定が完了したら、「**[!UICONTROL 次へ]**」を選択して続行します。

## **[!UICONTROL セグメントの]** スケジュール  {#segment-schedule}

適用先：広告の宛先、ソーシャルの宛先

![セグメントスケジュールの手順](../assets/ui/activate-destinations/segment-schedule-icon.png)

**[!UICONTROL セグメントスケジュール]**&#x200B;ページで、送信先にデータを送信する開始日と、送信先にデータを送信する頻度を設定できます。

>[!IMPORTANT]
>
>ソーシャルの宛先の場合は、この手順でオーディエンスの接触チャネルを選択する必要があります。次の手順に進むには、まず次の画像のオプションのいずれかを選択してください。

![Facebookオーディエンス接触チャネル](../assets/catalog/social/facebook/facebook-origin-audience.png)

>[!IMPORTANT]
>
>Google Customer Matchの場合、[!DNL IDFA]または[!DNL GAID]セグメントをアクティブ化する際に、この手順で[!UICONTROL アプリID]を指定する必要があります。

![アプリidを入力](../assets/catalog/advertising/google-customer-match/gcm-destination-appid.png)

## **[!UICONTROL 属性]** 手順の選択  {#select-attributes}

適用先：電子メールマーケティングの宛先とクラウドストレージの宛先

![属性の選択手順](../assets/ui/activate-destinations/select-attributes-icon.png)

**[!UICONTROL 属性を選択]**&#x200B;ページで、**[!UICONTROL 追加新しいフィールド]**&#x200B;を選択し、送信先に送信する属性を選択します。

>[!NOTE]
>
> Adobe Experience Platformは、スキーマで推奨され、よく使用される4つの属性を使用して、選択範囲を事前入力します。`person.name.firstName`、`person.name.lastName`、`personalEmail.address`、`segmentMembership.status`。

`segmentMembership.status`が選択されているかどうかによって、ファイルのエクスポートは次の方法で異なります。
* `segmentMembership.status`フィールドを選択した場合、エクスポートされたファイルには、最初のフルスナップショットに「**[!UICONTROL アクティブ]**」メンバーが含まれ、以降の増分エクスポートには「**[!UICONTROL アクティブ]**」メンバーと「**[!UICONTROL 期限切れ]**」メンバーが含まれます。
* `segmentMembership.status`フィールドが選択されていない場合、エクスポートされたファイルには、最初のフルスナップショットと以降の増分エクスポートに&#x200B;**[!UICONTROL アクティブ]**&#x200B;メンバーのみが含まれます。

![推奨属性](../assets/ui/activate-destinations/mandatory-deduplication.png)

### 必須属性{#mandatory-attributes}

属性を必須としてマーク付けすると、[!DNL Platform]は、特定の属性を含むプロファイルのみをエクスポートします。 その結果、フィルタリングの追加形式として使用できます。 属性を必須としてマークするのは、**必須ではありません**。

必須属性を選択しないと、その属性に関係なく、すべての修飾プロファイルがエクスポートされます。

属性の1つは、スキーマーの[一意の識別子](../../destinations/catalog/email-marketing/overview.md#identity)にすることをお勧めします。 必須属性について詳しくは、[電子メールマーケティングの宛先](../../destinations/catalog/email-marketing/overview.md#identity)ドキュメントのIDに関する節を参照してください。

### 重複排除 - 重複キー{#deduplication-keys}

>[!IMPORTANT]
>
>重複排除 - 重複キーを使用するオプションは現在ベータ版で、一部のお客様のみ利用できます。

重複排除 - 重複キーを使用すると、同じプロファイルの複数のレコードを1つのエクスポートファイルに含めることができなくなります。

[!DNL Platform]で重複排除 - 重複キーを使用する方法は3つあります。

* 単一のID名前空間を[!UICONTROL 重複排除 - 重複キー]として使用する
* [!DNL XDM]プロファイルの単一のプロファイル属性を[!UICONTROL 重複排除 - 重複キー]として使用する
* [!DNL XDM]プロファイルの2つのプロファイル属性を組み合わせて複合キーとして使用する

>[!IMPORTANT]
>
> 1つのID名前空間を宛先に書き出すことができ、名前空間は自動的に重複排除 - 重複キーとして設定されます。 宛先への複数の名前空間の送信はサポートされていません。
> 
> ID名前空間とプロファイル属性の組み合わせを重複排除 - 重複キーとして使用することはできません。

### 重複排除 - 重複の例{#deduplication-example}

次の例は、選択した重複排除 - 重複キーに応じた重複排除 - 重複の動作を示します。

次の2つのプロファイルを考えてみましょう。

**プロファイルA**

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

**プロファイルB**

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

### 重複排除 - 重複の使用例1:重複排除 - 重複なし

重複排除 - 重複を使用しない場合、エクスポートファイルには次のエントリが含まれます。

| personalEmail | firstName | lastName |
|---|---|---|
| johndoe@example.com | John | Doe |
| johndoe@example.com | John | D |


### 重複排除 - 重複の使用例2:ID名前空間に基づく重複排除 - 重複

[!DNL Email]名前空間による重複排除 - 重複を想定すると、エクスポートファイルには次のエントリが含まれます。 プロファイルBはセグメントに適した最新のものなので、エクスポートできるのはこれだけです。

| Email* | personalEmail | firstName | lastName |
|---|---|---|---|
| johndoe_1@example.com | johndoe@example.com | John | D |
| johndoe_2@example.com | johndoe@example.com | John | D |

### 重複排除 - 重複の使用例3:単一のプロファイル属性に基づく重複排除 - 重複

`personal Email`属性で重複排除 - 重複した場合、エクスポートファイルには次のエントリが含まれます。 プロファイルBはセグメントに適した最新のものなので、エクスポートできるのはこれだけです。

| personalEmail* | firstName | lastName |
|---|---|---|
| johndoe@example.com | John | D |


### 重複排除 - 重複の使用例4:2つのプロファイル属性に基づく重複排除 - 重複(複合重複排除 - 重複キー)

複合キー`personalEmail + lastName`による重複排除 - 重複を想定すると、エクスポートファイルには次のエントリが含まれます。

| personalEmail* | lastName* | firstName |
|---|---|---|
| johndoe@example.com | D | John |
| johndoe@example.com | Doe | John |


Adobeでは、すべてのプロファイルレコードが一意に識別されるように、[!DNL CRM ID]や電子メールアドレスなどのID名前空間を重複排除 - 重複キーとして選択することをお勧めします。

>[!NOTE]
> 
>データセット内の特定のフィールド（データセット全体ではなく）に対してデータ使用ラベルが適用されている場合、アクティベーション上でこれらのフィールドレベルのラベルが適用されるのは、次の条件の下です。
>
>* これらのフィールドは、セグメント定義で使用されます。
>* フィールドは、ターゲット先の投影属性として設定されます。

>
> 
例えば、フィールド`person.name.firstName`に、宛先のマーケティングアクションと競合する特定のデータ使用ラベルが含まれている場合、レビュー手順でデータ使用ポリシー違反が表示されます。 詳しくは、[Adobe Experience Platform](../../rtcdp/privacy/data-governance-overview.md#destinations)でのデータ管理を参照してください。

## **[!UICONTROL Reviewstep]**  {#review}

適用先：すべての宛先

![レビュー手順](../assets/ui/activate-destinations/review-icon.png)

「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>この手順では、Adobe Experience Platformがデータ使用ポリシーの違反を確認します。 次に、ポリシー違反の例を示します。 セグメントアクティベーションのワークフローは、違反を解決するまで完了できません。 ポリシー違反の解決方法について詳しくは、「データ管理ドキュメント」の「[ポリシーの適用](../../rtcdp/privacy/data-governance-overview.md#enforcement)」を参照してください。

![データポリシー違反](../assets/common/data-policy-violation.png)

ポリシー違反が検出されなかった場合は、[**[!UICONTROL 完了]**]を選択して、選択と開始が宛先にデータを送信することを確認します。

![confirm-selection](../assets/ui/activate-destinations/confirm-selection.png)

## セグメントのアクティベーションが成功したことを確認します。 {#verify-activation}

### 電子メールマーケティングの宛先およびクラウドストレージの宛先  {#esp-and-cloud-storage}

電子メールマーケティングのリンク先とクラウドストレージのリンク先の場合、Adobe Experience Platformは指定したストレージーの場所にタブ区切りの`.csv`ファイルを作成します。 新しいファイルはストレージの場所に毎日作成されます。デフォルトのファイル形式は次のとおりです。
`<destinationName>_segment<segmentID>_<timestamp-yyyymmddhhmmss>.csv`

3 日連続で受け取るファイルは次のようになります。

```console
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200409052200.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200410061130.csv
```

これらのファイルがストレージの場所に存在すれば、アクティベーションは成功しています。書き出したファイルの構造を理解するには、サンプルの.csvファイル](../assets/common/sample_export_file_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv)を[ダウンロードします。 このサンプルファイルには、プロファイル属性`person.firstname`、`person.lastname`、`person.gender`、`person.birthyear`、`personalEmail.address`が含まれます。

## 広告の宛先

データをアクティブ化するそれぞれの広告先のアカウントを確認します。 アクティベーションに成功した場合、オーディエンスは広告プラットフォームに入力されます。

## ソーシャルの宛先

[!DNL Facebook]の場合、アクティベーションが成功したときは、[[!UICONTROL Facebook広告マネージャー]](https://www.facebook.com/adsmanager/manage/)に[!DNL Facebook]カスタムオーディエンスがプログラム的に作成されることを意味します。 ユーザーがアクティブ化されたセグメントに対してオーディエンスが資格を持つかどうかによって、ユーザーのセグメントメンバーシップが追加および削除されます。

>[!TIP]
>
>Adobe Experience Platformと[!DNL Facebook]の統合は、履歴オーディエンスのバックフィルをサポートします。 宛先に対してセグメントをアクティブ化すると、すべての過去のセグメント資格情報が[!DNL Facebook]に送信されます。

## アクティベーションの無効化 {#disable-activation}

既存のアクティベーションフローを無効化するには、次の手順に従います。

1. 左側のナビゲーションバーで「**[!UICONTROL 宛先]**」を選択し、「**[!UICONTROL 参照]**」タブをクリックして、宛先名をクリックします。
2. 右側のパネルの「**[!UICONTROL 有効]**」コントロールをクリックして、アクティベーションフローの状態を変更します。
3. **データフロー状態の更新**&#x200B;ウィンドウで、「**確認**」を選択してアクティベーションフローを無効にします。
