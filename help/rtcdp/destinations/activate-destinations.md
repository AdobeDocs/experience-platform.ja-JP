---
keywords: activate destination;activate destinations;activate data
title: 宛先へのプロファイルとセグメントのアクティブ化
type: Tutorial
seo-title: 宛先へのプロファイルとセグメントのアクティブ化
description: セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。
seo-description: セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '1552'
ht-degree: 28%

---


# 宛先へのプロファイルとセグメントのアクティブ化

セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。

## 前提条件 {#prerequisites}

宛先へのデータをアクティブ化するには、[宛先が接続されている](/help/rtcdp/destinations/connect-destination.md)必要があります。まだの場合は、[宛先カタログ](/help/rtcdp/destinations/destinations-catalog.md)に移動し 、サポートされている宛先を参照して、1 つ以上の宛先を設定します。

## データのアクティブ化 {#activate-data}

アクティブ化ワークフローの手順は、宛先のタイプによって少し異なります。 すべてのタイプの宛先に対する完全なワークフローを以下に示します。

### データをアクティブにする宛先の選択 {#select-destination}

適用先：すべての宛先

1. AdobeReal-time CDPユーザー・インターフェイスで、 **[!UICONTROL Destinations]** / **[!UICONTROL Browse]**（宛先）に移動し、セグメントをアクティブにする宛先を選択します。
   ![リンク先を参照](assets/oracle-eloqua-connect.png)
2. 宛先の名前をクリックします。これにより、アクティベーションワークフローに移動します。
   ![activate-flow宛先に対するアクティベーションワークフローが既に存在する場合は、目的の宛先に対して現在アクティブ化されているセグメントを確認できます。](assets/activate-flow.png)右側のパネルで「**[!UICONTROL アクティベーションの編集]**」を選択し、以下の手順に従ってアクティベーションの詳細を変更します。
3. Select **[!UICONTROL Activate]**.

<br> 

### **[!UICONTROL セグメントの選択]** 手順 {#select-segments}

適用先：すべての宛先

![セグメントの選択手順](/help/rtcdp/destinations/assets/select-segments-icon.png)


In the **[!UICONTROL Activate destination]** workflow, on the **[!UICONTROL Select Segments]** page, select one or more segments to activate to the destination. 「 **[!UICONTROL 次へ]** 」を押して次の手順に進みます。
![segments-to-destination](assets/email-select-segments.png)

<br> 

### **[!UICONTROL IDマッピング]** 手順 {#identity-mapping}

適用先：ソーシャルリンク先とGoogle Customer Matchの広告先

![IDマッピング手順](/help/rtcdp/destinations/assets/identity-mapping-icon.png)

*ソーシャル宛先の場合*、 **[!UICONTROL IDマッピング手順で]** 、宛先のターゲットIDとしてマッピングするソース属性を選択できます。 この手順は、スキーマで使用しているプライマリIDに応じて、オプションまたは必須です。 <br> 

*プライマリIDとしての電子メールアドレス*:スキーマでプライマリIDとして電子メールアドレスを使用している場合は、次に示すように、IDマッピング手順をスキップできます。

![IDとしての電子メールアドレス](assets/email-as-identity.gif)

<br> 

*プライマリIDとしての別のID*:スキーマで *報酬ID* 、 *忠誠度ID*、などの別のIDを主IDとして使用する場合は、次に示すように、IDスキーマからの電子メールアドレスをソーシャルの宛先のターゲットIDとして手動でマッピングする必要があります。

![IDとしての忠誠度ID](assets/rewardsid-as-identity.gif)


