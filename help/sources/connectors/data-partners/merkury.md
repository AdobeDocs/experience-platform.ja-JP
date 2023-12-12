---
title: マークエンタープライズ ID 解決ソースの概要
description: ユーザーインターフェイスを使用して Merkury Enterprise ID 解決をAdobe Experience Platformに接続する方法を説明します。
badge: ベータ版
source-git-commit: 12f73ac2578b6c5b024cc4ebdd75cd945c7b55c9
workflow-type: tm+mt
source-wordcount: '498'
ht-degree: 44%

---

# [!DNL Merkury Enterprise Identity Resolution]

>[!NOTE]
>
>[!DNL Merkury Enterprise Identity Resolution] ソースはベータ版です。詳しくは、 [ソースの概要](../../home.md#terms-and-conditions) ベータラベル付きのソースの使用に関する詳細

Adobe Experience Platformは、データパートナーアプリケーションからデータを取り込む機能を備えています。 データパートナーのサポートには以下が含まれます。 [!DNL Merkury Enterprise Identity Resolution].

以下を使用できます。 [!DNL Merkury] 作成者 [!DNL Merkle] を使用すれば、cookie を使用しなくても、より多くのデジタル訪問者を認識し、顧客が必要とする、関連性の高いパーソナライズされたエクスペリエンスを配信できます。

以下を利用できます。 **ユーザー ID** の一部として [!DNL Merkury] 組織が個人に関して把握しているすべての情報を 1 つの包括的なプロファイルに組み合わせるソース。 以下の詳細が含まれます。

- デジタル動作
- 購入環境設定
- 名前、電子メールアドレス、物理アドレス、デバイス ID などの情報の識別。

取り込んだデータは、Experience Data Model(XDM)JSON、XDM Parquet、または区切り形式で書式設定できます。 プロセスの各ステップは、ソース作業に統合されます

![マークソースのデータ処理ワークフローの図です。](../../images/tutorials/create/merkury-enterprise-identity-resolution-assets/architecture.png)

## IP アドレス許可リスト

ソースコネクタを操作する前に、IP アドレスのリストを許可リストに追加する必要があります。 地域固有の IP アドレスを許可リストに追加しないと、ソースを使用する際にエラーが発生したり、パフォーマンスが低下する場合があります。 詳しくは、[IP アドレスの許可リスト](../../ip-address-allow-list.md)ページを参照してください。

## ファイルとディレクトリの命名制約

クラウドストレージファイルまたはディレクトリに名前を付ける際に考慮する必要がある制約のリストを次に示します。

- ディレクトリ名とファイルコンポーネント名は 255 文字を超えてはなりません。
- ディレクトリ名とファイル名の末尾にスラッシュ（`/`）は使用できません。使用した場合、自動的に削除されます。
- 次の予約 URL 文字は、適切にエスケープする必要があります。`! ' ( ) ; @ & = + $ , % # [ ]`
- 次の文字は使用できません。`" \ / : | < > * ?`
- 無効な URL パス文字は使用できません。`\uE000` のようなコードポイントは、NTFS ファイル名では有効ですが、有効な Unicode 文字ではありません。また、一部の ASCII 文字や Unicode 文字、例えば制御文字（0x00 ～ 0x1F、\u0081 など）も使用できません。HTTP/1.1 で Unicode 文字列を規定するルールについては、[RFC 2616、セクション 2.2：基本ルール](https://www.ietf.org/rfc/rfc2616.txt)および [RFC 3987](https://www.ietf.org/rfc/rfc3987.txt) を参照してください。
- 次のファイル名は使用できません：LPT1、LPT2、LPT3、LPT4、LPT5、LPT6、LPT7、LPT8、LPT9、COM1、COM2、COM3、COM4、COM5、COM6、COM7、COM8、COM9、PRN、AUX、NUL、CON、CLOCK$、ドット文字（.）、2 つのドット文字（..）。

## 前提条件

使用を開始する前に、次の前提条件を満たす必要があります。 [!DNL Merkury] ソース：

- 次を完了する必要があります： [!DNL Merkury] の設定と [!DNL Merkury] チーム。
- 資格情報（アクセスキー、秘密鍵、バケット名）を [!DNL Merkury] チーム。 

>[!NOTE]
>
>次のようなファイルパス： `myBucket/folder/subfolder/subsubfolder/abc.csv` 次にのみアクセスするように設定します： `subsubfolder/abc.csv`. サブフォルダーにアクセスする場合は、 bucket パラメーターを myBucket に、 folderPath を folder/subfolder に指定して、ではなく、サブフォルダーでファイルの調査が開始されるようにします。 `subsubfolder/abc.csv`.

## 次の手順

このドキュメントを読むと、データをから取り込むために必要な前提条件の設定が完了します。 [!DNL Merkury] アカウントからExperience Platformへ。 次のガイドに進むことができます： [接続 [!DNL Merkury] ユーザーインターフェイスを使用してExperience Platformを設定するには](../../tutorials/ui/create/data-partners/merkury.md).
