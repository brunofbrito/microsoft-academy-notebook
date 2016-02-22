# [Introduction to ASP.NET MVC](https://mva.microsoft.com/en-US/training-courses/introduction-to-aspnet-mvc-8322?l=nKZwZ8Zy_3504984382)

## Basics of MVC and the Moving Parts

Nas Views não se deve colocar qualquer lógica, como por exemplo IF statements

Ciclo completo:
Request -> Controller -> Model -> View -> Response

## Creating and Configuring Models
Escrever `prop` e TAB gera `public int MyProperty { get; set; }`

Relação de 1 Album para muitas Reviews:

```
public class Album
{
    public virtual List<Review> Review { get; set; }
}
```

Podemos usar DataTypes para validar, desta maneira (altera de imediato o HTML do form):

```
[DataType(DataType.EmailAddress)]
public string ReviewerEmail  { get; set; }
```

Também podemos adicionar nomes específicos para as labels dos forms e tornar campos "Required":

```
[Required(ErrorMessage = "The email address is required")]
[Display(Name="Email Address")]
public string ReviewerEmail  { get; set; }
```

## The Power of Visual Studio

**Browser Link:** O Inspector do IE está ligado ao Visual Studio - abre a view correcta enquanto inspeccionamos no browser.

Podemos editar conteúdo no browser e ficará automaticamente alterado no ficheiro do Visual Studio.

