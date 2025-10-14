---
title: Experience Platform UI を使用した、オンデマンドによるバッチ宛先へのファイルの書き出し
type: Tutorial
description: Experience Platform UI を使用して、オンデマンドでファイルをバッチ宛先に書き出す方法を説明します。
exl-id: 0cbe5089-b73d-4584-8451-2fc34d47c357
source-git-commit: d3bd76f5b36b6a6afcb67fe923eb8e4f3d7a9415
workflow-type: tm+mt
source-wordcount: '690'
ht-degree: 16%

---


# Experience Platform UI を使用した、オンデマンドによるバッチ宛先へのファイルの書き出し

>[!IMPORTANT]
> 
>データをアクティブ化するには、**[!UICONTROL 宛先の表示]**、**[!UICONTROL 宛先のアクティブ化]**、**[!UICONTROL プロファイルの表示]** および **[!UICONTROL セグメントの表示]**&#x200B;[&#x200B; アクセス制御権限 &#x200B;](/help/access-control/home.md#permissions) が必要です。 [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

## **[!UICONTROL 今すぐファイルを書き出し]**&#x200B;の概要 {#overview}

>[!CONTEXTUALHELP]
>id="platform_destinations_activationchaining_activatenow"
>title="今すぐファイルを書き出し"
>abstract="このコントロールを選択すると、以前にスケジュールされた書き出しに加えて完全なファイル書き出しが実行されます。ファイルの書き出しが直ちにトリガーされ、Experience Platform のセグメント化実行から最新の結果が取得されます。"

この記事では、Experience Platform UI を使用して、[&#x200B; クラウドストレージ &#x200B;](/help/destinations/catalog/cloud-storage/overview.md) や [&#x200B; メールマーケティング &#x200B;](/help/destinations/catalog/email-marketing/overview.md) の宛先など、オンデマンドでファイルをバッチ宛先に書き出す方法について説明します。

**[!UICONTROL 今すぐファイルを書き出し]** コントロールを使用すると、以前にスケジュールされたオーディエンスの現在の書き出しスケジュールを中断することなく、完全なファイルを書き出すことができます。 この書き出しは、以前にスケジュールされた書き出しに加えて行われ、オーディエンスの書き出し頻度は変更されません。 ファイルの書き出しが直ちにトリガーされ、Experience Platform のセグメント化実行から最新の結果が取得されます。

この目的で、Experience Platform API を使用することもできます。 [&#x200B; アドホックアクティベーション API を使用して、オンデマンドでオーディエンスをバッチ宛先に対してアクティブ化する &#x200B;](/help/destinations/api/ad-hoc-activation-api.md) 方法を参照してください。

## 前提条件 {#prerequisites}

オンデマンドでファイルをバッチ宛先に書き出すには、正常に [&#x200B; 宛先に接続 &#x200B;](./connect-destination.md) されている必要があります。 まだ接続していない場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照し、使用する宛先を設定します。

## オンデマンドでファイルを書き出す方法 {#how-to-export-files-on-demand}

1. **[!UICONTROL 接続/宛先]** に移動し、「**[!UICONTROL 参照]** タブとフィルター記号を選択して、目的のバッチ宛先に対する既存の接続を表示します。

   ![&#x200B; 「参照」タブに移動して既存のデータフローをフィルタリングする方法をハイライト表示した画像。](../assets/ui/activate-on-demand/browse-tab.png)

2. 目的の宛先接続を選択して、宛先への既存のデータフローを検査します。

   ![&#x200B; フィルターされたデータフローをハイライト表示した画像。](../assets/ui/activate-on-demand/filtered-dataflow.png)

3. 「**[!UICONTROL アクティベーションデータ]**」タブを選択し、オンデマンドでファイルを書き出すオーディエンスを選択し、「**[!UICONTROL ファイルを今すぐ書き出し]**」コントロールを選択して、1 回限りの書き出しをトリガーにします。これにより、選択した各オーディエンスのファイルがバッチ宛先に配信されます。

   ![&#x200B; 「今すぐファイルを書き出し」ボタンをハイライト表示した画像。](../assets/ui/activate-on-demand/bulk-export-file-now.png)

4. **[!UICONTROL はい]** を選択して、ファイルの書き出しを確認してトリガーします。

   ![&#x200B; 「今すぐファイルを書き出し」確認ダイアログを示す画像。](../assets/ui/activate-on-demand/confirm-activation.png)

5. 確認メッセージが表示され、ファイルの書き出しが開始されたことが示されます。

   ![&#x200B; アドホックアクティベーションが成功したことを確認する画像 &#x200B;](../assets/ui/activate-on-demand/ad-hoc-success.png)

6. 「**[!UICONTROL データフロー実行]**」タブに切り替えて、ファイルの書き出しが開始されたことを確認することもできます。

## 注意点 {#considerations}

**[!UICONTROL 今すぐファイルを書き出す]** コントロールを使用する場合は、次の点に注意してください。

* **[!UICONTROL 今すぐファイルを書き出す]** は、バッチアクティベーションデータフローのスケジュールが現在の日付と重複するオーディエンスに対してのみ機能します。 これには、終了日（書き出し頻度 **[!UICONTROL 1 回]**）がない、または終了日がまだ過ぎていないスケジュールのオーディエンスが含まれます。
* 既存のデータフローにオーディエンスを追加する場合は、少なくとも **1 時間** 待ってから、「今すぐファイルを書き出し **[!UICONTROL コントロールを使用し]** ください。
* オーディエンスの結合ポリシーを変更した場合、または新しい結合ポリシーを使用するオーディエンスを作成した場合は、「**[!UICONTROL ファイルを今すぐ書き出し]**」コントロールを使用するまで 24 時間待ちます。

## UI エラーメッセージ {#ui-error-messages}

**[!UICONTROL 今すぐファイルを書き出す]** コントロールを使用すると、以下に示すエラーメッセージが表示される場合があります。 テーブルを確認して、表示されたときに対処する方法を理解します。

| エラーメッセージ | 解決策 |
|---------|----------|
| 実行 ID `flow run ID` の注文 `dataflow ID` に対して、オーディエンス `segment ID` に対する実行は既に行われています | このエラーメッセージは、アドホックアクティベーションフローが現在オーディエンスに対して進行中であることを示しています。 ジョブが終了するのを待ってから、アクティベーションジョブを再度トリガーします。 |
| オーディエンス `<segment name>` このデータフローの一部ではないか、スケジュール範囲外です。 | このエラーメッセージは、アクティブ化するように選択したオーディエンスがデータフローにマッピングされていないか、オーディエンスに対して設定されたアクティベーションスケジュールが期限切れか、まだ開始されていないことを示しています。 オーディエンスが実際にデータフローにマッピングされているかどうかを確認し、オーディエンスのアクティベーションスケジュールが現在の日付と重なっていることを確認します。 |

## 関連情報 {#related-information}

* [Experience Platform API を使用して、オンデマンドでバッチ宛先に対してオーディエンスをアクティブ化します](/help/destinations/api/ad-hoc-activation-api.md)
* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](/help/destinations/ui/activate-batch-profile-destinations.md)
