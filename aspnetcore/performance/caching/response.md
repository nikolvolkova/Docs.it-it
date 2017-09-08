---
title: La memorizzazione nella cache di risposta
author: rick-anderson
description: Viene illustrato come utilizzare la memorizzazione nella cache per ridurre la larghezza di banda e migliorare le prestazioni di risposta.
keywords: ASP.NET Core, risposta, la memorizzazione nella cache, le intestazioni HTTP
ms.author: riande
manager: wpickett
ms.date: 7/10/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 97ca23d48763a96da917c993feb8aadebb450ab5
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="response-caching"></a><span data-ttu-id="7dcfd-104">La memorizzazione nella cache di risposta</span><span class="sxs-lookup"><span data-stu-id="7dcfd-104">Response Caching</span></span>

<span data-ttu-id="7dcfd-105">Da [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), e [Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="7dcfd-105">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Steve Smith](http://ardalis.com)</span></span>

[<span data-ttu-id="7dcfd-106">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="7dcfd-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)

## <a name="what-is-response-caching"></a><span data-ttu-id="7dcfd-107">Che cos'è la memorizzazione nella cache di risposta</span><span class="sxs-lookup"><span data-stu-id="7dcfd-107">What is Response Caching</span></span>

<span data-ttu-id="7dcfd-108">*La memorizzazione nella cache di risposta* aumenta intestazioni correlate alla cache delle risposte.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-108">*Response caching* adds cache-related headers to responses.</span></span> <span data-ttu-id="7dcfd-109">Queste intestazioni specificano la modalità client, proxy e middleware per memorizzare risposte.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-109">These headers specify how you want client, proxy and middleware to cache responses.</span></span> <span data-ttu-id="7dcfd-110">La memorizzazione nella cache di risposta, è possibile ridurre il numero di richieste di che un client o proxy effettua al server web.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-110">Response caching can reduce the number of requests a client or proxy makes to the web server.</span></span> <span data-ttu-id="7dcfd-111">La memorizzazione nella cache di risposta consente inoltre di ridurre la quantità di lavoro del server web esegue per generare la risposta.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-111">Response caching can also reduce the amount of work the web server performs to generate the response.</span></span> 

<span data-ttu-id="7dcfd-112">L'intestazione HTTP primario utilizzato per la memorizzazione nella cache è `Cache-Control`.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-112">The primary HTTP header used for caching is `Cache-Control`.</span></span> <span data-ttu-id="7dcfd-113">Vedere il [la memorizzazione nella cache di HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-113">See the [HTTP 1.1 Caching](https://tools.ietf.org/html/rfc7234#section-5.2) for more information.</span></span> <span data-ttu-id="7dcfd-114">Direttive cache comuni:</span><span class="sxs-lookup"><span data-stu-id="7dcfd-114">Common cache directives:</span></span>

* [<span data-ttu-id="7dcfd-115">public</span><span class="sxs-lookup"><span data-stu-id="7dcfd-115">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)
* [<span data-ttu-id="7dcfd-116">private</span><span class="sxs-lookup"><span data-stu-id="7dcfd-116">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)
* [<span data-ttu-id="7dcfd-117">no-cache</span><span class="sxs-lookup"><span data-stu-id="7dcfd-117">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)
* [<span data-ttu-id="7dcfd-118">Pragma</span><span class="sxs-lookup"><span data-stu-id="7dcfd-118">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)
* [<span data-ttu-id="7dcfd-119">Variare</span><span class="sxs-lookup"><span data-stu-id="7dcfd-119">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)

<span data-ttu-id="7dcfd-120">Il server web è possibile memorizzare nella cache le risposte aggiungendo la risposta di middleware di memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-120">The web server can cache responses by adding the response caching middleware.</span></span> <span data-ttu-id="7dcfd-121">Vedere [risposta la memorizzazione nella cache middleware](middleware.md) per ulteriori informazioni.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-121">See [Response caching middleware](middleware.md) for more information.</span></span>

## <a name="distributed-cache-tag-helper"></a><span data-ttu-id="7dcfd-122">Helper di Tag Cache distribuita</span><span class="sxs-lookup"><span data-stu-id="7dcfd-122">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="7dcfd-123">Il [Helper di Tag della Cache distribuita](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) Abilita memorizzazione nella cache distribuita.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-123">The [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/DistributedCacheTagHelper) enables distributed caching.</span></span>


## <a name="responsecache-attribute"></a><span data-ttu-id="7dcfd-124">Attributo ResponseCache</span><span class="sxs-lookup"><span data-stu-id="7dcfd-124">ResponseCache Attribute</span></span>

<span data-ttu-id="7dcfd-125">Il `ResponseCacheAttribute` specifica i parametri necessari per l'impostazione delle intestazioni appropriate nella cache delle risposte.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-125">The `ResponseCacheAttribute` specifies the parameters necessary for setting appropriate headers in response caching.</span></span> <span data-ttu-id="7dcfd-126">Vedere [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) per una descrizione dei parametri.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-126">See [ResponseCacheAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute)  for a description of the parameters.</span></span>

>[!WARNING]
> <span data-ttu-id="7dcfd-127">Disabilitare la memorizzazione nella cache per il contenuto che contiene informazioni per i client autenticati.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-127">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="7dcfd-128">La memorizzazione nella cache deve essere abilitata solo per il contenuto che non cambia in base all'identità dell'utente, o se un utente è connesso.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-128">Caching should only be enabled for content that does not change based on a user's identity, or whether a user is logged in.</span></span>

<span data-ttu-id="7dcfd-129">`VaryByQueryKeys string[]`(richiede ASP.NET Core 1.1.0 e versioni successive): se è impostata, la risposta di memorizzazione nella cache middleware varia la risposta memorizzata dai valori di elenco specificato di chiavi di query.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-129">`VaryByQueryKeys string[]` (requires ASP.NET Core 1.1.0 and higher): When set, the response caching middleware will vary the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="7dcfd-130">La risposta di middleware di memorizzazione nella cache deve essere abilitata per impostare il `VaryByQueryKeys` proprietà, altrimenti un'eccezione di runtime verrà generata.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-130">The response caching middleware must be enabled to set the `VaryByQueryKeys` property, otherwise a runtime exception will be thrown.</span></span> <span data-ttu-id="7dcfd-131">Non è presente alcuna intestazione HTTP corrispondente per il `VaryByQueryKeys` proprietà.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-131">There is no corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="7dcfd-132">Questa proprietà è una funzionalità HTTP gestita dalla risposta middleware di memorizzazione nella cache.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-132">This property is an HTTP feature handled by the response caching middleware.</span></span> <span data-ttu-id="7dcfd-133">Per il middleware gestire una risposta memorizzata nella cache, la stringa di query e il valore di stringa di query deve corrispondere una richiesta precedente.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-133">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="7dcfd-134">Ad esempio, si consideri la seguente sequenza:</span><span class="sxs-lookup"><span data-stu-id="7dcfd-134">For example, consider the following sequence:</span></span>

| <span data-ttu-id="7dcfd-135">Richiesta</span><span class="sxs-lookup"><span data-stu-id="7dcfd-135">Request</span></span>          | <span data-ttu-id="7dcfd-136">Risultato</span><span class="sxs-lookup"><span data-stu-id="7dcfd-136">Result</span></span> |
| ----------------- | ------------ | 
| `http://example.com?key1=value1` | <span data-ttu-id="7dcfd-137">restituito dal server</span><span class="sxs-lookup"><span data-stu-id="7dcfd-137">returned from server</span></span> |
| `http://example.com?key1=value1` | <span data-ttu-id="7dcfd-138">restituito dal middleware</span><span class="sxs-lookup"><span data-stu-id="7dcfd-138">returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="7dcfd-139">restituito dal server</span><span class="sxs-lookup"><span data-stu-id="7dcfd-139">returned from server</span></span> |

<span data-ttu-id="7dcfd-140">La prima richiesta viene restituita dal server e memorizzati nella cache nel middleware.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-140">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="7dcfd-141">La seconda richiesta viene restituita dal middleware perché la richiesta precedente corrisponde alla stringa di query.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-141">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="7dcfd-142">La terza richiesta non è nella cache middleware perché il valore di stringa di query non corrisponde a una richiesta precedente.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-142">The third request is not in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="7dcfd-143">Il `ResponseCacheAttribute` viene utilizzato per configurare e creare (tramite `IFilterFactory`) un `ResponseCacheFilter`.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-143">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a `ResponseCacheFilter`.</span></span> <span data-ttu-id="7dcfd-144">Il `ResponseCacheFilter` esegue l'operazione di aggiornamento delle intestazioni HTTP appropriate e le funzionalità di risposta.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-144">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="7dcfd-145">Il filtro:</span><span class="sxs-lookup"><span data-stu-id="7dcfd-145">The filter:</span></span>

* <span data-ttu-id="7dcfd-146">Rimuove tutte le intestazioni esistenti per `Vary`, `Cache-Control`, e `Pragma`.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-146">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="7dcfd-147">Scrive le intestazioni appropriate in base alle proprietà impostate `ResponseCacheAttribute`.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-147">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="7dcfd-148">Aggiorna la risposta HTTP funzionalità di cache se `VaryByQueryKeys` è impostata.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-148">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="7dcfd-149">Variare</span><span class="sxs-lookup"><span data-stu-id="7dcfd-149">Vary</span></span>

<span data-ttu-id="7dcfd-150">Questa intestazione viene scritto solo quando il `VaryByHeader` proprietà è impostata.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-150">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="7dcfd-151">È impostato sul `Vary` valore della proprietà.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-151">It is set to the `Vary` property's value.</span></span> <span data-ttu-id="7dcfd-152">L'esempio seguente usa il `VaryByHeader` proprietà.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-152">The following sample uses the `VaryByHeader` property.</span></span>

<span data-ttu-id="7dcfd-153">[!code-csharp[Principale](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="7dcfd-153">[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]</span></span>

<span data-ttu-id="7dcfd-154">È possibile visualizzare le intestazioni di risposta con gli strumenti di rete del browser.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-154">You can view the response headers with your browsers network tools.</span></span> <span data-ttu-id="7dcfd-155">La figura seguente mostra il F12 Edge in uscita nel **rete** scheda quando il `About2` viene aggiornato il metodo di azione.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-155">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed.</span></span> 

![Bordo output F12 nel * * scheda di rete * * quando viene chiamato il metodo di azione 'About2'](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="7dcfd-157">NoStore e Location.None</span><span class="sxs-lookup"><span data-stu-id="7dcfd-157">NoStore and Location.None</span></span>

<span data-ttu-id="7dcfd-158">`NoStore`sostituisce la maggior parte delle altre proprietà.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-158">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="7dcfd-159">Quando questa proprietà è impostata su `true`, `Cache-Control` intestazione verrà impostata su "no-store".</span><span class="sxs-lookup"><span data-stu-id="7dcfd-159">When this property is set to `true`, the `Cache-Control` header will be set to "no-store".</span></span> <span data-ttu-id="7dcfd-160">Se `Location` è impostato su `None`:</span><span class="sxs-lookup"><span data-stu-id="7dcfd-160">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="7dcfd-161">`Cache-Control` è impostato su `"no-store, no-cache"`.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-161">`Cache-Control` is set to `"no-store, no-cache"`.</span></span> 
* <span data-ttu-id="7dcfd-162">`Pragma` è impostato su `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-162">`Pragma` is set to `no-cache`.</span></span> 

<span data-ttu-id="7dcfd-163">Se `NoStore` è `false` e `Location` è `None`, `Cache-Control` e `Pragma` verrà impostato su `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-163">If `NoStore` is `false` and `Location` is `None`,  `Cache-Control` and `Pragma` will be set to `no-cache`.</span></span>

<span data-ttu-id="7dcfd-164">In genere si imposta `NoStore` a `true` nelle pagine di errore.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-164">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="7dcfd-165">Ad esempio:</span><span class="sxs-lookup"><span data-stu-id="7dcfd-165">For example:</span></span>

<span data-ttu-id="7dcfd-166">[!code-csharp[Principale](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="7dcfd-166">[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]</span></span>

<span data-ttu-id="7dcfd-167">Restituirà le intestazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7dcfd-167">This will result in the following headers:</span></span>

```javascript
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="7dcfd-168">Posizione e la durata</span><span class="sxs-lookup"><span data-stu-id="7dcfd-168">Location and Duration</span></span>

<span data-ttu-id="7dcfd-169">Per abilitare la memorizzazione nella cache, `Duration` deve essere impostata su un valore positivo e `Location` deve essere `Any` (impostazione predefinita) o `Client`.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-169">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="7dcfd-170">In questo caso, il `Cache-Control` intestazione verrà impostata sul valore del percorso seguito da "max-age" della risposta.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-170">In this case, the `Cache-Control` header will be set to the location value followed by the "max-age" of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="7dcfd-171">`Location`di opzioni di `Any` e `Client` tradurre `Cache-Control` valori di intestazione `public` e `private`rispettivamente.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-171">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="7dcfd-172">Come indicato in precedenza, l'impostazione `Location` a `None` imposterà entrambi `Cache-Control` e `Pragma` intestazioni per `no-cache`.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-172">As noted previously, setting `Location` to `None` will set both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="7dcfd-173">Di seguito è riportato un esempio che mostra le intestazioni ottenuto impostando `Duration` e lasciando il valore predefinito `Location` valore.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-173">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value.</span></span>

<span data-ttu-id="7dcfd-174">[!code-csharp[Principale](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="7dcfd-174">[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]</span></span>

<span data-ttu-id="7dcfd-175">Genera le intestazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="7dcfd-175">Produces the following headers:</span></span>

```javascript
Cache-Control: public,max-age=60
   ```

### <a name="cache-profiles"></a><span data-ttu-id="7dcfd-176">Profili della cache</span><span class="sxs-lookup"><span data-stu-id="7dcfd-176">Cache Profiles</span></span>

<span data-ttu-id="7dcfd-177">Anziché ripetere `ResponseCache` su molti attributi di azione controller, i profili di cache possono essere configurate come opzioni durante la configurazione di MVC nel `ConfigureServices` metodo `Startup`.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-177">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="7dcfd-178">Valori trovati in un profilo della cache di cui viene fatto riferimento da utilizzare come impostazioni predefinite per il `ResponseCache` attributo e verranno sovrascritte dalle eventuali proprietà specificate per l'attributo.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-178">Values found in a referenced cache profile will be used as the defaults by the `ResponseCache` attribute, and will be overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="7dcfd-179">Impostazione di un profilo della cache:</span><span class="sxs-lookup"><span data-stu-id="7dcfd-179">Setting up a cache profile:</span></span>

<span data-ttu-id="7dcfd-180">[!code-csharp[Principale](response/sample/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="7dcfd-180">[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)]</span></span> 

<span data-ttu-id="7dcfd-181">Riferimento a un profilo della cache:</span><span class="sxs-lookup"><span data-stu-id="7dcfd-181">Referencing a cache profile:</span></span>

<span data-ttu-id="7dcfd-182">[!code-csharp[Principale](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]</span><span class="sxs-lookup"><span data-stu-id="7dcfd-182">[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]</span></span>

<span data-ttu-id="7dcfd-183">Il `ResponseCache` attributo può essere applicato sia per le azioni (metodi), nonché i controller (classi).</span><span class="sxs-lookup"><span data-stu-id="7dcfd-183">The `ResponseCache` attribute can be applied both to actions (methods) as well as controllers (classes).</span></span> <span data-ttu-id="7dcfd-184">Gli attributi a livello di metodo sostituiranno le impostazioni specificate negli attributi a livello di classe.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-184">Method-level attributes will override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="7dcfd-185">Nell'esempio precedente, un attributo a livello di classe specifica una durata di 30 secondi, mentre gli attributi di un livello di metodo fa riferimento a un profilo della cache con una durata impostata su 60 secondi.</span><span class="sxs-lookup"><span data-stu-id="7dcfd-185">In the above example, a class-level attribute specifies a duration of 30 seconds while a method-level attributes references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="7dcfd-186">L'intestazione risulta:</span><span class="sxs-lookup"><span data-stu-id="7dcfd-186">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
   ```

  ### <a name="additional-resources"></a><span data-ttu-id="7dcfd-187">Risorse aggiuntive</span><span class="sxs-lookup"><span data-stu-id="7dcfd-187">Additional Resources</span></span>

* [<span data-ttu-id="7dcfd-188">Memorizzazione nella cache in HTTP dalla specifica</span><span class="sxs-lookup"><span data-stu-id="7dcfd-188">Caching in HTTP from the specification</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="7dcfd-189">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="7dcfd-189">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)