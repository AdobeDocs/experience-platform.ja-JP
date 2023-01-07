---
keywords: Experience Platform；ホーム；人気の高いトピック；tableau;Tableau；クエリサービス；クエリサービス；クエリサービスへの接続；
solution: Experience Platform
title: Tableau のクエリーサービスへの接続
description: このドキュメントでは、Tableau とAdobe Experience Platform Query Service を接続する手順について説明します。
exl-id: f380aacd-5091-41bc-97ca-593e0b1670fd
source-git-commit: 58eadaaf461ecd9598f3f508fab0c192cf058916
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 2%

---

# 接続 [!DNL Tableau] クエリサービスへ

このドキュメントでは、 [!DNL Tableau] Adobe Experience Platform [!DNL Query Service].

>[!NOTE]
>
> このガイドは、ユーザーが既に [!DNL Tableau] インターフェイスの操作方法を熟知している。 詳細情報： [!DNL Tableau] は [公式 [!DNL Tableau] ドキュメント](https://help.tableau.com/current/pro/desktop/en-us/default.htm).

接続するには [!DNL Tableau] から [!DNL Query Service]，開く [!DNL Tableau]、および **[!DNL To a Server]** セクション選択 **[!DNL More]** 続いて **[!DNL PostgreSQL]**

![この [!DNL Tableau] ダッシュボードに「その他」と「 [!DNL PostgreSQL] ハイライト表示されました。](../images/clients/tableau/open-connection.png)

これで、Adobe Experience Platformに接続する値を入力できるようになりました。 データベース名、ホスト、ポート、ログイン資格情報の検索の詳細については、 [資格情報ガイド](../ui/credentials.md). 資格情報を検索するには、にログインします。 [!DNL Platform]を選択し、「 **[!UICONTROL クエリ]**&#x200B;に続いて **[!UICONTROL 資格情報]**.

必ず **[!UICONTROL SSL が必要]** 」ボックスをクリックして接続を試みます。

>[!IMPORTANT]
>
>詳しくは、 [[!DNL Query Service] SSL ドキュメント](./ssl-modes.md) を参照して、Adobe Experience Platform Query Service へのサードパーティ接続の SSL サポートと、 `verify-full` SSL モード。

![この [!DNL PostgreSQL] 接続ダイアログが開き、接続の詳細が完了しました。](../images/clients/tableau/sign-in.png)

>[!IMPORTANT]
>
>サードパーティの BI ツールのネストされたデータ構造は、フラット化して、使いやすさを向上させ、データの取得、分析、変換、レポートに必要なワークロードを削減できます。 詳しくは、[`FLATTEN` 機能](../best-practices/flatten-nested-data.md) を参照してください。

すべての資格情報を入力したら、「 」を選択します。 **[!DNL Sign In]** をクリックして続行します。

これで、Adobe Experience Platformと接続し、テーブルのリストが横に表示されます。

![新しい [!DNL Tableau] ダッシュボードとクエリサービステーブルが左側のパネルでハイライト表示されています。](../images/clients/tableau/connected.png)

## 次の手順

これで、 [!DNL Query Service]を使用する場合、 [!DNL Tableau] クエリを書き込みます。 クエリの書き込みと実行の方法について詳しくは、 [クエリの実行](../best-practices/writing-queries.md).
