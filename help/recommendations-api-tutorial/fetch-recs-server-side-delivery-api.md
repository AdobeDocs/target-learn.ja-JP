---
title: Delivery APIでRecommendationsを取得する方法
description: チュートリアルのこの部分では、Adobe Target Delivery APIを使用してRecommendationsコンテンツを取得するために必要な手順を開発者向けに説明します。
role: Developer
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Recommendations, Administration & Configuration
doc-type: tutorial
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 553d1208-647f-479d-acc7-d7760469d642
source-git-commit: a6b645b6d9693a4c8882fd47ee0d61698c0b834d
workflow-type: tm+mt
source-wordcount: '1418'
ht-degree: 2%

---

# 配信APIを使用した[!DNL Recommendations]の取得

Adobe TargetとAdobe Targetの[!DNL Recommendations] APIを使用してWebページに応答を配信できますが、アプリ、画面、コンソール、Eメール、キオスク、その他のディスプレイデバイスを含む、HTML以外のエクスペリエンスでも使用できます。 つまり、[!DNL Target]ライブラリとJavaScriptを使用できない場合でも、**[!DNL Target]Delivery API**&#x200B;を使用すれば、幅広い[!DNL Target]機能にアクセスしてパーソナライズされたエクスペリエンスを提供できます。

>[!NOTE]
>
> 実際のレコメンデーション（推奨商品または品目）を含むコンテンツをリクエストする場合は、 [!DNL Target] Delivery APIを使用します。

レコメンデーションを取得するには、Adobe Target Delivery APIのPOST呼び出しを適切なコンテキスト情報と共に送信します。これには、ユーザーID（最近表示された品目などのプロファイル固有のレコメンデーションで使用）、関連するmbox名、mboxパラメーター、プロファイルパラメーター、その他の属性が含まれます。 応答には、推奨されるentity.idが（他のエンティティデータも含めることができます）JSON形式またはHTML形式で含まれ、デバイスに表示できます。

