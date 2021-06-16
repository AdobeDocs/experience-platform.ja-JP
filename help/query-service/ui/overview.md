---
keywords: Experience Platform；ホーム；人気のあるトピック；クエリサービス；クエリサービス；クエリ；クエリエディター；クエリエディター；クエリエディター；クエリエディター；
solution: Experience Platform
title: クエリサービスUIガイド
topic-legacy: guide
description: Adobe Experience Platformクエリサービスは、クエリの書き込みと実行、以前に実行されたクエリの表示、IMS組織内のユーザーが保存したクエリへのアクセスに使用できるユーザーインターフェイスを提供します。
exl-id: 99ad25e4-0ca4-4bd1-b701-ab463197930b
source-git-commit: 483bcea231ed5f25c76771d0acba7e0c62dfed16
workflow-type: tm+mt
source-wordcount: '628'
ht-degree: 44%

---

# [!DNL Query Service] ガイド

Adobe Experience Platform [!DNL Query Service]は、クエリの書き込みと実行、以前に実行されたクエリの表示、IMS組織内のユーザーが保存したクエリへのアクセスに使用できるユーザーインターフェイスを提供します。 [Adobe Experience Platform](https://platform.adobe.com) 内の UI にアクセスするには、左側のナビゲーションで「**[!UICONTROL クエリ]**」を選択します。

## [!DNL Query Editor]

[!DNL Query Editor]を使用すると、外部クライアントを使用せずにクエリを書き込み、実行できます。 「**[!UICONTROL クエリを作成]**」を選択して[!DNL Query Editor]を開き、新しいクエリを作成します。 [!DNL Query Editor]には、「**[!UICONTROL ログ]**」または「**[!UICONTROL 参照]**」タブからクエリを選択してアクセスすることもできます。 以前に実行または保存したクエリを選択すると、[!DNL Query Editor]が開き、選択したクエリのSQLが表示されます。

![画像](../images/ui/overview/overview.png)

[!DNL Query Editor] は、クエリの入力を開始できる編集スペースを提供します。入力時に、エディターはテーブル内のSQL予約語、テーブル、およびフィールド名を自動的に補完します。 クエリの作成が完了したら、「**再生**」ボタンを選択してクエリを実行します。 エディターの下の「**[!UICONTROL コンソール]**」タブに、[!DNL Query Service]が現在何を実行しているか、クエリがいつ返されたかが表示されます。 コンソールの横にある「**[!UICONTROL 結果]**」タブには、クエリ結果が表示されます。[!DNL Query Editor]の使用について詳しくは、[クエリエディターのガイド](./user-guide.md)を参照してください。

![画像](../images/ui/overview/query-editor.png)

## 参照

「**[!UICONTROL 参照]**」タブには、組織のユーザーによって保存されたクエリが表示されます。これらをクエリプロジェクトと考えると便利です。ここで保存したクエリは、まだ作成中の可能性があります。「**[!UICONTROL 参照]**」タブに表示されるクエリは、「**[!UICONTROL ログ]**」タブに実行クエリとしても表示されます（以前に[!DNL Query Service]によって実行されている場合）。

![画像](../images/ui/overview/browse.png)

| 列 | 説明 |
| --- | --- |
| 名前 | ユーザーが作成したクエリ名。名前を選択して、[!DNL Query Editor]でクエリを開くことができます。 検索バーを使用して、クエリの名前で検索することもできます。検索では大文字と小文字が区別されます。 |
| SQL | SQL クエリの最初の数文字。コードの上にカーソルを置くと、完全なクエリが表示されます。 |
| 変更者 | 最後にクエリを変更したユーザー。[!DNL Query Service]へのアクセス権を持つ組織内のすべてのユーザーがクエリを変更できます。 |
| 最終変更日 | ブラウザーのタイムゾーンでの、クエリが最後に編集された日付と時間。 |

## ログ

「**[!UICONTROL ログ]**」タブには、以前に実行されたクエリのリストが表示されます。デフォルトでは、ログはクエリを逆年代順にリストします。

![画像](../images/ui/overview/log.png)

| 列 | 説明 |
| --- | --- |
| **[!UICONTROL 名前]** | クエリ名。SQL クエリの最初の数文字で構成されます。名前を選択すると[!DNL Query Editor]が開き、クエリを編集できます。 検索バーを使用して、クエリの名前で検索できます。検索では大文字と小文字が区別されます。 |
| **[!UICONTROL 作成者]** | クエリを作成した人物の名前。 |
| **[!UICONTROL クライアント]** | クエリに使用されるクライアント。 |
| **[!UICONTROL データセット]** | クエリが使用する入力データセット。データセットを選択して、入力データセットの詳細画面に移動します。 |
| **[!UICONTROL ステータス]** | クエリの現在の状態。 |
| **[!UICONTROL 前回の実行]** | 最後にクエリが実行された日時。この列の上にある矢印を選択して、リストを昇順または降順に並べ替えることができます。 |
| **[!UICONTROL 実行時]** | クエリの実行に要した時間。 |

## 資格情報

「**[!UICONTROL 資格情報]**」タブには、 の資格情報が表示されます。[!DNL Postgres]任意のフィールドの横にある&#x200B;**[!UICONTROL コピー]**&#x200B;アイコンを選択して、その内容をキーボードバッファに保存します。 これらの資格情報を使用して外部クライアントと接続する方法の詳細については、[クライアントとの接続ガイド](../clients/overview.md)を参照してください。

![画像](../images/ui/overview/credentials.png)

## 次の手順

これで[!DNL Platform]の[!DNL Query Service]ユーザーインターフェイスに慣れたので、[!DNL Query Editor]にアクセスして独自のクエリプロジェクトを作成し、組織内の他のユーザーと共有できます。 [!DNL Query Editor]でのクエリの作成と実行について詳しくは、[クエリエディターのユーザーガイド](./user-guide.md)を参照してください。