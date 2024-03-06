---
title: Acxiom 見込みデータのインポート
description: UI を使用して Acxiom Prospecting Data をAdobe Experience PlatformとAdobe Real-time Customer Data Platformに接続する方法を説明します。
badge: ベータ版
source-git-commit: 64975ccb6a44730489427cef745f3dbce5bcedf1
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 37%

---

# [!DNL Acxiom Prospecting Data Import]

>[!NOTE]
>
>[!DNL Acxiom Prospecting Data Import] ソースはベータ版です。詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

Adobe Experience Platformは、データパートナーアプリケーションからデータを取り込む機能を備えています。 データおよび ID パートナーのサポートには以下が含まれます。 [!DNL Acxiom Prospecting Data Import].

[!DNL Acxiom]のAdobe Real-time Customer Data Platform向けの予測データインポートは、可能な限り生産的な見込み客オーディエンスを提供するプロセスです。 [!DNL Acxiom] は、安全なエクスポートを通じてReal-Time CDPのファーストパーティデータを取得し、受賞歴のある衛生および ID 解決システムを通じてそのデータを実行します。 抑制リストとして利用可能なデータファイルを作成する。 このデータファイルは、 [!DNL Acxiom Global] データベース。見込み客リストをインポート用にカスタマイズできます。

以下を使用すると、 [!DNL Acxiom] からの応答を取得およびマッピングするソース [!DNL Acxiom] を使用した見込み客サービス [!DNL Amazon S3] をドロップポイントとして使用します。

## 前提条件

Experience Platformでバケットにアクセスするには、次の資格情報に有効な値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| [!DNL Acxiom] 認証キー | 認証キー。 この値は、 [!DNL Acxiom] チーム。 |
| [!DNL Amazon S3] アクセスキー | バケットのアクセスキー ID。 この値は、 [!DNL Acxiom] チーム。 |
| [!DNL Amazon S3] 秘密鍵 | バケットの秘密鍵 ID。 この値は、 [!DNL Acxiom] チーム。 |
| バケット名 | これはファイルが共有されるグループです。 この値は、 [!DNL Acxiom] チーム。 |

### 権限

次の両方を持つ必要があります。 **[!UICONTROL ソースを表示]** および **[!UICONTROL ソースの管理]** アカウントに対して有効になっている権限で、 [!DNL Acxiom Prospecting Data Import] アカウントからExperience Platformへ。 製品管理者に問い合わせて、必要な権限を取得してください。 詳しくは、 [アクセス制御 UI ガイド](../../../access-control/abac/ui/permissions.md).

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際は、以下の制限事項を考慮に入れる必要があります。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。 `\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## 次の手順

このドキュメントを読むと、データを [!DNL Acxiom] アカウントからExperience Platformへ。 次のガイドに進むことができます： [接続 [!DNL Acxiom Prospecting Data Import] ユーザーインターフェイスを使用してExperience Platformを設定するには](../../tutorials/ui/create/data-partners/acxiom-prospecting-data-import.md).