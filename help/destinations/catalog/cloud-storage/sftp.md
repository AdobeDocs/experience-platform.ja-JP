---
keywords: SFTP;sftp
title: SFTP の宛先
seo-title: SFTP の宛先
description: SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的に Experience Platform から書き出します。
seo-description: SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的に Experience Platform から書き出します。
translation-type: tm+mt
source-git-commit: 7aadb4b7e7c36b659490d155ad4cfa7ef0a24306
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 47%

---


# SFTP の宛先

## 概要

SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的に Experience Platform から書き出します。

## エクスポートの種類{#export-type}

**プロファイルベース**  — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](../../ui/activate-destinations.md#select-attributes)。

![SFTPプロファイルベースのエクスポートタイプ](../../assets/catalog/cloud-storage/sftp/catalog.png)

## 宛先の接続 {#connect-destination}

SFTP を含むクラウドストレージの宛先への接続方法については、「[クラウドストレージの宛先のワークフロー](./workflow.md)」を参照してください。

SFTP の宛先については、宛先の作成ワークフローの&#x200B;**認証**&#x200B;手順で次の情報を入力します 。

* **ホスト**:SFTPストレージの場所のアドレス
* **ユーザー名**:SFTPストレージの場所にログインするためのユーザー名
* **パスワード**:SFTPストレージの場所にログインするためのパスワード

## エクスポートされたデータ{#exported-data}

SFTPの送信先の場合、プラットフォームは、指定したストレージーの場所にタブ区切りの`.txt`ファイルまたは`.csv`ファイルを作成します。 ファイルについて詳しくは、セグメントアクティベーションチュートリアルの「電子メールマーケティングの宛先とクラウドストレージの宛先」[を参照してください。](../../ui/activate-destinations.md#esp-and-cloud-storage)