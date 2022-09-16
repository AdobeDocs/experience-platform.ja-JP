---
title: （ベータ版）Experience PlatformUI を使用して、オンデマンドでバッチ保存先にファイルを書き出す
type: Tutorial
description: Experience PlatformUI を使用して、オンデマンドでファイルをバッチ保存先に書き出す方法を説明します。
exl-id: 0cbe5089-b73d-4584-8451-2fc34d47c357
source-git-commit: 874c590e83712a45e75308239fb71db04614bd1e
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 14%

---

# （ベータ版）Experience PlatformUI を使用して、オンデマンドでバッチ保存先にファイルを書き出す

>[!IMPORTANT]
>
>この **[!UICONTROL ファイルを今すぐ書き出し]** 」オプションを使用するAdobe Experience Platform Destination SDKは現在ベータ版です。 ドキュメントと機能は変更される場合があります。
>この機能へのアクセス権については、アドビ担当者にお問い合わせください。

>[!IMPORTANT]
> 
>データをアクティブ化するには、 **[!UICONTROL 宛先の管理]**, **[!UICONTROL 宛先のアクティブ化]**, **[!UICONTROL プロファイルの表示]**、および **[!UICONTROL セグメントを表示]** [アクセス制御権限](/help/access-control/home.md#permissions). 詳しくは、 [アクセス制御の概要](/help/access-control/ui/overview.md) または製品管理者に問い合わせて、必要な権限を取得してください。

## **[!UICONTROL ファイルを今すぐ書き出し]** 概要 {#overview}

>[!CONTEXTUALHELP]
>id="platform_destinations_activationchaining_activatenow"
>title="ファイルを今すぐ書き出し"
>abstract="以前にスケジュールされたエクスポートに加えて、フルファイルエクスポートも配信する場合は、このコントロールを選択します。  ファイルの書き出しが直ちにトリガーされ、Experience Platform のセグメント化の実行から最新の結果が取得されます。"

この記事では、Experience PlatformUI を使用して、オンデマンドでファイルをバッチ保存先 ( 例： [クラウドストレージ](/help/destinations/catalog/cloud-storage/overview.md) および [電子メールマーケティング](/help/destinations/catalog/email-marketing/overview.md) 宛先。

この **[!UICONTROL ファイルを今すぐ書き出し]** 「 」コントロールを使用すると、以前にスケジュールされたセグメントの現在の書き出しスケジュールを中断することなく、完全なファイルを書き出すことができます。 この書き出しは、以前にスケジュールされた書き出しに加えて行われ、セグメントの書き出し頻度は変更されません。  ファイルの書き出しが直ちにトリガーされ、Experience Platform のセグメント化の実行から最新の結果が取得されます。

この目的でExperience PlatformAPI を使用することもできます。 方法を読む [アドホックアクティベーション API を使用して、バッチ保存先に対してオンデマンドでオーディエンスセグメントをアクティブ化します](/help/destinations/api/ad-hoc-activation-api.md).

## 前提条件 {#prerequisites}

オンデマンドでファイルをバッチ保存先に書き出すには、 [宛先に接続されている](./connect-destination.md). まだ接続していない場合は、[宛先カタログ](../catalog/overview.md)に移動し、サポートされている宛先を参照し、使用する宛先を設定します。

## オンデマンドでファイルを書き出す方法 {#how-to-export-files-on-demand}

1. に移動します。 **[!UICONTROL 接続/宛先]**&#x200B;を選択し、 **[!UICONTROL 参照]** タブおよびフィルター記号を使用して、目的のバッチ保存先への既存の接続を表示します。

   ![「参照」タブに移動し、既存のデータフローをフィルタリングする方法を強調した画像。](../assets/ui/activate-on-demand/browse-tab.png)

2. 目的の宛先接続を選択して、宛先への既存のデータフローを検査します。

   ![フィルタリングしたデータフローをハイライトした画像。](../assets/ui/activate-on-demand/filtered-dataflow.png)

3. を選択します。 **[!UICONTROL アクティベーションデータ]** 」タブをクリックし、ファイルをオンデマンドで書き出すセグメントを選択して、「 **[!UICONTROL ファイルを今すぐ書き出し]** ファイルをバッチ保存先に配信する 1 回限りのエクスポートをトリガーするためのコントロール。

   >[!IMPORTANT]
   >
   >ファイルをオンデマンドで一括で書き出す複数のセグメントの選択は、現在、UI ではサポートされていません。 以下を使用： [アドホックアクティベーション API](/help/destinations/api/ad-hoc-activation-api.md) そのために

   ![「ファイルを今すぐ書き出し」ボタンをハイライトした画像。](../assets/ui/activate-on-demand/activate-segment-on-demand.png)

4. 選択 **[!UICONTROL はい]** をクリックして、ファイルのエクスポートを確認してトリガーを設定します。

   ![「ファイルを今すぐ書き出し」確認ダイアログを示す画像。](../assets/ui/activate-on-demand/confirm-activation.png)

5. ファイルのエクスポートが開始されたことを知らせる確認メッセージが表示されます。

   ![アドホックアクティベーションが成功したことを示す画像。](../assets/ui/activate-on-demand/ad-hoc-success.png)

6. また、 **[!UICONTROL データフローの実行]** 「 」タブをクリックして、ファイルの書き出しがキックオフされたことを確認します。

## 注意点 {#considerations}

を使用する際は、次の点に注意してください。 **[!UICONTROL ファイルを今すぐ書き出し]** コントロール：

* **[!UICONTROL ファイルを今すぐ書き出し]** は、バッチアクティベーションデータフローのスケジュールが現在の日付と重複するセグメントに対してのみ機能します。 これには、終了日のないスケジュール ( エクスポート頻度： **[!UICONTROL 1 回]**) または終了日が過ぎていない場合にのみ有効です。
* 既存のデータフローにセグメントを追加する場合は、を使用するまで 15 分以上待ちます。 **[!UICONTROL ファイルを今すぐ書き出し]** コントロール。
* セグメントの結合ポリシーを変更した場合、または新しい結合ポリシーを使用するセグメントを作成した場合は、 **[!UICONTROL ファイルを今すぐ書き出し]** コントロール。

## UI エラーメッセージ {#ui-error-messages}

を使用する場合、 **[!UICONTROL ファイルを今すぐ書き出し]** コントロールを使用すると、以下のエラーメッセージが表示される場合があります。 表を見て、表示される際の対処方法を理解してください。

| エラーメッセージ | 解像度 |
|---------|----------|
| セグメントに対して既に実行中です `segment ID` 注文 `dataflow ID` 実行 id を使用 `flow run ID` | このエラーメッセージは、セグメントに対するアドホックアクティベーションフローが現在進行中であることを示します。 ジョブが完了するのを待ってから、アクティベーションジョブを再度トリガーします。 |
| セグメント `<segment name>` は、このデータフローに含まれていないか、スケジュール範囲外です。 | このエラーメッセージは、アクティブ化するように選択したセグメントがデータフローにマッピングされていない、またはセグメント用に設定されたアクティベーションスケジュールが期限切れになっているか、まだ開始されていないことを示します。 セグメントが実際にデータフローにマッピングされているかどうかを確認し、セグメントのアクティベーションスケジュールが現在の日付と重複していることを確認します。 |

## 関連情報 {#related-information}

* [Experience PlatformAPI を使用して、オンデマンドでオーディエンスセグメントをバッチ宛先に対してアクティブ化します](/help/destinations/api/ad-hoc-activation-api.md)
* [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](/help/destinations/ui/activate-batch-profile-destinations.md)
