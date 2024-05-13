# Integrazione .NET Core 8
## Cohesion2NETCore
Adattamento della classe CohesionSSO.cs per l'utilizzo con .NET Core.

## Per integrare Cohesion
Per integrare l'autenticazione in Cohesion nel proprio sito è sufficiente aggiungere la classe C# CohesionSSO.cs al proprio progetto .NET Core e richiamare i metodi `ValidateFE` e `LogoutFE`.
I metodi vanno richiamati dopo aver inizializzato la classe mediante il costruttore disponibile ed utilizzando la programmazione asincrona tramite i costrutti `async` e `await`.

Una volta autenticati, il token di autenticazione verrà automaticamente salvato nella variabile di sessione `token`, che conterrà l'XML con le informazioni specifiche dell'utente.

La classe utilizza la [cookie authentication](https://learn.microsoft.com/en-us/aspnet/core/security/authentication/cookie?view=aspnetcore-8.0): nel cookie viene automaticamente registrato il codice fiscale dell'utente (nodo `<login>` del token).

Il costruttore della classe prevede i seguenti parametri, eventualmente definibili nel file `appsettings.json` del progetto:

- `HttpContext`: contesto HTTP dal quale vengono automaticamente estrapolate le informazioni relative alla richiesta, alla risposta e alla sessione
- `SSOCheckURL`: parametro fisso della pagina SSO di Cohesion
- `SSOWebCheckSessionURL`: parametro fisso della pagina di recupero token di autenticazione
- `SuccessURL`: URL a cui si viene reindirizzati se l'autenticazione è andata a buon fine
- `ErrorURL`: URL a cui si viene reindirizzati in caso di errore di autenticazione
- `SSOAdditionalData`: parametri opzionali di autenticazione
- `IdSito`: fornito dalla Regione Marche al momento dell'abilitazione a Cohesion

Di seguito un esempio di codice (pattern MVC) in cui i metodi vengono richiamati all'interno delle action `Login` e `Logout` di un controller `AccountController`:

```C#

public class AccountController : Controller
{
    private readonly IHttpContextAccessor _httpContextAccessor;
    private readonly CohesionSettingsOptions _cohesionOptions;

    public AccountController(IHttpContextAccessor httpContextAccessor, IOptions<CohesionSettingsOptions> cohesionOptions)
    {
        _httpContextAccessor = httpContextAccessor;
        _cohesionOptions = cohesionOptions.Value;
    }

    public async Task Login()
    {
        var httpContext = _httpContextAccessor.HttpContext;

        if (httpContext == null)
        {
            return;
        }

        var request = httpContext.Request;
        var response = httpContext.Response;
        var session = httpContext.Session;

        if (request == null || response == null || session == null)
        {
            return;
        }

        var cohesionSSO = new CohesionSSO(httpContext, _cohesionOptions.SSOCheckURL, _cohesionOptions.SSOWebCheckSessionURL, _cohesionOptions.SuccessURL, _cohesionOptions.ErrorURL, _cohesionOptions.SSOAdditionalData, _cohesionOptions.IdSito);
        await cohesionSSO.ValidateFE();
    }

    public async Task Logout()
    {
        var httpContext = _httpContextAccessor.HttpContext;

        if (httpContext == null)
        {
            return;
        }

        var request = httpContext.Request;
        var response = httpContext.Response;
        var session = httpContext.Session;

        if (request == null || response == null || session == null)
        {
            return;
        }

        var cohesionSSO = new CohesionSSO(httpContext, _cohesionOptions.SSOCheckURL, _cohesionOptions.SSOWebCheckSessionURL, _cohesionOptions.SuccessURL, _cohesionOptions.ErrorURL, _cohesionOptions.SSOAdditionalData, _cohesionOptions.IdSito);
        await cohesionSSO.LogoutFE();
    }
}

```

Per ulteriori dettagli, fare riferimento al codice della classe e alla documentazione ufficiale disponibile nela sezione [Integrazione Classe C#](https://dariscappelletti.github.io/CohesionID-Docs/Integrazione-Classe-C%23/).
