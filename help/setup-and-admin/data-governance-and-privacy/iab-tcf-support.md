---
title: IAB TCF 2.2のサポート
description: IAB TCFへのAudience Manager プラグインと、Adobeのオプトインオブジェクトおよび同意管理プロバイダー（CMP）との連携について説明します。
feature: Data Governance & Privacy
thumbnail: 26434.jpg
kt: 5027
role: Developer
level: Experienced
exl-id: 04b4e786-0457-4dcc-bcf9-a79eda67bb2e
TQID: https://experienceleague.adobe.com/Nt-232j7k4Gkm-Xu-jHNOpHhl8hFfvXYXLtWSwipQwA
product_v2:
  - id: df80eeb1-8d72-467e-b0df-9d51c7d3a0a1
feature_v2:
  - id: a8b0238e-1d43-4679-a3b4-5ba1bad83baa
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c7d04a2c-412a-4c9d-9d7a-4456eaa5adeb
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 3152e8fc51e0e06c90c17dce0aa203a27995e88d
workflow-type: tm+mt
source-wordcount: 1148
ht-degree: 1%

---

# AUDIENCE MANAGERでのIAB TCF 2.2のサポート {#iab-tcf-support-in-audience-manager}

Adobeでは、オプトイン機能およびAudience Manager プラグインを通じて、IAB Transparency and Consent Framework 2.2 （TCF 2.2）のサポートを通じて、ユーザーのプライバシーに関する選択肢を管理および伝えることができます。 この記事では、IAB TCFに対するAudience Manager プラグインの概要と、Adobeのオプトインオブジェクトおよび同意管理プロバイダー（CMP）との連携について説明するドキュメントと連携します。 IABについて詳しくは、同社のWeb サイト（[https://www.iabeurope.eu/](https://www.iabeurope.eu/)）を参照してください。

## 最初の手順：Experience Cloud ID オプトインについて {#first-step-understand-ecid-s-opt-in}

IAB TCFの操作方法を理解するには、まず、Experience Cloud ID サービス （ECID） ライブラリの一部である[!DNL Opt-in]機能について理解する必要があります。 オプトインの仕組みをご存知でない場合は、最初に[この記事](https://experienceleague.adobe.com/docs/core-services-learn/tutorials/id-service/use-opt-in-to-control-experience-cloud-activities-based-on-user-consent.html)を参照してください。 また、オプトイン [&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=ja)を確認する必要があります。 これらのリソースを完了したら、このページに戻って続行します。

## IAB TCF用Audience Manager プラグイン {#the-audience-manager-plug-in-for-iab-tcf}

これで、オプトインサービスの仕組みに関する基本的な理解が得られたので、Audience Managerでは、プラグインを介してオプトインオブジェクトに対して行われる[!DNL IAB Transparency and Consent Framework (TCF)]のサポートに重ね合わせることができます。

IAB TCF用のAudience Manager プラグインは、オプトイン機能を拡張し、AAMのお客様がIAB TCFに従ってダウンストリームパートナーに対してユーザープライバシーの選択肢を評価、尊重、転送できるようにします。 これは、データコントローラー（Adobeのお客様）とベンダー（DMP、DSP、SSP、Ad Serverなど）の標準を提供します。 を使用して、同意に関する全体像を把握できます。

## IAB TCFを有効にする {#enabling-iab-tcf}

以下の短いビデオに示すように、Adobe Experience Platform Launchを使用している場合は、Audience Manager プラグインをIAB TCF用に有効にするのは簡単です。これは簡単なチェックボックスです。

>[!VIDEO](https://video.tv.adobe.com/v/26433/?quality=12)

または、Launchを使用していない場合は、`isIabContext=true`を使用してExperience Cloud Visitorのインスタンス化を有効にすることもできます。 これにより、IAB TCF フローが開始されます。つまり、IAB TCFを使用してIAB TC文字列をクエリし、オプトインに戻すことで、同意収集に別のステップを追加し、その後、Experience Cloud ソリューションと通信します。

## IAB TC文字列 {#iab-tcf-consent-string}

IABが提供する標準の1つは、「同意文字列」（「DaisyBit」とも呼ばれます）であり、実際には2つのリストが組み合わされています。

1. 目的：**どのような**&#x200B;の同意を得ていますか？
1. ベンダー：**誰**&#x200B;が同意を与えていますか？

### 目的 {#purposes}

IAB TCF 2.2では、同意を収集するための「目的」が10個あります（ベンダーが訪問者のデータで何ができるか）。 Adobe Audience Managerでは、ベンダーの同意に加えて、次の目的のために10個すべてを必要とするのではなく、同意のみを必要とします。

* **目的1:** デバイスに情報を保存またはアクセスする
* **目的10:**&#x200B;製品の開発と改善；
* **特別な目的1:** セキュリティを確保し、不正行為を防止し、デバッグします。

これはIAB TC文字列の最初の部分であり、単に1と0として記録され、その目的/活動が承認されるかどうかを示します。

>[!NOTE]
>
>IAB規制に従い、Special Purpose 1 （セキュリティの確保、不正行為の防止、デバッグ）は常に同意されており、ユーザーは異議を唱えることはできません。

### ベンダー {#vendors}

IAB TC文字列のもう1つの部分は、数百のベンダーの長いリストです。これにより、訪問者は、サイトにタグを持ち、使用するベンダーを選択できる該当するベンダーのリストを表示できます。 ベンダーはリスト内での自分の位置を維持します。 このリストのAdobe Audience Managerのベンダー番号は565です。 リスト内のその番号に「1」がある場合、Audience Managerはリストの先頭から承認済みの目的を実行できます。 AAMのスポットが「0」の場合、データを使用して何もできません。

**お客様がIAB TCFを使用してこれらの目的とベンダーを選択したり、すべてのアクティビティを承認/拒否したりするためのUIをAudience Managerで提供するには、IAB TCFに登録されているCMP パートナーを使用するか、IAB TCFをサポートし、IAB TCFに登録されているCMP パートナーを作成する必要があります。**

## オプトイン：IAB アプリケーションとAdobe アプリケーション間の変換 {#opt-in-translating-between-iab-and-adobe-solutions}

IAB TCFを使用する利点の1つは、上記の標準的な目的により、Adobe ソリューションのリストよりも、エンドユーザーが承認済みの内容をより深く理解できることです。 エンドユーザーは、Audience Managerまたは[!DNL Target]を「承認」することの意味を理解していないかもしれませんが、「デバイス上の情報を保存および/またはアクセスする」または「製品を開発および改良する」ことは、おそらく理解しやすく、同意しやすいでしょう。

Audience Managerを承認するため（例：オプトインのIAB目的を翻訳してAAMに「はい」票を付与するには、上記の目的1および目的10は、エンドユーザーの同意を得る必要があります。 これらのいずれかが承認されていない場合、またはベンダーが承認されていない場合、AAMはピクセルファイヤーを実行したり、Cookieを設定したりしません。 また、多くのお客様が「オールオアナッシング」 UIをエンドユーザーに提供することを選択していることを知っておくのも良いことです。このUIは、Audience Manager（およびその他のExperience Cloud ソリューション）の使用を許可または禁止します。

[&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html?lang=en)には、Audience Manager Plug-In for IAB TCF フローがパブリッシャーと広告主の両方のユースケースにどのように適用されるかについて、素晴らしい情報がいくつかあります。

## IAB：同意をダウンストリームに送信する {#iab-sending-consent-downstream}

IAB TCF用のAudience Manager プラグインを使用すると、ユーザーの同意選択は、グローバルベンダーリストに存在するパートナーのプラットフォームレベル（サードパーティ） ID同期にも送信され、パートナーはユーザーの同意情報を持ち、それに基づいてアクションを実行できます。 この情報は、次の2つの変数で送信されます。

* gdpr = 1
* gdpr_consent = [ エンコードされた同意文字列]

注意点としては、ユーザーがIAB コンテキストにあり、同意を提供しない（または否定的な同意を提供しない）場合、Audience ManagerはIAB TC文字列をまったく収集せず、したがって呼び出しはドロップされます。 その場合、同意は下流には渡されません。

## Demo {#demo}

以下のビデオでは、ECIDとソリューションのCookieとビーコンが、IAB ユーザー選択の影響を受ける方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/26434/?quality=12)

実装およびテスト方法、ユースケース、ワークフローなど、IAB TCF 2.2用のAudience Manager プラグインについて詳しくは、[&#x200B; ドキュメント &#x200B;](https://experienceleague.adobe.com/docs/audience-manager/user-guide/overview/data-privacy/consent-management/aam-iab-plugin.html)を参照してください。
