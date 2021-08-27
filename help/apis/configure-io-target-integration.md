---
title: Adobe Target APIの認証を設定する方法
description: このチュートリアルでは、Adobe Target APIとのやり取りに必要な認証トークンを生成するために必要な手順を開発者に説明します。 以下の手順に従って、Adobe開発者コンソールを使用して、Target APIの使用に必要なbearerアクセストークンを生成し、テストします。
role: Developer, Admin, Architect
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Administration & Configuration
doc-type: tutorial
kt: null
thumbnail: null
author: Judy Kim
exl-id: 8a1e93e4-67b2-4942-a8da-fc0f2cbb2df2
source-git-commit: d1517f0763290eb61a9e4eef4f2eb215a9cdd667
workflow-type: tm+mt
source-wordcount: '1883'
ht-degree: 2%

---

# Adobe Target APIの認証の設定

[!DNL Recommendations] Admin APIを含むAdobe Target Admin APIは、認証によって保護され、許可されたユーザーのみがAdobe Targetにアクセスするようにします。 [Adobe開発者コンソール](https://console.adobe.io/)を使用して、[!DNL Target]を含むすべてのAdobe Experience Cloudソリューションでこの認証を管理します。

このレッスンでは、Adobe Target APIとのやり取りに必要な認証トークンを生成するために必要な事前の手順について説明します。 以降の節では、以下をおこないます。

1. プロジェクト開発者コンソールでプロジェクト(旧称「Adobe」)を作成します。
2. プロジェクトの詳細をPostmanに書き出します。
3. bearerアクセストークンを生成します。
4. bearerアクセストークンをテストします。

## 前提条件

| リソース | 詳細 |
| --- | --- |
| 郵便配達人 | これらの手順を正しく完了するには、お使いのオペレーティングシステム用の[Postmanアプリ](https://www.postman.com/downloads/)を入手します。 Postman basicは、アカウントの作成を無料で行います。 Adobe Target APIを一般的に使用するために必須ではありませんが、PostmanはAPIワークフローを容易にします。Adobe Targetは、APIの実行とその動作の学習に役立つ、複数のPostmanコレクションを提供します。 このチュートリアルの残りの部分は、Postmanに関する実務知識を前提としています。 サポートが必要な場合は、[Postmanのドキュメント](https://learning.getpostman.com/)を参照してください。 |
| 参照 | このチュートリアルの残りの部分では、次のリソースに関する知識が前提となります。<UL><li>[Adobe I/OGithub](https://github.com/adobeio)</li><li>[TargetAdobe I/Oのドキュメント](https://developers.adobetarget.com/api/#introduction)</li><li>[Recommendations APIドキュメント](https://developers.adobetarget.com/api/recommendations/)</li></ul> |

## Adobe I/Oプロジェクト

この節では、Adobe開発者コンソールにアクセスし、[!DNL Adobe Target]のプロジェクトを作成します。 詳しくは、[プロジェクトのドキュメント](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)を参照してください。

<!--1. Generate your private key and public certificate, per the [documentation on authentication](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/JWTCertificate.md). //<!--as described in **Step 1** of [How to set up Adobe IO: Authentication - Step by Step](https://helpx.adobe.com/marketing-cloud-core/kb/adobe-io-authentication-step-by-step.html). After completing Step 1, return to this tutorial and resume with Step 2, below. // The outcome of this step should be the creation of a `private.key` file and a `certificate_pub.crt` file. Return to this tutorial once you have generated these two files.-->

1. [Adobe Admin Console](https://adminconsole.adobe.com/)で、Adobeユーザーアカウントに[製品管理者](https://helpx.adobe.com/enterprise/using/admin-roles.html)と[開発者](https://helpx.adobe.com/enterprise/using/manage-developers.html)の両方のレベルで[!DNL Target]へのアクセス権が付与されていることを確認します。

2. [Adobe開発者コンソール](https://console.adobe.io/)で、この統合を作成するExperience Cloud組織を選択します。 (1つのExperience Cloud組織にのみアクセスできる可能性があります)。

   ![configure-io-target-createproject2.png](assets/configure-io-target-createproject2.png)

3. 「**[!UICONTROL 新しいプロジェクトを作成]**」をクリックします。

   ![configure-io-target-createproject3.png](assets/configure-io-target-createproject3.png)

4. 「**[!UICONTROL APIを追加]**」をクリックして、Adobe サービスと製品にアクセスするためのREST APIをプロジェクトに追加します。

   ![APIを追加](assets/configure-io-target-createproject4.png)

5. 統合先のAdobe サービスとして&#x200B;**[!DNL Adobe Target]**&#x200B;を選択します。 表示される「**[!UICONTROL 次へ]**」ボタンをクリックします。

   ![configure-io-target-createproject5](assets/configure-io-target-createproject5.png)

6. 公開鍵と秘密鍵を、Target用に作成するサービスアカウント統合に関連付けるオプションを選択します。 このチュートリアルでは、**[!UICONTROL オプション1を選択します。キーペア]**&#x200B;を生成し、**[!UICONTROL キーペアを生成]**をクリックします。
   ![configure-io-target-createproject6](assets/configure-io-target-createproject6.png)

7. 結果をメモしておきます。 指示に従って、自動的にダウンロードされた設定ファイル(`config`)をメモします。このファイルには、秘密鍵が含まれています。 「**[!UICONTROL 次へ]**」をクリックします。
   ![configure-io-target-createproject7](assets/configure-io-target-createproject7.png)
8. ファイルシステムで、前の手順で作成した圧縮設定ファイルの`config`の場所を確認します。 この`config`ファイルには秘密鍵が含まれていますが、後で必要になります。 ファイルシステム内の正確な場所は、次に示す場所とは異なる場合があります。
   ![configure-io-target-createproject8](assets/configure-io-target-createproject8.png)
9. Adobe開発者コンソールに戻り、[!DNL Recommendations]を使用しているプロパティに対応する[製品プロファイル](https://helpx.adobe.com/enterprise/using/manage-products-and-profiles.html)を選択します。 （プロパティを使用しない場合は、「デフォルトのワークスペース」オプションを選択します）。 「**[!UICONTROL 設定済みAPIを保存]**」をクリックします。
   ![configure-io-target-createproject9](assets/configure-io-target-createproject9.png)

10. 「**[!UICONTROL 統合を作成]**」をクリックします。 APIが正常に設定されたことを示す一時的なメッセージが表示されます。

11. 最後の手順として、プロジェクトの名前を元の`Project 1`よりも意味のある名前に変更します。 それには、図のようにナビゲーションパスを使用してプロジェクトに移動し、「**[!UICONTROL プロジェクトを編集]**」をクリックして「**[!UICONTROL プロジェクトを編集]」モーダルにアクセスし、プロジェクトの名前を変更します。

![configure-io-target-createproject11](assets/configure-io-target-createproject11.png)

>[!NOTE]
> 
>このチュートリアルでは、プロジェクトに「Target統合」という名前を付けます。 Adobe Target以外の用途でプロジェクトを使用することを想定している場合は、それに応じて名前を付けることができます。 例えば、Adobe Experience Cloudの他のソリューションで使用できるので、「AdobeAPI」または「Experience CloudAPI」という名前を選択できます。

## プロジェクトの詳細の書き出し

これで、[!DNL Target]へのアクセスに使用できるAdobeプロジェクトが作成されたので、そのプロジェクトの詳細をAdobeAPIリクエストと共に送信する必要があります。 これらの詳細は、複数の[!DNL Target] APIを含む複数のAdobeAPIとやり取りするために必要です。 例えば、統合の詳細には、[!DNL Target] Admin APIで必要な認証情報と認証情報が含まれます。 したがって、PostmanでAPIを使用するには、これらの詳細をPostmanに取り込む必要があります。

Postmanでプロジェクトの詳細を指定する方法は多数ありますが、この節では、事前に作成された機能とコレクションを利用します。 まず、（この節の）統合の詳細をPostman環境に書き出します。 次に（次の節で）、必要なAdobeリソースへのアクセスを許可するbearerアクセストークンを生成します。

>[!NOTE]
>
>[!DNL Target]を含む、あらゆるExperience Cloudソリューションに適用できるビデオ手順については、 [PostmanとExperience PlatformAPIの使用](https://experienceleague.adobe.com/docs/platform-learn/tutorials/platform-api-authentication.html?lang=en)を参照してください。 以下の節は、[!DNL Target] APIに関連しています。
>
> 1. PostmanへのAdobe I/O統合の詳細の書き出し
> 2. Postmanでのアクセストークンの生成

>
> これらの手順も以下に示します。

1. 引き続き[Adobeデベロッパーコンソール](https://console.adobe.io/)で、新しいプロジェクトの&#x200B;**[!UICONTROL サービスアカウント(JWT)]**&#x200B;資格情報に移動して表示します。 図のように、左側のナビゲーションまたは&#x200B;**[!UICONTROL Credentials]**セクションを使用します。
   ![JWT1](assets/configure-io-target-jwt1.png)
秘密鍵証明 **[!UICONTROL 書の詳細]**&#x200B;で、公開鍵 **、クライアントID** ****、およびサービスアカウントに関連するその他の情報を確認できます。
   ![JWT1a](assets/configure-io-target-jwt1a.png)
2. **[!UICONTROL Adobe Target]** APIに関する情報に移動するには、をクリックします。 図のように、左側のナビゲーションまたは&#x200B;**[!UICONTROL Connected products and services]**セクションを使用します。
   ![JWT2](assets/configure-io-target-jwt2.png)
3. **[!UICONTROL Postmanのダウンロード]** / **[!UICONTROL サービスアカウント(JWT)]**をクリックして、Postman環境の認証情報をキャプチャするJSONファイルを作成します。
   ![JWT3](assets/configure-io-target-jwt3.png)
：ファイルシステムにJSONファイルが記録されます。
   ![JWT3a](assets/configure-io-target-jwt3a.png)
4. Postmanで、歯車アイコンをクリックして環境を管理し、「**読み込み**」をクリックしてJSONファイル（環境）を読み込みます。
   ![JWT4](assets/configure-io-target-jwt4.png)
5. ファイルを選択し、「**開く**」をクリックします。
   ![JWT5](assets/configure-io-target-jwt5.png)
6. ポストマン&#x200B;**環境の管理**モーダルで、新しく読み込んだ環境の名前をクリックして検査します。 (環境名は、ここに示す名前とは異なる場合があります。 必要に応じて名前を編集します。 必ずしもAdobeプロジェクトの名前と一致する必要はありません)。
   ![JWT6](assets/configure-io-target-jwt6.png)
7. （他の変数と共に） `CLIENT_SECRET`と`API_KEY`には、Adobe開発者コンソールで定義したとおり、統合から取得した値が事前に設定されていることに注意してください。 (Postman `CLIENT_SECRET`変数は、開発者コンソールに表示される`CLIENT SECRET`Adobeの秘密鍵証明書と一致する必要があります。Postmanの`API_KEY`も、開発者コンソールの`CLIENT ID`と同じく一致する必要があります。) これに対して、`PRIVATE_KEY`、`JWT_TOKEN`、`ACCESS_TOKEN`は空白です。 まず、`PRIVATE_KEY`値を指定します。
   ![JWT7](assets/configure-io-target-jwt7.png)

   >[!NOTE]
   >
   >**驚き！**
   >
   >ポップクイズ！ 秘密鍵がどこにあるか覚えてる？
   >そうです、Adobe開発者コンソールからダウンロードした`config`ファイル内にあります。

8. ファイルシステムから`config`ファイルを開き、`private`キーファイルを開きます。
   ![JWT8](assets/configure-io-target-jwt8.png)
9. `private`キーファイルの内容全体を選択してコピーします。
   ![JWT9](assets/configure-io-target-jwt9.png)
10. Postmanで、秘密鍵の値を&#x200B;**INITIAL VALUE**&#x200B;フィールドと&#x200B;**CURRENT VALUE**フィールドに貼り付けます。
   ![JWT10](assets/configure-io-target-jwt10.png)
11. 「**[!UICONTROL 更新]**」をクリックし、環境モーダルを閉じます。


## bearerアクセストークンの生成

この節では、Adobe Target APIとのインタラクションを認証するために必要なbearerアクセストークンを生成します。 bearerアクセストークンを生成するには、（前の節で確立した）統合の詳細を[AdobeIdentity Managementサービス(IMS)](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/AuthenticationOverview/AuthenticationGuide.md)に送信する必要があります。 これをおこなう方法はいくつかありますが、このチュートリアルでは、IMS APIに対するカスタムPOSTリクエストを作成します。 冗談だ。 このチュートリアルでは、プロセスを直接簡単にする、事前にビルドされたIMS呼び出しを含むPostmanコレクションを活用します。 コレクションを読み込むと、必要に応じて再利用し、Adobe Targetだけでなく他のAdobeAPI用にも新しいトークンを生成できます。

1. [AdobeIdentity ManagementサービスAPIのサンプル呼び出し](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/ims)に移動します。
   ![token1](assets/configure-io-target-generatetoken1.png)
2. 「**Adobe I/Oアクセストークン生成ポストマン」コレクション**をクリックします。
   ![token2](assets/configure-io-target-generatetoken2.png)
3. **Raw**をクリックし、結果のJSONをクリップボードにコピーして、このコレクションの生のJSONを取得します。 （または、生のJSONを.jsonファイルとして保存することもできます）。
   ![token3](assets/configure-io-target-generatetoken3.png)
4. Postmanで、生のJSONを貼り付けてクリップボードから送信し、コレクションを読み込みます。 （または、保存した.jsonファイルをアップロードすることもできます）。 「**続行**」をクリックします。
   ![token4](assets/configure-io-target-generatetoken4.png)
5. **[!UICONTROL IMSを選択します。JWTAdobe I/Oアクセストークン生成ポストマンコレクションの「ユーザートークン]**&#x200B;を介して+認証を生成」リクエストで、環境が選択されていることを確認し、「**送信**」をクリックしてトークンを生成します。

   ![token5](assets/configure-io-target-generatetoken5.png)

   >[!NOTE]
   >
   >このbearerアクセストークンは24時間有効です。 新しいトークンを生成する必要が生じたらいつでも、リクエストを再度送信します。

6. 環境の管理モーダルをもう一度開き、環境を選択します。
   ![token6](assets/configure-io-target-jwt11.png)
7. `ACCESS_TOKEN`と`JWT_TOKEN`の値が設定されていることに注意してください。
   ![token7](assets/configure-io-target-generatetoken7.png)

>[!NOTE]
>
>Q:JSON Webトークン(JWT)とbearerアクセストークンを生成するには、Adobe I/Oアクセストークン生成ポストマンコレクションを使用する必要がありますか？
>
>回答：いや！ Adobe I/Oアクセストークン生成Postmanコレクションは、PostmanでJWTおよびbearerアクセストークンをより簡単に生成できる便利な方法として利用できます。 または、Adobe開発者コンソール内の機能を使用してbearerアクセストークンを手動で生成できます。

## bearerアクセストークンのテスト

この演習では、[!DNL Target]アカウントからアクティビティのリストを取得するAPIリクエストを送信し、新しいBearerアクセストークンを使用します。 正常な応答は、Adobeプロジェクトと認証がAPIを使用するために期待どおりに動作していることを示します。

1. [Adobe Target Admin APIのPostman Collection](https://developers.adobetarget.com/api/#admin-postman-collection)を読み込みます。 Postmanにコレクションが読み込まれるまで、すべてのプロンプトに従います。
   ![testtoken1](assets/configure-io-target-testtoken0.png)
1. コレクションを展開し、**[!UICONTROL リストアクティビティ]**リクエストをメモします。
   ![testtoken1](assets/configure-io-target-testtoken1.png)
1. `{{access_token}}`などの変数は、最初は未解決です。 この問題を解決する方法はいくつかあります。例えば、`{{access_token}}`という新しいコレクション変数を定義できますが、このチュートリアルでは、代わりにAPIリクエストを変更して、以前使用していたPostman環境を活用します。 これにより、環境は、AdobeAPI間で共通するすべての変数を1つの一貫した統合として引き続き機能します。
   ![testtoken2](assets/configure-io-target-testtoken2.png)
1. `{{access_token}}`を`{{ACCESS_TOKEN}}`に置き換えるには、と入力します。
   ![testtoken3](assets/configure-io-target-testtoken3.png)
1. `{{api_key}}`を`{{API_KEY}}`に置き換えるには、と入力します。
   ![testtoken4](assets/configure-io-target-testtoken4.png)
1. `{{tenant}}`を`{{TENANT_ID}}`に置き換えるには、と入力します。 `{{TENANT_ID}}`はまだ認識されていません。
   ![testtoken4](assets/configure-io-target-testtoken4a.png)
1. 環境の管理モーダルを開き、環境を選択します。
   ![JWT11](assets/configure-io-target-jwt11.png)
1. 新しい`{{TENANT_ID}}`環境変数を追加するには、と入力します。 新しい`TENANT_ID`環境変数の&#x200B;**INITIAL VALUE**&#x200B;フィールドと&#x200B;**CURRENT VALUE**&#x200B;フィールドにテナントID値をコピーして貼り付けます。

   ![testtoken5](assets/configure-io-target-testtoken5.png)

   >[!NOTE]
   >
   >テナントIDが[!DNL Target] `clientcode`と異なります。 [!DNL Target]にログインすると、URLにテナントIDが存在します。 テナントIDを取得するには、[!DNL Adobe Experience Cloud]にログインし、[!DNL Target]を開いて[!DNL Target]カードをクリックします。 URLサブドメインで指定されているテナントID値を使用します。
   >
   >例えば、Adobe Targetにログインした際のURLが
   >
   >`<https://mycompany.experiencecloud.adobe.com/...>`
   >
   >テナントIDが「mycompany」になります。

1. 正しい環境を選択したことを確認した後、リクエストを送信します。 アクティビティのリストを含む応答を受け取ります。
   ![testtoken6](assets/configure-io-target-testtoken6.png)

おめでとう！ これで、Adobe認証を確認したので、Adobe Target API(および他のAdobeAPI)とやり取りするために使用できます。 例えば、[Recommendations API](https://experienceleague.adobe.com/docs/target-learn/recommendations-api-tutorial/recs-api-overview.html?lang=en)を使用して、レコメンデーションを作成または管理できます。
