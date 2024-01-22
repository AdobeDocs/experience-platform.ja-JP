---
title: （ベータ版）Experience Platform UI を使用した、オンデマンドによるバッチ保存先へのファイルの書き出し
type: Tutorial
description: Experience PlatformUI を使用して、オンデマンドでファイルをバッチ保存先に書き出す方法を説明します。
exl-id: 0cbe5089-b73d-4584-8451-2fc34d47c357
source-git-commit: fbc2a6c81682797af4674adabff358a62d973007
workflow-type: tm+mt
source-wordcount: '743'
ht-degree: 20%

---

# （ベータ版）Experience Platform UI を使用した、オンデマンドによるバッチ保存先へのファイルの書き出し

>[!IMPORTANT]
>
>The **[!UICONTROL ファイルを今すぐ書き出し]** 」オプションは現在ベータ版です。 ドキュメントと機能は変更される場合があります。
>この機能に対するアクセス権については、アドビ担当者にお問い合わせください。

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の表示]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). [アクセス制御の概要](/help/access-control/ui/overview.md)を参照するか、製品管理者に問い合わせて必要な権限を取得してください。

## **[!UICONTROL 今すぐファイルを書き出し]**&#x200B;の概要 {#overview}

>[!CONTEXTUALHELP]
>id="platform_destinations_activationchaining_activatenow"
>title="今すぐファイルを書き出し"
>abstract="このコントロールを選択すると、以前にスケジュールされた書き出しに加えて完全なファイル書き出しが実行されます。ファイルの書き出しが直ちにトリガーされ、Experience Platform のセグメント化実行から最新の結果が取得されます。"

この記事では、Experience PlatformUI を使用して、オンデマンドでファイルをバッチ保存先 ( 例： [クラウドストレージ](/help/destinations/catalog/cloud-storage/overview.md) および [電子メールマーケティング](/help/destinations/catalog/email-marketing/overview.md) 宛先。

The **[!UICONTROL ファイルを今すぐ書き出し]** 「 」コントロールを使用すると、以前にスケジュールされたオーディエンスの現在の書き出しスケジュールを中断することなく、完全なファイルを書き出すことができます。 このエクスポートは、以前にスケジュールされたエクスポートに加えておこなわれ、オーディエンスのエクスポート頻度は変更されません。 ファイルの書き出しが直ちにトリガーされ、Experience Platform のセグメント化実行から最新の結果が取得されます。

この目的でExperience PlatformAPI を使用することもできます。 方法を読む [アドホックアクティベーション API を使用して、オーディエンスをオンデマンドでバッチ保存先にアクティブ化します](/help/destinations/api/ad-hoc-activation-api.md).

## 前提条件 {#prerequisites}

オンデマンドでファイルをバッチ保存先に書き出すには、 [宛先に接続されている](./connect-destination.md). まだ接続していない場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照し、使用する宛先を設定します。

## オンデマンドでファイルを書き出す方法 {#how-to-export-files-on-demand}

1. に移動します。 **[!UICONTROL 接続/宛先]**&#x200B;を選択し、 **[!UICONTROL 参照]** タブおよびフィルター記号を使用して、目的のバッチ保存先への既存の接続を表示します。

   ![「参照」タブに移動し、既存のデータフローをフィルタリングする方法を強調した画像。](../assets/ui/activate-on-demand/browse-tab.png)

2. 目的の宛先接続を選択して、宛先への既存のデータフローを検査します。

   ![フィルタリングしたデータフローをハイライトした画像。](../assets/ui/activate-on-demand/filtered-dataflow.png)

3. を選択します。 **[!UICONTROL アクティベーションデータ]** 」タブをクリックし、オンデマンドでファイルを書き出すオーディエンスを選択して、 **[!UICONTROL ファイルを今すぐ書き出し]** ファイルをバッチ保存先に配信する 1 回限りのエクスポートをトリガーするためのコントロール。

   >[!IMPORTANT]
   >
   >オンデマンドでファイルを一括で書き出す複数のオーディエンスの選択は、現在、UI ではサポートされていません。 以下を使用します。 [アドホックアクティベーション API](/help/destinations/api/ad-hoc-activation-api.md) そのために。

   ![「ファイルを今すぐ書き出し」ボタンをハイライトした画像。](../assets/ui/activate-on-demand/activate-segment-on-demand.png)

4. 選択 **[!UICONTROL はい]** をクリックして、ファイルのエクスポートを確認してトリガーを設定します。

   ![「ファイルを今すぐ書き出し」確認ダイアログを示す画像。](../assets/ui/activate-on-demand/confirm-activation.png)

5. ファイルのエクスポートが開始されたことを知らせる確認メッセージが表示されます。

   ![アドホックアクティベーションが成功したことを示す画像。](../assets/ui/activate-on-demand/ad-hoc-success.png)

6. また、 **[!UICONTROL データフローの実行]** 「 」タブをクリックして、ファイルの書き出しがキックオフされたことを確認します。

## 注意点 {#considerations}

を使用する際は、次の点に注意してください。 **[!UICONTROL ファイルを今すぐ書き出し]** コントロール：

* **[!UICONTROL ファイルを今すぐ書き出し]** は、バッチアクティベーションデータフローのスケジュールが現在の日付と重複するオーディエンスに対してのみ機能します。 これには、終了日のないスケジュール ( エクスポート頻度： **[!UICONTROL 1 回]**) または終了日が過ぎていない場合にのみ有効です。
* オーディエンスを既存のデータフローに追加する場合、を使用するまで 15 分以上待ちます。 **[!UICONTROL ファイルを今すぐ書き出し]** コントロール。
* オーディエンスの結合ポリシーを変更する場合、または新しい結合ポリシーを使用するオーディエンスを作成する場合は、 **[!UICONTROL ファイルを今すぐ書き出し]** コントロール。

## UI エラーメッセージ {#ui-error-messages}

を使用する場合、 **[!UICONTROL ファイルを今すぐ書き出し]** コントロールを使用すると、次のエラーメッセージが表示される場合があります。 表を見て、表示される際の対処方法を理解してください。

| エラーメッセージ | 解決策 |
|---------|----------|
| オーディエンスに対して既に実行中です `segment ID` 注文 `dataflow ID` 実行 id を使用 `flow run ID` | このエラーメッセージは、オーディエンスに対するアドホックアクティベーションフローが現在進行中であることを示します。 ジョブが完了するのを待ってから、アクティベーションジョブを再度トリガーします。 |
| オーディエンス `<segment name>` は、このデータフローに含まれていないか、スケジュール範囲外です。 | このエラーメッセージは、アクティブ化するように選択したオーディエンスがデータフローにマッピングされていない、またはオーディエンス用に設定されたアクティベーションスケジュールが期限切れか、まだ開始されていないことを示します。 オーディエンスが実際にデータフローにマッピングされているかどうかを確認し、オーディエンスのアクティベーションスケジュールが現在の日付と重複していることを確認します。 |

## 関連情報 {#related-information}

* [Experience PlatformAPI を使用して、オーディエンスをオンデマンドでバッチ宛先にアクティブ化します。](/help/destinations/api/ad-hoc-activation-api.md)
* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](/help/destinations/ui/activate-batch-profile-destinations.md)