Adobe Experience Platformへのデータ取り込み時に、電子メールのハッシュ要件に従って顧客の電子メールアドレスをハッシュ化した場合は、ターゲットID `Email_LC_SHA256` として選択し [!DNL Facebook] ます [](/help/rtcdp/destinations/facebook-destination.md#email-hashing-requirements)。 <br> 使用している電子メールアドレス `Email` がハッシュ化されていない場合は、ターゲットIDを選択します。 AdobeReal-time CDPは、 [!DNL Facebook] 要件を満たすためにEメールアドレスをハッシュ化します。

![フィールドへの入力後のIDマッピング](assets/identity-mapping.png)

<br> 

### **[!UICONTROL 設定手順]** {#configure}

適用先：電子メールマーケティングの宛先とクラウドストレージの宛先

![設定手順](/help/rtcdp/destinations/assets/configure-icon.png)

この手順はオプションです。「 **[!UICONTROL 設定]** 」の手順では、書き出す各セグメントのファイル名を設定できます。 デフォルトのファイル名は、宛先名、セグメントID、日時インジケーターで構成されます。 例えば、異なるキャンペーンを区別したり、ファイルにデータの書き出し時間を付けたりするために、書き出したファイル名を編集できます。

「 **[!UICONTROL 次へ]** 」を選択してデフォルトのファイル名を使用するか、鉛筆アイコンをクリックしてモーダルウィンドウを開き、ファイル名を編集します。 ファイル名は255文字までに制限されます。

![ファイル名の設定](assets/activation-workflow-configure-step.png)

ファイル名エディターで、別のコンポーネントを選択してファイル名に追加できます。 宛先名とセグメントIDはファイル名から削除できません。 これらに加えて、次を追加できます。

* **[!UICONTROL セグメント名]**:ファイル名にセグメント名を追加できます。
* **[!UICONTROL 日時]**:形式を追加するか、ファイルが生成される時刻の10桁のタイムスタンプをUnixに追加するかを選択します。 `MMDDYYYY_HHMMSS` ファイルに、増分書き出しのたびに動的なファイル名を生成する場合は、次のいずれかのオプションを選択します。
* **[!UICONTROL カスタムテキスト]**:ファイル名追加にカスタムテキストを追加します。

「 **[!UICONTROL Apply changes]** 」を選択して、選択を確認します。

>[!IMPORTANT]
> 
>「 **[!UICONTROL 日付と時間]** 」コンポーネントを選択しない場合、ファイル名は静的になり、新しくエクスポートされたファイルによって、ストレージー上の前のファイルが上書きされ、各エクスポートで上書きされます。 ストレージの場所から電子メールマーケティングプラットフォームに定期的にインポートジョブを実行する場合は、このオプションをお勧めします。

![ファイル名の編集オプション](assets/activate-workflow-configure-step-2.png)

<br> 

### **[!UICONTROL セグメントスケジュール]** の手順 {#segment-schedule}

適用先：広告の宛先、ソーシャルの宛先

![セグメントスケジュールの手順](/help/rtcdp/destinations/assets/segment-schedule-icon.png)

On the **[!UICONTROL Segment schedule]** page, you can set the start date for sending data to the destination, as well as the frequency of sending data to the destination.

>[!IMPORTANT]
>
>ソーシャルの宛先の場合は、この手順でオーディエンスの接触チャネルを選択する必要があります。次の手順に進むには、まず次の画像のオプションのいずれかを選択してください。

![データ接触チャネルの選択](assets/choose-data-origin.png)

<br> 

### **[!UICONTROL スケジュール]** 手順 {#scheduling}

適用先：電子メールマーケティングの宛先とクラウドストレージの宛先

![セグメントスケジュールの手順](assets/scheduling-icon.png)

On the **[!UICONTROL Scheduling]** page, you can see the start date for sending data to the destination as well as the frequency of sending data to the destination. これらの値は編集できません。

<br> 

### **[!UICONTROL 属性の選択]** 手順 {#select-attributes}

適用先：電子メールマーケティングの宛先とクラウドストレージの宛先

![属性の選択手順](/help/rtcdp/destinations/assets/select-attributes-icon.png)


On the **[!UICONTROL Select Attributes]** page, select **[!UICONTROL Add new field]** and select the attributes that you want to send to the destination.

>[!NOTE]
>
> AdobeReal-time CDPは、お使いのスキーマから推奨される、一般的に使用される4つの属性を使用して、選択範囲を事前に設定します。 `person.name.firstName`、 `person.name.lastName`、 `personalEmail.address`、 `segmentMembership.status`。

ファイルのエクスポートは、選択されているかどうかに応じて、次のように異な `segmentMembership.status` ります。
* この `segmentMembership.status` フィールドを選択した場合、エクスポートされたファイルには、最初の完全なスナップショットに「アクティブ **」メンバーが含まれ、以降の増分エクスポートには「** アクティブ **」メンバーと「** 期限切れ **** 」メンバーが含まれます。
* この `segmentMembership.status` フィールドを選択しない場合、エクスポートされたファイルには、最初のフルスナップショットとそれ以降の増分エクスポートでは、 **Active** メンバーのみが含まれます。

![推奨属性](/help/rtcdp/destinations/assets/recommended-attributes.png)


We recommend one of the attributes to be a [unique identifier](/help/rtcdp/destinations/email-marketing-destinations.md#identity) from your schema. 必須属性について詳しくは、「[電子メールマーケティングの宛先](/help/rtcdp/destinations/email-marketing-destinations.md#identity)」で「ID」を参照してください。

>[!NOTE]
> 
>データセット内の特定のフィールド（データセット全体ではなく）に対してデータ使用ラベルが適用されている場合、アクティベーション上でこれらのフィールドレベルのラベルが適用されるのは、次の条件の下です。
>* これらのフィールドは、セグメント定義で使用されます。
>* フィールドは、ターゲット先の投影属性として設定されます。

>
> 
下のスクリーンショットを見てみましょう。 例えば、フィールドに、宛先のマーケティングの使用例と競合する特定のデータ使用ラベルが含まれ `person.name.firstName` ている場合、レビュー手順（手順9）でデータ使用ポリシー違反が表示されます。 詳細については、「 [Data Governance in Real-time CDP](/help/rtcdp/privacy/data-governance-overview.md#destinations)」を参照してください。

![destination-attributes](assets/select-attributes-step.png)

<br> 

### **[!UICONTROL レビュー]** 手順 {#review}

適用先：すべての宛先

![レビュー手順](/help/rtcdp/destinations/assets/review-icon.png)

「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

>[!IMPORTANT]
>
>この手順では、リアルタイムCDPがデータ使用ポリシー違反をチェックします。 次に、ポリシー違反の例を示します。 セグメントアクティベーションのワークフローは、違反を解決するまで完了できません。 ポリシー違反の解決方法について詳しくは、「データ管理ドキュメント」の「 [ポリシーの適用](/help/rtcdp/privacy/data-governance-overview.md#enforcement) 」を参照してください。

![データポリシー違反](assets/data-policy-violation.png)

ポリシー違反が検出されなかった場合は、「 **[!UICONTROL Finish]** 」を選択して、選択を確定し、開始が宛先にデータを送信することを確認します。

![confirm-selection](assets/confirm-selection.png)


## アクティベーションの編集 {#edit-activation}

リアルタイム CDP の既存のアクティベーションフローを編集するには、次の手順に従います。

1. 左側のナビゲーションバーで「**[!UICONTROL 宛先]**」を選択し、「**[!UICONTROL 参照]**」タブをクリックして、宛先名をクリックします。
2. 右側のパネルで「**[!UICONTROL アクティベーションの編集]**」を選択し、宛先に送信するセグメントを変更します。

## セグメントのアクティベーションが成功したことを確認します。 {#verify-activation}

### 電子メールマーケティングの宛先およびクラウドストレージの宛先 {#esp-and-cloud-storage}

電子メールマーケティングの宛先とクラウドストレージの宛先の場合、アドビのリアルタイム CDP はストレージの指定した場所に、タブ区切りの `.csv` または `.txt` ファイルを作成します。新しいファイルはストレージの場所に毎日作成されます。The default file format is:
`<destinationName>_segment<segmentID>_<timestamp-yyyymmddhhmmss>.csv|txt`

ファイル形式は編集できます。 詳しくは、クラウドストレージの [宛先と電子メールマーケティングの宛先を設定する手順に進んでください](/help/rtcdp/destinations/activate-destinations.md#configure) 。

デフォルトのファイル形式では、3日連続で受け取るファイルは次のようになります。

```console
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200409052200.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200410061130.csv
```

これらのファイルがストレージの場所に存在すれば、アクティベーションは成功しています。書き出したファイルの構造を理解するには、サンプルの.csvファイルを [ダウンロードし](assets/sample_export_file_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv)ます。 このサンプルファイルには、プロファイル属性 `person.firstname`、、、、お `person.lastname`よびが含まれてい `person.gender``person.birthyear``personalEmail.address`ます。

### 広告の宛先

データをアクティブ化するそれぞれの広告先のアカウントを確認します。 アクティベーションに成功した場合、オーディエンスは広告プラットフォームに入力されます。

### ソーシャルネットワークの宛先

For [!DNL Facebook], a successful activation means that a [!DNL Facebook] custom audience would be created programmatically in [[!UICONTROL Facebook Ads Manager]](https://www.facebook.com/adsmanager/manage/). ユーザーがアクティブ化されたセグメントに対してオーディエンスが資格を持つかどうかによって、ユーザーのセグメントメンバーシップが追加および削除されます。

>[!TIP]
>
>AdobeのリアルタイムCDPとの統合により、履歴オーディエンスのバックフィルが [!DNL Facebook] サポートされます。 宛先に対してセグメントをアクティブ化する [!DNL Facebook] と、すべての過去のセグメント資格情報がに送信されます。

## アクティベーションの無効化 {#disable-activation}

既存のアクティベーションフローを無効化するには、次の手順に従います。

1. 左側のナビゲーションバーで「**[!UICONTROL 宛先]**」を選択し、「**[!UICONTROL 参照]**」タブをクリックして、宛先名をクリックします。
2. 右側のパネルの「**[!UICONTROL 有効]**」コントロールをクリックして、アクティベーションフローの状態を変更します。
3. **データフロー状態の更新**&#x200B;ウィンドウで、「**確認**」を選択してアクティベーションフローを無効にします。
