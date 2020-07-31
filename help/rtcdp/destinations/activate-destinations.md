---
title: 宛先へのプロファイルとセグメントのアクティブ化
seo-title: 宛先へのプロファイルとセグメントのアクティブ化
description: セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。
seo-description: セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。
translation-type: tm+mt
source-git-commit: 098dd31be4d6ee6971cd87bcbfe0f686e34918e1
workflow-type: tm+mt
source-wordcount: '1036'
ht-degree: 56%

---


# 宛先へのプロファイルとセグメントのアクティブ化

セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。

## 前提条件 {#prerequisites}

宛先へのデータをアクティブ化するには、[宛先が接続されている](/help/rtcdp/destinations/connect-destination.md)必要があります。まだの場合は、[宛先カタログ](/help/rtcdp/destinations/destinations-catalog.md)に移動し 、サポートされている宛先を参照して、1 つ以上の宛先を設定します。

## データのアクティブ化 {#activate-data}

1. **[!UICONTROL 宛先／参照]**&#x200B;で、セグメントをアクティブ化する宛先を選択します。
2. 宛先の名前をクリックします。これにより、「アクティブ化」のフローに移動します。
   ![activate-flow](/help/rtcdp/destinations/assets/activate-flow.png)
宛先に対するアクティベーションフローが既に存在する場合は、その宛先に現在送信されているセグメントを確認できます。右側のパネルで「**[!UICONTROL アクティベーションの編集]**」を選択し、以下の手順に従ってアクティベーションの詳細を変更します。
3. **[!UICONTROL アクティブ化]**&#x200B;を選択します。
4. **[!UICONTROL 宛先のアクティブ化]**&#x200B;ワークフローの「**[!UICONTROL セグメントを選択]**」ページで、宛先に送信するセグメントを選択します。
   ![segments-to-destination](/help/rtcdp/destinations/assets/email-select-segments.png)
