---
description: Destination SDK で作成された宛先でサポートされるターゲット ID の設定方法を説明します。
title: ID 名前空間設定
exl-id: 30c0939f-b968-43db-b09b-ce5b34349c6e
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '925'
ht-degree: 60%

---

# ID 名前空間設定

Experience Platform は、ID 名前空間を使用して、特定の ID のタイプを記述します。例えば、`Email` と呼ばれる ID 名前空間は、`name@email.com` のような値をメールアドレスとして識別します。

作成する宛先のタイプ（ストリーミングまたはファイルベース）に応じて、次の ID 名前空間要件に注意してください。

* Destination SDKでリアルタイム（ストリーミング）宛先を作成する場合、ユーザーがプロファイル属性や ID をマッピングできる [ パートナースキーマの設定 ](schema-configuration.md) に加えて、宛先プラットフォームでサポートされている *少なくとも 1 つ* ID 名前空間も定義する必要があります。 例えば、宛先プラットフォームがハッシュ化されたメールと [!DNL IDFA] を受け入れる場合、これら 2 つの ID をとして定義する必要があります [ 詳しくは、このドキュメントの後半で説明します ](#supported-parameters)。

  >[!IMPORTANT]
  >
  >ストリーミング宛先に対してオーディエンスをアクティブ化する場合、ユーザーは、ターゲットプロファイル属性に加えて、_少なくとも 1 つのターゲット ID_ もマッピングする必要があります。 そうでない場合、オーディエンスは宛先プラットフォームに対してアクティブ化されません。

* Destination SDKを使用してファイルベースの宛先を作成する場合、ID 名前空間の設定は _オプション_ です。

Experience Platform の ID 名前空間について詳しくは、[ID 名前空間ドキュメント](../../../../identity-service/features/namespaces.md)を参照してください。

宛先用に ID 名前空間を設定する場合、宛先でサポートされているターゲット ID マッピングを微調整できます。以下に例を示します。

* ユーザーは、XDM 属性を ID 名前空間にマッピングできます。
* ユーザーは、[標準的な ID 名前空間](../../../../identity-service/features/namespaces.md#standard)を独自の ID 名前空間にマッピングできます。
* ユーザーは、[カスタム ID 名前空間](../../../../identity-service/features/namespaces.md#manage-namespaces)を独自の ID 名前空間にマッピングできます。

このコンポーネントがDestination SDKで作成される統合のどこに適合するかを把握するには、[ 設定オプション ](../configuration-options.md) ドキュメントの図を参照するか、[Destination SDKを使用したファイルベースの宛先の設定 ](../../guides/configure-file-based-destination-instructions.md#create-server-file-configuration) 方法に関するガイドを参照してください。

`/authoring/destinations` エンドポイントを介して、サポートされる ID 名前空間を設定できます。このページに表示されるコンポーネントを設定できる、詳細な API 呼び出しの例については、以下の API リファレンスページを参照してください。

* [宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)
* [宛先設定の更新](../../authoring-api/destination-configuration/update-destination-configuration.md)

この記事では、宛先に使用できる、サポートされるすべての ID 名前空間設定オプションを説明し、Experience Platform UI で顧客に何が表示されるかを示します。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## サポートされる統合タイプ {#supported-integration-types}

このページで説明される機能をサポートする統合のタイプについて詳しくは、以下の表を参照してください。

| 統合タイプ | 機能のサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | はい（必須） |
| ファイルベースの（バッチ）統合 | はい（オプション） |

## サポートされるパラメーター {#supported-parameters}

宛先がサポートするターゲット ID を定義する際に、以下の表で説明されているパラメーターを使用して、その動作を設定できます。

| パラメーター | タイプ | 必須／オプション | 説明 |
|---------|----------|---|------|
| `acceptsAttributes` | ブール値 | オプション | 顧客が標準的なプロファイル属性を設定中の ID にマッピングできるかどうかを示します。 |
| `acceptsCustomNamespaces` | ブール値 | オプション | 顧客がカスタム ID 名前空間を設定中の ID 名前空間にマッピングできるかどうかを示します。 |
| `acceptedGlobalNamespaces` | - | オプション | 設定中の ID に顧客がマッピングできる[標準的な ID 名前空間](../../../../identity-service/features/namespaces.md#standard)（例えば、[!UICONTROL IDFA]）を示します。 |
| `transformation` | 文字列 | オプション | ソースフィールドが XDM 属性かカスタム ID 名前空間のどちらかの場合に、Experience Platform UI に「[[!UICONTROL  変換を適用 ]](../../../ui/activate-segment-streaming-destinations.md#apply-transformation)」チェックボックスを表示します。 このオプションを使用して、書き出し時にソース属性をハッシュ化する機能をユーザーに提供します。このオプションを有効にするには、値を `sha256(lower($))` に設定します。 |
| `requiredTransformation` | 文字列 | オプション | 顧客がこのソース ID 名前空間を選択すると、「[[!UICONTROL 変換を適用]](../../../ui/activate-segment-streaming-destinations.md#apply-transformation)」チェックボックスが自動的にマッピングに適用され、顧客は無効にすることができなくなります。このオプションを有効にするには、値を `sha256(lower($))` に設定します。 |

{style="table-layout:auto"}


```json
"identityNamespaces":{
      "external_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "acceptedGlobalNamespaces":{
            "Email":{
            }
         }
      },
      "another_id":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true
      }
   }
```

どの [!DNL Experience Platform] ID の顧客が宛先に書き出すことができるかを示す必要があります。例として、[!DNL Experience Cloud ID]、ハッシュ化されたメール、デバイス ID（[!DNL IDFA]、[!DNL GAID]）などがあります。これらの値は、[!DNL Experience Platform] ID 名前空間であり、顧客が宛先から ID 名前空間にマッピングできます。

ID 名前空間は、[!DNL Experience Platform] と宛先が 1 対 1 で対応している必要はありません。
例えば、顧客は [!DNL Experience Platform] [!DNL IDFA] 名前空間を宛先からの [!DNL IDFA] 名前空間にマッピングすることができ、また顧客は同じ [!DNL Experience Platform] [!DNL IDFA] 名前空間を宛先の [!DNL Customer ID] 名前空間にマッピングすることもできます。

ID について詳しくは、[ID 名前空間の概要](../../../../identity-service/features/namespaces.md)を参照してください。

## マッピングに関する考慮事項

お客様がソース ID 名前空間を選択して、ターゲットマッピングを選択しない場合、Experience Platformは、自動的に同じ名前の属性でターゲットマッピングを設定します。

## オプションのソースフィールドハッシュの設定

Experience Platformのお客様は、ハッシュ化された形式またはプレーンテキストでExperience Platformにデータを取り込むことを選択できます。 宛先プラットフォームがハッシュ化されたデータとハッシュ化されていないデータの両方を受け入れる場合、宛先に書き出される際に、Experience Platformがソースフィールド値をハッシュ化する必要があるかどうかを顧客が選択できるようにすることができます。

以下の設定は、Experience Platform UI のマッピング手順でオプションの [ 変換を適用 ](../../../ui/activate-segment-streaming-destinations.md#apply-transformation) オプションを有効にします。

```json {line-numbers="true" highlight="5"}
"identityNamespaces":{
      "Customer_contact":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "transformation": "sha256(lower($))",
         "acceptedGlobalNamespaces":{
            "Email":{
            },
            "Phone":{
            }
         }
      }
   }
```

ハッシュ化されていないソースフィールドを使用している場合に、このオプションを有効にすると、Adobe Experience Platform でアクティベーション時に自動的にハッシュ化されます。

ハッシュ化されていないソース属性を、宛先によってハッシュ化されることが期待されているターゲット属性（例：`email_lc_sha256` や `phone_sha256`）にマッピングしている場合、アクティベーション時に Adobe Experience Platform にソース属性を自動的にハッシュ化させるために、「**変換を適用**」オプションをオンにします。

## 必須のソースフィールドハッシュの設定

宛先がハッシュ化されたデータのみを受け入れる場合、書き出された属性がExperience Platformによって自動ハッシュ化されるように設定できます。 以下の設定は、`Email` および `Phone` ID がマッピングされると、「**変換を適用**」オプションを自動的にオンにします。

```json {line-numbers="true" highlight="8,11"}
"identityNamespaces":{
      "Customer_contact":{
         "acceptsAttributes":true,
         "acceptsCustomNamespaces":true,
         "transformation": "sha256(lower($))",
         "acceptedGlobalNamespaces":{
            "Email":{
               "requiredTransformation": "sha256(lower($))"
            },
            "Phone":{
               "requiredTransformation": "sha256(lower($))"
            }
         }
      }
   }
```

## 次の手順 {#next-steps}

この記事を読むことで、Destination SDK で作成された宛先に対する ID 名前空間の設定方法ついて、理解を深めることができました。

その他の宛先コンポーネントについて詳しくは、以下の記事を参照してください。

* [顧客認証](customer-authentication.md)
* [OAuth2 認証](oauth2-authorization.md)
* [顧客データフィールド](customer-data-fields.md)
* [UI 属性](ui-attributes.md)
* [スキーマ設定](schema-configuration.md)
* [ID 名前空間設定](identity-namespace-configuration.md)
* [サポートされるマッピング設定](supported-mapping-configurations.md)
* [宛先配信](destination-delivery.md)
* [オーディエンスメタデータ設定](audience-metadata-configuration.md)
* [集計ポリシー](aggregation-policy.md)
* [バッチ設定](batch-configuration.md)
* [プロファイル選定履歴](historical-profile-qualifications.md)
