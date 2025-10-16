---
title: キャピラリーストリーミングイベントの概要
description: キャピラリからExperience Platformにデータをストリーミングする方法を説明します。
badge: ベータ版
exl-id: 3b8eb2f6-3b4a-4b91-89d4-b6d9027c6ab4
source-git-commit: 428aed259343f56a2bf493b40ff2388340fffb7b
workflow-type: tm+mt
source-wordcount: '554'
ht-degree: 2%

---

# [!DNL Capillary Streaming Events]

>[!AVAILABILITY]
>
>[!DNL Capillary Streaming Events] ソースはベータ版です。ベータラベル付きソースの使用について詳しくは、ソースの概要の [&#x200B; 利用条件 &#x200B;](../../home.md#terms-and-conditions) を参照してください。

[!DNL Capillary Technologies] は、世界中の 300 以上のブランドから信頼されている、主要なロイヤルティおよびエンゲージメントプラットフォームです。 [!DNL Capillary Streaming Events] ソースを使用すると、ビジネスが顧客プロファイル、ロイヤルティデータ、トランザクションイベントを、[!DNL Capillary] からAdobe Experience Platformへとシームレスにストリーミングできるようになります。 [!DNL Capillary] ソースを接続すると、リアルタイムのパーソナライゼーション、高度なオーディエンスのセグメント化、オムニチャネルキャンペーンオーケストレーションが可能になります。

[!DNL Capillary] をExperience Platformと統合すると、次のことが可能になります。

* **ロイヤルティポイント、階層および報酬** をリアルタイムで同期します。
* **トランザクションデータ** をExperience Platformに送信して、分析とアクティベーションを行います。
* Real-Time CDP、Experience Platform、Adobe Journey Optimizerを活用して、セグメント化、Journey Orchestration、パーソナライゼーションを行います。

## 前提条件

[!DNL Capillary] をAdobe Experience Platformに接続する前に、次の点を確認してください。

* 有効な **Adobe組織 ID** と、有効なExperience Platform サンドボックスへのアクセス。
* **[!UICONTROL アカウントをExperience Platformに接続するには、アカウントで]** ソースの表示 **[!UICONTROL および]** ソースの管理 [!DNL Capillary] 権限の両方が有効になっている必要があります。 必要な権限を取得するには、製品管理者にお問い合わせください。 詳しくは、[&#x200B; アクセス制御 UI ガイド &#x200B;](../../../access-control/ui/overview.md) を参照してください。

### スキーマの作成

エクスペリエンスデータモデル（XDM）スキーマを作成して、[!DNL Capillary] から送信される可能なフィールドとデータタイプを保存できるデータセットを記述する必要があります。

1. Adobe Experience Platformにログインし、組織のログインからExperience Platformにアクセスします。
2. 左側のナビゲーションパネルで「**[!UICONTROL スキーマ]**」を選択して、「[!UICONTROL &#x200B; スキーマ &#x200B;] ワークスペースを開きます。
3. 右上隅の「**[!UICONTROL スキーマを作成]**」を選択します。
4. スキーマを作成ダイアログで、**[!UICONTROL 手動作成]** （フィールドとフィールドグループを自分で追加）または **[!UICONTROL ML 支援による作成]** （CSV ファイルをアップロードし、機械学習を使用して推奨スキーマを生成）を選択します。
5. スキーマの基本クラス（例：XDM 個人プロファイル、XDM ExperienceEvent、その他）を選択します。 **[!UICONTROL その他]** を選択した場合は、使用可能なカスタム クラスまたは標準クラスから選択できます。
6. スキーマのわかりやすい名前と説明を入力します。
7. スキーマエディターを使用して、フィールドグループ（再利用可能なフィールドブロック）を追加し、個々のフィールドを定義し（名前、データタイプ、オプションをカスタマイズ）、既存のデータタイプがニーズに合わない場合は、オプションでカスタムデータタイプまたはフィールドグループを作成します。
8. キャンバスでスキーマ構造を確認します。 「**[!UICONTROL 完了]**」を選択して、スキーマを作成します。
9. （オプション）スキーマエディターで、必要に応じてフィールドを編集し、説明を追加して、フィールドグループを調整します。

XDM スキーマの作成方法に関する詳細な手順については、[&#x200B; スキーマエディターを使用したスキーマの作成 &#x200B;](../../../xdm/tutorials/create-schema-ui.md) に関するガイドを参照してください。

### データセットの作成

次に、作成したばかりのスキーマを参照するデータセットを作成する必要があります。

1. Experience Platform UI の左側のナビゲーションで「[!UICONTROL &#x200B; データセット &#x200B;]」を選択して、「[!UICONTROL &#x200B; データセット &#x200B;] ワークスペースを開きます。
2. 右上の「**[!UICONTROL データセットを作成]**」を選択します。
3. 作成オプションで、「**[!UICONTROL スキーマからデータセットを作成]**」を選択します。
4. リストから、以前に作成した XDM スキーマを検索して選択します。 スキーマを見つけたら、「**[!UICONTROL 次へ]**」を選択します。
5. データセットの一意のわかりやすい名前を入力します。
6. オプションで、今後のユーザーがデータセットを識別しやすくする説明を追加します。
7. 「**[!UICONTROL 完了]**」を選択して、データセットを作成します。

データセットの作成方法に関する詳細な手順については、[&#x200B; データセット UI ガイド &#x200B;](../../../catalog/datasets/user-guide.md) を参照してください。

## [!DNL Capillary Streaming Events] をExperience Platformに接続

[!DNL Capillary] の前提条件の設定が完了したら、[[!DNL Capillary Streaming Events] UI チュートリアル &#x200B;](../../tutorials/ui/create/loyalty/capillary.md) を読んで、[!DNL Capillary] からExperience Platformにアカウントを接続し、データをストリーミングする方法を学習します。
