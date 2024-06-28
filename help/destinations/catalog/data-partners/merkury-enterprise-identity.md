---
title: Merkury エンタープライズ Id の宛先
description: Adobe Experience Platform UI を使用して Merkury エンタープライズ ID 宛先接続を作成する方法を説明します。
hide: true
hidefromtoc: true
source-git-commit: 66a0a085e696dbe13d0368da395f655c7ca01a97
workflow-type: tm+mt
source-wordcount: '1502'
ht-degree: 16%

---


# Merkury エンタープライズ Id の宛先

>[!NOTE]
>
>宛先コネクタとドキュメントページは、Merkury チームが作成および管理します。 お問い合わせや更新のリクエストについては、Merkury アカウント担当者にお問い合わせください。

## 概要

Merkury Enterprise Identity の宛先を使用して、より正確で包括的かつインサイトに満ちた消費者プロファイルを作成します。 プロファイルデータが改善されたことで、マーケターはより優れたインサイト、セグメントおよびモデルを強化し、より正確なターゲティングと予測モデリングを実現できます。

![取り込みやアクティブ化を含め、Merkury とExperience Platformの間の相互接続を示す図](../../assets/catalog/data-partners/merkury-identity/media/image1.png)

このドキュメントページの手順に従って、Adobe Experience Platform ユーザーインターフェイスを使用して Merkury ID 宛先接続を作成し、識別およびエンリッチメントのためにオーディエンスをアクティブ化します。

>[!NOTE]
>
>Merkury Connect アカウントを使用してメディア宛先に対するオーディエンスをアクティブ化したい場合は、代わりに Merkury Connections の宛先を使用します。

![宛先カタログでハイライト表示された Merkury エンタープライズ IDExperience Platformカード。](../../assets/catalog/data-partners/merkury-identity/media/image2.png)

## ユースケース

Merkury Enterprise Identity Destination は、次の Merkury 機能のために、消費者 PII を安全に転送する機能を提供します。

* **データ品質**：データのハイジーンと標準化により、消費者プロファイルのデータ品質を向上させます。 Merkury には、最も高度なダイレクトメールマーケティングのユースケースをサポートするために、米国の郵便衛生と移動識別が含まれています。
* **ID 解決**:Merkury の個人 ID と世帯 ID に基づいて、顧客の正確で包括的な単一ビューを作成します。 Merkury ID は、Merkury の包括的な米国の成人消費者 ID グラフ（2 億 6,800 万人以上）を活用して、深いレベルのプロファイルリンクを提供します。
* **エンリッチメント**:Merkury データを使用して、インサイトとパーソナライゼーションを向上させます。 Merkury Data には、人口統計、ライフスタイル、金融、ライフイベント、Merkury Data Suite の購入データなど、10,000 を超える利用可能なデータ属性が含まれています。

>[!NOTE]
>
>これらのユースケースは、宛先コネクタとソースコネクタの両方を組み合わせて実行されます。 顧客は、この宛先コネクタを使用して既存の顧客レコードを書き出してエンリッチメントを行うことから始めます。 Merkury のサービスは、ファイルを検索して取得し、Merkury のデータで強化してファイルを生成します。 次に、対応する Merkury ソースコネクタソースカードを使用して、ハイドレートされた顧客プロファイルをAdobe Real-Time CDPに取り込みます。

## 前提条件

