---
title: 宛先へのプロファイルとセグメントのアクティブ化
seo-title: 宛先へのプロファイルとセグメントのアクティブ化
description: セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。
seo-description: セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。
translation-type: tm+mt
source-git-commit: 24e4746b28620210c138a1e803b6afadff79ab30
workflow-type: tm+mt
source-wordcount: '889'
ht-degree: 53%

---


# 宛先へのプロファイルとセグメントのアクティブ化

セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。

## 前提条件 {#prerequisites}

宛先へのデータをアクティブ化するには、[宛先が接続されている](/help/rtcdp/destinations/assets/connect-destination-1.png)必要があります。まだの場合は、[宛先カタログ](/help/rtcdp/destinations/destinations-catalog.md)に移動し 、サポートされている宛先を参照して、1 つ以上の宛先を設定します。

## データのアクティブ化 {#activate-data}

1. **[!UICONTROL 宛先／参照]**&#x200B;で、セグメントをアクティブ化する宛先を選択します。
2. 宛先の名前をクリックします。これにより、「アクティブ化」のフローに移動します。
   ![activate-flow](/help/rtcdp/destinations/assets/activate-flow.png)
宛先に対するアクティベーションフローが既に存在する場合は、その宛先に現在送信されているセグメントを確認できます。右側のレールで「**[!UICONTROL アクティベーションの編集]**」を選択し、以下の手順に従ってアクティベーションの詳細を変更します。
3. **[!UICONTROL アクティブ化]**&#x200B;を選択します。
4. In the **[!UICONTROL Activate destination]** workflow, on the **[!UICONTROL Select Segments]** page, select which segments to send to the destination.
   ![segments-to-destination](/help/rtcdp/destinations/assets/select-segments.png)
5. *オプション*&#x200B;この手順は、セグメントをアクティブ化する宛先のタイプによって異なります。 <br> *電子メールマーケティングの宛先* 、 *クラウドストレージの宛先については、属性を*&#x200B;選択 **[!UICONTROL ページで、]****** 新しいフィールドを選択し、宛先に送信する属性を選択します。
属性の 1 つをユニオンスキーマの[一意の識別子](/help/rtcdp/destinations/email-marketing-destinations.md#identity)にすることをお勧めします。必須属性について詳しくは、「[電子メールマーケティングの宛先](/help/rtcdp/destinations/email-marketing-destinations.md#identity)」で「ID」を参照してください。
   ![destination-attributes](/help/rtcdp/destinations/assets/select-attributes-step.png)

   <br> 

   *ソーシャル宛先の場合*、 **[!UICONTROL IDマッピング手順で]** 、宛先のターゲットIDとしてマッピングするソース属性を選択できます。 この手順は、スキーマで使用しているプライマリIDに応じて、オプションまたは必須です。 <br> 

   *プライマリIDとしての電子メールアドレス*: スキーマでプライマリIDとして電子メールアドレスを使用している場合は、次に示すように、IDマッピング手順をスキップできます。

   ![IDとしての電子メールアドレス](/help/rtcdp/destinations/assets/email-as-identity.gif)

   <br> 

   *プライマリIDとしての別のID*: スキーマで *報酬ID* 、 *忠誠度ID*、などの別のIDを主IDとして使用する場合は、次に示すように、IDスキーマからの電子メールアドレスをソーシャルの宛先のターゲットIDとして手動でマッピングする必要があります。

   ![IDとしての忠誠度ID](/help/rtcdp/destinations/assets/rewardsid-as-identity.gif)


   Facebookの電子メールハッシュ要件 `Email_LC_SHA256` に従って、Adobe Experience Platformにデータを取り込む際に顧客の電子メールアドレスをハッシュ化した場合は、ターゲットIDとして選択 [します](/help/rtcdp/destinations/facebook-destination.md#email-hashing-requirements)。 <br> 使用している電子メールアドレス `Email` がハッシュ化されていない場合は、ターゲットIDを選択します。 Adobe Real-time CDPは、Facebookの要件に準拠するために電子メールアドレスをハッシュします。

   ![フィールドへの入力後のIDマッピング](/help/rtcdp/destinations/assets/identity-mapping.png)

6. On the **[!UICONTROL Segment schedule]** page, you can see the start date for sending data to the destination, as well as the frequency of sending data to the destination.

   >[!IMPORTANT]
   >
   >ソーシャルターゲットの場合は、この手順でオーディエンスの接触チャネルを選択する必要があります。 次の手順に進むには、次の画像のオプションの1つを選択してから行います。

   ![データ接触チャネルの選択](/help/rtcdp/destinations/assets/choose-data-origin.png)

7. 「**[!UICONTROL 確認]**」ページには、選択の概要が表示されます。「**[!UICONTROL キャンセル]**」を選択してフローを分割するか、「**[!UICONTROL 戻る]**」を選択して設定を変更する、または、「**[!UICONTROL 完了]**」を選択して確定し、宛先へのデータの送信を開始します。

![confirm-selection](/help/rtcdp/destinations/assets/confirm-selection.png)

## アクティベーションの編集 {#edit-activation}

Real-time CDP の既存のアクティベーションフローを編集するには、次の手順に従います。

1. 左側のナビゲーションバーで「**[!UICONTROL 宛先]**」を選択し、「**[!UICONTROL 参照]**」タブをクリックして、宛先名をクリックします。
2. 右側のレールで「**[!UICONTROL アクティベーションの編集]**」を選択し、宛先に送信するセグメントを変更します。

## セグメントのアクティベーションが成功したことを確認します。 {#verify-activation}

### 電子メールマーケティングの宛先およびクラウドストレージの宛先

電子メールマーケティングの宛先とクラウドストレージの宛先の場合、Adobe Real-time CDP はストレージの指定した場所に、タブ区切りの `.txt` または `.csv` ファイルを作成します。新しいファイルはストレージの場所に毎日作成されます。ファイル形式は、`<destination name>id<destination id><timestamp-yyyymmddhhmmss>` です。

3 日連続で受け取るファイルは次のようになります。

```
Salesforce_id3544_20191120110000.csv
Salesforce_id3544_20191121123000.csv
Salesforce_id3544_20191122124530.csv
```

これらのファイルがストレージの場所に存在すれば、アクティベーションは成功しています。

### 広告の宛先

データをアクティブ化する対象の広告の宛先を確認します。アクティベーションに成功した場合、オーディエンスは広告プラットフォームに入力されます。

### ソーシャルネットワークの宛先

Facebookの場合、アクティベーションが成功すると、Facebookのカスタムオーディエンスが [Facebook広告マネージャーでプログラム的に作成され](https://www.facebook.com/adsmanager/manage/)ます。 ユーザーがアクティブ化されたセグメントに対して資格を持つか資格を失うかにより、オーディエンスのセグメントメンバーシップが追加および削除されます。

>[!TIP]
>
>Adobe Real-time CDPとFacebookの統合により、履歴オーディエンスのバックフィルがサポートされます。 宛先にセグメントをアクティブ化すると、すべての過去のセグメント資格情報がFacebookに送信されます。

## アクティベーションの無効化 {#disable-activation}

既存のアクティベーションフローを無効化するには、次の手順に従います。

1. 左側のナビゲーションバーで「**[!UICONTROL 宛先]**」を選択し、「**[!UICONTROL 参照]**」タブをクリックして、宛先名をクリックします。
2. 右側のレールの「**[!UICONTROL 有効]**」コントロールをクリックして、アクティベーションフローの状態を変更します。
3. 「**データフロー状態の更新**」ウィンドウで、「**確認**」を選択してアクティベーションフローを無効にします。

AWS Kinesisで、アクセスキー — 秘密アクセスキーペアを生成し、Adobe Real-time CDPにAWS Kinesisアカウントへのアクセスを許可します。 詳しくは、 [AWS Kinesisドキュメント](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)を参照してください。