---
keywords: SFTP;sftp
title: SFTP 接続
description: SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的にAdobe Experience Platformから書き出します。
exl-id: 27abfc38-ec19-4321-b743-169370d585a0
source-git-commit: 691e3181e05a24b6bb0ebbe8e0f797a2b4c572d2
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 2%

---

# SFTP 接続

## 概要 {#overview}

SFTP サーバーへのライブアウトバウンド接続を作成し、区切られたデータファイルを定期的にAdobe Experience Platformから書き出します。

>[!IMPORTANT]
>
> Adobeは SFTP サーバーへのデータの書き出しをサポートしていますが、データを書き出す際に推奨されるクラウドストレージの場所は次のとおりです [!DNL Amazon S3] および [!DNL Azure Blob].

## エクスポートのタイプと頻度 {#export-type-frequency}

宛先の書き出しのタイプと頻度について詳しくは、次の表を参照してください。

| 項目 | タイプ | 備考 |
---------|----------|---------|
| 書き出しタイプ | **[!UICONTROL プロファイルベース]** | セグメントのすべてのメンバーを、目的のスキーマフィールド ( 例：（電子メールアドレス、電話番号、姓）。「プロファイル属性を選択」画面で選択します。 [宛先のアクティベーションワークフロー](../../ui/activate-batch-profile-destinations.md#select-attributes). |
| 書き出し頻度 | **[!UICONTROL バッチ]** | バッチ宛先では、ファイルが 3 時間、6 時間、8 時間、12 時間、24 時間の単位でダウンストリームプラットフォームに書き出されます。 詳細を表示 [バッチファイルベースの宛先](/help/destinations/destination-types.md#file-based). |

{style=&quot;table-layout:auto&quot;}

![SFTP プロファイルベースの書き出しタイプ](../../assets/catalog/cloud-storage/sftp/catalog.png)

## 宛先に接続 {#connect}

この宛先に接続するには、 [宛先設定のチュートリアル](../../ui/connect-destination.md).

### 接続パラメーター {#parameters}

>[!CONTEXTUALHELP]
>id="platform_destinations_connect_sftp_rsa"
>title="RSA 公開鍵"
>abstract="必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、Base64 エンコードされた文字列として書き込む必要があります。"

While [設定](../../ui/connect-destination.md) この宛先には、次の情報を指定する必要があります。

* **ホスト**:SFTP ストレージの場所のアドレス
* **ユーザー名**:SFTP ストレージの場所にログインするユーザー名
* **パスワード**:SFTP ストレージの場所にログインするためのパスワード
* **[!UICONTROL 名前]**:この宛先を識別するのに役立つ名前を入力します。
* **[!UICONTROL 説明]**:この宛先の説明を入力します。
* **[!UICONTROL フォルダーパス]**:書き出したファイルをホストする保存先フォルダーのパスを入力します。

必要に応じて、RSA 形式の公開鍵を添付して、書き出したファイルに暗号化を追加できます。 公開鍵は、 [!DNL Base64] エンコードされた文字列。

## 書き出されたデータ {#exported-data}

の場合 [!DNL SFTP] 宛先、Platform は、 `.csv` ファイルを指定したストレージの場所に保存します。 ファイルの詳細については、 [プロファイルの一括書き出し先に対するオーディエンスデータのアクティブ化](../../ui/activate-batch-profile-destinations.md) （セグメントのアクティベーションに関するチュートリアル）。

## IP アドレスの許可リスト

参照： [クラウドストレージの宛先の IP アドレス許可リスト](ip-address-allow-list.md) 許可リストにAdobeIP を追加する必要がある場合。
