---
title: カスタム条件の管理方法
description: チュートリアルのこの部分では、Adobe Target API を使用してAdobe Target Recommendationsの条件を管理、作成、リスト、編集、取得、削除するために必要な手順を開発者にガイドします。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration
doc-type: tutorial
kt: 3815
author: Judy Kim
exl-id: ee63bd3e-200a-4c08-b364-9f17a479033b
source-git-commit: cee2618bb92284da1f82d108a0aff0d39340a15b
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 1%

---

# カスタム条件の管理

時には [!DNL Recommendations] は、プロモーションする特定の項目を表示できません。 このような場合、カスタム条件を使用すると、特定のキー品目またはカテゴリに対して、レコメンデーション品目の特定のセットを配信できます。 主要品目またはカテゴリと推奨品目の間のマッピングを定義し、そのマッピングをカスタム条件としてインポートします。 このプロセスについては、 [カスタム条件ドキュメント](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/recommendations-csv.html?lang=en). このドキュメントで述べたように、カスタム条件の作成、編集、削除には、 [!DNL Target] ユーザーインターフェイス (UI) しかし、 [!DNL Target] また、には、カスタム条件をより詳細に管理するための一連のカスタム条件 API が用意されています。

>[!IMPORTANT]
>
>カスタム条件では、次の使用ガイドラインに従います。
>
> API を使用して、特定のカスタム条件に対してすべての操作（作成、編集、削除）をおこなうか、UI を使用してすべての操作（作成、編集、削除）をおこないます。 UI と API を組み合わせてカスタム条件を管理すると、情報が競合したり、予期しない結果が生じる場合があります。 例えば、UI でカスタム条件を作成し、API で編集した場合、API で表示されるように、バックエンドで更新される場合でも、UI での更新は反映されません。

## カスタム条件の作成

