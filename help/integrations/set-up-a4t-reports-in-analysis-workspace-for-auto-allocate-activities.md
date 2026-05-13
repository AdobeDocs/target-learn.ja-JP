---
title: '[!UICONTROL Auto-Allocate]件のアクティビティについて [!DNL Analysis Workspace] でA4T レポートを設定する方法'
description: '[!UICONTROL Auto-Allocate] アクティビティの実行中に [!DNL Adobe] [!DNL Analysis Workspace]で[!UICONTROL Analytics for Target] （A4T）レポートを設定する方法を教えてください。'
role: User
level: Intermediate
topic: Personalization, Integrations
feature: Analytics for Target (A4T), Auto-Target, Integrations
doc-type: tutorial
kt: null
exl-id: 7d53adce-cc05-4754-9369-9cc1763a9450
TQID: https://experienceleague.adobe.com/5oQMgqqxw2VN-6cb29j4bwEP6VYmGRLXIp5AMJ3WWM4
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2: id: df62f171-ac37-440f-8f0f-f41a72ebdd34
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554
level_v2: id: b5a62a22-46f7-4f0d-b151-3fc640bef588
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: c0b4abf2d4ead4d58a8db6e8970857b7b50dbe5c
workflow-type: tm+mt
source-wordcount: 1390
ht-degree: 2%

---

# [!DNL Auto-Allocate]件のアクティビティに対して[!DNL Analysis Workspace]でA4T レポートを設定します

[!DNL Adobe Target]の[[!UICONTROL Auto-Allocate] アクティビティ ](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html){target=_blank}は、2つ以上のエクスペリエンスの中から勝者を特定し、テストの実行と学習を続ける間に、訪問者のトラフィックを自動的に勝者に再割り当てします。 [!UICONTROL Auto-Allocate]の[!UICONTROL Analytics for Target] （A4T）統合により、[!DNL Adobe Analytics]でレポートデータを表示でき、[!DNL Analytics]で定義されたカスタムイベントまたは指標に対して最適化できます。

