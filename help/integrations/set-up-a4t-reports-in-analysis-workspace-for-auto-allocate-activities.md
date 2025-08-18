---
title: ' [!DNL Analysis Workspace]  アクティビティ用に A4T レポートを [!UICONTROL Auto-Allocate] 定する方法'
description: '[!UICONTROL Analytics for Target] アクティビティを実行する際に  [!DNL Adobe] [!DNL Analysis Workspace] で [!UICONTROL Auto-Allocate] （A4T）レポートを設定する方法。'
role: User
level: Intermediate
topic: Personalization, Integrations
feature: Analytics for Target (A4T), Auto-Target, Integrations
doc-type: tutorial
kt: null
exl-id: 7d53adce-cc05-4754-9369-9cc1763a9450
source-git-commit: 190a67832f378e15090115420bfaf8a4af4b9cb9
workflow-type: tm+mt
source-wordcount: '1339'
ht-degree: 1%

---

# [!DNL Analysis Workspace] での [!DNL Auto-Allocate] アクティビティ用 A4T レポートの設定

[[!UICONTROL Auto-Allocate] の ](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=ja){target=_blank} アクティビティは [!DNL Adobe Target]2 つ以上のエクスペリエンスの中から勝者を特定し、訪問者のトラフィックをその勝者に自動的に再配分します。その間もテストによる学習は続けられます。 [!UICONTROL Analytics for Target] の [!UICONTROL Auto-Allocate] （A4T）統合により、レポートデータを [!DNL Adobe Analytics] で表示でき、[!DNL Analytics] で定義されたカスタムイベントや指標に対して最適化を行うことができます。

