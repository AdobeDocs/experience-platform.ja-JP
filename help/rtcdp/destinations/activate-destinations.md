---
title: 宛先へのプロファイルとセグメントのアクティブ化
seo-title: 宛先へのプロファイルとセグメントのアクティブ化
description: セグメントを宛先にマッピングして、Adobe Real-time Customer Data Platformで保有するデータをアクティブ化します。 これを行うには、次の手順に従います。
seo-description: セグメントを宛先にマッピングして、Adobe Real-time Customer Data Platformで保有するデータをアクティブ化します。 これを行うには、次の手順に従います。
translation-type: tm+mt
source-git-commit: 3b9584cca8943c52bb3d8e4512d327d3dbeb9e04

---


# 宛先へのプロファイルとセグメントのアクティブ化

セグメントを宛先にマッピングして、Adobe Real-time Customer Data Platformで保有するデータをアクティブ化します。 これを行うには、次の手順に従います。

## 前提条件 {#prerequisites}

宛先へのデータをアクティブ化するには、宛先が正常に接続され [ている必要があります](/help/rtcdp/destinations/assets/connect-destination.png)。 まだ保存していない場合は、保存先カタログに移動し [、サポートされている](/help/rtcdp/destinations/destinations-catalog.md)、保存先を参照して、1つ以上の保存先を設定します。

## データのアクティブ化 {#activate-data}

1. 宛先/ **参照で**、セグメントをアクティブにする宛先を選択します。
2. 宛先の名前をクリックします。 これにより、「アクティブ化」のフローに移動します。
   ![activate-flow宛先に対するアクテ](/help/rtcdp/destinations/assets/activate-flow.png)ィブ化フローが既に存在する場合は、その宛先に現在送信されているセグメントを確認できます。 右側のレ **ールで** 「アクティベーションを編集」を選択し、以下の手順に従ってアクティベーションの詳細を変更します。
3. Select **Activate**;
4. 宛先のア **クティブ化** ウィザードの **セグメントを選択** ページで、宛先に送信するセグメントを選択します。
   ![segments-to-destination](/help/rtcdp/destinations/assets/select-segments.png)
5. *条件*。 この手順は、電子メールマーケティングの宛先にマッピングされたセグメントにのみ適用されます。 <br> 宛先属性 **ページで** 、「新し **いフィールドの追加** 」を選択し、宛先に送信する属性を選択します。
属性の1つをユニオンスキーマの一意の [識別子にす](/help/rtcdp/destinations/email-marketing-destinations.md#identity) ることをお勧めします。 必須属性について詳しくは、「電子メールマーケティングのリンク先 [のID](/help/rtcdp/destinations/email-marketing-destinations.md#identity) 」を参照してください。
   ![destination-attributes](/help/rtcdp/destinations/assets/destination-attributes.png)
6. スケジュール **ページで** 、宛先にデータを送信する開始日と、宛先にデータを送信する頻度を確認できます。
7. [レビュ **ー** ]ページには、選択の概要が表示されます。 「キャ **ンセル** 」を選択してフローを分割するか、「戻る **」を選択して設定を変更するか、「完了****** 」を選択して確定し、宛先へのデータの送信を開始します。

![確認選択](/help/rtcdp/destinations/assets/confirm-selection.png)

## アクティベーションを編集 {#edit-activation}

次の手順に従って、リアルタイムCDPの既存のアクティブ化フローを編集します。

1. 左側のナ **ビゲーション** ・バーで「Destinations」を選択し、「 **Browse** 」タブをクリックして、表示先の名前をクリックします。
2. 右側のレ **[!UICONTROL ールで]** 「アクティブ化を編集」を選択し、宛先に送信するセグメントを変更します。

## セグメントのアクティブ化が成功したことを確認します。 {#verify-activation}

### 電子メールマーケティングの宛先

電子メールマーケティングの宛先の場合、Adobe Real-time CDPは指定したストレージの場所に `.txt` タブ区切り `.csv` ファイルまたはファイルを作成します。 新しいファイルが毎日ストレージの場所に作成されることを期待する。 The file format is:
`<destination name>id<destination id><timestamp-yyyymmddhhmmss>`

3日連続で受け取るファイルは次のようになります。

```
Salesforce_id3544_20191120110000.csv
Salesforce_id3544_20191121123000.csv
Salesforce_id3544_20191122124530.csv
```

これらのファイルが保存場所に存在する場合は、アクティブ化の成功を確認します。

### 広告の宛先

データをアクティブ化する対象の広告の宛先を確認します。 アクティブ化に成功した場合、オーディエンスは広告プラットフォームに入力されます。

## アクティブ化の無効化 {#disable-activation}

既存のアクティブ化フローを無効にするには、次の手順に従います。

1. 左側のナ **ビゲーション** ・バーで「Destinations」を選択し、「 **Browse** 」タブをクリックして、表示先の名前をクリックします。
2. 右側のレール **[!UICONTROL の「有効]** 」(Enabled)コントロールをクリックして、アクティブ化フローの状態を変更します。
3. [データフロ **ー状態の更新** ]ウィンドウで、[確認 **]を選択してアク** ティブ化フローを無効にします。