豊富な分析機能は[!DNL Adobe Analytics] [!DNL Analysis Workspace]で利用できますが、[!UICONTROL Auto-Allocate]のアクティビティを正しく解釈するために、デフォルトの[!UICONTROL Analytics for Target] パネルに対するいくつかの変更が必要になる場合があります。 これらの変更は、[最適化指標の条件](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-at-aa.html?lang=ja#supported){target=_blank}のニュアンスにより必要です。

最適化指標の各タイプでは、次のように、A4Tで異なるレポート設定が必要です。

* [!DNL Analytics]指標の使用

   * [!UICONTROL Maximize metric value per visitor]
   * [!UICONTROL Maximize unique visitor conversion rate]

* [!DNL Target]定義のコンバージョン指標の使用

このチュートリアルでは、A4T ガイダンス全体と、基準に固有のレポート設定の手順について説明します。

## 最適化条件「[!UICONTROL Maximize Metric Value Per Visitor]」を持つAnalytics指標

**定義**: （全体的な指標値） / （#人の訪問者）

レポートを設定するには、A4T レポートで次の変更を行います。

| 変更が必要 | [!DNL Target] トリガーレポート | A4T パネルレポート |
| --- | --- | --- |
| [!DNL Analytics]指標の指標の値を最大化 | <ul><li>[!UICONTROL Confidence]指標を削除します。</li><li>[!UICONTROL Lift (Low)]と[!UICONTROL Lift (High)]を削除します。 [!UICONTROL Lift (Med)]を保持します。</li><li>混乱を避けるために、[!UICONTROL Conversion Rate]列のパーセント表示のチェックを外します。 以下の[A4T](#guidance)の全体的なガイダンスを参照してください。</li><li>[!UICONTROL Conversion] レート指標の名前を「指標/訪問者」に変更します。</li></ul> | <ul><li>[!UICONTROL Confidence]指標を削除します。</li><li>[!UICONTROL Lift (Low)]と[!UICONTROL Lift (High)]を削除して[!UICONTROL Lift (Med)]を保持します。</li><li>混乱を避けるために、[!UICONTROL Conversion Rate]列のパーセント表示のチェックを外します。 以下の[A4T](#guidance)の全体的なガイダンスを参照してください。</li><li>[!UICONTROL Conversion] レート指標の名前を「指標/訪問者」に変更します。</li><li>日付と時刻の範囲が、[!DNL Target] レポートに表示される値と一致していることを確認します。 以下の[A4T](#guidance)の全体的なガイダンスを参照してください。</li></ul> |

![収益の指標の値を最大化](/help/integrations/assets/maximize-metric-value-revenue.png)

## 最適化条件「[!UICONTROL Unique Visitor Conversion Rate]」を持つ[!DNL Analytics]指標

**定義**: （指標の正の値を持つユニーク訪問者の数） / （ユニーク訪問者の合計数）

例：最適化指標が[!UICONTROL Revenue]であるとします。 アクティビティには5人のユニーク訪問者があり、そのうちの3人が購入します。 この例では、この値= （3人の訪問者のうち、[!UICONTROL Revenue]が肯定的） / （5人のユニーク訪問者） = 0.6 = 60%です。

>[!NOTE]
>
>ここで参照するコンバージョン率は、クリック数やインプレッション数など、注文以外のアクションを指します。 このような場合でも、基準は、それぞれページをクリックまたは表示する訪問者の数を最大化することになります。

レポートを設定するには、A4T レポートで次の変更を行います。

| 変更が必要 | ターゲットトリガーレポート | A4T パネルレポート |
| --- | --- | --- |
| [!DNL Analytics]指標のコンバージョンを最大化 | <ul><li>[!UICONTROL Confidence]指標を削除します。</li><li>3つの[!UICONTROL Lift]指標をすべて削除します。</li><li>混乱を避けるために、[!UICONTROL Conversion Rate]列のパーセント表示のチェックを外します。 以下の[A4T](#guidance)の全体的なガイダンスを参照してください。</li></ul> | <ul><li>[!UICONTROL Confidence]指標を削除します。</li><li>3つの[!UICONTROL Lift]指標をすべて削除します。</li><li>分析されたアクティビティを閲覧した正の指標値を持つ訪問者をフィルタリングするセグメントを作成します。 以下の「[ セグメントを作成](#segment)」を参照してください。</li><li>自動入力された[!UICONTROL Conversion Rate]指標を置き換えて、[!UICONTROL Unique visitors]の除算を正の指標の値とユニーク訪問者に置き換えます。 以下の「[ コンバージョン率メトリックを更新](#update-conversion-metric)」を参照してください。</li><li>混乱を避けるために、[!UICONTROL Conversion Rate]列のパーセント表示のチェックを外します。 以下の[A4T](#guidance)の全体的なガイダンスを参照してください。</li><li>日付と時刻の範囲が、[!DNL Target] レポートに表示される値と一致していることを確認します。 以下の[A4T](#guidance)の全体的なガイダンスを参照してください。</li></ul> |

### デフォルトのA4T パネルレポート – 追加ガイダンス

以下の節では、デフォルトのA4T パネルレポートを設定する際の追加ガイダンスについて詳しく説明します。

#### セグメントの作成 {#segment}

1. 左側のパネルの&#x200B;**[!UICONTROL Segments]**&#x200B;の横にある&#x200B;**&quot;+&quot;記号**&#x200B;をクリックします。

   左側のパネルのセグメントの横に![ プラス記号があります。](/help/integrations/assets/plus-sign.png)

1. 「正の指標の値を持つ訪問者」セグメントのタイトルを付けます。
1. **[!UICONTROL Definition]**&#x200B;で、**[!UICONTROL Include]**&#x200B;の横にある&#x200B;**[!UICONTROL Visitor]**&#x200B;を選択します。
1. **[!UICONTROL Definition]**&#x200B;で、アクティビティの最適化指標を選択します。

   この例では、[!UICONTROL Revenue]を最適化指標と見なします。

1. 「[!UICONTROL is greater than]」演算子を選択し、「0」を指定します。

   これらの設定は、正の指標値を持つすべての訪問者に対してフィルタリングされます。

1. **[!UICONTROL Save]** をクリックします。

   ![正の指標値](/help/integrations/assets/positive-metric-value.png)

1. 「正の指標の値を持つ訪問者」という名前の新しく作成したセグメントをA4T パネルに追加します。
1. 「正の指標値を持つ訪問者」と同じ列に[!UICONTROL Unique Visitors]指標をドラッグ&amp;ドロップします。

   この設定は、指標の値が正のユーザーのすべてのユニーク訪問者のセグメントを作成します。 この例では、収益が0より大きいすべてのユニーク訪問者。

#### [!UICONTROL Conversion Rate]指標の更新 {#update-conversion-metric}

1. まだ行っていない場合は、以下の説明に従って、パネルから既存の[!UICONTROL Conversion Rate]列を削除します。
1. 左側のパネルの「**[!UICONTROL Metrics]**」セクションの横にある「+」記号をクリックして、指標を追加します。
1. 指標に「コンバージョン率」という名前を付け、以下に示すように「（[!UICONTROL Unique Visitors] with positive metric value）」を「ユニーク訪問者」で割って定義します。

   「正の指標の値を持つ訪問者」の新しく作成されたセグメント（以下で定義された手順）、除算演算子、分子の「一意の訪問者」指標、および「一意の訪問者」を分母として追加します。

   ![A4T パネルのコンバージョン率。](/help/integrations/assets/conversion-rate.png)

1. **[!UICONTROL Save]** をクリックします。

1. 新しく作成した「コンバージョン率」指標を既存のパネルにドラッグ&amp;ドロップします。
1. 歯車アイコンをクリックし、**[!UICONTROL Percent]** チェックボックスの選択を解除します。この値は混乱につながる可能性があるからです。

   レポートの正しい設定は、次の図のような結果を生成します。

   ![A4T パネルレポートのユニーク訪問コンバージョン率](/help/integrations/assets/a4t-aa-maximize-metric-value-revenue.png)

## [!DNL Target]定義されたコンバージョン率

レポートを設定するには、A4T レポートで次の変更を行います。

| 変更が必要 | ターゲットトリガーレポート | A4T パネルレポート |
| --- | --- | --- |
| [!DNL Target]のコンバージョン指標を含む[!DNL Analytics]のレポート | <ul><li>[!UICONTROL Confidence]指標を削除します。</li><li>[!UICONTROL Lift (Low)]と[!UICONTROL Lift (High)]を削除します。 上昇率を維持（Med）。</li><li>混乱を避けるために、[!UICONTROL Conversion Rate]列のパーセント表示のチェックを外します。 以下の[A4T](#guidance)の全体的なガイダンスを参照してください。</li></ul> | <ul><li>[!UICONTROL Confidence]指標を削除します。</li><li>[!UICONTROL Lift (Low)]と[!UICONTROL Lift (High)]を削除します。 [!UICONTROL Lift (Med)]を保持します。</li><li>混乱を避けるために、[!UICONTROL Conversion Rate]列のパーセント表示のチェックを外します。 以下の[A4T](#guidance)の全体的なガイダンスを参照してください。</li><li>日付と時刻の範囲が、[!DNL Target] レポートに表示される値と一致していることを確認します。 以下の[A4T](#guidance)の全体的なガイダンスを参照してください。</li></ul> |

レポートの正しい設定は、次の図のような結果を生成します。

![ アクティビティコンバージョン ](/help/integrations/assets/optimized-table.png)

## A4Tの全体的なガイダンス {#guidance}

[!UICONTROL Target]のレポート画面からリンクをクリックすると、事前定義済みの[!UICONTROL Analytics for Target] パネルに移動できます（このガイドでは後で「[!DNL Target] トリガーレポート」と呼びます）。 または、[!DNL Analytics]でA4T パネルを作成することもできます（詳細については、この節で後ほど説明します）。

次の節では、選択する方法に応じて、必要な設定を指定します。 ただし、次の手順は、A4Tの全体的なガイダンスとして機能します。

* パネル作成方法に関係なく、A4T パネルから信頼度指標を削除します（両方について詳しくは後述します）。 代わりに、[!DNL Target] レポートでこれらの値を参照してください。 さらに、アクティビティの勝者は[!DNL Target] レポートで特定できます。 アクティビティの勝者を特定する方法の詳細については、以下の「[ アクティビティの勝者を特定](#winner)」セクションを参照してください。
>>
* 混乱を避けるには、[!UICONTROL Conversion Rate]指標の「[!UICONTROL Percent]」表示のチェックを外します。 以下の[!UICONTROL Conversion Rate]列](#hide-percentage)から割合を非表示にするを参照してください。[
>>
* A4T パネルを構築する場合は、日付と時刻の範囲が[!DNL Target] レポートの範囲と一致していることを確認します。 以下の「[A4T パネルで日時を調整する](#aligning-date-and-time)」を参照してください。

### [!UICONTROL Conversion Rate]列から割合を非表示にする {#hide-percentage}

1. [!UICONTROL Conversion Rate]列のタイトルの横にある&#x200B;**歯車** アイコンをクリックします。

   コンバージョン率の列の![歯車アイコン ](/help/integrations/assets/coversion-rate-gear-icon.png)

   [!UICONTROL Column]設定ダイアログボックスに次の内容が表示されます。

   ![列設定ダイアログボックス ](/help/integrations/assets/column-settings-dialog-box.png){width="200"}

1. 「**[!UICONTROL Percent]**」チェックボックスの選択を解除します。

   A4T パネルに、[!UICONTROL Conversion Rate]としてパーセンテージが含まれず、次に示すように[!DNL Target]と一致するようになりました。

   ![ コンバージョン率の列に割合が表示されない](/help/integrations/assets/no-percentages.png)

### A4T パネルで日時を調整する {#aligning-date-and-time}

1. 各パネルの下で、パネルが参照する日付範囲を確認して、日付範囲が[!DNL Target] レポートの日付範囲と一致することを確認します。

   ![A4T パネルの日付範囲](/help/integrations/assets/date-range.png)

1. [!DNL Analytics]で、時間範囲を12:00am ～ 11:59pmに設定します。

### アクティビティ勝者の特定 {#winner}

[!DNL Auto-Allocate]件のアクティビティの勝者は、信頼値が95%以上の勝利コンバージョン率がある場合に選択されます。 信頼度計算は、[!UICONTROL Auto-Allocate]のアクティビティに対して[!DNL Target]が推奨するより保守的な方法を反映しているので、これらの値は[!DNL Target]のレポートで参照する必要があります。 *[!UICONTROL Adobe Target Business Practitioner Guide]*&#x200B;の「[自動配分の統計的保証](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/determine-winner.html#section_7AF3B93E90BA4B80BC9FC4783B6A389C){target=_blank}」を参照してください。

>[!NOTE]
>
>[!DNL Analysis Workspace]のA4T パネルでは、「まだ勝者なし」バッジと「勝者」バッジは使用できません。 また、[!UICONTROL Auto-Allocate]件のアクティビティに関する[!DNL Target]件のレポートに表示される勝者「星」バッジは無視してください。 *[!UICONTROL Adobe Target Business Practitioner Guide]*&#x200B;の&#x200B;*A4T サポートの[自動配分](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-at-aa.html?lang=en#aa){target=_blank}と自動ターゲットアクティビティ*&#x200B;を参照してください。

### [!DNL Analysis Workspace] での [!UICONTROL Auto-Allocate] パネル用の A4T の作成

1. [!UICONTROL Auto-Allocate] アクティビティレポートのA4T パネルを作成するには、次に示すように、[!DNL Analysis Workspace]の[!UICONTROL Analytics for Target] パネルから開始します。

   ![Analytics for Target – 自動割り当てレポート ](/help/integrations/assets/a4t-auto-allocate-report.png)

1. 次の選択を行います。

   * **[!UICONTROL Control Experience]**：任意のエクスペリエンスを選択します。
   * **[!UICONTROL Normalizing Metric]**: **[!UICONTROL Visitors]**&#x200B;を選択します（デフォルトでA4T パネルに含まれます）。 [!UICONTROL Auto-Allocate] では、ユニーク訪問者ごとのコンバージョン率が常に正規化されます。
   * **成功指標**: アクティビティの作成時に使用したのと同じ（最適化）指標を選択します。 これが[!DNL Target]定義のコンバージョン指標の場合は、**[!UICONTROL Activity Conversion]**&#x200B;を選択します。 それ以外の場合は、使用した [!DNL Adobe Analytics] 指標を選択します。