>[!IMPORTANT]
>
>* 宛先に接続するには、 **宛先の表示** および **宛先の管理**, **宛先のアクティブ化**, **プロファイルの表示**、および **セグメントの表示** [[ アクセス制御権限 ]](https://experienceleague.adobe.com/en/docs/experience-platform/access-control/home#permissions). を読み取る [[ アクセス制御の概要 ]](https://experienceleague.adobe.com/en/docs/experience-platform/access-control/ui/overview) または、製品管理者に問い合わせて、必要な権限を取得してください。
>* エクスポートする *id*、が必要です **ID グラフの表示** [[ アクセス制御権限 ]](https://experienceleague.adobe.com/en/docs/experience-platform/access-control/home#permissions).\![宛先に対してオーディエンスをアクティブ化するために、ワークフローで強調表示されている ID 名前空間を選択します。](../../assets/catalog/data-partners/merkury-identity/media/image3.png)

## サポートされている ID {#supported-identities}

| ターゲット ID | 説明 | 注意点 |
|---|---|---|
| GAID | Google Advertising ID | ソース ID が GAID 名前空間の場合は、GAID ターゲット ID を選択します。 |
| IDFA | Apple の広告主 ID | ソース ID が IDFA 名前空間の場合は、IDFA ターゲット ID を選択します。 |
| ECID | Experience Cloud ID | ECID を表す名前空間。 この名前空間は、「Adobe Marketing Cloud ID」、「Adobe Experience Cloud ID」、「Adobe Experience Platform ID」という別名で呼ばれることもあります。次のドキュメントを参照してください： [ECID](/help/identity-service/features/ecid.md) を参照してください。 |
| phone_sha256 | SHA256 アルゴリズムでハッシュ化された電話番号 | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化された電話番号の両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| email_lc_sha256 | SHA256 アルゴリズムでハッシュ化されたメールアドレス | Adobe Experience Platform では、プレーンテキストと SHA256 でハッシュ化されたメールアドレスの両方がサポートされています。ハッシュ化されていない属性がソースフィールドに含まれている場合は、「**[!UICONTROL 変換を適用]**」オプションをオンにして、アクティブ化時に [!DNL Platform] がデータを自動的にハッシュ化するように設定します。 |
| extern_id | カスタムユーザー ID | ソース ID がカスタム名前空間の場合は、このターゲット ID を選択します。 |

{style="table-layout:auto"}

## サポートされるオーディエンス

この節では、この宛先に書き出すことができるオーディエンスのタイプについて説明します。

| **オーディエンス** | **サポート** | **説明** | **起源** |
|---|---|---|---|
| セグメント化サービス | ✓ | Experience Platformを通じて生成されたオーディエンス [[ セグメント サービス ]](https://experienceleague.adobe.com/en/docs/experience-platform/segmentation/home). |
| カスタムアップロード | x | オーディエンス [[ 読み込み ]](https://experienceleague.adobe.com/en/docs/experience-platform/segmentation/ui/overview#import-audience) を CSV ファイルからExperience Platformに変換します。 |

{style="table-layout:auto"}

## 書き出しのタイプと頻度

宛先の書き出しのタイプと頻度について詳しくは、以下の表を参照してください。

|**Audience**|**Supported**|**Description origin**|            
|---|---|---|      
|Segmentation Service|✓|Audiences generated through the Experience Platform [[Segmentation Service]](https://experienceleague.adobe.com/en/docs/experience-platform/segmentation/home).|
Custom uploads|X|Audiences [[imported]](https://experienceleague.adobe.com/en/docs/experience-platform/segmentation/ui/overview#import-audience) into Experience Platform from CSV files.

{style="table-layout:auto"}

## 宛先への接続

>[!IMPORTANT]
>
>宛先に接続するには、 **宛先の表示** および **データセット宛先の管理とアクティブ化** [[ アクセス制御権限 ]](https://experienceleague.adobe.com/en/docs/experience-platform/access-control/home#permissions). を読み取る [[ アクセス制御の概要 ]](https://experienceleague.adobe.com/en/docs/experience-platform/access-control/ui/overview) または、製品管理者に問い合わせて、必要な権限を取得してください。

この宛先に接続するには、で説明されている手順に従います [[ 宛先設定のチュートリアル ]](https://experienceleague.adobe.com/en/docs/experience-platform/destinations/ui/connect-destination). 宛先の設定ワークフローで、以下の 2 つの節でリストされているフィールドに入力します。

### 宛先に対する認証

宛先に対して認証するには、必須フィールドに入力し、を選択します。 **宛先への接続**.

Experience Platformのバケットにアクセスするには、次の資格情報に有効な値を指定する必要があります。

| **認証情報** | **説明** |
|---|---|
| アクセスキー | バケットのアクセスキー ID。 この値は Merkury チームから取得できます。 |
| 秘密鍵 | バケットの秘密鍵 ID。 この値は Merkury チームから取得できます。 |
| バケット名 | これは、ファイルが共有されるバケットです。 この値は Merkury チームから取得できます。 |

{style="table-layout:auto"}

![新しい宛先作成画面](../../assets/catalog/data-partners/merkury-identity/media/image4.png)

### 宛先の詳細を入力

宛先の詳細を設定するには、以下の必須フィールドとオプションフィールドに入力します。UI のフィールドの横のアスタリスクは、そのフィールドが必須であることを示します。

![宛先の詳細のスクリーンショット](../../assets/catalog/data-partners/merkury-identity/media/image6.png)


* **名前（必須）**  – 保存先の名前
* **説明**  – 宛先の目的の簡単な説明
* **バケット名（必須）** - S3 で設定されたAmazon S3 バケットの名前
* **フォルダーパス （必須）** - バケット内のサブディレクトリを使用する場合は、パスを定義するか、「/」を使用してルートパスを参照する必要があります。
* **ファイルタイプ**  – 書き出したファイルでExperience Platformが使用するフォーマットを選択します。 お使いのアカウントで想定されるファイルタイプについては、Merkury チームにお問い合わせください。

>[!NOTE]
>
>「CSV」オプションを選択すると、「区切り文字」、「引用符文字」、「エスケープ文字」、「空の値」、「Null 値」、「圧縮形式」、「マニフェストファイルを含める」の各オプションが表示されます。具体的には、お使いのアカウントに適した設定を Merkury チームが選択します。

![csv オプションの画像](../../assets/catalog/data-partners/merkury-identity/media/image8.png)

### 既存のアカウント

Merkury エンタープライズ ID 宛先を使用して既に定義されているアカウントが、リストポップアップに表示されます。 選択すると、右側のパネルにアカウントの詳細が表示されます。 に移動したら、UI から例を表示します。 **宛先** > **アカウント**;

![宛先アカウントページの宛先アカウントのスクリーンショット](../../assets/catalog/data-partners/merkury-identity/media/image5.png)


### アラートの有効化

アラートを有効にすると、宛先へのデータフローのステータスに関する通知を受け取ることができます。リストからアラートを選択して、データフローのステータスに関する通知を受け取るよう登録します。アラートについて詳しくは、のガイドを参照してください。 [ui を使用した宛先アラートの購読](https://experienceleague.adobe.com/en/docs/experience-platform/destinations/ui/alerts).

宛先接続への詳細の入力を終えたら以下を選択します **次**.

## この宛先に対してオーディエンスをアクティブ化

>[!IMPORTANT]
>
>* データをアクティブ化するには、宛先の表示、宛先のアクティブ化、プロファイルの表示およびセグメントの表示に対するアクセス制御権限が必要です。 詳しくは、アクセス制御の概要または製品管理者に問い合わせて、必要な権限を取得してください。
>* ID を書き出すには、ID グラフの表示アクセス制御権限が必要です。

Read [プロファイル書き出しのバッチ宛先に対するオーディエンスデータの有効化](https://experienceleague.adobe.com/en/docs/experience-platform/destinations/ui/activate/activate-batch-profile-destinations) この宛先に対してオーディエンスをアクティブ化する手順については、を参照してください。

## マッピングの提案

Merkury 側のファイルの正しい処理には、名前とアドレスの要素が必要です。 すべての要素が必要なわけではありませんが、できるだけ多くを提供すると、マッチングを成功させるのに役立ちます。

以下の表に、マッピングの提案を示します。この提案には、顧客がプロファイル属性をマッピングできる Merkury 処理で使用される、宛先側の属性がリストされています。 すべての要素が必要とは限らず、ソース値はアカウントのニーズに応じて異なるので、これらの要素を候補として扱います。

| ターゲットフィールド | ソースの説明 |
|---|---|
| ID | Merkury エンタープライズ ID 解決ソースコネクタを介して Merkury データをExperience Platformにマッピングするために使用する ID フィールド |
| Input_First_Name | この `person.name.firstName` Experience Platform内の値。 |
| Input_Last_Name | この `person.name.lastName` Experience Platform内の値。 |
| Input_Address_Line_1 | この `mailingAddress.street` Experience Platform内の値。 |
| Input_City | この `mailingAddress.city` Experience Platform内の値。 |
| Input_State_Province_Code | この `mailingAddress.state` Experience Platform内の値。 状態が 2 文字コード形式の場合は、を使用します。 |
| Input_State_Province_Name | この `mailingAddress.state` Experience Platform内の値。 状態が完全な状態名の場合は、を使用します |
| Input_Postal_Code | この `mailingAddress.postalCode` Experience Platform内の値。 |
| Input_Email_Address | プロファイルのメールアドレスとしてマッピングする値。 |
| Input_Phone | プロファイルの電話番号としてマッピングする値。 |

{style="table-layout:auto"}

## データの書き出しを検証する

データが正常に書き出されたかどうかを確認するには、Amazon S3 ストレージバケットを確認し、書き出されたファイルに、想定どおりのプロファイル母集団が含まれていることを確認します。

## データの使用とガバナンス

Adobe Experience Platformのすべての宛先は、データを処理する際のデータ使用ポリシーに準拠しています。 Adobe Experience Platformによるデータガバナンスの実施方法について詳しくは、以下を参照してください [データガバナンスの概要](https://experienceleague.adobe.com/en/docs/experience-platform/data-governance/home).

## 次の手順

このチュートリアルに従うと、Experience Platformから Merkury Managed S3 の場所にプロファイルデータを書き出すデータフローを正常に作成できます。 次に、処理を設定できるように、アカウント名、ファイル名、バケットパスを Merkury 担当者に連絡する必要があります。
