---
title: Introduzione a Razor Pages in ASP.NET Core
author: Rick-Anderson
description: Il documento offre una panoramica sull'uso di Razor Pages in ASP.NET Core per agevolare lo sviluppo di scenari incentrati sulle pagine.
keywords: ASP.NET Core,Razor Pages
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/razor-pages/index
ms.openlocfilehash: a7c545178f3cdc4bfc095d41ac997d2e46634d30
ms.sourcegitcommit: 19acf7f85f36ecd5a0271fba0cff5e91b85f46ab
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/09/2017
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a>Introduzione a Razor Pages in ASP.NET Core

Di [Rick Anderson](https://twitter.com/RickAndMSFT) e [Ryan Nowak](https://github.com/rynowak)

Razor Pages è una nuova funzionalità di ASP.NET Core MVC che semplifica e rende più produttiva la scrittura di codice incentrata sulle pagine.

Se si sta cercando un'esercitazione in cui si usa l'approccio Model-View-Controller, vedere l'[introduzione ad ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).

<a name="prerequisites"></a>

## <a name="aspnet-core-20-prerequisites"></a>Prerequisiti di ASP.NET Core 2.0

Installare [.NET Core](https://www.microsoft.com/net/core) 2.0.0 o versione successiva.

Se si usa Visual Studio, installare [Visual Studio](https://www.visualstudio.com/vs/) 2017 versione 15.3 o versione successiva con i carichi di lavoro seguenti:

* **Sviluppo ASP.NET e Web**
* **Sviluppo multipiattaforma .NET Core**

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a>Creazione di un progetto Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Per istruzioni dettagliate su come creare un progetto Razor Pages con Visual Studio, vedere la [guida introduttiva a Razor Pages](xref:tutorials/razor-pages/razor-pages-start).

#   <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio per Mac](#tab/visual-studio-mac)

Eseguire `dotnet new razor` dalla riga di comando.

Aprire il file *CSPROJ* generato da Visual Studio per Mac.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code) 

Eseguire `dotnet new razor` dalla riga di comando.

#   <a name="net-core-clitabnetcore-cli"></a>[Interfaccia della riga di comando di .NET Core](#tab/netcore-cli) 

Eseguire `dotnet new razor` dalla riga di comando.

---

## <a name="razor-pages"></a>Razor Pages

La funzionalità Razor Pages è abilitata in *Startup.cs*:

[!code-cs[main](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

Si consideri una pagina di base: <a name="OnGet"></a>

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

Il codice precedente è molto simile a un file di visualizzazione Razor. Ciò che lo differenzia è la direttiva `@page`. `@page` trasforma il file in un'azione MVC, ovvero gestisce le richieste direttamente, senza passare attraverso un controller. `@page` deve essere la prima direttiva Razor in una pagina. `@page` influisce sul comportamento di altri costrutti Razor.

Nei due file seguenti viene visualizzata una pagina simile che usa una classe `PageModel`. Il file *Pages/Index2.cshtml*:

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

Il file"code-behind" *Pages/Index2.cshtml.cs*:

[!code-cs[main](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

Per convenzione, il file di classe `PageModel` ha lo stesso nome del file della pagina Razor con l'aggiunta di *.cs*. Ad esempio, la pagina Razor precedente è *Pages/Index2.cshtml*. Il file che contiene la classe `PageModel` è denominato *Pages/Index2.cshtml.cs*.

Le associazioni dei percorsi URL alle pagine sono determinate dalla posizione della pagina nel file system. Nella tabella seguente sono riportati alcuni percorsi di pagina Razor con gli URL corrispondenti:

| Percorso e nome file               | URL corrispondente |
| ----------------- | ------------ |
| */Pages/Index.cshtml* | `/` o `/Index` |
| */Pages/Contact.cshtml* | `/Contact` |
| */Pages/Store/Contact.cshtml* | `/Store/Contact` |
| */Pages/Store/Index.cshtml* | `/Store` o `/Store/Index` |

Note:

* Il runtime cerca i file delle pagine Razor nella cartella *Pages* per impostazione predefinita.
* `Index` è la pagina predefinita quando un URL non include una pagina.

## <a name="writing-a-basic-form"></a>Scrittura di un form di base

Le funzionalità Razor Pages sono progettate in modo da semplificare i modelli normalmente usati con i Web browser. L'[associazione di modelli](xref:mvc/models/model-binding), gli [helper tag](xref:mvc/views/tag-helpers/intro) e gli helper HTML *funzionano* tutti con le proprietà definite in una classe di pagina Razor. Si consideri una pagina che implementa un form "contact us" di base per il modello `Contact`:

Per gli esempi riportati in questo documento, `DbContext` viene inizializzato nel file [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16).

[!code-cs[main](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

Il modello di dati:

[!code-cs[main](index/sample/RazorPagesContacts/Data/Customer.cs)]

Il contesto del database:

[!code-cs[main](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

Il file di visualizzazione *Pages/Create.cshtml*:

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

Il file code-behind *Pages/Create.cshtml.cs* per la visualizzazione:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

Per convenzione, la classe `PageModel` è denominata `<PageName>Model` e si trova nello stesso spazio dei nomi della pagina.

L'uso di un file `PageModel` code-behind supporta il testing unità, ma richiede la scrittura di un costruttore e una classe espliciti. Le pagine senza file `PageModel` code-behind supportano la compilazione di runtime e questo può essere un vantaggio in fase di sviluppo.  <!-- review: advantage because you can make changes and refresh the browser without explicitly compiling the app -->

La pagina contiene un oggetto `OnPostAsync`, ovvero un *metodo gestore* che viene eseguito per le richieste `POST`, quando un utente invia il form. È possibile aggiungere metodi gestore per qualsiasi verbo HTTP. I gestori più comuni sono:

* `OnGet` per inizializzare lo stato necessario per la pagina. Esempio di [OnGet](#OnGet).
* `OnPost` per gestire gli invii di form.

Il suffisso `Async` nel nome è facoltativo, ma viene spesso usato per convenzione per le funzioni asincrone. Il codice `OnPostAsync` nell'esempio precedente è simile a ciò che in genere si scrive in un controller. Il codice precedente è tipico di Razor Pages. La maggior parte delle primitive di MVC, ad esempio l'[associazione di modelli](xref:mvc/models/model-binding), la [convalida](xref:mvc/models/validation) e i risultati dell'azione vengono condivise.  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

Il metodo `OnPostAsync` precedente:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

Il flusso di base di `OnPostAsync`:

Verificare se sono presenti errori di convalida.

*  Se non sono presenti errori, salvare i dati e reindirizzare.
*  Se sono presenti errori, visualizzare di nuovo la pagina con i messaggi di convalida. La convalida lato client è identica alle applicazioni ASP.NET Core MVC tradizionali. In molti casi gli errori di convalida vengono rilevati nel client e mai inviati al server.

Quando i dati vengono immessi correttamente, il metodo gestore `OnPostAsync` chiama il metodo helper `RedirectToPage` per restituire un'istanza di `RedirectToPageResult`. `RedirectToPage` è un nuovo risultato dell'azione, simile a `RedirectToAction` o `RedirectToRoute`, ma personalizzato per le pagine. Nell'esempio precedente viene reindirizzato alla pagina di indice radice (`/Index`). Il metodo `RedirectToPage` è descritto in dettaglio nella sezione [Generazione di URL per le pagine](#url_gen).

Quando il form inviato contiene errori di convalida (che vengono passati al server), il metodo gestore `OnPostAsync` chiama il metodo helper `Page`. `Page` restituisce un'istanza di `PageResult`. La restituzione di `Page` è simile al modo in cui le azioni nel controller restituiscono `View`. `PageResult` è il tipo restituito <!-- Review  --> predefinito per un metodo gestore. Un metodo gestore che restituisce `void` esegue il rendering della pagina.

La proprietà `Customer` usa l'attributo `[BindProperty]` optare per consentire l'associazione di modelli.

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

Per impostazione predefinita Razor Pages associa le proprietà solo ai verbi non GET. Il binding alle proprietà può ridurre la quantità di codice da scrivere. Riduce il codice usando la stessa proprietà per il eseguire il rendering dei campi del form (`<input asp-for="Customer.Name" />`) e accettare l'input.

La home page (*Index.cshtml*):

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

Il file code-behind *Index.cshtml.cs*:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

Il file *Index.cshtml* contiene il markup seguente per creare un collegamento di modifica per ogni contatto:

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

[Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) ha usato l'attributo [asp-route-{value}](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#route) per generare un collegamento alla pagina di modifica. Il collegamento contiene i dati della route con l'ID contatto. Ad esempio `http://localhost:5000/Edit/1`.

Il file *Pages/Edit.cshtml*:

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

La prima riga contiene la direttiva `@page "{id:int}"`. Il vincolo di routing `"{id:int}"` indica alla pagina di accettare le richieste inviate alla pagina che contengono i dati della route `int`. Se una richiesta inviata alla pagina non contiene dati della route che possono essere convertiti in un oggetto `int`, il runtime restituisce un errore HTTP 404 (non trovato).

Il file *Pages/Edit.cshtml.cs*:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

Il file *Index.cshtml* contiene anche il markup per creare un pulsante di eliminazione per ogni contatto cliente:

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

Quando viene eseguito il rendering del pulsante di eliminazione in HTML, il relativo `formaction` include i parametri per:

* L'ID di contatto cliente dall'attributo `asp-route-id`.
* L'`handler` specificato dall'attributo `asp-page-handler`.

Di seguito è riportato un esempio di pulsante di eliminazione di cui è stato eseguito il rendering con un ID di contatto cliente `1`:

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

Quando il pulsante è selezionato, viene inviata una richiesta `POST` di modulo al server. Per convenzione, il nome del metodo del gestore viene selezionato in base al valore del parametro `handler` in base allo schema `OnPost[handler]Async`.

Poiché in questo esempio l'`handler` è `delete`, il metodo `OnPostDeleteAsync` viene usato per elaborare la richiesta `POST`. Se `asp-page-handler` viene impostato su un valore diverso, come `remove`, viene selezionato un metodo gestore della pagina con il nome `OnPostRemoveAsync`.

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

Il metodo `OnPostDeleteAsync`:

* Accetta l'`id` dalla stringa di query.
* Interroga il database in merito al contatto del cliente con `FindAsync`.
* Se viene trovato il contatto cliente, viene rimosso dall'elenco dei contatti del cliente. Il database viene aggiornato.
* Chiama `RedirectToPage` per reindirizzare alla pagina di indice radice (`/Index`).

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a>XSRF/CSRF e Razor Pages

Non è necessario scrivere codice per la [convalida antifalsificazione](xref:security/anti-request-forgery). La generazione e la convalida del token antifalsificazione sono automaticamente incluse in Razor Pages.

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a>Uso di layout, righe parzialmente eseguite, modelli e helper tag con Razor Pages

Le pagine funzionano con tutte le funzionalità del motore di visualizzazione Razor. I layout, le righe parzialmente eseguite, i modelli, gli helper tag, *_ViewStart.cshtml* e *_ViewImports.cshtml* funzionano esattamente come nelle normali visualizzazioni Razor.

La pagina verrà ora riorganizzata in modo da usufruire di alcune di queste funzionalità.

Aggiungere una [pagina di layout](xref:mvc/views/layout) a *Pages/_Layout.cshtml*:

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

Il [Layout](xref:mvc/views/layout):

* Controlla il layout di ogni pagina, a meno che la pagina non accetti il layout.
* Importa le strutture HTML, ad esempio JavaScript e i fogli di stile.

Vedere l'articolo sulla [pagina Layout](xref:mvc/views/layout) per altre informazioni.

La proprietà [Layout](xref:mvc/views/layout#specifying-a-layout) proprietà viene impostata in *Pages/_ViewStart.cshtml*:

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

**Nota**: il layout è nella cartella *Pages*. Le pagine cercano altre visualizzazioni (layout, modelli, righe parzialmente eseguite) in modo gerarchico, partendo dalla stessa cartella della pagina corrente. Un layout nella cartella *Pages* può essere usato da qualsiasi pagina Razor della cartella *Pages*.

Si consiglia di **non** inserire il file di layout nella cartella *Views/Shared*. *Views/Shared* è un modello destinato alle visualizzazioni MVC. Le pagine Razor devono basarsi sulla gerarchia di cartelle, non su convenzioni di percorso.

La ricerca delle visualizzazioni da una pagina Razor include la cartella *Pages*. Il layout, i modelli e le righe parzialmente eseguite in uso con i controller MVC e le visualizzazioni Razor standard *funzionano solo*.

Aggiungere un file *Pages/_ViewImports.cshtml*:

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

`@namespace` viene spiegato in seguito nell'esercitazione. La direttiva `@addTagHelper` inserisce gli [helper tag predefiniti](xref:mvc/views/tag-helpers/builtin-th/Index) in tutte le pagine presenti nella cartella *Pages*.

<a name="namespace"></a>

Quando la direttiva `@namespace` viene usata in modo esplicito in una pagina:

[!code-cshtml[main](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

La direttiva imposta lo spazio dei nomi per la pagina. La direttiva `@model` non deve necessariamente includere lo spazio dei nomi.

Se la direttiva `@namespace` è contenuta in *_ViewImports.cshtml*, lo spazio dei nomi specificato indica il prefisso per lo spazio dei nomi generato nella pagina che importa la direttiva `@namespace`. Il resto dello spazio dei nomi generato (la parte del suffisso) è il percorso relativo separato dal punto tra la cartella contenente *_Viewimports.cshtml* e la cartella contenente la pagina.

Ad esempio, il file code-behind *Pages/Customers/Edit.cshtml.cs* imposta in modo esplicito lo spazio dei nomi:

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

Il file *Pages/_ViewImports.cshtml* file imposta il seguente spazio dei nomi:

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

Lo spazio dei nomi generato per la pagina Razor *Pages/Customers/Edit.cshtml* corrisponde al file code-behind. La direttiva `@namespace` è stata progettata in modo che le classi C# aggiunte a un progetto e il codice generato dalle pagine *funzionino* senza dover aggiungere una direttiva `@using` per il file code-behind.

**Nota:** `@namespace` funziona anche con le normali visualizzazioni Razor.

Il file di visualizzazione *Pages/Create.cshtml* originale:

[!code-cshtml[main](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

Il file di visualizzazione *Pages/Create.cshtml* aggiornato:

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

Il [progetto iniziale per Razor Pages](#rpvs17) contiene il file *Pages/_ValidationScriptsPartial.cshtml*, che esegue la convalida sul lato client.

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a>Generazione di URL per le pagine

La pagina `Create`, riportata in precedenza, usa `RedirectToPage`:

[!code-cs[main](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

L'applicazione ha la struttura di file o cartella seguente:

* */Pages*

  * *Index.cshtml*
  * */Customer*

    * *Create.cshtml*
    * *Edit.cshtml*
    * *Index.cshtml*

Le pagine *Pages/Customers/Create.cshtml* e *Pages/Customers/Edit.cshtml* reindirizzano a *Pages/Index.cshtml* dopo l'esecuzione. La stringa `/Index` fa parte dell'URI di accesso alla pagina precedente. La stringa `/Index` può essere usata per generare gli URI alla pagina *Pages/Index.cshtml*. Ad esempio:

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

Il nome della pagina è il percorso alla pagina dalla cartella radice */Pages*, inclusa una barra iniziale (`/`), ad esempio `/Index`. Gli esempi di generazione di URL precedenti sono molto più ricco di funzionalità rispetto a livello di codice solo un URL. La generazione di URL usa il [routing](xref:mvc/controllers/routing) ed è in grado di generare e codificare i parametri in base al modo in cui la route è definita nel percorso di destinazione.

La generazione di URL per le pagine supporta i nomi relativi. La tabella seguente indica quale pagina di indice viene selezionata con diversi parametri `RedirectToPage` da *Pages/Customers/Create.cshtml*:

| RedirectToPage(x)| Pagina |
| ----------------- | ------------ |
| RedirectToPage("/Index") | *Pages/Index* |
| RedirectToPage("./Index"); | *Pages/Customers/Index* |
| RedirectToPage("../Index") | *Pages/Index* |
| RedirectToPage("Index")  | *Pages/Customers/Index* |

`RedirectToPage("Index")`, `RedirectToPage("./Index")`, e `RedirectToPage("../Index")` sono *nomi relativi*. Il parametro `RedirectToPage` è *combinato* con il percorso della pagina corrente per calcolare il nome della pagina di destinazione.  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page. -- page name, not page path -->

Il collegamento dei nomi relativi è utile quando si compilano siti con una struttura complessa. Se si usano i nomi relativi per il collegamento tra le pagine in una cartella, è possibile rinominare tale cartella. Tutti i collegamenti continuano a funzionare perché non includono il nome della cartella.

## <a name="tempdata"></a>TempData

ASP.NET Core espone la proprietà [TempData](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller#Microsoft_AspNetCore_Mvc_Controller_TempData) per un [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller). Questa proprietà archivia i dati finché non viene letta. I metodi `Keep` e `Peek` possono essere usati per esaminare i dati senza eliminazione. `TempData` è utile per il reindirizzamento quando i dati sono necessari per più di una singola richiesta.

L'attributo `[TempData]` è nuovo in ASP.NET Core 2.0 ed è supportato per controller e pagine.

Il codice seguente imposta il valore di `Message` usando `TempData`:

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

Il markup seguente nel file *Pages/Customers/Index.cshtml* visualizza il valore di `Message` usando `TempData`.

```cshtml
<h3>Msg: @Model.Message</h3>
```

Il file code-behind *Pages/Customers/Index.cshtml.cs* applica l'attributo `[TempData]` alla proprietà `Message`.

```cs
[TempData]
public string Message { get; set; }
```

Per altre informazioni, vedere [TempData](xref:fundamentals/app-state#temp).

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a>Più gestori per pagina

La pagina seguente genera markup per due gestori di pagina usando l'helper tag `asp-page-handler`:

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there is no `asp-` attribute   -->

Il form nell'esempio precedente contiene due pulsanti di invio, ognuno dei quali usa `FormActionTagHelper` per l'invio a un URL diverso. L'attributo `asp-page-handler` è correlato a `asp-page`. `asp-page-handler` genera gli URL che indirizzano a ognuno dei metodi gestore definiti da una pagina. `asp-page` non è specificato poiché l'esempio è collegato alla pagina corrente.

Il file code-behind:

[!code-cs[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

Il codice precedente usa *metodi gestore denominati*. I metodi gestore denominati vengono creati usando il testo che nel nome segue `On<HTTP Verb>` e precede `Async` (se presente). Nell'esempio precedente i metodi della pagina sono OnPost**JoinList**Async e OnPost**JoinListUC**Async. Rimuovendo *OnPost* e *Async*, i nomi dei gestori sono `JoinList` e `JoinListUC`.

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

Usando il codice precedente, il percorso URL che indirizza a `OnPostJoinListAsync` è `http://localhost:5000/Customers/CreateFATH?handler=JoinList`. Il percorso URL che indirizza a `OnPostJoinListUCAsync` è `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.

## <a name="customizing-routing"></a>Personalizzazione del routing

Se si vuole omettere la stringa di query `?handler=JoinList` dall'URL, è possibile modificare la route in modo da inserire il nome del gestore nella parte di percorso dell'URL. È possibile personalizzare la route aggiungendo un modello di route racchiuso tra virgolette doppie dopo la direttiva `@page`.

[!code-cshtml[main](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

La route precedente inserisce il nome del gestore nel percorso URL anziché nella stringa di query. `?` che segue `handler` indica che il parametro di route è facoltativo.

È possibile usare `@page` per aggiungere altri segmenti e parametri alla route di una pagina. Tutti gli elementi presenti vengono **aggiunti** alla route predefinita della pagina. L'uso di un percorso assoluto o virtuale per modificare la route della pagina, ad esempio `"~/Some/Other/Path"`, non è supportato.

## <a name="configuration-and-settings"></a>Configurazione e impostazioni

Per configurare le opzioni avanzate, usare il metodo di estensione `AddRazorPagesOptions` nel generatore MVC:

[!code-cs[main](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

Attualmente è possibile usare `RazorPagesOptions` per impostare la directory radice per le pagine o aggiungere convenzioni di modello applicativo per le pagine. Si spera che in questo modo si potrà usufruire di una maggiore estendibilità in futuro.

Per la precompilazione delle visualizzazioni, vedere l'articolo sulla [compilazione delle visualizzazioni Razor](xref:mvc/views/view-compilation).

[Scaricare o visualizzare il codice di esempio](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).

Vedere l'articolo [Getting started with Razor Pages in ASP.NET Core](xref:tutorials/razor-pages/razor-pages-start) (Guida introduttiva a Razor Pages in ASP.NET Core), che si basa su questa introduzione.
