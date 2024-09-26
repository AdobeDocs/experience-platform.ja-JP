---
title: Merkury エンタープライズ Id の宛先
description: Adobe Experience Platform UI を使用して Merkury エンタープライズ ID 宛先接続を作成する方法を説明します。
last-substantial-update: 2024-07-20T00:00:00Z
exl-id: a5452183-289c-49c3-9574-e09b0153dc00
source-git-commit: 2b84b5106105339ab243a9f4412b47692caedf3c
workflow-type: tm+mt
source-wordcount: '1469'
ht-degree: 18%

---

# Merkury エンタープライズ Id の宛先

>[!NOTE]
>
>宛先コネクタとドキュメントページは、[!DNL Merkury] チームが作成および管理します。 お問い合わせや更新のリクエストについては、[!DNL Merkury] アカウント担当者にお問い合わせください。

## 概要

[!DNL Merkury Enterprise Identity] の宛先を使用して、より正確で包括的なインサイトに満ちた消費者プロファイルを作成します。 プロファイルデータが改善されたことで、マーケターはより優れたインサイト、セグメントおよびモデルを強化し、より正確なターゲティングと予測モデリングを実現できます。

![ 取り込みや有効化を含む、メルクリーとExperience Platformの相互接続を示す図 ](../../assets/catalog/data-partners/merkury-identity/media/image1.png)

このドキュメントページの手順に従って、Adobe Experience Platform ユーザーインターフェイスを使用して [!DNL Merkury Identity] しい宛先接続を作成し、識別およびエンリッチメントのためにオーディエンスをアクティブ化します。

>[!NOTE]
>
>[!DNL Merkury Connect] アカウントを使用して、メディアの宛先に対するオーディエンスのアクティブ化を行う場合は、代わりに [!DNL Merkury Connections] の宛先を使用します。

![ 宛先カタログでハイライト表示された Merkury Enterprise IdExperience Platformカード。](../../assets/catalog/data-partners/merkury-identity/media/image2.png)

## ユースケース

[!DNL Merkury Enterprise Identity] の宛先は、次の [!DNL Merkury] 機能のために、消費者 PII を安全に転送する機能を提供します。

* **データ品質**：データのハイジーンと標準化により、消費者プロファイルのデータ品質を向上させます。 [!DNL Merkury] には、最も高度なダイレクトメールマーケティングのユースケースをサポートするために、米国の郵便ハイジーンと移動識別が含まれます。
* **ID 解決**：個人 ID と世帯 ID に基づいて、顧客の正確で包括的な単一ビュー [!DNL Merkury] 構築します。 Merkury ID は、[!DNL Merkury] の包括的な米国の成人消費者 ID グラフ（2 億 6,800 万人以上）を活用して、深いレベルのプロファイルリンクを提供します。
* **エンリッチメント**:[!DNL Merkury Data] を使用して、インサイトとパーソナライゼーションを向上させます。 [!DNL Merkury Data] には、人口統計、ライフスタイル、金融、ライフイベント、[!DNL Merkury Data Suite] からの購入データなど、10,000 を超える利用可能なデータ属性が含まれています。

>[!NOTE]
>
>これらのユースケースは、宛先コネクタとソースコネクタの両方を組み合わせて実行されます。 顧客は、この宛先コネクタを使用して既存の顧客レコードを書き出してエンリッチメントを行うことから始めます。 [!DNL Merkury] のサービスはファイルを検索して取得し、[!DNL Merkury] のデータでエンリッチメントしてファイルを生成します。 次に、対応する [!DNL Merkury] Source コネクタのソースカードを使用して、ハイドレートされた顧客プロファイルをAdobe Real-Time CDPに取り込みます。

## 前提条件

