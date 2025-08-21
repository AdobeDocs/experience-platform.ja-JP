---
title: 2024 年 11 月以前に作成されたデータフローのデータセット書き出しスケジュールを拡張します
description: 2024 年 11 月より前に作成され、2025 年 9 月 1 日に機能が停止するデータセット書き出しデータフローの書き出しスケジュールを拡張する方法を説明します。
type: Tutorial
exl-id: a756886b-3f4b-4427-bd26-817221ba68aa
source-git-commit: 6f8b906729ec31cc0c4847ccd0ae0f89f63a1627
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 0%

---

# 2024 年 11 月以前に作成されたデータフローのデータセット書き出しスケジュールを拡張します

>[!IMPORTANT]
>
>**アクションが必要**:2024 年 11 月より前に組織で [ データセット書き出しデータフロー ](export-datasets.md) が作成されている場合、これらのデータフローは 2025 年 9 月 1 日に機能しなくなります。 このガイドでは、保持するデータフローの書き出しスケジュールをこの日付を超えて延長する方法について説明します。

## 概要 {#overview}

Experience Platformの [2024 年 9 月リリース ](/help/release-notes/2024/september-2024.md#destinations) では、Adobeは、2024 年 9 月リリースより前に作成されたすべてのデータセット書き出しデータフローのデフォルト終了日 **2025 年 5 月 1 日** を導入しました。

**2024 年 11 月以前** に作成したすべてのデータセット書き出しデータフローについて、この日付は 2025 年 9 月 1 日 **に更新されました**。

2024 年 11 月より前に作成されたデータセット書き出しデータフローでは、有効期限を手動で延長しない限り、**2025 年 9 月 1 日** にデータの書き出しが停止します。

**2025 年 9 月 1 日** 以降もデータの書き出しを維持するためにデータフローが必要な場合、このガイドの手順に従って、データセットを書き出す各宛先のスケジュールを拡張する必要があります。

## 影響を受ける宛先 {#affected-destinations}

組織には、以下にリストされている宛先にデータを送信するアクティブなデータセット書き出しデータフローがある場合があります。 次の節の手順に従い、チュートリアルビデオを視聴して、どのデータセットに有効期限が設定されているかを特定する方法を説明します。

* [[!DNL Azure Data Lake Storage Gen2]](../catalog/cloud-storage/adls-gen2.md)
* [[!DNL Data Landing Zone]](../catalog/cloud-storage/data-landing-zone.md)
* [[!DNL Google Cloud Storage]](../catalog/cloud-storage/google-cloud-storage.md)
* [[!DNL Amazon S3]](../catalog/cloud-storage/amazon-s3.md#changelog)
* [[!DNL Azure Blob]](../catalog/cloud-storage/azure-blob.md#changelog)
* [[!DNL SFTP]](../catalog/cloud-storage/sftp.md#changelog)
* [[!DNL Marketo Measure Ultimate]](../catalog/adobe/marketo-measure-ultimate.md)

## ビデオチュートリアル {#video}

終了日が予定されているデータセットの書き出しを特定し、保持するデータフローの書き出しスケジュールを延長する方法の手順を示すデモについては、以下のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3470518/)

## 手順 1：影響を受けるデータフローの特定 {#identify-dataflows}

データセット書き出しデータフローの書き出しスケジュールを拡張する前に、まず、今後の有効期限の影響を受けるデータフローを特定する必要があります。 アクションが必要なデータフローを見つけるには、次の手順に従います。

1. Experience Platform UI で **[!UICONTROL 宛先]**/**[!UICONTROL カタログ]** に移動します。
2. アクティブなデータセット書き出しデータフローがある宛先で「**[!UICONTROL アクティブ化]**」を選択します。

   >[!TIP]
   >
   >カタログの左側にある **[!UICONTROL データタイプ]** フィルターを使用して、使用可能な宛先を **[!UICONTROL データセット]** でフィルタリングします。

3. **[!UICONTROL データセット]** データタイプを選択して、データセット書き出しを含むデータフローのみを表示します。
   ![ データタイプでデータフローをフィルタリングする方法を示すスクリーンショット。](/help/destinations/assets/ui/export-datasets/dataset-type.png)
4. **[!UICONTROL 作成済み]** 列ヘッダーを選択し、**[!UICONTROL 昇順で並べ替え]** を選択して古いデータフローを表示します。
   ![ データフローを昇順に並べ替える方法を示すスクリーンショット。](/help/destinations/assets/ui/export-datasets/sort-ascending.png)
5. 2024 年 11 月より前に作成されたデータフローのうち、保持するデータフローを特定します。

## 手順 2：データセット書き出しワークフローへのアクセス {#access-workflow}

保持するデータフローごとに、データセットを書き出しワークフローにアクセスして、スケジュールを変更する必要があります。

1. **[!UICONTROL 名前]** 列のデータフロー名を選択します。 これにより、「データフローの実行 **[!UICONTROL ページが表示さ]** ます。
2. このページで、「**[!UICONTROL データセットを書き出し]**」オプションを選択します。
   ![ データフロー実行ページの「データセットを書き出し」オプションを示すスクリーンショット。](/help/destinations/assets/ui/export-datasets/export-datasets-option.png)
3. **[!UICONTROL データセットを選択]** ページで「**[!UICONTROL 次へ]**」を選択します。 データフローに新しいデータセットを追加する必要はありません。
4. これにより、**[!UICONTROL スケジュール]** ページに移動し、データセット書き出しの有効期限を知らせる通知も表示されます。
   ![ 有効期限通知を含むデータセット書き出しデータフロー ](/help/destinations/assets/ui/export-datasets/dataset-export-notification.png)

## 手順 3：書き出しスケジュールの拡張 {#extend-export-schedule}

これで、書き出しスケジュールを変更して、2025 年 9 月 1 日を超えて拡張できます。

1. **[!UICONTROL スケジュールを編集]** を選択します。
   ![ 「スケジュールを編集」ボタンを示すスケジュール設定ステップのスクリーンショット。](/help/destinations/assets/ui/export-datasets/edit-schedule.png)
2. 新しい書き出しスケジュールを選択し、「**[!UICONTROL 保存]**」を選択します。
   ![ スケジュールオプションを示すスケジュール設定ステップのスクリーンショット。](/help/destinations/assets/ui/export-datasets/edit-schedule-calendar.png)

   >[!TIP]
   >
   >データセット書き出しスケジュールの設定方法に関する詳細なガイダンスについては、[ データセット書き出しドキュメント ](export-datasets.md#scheduling) を参照してください。

## 2025 年 9 月 1 日の期限に間に合わなかった場合はどうなりますか？ {#missed-deadline}

データセット書き出しデータフローが 2025 年 9 月 1 日に期限切れになり、スケジュールを延長していない場合は、**30 日の猶予期間があり** データが失われることなくデータフローを再度有効にするためにAdobeに連絡できます。 これには、9 月 1 日からAdobeに連絡した日付までの間に書き出されなかったデータが含まれます。

>[!IMPORTANT]
>
>Adobeはこの猶予期間を提供しますが、データの書き出しが中断されないようにし、サービスが中断される可能性を避けるために、2025 年 9 月 1 日の期限より前にスケジュールを延長することを強くお勧めします。
