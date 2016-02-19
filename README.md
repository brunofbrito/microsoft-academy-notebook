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
