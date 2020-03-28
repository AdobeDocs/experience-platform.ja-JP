---
title: 宛先へのプロファイルとセグメントのアクティブ化
seo-title: 宛先へのプロファイルとセグメントのアクティブ化
description: セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。
seo-description: セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。
translation-type: tm+mt
source-git-commit: 336aa90cf1e059a92a36dd0ef3222ef6a6f5123b

---


# 宛先へのプロファイルとセグメントのアクティブ化

セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。

## 前提条件 {#prerequisites}

宛先へのデータをアクティブ化するには、[宛先が接続されている](/help/rtcdp/destinations/assets/connect-destination.png)必要があります。まだの場合は、[宛先カタログ](/help/rtcdp/destinations/destinations-catalog.md)に移動し 、サポートされている宛先を参照して、1 つ以上の宛先を設定します。

## データのアクティブ化 {#activate-data}

1. In **[!UICONTROL Destinations > Browse]**, select the destination where you want to activate your segments.
2. 宛先の名前をクリックします。これにより、「アクティブ化」のフローに移動します。
   ![activate-flow](/help/rtcdp/destinations/assets/activate-flow.png)
宛先に対するアクティベーションフローが既に存在する場合は、その宛先に現在送信されているセグメントを確認できます。Select **[!UICONTROL Edit activation]** in the right rail and follow the steps below to modify the activation details.
3. Select **[!UICONTROL Activate]**;
4. ワークフロー **[!UICONTROL Activate destination]** のページで、 **[!UICONTROL Select Segments]** 送信先に送信するセグメントを選択します。
   ![segments-to-destination](/help/rtcdp/destinations/assets/select-segments.png)
5. *オプション*&#x200B;この手順は、電子メールマーケティングの宛先にマッピングされたセグメントにのみ適用されます。<br> ページ **[!UICONTROL Destination Attributes]** で、宛先 **[!UICONTROL Add new field]** に送信する属性を選択します。
属性の 1 つをユニオンスキーマの[一意の識別子](/help/rtcdp/destinations/email-marketing-destinations.md#identity)にすることをお勧めします。必須属性について詳しくは、「[電子メールマーケティングの宛先](/help/rtcdp/destinations/email-marketing-destinations.md#identity)」で「ID」を参照してください。
   ![destination-attributes](/help/rtcdp/destinations/assets/destination-attributes.png)
6. On the **[!UICONTROL Schedule]** page, you can see the start date for sending data to the destination, as well as the frequency of sending data to the destination.
7. On the **[!UICONTROL Review]** page, you can see a summary of your selection. Select **[!UICONTROL Cancel]** to break up the flow, **[!UICONTROL Back]** to modify your settings, or **[!UICONTROL Finish]** to confirm your selection and start sending data to the destination.

![confirm-selection](/help/rtcdp/destinations/assets/confirm-selection.png)

## アクティベーションの編集 {#edit-activation}

Real-time CDP の既存のアクティベーションフローを編集するには、次の手順に従います。

1. Select **[!UICONTROL Destinations]** in the left navigation bar, then click the **[!UICONTROL Browse]** tab, and click the destination name.
2. Select **[!UICONTROL Edit activation]** in the right rail to change which segments to send to the destination.

## セグメントのアクティベーションが成功したことを確認します。 {#verify-activation}

### 電子メールマーケティングの宛先 およびクラウドストレージの宛先

For email marketing destinations and cloud storage destinations, Adobe Real-time CDP creates a tab-delimited `.txt` or `.csv` file in the storage location that you provided. 新しいファイルはストレージの場所に毎日作成されます。ファイル形式は、`<destination name>id<destination id><timestamp-yyyymmddhhmmss>` です。

3 日連続で受け取るファイルは次のようになります。

```
Salesforce_id3544_20191120110000.csv
Salesforce_id3544_20191121123000.csv
Salesforce_id3544_20191122124530.csv
```

これらのファイルがストレージの場所に存在すれば、アクティベーションは成功しています。

### 広告の宛先

データをアクティブ化する対象の広告の宛先を確認します。アクティベーションに成功した場合、オーディエンスは広告プラットフォームに入力されます。

## アクティベーションの無効化 {#disable-activation}

既存のアクティベーションフローを無効化するには、次の手順に従います。

1. Select **[!UICONTROL Destinations]** in the left navigation bar, then click the **[!UICONTROL Browse]** tab, and click the destination name.
2. Click the **[!UICONTROL Enabled]** control in the right rail to change the activation flow state.
3. 「**データフロー状態の更新**」ウィンドウで、「**確認**」を選択してアクティベーションフローを無効にします。