>[!IMPORTANT]
>
>* 宛先に接続するには、**宛先の表示** と **宛先の管理**、**宛先のアクティブ化**、**プロファイルの表示**、**セグメントの表示**[[ アクセス制御権限 ]](https://experienceleague.adobe.com/en/docs/experience-platform/access-control/home#permissions) が必要です。 [[ アクセス制御の概要 ]](https://experienceleague.adobe.com/en/docs/experience-platform/access-control/ui/overview) を読むか、製品管理者に問い合わせて、必要な権限を取得してください。
>* *ID* を書き出すには、**ID グラフを表示** [[ アクセス制御権限 ]](https://experienceleague.adobe.com/en/docs/experience-platform/access-control/home#permissions) が必要です。\![ 宛先に対してオーディエンスをアクティブ化するために、ワークフローでハイライト表示されている ID 名前空間を選択 ](../../assets/catalog/data-partners/merkury-identity/media/image3.png)

## サポートされている ID {#supported-identities}

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | GOOGLE ADVERTISING ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。詳しくは、[ECID](/help/identity-service/features/ecid.md) に関する次のドキュメントを参照してください。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| extern_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| **オーディエンス** | **サポート対象** | **説明** | **接触チャネル** |
|---|---|---|---|
| セグメント化サービス | ✓ | Experience Platform[[ セグメント化サービス ]](https://experienceleague.adobe.com/ja/docs/experience-platform/segmentation/home) を通じて生成されたオーディエンス。 |
| カスタムアップロード | x | オーディエンス [[ インポート済み ]](https://experienceleague.adobe.com/en/docs/experience-platform/segmentation/ui/overview#import-audience) を CSV ファイルからExperience Platformにインポートします。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

| **オーディエンス** | **サポート対象** | **説明の起源** |
|---|---|---|      
| セグメント化サービス | ✓ | Experience Platform[[ セグメント化サービス ]](https://experienceleague.adobe.com/ja/docs/experience-platform/segmentation/home) を通じて生成されたオーディエンス。 |
| カスタムアップロード | X | オーディエンス [[ インポート済み ]](https://experienceleague.adobe.com/en/docs/experience-platform/segmentation/ui/overview#import-audience) を CSV ファイルからExperience Platformにインポートします。 |

{style="table-layout:auto"}

## 宛先への接続

>[!IMPORTANT]
>
>宛先に接続するには、**宛先の表示** と **データセット宛先の管理とアクティブ化** [[ アクセス制御権限 ]](https://experienceleague.adobe.com/en/docs/experience-platform/access-control/home#permissions) が必要です。 [[ アクセス制御の概要 ]](https://experienceleague.adobe.com/en/docs/experience-platform/access-control/ui/overview) を読むか、製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、[[ 宛先設定のチュートリアル ]](https://experienceleague.adobe.com/en/docs/experience-platform/destinations/ui/connect-destination) の手順に従います。 宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証

宛先に対して認証するには、必須フィールドに入力し、「**宛先に接続**」を選択します。

Experience Platformのバケットにアクセスするには、次の資格情報に有効な値を指定する必要があります。

| **認証情報** | **説明** |
|---|---|
| アクセスキー | バケットのアクセスキー ID。 この値は Merkury チームから取得できます。 |
| 秘密鍵 | バケットの秘密鍵 ID。 この値は Merkury チームから取得できます。 |
| バケット名 | これは、ファイルが共有されるバケットです。 この値は Merkury チームから取得できます。 |

{style="table-layout:auto"}

![ 新しい宛先作成画面 ](../../assets/catalog/data-partners/merkury-identity/media/image4.png)

### 宛先の詳細を入力

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![ 宛先の詳細のスクリーンショット ](../../assets/catalog/data-partners/merkury-identity/media/image6.png)


* **名前（必須）** – 宛先を保存する名前
* **説明** – 宛先の目的の短い説明
* **バケット名（必須）** - S3 に設定されたAmazon S3 バケットの名前
* **フォルダーパス （必須）** - バケット内のサブディレクトリを使用する場合は、パスを定義するか、「/」を使用してルートパスを参照する必要があります。
* **ファイルの種類** – 書き出したファイルに使用するExperience Platformの形式を選択します。 お使いのアカウントで想定されるファイルタイプについては、Merkury チームにお問い合わせください。

>[!NOTE]
>
>「CSV」オプションを選択すると、「区切り文字」、「引用符文字」、「エスケープ文字」、「空の値」、「Null 値」、「圧縮形式」、「マニフェストファイルを含める」の各オプションが表示されます。ご利用のアカウントに適した設定については、Merkury のチームにお問い合わせください。

![csv オプションの画像 ](../../assets/catalog/data-partners/merkury-identity/media/image8.png)

### 既存のアカウント

Merkury エンタープライズ ID 宛先を使用して既に定義されているアカウントが、リストポップアップに表示されます。 選択すると、右側のパネルにアカウントの詳細が表示されます。 **Destinations**/**Accounts** に移動すると、UI から例を表示できます。

![ 宛先アカウントページの宛先アカウントのスクリーンショット ](../../assets/catalog/data-partners/merkury-identity/media/image5.png)


### アラートの有効化

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、[UI を使用した宛先アラートの購読 ](https://experienceleague.adobe.com/ja/docs/experience-platform/destinations/ui/alerts) についてのガイドを参照してください。

宛先接続への詳細の入力を終えたら「**次へ**」を選択します。

## この宛先に対してオーディエンスをアクティブ化

>[!IMPORTANT]
>
>* データをアクティブ化するには、**宛先の表示**、**宛先のアクティブ化**、**プロファイルの表示** および **セグメントの表示** のアクセス制御権限が必要です。 詳しくは、アクセス制御の概要または製品管理者に問い合わせて、必要な権限を取得してください。
>* ID を書き出すには、**ID グラフの表示** アクセス制御権限が必要です。

この宛先に対してオーディエンスをアクティブ化する手順については、[ バッチプロファイル書き出し宛先に対するオーディエンスデータのアクティブ化 ](https://experienceleague.adobe.com/en/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations) を参照してください。

## マッピングの提案

[!DNL Merkury] 側でのファイルの正しい処理には、name 要素と address 要素が必要です。 すべての要素が必要なわけではありませんが、できるだけ多くを提供すると、マッチングを成功させるのに役立ちます。

以下の表に、マッピングの提案を示します。この提案は、顧客がプロファイル属性をマッピングする [!DNL Merkury] 処理で使用される、宛先側の属性をリストしています。 すべての要素が必要とは限らず、ソース値はアカウントのニーズに応じて異なるので、これらの要素を候補として扱います。

| ターゲットフィールド | Sourceの説明 |
|---|---|
| ID | [!DNL Merkury Enterprise Identity] Source コネクタを介して [!DNL Merkury] データをExperience Platformにマッピングするために使用する ID フィールド |
| Input_First_Name | Experience Platform内の `person.name.firstName` 値。 |
| Input_Last_Name | Experience Platform内の `person.name.lastName` 値。 |
| Input_Address_Line_1 | Experience Platform内の `mailingAddress.street` 値。 |
| Input_City | Experience Platform内の `mailingAddress.city` 値。 |
| Input_State_Province_Code | Experience Platform内の `mailingAddress.state` 値。 状態が 2 文字コード形式の場合は、を使用します。 |
| Input_State_Province_Name | Experience Platform内の `mailingAddress.state` 値。 状態が完全な状態名の場合は、を使用します |
| Input_Postal_Code | Experience Platform内の `mailingAddress.postalCode` 値。 |
| Input_Email_Address | プロファイルのメールアドレスとしてマッピングする値。 |
| Input_Phone | プロファイルの電話番号としてマッピングする値。 |

{style="table-layout:auto"}

## データの書き出しを検証する

データが正常に書き出されたかどうかを確認するには、Amazon S3 ストレージバケットを確認し、書き出されたファイルに、想定どおりのプロファイル母集団が含まれていることを確認します。

## データの使用とガバナンス

Adobe Experience Platformのすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 Adobe Experience Platformによるデータガバナンスの実施方法について詳しくは、[ データガバナンスの概要 ](https://experienceleague.adobe.com/en/docs/experience-platform/data-governance/home) を参照してください。

## 次の手順

このチュートリアルでは、Experience Platformから [!DNL Merkury] managed S3 の場所にプロファイルデータを書き出すデータフローを正常に作成しました。 次に、処理を設定できるように、アカウント名、ファイル名、バケットパスを [!DNL Merkury] 担当者に連絡する必要があります。