を使用してカスタム条件を作成するには [カスタム条件 API の作成](https://developers.adobetarget.com/api/recommendations/#operation/createCriteriaCustom)の場合の構文は次のとおりです。

`POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom`

>[!WARNING]
>
>カスタム条件の作成 API を使用して作成したカスタム条件は、この演習で説明するように、UI に表示され、そこで保持されます。 UI で編集や削除を行うことはできません。 編集または削除できます **API を使用**&#x200B;しかし、どちらの方法でも、引き続き [!DNL Target] UI UI からの編集または削除のオプションを維持するには、次の手順に従って UI を使用してカスタム条件を作成します。 [ドキュメント](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/recommendations-csv.html?lang=en)（カスタム条件の作成 API を使用するのとは異なります）。

上記の警告を読んだ後で、このチュートリアルを続行し、UI から削除できない新しいカスタム条件を作成することをお勧めします。

1. 検証 `TENANT_ID` および `API_KEY` 対象 **カスタム条件の作成** 前に確立したPostman環境変数を参照します。 比較には、以下の画像を使用します。

   ![CreateCustomCriteria1](assets/CreateCustomCriteria1.png)

2. を **本文** as **raw** カスタム条件の CSV ファイルの場所を定義する JSON。 例を [カスタム条件 API の作成](https://developers.adobetarget.com/api/recommendations/#operation/getAllCriteriaCustom) テンプレートとしてのドキュメント ( `environmentId` および必要に応じてその他の値 この例では、キーとして LAST_PURCHASED を使用します。

   ![CreateCustomCriteria2](assets/CreateCustomCriteria2.png)

3. リクエストを送信し、作成したカスタム条件の詳細を含む応答を観察します。

   ![CreateCustomCriteria3](assets/CreateCustomCriteria3.png)

4. カスタム条件が作成されたことを確認するには、Adobe Target内で、 **[!UICONTROL Recommendations] > [!UICONTROL 条件]** 名前で条件を検索するか、 **カスタム条件 API のリスト** 次の手順で使用します。

   ![CreateCustomCriteria4](assets/CreateCustomCriteria4.png)

この場合、エラーが発生します。 カスタム条件をより詳細に調べ、 **カスタム条件 API のリスト**.

## カスタム条件のリスト

すべてのカスタム条件のリストと各条件の詳細を取得するには、 [カスタム条件 API のリスト](https://developers.adobetarget.com/api/recommendations/#operation/getAllCriteriaCustom). 構文は次のとおりです。

`GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom`

1. 検証 `TENANT_ID` および `API_KEY` 前と同様に、リクエストを送信します。 応答では、カスタム条件 ID と、前述のエラーメッセージに関する詳細をメモします。
   ![ListCustomCriteria](assets/ListCustomCriteria.png)

この場合、エラーは、サーバー情報が正しくないために発生しました。これは、 [!DNL Target] は、カスタム条件定義を含む CSV ファイルにアクセスできません。 カスタム条件を編集して修正しましょう。

## カスタム条件の編集

カスタム条件定義の詳細を変更するには、 [カスタム条件 API の編集](https://developers.adobetarget.com/api/recommendations/#operation/updateCriteriaCustom). 構文は次のとおりです。

`POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. 検証 `TENANT_ID` および `API_KEY`前と同様に。
   ![EditCustomCriteria1](assets/EditCustomCriteria1.png)

1. 編集する（単一の）カスタム条件の条件 ID を指定します。
   ![EditCustomCriteria2](assets/EditCustomCriteria2.png)

1. 本文で、更新された JSON に正しいサーバー情報を入力します。 （この手順では、アクセス可能なサーバーへの FTP アクセスを指定します）。
   ![EditCustomCriteria3](assets/EditCustomCriteria3.png)

1. リクエストを送信し、応答をメモします。
   ![EditCustomCriteria4](assets/EditCustomCriteria4.png)

次に、 **カスタム条件 API の取得**.

## カスタム条件の取得

特定のカスタム条件のカスタム条件の詳細を表示するには、 [カスタム条件 API の取得](https://developers.adobetarget.com/api/recommendations/#operation/getCriteriaCustom). 構文は次のとおりです。

`GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. 詳細を取得するカスタム条件の条件 ID を指定します。 リクエストを送信し、応答を確認します。
   ![GetCustomCriteria.png](assets/GetCustomCriteria.png)
1. 検証が成功しました。 （この場合は、それ以上 FTP エラーがないことを確認します）。
   ![GetCustomCriteria1.png](assets/GetCustomCriteria1.png)
1. （オプション）UI に更新が正確に反映されていることを確認します。
   ![GetCustomCriteria2.png](assets/GetCustomCriteria2.png)

## カスタム条件の削除

前述の条件 ID を使用して、 [カスタム条件 API の削除](https://developers.adobetarget.com/api/recommendations/#operation/deleteCriteriaCustom). 構文は次のとおりです。

`DELETE https://mc.adobe.io/{{TENANT_ID}}/target/recs/criteria/custom/:criteriaId`

1. 削除する（単一の）カスタム条件の条件 ID を指定します。 「**送信**」をクリックします。
   ![DeleteCustomCriteria1](assets/DeleteCustomCriteria1.png)

1. カスタム条件の取得を使用して、条件が削除されたことを確認します。
   ![DeleteCustomCriteria2](assets/DeleteCustomCriteria2.png)
この場合、404 エラーが発生すると、削除された条件が見つからないことが示されます。

>[!NOTE]
>なお、この条件は、 [!DNL Target] UI は削除されましたが、カスタム条件の作成 API を使用して作成されたので削除されました。

おめでとうございます。 を使用して、カスタム条件の作成、リスト、編集、削除、詳細の取得をおこなうことができるようになりました。 [!DNL Recommendations] API 次の節では、 [!DNL Target] レコメンデーションを取得するための Delivery API。

[次：「サーバー側配信 API を使用したRecommendationsの取得」>](https://developer.adobe.com/target/before-administer/recs-api/fetch-recs-server-side-delivery-api/){target=&quot;_blank&quot;}
