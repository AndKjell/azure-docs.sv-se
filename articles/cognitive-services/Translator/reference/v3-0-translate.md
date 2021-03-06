---
title: API för textöversättning översätta metod
titleSuffix: Azure Cognitive Services
description: Använd metoden Translator Text API översätta.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: reference
ms.date: 03/29/2018
ms.author: v-jansko
ms.openlocfilehash: 1841730a39d29c5fe1f3451b7614818e924b339f
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128765"
---
# <a name="translator-text-api-30-translate"></a>Translator Text API 3.0: översätta

Översätter text.

## <a name="request-url"></a>Fråge-URL

Skicka en `POST` begäran om att:

```HTTP
https://api.cognitive.microsofttranslator.com/translate?api-version=3.0
```

## <a name="request-parameters"></a>Begäranparametrar

Parametrarna som skickades mot frågesträngen är:

<table width="100%">
  <th width="20%">Frågeparameter</th>
  <th>Beskrivning</th>
  <tr>
    <td>API-versionen</td>
    <td>*Obligatoriska parametern*.<br/>Versionen av API: et som begärs av klienten. Värdet måste vara `3.0`.</td>
  </tr>
  <tr>
    <td>från</td>
    <td>*Valfri parameter*.<br/>Anger språket i indatatexten. Hitta vilka språk är tillgängliga att översätta från genom att leta upp [språk som stöds](.\v3-0-languages.md) med hjälp av den `translation` omfång. Om den `from` parametern inte anges, automatisk språkidentifiering används för att fastställa en källspråket.</td>
  </tr>
  <tr>
    <td>till</td>
    <td>*Obligatoriska parametern*.<br/>Anger språket i utdata texten. Målspråket som måste vara något av de [språk som stöds](.\v3-0-languages.md) ingår i den `translation` omfång. Till exempel använda `to=de` att översätta tyska.<br/>Det är möjligt att översätta på flera språk samtidigt genom att upprepa parametern i frågesträngen. Till exempel använda `to=de&to=it` att översätta tyska och italienska.</td>
  </tr>
  <tr>
    <td>textType</td>
    <td>*Valfri parameter*.<br/>Anger om texten som översätts är oformaterad text eller HTML-text. All HTML-kod måste vara en korrekt strukturerad, fullständig element. Möjliga värden är: `plain` (standard) eller `html`.</td>
  </tr>
  <tr>
    <td>category</td>
    <td>*Valfri parameter*.<br/>En sträng som anger kategorin (domän) för översättningen. Den här parametern används för att hämta översättningar från ett anpassat system som skapats med [anpassad Translator](../customization.md). Standardvärdet är: `general`.</td>
  </tr>
  <tr>
    <td>ProfanityAction</td>
    <td>*Valfri parameter*.<br/>Anger hur profanities ska hanteras på översättningar. Möjliga värden är: `NoAction` (standard), `Marked` eller `Deleted`. Information om olika sätt att behandla svordomar finns i [svordomar hantering](#handle-profanity).</td>
  </tr>
  <tr>
    <td>ProfanityMarker</td>
    <td>*Valfri parameter*.<br/>Anger hur profanities bör markeras i översättningar. Möjliga värden är: `Asterisk` (standard) eller `Tag`. Information om olika sätt att behandla svordomar finns i [svordomar hantering](#handle-profanity).</td>
  </tr>
  <tr>
    <td>includeAlignment</td>
    <td>*Valfri parameter*.<br/>Anger om du vill inkludera justering projektion från källtext till översatt text. Möjliga värden är: `true` eller `false` (standard). </td>
  </tr>
  <tr>
    <td>includeSentenceLength</td>
    <td>*Valfri parameter*.<br/>Anger om du vill inkludera mening gränser för indatatexten och den översatta texten. Möjliga värden är: `true` eller `false` (standard).</td>
  </tr>
  <tr>
    <td>suggestedFrom</td>
    <td>*Valfri parameter*.<br/>Anger en återställningsplats språk om det inte går att identifiera språket i indatatexten. Automatisk identifiering av språk används när den `from` parametern utelämnas. Om identifieringen misslyckas den `suggestedFrom` antas språk.</td>
  </tr>
  <tr>
    <td>fromScript</td>
    <td>*Valfri parameter*.<br/>Anger skriptet på indatatexten.</td>
  </tr>
  <tr>
    <td>toScript</td>
    <td>*Valfri parameter*.<br/>Anger skriptet på den översatta texten.</td>
  </tr>
</table> 

Begärandehuvuden är:

<table width="100%">
  <th width="20%">Rubriker</th>
  <th>Beskrivning</th>
  <tr>
    <td>_En auktorisering_<br/>_Rubrik_</td>
    <td>*Nödvändiga begärandehuvudet*.<br/>Se [tillgängliga alternativ för autentisering](./v3-0-reference.md#authentication).</td>
  </tr>
  <tr>
    <td>Innehållstyp</td>
    <td>*Nödvändiga begärandehuvudet*.<br/>Anger innehållstypen för nyttolasten. Möjliga värden är: `application/json`.</td>
  </tr>
  <tr>
    <td>Content-Length</td>
    <td>*Nödvändiga begärandehuvudet*.<br/>Längden på det begärda innehållet.</td>
  </tr>
  <tr>
    <td>X-ClientTraceId</td>
    <td>*Valfritt*.<br/>En klientgenererade GUID för unik identifiering på begäran. Du kan utelämna den här rubriken om du inkluderar trace-ID i frågesträngen med hjälp av en frågeparameter som heter `ClientTraceId`.</td>
  </tr>
</table> 

## <a name="request-body"></a>Begärandetext

Brödtexten i begäran är en JSON-matris. Varje matriselement är ett JSON-objekt med en strängegenskap med namnet `Text`, som representerar strängen att översätta.

```json
[
    {"Text":"I would really like to drive your car around the block a few times."}
]
```

Följande begränsningar gäller:

* Matrisen kan ha högst 25 element.
* Hela texten i begäran får inte överskrida 5 000 tecken inklusive blanksteg.

## <a name="response-body"></a>Svarstext

Ett lyckat svar är en JSON-matris med ett resultat för varje sträng i Indatamatrisen. En resultatobjektet innehåller följande egenskaper:

  * `detectedLanguage`: Ett objekt som beskriver det identifierade språket via följande egenskaper:

      * `language`: En sträng som representerar koden för identifierat språk.

      * `score`: Ett flyttalsvärde som anger förtroende i resultatet. Poängen är mellan noll och ett och en låg poäng indikerar ett låga förtroende.

    Den `detectedLanguage` egenskapen finns bara i resultatobjektet när automatisk identifiering av språk som efterfrågas.

  * `translations`: En matris med translation resultat. Storleken på matrisen stämmer med antalet mål språk som anges via den `to` frågeparameter. Varje element i matrisen innehåller:

    * `to`: En sträng som representerar språkkod för språket som mål.

    * `text`: En sträng som ger den översatta texten.

    * `transliteration`: Ett objekt som ger den översatta texten i skriptet som anges av den `toScript` parametern.

      * `script`: En sträng som anger att mål-skriptet.   

      * `text`: En sträng som ger den översatta texten i mål-skriptet.

    Den `transliteration` objekt ingår inte om transkriberingsspråk inte ägde rum.

    * `alignment`: Ett objekt med en enkel sträng-egenskap med namnet `proj`, vilket mappar textinmatning till översatt text. Justering informationen tillhandahålls endast när parametern begäran `includeAlignment` är `true`. Justering returneras som ett strängvärde i följande format: `[[SourceTextStartIndex]:[SourceTextEndIndex]–[TgtTextStartIndex]:[TgtTextEndIndex]]`.  Avgränsas start och slut-index, ett streck separerar språk och blanksteg mellan ord. Ett ord som kan anpassas till noll, ett eller flera ord på andra språk och justerade orden kanske inte är sammanhängande. Om det finns ingen information om justering ska textjustering elementet vara tomt. Se [få justering information](#obtain-alignment-information) för ett exempel och begränsningar.

    * `sentLen`: Ett objekt som returnerar mening gränser i inkommande och utgående texter.

      * `srcSentLen`: En heltalsmatris med som representerar längden på meningarna i indatatexten. Längden på matrisen är antalet meningar och värdena är längden på varje mening.

      * `transSentLen`: En heltalsmatris med som representerar längden på meningarna i den översatta texten. Längden på matrisen är antalet meningar och värdena är längden på varje mening.

    Mening gränser är endast ingår när parameter för förfrågan `includeSentenceLength` är `true`.

  * `sourceText`: Ett objekt med en enkel sträng-egenskap med namnet `text`, vilket ger indatatexten i standardskriptet för språket som källa. `sourceText` Egenskapen finns bara när indata uttrycks i ett skript som inte är vanligt skriptet för språket. Exempel: om indata har arabiska som skrivits i latinska alfabetet, sedan `sourceText.text` skulle samma Arabic text konverteras till Arabrepubliken skript.

Exempel på JSON-svaren finns i den [exempel](#examples) avsnittet.

## <a name="response-status-codes"></a>Svarsstatuskoder

Här följer möjliga HTTP-statuskoder som returnerar en begäran. 

<table width="100%">
  <th width="20%">Statuskod</th>
  <th>Beskrivning</th>
  <tr>
    <td>200</td>
    <td>Lyckades.</td>
  </tr>
  <tr>
    <td>400</td>
    <td>En av frågeparametrarna är saknas eller är inte giltig. Korrigera parametrarna innan du försöker igen.</td>
  </tr>
  <tr>
    <td>401</td>
    <td>Det gick inte att autentisera begäran. Kontrollera att autentiseringsuppgifterna är angivna och giltiga.</td>
  </tr>
  <tr>
    <td>403</td>
    <td>Begäran har inte behörighet. Finns information om felmeddelandet. Detta innebär ofta att alla kostnadsfria översättningar som medföljer en utvärderingsprenumeration har förbrukats.</td>
  </tr>
  <tr>
    <td>429</td>
    <td>Anroparen skickar för många förfrågningar.</td>
  </tr>
  <tr>
    <td>500</td>
    <td>Det uppstod ett oväntat fel. Om felet kvarstår bör du rapportera det med: datum och tid för fel, begärande-ID från svarshuvud `X-RequestId`, och klient-ID från begärandehuvudet `X-ClientTraceId`.</td>
  </tr>
  <tr>
    <td>503</td>
    <td>Servern är inte tillgänglig för tillfället. Gör om begäran. Om felet kvarstår bör du rapportera det med: datum och tid för fel, begärande-ID från svarshuvud `X-RequestId`, och klient-ID från begärandehuvudet `X-ClientTraceId`.</td>
  </tr>
</table> 

## <a name="examples"></a>Exempel

### <a name="translate-a-single-input"></a>Översätta en enda indata

Det här exemplet visar hur du översätta en mening från engelska till kinesiska (förenklad).

# <a name="curltabcurl"></a>[CURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Svarstexten är:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    }
]
```

Den `translations` matris innehåller ett element som tillhandahåller översättningen av den enda textstycke i indata.

### <a name="translate-a-single-input-with-language-auto-detection"></a>Översätta en enkel inmatning med automatisk identifiering av språk

Det här exemplet visar hur du översätta en mening från engelska till kinesiska (förenklad). Begäran anger inte språket. Automatisk identifiering av källspråket används i stället.

# <a name="curltabcurl"></a>[CURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Svarstexten är:

```
[
    {
        "detectedLanguage": {"language": "en", "score": 1.0},
        "translations":[
            {"text": "你好, 你叫什么名字？", "to": "zh-Hans"}
        ]
    }
]
```
Svaret liknar svaret från föregående exempel. Eftersom automatisk språkidentifiering-begärdes innehåller även information om språk som har identifierats för indatatexten i svaret. 

### <a name="translate-with-transliteration"></a>Översätt med transkriberingsspråk

Vi utökar det tidigare exemplet genom att lägga till transkriberingsspråk. Följande begäran begär en kinesiska översättning som skrivits i Latin-skript.

# <a name="curltabcurl"></a>[CURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=zh-Latn" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Svarstexten är:

```
[
    {
        "detectedLanguage":{"language":"en","score":1.0},
        "translations":[
            {
                "text":"你好, 你叫什么名字？",
                "transliteration":{"text":"nǐ hǎo , nǐ jiào shén me míng zì ？","script":"Latn"},
                "to":"zh-Hans"
            }
        ]
    }
]
```

Översättning resultatet innehåller nu en `transliteration` -egenskapen, vilket ger den översatta texten med latinska tecken.

### <a name="translate-multiple-pieces-of-text"></a>Översätt flera typer av text

Översättning av flera strängar på samma gång är bara att ange en matris med strängar i begärandetexten.

# <a name="curltabcurl"></a>[CURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}, {'Text':'I am fine, thank you.'}]"
```

---

Svarstexten är:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"}
        ]
    },            
    {
        "translations":[
            {"text":"我很好，谢谢你。","to":"zh-Hans"}
        ]
    }
]
```

### <a name="translate-to-multiple-languages"></a>Översätt till flera språk

Det här exemplet visar hur du översätta samma indata för flera språk i en begäran.

# <a name="curltabcurl"></a>[CURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'Hello, what is your name?'}]"
```

---

Svarstexten är:

```
[
    {
        "translations":[
            {"text":"你好, 你叫什么名字？","to":"zh-Hans"},
            {"text":"Hallo, was ist dein Name?","to":"de"}
        ]
    }
]
```

### <a name="handle-profanity"></a>Hantera olämpligt språk

Normalt behåller tjänsten Translator svordomar som finns i källan i översättningen. Graden av svordomar och kontext som gör ord olämpliga skiljer sig åt mellan kulturer och därmed graden av olämpligt språk på språket som mål kan framhävas eller minskas.

Du kan använda svordomar filtrering alternativet om du vill undvika svordomar i översättning, oavsett förekomsten av svordomar i källtext. Alternativet kan du välja om du vill se svordomar bort, oavsett om du vill markera profanities med lämpliga taggar (vilket ger dig möjlighet att lägga till egna efterbearbeta), eller om du vill att någon åtgärd krävs. Godkända värden för `ProfanityAction` är `Deleted`, `Marked` och `NoAction` (standard).

<table width="100%">
  <th width="20%">ProfanityAction</th>
  <th>Åtgärd</th>
  <tr>
    <td>`NoAction`</td>
    <td>Detta är standardbeteendet. Svordomar skickas från källan till målet.<br/><br/>
    **Exempel källan (japanska)**: 彼はジャッカスです。<br/>
    **Exempel Translation (på engelska)**: han är en jackass.
    </td>
  </tr>
  <tr>
    <td>`Deleted`</td>
    <td>Olämpliga ord. tas bort från utdata utan ersättning.<br/><br/>
    **Exempel källan (japanska)**: 彼はジャッカスです。<br/>
    **Exempel Translation (på engelska)**: han är en.
    </td>
  </tr>
  <tr>
    <td>`Marked`</td>
    <td>Olämpliga ersättas med en markör i utdata. Markörens beror på den `ProfanityMarker` parametern.<br/><br/>
För `ProfanityMarker=Asterisk`, olämpliga ord har ersatts med `***`:<br/>
    **Exempel källan (japanska)**: 彼はジャッカスです。<br/>
    **Exempel Translation (på engelska)**: han är en \* \* \*.<br/><br/>
För `ProfanityMarker=Tag`, olämpliga ord. omges av XML-taggar &lt;svordomar&gt; och &lt;/profanity&gt;:<br/>
    **Exempel källan (japanska)**: 彼はジャッカスです。<br/>
    **Exempel Translation (på engelska)**: han är en &lt;svordomar&gt;jackass&lt;/profanity&gt;.
  </tr>
</table> 

Exempel:

# <a name="curltabcurl"></a>[CURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'This is a fucking good idea.'}]"
```

---

Detta returnerar:

```
[
    {
        "translations":[
            {"text":"Das ist eine *** gute Idee.","to":"de"}
        ]
    }
]
```

Jämför med:

# <a name="curltabcurl"></a>[CURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de&profanityAction=Marked&profanityMarker=Tag" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'This is a fucking good idea.'}]"
```

---

Den senaste begäran returnerar:

```
[
    {
        "translations":[
            {"text":"Das ist eine <profanity>verdammt</profanity> gute Idee.","to":"de"}
        ]
    }
]
```

### <a name="translate-content-with-markup-and-decide-whats-translated"></a>Översätt innehåll med markup och Bestäm vad är översatt?

Det är vanligt att översätta innehåll som innehåller markup, till exempel innehåll från en HTML-sida eller innehåll från ett XML-dokument. Inkludera Frågeparametern `textType=html` vid översättning av innehåll med taggar. Dessutom kan är ibland det praktiskt att utesluta translation specifikt innehåll. Du kan använda attributet `class=notranslate` och ange innehåll som ska vara på dess ursprungliga språk. I följande exempel innehållet i först `div` elementet kommer inte att översättas när innehållet i andra `div` elementet kommer att översättas.

```
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

Här är ett exempel på begäran för att illustrera.

# <a name="curltabcurl"></a>[CURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=zh-Hans&textType=html" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'<div class=\"notranslate\">This will not be translated.</div><div>This will be translated.</div>'}]"
```

---

Svaret är:

```
[
    {
        "translations":[
            {"text":"<div class=\"notranslate\">This will not be translated.</div><div>这将被翻译。</div>","to":"zh-Hans"}
        ]
    }
]
```

### <a name="obtain-alignment-information"></a>Hämta information för justering

För att få information om justering, ange `includeAlignment=true` i frågesträngen.

# <a name="curltabcurl"></a>[CURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeAlignment=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation.'}]"
```

---

Svaret är:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique.",
                "to":"fr",
                "alignment":{"proj":"0:2-0:1 4:9-3:9 11:14-11:19 16:17-21:24 19:25-40:50 27:37-29:38 38:38-51:51"}
            }
        ]
    }
]
```

Justering av information som börjar med `0:2-0:1`, vilket innebär att de första tre tecknen i texten källa (`The`) mappas till de första två tecknen i den översatta texten (`La`).

Observera följande begränsningar:

* Justering returneras bara för en delmängd av språkpar:
  - från engelska till andra språk.
  - från ett annat språk till engelska förutom kinesiska, förenklad kinesiska, traditionell kinesiska och lettiska till engelska.
  - från japanska till koreanska eller från koreanska till japanska.
* Du får inte justering om meningen är en kapslade översättning. Exempel på en kapslade översättning är ”detta är ett test”, ”jag älskar du” och andra hög frekvens meningar.

### <a name="obtain-sentence-boundaries"></a>Hämta mening gränser

För att få information om Meningslängd i källtext och översatt text, ange `includeSentenceLength=true` i frågesträngen.

# <a name="curltabcurl"></a>[CURL](#tab/curl)

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=fr&includeSentenceLength=true" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The answer lies in machine translation. The best machine translation technology cannot always provide translations tailored to a site or users like a human. Simply copy and paste a code snippet anywhere.'}]"
```

---

Svaret är:

```
[
    {
        "translations":[
            {
                "text":"La réponse se trouve dans la traduction automatique. La meilleure technologie de traduction automatique ne peut pas toujours fournir des traductions adaptées à un site ou des utilisateurs comme un être humain. Il suffit de copier et coller un extrait de code n’importe où.",
                "to":"fr",
                "sentLen":{"srcSentLen":[40,117,46],"transSentLen":[53,157,62]}
            }
        ]
    }
]
```

### <a name="translate-with-dynamic-dictionary"></a>Omvandla dynamisk ordlista

Om du redan vet översättning som du vill använda på ett ord eller en fras kan ange du den som markup i begäran. Dynamisk ordlistan är endast säkra för sammansatt substantiv som egennamn och produktnamn.

Kod för att ange använder du följande syntax.

``` 
<mstrans:dictionary translation=”translation of phrase”>phrase</mstrans:dictionary>
```

Anta exempelvis att engelska meningen ”word wordomatic är en post i ordlistan”. Att bevara ordet _wordomatic_ i översättningen, skickar du begäran:

```
curl -X POST "https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&from=en&to=de" -H "Ocp-Apim-Subscription-Key: <client-secret>" -H "Content-Type: application/json" -d "[{'Text':'The word <mstrans:dictionary translation=\"wordomatic\">word or phrase</mstrans:dictionary> is a dictionary entry.'}]"
```

Resultatet är:

```
[
    {
        "translations":[
            {"text":"Das Wort "wordomatic" ist ein Wörterbucheintrag.","to":"de"}
        ]
    }
]
```

Den här funktionen fungerar på samma sätt med `textType=text` eller med `textType=html`. Funktionen bör användas sparsamt. Det lämpliga och mycket bättre sättet att anpassa translation är med hjälp av anpassade Translator. Anpassade Translator blir fullt ut utnyttja kontext och statistiska sannolikhet. Om du har eller har råd att skapa träningsdata som visar ditt arbete eller en fras i sammanhanget, får mycket bättre resultat. [Mer information om anpassade Translator](../customization.md).
 