[Web Essentials](http://vswebessentials.com/):

1. (F12 Auto Sync) Alteramos algo no CSS no inspector, actualiza o ficheiro CSS no Visual Studio
2. Tem um "Recorder" de unused CSS
3. Suporta Zen Coding
4. CSS tem Color Picker

Ver também o [SideWaffle](http://sidewaffle.com/) para adicionar snippets e templates ao Visual Studio.

## Customizing Controllers

ActionResult:
- FileResult
- JsonResult
- ViewResult (90% das vezes)

Para indicar quais os atributos que pretendemos fazer binding, ou criamos um View model ou:
`Edit([Bind(Include = "SongID, Title, Length")] Song song)`

Filtros - ciclo completo:
User Request -> MVC Controller -> Pre-execution Filter Code -> Action is Executed -> Post-execution Filter Code -> Model is combined with the View -> HTML is returned

"When in doubt enable SSL"

User roles: `[Authorize(Roles="Dinner")]`

Para Vanity URLs: utilizar o Attribute Routing com RouteAttribute

`[RoutePrefix("Album")]`
`[Route("Album/{id:int}")]`

## Customizing Views

Considerando este exemplo:

```
public class AlbumController : Controller
{
  public ActionResult Index()
  {
    return View("Default");
  }
}
```

Vai procurar a view "Default" na pasta "Album" e **se não encontrar vai à procura na pasta "Shared" antes de dar erro**

Razor Syntax - @ - para inserir server-side code na view (eventualmente poderá ser necessário @@)

HtmlHelper and Lambdas - os Helper Methods precisam da propriedade e não do valor, ou seja: `@Html.DisplayFor(Model.Name)` passaria o valor de Name, `@Html.DisplayFor(m => m.Name)` já daria o resultado pretendido

Exemplos: ActionLink, DisplayNameFor, DisplayFor, HtmlHelper.BeginForm(), LabelFor, InputFor, DropDownList

BeginForm parameters: Action name, Controller name, Form method (Get/Post)

Layouts: RenderBody(), RenderSection(name, required), @section

## Introduction to Bootstrap

Blog resource: [James Chambers](http://jameschambers.com/about/bootstrapping-mvc.html)

[Bootswatch](https://bootswatch.com/) para Bootstrap themes

## Introduction to Authentication

ASP.NET Identity corre em MVC, Web Forms, Web API, etc

[Authorize()] para forçar login/autenticação

Startup.cs tem um ConfigureAuth(app) onde está a "magia"

OAuth dá sempre AppID e AppSecret, depois podemos ir a App_Start e StartupAuth inserir os dados nos campos respectivos

Podemos adicionar campos como data de nascimento do user etc adicionando esses campos em Models/IdentityModels.cs e actualizando a view

Novo no Identity 2.0:

- Two-Factor Authentication
- Account Lockout
- Account confirmation
- Password reset
- Sign-out everywhere
- Enhanced password validator
- IQueryable for users and roles

# [Developing ASP.NET MVC 4 Web Applications Jump Start](https://mva.microsoft.com/en-US/training-courses/developing-aspnet-mvc-4-web-applications-jump-start-8239?l=fwYCcoJy_2404984382)

## Introduction to MVC

Models encapsulate objects and data
Views generate the user interface
Controllers interact with user actions

## Developing ASP.NET MVC Models

Considerando este código:

```
[Required(ErrorMessage = "{0} is required")]
[Display(Name = "Speaker Name")]
public String Name { get; set; }
```

Escrevendo `{0}` garante que ele chamará o nome da variável presente em `[Display]`

Partial/"Buddy class":

```
[MetadataType(typeof(SpeakerMetadata))]
public partial class Speaker
```

Exemplo de um DbContext:

```
namespace Conference.Models
{
  public class ConferenceContext : DbContext
  {
    public DbSet<Session> Sessions { get; set; }
    public DbSet<Speaker> Speakers { get; set; }
  }
}
```

O `virtual` funciona como foreign key e permite lazy loading entre os 2

`public class ConferenceContextInitializer : DropCreateDatabaseAlways<ConferenceContext>` se quisermos começar sempre de novo na database

Workflows possíveis Entity Framework: Code First, Database First, Model First.

## Developing MVC Controllers

**Writing Controller Actions:**

- Create public method
- Return a class that derives from ActionResult
- Add parameters to the method
- Insert code to perform the operation and return the result (aka DO STUFF)

```
public ActionResult GetSessionByTitle(string title) {
  DO STUFF
  return View("Details", session)
}
```

**Filters:**

Útil para Authorization/Logging/Caching

4 tipos diferentes de filtros:

- Authorization filters (eg: [Authorize(Roles = "Administrators")]) - run before any other filter and before the code in the action method
- Action filters - run before and after the code in the action method
- Result filters - run before and after a result is returned from an action method
- Exception filters - run only if the action method or another filter throws an exception

## Developing ASP.NET MVC Views

Exemplo de um `foreach` numa view:

```
@model IEnumerable<MyWebSite.Models.Product>
<h1>Product Catalog</h1>
@foreach (var Product in Model)
{
  <div>Name: @Product.Name</div>
}
```

**Razor syntax:**

- `@` para identificar server-side C# code
- `@@` para fazer render de uma @ numa página HTML
- `@:` para declrar uma linha de texto como conteúdo e não código
- `<text>` para declarar várias linhas de texto como conteúdo e não código

Para fazer render de texto sem HTML encoding, `HTML.Raw()`

**Action Helpers:**

```
HTMl.ActionLink()
URL.Action()
HTML.DisplayNameFor()
HTML.DisplayFor()
HTML.BeginForm()
HTML.LabelFor()
HTML.EditorFor()
HTML.ValidationSummary()
HTML.ValidationMessageFor()
```

**Partials/Shared:**

Ficheiro `_Layout.cshtml`

```
Html.Partial
Html.RenderPartial
Html.RenderAction
RenderView
Styles.Render
Scripts.Render
```

## Integrating JavaScript and MVC 4

**AJAX:**

1. Create your web application without AJAX
2. Add or modify views, to render only the specific sections that you want to update on the webpage
3. Update the `ViewController` class to return the `PartialView` class

Exemplo:

```
public PartialViewResult _CommentForm(int32 sessionID)
{
  Comment comment = new Comment() { SessionID = sessionID };
  return PartialView("_CommentForm", comment);
}
```

Na View:

```
@*@using(Html.BeginForm("_Submit", "Comment", FormMethod.Post)) {*@
  @using(Ajax.BeginForm("_Submit", "Comment", new AjaxOptions() { UpdateTargetId = "comments"})
    @Html.AntiForgeryToken()
    @Html.Action("_CommentForm", new { SessionID = ViewBag.SessionID })
}
```

BundleConfig.cs é onde se colocam os scripts e depois para os chamar basta `@Scripts.Render("~/bundles/jquery")`

O NuGet Package Manager também tem packages de JavaScript.

## Implementing Web APIs

To create a Web API for an MVC4 application:

1. Implement a Web API template in your project
2. Add an MVC API controller class to the project
3. Add action methods to the controller class

Web API devolve em JSON ou XML

## Deploying to Windows Azure

Requirements:

1. ASP.NET Framework 4.0
2. MVC 4 runtime
3. Database server
4. Entity Framework
5. Membership Providers

Com Azure:

1. Create new web application
2. Download a publishing profile
3. Start the Publish wizard and import the publishing profile
4. Configure connection strings
5. Observe that Microsoft Visual Studio publishes the web application on Windows Azure

Web.config tem 2 overrides:

1. Web.Debug.config
2. Web.Release.config

Publish Profile traz as passwords.

Com Entity Framework Code First, o deploy só corre bem com Code First Migrations.

`enable-migrations` na consola (cria um "Migrations" folder)
`add-migration "Initial Migration to have fun"`
`update-database -verbose` (-verbose mostra o que está a acontecer)

Recomendado: começar com as migrações ASAP

## Visual Studio 2013/MVC 5

SignalR => Web Sockets

O SignalR gera o JavaScript necessário

# Implementing Entity Framework with MVC

## Introduction to Entity Framework

Entity Framework = Object Relational Mapping

1. Maps your database types to your code types
2. Avoids repetitive data access code

EF6 requires Visual Studio 2010 or greater - instala-se via NuGet e faz parte do MVC, Web Forms e Web API templates com Identity

`Install-Package EntityFramework`

## Beginning Code First

## Managing Relationships

## Managing the database

## Managing transactions

## Integrating Extra Features and Looking Forward