[!DNL Adobe Analytics] [!DNL Analysis Workspace] では豊富な分析機能を利用できますが、アクティビティを正しく解釈するには、デフォルトの [!UICONTROL Analytics for Target] パネルにいくつかの変更を加える必要が [!UICONTROL Auto-Allocate] る場合があります。 これらの変更は、[ 最適化指標の条件 ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-at-aa.html?lang=ja#supported){target=_blank} のニュアンスのために必要です。

次に示すように、A4T では最適化指標のタイプごとに異なるレポート設定が必要です。

* [!DNL Analytics] 指標の使用

   * [!UICONTROL Maximize metric value per visitor]
   * [!UICONTROL Maximize unique visitor conversion rate]

* [!DNL Target] 定義のコンバージョン指標の使用

このチュートリアルでは、A4T の全体的なガイダンスと、条件固有のレポート設定手順を説明します。

## 「[!UICONTROL Maximize Metric Value Per Visitor]」の最適化条件を備えた Analytics 指標

**定義**: （全体的な指標値）/ （訪問者数）

レポートを設定するには、A4T レポートで次の変更を行います。

| 必要な変更 | [!DNL Target] トリガーレポート | A4T パネルレポート |
| --- | --- | --- |
| 特定の指標に対する指標値 [!DNL Analytics] 最大化 | <ul><li>指標 [!UICONTROL Confidence] 削除します。</li><li>[!UICONTROL Lift (Low)] と [!UICONTROL Lift (High)] を削除します。 [!UICONTROL Lift (Med)] を守りなさい。</li><li>混乱を避けるために、[!UICONTROL Conversion Rate] 列からパーセンテージ プレゼンテーションをオフにします。 以下の [A4T の全体的なガイダンス ](#guidance) を参照してください。</li><li>[!UICONTROL Conversion] レート指標の名前を「指標/訪問者」に変更します。</li></ul> | <ul><li>指標 [!UICONTROL Confidence] 削除します。</li><li>[!UICONTROL Lift (Low)] を削除して [!UICONTROL Lift (High)] を保持 [!UICONTROL Lift (Med)] ます。</li><li>混乱を避けるために、[!UICONTROL Conversion Rate] 列からパーセンテージ プレゼンテーションをオフにします。 以下の [A4T の全体的なガイダンス ](#guidance) を参照してください。</li><li>[!UICONTROL Conversion] レート指標の名前を「指標/訪問者」に変更します。</li><li>日付と時間の範囲が、[!DNL Target] レポートに表示される値に合っていることを確認します。 以下の [A4T の全体的なガイダンス ](#guidance) を参照してください。</li></ul> |

![ 売上高に対する指標値の最大化 ](/help/integrations/assets/maximize-metric-value-revenue.png)

## 「[!DNL Analytics]」の最適化条件を使用した指標のカスタマイ [!UICONTROL Unique Visitor Conversion Rate]

**定義**: （指標の正の値を持つユニーク訪問者の数）/ （ユニーク訪問者の合計数）

例：最適化指標が [!UICONTROL Revenue] であるとします。 アクティビティには 5 人のユニーク訪問者がいて、そのうち 3 人が購入します。 この例では、この値=（[!UICONTROL Revenue] が肯定的な訪問者が 3 人）/（ユニーク訪問者の合計が 5 人）= 0.6 = 60% になります。

>[!NOTE]
>
>ここで参照するコンバージョン率は、クリック数やインプレッション数など、注文以外のアクションを指す場合があります。 この場合、条件では、ページをクリックまたは表示した訪問者の数を引き続き最大化します。

レポートを設定するには、A4T レポートで次の変更を行います。

| 必要な変更 | ターゲットトリガーレポート | A4T パネルレポート |
| --- | --- | --- |
| [!DNL Analytics] 指標のコンバージョンを最大化する | <ul><li>指標 [!UICONTROL Confidence] 削除します。</li><li>3 つの [!UICONTROL Lift] 指標をすべて削除します。</li><li>混乱を避けるために、[!UICONTROL Conversion Rate] 列からパーセンテージ プレゼンテーションをオフにします。 以下の [A4T の全体的なガイダンス ](#guidance) を参照してください。</li></ul> | <ul><li>指標 [!UICONTROL Confidence] 削除します。</li><li>3 つの [!UICONTROL Lift] 指標をすべて削除します。</li><li>セグメントを作成して、分析されたアクティビティを表示した訪問者を正の指標値でフィルタリングします。 以下の [ セグメントの作成 ](#segment) を参照してください。</li><li>自動入力された [!UICONTROL Conversion Rate] 指標を、正の指標値を持つ [!UICONTROL Unique visitors] とユニーク訪問者の間の除算に置き換えます。 以下の [ コンバージョン率指標の更新 ](#update-conversion-metric) を参照してください。</li><li>混乱を避けるために、[!UICONTROL Conversion Rate] 列からパーセンテージ プレゼンテーションをオフにします。 以下の [A4T の全体的なガイダンス ](#guidance) を参照してください。</li><li>日付と時間の範囲が、[!DNL Target] レポートに表示される値に合っていることを確認します。 以下の [A4T の全体的なガイダンス ](#guidance) を参照してください。</li></ul> |

### デフォルトの A4T パネルレポート – 追加のガイダンス

以下の節では、デフォルトの A4T パネルレポートを設定する際の追加のガイダンスについて詳しく説明します。

#### セグメントの作成 {#segment}

1. 左側のパネルで **の横にある**+」記号 **[!UICONTROL Segments]** クリックします。

   ![ 左パネルでセグメントの横にあるプラス記号 ](/help/integrations/assets/plus-sign.png)

1. セグメントに「指標値がプラスの訪問者数」というタイトルを付けます。
1. **[!UICONTROL Definition]** の下の **[!UICONTROL Include]** の横にある **[!UICONTROL Visitor]** を選択します。
1. 「**[!UICONTROL Definition]**」の下で、アクティビティの最適化指標を選択します。

   この例では、最適化指標として [!UICONTROL Revenue] を使用します。

1. 「[!UICONTROL is greater than]」演算子を選択し、「0」を指定します。

   これらの設定では、指標の値が正のすべての訪問者をフィルタリングします。

1. **[!UICONTROL Save]** をクリックします。

   ![ 正の指標値 ](/help/integrations/assets/positive-metric-value.png)

1. 「指標値が正の訪問者」という名前の新しく作成されたセグメントを A4T パネルに追加します。
1. 「指標値がプラスの訪問者数」と同じ列に [!UICONTROL Unique Visitors] 指標をドラッグ&amp;ドロップします。

   この設定は、指標の値が肯定的なすべてのユニーク訪問者のセグメントを作成します。 この例では、売上高が 0 より大きいすべてのユニーク訪問者です。

#### [!UICONTROL Conversion Rate] 指標の更新 {#update-conversion-metric}

1. まだ削除していない場合は、次に説明するように、パネルから既存の [!UICONTROL Conversion Rate] 列を削除します。
1. 指標を追加するには、左側のパネルで「**[!UICONTROL Metrics]**」セクションの横にある「+」記号をクリックします。
1. 次に示すように、指標に「コンバージョン率」という名前を付け、「（指標の値が正の [!UICONTROL Unique Visitors]）」を「ユニーク訪問者」で割ったものと定義します。

   「指標値が正の訪問者」、除算演算子、分子の「ユニーク訪問者」指標および「ユニーク訪問者」の新しく作成されたセグメント（以下で定義された手順）を分母として追加します。

   ![A4T パネルでのコンバージョン率。](/help/integrations/assets/conversion-rate.png)

1. **[!UICONTROL Save]** をクリックします。

1. 新しく作成した「コンバージョン率」指標を既存のパネルにドラッグ&amp;ドロップします。
1. 歯車アイコンをクリックし、「**[!UICONTROL Percent]**」チェックボックスの選択を解除します。この値は混乱を招く可能性があるからです。

   レポートを正しく設定すると、次の図のような結果になります。

   ![A4T パネルレポートにおけるユニーク訪問のコンバージョン率 ](/help/integrations/assets/a4t-aa-maximize-metric-value-revenue.png)

## [!DNL Target] 定義のコンバージョン率

レポートを設定するには、A4T レポートで次の変更を行います。

| 必要な変更 | ターゲットトリガーレポート | A4T パネルレポート |
| --- | --- | --- |
| コンバージョン指標を使用した [!DNL Analytics] レポ [!DNL Target] ト | <ul><li>指標 [!UICONTROL Confidence] 削除します。</li><li>[!UICONTROL Lift (Low)] と [!UICONTROL Lift (High)] を削除します。 上昇率（Med）を維持します。</li><li>混乱を避けるために、[!UICONTROL Conversion Rate] 列からパーセンテージ プレゼンテーションをオフにします。 以下の [A4T の全体的なガイダンス ](#guidance) を参照してください。</li></ul> | <ul><li>指標 [!UICONTROL Confidence] 削除します。</li><li>[!UICONTROL Lift (Low)] と [!UICONTROL Lift (High)] を削除します。 [!UICONTROL Lift (Med)] を守りなさい。</li><li>混乱を避けるために、[!UICONTROL Conversion Rate] 列からパーセンテージ プレゼンテーションをオフにします。 以下の [A4T の全体的なガイダンス ](#guidance) を参照してください。</li><li>日付と時間の範囲が、[!DNL Target] レポートに表示される値に合っていることを確認します。 以下の [A4T の全体的なガイダンス ](#guidance) を参照してください。</li></ul> |

レポートを正しく設定すると、次の図のような結果になります。

![ アクティビティのコンバージョン ](/help/integrations/assets/optimized-table.png)

## A4T の全体的なガイダンス {#guidance}

[!UICONTROL Analytics for Target] のレポート画面からリンクをクリックして、事前定義済みの [!UICONTROL Target] パネルに移動できます（これは、このガイドの後半で「[!DNL Target] トリガーレポート」と呼ばれます）。 または、[!DNL Analytics] で A4T パネルを作成することもできます（詳細はこの節の後半で説明します）。

次の節では、選択したこれらの方法に応じて、必要な設定を指定します。 ただし、次の手順は、A4T の全体的なガイダンスとして機能します。

* パネルの作成方法に関係なく、A4T パネルから信頼性指標を削除します（両方とも以下で詳しく説明します）。 代わりに、[!DNL Target] レポートでこれらの値を参照してください。 さらに、アクティビティの勝者は、レポートで特定 [!DNL Target] きます。 アクティビティ勝者の識別について詳しくは、以下の [ アクティビティ勝者の識別 ](#winner) の節を参照してください。
&#x200B;>>
* 混乱を避けるために、[!UICONTROL Percent] 指標の「[!UICONTROL Conversion Rate]」表示をオフにします。 以下の [[!UICONTROL Conversion Rate] 列から割合を非表示にする ](#hide-percentage) を参照してください。
&#x200B;>>
* A4T パネルを作成する場合は、日付と時間の範囲が [!DNL Target] レポートと一致していることを確認します。 以下の [A4T パネルでの日付と時刻の調整 ](#aligning-date-and-time) を参照してください。

### [!UICONTROL Conversion Rate] 列からパーセンテージを非表示にします {#hide-percentage}

1. **列のタイトルの横にある** 歯車 [!UICONTROL Conversion Rate] アイコンをクリックします。

   ![ コンバージョンレート列の歯車アイコン ](/help/integrations/assets/coversion-rate-gear-icon.png)

   [!UICONTROL Column] の設定ダイアログボックスが表示されます。

   ![ 列設定ダイアログボックス ](/help/integrations/assets/column-settings-dialog-box.png){width="200"}

1. 「**[!UICONTROL Percent]**」チェックボックスの選択を解除します。

   以下に示すように、A4T パネルに [!UICONTROL Conversion Rate] としてパーセンテージが含まれず、[!DNL Target] に一致するようになりました。

   ![ コンバージョン率の列に割合が表示されない ](/help/integrations/assets/no-percentages.png)

### A4T パネルでの日時の位置揃え {#aligning-date-and-time}

1. 各パネルの下で、パネルが参照する日付範囲を確認して、日付範囲が [!DNL Target] レポートの日付範囲と一致していることを確認します。

   ![A4T パネルの日付範囲 ](/help/integrations/assets/date-range.png)

1. [!DNL Analytics] では、時間範囲を 12:00am ～ 11:59pm に設定します。

### アクティビティの勝者の特定 {#winner}

アクティビティの勝者 [!DNL Auto-Allocate]、信頼度の値が 95% 以上の勝者コンバージョン率がある場合に選択されます。 これらの値は、[!DNL Target] のレポートで参照する必要があります。信頼性計算は、[!DNL Target] のアクティビティに推奨され [!UICONTROL Auto-Allocate] より保守的な方法を反映しているからです。 [ 自動配分の統計的保証 ](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/determine-winner.html?lang=ja#section_7AF3B93E90BA4B80BC9FC4783B6A389C){target=_blank} を参 *[!UICONTROL Adobe Target Business Practitioner Guide]* してください。

>[!NOTE]
>
>「まだ勝者がありません」と「勝者」のバッジは [!DNL Analysis Workspace] の A4T パネル内では使用できません。 また、[!DNL Target] のアクティビティのレポートに表示され [!UICONTROL Auto-Allocate] 勝者の「星」バッジは無視する必要があります。 [A4T での自動配分と自動ターゲットアクティビティのサポート ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-at-aa.html?lang=ja#aa){target=_blank} の *自動配分* を参 *[!UICONTROL Adobe Target Business Practitioner Guide]* してください。

### [!DNL Analysis Workspace] での [!UICONTROL Auto-Allocate] パネル用の A4T の作成

1. [!UICONTROL Auto-Allocate] アクティビティレポート用に A4T パネルを作成するには、次に示すように、[!UICONTROL Analytics for Target] の [!DNL Analysis Workspace] パネルから開始します。

   ![Analytics for Target – 自動配分レポート ](/help/integrations/assets/a4t-auto-allocate-report.png)

1. 次の選択を行います。

   * **[!UICONTROL Control Experience]**：任意のエクスペリエンスを選択します。
   * **[!UICONTROL Normalizing Metric]**: **[!UICONTROL Visitors]** を選択します（デフォルトでは A4T パネルに含まれます）。 [!UICONTROL Auto-Allocate] では、ユニーク訪問者ごとのコンバージョン率が常に正規化されます。
   * **成功指標**: アクティビティの作成時に使用した指標と同じ（最適化）指標を選択します。 これが [!DNL Target] 定義のコンバージョン指標である場合、「**[!UICONTROL Activity Conversion]**」を選択します。 それ以外の場合は、使用した [!DNL Adobe Analytics] 指標を選択します。









