---
title: Web.Travelサンプルアプリのダウンロードと更新
seo-title: サンプルアプリをダウンロードし、Mobile Services SDKを検証する
description: 'We.Travelサンプルアプリは、AdobeのMobile Services SDK v4で事前に実装されています。 組織およびソリューションのExperience Cloudアカウントを指すように更新する必要があります。   '
seo-description: We.Travelサンプルアプリは、AdobeのMobile Services SDK v4で事前に実装されています。 組織およびソリューションのExperience Cloudアカウントを指すように更新する必要があります。
feature: mobile
kt: 3040
audience: developer
doc-type: tutorial
activity-type: implement
translation-type: tm+mt
source-git-commit: b331bb29c099bd91df27300ebe199cafa12516db
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 0%

---


# Web.Travelサンプルアプリのダウンロードと更新

We.Travelサンプルアプリは、AdobeのMobile Services SDK v4で事前に実装されています。 組織およびソリューションのExperience Cloudアカウントを指すように、更新するだけです。

## 学習目標

このレッスンを終了すると、次のことができます。

* Android StudioでWe.Travelサンプルアプリをダウンロードして開く
* 次のMobile Services SDK設定の確認と更新 [!DNL Target]

## Web.Travelアプリのダウンロード

* sample-app-android-SDKv4-Base-Version.zipをダウンロードします。 [](assets/sample-app-android-SDKv4-Base-Version.zip)
* zipファイルを解凍します。
* Android Studioで既存のプロジェクトとしてアプリケーションを開く（「Invalid VCS root mapping」に関するエラーは無視）
* エミュレーターでアプリを実行し、アプリが構築され、ホーム画面が表示されることを確認します
* アプリを閲覧し、予約プロセスが完了できることを確認します（「続行」をクリックして請求画面をスキップします）。

   ![appConfirmation画面を開き](assets/wetravel_homeScreen.png)![ます。](assets/wetravel_confirmationScreen.png)

## 次のMobile Services SDK設定の確認と更新 [!DNL Target]

ドキュメントに [従って、Web.Travelアプリ内にAdobeMobile Services SDKがプレインストールさ](https://docs.adobe.com/content/help/en/mobile-services/android/getting-started-android/requirements.html)れました。 次に、自分のア [!DNL Target] カウントを指すようにインストールを更新します。

最初に、Mobile Servicesユーザーインターフェイスで新しいアプリを作成します。

1. Log in to the [Adobe Mobile Services interface](https://mobilemarketing.adobe.com).
1. 「アプリ [!UICONTROL の]管理 **[!UICONTROL 」に移動し、このチュートリアルで使用する新しいアプリを追加するには、]** (アプリを管理/********&#x200B;追加追加アプリを追加します)。
1. 実稼動データ以外のAnalyticsのレポートスイートを選択し、アプリに名前を付け、「 **[!UICONTROL 標準]** 」タイプを選択して「 **[!UICONTROL 保存]**」をクリックします。
1. アプリを追加したら、次の画面の「 [!DNL Target] SDKのTargetオプション [!UICONTROL 」セクションにクライアントコードを追加します(クライアントコードは、設定/ダウンロードボタンの「導入設定][!DNL Target]************`at.js` 」の下のインターフェイスにあります)。
1. 「 [!UICONTROL 要求のタイムアウト] 」設定では、タイムアウトの指示を実行する前に、アプリが [!DNL Target] サーバーからの応答を待機する時間を指定します。 デフォルト設定のままにします。
1. 「 [!UICONTROL 訪問者IDサービス] 」を有効にし、ドロップダウンで [!UICONTROL 組織] (Organization)が選択されていることを確認します。
1. ウィンドウの右上の **[!UICONTROL 「保存]** 」をクリックして変更を保存します(「 [!UICONTROL ユニバーサルリンク]」、「アプリリンク [!UICONTROL 」、「プッシュサービス」の各オプションではなく、ウィンドウの右上にある] )。
1. ページ下部の「アプリのSDKのダウンロード」セクションまでスクロールし、設定ファイルをダウンロードします。

   ![設定ファイルのダウンロード](assets/config_file.jpg)

1. Android Studioプロジェクトのアセットフォルダー（アプリ/src/main/assets）にある `ADBMobileConfig.json` ファイルを置き換えます。

1. 次に、 `ADBMobileConfig.json` ファイルを開き、 [!DNL Target] クライアントコードやAnalyticsの詳細など、期待される変更が含まれていることを確認します。
   ![設定ファイルのダウンロード](assets/client_code.jpg)

設定が表示されない場合は、 **[!UICONTROL Mobile Services]** インターフェイスの適切な「 [!UICONTROL 保存] 」ボタンをクリックし、正しい場所にファイルをコピーしたことを確認します。

おめでとう！ SDKをアカウントの詳細で更新しました [!DNL Target] 。 次のレッスンでリクエストを追加した後、設定の検証を行い [!DNL Target] ます。

**[次へ： &quot;追加Targetリクエスト&quot; >](add-requests.md)**
