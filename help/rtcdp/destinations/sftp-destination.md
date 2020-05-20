---
title: SFTP の宛先
seo-title: SFTP の宛先
description: SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的に Experience Platform から書き出します。
seo-description: SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的に Experience Platform から書き出します。
translation-type: tm+mt
source-git-commit: c3fe5753fb23f99076f9c85b4e07af2d25a121a9
workflow-type: tm+mt
source-wordcount: '132'
ht-degree: 66%

---


# SFTP の宛先

## 概要

SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的に Experience Platform から書き出します。

データを書き出すには、次の手順を実行します。

## 宛先の接続 {#connect-destination}

SFTP を含むクラウドストレージの宛先への接続方法については、「[クラウドストレージの宛先のワークフロー](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)」を参照してください。

SFTP送信先の場合は、送信先の作成ワークフローの **認証** 手順で次の情報を入力します。

* **ホスト**: SFTPストレージの場所のアドレス
* **ユーザー名**: SFTPストレージの場所にログインするためのユーザー名。
* **パスワード**: SFTPストレージの場所にログインするためのパスワード