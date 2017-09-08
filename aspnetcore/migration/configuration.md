---
title: Migrazione della configurazione
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 8468d859-ff32-4a92-9e62-08c4a9e36594
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/configuration
ms.openlocfilehash: f19a8389bd8c4035292306216522994b1dbdc506
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/11/2017
---
# <a name="migrating-configuration"></a><span data-ttu-id="56b70-103">Migrazione della configurazione</span><span class="sxs-lookup"><span data-stu-id="56b70-103">Migrating Configuration</span></span>

<span data-ttu-id="56b70-104">Da [Steve Smith](http://ardalis.com) e [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="56b70-104">By [Steve Smith](http://ardalis.com) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="56b70-105">Nell'articolo precedente, abbiamo iniziato [la migrazione di un progetto MVC ASP.NET ad ASP.NET MVC Core](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="56b70-105">In the previous article, we began [migrating an ASP.NET MVC project to ASP.NET Core MVC](mvc.md).</span></span> <span data-ttu-id="56b70-106">In questo articolo è la migrazione di configurazione.</span><span class="sxs-lookup"><span data-stu-id="56b70-106">In this article, we migrate configuration.</span></span>

[<span data-ttu-id="56b70-107">Visualizzare o scaricare codice di esempio</span><span class="sxs-lookup"><span data-stu-id="56b70-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/configuration/samples)

## <a name="setup-configuration"></a><span data-ttu-id="56b70-108">Configurazione dell'installazione</span><span class="sxs-lookup"><span data-stu-id="56b70-108">Setup Configuration</span></span>

<span data-ttu-id="56b70-109">ASP.NET Core non utilizzerà più il *Global. asax* e *Web. config* file utilizzato per le versioni precedenti di ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="56b70-109">ASP.NET Core no longer uses the *Global.asax* and *web.config* files that previous versions of ASP.NET utilized.</span></span> <span data-ttu-id="56b70-110">Nelle versioni precedenti di ASP.NET, la logica di avvio dell'applicazione è stata inserita un `Application_StartUp` metodo all'interno di *Global. asax*.</span><span class="sxs-lookup"><span data-stu-id="56b70-110">In earlier versions of ASP.NET, application startup logic was placed in an `Application_StartUp` method within *Global.asax*.</span></span> <span data-ttu-id="56b70-111">Più avanti, in ASP.NET MVC, un *Startup.cs* file è stato incluso nella radice del progetto; e, è stato chiamato quando l'applicazione avviata.</span><span class="sxs-lookup"><span data-stu-id="56b70-111">Later, in ASP.NET MVC, a *Startup.cs* file was included in the root of the project; and, it was called when the application started.</span></span> <span data-ttu-id="56b70-112">ASP.NET Core ha adottato questo approccio completamente inserendo tutta la logica di avvio di *Startup.cs* file.</span><span class="sxs-lookup"><span data-stu-id="56b70-112">ASP.NET Core has adopted this approach completely by placing all startup logic in the *Startup.cs* file.</span></span>

<span data-ttu-id="56b70-113">Il *Web. config* file è stato sostituito anche in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="56b70-113">The *web.config* file has also been replaced in ASP.NET Core.</span></span> <span data-ttu-id="56b70-114">Configurazione stessa ora può essere configurati, come parte della procedura di avvio dell'applicazione descritto in *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="56b70-114">Configuration itself can now be configured, as part of the application startup procedure described in *Startup.cs*.</span></span> <span data-ttu-id="56b70-115">Configurazione può ancora utilizzare file XML, ma in genere i progetti ASP.NET Core verranno posizionati i valori di configurazione in un file in formato JSON, ad esempio *appSettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="56b70-115">Configuration can still utilize XML files, but typically ASP.NET Core projects will place configuration values in a JSON-formatted file, such as *appsettings.json*.</span></span> <span data-ttu-id="56b70-116">Sistema di configurazione di ASP.NET Core inoltre facilmente accessibili le variabili di ambiente, che possono fornire un percorso più sicuro e potente per valori specifici dell'ambiente.</span><span class="sxs-lookup"><span data-stu-id="56b70-116">ASP.NET Core's configuration system can also easily access environment variables, which can provide a more secure and robust location for environment-specific values.</span></span> <span data-ttu-id="56b70-117">Ciò vale soprattutto per i segreti, ad esempio le stringhe di connessione e le chiavi API devono essere archiviate nel controllo del codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="56b70-117">This is especially true for secrets like connection strings and API keys that should not be checked into source control.</span></span> <span data-ttu-id="56b70-118">Vedere [configurazione](../fundamentals/configuration.md) per ulteriori informazioni sulla configurazione di ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="56b70-118">See [Configuration](../fundamentals/configuration.md) to learn more about configuration in ASP.NET Core.</span></span>

<span data-ttu-id="56b70-119">Per questo articolo, si inizierà con il progetto ASP.NET Core parzialmente migrati da [l'articolo precedente](mvc.md).</span><span class="sxs-lookup"><span data-stu-id="56b70-119">For this article, we are starting with the partially-migrated ASP.NET Core project from [the previous article](mvc.md).</span></span> <span data-ttu-id="56b70-120">Configurazione del programma di installazione, aggiungere il seguente costruttore e proprietà per il *Startup.cs* file si trova nella radice del progetto:</span><span class="sxs-lookup"><span data-stu-id="56b70-120">To setup configuration, add the following constructor and property to the *Startup.cs* file located in the root of the project:</span></span>

<span data-ttu-id="56b70-121">[!code-csharp[Principale](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]</span><span class="sxs-lookup"><span data-stu-id="56b70-121">[!code-csharp[Main](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-21)]</span></span>

<span data-ttu-id="56b70-122">Si noti che a questo punto, il *Startup.cs* file non verrà compilato, come è necessario aggiungere il seguente `using` istruzione:</span><span class="sxs-lookup"><span data-stu-id="56b70-122">Note that at this point, the *Startup.cs* file will not compile, as we still need to add the following `using` statement:</span></span>

```csharp
using Microsoft.Extensions.Configuration;
```

<span data-ttu-id="56b70-123">Aggiungere un *appSettings. JSON* file nella radice del progetto utilizzando il modello di elemento appropriato:</span><span class="sxs-lookup"><span data-stu-id="56b70-123">Add an *appsettings.json* file to the root of the project using the appropriate item template:</span></span>

![Aggiungere AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a><span data-ttu-id="56b70-125">Migrazione delle impostazioni di configurazione dal Web. config</span><span class="sxs-lookup"><span data-stu-id="56b70-125">Migrate Configuration Settings from web.config</span></span>

<span data-ttu-id="56b70-126">Il progetto ASP.NET MVC incluso la stringa di connessione di database richiesti in *Web. config*, nel `<connectionStrings>` elemento.</span><span class="sxs-lookup"><span data-stu-id="56b70-126">Our ASP.NET MVC project included the required database connection string in *web.config*, in the `<connectionStrings>` element.</span></span> <span data-ttu-id="56b70-127">Nel progetto ASP.NET di base, verrà conservata nel *appSettings. JSON* file.</span><span class="sxs-lookup"><span data-stu-id="56b70-127">In our ASP.NET Core project, we are going to store this information in the *appsettings.json* file.</span></span> <span data-ttu-id="56b70-128">Aprire *appSettings. JSON*e si noti che include le operazioni seguenti:</span><span class="sxs-lookup"><span data-stu-id="56b70-128">Open *appsettings.json*, and note that it already includes the following:</span></span>

<span data-ttu-id="56b70-129">[!code-json[Principale](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="56b70-129">[!code-json[Main](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]</span></span>


<span data-ttu-id="56b70-130">Nella riga evidenziata illustrata in precedenza, modificare il nome del database da **_CHANGE_ME** per il nome del database.</span><span class="sxs-lookup"><span data-stu-id="56b70-130">In the highlighted line depicted above, change the name of the database from **_CHANGE_ME** to the name of your database.</span></span>

## <a name="summary"></a><span data-ttu-id="56b70-131">Riepilogo</span><span class="sxs-lookup"><span data-stu-id="56b70-131">Summary</span></span>

<span data-ttu-id="56b70-132">ASP.NET Core colloca tutti logica di avvio per l'applicazione in un singolo file, in cui i servizi necessari e le dipendenze possono essere definite e configurate.</span><span class="sxs-lookup"><span data-stu-id="56b70-132">ASP.NET Core places all startup logic for the application in a single file, in which the necessary services and dependencies can be defined and configured.</span></span> <span data-ttu-id="56b70-133">Sostituisce il *Web. config* file con una funzionalità di configurazione flessibile in grado di sfruttare un'ampia gamma di formati di file, ad esempio JSON, nonché le variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="56b70-133">It replaces the *web.config* file with a flexible configuration feature that can leverage a variety of file formats, such as JSON, as well as environment variables.</span></span>