5. *オプション*&#x200B;この手順は、セグメントをアクティブ化する宛先のタイプによって異なります。 <br> *電子メールマーケティングの宛先* 、 *クラウドストレージの宛先については、属性を*&#x200B;選択 **[!UICONTROL ページで、]****** 新しいフィールドを選択し、宛先に送信する属性を選択します。
属性の 1 つをユニオンスキーマの[一意の識別子](/help/rtcdp/destinations/email-marketing-destinations.md#identity)にすることをお勧めします。必須属性について詳しくは、「[電子メールマーケティングの宛先](/help/rtcdp/destinations/email-marketing-destinations.md#identity)」で「ID」を参照してください。

   >[!NOTE]
   > 
   >データセット内の特定のフィールド（データセット全体ではなく）に対してデータ使用ラベルが適用されている場合、アクティベーション上でこれらのフィールドレベルのラベルが適用されるのは、次の条件の下です。
   >* これらのフィールドは、セグメント定義で使用されます。
   >* フィールドは、ターゲット先の投影属性として設定されます。

   >
   > 下のスクリーンショットを見てみましょう。 例えば、フィールドに、宛先のマーケティングの使用例と競合する特定のデータ使用ラベルが含まれ `person.name.first.Name` ている場合、レビュー手順（手順7）でデータ使用ポリシー違反が表示されます。 詳細については、「 [Data Governance in Real-time CDP」を参照してください。](/help/rtcdp/privacy/data-governance-overview.md#destinations)

   ![destination-attributes](/help/rtcdp/destinations/assets/select-attributes-step.png)

   <br> 

   *ソーシャル宛先の場合*、 **[!UICONTROL IDマッピング手順で]** 、宛先のターゲットIDとしてマッピングするソース属性を選択できます。 この手順は、スキーマで使用しているプライマリIDに応じて、オプションまたは必須です。 <br> 

   *プライマリIDとしての電子メールアドレス*: スキーマでプライマリIDとして電子メールアドレスを使用している場合は、次に示すように、IDマッピング手順をスキップできます。

   ![IDとしての電子メールアドレス](/help/rtcdp/destinations/assets/email-as-identity.gif)

   <br> 

   *プライマリIDとしての別のID*: スキーマで *報酬ID* 、 *忠誠度ID*、などの別のIDを主IDとして使用する場合は、次に示すように、IDスキーマからの電子メールアドレスをソーシャルの宛先のターゲットIDとして手動でマッピングする必要があります。

   ![IDとしての忠誠度ID](/help/rtcdp/destinations/assets/rewardsid-as-identity.gif)


   データ取り込み時 `Email_LC_SHA256` にターゲットの電子メールアドレスをAdobe Experience Platformにハッシュ化する場合は、電子メールのハッシュ要件に従って、 [!DNL Facebook] IDとして選択します [](/help/rtcdp/destinations/facebook-destination.md#email-hashing-requirements)。 <br> 使用している電子メールアドレス `Email` がハッシュ化されていない場合は、ターゲットIDを選択します。 AdobeReal-time CDPは、 [!DNL Facebook] 要件を満たすためにEメールアドレスをハッシュ化します。

   ![フィールドへの入力後のIDマッピング](/help/rtcdp/destinations/assets/identity-mapping.png)

6. 「**[!UICONTROL セグメントスケジュール]**」ページで 、宛先へのデータ送信の開始日と頻度を確認できます。

   >[!IMPORTANT]
   >
   >ソーシャルの宛先の場合は、この手順でオーディエンスの接触チャネルを選択する必要があります。次の手順に進むには、まず次の画像のオプションのいずれかを選択してください。

   ![データ接触チャネルの選択](/help/rtcdp/destinations/assets/choose-data-origin.png)

7. 「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

   >[!IMPORTANT]
   >
   >この手順では、リアルタイムCDPがデータ使用ポリシー違反をチェックします。 次に、ポリシー違反の例を示します。 セグメントアクティベーションのワークフローは、違反を解決するまで完了できません。 ポリシー違反の解決方法について詳しくは、「データ管理ドキュメント」の「 [ポリシーの適用](/help/rtcdp/privacy/data-governance-overview.md#enforcement) 」を参照してください。

![confirm-selection](/help/rtcdp/destinations/assets/data-policy-violation.png)

ポリシー違反が検出されなかった場合は、「 **[!UICONTROL Finish]** 」を選択して、選択を確定し、開始が宛先にデータを送信することを確認します。

![confirm-selection](/help/rtcdp/destinations/assets/confirm-selection.png)



## アクティベーションの編集 {#edit-activation}

リアルタイム CDP の既存のアクティベーションフローを編集するには、次の手順に従います。

1. 左側のナビゲーションバーで「**[!UICONTROL 宛先]**」を選択し、「**[!UICONTROL 参照]**」タブをクリックして、宛先名をクリックします。
2. 右側のパネルで「**[!UICONTROL アクティベーションの編集]**」を選択し、宛先に送信するセグメントを変更します。

## セグメントのアクティベーションが成功したことを確認します。 {#verify-activation}

### 電子メールマーケティングの宛先およびクラウドストレージの宛先 {#esp-and-cloud-storage}

電子メールマーケティングの宛先とクラウドストレージの宛先の場合、アドビのリアルタイム CDP はストレージの指定した場所に、タブ区切りの `.txt` または `.csv` ファイルを作成します。新しいファイルはストレージの場所に毎日作成されます。ファイル形式は、`<destinationName>_segment<segmentID>_<timestamp-yyyymmddhhmmss>.csv` です。

3 日連続で受け取るファイルは次のようになります。

```
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200409052200.csv
Salesforce_Marketing_Cloud_segment12341e18-abcd-49c2-836d-123c88e76c39_20200410061130.csv
```

これらのファイルがストレージの場所に存在すれば、アクティベーションは成功しています。書き出したファイルの構造を理解するには、サンプルの.csvファイルを [ダウンロードし](/help/rtcdp/destinations/assets/sample_export_file_segment12341e18-abcd-49c2-836d-123c88e76c39_20200408061804.csv)ます。 このサンプルファイルには、プロファイル属性 `person.firstname`、、、、お `person.lastname`よびが含まれてい `person.gender``person.birthyear``personalEmail.address`ます。

### 広告の宛先

データをアクティブ化する対象の広告の宛先を確認します。アクティベーションに成功した場合、オーディエンスは広告プラットフォームに入力されます。

### ソーシャルネットワークの宛先

For [!DNL Facebook], a successful activation means that a [!DNL Facebook] custom audience would be created programmatically in [Facebook Ads Manager](https://www.facebook.com/adsmanager/manage/). ユーザーがアクティブ化されたセグメントに対してオーディエンスが資格を持つかどうかによって、ユーザーのセグメントメンバーシップが追加および削除されます。

>[!TIP]
>
>AdobeのリアルタイムCDPとの統合により、履歴オーディエンスのバックフィルが [!DNL Facebook] サポートされます。 宛先に対してセグメントをアクティブ化する [!DNL Facebook] と、すべての過去のセグメント資格情報がに送信されます。

## アクティベーションの無効化 {#disable-activation}

既存のアクティベーションフローを無効化するには、次の手順に従います。

1. 左側のナビゲーションバーで「**[!UICONTROL 宛先]**」を選択し、「**[!UICONTROL 参照]**」タブをクリックして、宛先名をクリックします。
2. 右側のパネルの「**[!UICONTROL 有効]**」コントロールをクリックして、アクティベーションフローの状態を変更します。
3. **データフロー状態の更新**&#x200B;ウィンドウで、「**確認**」を選択してアクティベーションフローを無効にします。