Adobe Targetの[Delivery API](https://developers.adobetarget.com/api/delivery-api/)は、標準の[!DNL Target]リクエストが提供する既存の機能をすべて公開します。

>[!NOTE]
>Delivery APIは次のとおりです。
>* 場所とオーディエンスのエクスペリエンスまたはオファーをRESTfulな方法で取得できます。
>* 認証は不要です。
>* POSTのみ。
>* Cookieやリダイレクトの呼び出しを処理しません。
>* 「ユーザーの役割」を必要としない、または認識しない。 単にコンテンツを取得したり、[!DNL Target]エッジサーバーにイベントを報告したりします。


Delivery APIを使用して、レコメンデーションを含む[!DNL Target]エクスペリエンスを配信するには、次の手順に従います。

1. （Visual Experience Composerではなく）フォームベースのコンポーザーを使用して、[!DNL Target]アクティビティ（A/B、XT、AP、または[!DNL Recommendations]）を作成します。
2. Delivery APIを使用して、先ほど作成した[!DNL Target]アクティビティによって生成されたリクエストの応答を取得します。

<!-- Q: Why are BOTH steps necessary for this? If you have a Form-based recommendation defined for an mbox, what's the point/benefit of ALSO having the Delivery API step in to retrieve results? Why can't you just have the Form-based Rec deliver the results in the destination device...?? A: See use case below... it's when you want to "intercept" the pending results in order to do more stuff prior to displaying the results. Things like real-time comparisons to inventory levels. -->

## フォームベースのExperience Composerを使用したレコメンデーションの作成

Delivery APIで使用できるレコメンデーションを作成するには、[フォームベースのコンポーザー](https://docs.adobe.com/content/help/en/target/using/experiences/form-experience-composer.html)を使用します。

1. まず、レコメンデーションで使用するJSONベースのデザインを作成して保存します。 サンプルJSON、およびフォームベースのアクティビティの設定時にJSON応答を返す方法に関する背景情報については、[Recommendationデザインの作成](https://docs.adobe.com/content/help/en/target/using/recommendations/recommendations-design/create-design.html)に関するドキュメントを参照してください。 この例では、デザインの名前は&#x200B;*Simple JSON.*です。

   ![server-side-create-recs-json-design.png](assets/server-side-create-recs-json-design.png)

2. [!DNL Target]で、**[!UICONTROL アクティビティ] / [!UICONTROL アクティビティを作成] / [!UICONTROL Recommendations]**&#x200B;に移動し、「**[!UICONTROL フォーム]**」を選択します。

   ![server-side-create-recs.png](assets/server-side-create-recs.png)

3. プロパティを選択し、「**[!UICONTROL 次へ]**」をクリックします。
4. レコメンデーションの応答を受け取る場所を定義します。 次の例では、*api_charter*&#x200B;という名前の場所を使用しています。 先ほど作成したJSONベースのデザインを、*Simple JSON.*という名前で選択します。
   ![server-side-create-recs-form.png](assets/server-side-create-recs-form1.png)
5. レコメンデーションを保存してアクティブ化します。 結果が生成されます。 [結果の準備が整ったら](https://docs.adobe.com/content/help/en/target/using/recommendations/recommendations-activity/previewing-and-launching-your-recommendations-activity.html)、Delivery APIを使用して結果を取得できます。

## Delivery APIの使用

[Delivery API](https://developers.adobetarget.com/api/delivery-api/#tag/Delivery-API)の構文は次のとおりです。

`POST https://{{CLIENT_CODE}}.tt.omtrdc.net/rest/v1/delivery`

1. クライアントコードは必須です。 クライアントコードは、**[!UICONTROL Recommendations] /[!UICONTROL 設定]**&#x200B;に移動するとAdobe Targetに表示される場合があります。 **[!UICONTROL Recommendation APIトークン]**&#x200B;セクションの&#x200B;**[!UICONTROL クライアントコード]**の値に注意してください。
   ![client-code.png](assets/client-code.png)
1. クライアントコードを入手したら、Delivery API呼び出しを作成します。 次の例は、[Delivery API Postmanコレクション](https://developers.adobetarget.com/api/delivery-api/#section/Getting-Started/Postman-Collection)で提供される&#x200B;**[!UICONTROL Webバッチmbox Delivery API Call]**&#x200B;で始まり、関連する変更をおこないます。 次に例を示します。
   * **browser**&#x200B;と&#x200B;**address**&#x200B;オブジェクトは、HTML以外の使用例には必要ないので、 **Body**&#x200B;から削除されました。
   * *この例で* は、ロケーション名としてapi_charterが表示されます。
   * entity.idを指定します。これは、このレコメンデーションがコンテンツの類似性に基づいているからです。類似性には、現在の項目キーを[!DNL Target]に渡す必要があります。
      ![server-side-delivery-API-call.pngクエリパラメー](assets/server-side-delivery-api-call2.png)
ターを正しく設定するようにしてください。例えば、必ず 
`{{CLIENT_CODE}}`必要に応じて。<!--Q: In the updated call syntax, entity.id is listed as a profileParameter instead of an mboxParameter as in older versions. --> <!--Q: Old image ![server-side-create-recs-post.png](assets/server-side-create-recs-post.png) Old accompanying text: "Note this recommendation is based on Content Similar products based on the entity.id sent via mboxParameters." -->
      ![client-code3](assets/client-code3.png)
1. リクエストを送信します。 これは、*api_charter*&#x200B;の場所に対して実行されます。この場所では、アクティブなレコメンデーションが実行され、JSONデザインで定義され、推奨エンティティのリストが出力されます。
1. JSONデザインに基づいて応答を受け取ります。
   ![server-side-create-recs-json-response2.](assets/server-side-create-recs-json-response2.png)
png応答には、キーIDと、推奨エンティティのエンティティIDが含まれます。

この方法でDelivery APIを[!DNL Recommendations]と共に使用すると、HTML以外のデバイスで訪問者にレコメンデーションを表示する前に、追加の手順を実行できます。 例えば、Delivery APIからの応答を受け取って、最終結果を表示する前に、別のシステム（CMS、PIM、eコマースプラットフォームなど）からエンティティ属性の詳細（在庫、価格、評価など）をリアルタイムで追加検索できます。

このチュートリアルで概要を説明しているアプローチを使用すると、[!DNL Target]からの応答を活用してパーソナライズされたレコメンデーションを提供する任意のアプリケーションを入手できます。

## 実装例

以下のリソースは、HTMLに焦点を当てていない様々な実装の例を示しています。 システムとデバイスが関係するので、すべての実装は一意であることに注意してください。

| リソース | 詳細 |
| --- | --- |
| [Adobe Target Everywhere - IoTでサーバ側またはを実装](https://expleague.azureedge.net/labs/L733/index.html) | Adobe Targetのサーバー側APIを利用するReactアプリケーションに関する実践的な体験を提供するAdobe Summit2019 Lab。 |
| [AdobeSDKを使用しないモバイルアプリでのAdobe Target](https://community.tealiumiq.com/t5/Universal-Data-Hub/Adobe-Target-in-a-Mobile-App-Without-the-Adobe-SDK/ta-p/26753) | このガイドでは、AdobeSDKをインストールせずにモバイルアプリでAdobe Targetを設定する方法を説明します。 このソリューションでは、 Tealium SDKのWebビューとRemote Commandsモジュールを使用して、Adobe訪問者API(Experience Cloud)およびAdobe Target APIにリクエストを送受信します。 |
| [モバイルアプリにおけるAdobe Targetの仕組み](https://docs.adobe.com/content/help/en/target/using/implement-target/mobile-apps/mobile-how-target-works-mobile-apps.html) | [!DNL Target]がMobile SDKでどのように動作するか |
| [APIの [!DNL Target] extension in Experience Platform Launch and Implementing [!DNL Target] 設定](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/adobe-target) | Experience Platform Launchで[!DNL Target]拡張機能を設定し、アプリに[!DNL Target]拡張機能を追加し、[!DNL Target] APIを実装してアクティビティをリクエストし、オファーをプリフェッチし、ビジュアルプレビューモードを開始する手順。 |
| [Adobe Target Node Client](https://www.npmjs.com/package/@adobe/target-nodejs-sdk) | オープンソースの[!DNL Target] Node.js SDK v1.0 |
| [サーバー側の概要](https://docs.adobe.com/content/help/en/target/using/implement-target/server-side/api-and-sdk-overview.html) | Adobe Target Server Side Delivery API、Server Side Batch Delivery API、Node.js SDK、Adobe Target [!DNL Recommendations] APIに関する情報です。 |
| [Adobe Campaign Content Recommendations in Email](https://medium.com/adobetech/adobe-campaign-content-recommendations-in-email-b51ced771d7f) | Adobe CampaignのAdobe TargetとAdobe I/O Runtimeを介してEメールでコンテンツのレコメンデーションを活用する方法を説明するブログ。 |

## APIを使用した[!DNL Recommendations]設定の管理

ほとんどの場合、レコメンデーションはAdobe Target UIで設定され、前述の節で説明したような理由で、 [!DNL Target] APIを介して使用またはアクセスされます。 このUIとAPIの調整は一般的です。 ただし、セットアップと結果の使用の両方のAPIを介してすべてのアクションを実行したい場合があります。 一般的ではありませんが、ユーザーは絶対に設定、実行、*および*&#x200B;を使用して、レコメンデーションの結果を完全にAPIを使用して活用できます。

Adobe Target Recommendationsエンティティを管理し、サーバー側で配信する方法について、[前の節](manage-catalog.md)で学びました。 同様に、Adobe I/Oを使用すると、Adobe Targetにログインしなくても条件、プロモーション、コレクションおよびデザインテンプレートを管理できます。 すべての[!DNL Recommendations] APIの完全なリストは、[ここ](http://developers.adobetarget.com/api/recommendations/)にありますが、参照用の要約を示します。

| リソース | 詳細 |
| --- | --- |
| [コレクション](http://developers.adobetarget.com/api/recommendations/#tag/Collections) | コレクションのリスト、作成、取得、編集および削除を行います。 |
| [条件](http://developers.adobetarget.com/api/recommendations/#tag/Criteria) | 条件のリストと取得。 |
| [デザイン](http://developers.adobetarget.com/api/recommendations/#tag/Designs) | デザインのリスト、作成、取得、編集、削除および検証を行います。 |
| [エンティティ](http://developers.adobetarget.com/api/recommendations/#tag/Entities) | エンティティを保存、削除、取得します。 |
| [プロモーション](http://developers.adobetarget.com/api/recommendations/#tag/Promotions) | プロモーションのリスト、作成、取得、編集、削除を行います。 |
| [カテゴリ条件](http://developers.adobetarget.com/api/recommendations/#tag/Category-Criteria) | カテゴリ条件のリスト、作成、取得、編集、削除を行います。 |
| [カスタム条件](http://developers.adobetarget.com/api/recommendations/#tag/Custom-Criteria) | カスタム条件をリスト、作成、取得、編集および削除します。 |
| [品目基準](http://developers.adobetarget.com/api/recommendations/#tag/Item-Criteria) | 項目基準のリスト、作成、取得、編集、削除を行います。 |
| [人気度条件](http://developers.adobetarget.com/api/recommendations/#tag/Popularity-Criteria) | 人気度条件のリスト、作成、取得、編集および削除。 |
| [プロファイル属性条件](http://developers.adobetarget.com/api/recommendations/#tag/Profile-Attribute-Criteria) | プロファイル属性条件のリスト、作成、取得、編集、削除を行います。 |
| [最近の条件](http://developers.adobetarget.com/api/recommendations/#tag/Recent-Criteria) | 最近使用した条件のリスト、作成、取得、編集、削除を行います。 |
| [シーケンス条件](http://developers.adobetarget.com/api/recommendations/#tag/Sequence-Criteria) | シーケンス条件のリスト、作成、取得、編集および削除を行います。 |

## リファレンスドキュメント

* [Adobe Target APIドキュメント](https://developers.adobetarget.com/api/#getting-started)
* [Adobe Target Delivery API](https://developers.adobetarget.com/api/delivery-api/)
* [ [!DNL Recommendations] Eメールとの統合](https://docs.adobe.com/content/help/en/target/using/recommendations/recommendations-faq/integrating-recs-email.html)

## 概要とレビュー

おめでとう！ このチュートリアルを終了して、次の方法を学習しました。
* [Recommendations APIを使用したカタログの管理](manage-catalog.md)
* [Recommendations APIを使用したカスタム条件の管理](manage-custom-criteria.md)
* [RecommendationsでのDelivery APIの使用](fetch-recs-server-side-delivery-api.md)
