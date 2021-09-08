---
description: このページのコンテンツを、パートナーの宛先の残りの設定オプションと共に使用します。 このページでは、Adobe Experience Platformから宛先に書き出されるデータのメッセージ形式について説明し、他のページでは、宛先への接続と認証に関する詳細について説明します。
seo-description: Use the content on this page together with the rest of the configuration options for partner destinations. This page addresses the messaging format of data exported from Adobe Experience Platform to destinations, while the other page addresses specifics about connecting and authenticating to your destination.
seo-title: Message format
title: メッセージのフォーマット
source-git-commit: d60933d2083b7befcfa8beba4b1630f372c08cfa
workflow-type: tm+mt
source-wordcount: '1505'
ht-degree: 3%

---

# メッセージのフォーマット

## 前提条件 — Adobe Experience Platformの概念 {#prerequisites}

Adobe側のプロセスを理解するには、次のExperience Platform概念を理解してください。

* **エクスペリエンスデータモデル(XDM)**&#x200B;を参照してください。[XDMの概](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html?lang=ja) 要と  [Adobe Experience PlatformでXDMスキーマを作成する方法](https://experienceleague.adobe.com/docs/experience-platform/xdm/tutorials/create-schema-ui.html?lang=en)を参照してください。
* **クラス**. [UIでクラスを作成および編集します](https://experienceleague.adobe.com/docs/experience-platform/xdm/ui/resources/classes.html?lang=en)。
* **フィールドグループ**&#x200B;を参照してください。[フィールドグル](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/composition.html?lang=en#field-group) ープの定義 [と、フィールドグループの詳細](https://experienceleague.adobe.com/docs/experience-platform/xdm/tutorials/create-schema-ui.html?lang=en#field-group)を参照してください。
* **IdentityMap**&#x200B;を参照してください。IDマップは、Adobe Experience PlatformのすべてのエンドユーザーIDのマップを表します。 [XDMフィールドディクショナリ](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/field-dictionary.html?lang=en)の`xdm:identityMap`を参照してください。
* **SegmentMembership**&#x200B;を参照してください。[segmentMembership](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/field-dictionary.html?lang=en) XDM属性は、プロファイルがメンバーであるセグメントを通知します。 `status`フィールドの3つの異なる値については、[Segment Membership Details schema field field](https://experienceleague.adobe.com/docs/experience-platform/xdm/field-groups/profile/segmentation.html)のドキュメントを参照してください。

## 概要 {#overview}

このページのコンテンツを、パートナーの宛先](./configuration-options.md)の他の[設定オプションと共に使用します。 このページでは、Adobe Experience Platformから宛先に書き出されるデータのメッセージ形式について説明し、他のページでは、宛先への接続と認証に関する詳細について説明します。

Adobe Experience Platformは、様々なデータ形式で大量の宛先にデータを書き出します。 宛先のタイプの例としては、広告プラットフォーム(Google)、ソーシャルネットワーク(Facebook)、クラウドストレージの場所(Amazon S3、Azure Event Hubs)などがあります。

Experience Platformは、書き出されたメッセージの形式を、想定される形式に合わせて調整できます。 このカスタマイズを理解するには、次の概念が重要です。
* Adobe Experience Platformのソース(1)とターゲット(2)のXDMスキーマ
* パートナー側のメッセージの形式(3)
* 2つの間の変換レイヤー。[メッセージ変換テンプレート](./message-format.md#using-templating)を作成して定義できます。

![スキーマからJSONへの変換](./assets/transformations-3-steps.png)

Experience Platform では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。

宛先に対してデータをアクティブ化するユーザーは、データセットに使用するフィールドを、宛先の想定される形式に変換するスキーマにExperience Platformしてマッピングする必要があります。 Adobeは、会社のカスタムフィールドグループを作成し、ターゲットスキーマに追加します。 フィールドグループのフィールドは、受け取ることができるprofile属性フィールドによって異なります。

**ソースXDMスキーマ(1)**:顧客がExperience Platformで使用するスキーマを指します。Experience Platformでは、宛先のアクティブ化ワークフローの[マッピング手順](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activate-segment-streaming-destinations.html?lang=en#mapping)で、ユーザーはソーススキーマのフィールドを宛先のターゲットスキーマにマッピングします(2)。

**ターゲットXDMスキーマ(2)**:Adobeと共有するJSON標準スキーマ(3)に基づいて、Adobeチームが宛先のカスタムスキーマを作成します。プロジェクト](./overview.md#phased-approach)の[今後の段階では、独自に宛先のカスタムスキーマを作成できます。

**宛先プロファイル属性のJSON標準スキーマ(3)**:プラットフォームがサポー [トす](https://json-schema.org/learn/miscellaneous-examples.html) るすべてのプロファイル属性とそのタイプ(例：オブジェクト、文字列、配列)。宛先でサポートされるフィールドの例は、`firstName`、`lastName`、`gender`、`email`、`phone`、`productId`、`productName`などです。

上記で説明したスキーマ変換に基づいて、ソースXDMスキーマとパートナー側のサンプルスキーマの間でメッセージの構造がどのように変化するかを示します。

![変換メッセージの例](./assets/transformations-with-examples.png)

<br> 


## はじめに — 3つの基本的な属性の変換 {#getting-started}

変換プロセスの例として、以下の例では、Adobe Experience Platformの3つの一般的なプロファイル属性を使用しています。**名**、**姓**、**電子メールアドレス**。

>[!NOTE]
>
>顧客は、[宛先ワークフローのアクティブ化](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate-destinations.html#mapping)の&#x200B;**マッピング**&#x200B;手順で、ソースXDMスキーマの属性をAdobe Experience Platform UIのパートナーXDMスキーマにマッピングします。

プラットフォームが次のようなメッセージ形式を受信できるとします。

```curl
POST https://YOUR_REST_API_URL/users/
Content-Type: application/json
Authorization: Bearer YOUR_REST_API_KEY

{
  "attributes":
    {
      "first_name": "Yours",
      "last_name": "Truly",
      "external_id": "yourstruly@adobe.com"
    }
}
```

メッセージ形式を考慮すると、対応する変換は次のようになります。

| Adobe側のパートナーXDMスキーマの属性 | 変換 | HTTPメッセージの属性 |
|---------|----------|---------|
| `_your_custom_schema.firstName` | ` attributes.first_name` | `first_name` |
| `_your_custom_schema.lastName` | `attributes.last_name` | `last_name` |
| `personalEmail.address` | `attributes.external_id` | `external_id` |

## ID、属性、セグメントメンバーシップ変換にテンプレート言語を使用する {#using-templating}

Adobeは、[Jinjer](https://jinja.palletsprojects.com/en/2.11.x/)と類似したテンプレート言語を使用して、XDMスキーマのフィールドを、宛先でサポートされている形式に変換します。

この節では、入力XDMスキーマからテンプレートを介して、宛先で受け入れられるペイロード形式にこれらの変換が出力される方法の例をいくつか示します。 以下の例は、次のように、複雑さを増すことで分類されています。

1. 単純な変換の例です。 [プロファイル属性](./message-format.md#attributes)、[セグメントメンバーシップ](./message-format.md#segment-membership)、[ID](./message-format.md#identities)フィールドの単純な変換でテンプレートがどのように機能するかを説明します。
2. 上記のフィールドを組み合わせたテンプレートの複雑な例を増やしました。[セグメントとIDを送信するテンプレートを作成](./message-format.md#segments-and-identities)および[セグメント、ID、プロファイル属性を送信するテンプレートを作成](./message-format.md#segments-identities-attributes)します。
3. 業界パートナーのテンプレートの例を2つ示し、最も深く掘り下げます。

### プロファイル属性 {#attributes}

書き出したプロファイル属性を宛先に変換するには、以下のJSONとコードサンプルを参照してください。


>[!IMPORTANT]
>
>Adobe Experience Platformで使用可能なすべてのプロファイル属性のリストについては、[XDMフィールドディクショナリ](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/field-dictionary.html?lang=en)を参照してください。



**入力**

プロファイル1:

```json
{
    "attributes": {
        "firstName": {
            "value": "Hermione"
    },
    "birthDate": {}
  }
}
```

プロファイル2:

```json
{
  "attributes": {
    "firstName": {
      "value": "Harry"
    },
    "birthDate": {
        "value": "1980/07/31"
    }
  }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、[宛先サーバー設定](./server-and-template-configuration.md#template-specs)にテンプレートを挿入する前に、二重引用符`""`などの無効な文字をエスケープする必要があります。 二重引用符のエスケープについて詳しくは、[JSON標準](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf)の第9章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            {% for attribute in profile.attributes %}
            "{{ attribute.key }}":
                {% if attribute.value is empty %}
                    null
                {% else %}
                    "{{ attribute.value.value }}"
                {% endif %}
            {% if not loop.last %},{% endif %}
            {% endfor %}
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**


```json
{
    "profiles": [
        {
            "firstName": "Hermione",
            "birthDate": null
        },
        {
            "firstName": "Harry",
            "birthDate": "1980/07/31"
        }
    ]
}
```

### セグメントのメンバーシップ {#segment-membership}

[segmentMembership](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/field-dictionary.html?lang=en) XDM属性は、プロファイルがメンバーであるセグメントを通知します。
`status`フィールドの3つの異なる値については、[Segment Membership Details schema field field](https://experienceleague.adobe.com/docs/experience-platform/xdm/field-groups/profile/segmentation.html)のドキュメントを参照してください。

**入力**

プロファイル1:

```json
{
  "segmentMembership": {
    "ups": {
      "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "realized"
      },
      "788d8874-8007-4253-92b7-ee6b6c20c6f3": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "existing"
      },
      "8f812592-3f06-416b-bd50-e7831848a31a": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "exited"
      }
    }
  }
}
```

プロファイル2:

```json
{
  "segmentMembership": {
    "ups": {
      "32396e4b-16f6-4033-9702-fc69b5e24e7c": {
        "lastQualificationTime": "2021-08-20T17:23:04Z",
        "status": "realized"
      },
      "af854278-894a-4192-a96b-320fbf2623fd": {
        "lastQualificationTime": "2021-08-20T16:44:37Z",
        "status": "existing"
      },
      "66505bf9-bc08-4bac-afbc-8b6706650ea4": {
        "lastQualificationTime": "2019-08-20T17:23:04Z",
        "status": "realized"
      }
    }
  }
}
```

**テンプレート**


>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、[宛先サーバー設定](./server-and-template-configuration.md#template-specs)にテンプレートを挿入する前に、二重引用符`""`などの無効な文字をエスケープする必要があります。 二重引用符のエスケープについて詳しくは、[JSON標準](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf)の第9章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "AdobeExperiencePlatformSegments": {
                "add": [
                {% for segment in profile.segmentMembership.ups | added %}
                "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ],
                "remove": [
                {# Alternative syntax for filtering segments by status: #}
                {% for segment in removedSegments(profile.segmentMembership.ups) %}
                "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ]
            }
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

```json
{
    "profiles": [
        {
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075",
                    "788d8874-8007-4253-92b7-ee6b6c20c6f3"
                ],
                "remove": [
                    "8f812592-3f06-416b-bd50-e7831848a31a"
                ]
            }
        },
        {
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "32396e4b-16f6-4033-9702-fc69b5e24e7c",
                    "af854278-894a-4192-a96b-320fbf2623fd",
                    "66505bf9-bc08-4bac-afbc-8b6706650ea4"
                ],
                "remove": [
                ]
            }
        }
    ]
}
```

### ID {#identities}

Experience PlatformのIDについて詳しくは、[ID名前空間の概要](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja)を参照してください。

**入力**

プロファイル1:

```json
{
    "identityMap": {
        "email": [
            {
                "id": "johndoe@example.com"
            },
            {
                "id": "jd@example.com"
            }
        ],
        "external_id": [
            {
                "id": "123456"
            }
        ]
    }
}
```

プロファイル2:

```json
{
    "identityMap": {
        "email": [
            {
                "id": "jane.doe@example.com"
            }
        ]
    }
}
```

**テンプレート**


>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、[宛先サーバー設定](./server-and-template-configuration.md#template-specs)にテンプレートを挿入する前に、二重引用符`""`などの無効な文字をエスケープする必要があります。 二重引用符のエスケープについて詳しくは、[JSON標準](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf)の第9章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "identities": [
                {% for email in profile.identityMap.email %}
                {
                    "type": "email",
                    "id": "{{ email.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}

                {# Add a comma only if you have both emails and external_ids. #}
                {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}
                    ,
                {% endif %}

                {% for external in profile.identityMap.external_id %}
                {
                    "type": "external_id",
                    "id": "{{ external.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
            ]
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

```json
{
    "profiles": [
        {
            "identities": [
                {
                    "type": "email",
                    "id": "johndoe@example.com"
                },
                {
                    "type": "email",
                    "id": "jd@example.com"
                },
                {
                    "type": "external_id",
                    "id": "123456"
                }
            ]
        },
        {
            "identities": [
                {
                    "type": "email",
                    "id": "jane.doe@example.com"
                }
            ]
        }
    ]
}
```


### セグメントとIDを送信するテンプレートの作成 {#segments-and-identities}

この節では、AdobeXDMスキーマとパートナー宛先スキーマの間でよく使用される変換の例を示します。
次の例では、セグメントのメンバーシップとIDの形式を変換して、宛先に出力する方法を示します。

**入力**

プロファイル1:

```json
{
    "identityMap": {
        "email": [
            {
                "id": "johndoe@example.com"
            },
            {
                "id": "jd@example.com"
            }
        ],
        "external_id": [
            {
                "id": "123456"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "realized"
            },
            "788d8874-8007-4253-92b7-ee6b6c20c6f3": {
              "lastQualificationTime": "2019-11-20T13:15:49Z",
              "status": "existing"
            },
            "8f812592-3f06-416b-bd50-e7831848a31a": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "exited"
            }
        }
    }
}
```

プロファイル2:

```json
{
    "identityMap": {
        "email": [
            {
                "id": "jane.doe@example.com"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2021-08-31T10:01:42Z",
                "status": "realized"
            }
        }
    }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、[宛先サーバー設定](./server-and-template-configuration.md#template-specs)にテンプレートを挿入する前に、二重引用符`""`などの無効な文字をエスケープする必要があります。 二重引用符のエスケープについて詳しくは、[JSON標準](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf)の第9章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "identities": [
                {% for email in profile.identityMap.email %}
                {
                    "type": "email",
                    "id": "{{ email.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
                
                {# Add a comma only if you have both emails and external_ids. #}
                {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}
                    ,
                {% endif %}
                
                {% for external in profile.identityMap.external_id %}
                {
                    "type": "external_id",
                    "id": "{{ external.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    {% for segment in profile.segmentMembership.ups | added %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                    {% endfor %}
                ],
                "remove": [
                    {# Alternative syntax for filtering segments by status: #}
                    {% for segment in removedSegments(profile.segmentMembership.ups) %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                    {% endfor %}
                ]
            }
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

以下の`json`は、Adobe Experience Platformからエクスポートされたデータを表しています。

```json
{
    "profiles": [
        {
            "identities": [
                {
                    "type": "email",
                    "id": "johndoe@example.com"
                },
                {
                    "type": "email",
                    "id": "jd@example.com"
                },
                {
                    "type": "external_id",
                    "id": "123456"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075",
                    "788d8874-8007-4253-92b7-ee6b6c20c6f3"
                ],
                "remove": [
                    "8f812592-3f06-416b-bd50-e7831848a31a"
                ]
            }
        },
        {
            "identities": [
                {
                    "type": "email",
                    "id": "jane.doe@example.com"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075"
                ],
                "remove": []
            }
        }
    ]
}
```

### セグメント、IDおよびプロファイル属性を送信するテンプレートの作成 {#segments-identities-attributes}

この節では、AdobeXDMスキーマとパートナー宛先スキーマの間でよく使用される変換の例を示します。

もう1つの一般的な使用例は、セグメントのメンバーシップ、ID(例：電子メールアドレス、電話番号、広告ID)、プロファイル属性。 この方法でデータをエクスポートするには、次の例を参照してください。

**入力**

プロファイル1:

```json
{
    "attributes": {
        "firstName": {
            "value": "Hermione"
        },
        "birthDate": {}
    },
    "identityMap": {
        "email": [
            {
                "id": "johndoe@example.com"
            },
            {
                "id": "jd@example.com"
            }
        ],
        "external_id": [
            {
                "id": "123456"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "realized"
            },
            "788d8874-8007-4253-92b7-ee6b6c20c6f3": {
              "lastQualificationTime": "2019-11-20T13:15:49Z",
              "status": "existing"
            },
            "8f812592-3f06-416b-bd50-e7831848a31a": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "exited"
            }
        }
    }
}
```

プロファイル2:

```json
{
    "attributes": {
        "firstName": {
            "value": "Harry"
        },
        "birthDate": {
            "value": "1980/07/31"
        }
    },
    "identityMap": {
        "email": [
            {
                "id": "harry.p@example.com"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "realized"
            }
        }
    }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートに対して、[宛先サーバー設定](./server-and-template-configuration.md#template-specs)にテンプレートを挿入する前に、二重引用符`""`などの無効な文字をエスケープする必要があります。 二重引用符のエスケープについて詳しくは、[JSON標準](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf)の第9章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "attributes": {
            {% for attribute in profile.attributes %}
                "{{ attribute.key }}":
                    {% if attribute.value is empty %}
                        null
                    {% else %}
                        "{{ attribute.value.value }}"
                    {% endif %}
                {% if not loop.last %},{% endif %}
            {% endfor %}
            },
            "identities": [
                {% for email in profile.identityMap.email %}
                {
                    "type": "email",
                    "id": "{{ email.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}

                {# Add a comma only if we have both emails and external_ids. #}
                {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}
                    ,
                {% endif %}

                {% for external in profile.identityMap.external_id %}
                {
                    "type": "external_id",
                    "id": "{{ external.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                {% for segment in profile.segmentMembership.ups | added %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ],
                "remove": [
                {# Alternative syntax for filtering segments by status: #}
                {% for segment in removedSegments(profile.segmentMembership.ups) %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ]
            }
        }
    ]
}
```

**結果**

以下の`json`は、Adobe Experience Platformからエクスポートされたデータを表しています。

```json
{
    "profiles": [
        {
            "attributes": {
                "firstName": "Hermione",
                "birthDate": null
            },
            "identities": [
                {
                    "type": "email",
                    "id": "johndoe@example.com"
                },
                {
                    "type": "email",
                    "id": "jd@example.com"
                },
                {
                    "type": "external_id",
                    "id": "123456"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075",
                    "788d8874-8007-4253-92b7-ee6b6c20c6f3"
                ],
                "remove": [
                    "8f812592-3f06-416b-bd50-e7831848a31a"
                ]
            }
        },
        {
            "attributes": {
                "firstName": "Harry",
                "birthDate": "1980/07/21"
            },
            "identities": [
                {
                    "type": "email",
                    "id": "harry.p@example.com"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075"
                ],
                "remove": []
            }
        }
    ]
}
```

### リファレンス：変換テンプレートで使用されるコンテキストと関数

テンプレートに指定されたコンテキストには、`input`（この呼び出しで書き出されるプロファイル/データ）と`destination`(Adobeがデータを送信する宛先に関するデータ。すべてのプロファイルで有効)が含まれます。

以下の表に、上記の例に含まれる関数の説明を示します。

| 関数 | 説明 |
|---------|----------|
| `input.profile` | プロファイル。[JsonNode](http://fasterxml.github.io/jackson-databind/javadoc/2.11/com/fasterxml/jackson/databind/node/JsonNodeType.html)で表されます。 このページで後述するパートナーXDMスキーマに従います。 |
| `destination.segmentAliases` | Adobe Experience Platform名前空間のセグメントIDからパートナーのシステムのセグメントエイリアスにマッピングします。 |
| `destination.segmentNames` | Adobe Experience Platform名前空間のセグメント名からパートナーのシステムのセグメント名にマッピングします。 |
| `addedSegments(listOfSegments)` | ステータスが`realized`または`existing`のセグメントのみを返します。 |
| `removedSegments(listOfSegments)` | ステータス`exited`を持つセグメントのみを返します。 |

<!--

## What Adobe needs from you to set up your destination {#what-adobe-needs}

Based on the transformations outlined in the sections above, Adobe needs the following information to set up your destination:

* Considering *all* the fields that your platform can receive, Adobe needs the standard JSON schema that corresponds to your expected message format. Having the template allows Adobe to define transformations and to create a custom XDM schema for your company, which customers would use to export data to your destination.

-->
