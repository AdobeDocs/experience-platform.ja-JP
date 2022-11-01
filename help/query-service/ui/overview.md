---
keywords: Experience Platform；ホーム；人気の高いトピック；クエリサービス；クエリサービス；クエリ；クエリエディター；クエリエディター；クエリエディター；
solution: Experience Platform
title: クエリサービス UI ガイド
topic-legacy: guide
description: Adobe Experience Platformクエリサービスは、クエリの書き込みと実行、以前に実行されたクエリの表示、IMS 組織内のユーザーが保存したクエリへのアクセスに使用できるユーザーインターフェイスを提供します。
exl-id: 99ad25e4-0ca4-4bd1-b701-ab463197930b
source-git-commit: a085bac6b4ee825d534710ae91d6690fa076e873
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 28%

---

# [!DNL Query Service] UI ガイド

ザAdobe Experience Platform [!DNL Query Service] は、クエリの書き込みと実行、以前に実行したクエリの表示、IMS 組織内のユーザーが保存したクエリへのアクセスに使用できるユーザーインターフェイスを提供します。 [Adobe Experience Platform](https://platform.adobe.com) 内の UI にアクセスするには、左側のナビゲーションで「**[!UICONTROL クエリ]**」を選択します。

## [!DNL Query Editor]

この [!DNL Query Editor] を使用すると、外部クライアントを使用せずにクエリを書き込んだり、実行したりできます。 選択 **[!UICONTROL クエリを作成]** 開く [!DNL Query Editor] をクリックし、新しいクエリを作成します。 また、 [!DNL Query Editor] クエリを **[!UICONTROL ログ]** または **[!UICONTROL テンプレート]** タブ 以前に実行または保存したクエリを選択すると、 [!DNL Query Editor] 選択したクエリの SQL を表示します。

![「クエリの作成」がハイライト表示された「クエリ」ダッシュボード。](../images/ui/overview/overview.png)

[!DNL Query Editor] は、クエリの入力を開始できる編集スペースを提供します。 入力すると、エディターは、テーブル内の SQL 予約語、テーブル、およびフィールド名を自動的に完了します。 クエリの作成が完了したら、 **再生** ボタンを使用してクエリを実行します。 この **[!UICONTROL コンソール]** エディターの下のタブに、次の内容が表示されます。 [!DNL Query Service] は現在実行中で、クエリがいつ返されたかを示します。 コンソールの横にある「**[!UICONTROL 結果]**」タブには、クエリ結果が表示されます。詳しくは、 [クエリエディターガイド](./user-guide.md) を参照してください。 [!DNL Query Editor].

![ズームインした [!DNL Query Editor].](../images/ui/overview/query-editor.png)

## テンプレート {#browse}

この **[!UICONTROL テンプレート]** 「 」タブには、組織内のユーザーが保存したクエリが表示されます。 これらをクエリプロジェクトと考えると便利です。ここで保存したクエリは、まだ作成中の可能性があります。に表示されるクエリ **[!UICONTROL テンプレート]** 「 」タブは「実行クエリ」としても表示されます **[!UICONTROL ログ]** タブ ( 以前に [!DNL Query Service].

![保存済みのクエリが複数表示された「クエリダッシュボードのテンプレート」タブのズームインビュー。](../images/ui/overview/templates.png)

| 列 | 説明 |
| --- | --- |
| **[!UICONTROL 名前]** | 名前フィールドは、ユーザーが作成したクエリ名か、SQL クエリの最初の数文字のどちらかです。 クエリエディターで UI を使用して作成したクエリは、開始時にと名前が付けられます。 API を使用してクエリが作成された場合、クエリの名前は、クエリの作成に使用される最初の SQL のスニペットになります。 クエリ名を選択して、 [!DNL Query Editor]. 検索バーを使用して [!UICONTROL 名前] クエリの。 検索では大文字と小文字が区別されます。 |
| **[!UICONTROL SQL]** | SQL クエリの最初の数文字。コードの上にカーソルを置くと、完全なクエリが表示されます。 |
| **[!UICONTROL 変更者]** | 最後にクエリを変更したユーザー。組織内でへのアクセス権を持つユーザー [!DNL Query Service] でクエリを変更できます。 |
| **[!UICONTROL 最終変更日]** | ブラウザーのタイムゾーンでの、クエリが最後に編集された日付と時間。 |

## ログ

「**[!UICONTROL ログ]**」タブには、以前に実行されたクエリのリストが表示されます。デフォルトでは、ログはクエリを逆年代順にリストします。

![クエリダッシュボードの「ログ」タブに、クエリのリストが時系列順に表示されたズームインビュー。](../images/ui/overview/log.png)

| 列 | 説明 |
| --- | --- |
| **[!UICONTROL 名前]** | クエリ名。SQL クエリの最初の数文字で構成されます。名前を選択すると、 [!DNL Query Editor]をクリックし、クエリを編集できます。 検索バーを使用して、クエリの名前で検索できます。検索では大文字と小文字が区別されます。 |
| **[!UICONTROL 作成者]** | クエリを作成した人物の名前。 |
| **[!UICONTROL クライアント]** | クエリに使用されるクライアント。 |
| **[!UICONTROL データセット]** | クエリが使用する入力データセット。データセットを選択して、入力データセットの詳細画面に移動します。 |
| **[!UICONTROL ステータス]** | クエリの現在の状態。 |
| **[!UICONTROL 前回の実行]** | 最後にクエリが実行された日時。この列の上にある矢印を選択すると、リストを昇順または降順で並べ替えることができます。 |
| **[!UICONTROL 実行時間]** | クエリの実行に要した時間。 |

## 資格情報

この **[!UICONTROL 資格情報]** 「 」タブには、有効期限切れの資格情報と有効期限切れでない資格情報の両方が表示されます。 これらの資格情報を使用して外部クライアントと接続する方法の詳細については、 [資格情報ガイド](../clients/overview.md).

![「認証情報」タブがハイライトされた「クエリ」ダッシュボード。](../images/ui/overview/credentials.png)

## 次の手順

これで [!DNL Query Service] のユーザーインターフェイス [!DNL Platform]、 [!DNL Query Editor] ：独自のクエリプロジェクトの作成を開始し、組織内の他のユーザーと共有します。 でのクエリの作成と実行に関する詳細 [!DNL Query Editor]を参照し、 [[!DNL Query Editor] ユーザーガイド](./user-guide.md).