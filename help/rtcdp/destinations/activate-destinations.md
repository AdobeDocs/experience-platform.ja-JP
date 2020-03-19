---
title: 宛先へのプロファイルとセグメントのアクティブ化
seo-title: 宛先へのプロファイルとセグメントのアクティブ化
description: セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。
seo-description: セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。
translation-type: tm+mt
source-git-commit: 73925aa59f9981d8945fb0be6c4924e1831cf902

---


# 宛先へのプロファイルとセグメントのアクティブ化

セグメントを宛先にマッピングして、アドビのリアルタイム顧客データプラットフォームで保有するデータをアクティブ化します。これをおこなうには、次の手順に従います。

## 前提条件 {#prerequisites}

宛先へのデータをアクティブ化するには、[宛先が接続されている](/help/rtcdp/destinations/assets/connect-destination.png)必要があります。まだの場合は、[宛先カタログ](/help/rtcdp/destinations/destinations-catalog.md)に移動し 、サポートされている宛先を参照して、1 つ以上の宛先を設定します。

## データのアクティブ化 {#activate-data}

1. **宛先／参照**&#x200B;で、セグメントをアクティブ化する宛先を選択します。
2. 宛先の名前をクリックします。これにより、「アクティブ化」のフローに移動します。
   ![activate-flow](/help/rtcdp/destinations/assets/activate-flow.png)
宛先に対するアクティベーションフローが既に存在する場合は、その宛先に現在送信されているセグメントを確認できます。右側のレールで「**アクティベーションの編集**」を選択し、以下の手順に従ってアクティベーションの詳細を変更します。
3. **アクティブ化**&#x200B;を選択します。
4. **宛先のアクティブ化**&#x200B;ウィザードの&#x200B;**セグメントを選択**ページで、宛先に送信するセグメントを選択します。
   ![segments-to-destination](/help/rtcdp/destinations/assets/select-segments.png)
5. *オプション*&#x200B;この手順は、電子メールマーケティングの宛先にマッピングされたセグメントにのみ適用されます。「<br>宛先属性&#x200B;**」ページで**、「**新しいフィールドの追加**」を選択し、宛先に送信する属性を選択します。
属性の 1 つをユニオンスキーマの[一意の識別子](/help/rtcdp/destinations/email-marketing-destinations.md#identity)にすることをお勧めします。必須属性について詳しくは、「[電子メールマーケティングの宛先](/help/rtcdp/destinations/email-marketing-destinations.md#identity)」で「ID」を参照してください。
   ![destination-attributes](/help/rtcdp/destinations/assets/destination-attributes.png)
6. 「**スケジュール**」ページで 、宛先へのデータ送信の開始日と頻度を確認できます。
7. 「**確認**」ページには、選択の概要が表示されます。「**キャンセル**」を選択してフローを分割するか、「**戻る**」を選択して設定を変更する、または、「**完了**」を選択して確定し、宛先へのデータの送信を開始します。

![confirm-selection](/help/rtcdp/destinations/assets/confirm-selection.png)

## アクティベーションの編集 {#edit-activation}

Real-time CDP の既存のアクティベーションフローを編集するには、次の手順に従います。

1. 左側のナビゲーションバーで「**宛先**」を選択し、「**参照**」タブをクリックして、宛先名をクリックします。
2. 右側のレールで「**[!UICONTROL アクティベーションの編集]**」を選択し、宛先に送信するセグメントを変更します。

## セグメントのアクティベーションが成功したことを確認します。 {#verify-activation}

### 電子メールマーケティングの宛先 クラウドストレージの宛先

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

1. 左側のナビゲーションバーで「**宛先**」を選択し、「**参照**」タブをクリックして、宛先名をクリックします。
2. 右側のレールの「**[!UICONTROL 有効]**」コントロールをクリックして、アクティベーションフローの状態を変更します。
3. 「**データフロー状態の更新**」ウィンドウで、「**確認**」を選択してアクティベーションフローを無効にします。

