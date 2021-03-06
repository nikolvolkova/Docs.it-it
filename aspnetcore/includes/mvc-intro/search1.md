# <a name="adding-search-to-an-aspnet-core-mvc-app"></a>Aggiunta della funzionalità di ricerca a un'app ASP.NET Core MVC

Di [Rick Anderson](https://twitter.com/RickAndMSFT)

In questa sezione si aggiunge una funzionalità di ricerca al metodo di azione `Index` che consente di ricercare i film in base al *genere* o al *nome*.

Aggiornare il metodo `Index` con il codice seguente:
<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]
-->

[!code-csharp[Principale](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

La prima riga del metodo di azione `Index` crea una query [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) per selezionare i film:

```csharp
var movies = from m in _context.Movie
             select m;
```

La query viene *solo* definita in questo punto, ma **non** viene eseguita nel database.

Se il parametro `searchString` contiene una stringa, la query dei film viene modificata per filtrare in base al valore della stringa di ricerca:

[!code-csharp[Principale](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

Il codice `s => s.Title.Contains()` precedente è un'[espressione lambda](http://msdn.microsoft.com/library/bb397687.aspx). Le espressioni lambda vengono usate nelle query [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) basate sul metodo come argomenti dei metodi di operatori di query standard, quali il metodo [Where](http://msdn.microsoft.com/library/system.linq.enumerable.where.aspx) o `Contains` (usato nel codice precedente). Le query LINQ non vengono eseguite al momento della definizione o della modifica chiamando un metodo, ad esempio `Where`, `Contains` o `OrderBy`. L'esecuzione della query viene invece posticipata.  Per esecuzione posticipata si intende che la valutazione di un'espressione viene ritardata finché non viene eseguita l'iterazione del valore o non viene chiamato il metodo `ToListAsync`. Per altre informazioni sull'esecuzione posticipata di query, vedere [Esecuzione di query](http://msdn.microsoft.com/library/bb738633.aspx).

Nota: il metodo [Contains](http://msdn.microsoft.com/library/bb155125.aspx) viene eseguito sul database, non nel codice C# indicato in precedenza. La distinzione tra maiuscole/minuscole nella query dipende dal database e dalle regole di confronto. In SQL Server, [Contains](http://msdn.microsoft.com/library/bb155125.aspx) esegue il mapping a [SQL LIKE](http://msdn.microsoft.com/library/ms179859.aspx), che fa distinzione tra maiuscole e minuscole. In SQLite, con le regole di confronto predefinite si fa distinzione tra maiuscole e minuscole.

Passare a `/Movies/Index`. Accodare una stringa di query, ad esempio `?searchString=Ghost`, all'URL. Vengono visualizzati i film filtrati.

![Vista Index](../../tutorials/first-mvc-app/search/_static/ghost.png)

Se si modifica la firma del metodo `Index` in modo che contenga un parametro denominato `id`, il parametro `id` corrisponderà al segnaposto `{id}` facoltativo per le route predefinite impostate in *Startup.cs*.

[!code-csharp[Principale](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]