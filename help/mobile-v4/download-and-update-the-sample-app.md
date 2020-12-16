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
* [!DNL Target]のMobile Services SDK設定の確認と更新

## Web.Travelアプリのダウンロード

* [sample-app-android-SDKv4-Base-Version.zip](assets/sample-app-android-SDKv4-Base-Version.zip)をダウンロードします。
* zipファイルを解凍します。
* Android Studioで既存のプロジェクトとしてアプリケーションを開く（「Invalid VCS root mapping」に関するエラーは無視）
* エミュレーターでアプリを実行し、アプリが構築され、ホーム画面が表示されることを確認します
* アプリを閲覧し、予約プロセスが完了できることを確認します（「続行」をクリックして請求画面をスキップします）。

   ![appConfirmation画面を開き](assets/wetravel_homeScreen.png)![ます。](assets/wetravel_confirmationScreen.png)

## [!DNL Target]のMobile Services SDK設定の確認と更新

ドキュメント](https://docs.adobe.com/content/help/en/mobile-services/android/getting-started-android/requirements.html)に従って、Web.Travelアプリ[内にAdobeMobile Services SDKがプレインストールされました。 次に、自分の[!DNL Target]アカウントを指すようにインストールを更新します。

最初に、Mobile Servicesユーザーインターフェイスで新しいアプリを作成します。

1. [AdobeMobile Servicesインターフェイス](https://mobilemarketing.adobe.com)にログインします。
1. [!UICONTROL アプリの管理]に移動し、**[!UICONTROL 追加]**&#x200B;をクリックして、このチュートリアルで使用する新しいアプリを追加します(**[!UICONTROL アプリの管理]**/**[!UICONTROL 追加]**)。
1. 実稼動データ以外のデータを含むAnalyticsレポートスイートを選択し、アプリに名前を付け、「**[!UICONTROL 標準]**」タイプを選択して、「**[!UICONTROL 保存]**」をクリックします。
1. アプリを追加したら、次の画面の「[!UICONTROL SDKターゲットオプション]」セクションに[!DNL Target]クライアントコードを追加します（**[!UICONTROL セットアップ]**/**[!UICONTROL 実装]**/**[!UICONTROL 設定を編集]**、をクリックします）。`at.js`[!DNL Target]
1. [!UICONTROL 要求のタイムアウト]設定は、アプリが[!DNL Target]サーバーからの応答を待ってから、タイムアウトの指示を実行するまでの時間を指定します。 デフォルト設定のままにします。
1. [!UICONTROL 訪問者IDサービス]を有効にし、ドロップダウンで[!UICONTROL 組織]が選択されていることを確認します。
1. ウィンドウの右上の「**[!UICONTROL 保存]**」（[!UICONTROL ユニバーサルリンク]、[!UICONTROL アプリリンク]、または[!UICONTROL プッシュサービス]セクション内のものではありません）をクリックして、変更を保存します。
1. ページ下部の「アプリのSDKのダウンロード」セクションまでスクロールし、設定ファイルをダウンロードします。

   ![設定ファイルのダウンロード](assets/config_file.jpg)

1. Android Studioプロジェクトのアセットフォルダー(app/src/main/assets)にある`ADBMobileConfig.json`ファイルを置き換えます。

1. 次に、`ADBMobileConfig.json`ファイルを開き、[!DNL Target]クライアントコードやAnalyticsの詳細など、期待される変更が&lt;a0/>に含まれていることを確認します。
   ![設定ファイルのダウンロード](assets/client_code.jpg)

設定が表示されない場合は、[!UICONTROL Mobile Services]インターフェイスの右の「**[!UICONTROL 保存]**」ボタンをクリックし、正しい場所にファイルをコピーしたことを確認します。

おめでとう！ [!DNL Target]アカウントの詳細でSDKを更新しました。 次のレッスンで[!DNL Target]リクエストを追加した後、設定の検証を行います。

**[次へ：&quot;追加ターゲットリクエスト&quot; >](add-requests.md)**
