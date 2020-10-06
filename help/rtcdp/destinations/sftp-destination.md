---
keywords: SFTP;sftp
title: SFTP の宛先
seo-title: SFTP の宛先
description: SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的に Experience Platform から書き出します。
seo-description: SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的に Experience Platform から書き出します。
translation-type: tm+mt
source-git-commit: 67a353c950bef11ccbaa52c49d213f08449baa96
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 60%

---


# SFTP の宛先

## 概要

SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的に Experience Platform から書き出します。

## 書き出しタイプ {#export-type}

**プロファイルベース** — セグメントのすべてのメンバーを、必要なスキーマフィールド(例：電子メールアドレス、電話番号、姓)。 [宛先アクティベーションワークフローの属性を選択画面で選択](/help/rtcdp/destinations/activate-destinations.md#select-attributes)。

![SFTPプロファイルベースのエクスポートタイプ](/help/rtcdp/destinations/assets/sftp-export-type.png)

## 宛先の接続 {#connect-destination}

SFTP を含むクラウドストレージの宛先への接続方法については、「[クラウドストレージの宛先のワークフロー](/help/rtcdp/destinations/cloud-storage-destinations-workflow.md)」を参照してください。

SFTP の宛先については、宛先の作成ワークフローの&#x200B;**認証**&#x200B;手順で次の情報を入力します 。

* **Host**：SFTP ストレージ場所のアドレス
* **Username**：SFTP ストレージの場所にログインするユーザー名
* **Password**：SFTP ストレージの場所にログインするパスワード

## 書き出されたデータ {#exported-data}

For SFTP destinations, Adobe Real-time CDP creates a tab-delimited `.txt` or `.csv` file in the storage location that you provided. これらのファイルについて詳しくは、セグメントアクティベーションチュートリアルの [電子メールマーケティングの宛先とクラウドストレージの宛先](/help/rtcdp/destinations/activate-destinations.md#esp-and-cloud-storage) （英語）を参照してください。