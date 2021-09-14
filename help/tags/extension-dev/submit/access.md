---
title: ユーザーアクセスの許可
description: チームメンバーのタグユーザーアカウントと権限を Adobe Experience Platform で設定します。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: ht
source-wordcount: '738'
ht-degree: 100%

---

# ユーザーアクセスの許可

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

拡張機能パッケージを使用する前に、チームメンバーのユーザーアカウントおよび権限を設定する必要があります。それには、[Adobe Admin Console](https://adminconsole.adobe.com/) を使用します。

このドキュメントでは、Admin Console を介して Adobe Experience Platform のタグへのアクセスを許可する手順を示します。

## 前提条件

このガイドは、ユーザーが Admin Console で指定された組織管理者であることを前提としています。Admin Console とロールの割り当てに関する追加情報が必要な場合は、次のリソースを参照してください。

* [管理ユーザーガイド](https://helpx.adobe.com/jp/enterprise/administering/user-guide.html?topic=/enterprise/administering/morehelp/introduction.ug.js)：Admin Console 内のすべての機能に関する情報
* [エンタープライズ版の管理ロール](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html)：様々な種類の管理ロールに関する詳しい情報以下のガイドでは、ユーザーが組織管理者であることを前提としています。

## 組織の選択

Adobe Experience Cloud の組織管理者は、[Admin Console](https://adminconsole.adobe.com/) にサインインする必要があります。最初に表示される画面は概要です。

![Admin Console の「概要」タブ](../images/getting-started/admin-console-overview.png)

一部のユーザーは、複数の組織にアクセスすることができます。正しい組織にタグ機能を追加するには、画面の右上隅に表示される組織の名前を選択します。 次に、ドロップダウンリストからタグを使用する組織を選択します。

![Admin Console 組織選択ドロップダウン](../images/getting-started/admin-console-choose-org.png)

## 製品プロファイルの作成

製品プロファイルはグループです。個々の権限が製品プロファイルに割り当てられ、プロファイル内のすべてのユーザーがそれらの権限を継承します。

上部の「**[!UICONTROL 製品]**」リンクを選択し、左側の「**[!UICONTROL Experience Cloud]**」を選択します。データ収集 UI がリストに表示されない場合、お客様はアカウントチームに、パートナーはメール（ <ExchangeTechEC@adobe.com>）にお問い合わせください。

![Admin Console の「製品」タブ](../images/getting-started/admin-console-products-launch.png)

上のスクリーンショットはサンプルのプロファイルを示していますが、まだプロファイルがない可能性があります。 作成するには、「**[!UICONTROL 新規プロファイル]**」を選択します。 **新しいプロファイルを作成**&#x200B;画面で、「**プロファイル名**」（「データ収集テスト」など）と「**説明**」（オプション）を追加し、「**[!UICONTROL 保存]**」を選択します。

![「新規プロファイルを作成」の表示](../images/getting-started/admin-console-create-a-new-profile.png)

これで、製品プロファイルが組織に追加されました。 次に、製品プロファイルにユーザーを追加します。

## 製品プロファイルへのユーザーの割り当て

製品プロファイルでは、「**資格のあるユーザー**」と「**管理者**」に 0 が表示されていることに注意してください。作成した製品プロファイルの名前を選択します（この例では「データ収集テスト」）。

![製品プロファイルの表示](../images/getting-started/admin-console-profiles-add-user.png)

「**[!UICONTROL ユーザー]**」タブを選択します。ここでは、既存の Adobe ID ユーザーを電子メールで検索したり、新しいユーザーをこの製品プロファイルに追加したりできます。「**[!UICONTROL ユーザーリンクを追加]**」を選択します。

![「製品プロファイルユーザー」タブ](../images/getting-started/admin-console-add-launch-user.png)

該当するテキストフィールドに、名前、ユーザーグループまたはメールアドレスを入力します。 可能な場合は、姓と名を含めることをお勧めします。 「**[!UICONTROL 保存]**」を選択して、ユーザーを追加します。

![「ユーザーをプロファイル追加」表示](../images/getting-started/admin-console-add-user.png)

この製品プロファイルで必要なすべてのユーザーが揃ったら、ユーザーに権限を追加します。「**[!UICONTROL 権限]**」タブを選択します。権限設定画面に、**[!UICONTROL プロパティ]**、**[!UICONTROL 会社権限]**、および&#x200B;**[!UICONTROL プロパティ権限]**&#x200B;が表示されます。「**[!UICONTROL 編集]**」を選択します。

![「製品プロファイルの権限」タブ](../images/getting-started/admin-console-profile-permissions.png)

チームが拡張機能を作成するには、少なくとも次の権限が必要です。

* 会社グループの「プロパティの管理」。
* プロパティグループの「拡張機能の管理」、「環境の管理」および「開発」。

必要に応じて、権限が制限された製品プロファイルを後で作成できますが、ここでは、「**会社権限**」と「**プロパティ権限**」の両方で「**[!UICONTROL + すべてを追加]**」を選択します。必ず「**[!UICONTROL 保存]**」を選択してください。

![プロパティ権限の管理](../images/getting-started/admin-console-add-all-property-rights.png)

![会社権限の管理](../images/getting-started/admin-console-add-all-company-rights.png)

これまで、適切な組織を選択して製品プロファイルを作成し、製品プロファイルにユーザーを追加して権限を割り当てました。

これで、Admin Console の必要な設定が完了します。ユーザーとして設定されたユーザーとチームメンバーは、[データ収集 UI](https://launch.adobe.com/) にログインできるようになりました。

## プロビジョニングの確認

会社にタグへのアクセスがプロビジョニングされ、ユーザーが前述のように設定されたら、[データ収集 UI](https://launch.adobe.com/) の実稼動環境にアクセスできるようになります。タグのプロビジョニングが完了し、上記の Admin Console 手順を完了してもデータ収集 UI にログインできない場合は、アドビのサポート担当者にお問い合わせください。
