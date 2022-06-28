---
title: API を使用したRecommendationsカタログの管理方法
description: チュートリアルのこの部分では、Adobe Target API を使用してRecommendationsカタログ内のエンティティを作成、更新、保存、取得、削除するために必要な手順を、開発者にガイドします。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration
doc-type: tutorial
kt: 3815
author: Judy Kim
exl-id: 8060b69b-e8e5-4fe7-895f-742410d8ed45
source-git-commit: 0ecfde208b3e201de135512d5aab70192fc2b826
workflow-type: tm+mt
source-wordcount: '918'
ht-degree: 2%

---

# を [!DNL Recommendations] API を使用したカタログ

ここでは、JWT 認証フローを使用してアクセストークンを生成し、Adobe Target Admin API をAdobe I/Oで使用する方法を学びました。

以下を使用して、 [Recommendations API](https://developers.adobetarget.com/api/recommendations/) recommendations カタログの項目を追加、更新、削除する場合。 残りのAdobe Target Admin API と同様、 [!DNL Recommendations] API には認証が必要です。

>[!TIP]
>
>を **[!UICONTROL IMS:JWT 生成+ユーザートークンを介した認証]** 認証用にアクセストークンを更新する必要が生じたときにリクエストを送信します。リクエストの有効期限は 24 時間後に切れるからです。 詳しくは、 [AdobeAPI 認証の設定](https://developer.adobe.com/target/before-administer/configure-authentication/){target=_blank} を参照してください。

![JWT3ff](assets/configure-io-target-jwt3ff.png)

>[!NOTE]
>
>先に進む前に、 [Recommendations Postmanコレクション](https://developers.adobetarget.com/api/recommendations/#section/Postman).

## エンティティ保存 API での項目の作成と更新

次の手順で [!DNL Recommendations] CSV 製品フィードや [!DNL Target] 製品ページで実行されるリクエストには、 [エンティティ保存 API](https://developers.adobetarget.com/api/recommendations/#operation/saveEntities). このリクエストは、単一の [!DNL Target] 環境。 構文は次のとおりです。

```
POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities
```

たとえば、「保存エンティティ」を使用すると、在庫や価格のしきい値など、特定のしきい値を満たした場合にアイテムを更新し、それらのアイテムにフラグを設定してレコメンデーションを防ぐことができます。

1. に移動します。 **[!DNL Target]> [!UICONTROL 設定] > [!UICONTROL ホスト] > [!UICONTROL 環境]** を取得する [!DNL Target] 項目を追加または更新する環境 ID。

   ![SaveEntities1](assets/SaveEntities01.png)

2. 検証 `TENANT_ID` および `API_KEY` 前に確立したPostman環境変数を参照します。 比較には、以下の画像を使用します。 必要に応じて、API リクエストのヘッダーとパスを、以下の画像のヘッダーと一致するように変更します。

   ![SaveEntities3](assets/SaveEntities03.png)

3. JSON に次のように入力します。 **raw** コード **本文**. 忘れずに、 `environment` 変数を使用します。 （以下の例では、環境 ID は 6781 です）。

   ![SaveEntities4.png](assets/SaveEntities04.png)

   >!![NOTE]
   以下に、Toaster Oven 製品の関連エンティティ値を含む entity.id kit2001 を環境 6781 に追加する JSON のサンプルを示します。

   ```
      {
      "entities": [{
              "name": "Toaster Oven",
              "id": "kit2001",
              "environment": 6781,
              "categories": [
                  "housewares:appliances"
              ],
              "attributes": {
                  "inventory": 77,
                  "margin": 23,
                  "message": "crashing helicopter",
                  "pageUrl": "www.foobar.foo.com/helicopter.html",
                  "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                  "value": 19.2
              }
          }]
      }
   ```

4. 「**送信**」をクリックします。次の応答を受け取ります。

   ![SaveEntities5.png](assets/SaveEntities05.png)

JSON オブジェクトは、複数の製品を送信するように拡大・縮小できます。 例えば、この JSON は 2 つのエンティティを指定します。

```
    {
        "entities": [{
                "name": "Toaster Oven",
                "id": "kit2001",
                "environment": 6781,
                "categories": [
                    "housewares:appliances"
                ],
                "attributes": {
                    "inventory": 89,
                    "margin": 11,
                    "message": "Toaster Oven",
                    "pageUrl": "www.foobar.foo.com/helicopter.html",
                    "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                    "value": 102.5
                }
            },
            {
                "name": "Blender",
                "id": "kit2002",
                "environment": 6781,
                "categories": [
                    "housewares:appliances"
                ],
                "attributes": {
                    "inventory": 36,
                    "margin": 5,
                    "message": "Blender",
                    "pageUrl": "www.foobar.foo.com/helicopter.html",
                    "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                    "value": 54.5
                }
            }
        ]
    }
```

1. 今度は君の番だ！ 以下を使用： **エンティティを保存** 次の項目をカタログに追加する API。 上記のサンプル JSON を出発点として使用してください。 （追加のエンティティを含めるには、JSON を拡張する必要があります）。

   ![SaveEntities6.png](assets/SaveEntities06.png)

おぉ～最後の 2 つのアイテムは属してないみたい。 では、 **エンティティを取得** API を削除し、必要に応じて、 **エンティティを削除** API

## Get Entity API を使用した品目の詳細の取得

既存の項目の詳細を取得するには、 [エンティティ API を取得](https://developers.adobetarget.com/api/recommendations/#operation/getEntity). 構文は次のとおりです。

```
GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities/[entity.id]
```

エンティティの詳細は、一度に 1 つのエンティティに対してのみ取得できます。 「エンティティを取得」を使用して、カタログで期待どおりに更新がおこなわれたことを確認したり、カタログの内容を監査したりできます。

1. API リクエストで、変数を使用してエンティティ ID を指定します。 `entityId`. 次の例では、entityId=kit2004 のエンティティの詳細が返されます。

   ![GetEntity1](assets/GetEntity1.png)

2. 検証 `TENANT_ID` および `API_KEY` 前に確立したPostman環境変数を参照します。 比較には、以下の画像を使用します。 必要に応じて、API リクエストのヘッダーとパスを、以下の画像のヘッダーと一致するように変更します。

   ![GetEntity2](assets/GetEntity2.png)

3. リクエストを送信します。

   ![GetEntity3](assets/GetEntity3.png)
エンティティが見つからなかったことを示すエラーが表示された場合は、上の例に示すように、リクエストを正しい [!DNL Target] 環境。

   >[!NOTE]
   環境が明示的に指定されていない場合、Get Entity は、 [デフォルト環境](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html?lang=en) のみ。 デフォルト環境以外の環境から取り込む場合は、環境 ID を指定する必要があります。

4. 必要に応じて、 `environmentId` パラメーターを使用し、リクエストを再送信します。

   ![GetEntity4](assets/GetEntity4.png)

5. 別のものを送信 **エンティティを取得** リクエストを送信する際に、この時点で entityId=kit2005 のエンティティを調べます。

   ![GetEntity5](assets/GetEntity5.png)

これらのエンティティをカタログから削除する必要があると判断したとします。 次を使用します。 **エンティティを削除** API

## エンティティ削除 API での品目の削除

カタログから項目を削除するには、 [エンティティ削除 API](https://developers.adobetarget.com/api/recommendations/#operation/deleteEntities). 構文は次のとおりです。

```
DELETE https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities?ids=[comma-delimited-entity-ids]&environment=[environmentId]
```

>[!WARNING]
この API は、指定した ID で参照されているエンティティを削除します。
エンティティ ID を指定しない場合、指定した環境内のすべてのエンティティが削除されます。 環境 ID が指定されていない場合、エンティティはすべての環境から削除されます。 この機能は慎重に使用してください。

1. に移動します。 **[!DNL Target]> [!UICONTROL 設定] > [!UICONTROL ホスト] > [!UICONTROL 環境]** を取得する [!DNL Target] 項目を削除する環境 ID。

   ![DeleteEntities1](assets/SaveEntities01.png)

2. API リクエストで、構文を使用して、削除するエンティティのエンティティ ID を指定します `&ids=[comma-delimited-entity-ids]` （クエリパラメーター）。 複数のエンティティを削除する場合は、コンマで区切ります。

   ![DeleteEntities2](assets/DeleteEntities2.png)

3. 構文を使用して環境 ID を指定します `&environment=[environmentId]`そうしない場合、すべての環境のエンティティが削除されます。

   ![DeleteEntities3](assets/DeleteEntities3.png)

4. 検証 `TENANT_ID` および `API_KEY` 前に確立したPostman環境変数を参照します。 比較には、以下の画像を使用します。 必要に応じて、API リクエストのヘッダーとパスを、以下の画像のヘッダーと一致するように変更します。

   ![DeleteEntities4](assets/DeleteEntities4.png)

5. リクエストを送信します。

   ![DeleteEntities5](assets/DeleteEntities5.png)

6. 次を使用して結果を検証： **エンティティを取得**」と表示され、削除されたエンティティが見つからないことを示すようになりました。

   ![DeleteEntities6](assets/DeleteEntities6.png)

   ![DeleteEntities6](assets/DeleteEntities7.png)

おめでとう！ これで、 [!DNL Recommendations] カタログ内のエンティティの詳細を作成、更新、削除、取得するための API。 次の節では、カスタム条件の管理方法について説明します。

[次：「カスタム条件の管理」>](https://developer.adobe.com/target/before-administer/recs-api/manage-custom-criteria/){target=_blank}
