---
title: Adobe TargetAPI用の認証の設定方法
description: このチュートリアルでは、開発者がAdobe TargetAPIとのやり取りに必要な認証トークンを生成する手順を説明します。 次の手順に従って、AdobeDeveloper Consoleを使用してベアラアクセストークンを生成およびテストします。ベアラターゲットAPIを使用するには、この操作が必要です。
role: 開発者、管理者、アーキテクト
level: 中間
topic: パーソナライゼーション、管理、統合、開発
feature: API/SDK、管理と設定
doc-type: tutorial
kt: null
thumbnail: null
author: Judy Kim
translation-type: tm+mt
source-git-commit: 2c371ea17ce38928bcf3655a0d604a69e29963a0
workflow-type: tm+mt
source-wordcount: '1896'
ht-degree: 2%

---


# Adobe TargetAPIの認証の設定

[!DNL Recommendations] Admin APIを含むAdobe Target管理APIは、認証によって保護され、許可されたユーザーのみがAdobe TargetにアクセスするためにAPIを使用するようになります。 [Adobe開発者コンソール](https://console.adobe.io/)を使用して、[!DNL Target]を含むすべてのAdobe Experience Cloudソリューションでこの認証を管理します。

このレッスンでは、Adobe TargetAPIとのやり取りを正しく行うために必要な認証トークンを生成するために必要な暫定的な手順について説明します。 以降の節では、次の操作を行います。

1. Adobeデベロッパーコンソールでプロジェクト（旧称統合）を作成します。
2. プロジェクトの詳細をPostmanにエクスポートします。
3. ベアラアクセストークンを生成します。
4. bearerアクセストークンをテストします。

## 前提条件

| リソース | 詳細 |
| --- | --- |
| 郵便配達人 | これらの手順を正しく完了するには、お使いのオペレーティングシステムの[Postmanアプリ](https://www.postman.com/downloads/)を入手してください。 Postman Basicはアカウントの作成で自由です。 一般的にAdobe TargetAPIを使用するために必須ではありませんが、PostmanはAPIワークフローを容易にし、Adobe TargetはAPIの実行や動作の学習に役立ついくつかのポストマンコレクションを提供しています。 このチュートリアルの残りの部分では、Postmanの作業知識を前提としています。 援助が必要な場合は、[ポストマンのドキュメント](https://learning.getpostman.com/)を参照してください。 |
| リファレンス | このチュートリアルの残りの部分では、次のリソースに精通していることを前提としています。<UL><li>[Adobe I/Oギトブ](https://github.com/adobeio)</li><li>[ターゲットAdobe I/Oドキュメント](https://developers.adobetarget.com/api/#introduction)</li><li>[RecommendationsAPIドキュメント](https://developers.adobetarget.com/api/recommendations/)</li></ul> |

## Adobe I/Oプロジェクトの作成

この節では、Adobeデベロッパーコンソールにアクセスし、[!DNL Adobe Target]用のプロジェクトを作成します。 詳しくは、プロジェクト](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects.md)の[ドキュメントを参照してください。

<!--1. Generate your private key and public certificate, per the [documentation on authentication](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/JWT/JWTCertificate.md). //<!--as described in **Step 1** of [How to set up Adobe IO: Authentication - Step by Step](https://helpx.adobe.com/marketing-cloud-core/kb/adobe-io-authentication-step-by-step.html). After completing Step 1, return to this tutorial and resume with Step 2, below. // The outcome of this step should be the creation of a `private.key` file and a `certificate_pub.crt` file. Return to this tutorial once you have generated these two files.-->

1. [Adobe Admin Console](https://adminconsole.adobe.com/)で、Adobeユーザーアカウントに[製品管理者](https://helpx.adobe.com/enterprise/using/admin-roles.html)と[開発者](https://helpx.adobe.com/enterprise/using/manage-developers.html)の両方のレベルで[!DNL Target]へのアクセス権が与えられていることを確認します。

2. [Adobe開発者コンソール](https://console.adobe.io/)で、この統合を作成するExperience Cloud組織を選択します。 (1つのExperience Cloud組織にしかアクセスできない場合があります)。

   ![configure-io-ターゲット-createproject2.png](assets/configure-io-target-createproject2.png)

3. 「**[!UICONTROL 新しいプロジェクトを作成]**」をクリックします。

   ![configure-io-ターゲット-createproject3.png](assets/configure-io-target-createproject3.png)

4. **[!UICONTROL 追加 API]**&#x200B;をクリックしてREST APIをプロジェクトに追加し、Adobeサービスと製品にアクセスします。

   ![追加API](assets/configure-io-target-createproject4.png)

5. 統合するAdobeサービスとして&#x200B;**[!DNL Adobe Target]**&#x200B;を選択します。 表示される&#x200B;**[!UICONTROL 「次へ]**」ボタンをクリックします。

   ![configure-io-ターゲット-createproject5](assets/configure-io-target-createproject5.png)

6. 公開鍵と秘密鍵を、ターゲット用に作成するサービスアカウント統合に関連付けるオプションを選択します。 このチュートリアルでは、**[!UICONTROL オプション1:キーペア]**&#x200B;を生成し、**[!UICONTROL キーペアを生成]**をクリックします。
   ![configure-io-ターゲット-createproject6](assets/configure-io-target-createproject6.png)

7. 結果をメモしておけ 指示に従って、自動的にダウンロードされた設定ファイル(`config`)を控えておきます。このファイルには秘密鍵が含まれています。 「**[!UICONTROL 次へ]**」をクリックします。
   ![configure-io-ターゲット-createproject7](assets/configure-io-target-createproject7.png)
8. ファイルシステムで、`config`の場所を確認します。これは、前の手順で作成した圧縮構成ファイルです。 繰り返しますが、この`config`ファイルには秘密鍵が含まれています。後で必要になります。 ファイルシステム内の正確な場所は、ここに示す場所とは異なる場合があります。
   ![configure-io-ターゲット-createproject8](assets/configure-io-target-createproject8.png)
9. Adobeデベロッパーコンソールに戻り、[!DNL Recommendations]を使用しているプロパティに対応する[製品プロファイル](https://helpx.adobe.com/enterprise/using/manage-products-and-profiles.html)を選択します。 （プロパティを使用しない場合は、「デフォルトのワークスペース」オプションを選択します）。 「**[!UICONTROL 設定済みAPIを保存]**」をクリックします。
   ![configure-io-ターゲット-createproject9](assets/configure-io-target-createproject9.png)

10. 「**[!UICONTROL 統合を作成]**」をクリックします。 APIが正常に設定されたことを示す一時メッセージが表示されます。

11. 最後の手順として、プロジェクトの名前を元の`Project 1`よりも意味のある名前に変更します。 これを行うには、図に示すようにナビゲーションパスを使用してプロジェクトに移動し、「**[!UICONTROL プロジェクトを編集]**」をクリックして「**[!UICONTROL プロジェクトを編集]」モーダルにアクセスし、プロジェクトの名前を変更します。

![configure-io-ターゲット-createproject11](assets/configure-io-target-createproject11.png)

>[!NOTE]
> 
>このチュートリアルでは、プロジェクトに「ターゲット統合」という名前を付けます。 プロジェクトを単なるAdobe Target以外に使用することを想定している場合は、それに応じて名前を付けることができます。 例えば、Adobe Experience Cloudの他のソリューションと共に使用できるので、「AdobeAPI」または「Experience CloudAPI」という名前を付けることができます。

## プロジェクトの詳細のエクスポート

[!DNL Target]へのアクセスに使用できるAdobeプロジェクトができたので、そのプロジェクトの詳細とAdobeAPIリクエストを送信する必要があります。 これらの詳細は、複数の[!DNL Target] APIを含む複数のAdobeAPIとやり取りするために必要です。 例えば、統合の詳細には[!DNL Target]管理APIで必要な認証情報と認証情報が含まれます。 したがって、PostmanでAPIを使用するには、これらの詳細をPostmanに入手する必要があります。

Postmanではプロジェクトの詳細を指定する方法は多数ありますが、この節では、事前にビルドされた機能やコレクションを利用します。 最初に（この節で）統合の詳細をPostman環境にエクスポートします。 次に（次の節で）ベアラアクセストークンを生成し、必要なAdobeリソースへのアクセスを許可します。

>[!NOTE]
>
>[!DNL Target]を含む、Experience Cloudソリューションで使用できるビデオ手順については、[PostmanとExperience PlatformAPIの使用](https://docs.adobe.com/content/help/en/platform-learn/tutorials/apis/postman.html)を参照してください。 以下の節は[!DNL Target] APIに関連しています。
>
> 1. Adobe I/O統合の詳細をPostmanにエクスポート
> 2. ポストマンでのアクセストークンの生成

>
> 
これらの手順も以下に示します。

1. [Adobe開発者コンソール](https://console.adobe.io/)に移動し、新しいプロジェクトの&#x200B;**[!UICONTROL サービスアカウント(JWT)]**&#x200B;資格情報を表示に移動します。 図に示すように、左側のナビゲーションまたは&#x200B;**[!UICONTROL 資格情報]**セクションを使用します。
   ![JWT1](assets/configure-io-target-jwt1.png)
 **[!UICONTROL 秘密鍵証明書の詳細]**( **公開鍵**、 **クライアントID**、およびサービスアカウントに関連するその他の情報を表示できます)。
   ![JWT1a](assets/configure-io-target-jwt1a.png)
2. **[!UICONTROL Adobe Target]** APIに関する情報に移動します。 図のように、左側のナビゲーションまたは&#x200B;**[!UICONTROL 接続された製品とサービス]**セクションを使用します。
   ![JWT2](assets/configure-io-target-jwt2.png)
3. **[!UICONTROL Postman用にダウンロード]**/**[!UICONTROL サービスアカウント(JWT)]**をクリックして、Postman環境用の認証情報を取り込むJSONファイルを作成します。
   ![JWT3](assets/configure-io-target-jwt3.png)
ファイルシステムにJSONファイルを書き留めます。
   ![JWT3a](assets/configure-io-target-jwt3a.png)
4. Postmanで、ギアアイコンをクリックして環境を管理し、「**インポート**」をクリックしてJSONファイル(環境)をインポートします。
   ![JWT4](assets/configure-io-target-jwt4.png)
5. ファイルを選択し、「**開く**」をクリックします。
   ![JWT5](assets/configure-io-target-jwt5.png)
6. 「ポストマン&#x200B;**環境の管理**」モーダルで、新しく読み込んだ環境の名前をクリックして、それを検査します。 (環境名は、ここに表示されている名前とは異なる場合があります。 必要に応じて名前を編集します。 必ずしもAdobeプロジェクトの名前と一致する必要はありません)。
   ![JWT6](assets/configure-io-target-jwt6.png)
7. （他の変数と共に）`CLIENT_SECRET`と`API_KEY`には、Adobe開発者コンソールで定義されたとおりに、統合から取得した値が事前に設定されています。 (Postman `CLIENT_SECRET`変数は、Developer Consoleに表示される`CLIENT SECRET`Adobe資格情報と一致する必要があります。Postmanの`API_KEY`も同様に、Developer Consoleの`CLIENT ID`と一致する必要があります)。 一方、注記`PRIVATE_KEY`、`JWT_TOKEN`、`ACCESS_TOKEN`は空白です。 `PRIVATE_KEY`の値を指定して開始を行いましょう。
   ![JWT7](assets/configure-io-target-jwt7.png)

   >[!NOTE]
   >
   >**驚き！**
   >
   >クイズ！ 秘密鍵がどこにあるか覚えてる？
   >そうです。Adobeデベロッパーコンソールから以前にダウンロードした`config`ファイルにあります！

8. ファイルシステムから`config`ファイルを開き、`private`キーファイルを開きます。
   ![JWT8](assets/configure-io-target-jwt8.png)
9. `private`キーファイルの内容全体を選択してコピーします。
   ![JWT9](assets/configure-io-target-jwt9.png)
10. Postmanで、秘密鍵の値を&#x200B;**INITIAL VALUE**&#x200B;フィールドと&#x200B;**CURRENT VALUE**フィールドに貼り付けます。
   ![JWT10](assets/configure-io-target-jwt10.png)
11. 「**[!UICONTROL 更新]**」をクリックし、環境モーダルを閉じます。


## bearerアクセストークンの生成

この節では、ベアラアクセストークンを生成します。これは、Adobe TargetAPIとの対話を認証するために必要です。 ベアラアクセストークンを生成するには、（前の節で確立した）統合の詳細を[AdobeIdentity Managementサービス(IMS)](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/AuthenticationOverview/AuthenticationGuide.md)に送信する必要があります。 これを行う方法はいくつかありますが、このチュートリアルでは、IMS APIに対する特示POSTリクエストを作成する方法を紹介します。 冗談だ。 このチュートリアルでは、プロセスを直接簡単に実行できる、事前ビルドIMS呼び出しを含むPostmanコレクションを利用します。 コレクションを読み込むと、必要に応じて再利用し、Adobe Targetだけでなく他のAdobeAPI用にも新しいトークンを生成できます。

1. [AdobeIdentity ManagementサービスAPIサンプル呼び出し](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/ims)に移動します。
   ![token1](assets/configure-io-target-generatetoken1.png)
2. **Adobe I/Oアクセストークン生成ポストマンコレクション**をクリックします。
   ![token2](assets/configure-io-target-generatetoken2.png)
3. 「**生**」をクリックし、結果のJSONをクリップボードにコピーして、このコレクションの生のJSONを取得します。 （または、生のJSONを.jsonファイルとして保存できます）。
   ![token3](assets/configure-io-target-generatetoken3.png)
4. Postmanで、生のJSONを貼り付けてクリップボードから送信し、コレクションを読み込みます。 （または、保存した.jsonファイルをアップロードすることもできます）。 「**続行**」をクリックします。
   ![token4](assets/configure-io-target-generatetoken4.png)
5. **[!UICONTROL IMSを選択します。Adobe I/Oアクセストークン生成ポストマンコレクションのJWT Generate + Auth via User Token]**&#x200B;リクエストを生成し、環境が選択されていることを確認します。「**送信**」をクリックしてトークンを生成します。

   ![token5](assets/configure-io-target-generatetoken5.png)

   >[!NOTE]
   >
   >このベアラアクセストークンは24時間有効です。 新しいトークンを生成する必要が生じた場合は、必ずリクエストを再度送信します。

6. 「環境の管理」モーダルを再度開き、環境を選択します。
   ![token6](assets/configure-io-target-jwt11.png)
7. `ACCESS_TOKEN`と`JWT_TOKEN`の値が入力されるようになりました。
   ![token7](assets/configure-io-target-generatetoken7.png)

>[!NOTE]
>
>Q:JSON Web Token(JWT)およびbearerアクセストークンを生成するには、Adobe I/Oアクセストークン生成ポストマンコレクションを使用する必要がありますか。
>
>A:だめ！ Adobe I/Oアクセストークン生成ポストマンコレクションは、ポストマンでJWTおよびベアラアクセストークンをより容易に生成できる利便性として利用できる。 または、Adobe開発者コンソール内の機能を使用して、ベアラアクセストークンを手動で生成できます。

## bearerアクセストークンのテスト

この実習では、[!DNL Target]アカウントからアクティビティのリストを取得するAPIリクエストを送信して、新しいbearerアクセストークンを使用します。 正常に完了した場合は、APIを使用するために、Adobeプロジェクトと認証が期待どおりに動作していることを示します。

1. [Adobe Target管理APIs Postman Collection](https://developers.adobetarget.com/api/#admin-postman-collection)をインポートします。 コレクションがPostmanに読み込まれるまで、プロンプトに従って操作します。
   ![testtoken1](assets/configure-io-target-testtoken0.png)
1. コレクションを展開し、**[!UICONTROL リストアクティビティ]**リクエストをメモします。
   ![testtoken1](assets/configure-io-target-testtoken1.png)
1. `{{access_token}}`などの変数は、最初は未解決です。 この問題を解決するには、`{{access_token}}`という新しいコレクション変数を定義するなど、いくつかの方法がありますが、このチュートリアルでは、以前使用していたPostman環境を利用するようにAPIリクエストを変更します。 これにより、環境は、AdobeAPI間で共通するすべての変数を単一で一貫性のある統合として機能し続けることができます。
   ![testtoken2](assets/configure-io-target-testtoken2.png)
1. `{{access_token}}`を`{{ACCESS_TOKEN}}`に置き換えるには、と入力します。
   ![testtoken3](assets/configure-io-target-testtoken3.png)
1. `{{api_key}}`を`{{API_KEY}}`に置き換えるには、と入力します。
   ![testtoken4](assets/configure-io-target-testtoken4.png)
1. `{{tenant}}`を`{{TENANT_ID}}`に置き換えるには、と入力します。 メモ`{{TENANT_ID}}`は認識されていません。
   ![testtoken4](assets/configure-io-target-testtoken4a.png)
1. 「環境の管理」モーダルを開き、環境を選択します。
   ![JWT11](assets/configure-io-target-jwt11.png)
1. 新しい`{{TENANT_ID}}`環境変数を追加するには、と入力します。 新しい`TENANT_ID`環境変数の&#x200B;**INITIAL VALUE**&#x200B;フィールドと&#x200B;**CURRENT VALUE**&#x200B;フィールドに、テナントID値をコピーして貼り付けます。

   ![testtoken5](assets/configure-io-target-testtoken5.png)

   >[!NOTE]
   >
   >テナントIDが[!DNL Target] `clientcode`と異なります。 [!DNL Target]にログインすると、URLにテナントIDが存在します。 テナントIDを取得するには、[!DNL Adobe Experience Cloud]にログインし、[!DNL Target]を開き、[!DNL Target]カードをクリックします。 URLサブドメインに記載されているテナントID値を使用します。
   >
   >例えば、Adobe TargetにログインしたときのURLが
   >
   >`<https://mycompany.experiencecloud.adobe.com/...>`
   >
   >テナントIDが「mycompany」になる。

1. 正しい環境を選択したことを確認した上で、リクエストを送信します。 アクティビティのリストを含む回答を受け取ります。
   ![testtoken6](assets/configure-io-target-testtoken6.png)

おめでとう！ これでAdobe認証が検証されたので、これを使用してAdobe TargetAPI(および他のAdobeAPI)とやり取りできます。 例えば、[RecommendationsAPIs](https://docs.adobe.com/content/help/en/target-learn/recommendations-api-tutorial/recs-api-overview.html)を使用して、レコメンデーションを作成または管理できます。
