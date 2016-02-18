# [Introduction to ASP.NET MVC](https://mva.microsoft.com/en-US/training-courses/introduction-to-aspnet-mvc-8322?l=nKZwZ8Zy_3504984382)

## Intro:
Nas Views não se deve colocar qualquer lógica, como por exemplo IF statements

Ciclo completo: Request -> Controller -> Model -> View -> Response

## Models:
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

## The Power of Visual Studio:
