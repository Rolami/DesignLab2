# Designlab 2, Robin Larsson, NET22, Design Patterns

## Code A

Jag tycker att det är Abstract Factory method som påvisas i denna kod. Kollar man på koden tycker jag att den visar att man har en grundmetod som kallar på varianter av huvudobjektet. Huvudobjektet Restaurant är också en abstrakt klass, vilket är en ledtråd.

Det jag menar är följande kod :

```Csharp
     public abstract class Restaurant //Här är klassen abstrakt
    {

        public abstract IMeal PrepareMainCourse(); //den har ett abstrakt interface

        public void OrderDailySpecial() //En standardmetod
        {
            Console.WriteLine("Drink: sparkling water");

            var mainCourse = PrepareMainCourse(); //Prepare main course som varierar beroende på restauruangtyp
            mainCourse.ShowDescription(); //Texten som ändras i beskrivningen
        }
    }
}
```

Följande kod är ett interface för att skapa beskrivning av måltiden.

```Csharp
    public interface IMeal
    {
        void ShowDescription(); //Interfacet har en method som ärv och fylls på
    }
}
```

 Här kan man se att klassen Hamburger ärver ShowDescription från IMeal och skriver över sin egna beskrivning av vad den måltiden innehåller.

```Csharp
    public class Hamburger : IMeal
    {
        public void ShowDescription()
        {
            Console.WriteLine("Hamburger - with beef meat, Worcestershire sauce and cheese.");
        }
    }
}
```

## Code B

Denna kod har många exempel på hur man gör Singleton.

DotNetLazyGreeter:
    Använder Lazy<T>-klassen för "lazy initialization" (trög start).

DoubleCheckGreeter:
    Implementerar dubbelkollslåsning för trådsäkerhet vid instansskapande.

LocklessFullyLazyGreeter:
     Använder en inbäddad klass för att uppnå "lazy initialization" på ett trådsäkert sätt.

LocklessGreeter:
    Initialiserar instansen statiskt.

SimpleGreeter:
    Använder en simpel strategi för "lazy initialization".

SimpleThreadSafetyGreeter:
    Implementerar trådsäkerhet med en låsning under instansskapande.

## Code C

Denna kommer med vad jag kan se, två patterns, Strategy pattern och Dependency injection.

När man kollar på koden ser man följande:
Strategy pattern:

Gränssnittet IShippingProvider:
    Representerar strategin för att beräkna fraktkostnader.
    Deklarerar en metod CalculateCost(Order order).

Klassen FedEx:
    Implementerar gränssnittet IShippingProvider.
    Ger en specifik implementation för beräkning av fraktkostnader med FedEx.

Klassen ShippingCostCalculationService:
    Representerar kontexten i Strategy-mönstret.
    Tar emot en IShippingProvider-instans i konstruktorn, vilket indikerar den specifika strategin för beräkning av fraktkostnader.

Användning i klassen ShippingExecutor:
    Skapar en instans av FedEx (en konkret implementation av IShippingProvider).
    Skapar en instans av ShippingCostCalculationService med den injicerade FedEx-instansen.
    Anropar metoden Calculate på ShippingCostCalculationService för att beräkna fraktkostnaden.

Strategy pattern används här för att kapsla in olika algoritmer för beräkning av fraktkostnader (representerade av olika implementationer av 'IShippingProvider'). Det tillåter klientkoden ('ShippingCostCalculationService') att kopplas loss från de specifika implementationerna av fraktleverantören.

Dependency injection:

Kollar man igen på klassen ShippingCostCalculationService kan man se att den använder konstruktorinjektion för att ta emot en instans av 'IShippingProvider'. Det ger en flexibel kod där man kan injicera olika implementationer av IShippingProvider vid körning, vilket främjar lös koppling.

Sneglar man också på 'ShippingExecutor' kan man se en demonstration av Dependency Injection genom att skapa en instans av ShippingCostCalculationService med en specifik implementation av IShippingProvider (i detta fall FedEx).

Det går också att se på koden att IShippingProvider med FedEx, RoyalMail och UnitedParcelService har använt factory pattern för att lätt återanvända grundmetoder.

SortablePersons verkar inne Decorator Pattern. Den omsluter en List<Person> och lägger till sorteringsfunktionalitet utan att ändra den ursprungliga klassen.

Sen kollar vi in på ISort och implementionerna SortByFirstName, SortByLastName, SortByYearOfBirth ser de ut att använda sig av Template Method Pattern. Template (i detta fall Sort) defineras i gränssnittet och implementeras i varje klass med specifik sorteringslogik.

Klassen SortablePersons i sorteringsexemplet fungerar som ett kommandomönster (Command Pattern). Den kapslar in en förfrågan om att sortera en lista över personer och gör det möjligt för klienten (SortingExecutor) att parametrisera med olika sorteringsstrategier.
## Code D

De design patterns jag kan hitta i denna kodbas är följande: Factory Method, Adapter Pattern, Template Method och Decorator. 

```Csharp
public static class BillingSystemExecutor
{
    // ... Har klippt bort lite för att spara textrader.

    public static void Execute()
    {
        // ... Har klippt bort lite för att spara textrader.

        ISalaryProcessor salaryProcessor = new HRSystem();

        salaryProcessor.ProcessSalaries(employeesInfo);
    }
}

```

Factory pattern: Klassen BillingSystemExecutor använder en Factory Method genom att skapa en instans av HRSystem genom new HRSystem() för att implementera ISalaryProcessor-gränssnittet.

```Csharp
public class HRSystem : ISalaryProcessor
{
    // ... Har klippt bort lite för att spara textrader.

    private List<Employee> PrepareEmployeesForSalaryProcessing(string[,] rawEmployees)
    {
        // ... Har klippt bort lite för att spara textrader.

        var employeesForProcessing = new List<Employee>();

        for (int i = 0; i < rawEmployees.GetLength(0); i++)
        {
            // ... Har klippt bort lite för att spara textrader.

            var employee = new Employee
            {
                Id = Convert.ToInt32(id),
                Name = name,
                Salary = Convert.ToDecimal(salary),
            };

            employeesForProcessing.Add(employee);
        }

        return employeesForProcessing;
    }
}
```

Adapter Pattern: Klassen HRSystem fungerar som en adapter genom att anpassa string[,]-formatet från det externa systemet till dess interna List<Employee>-format i metoden PrepareEmployeesForSalaryProcessing.

```Csharp
public class MovieRegistry
{
    // ... Har klippt bort lite för att spara textrader.

    public XDocument GetAll()
    {
        return GetAllFromDatabase();
    }

    private XDocument GetAllFromDatabase()
    {
        // ... Har klippt bort lite för att spara textrader.

        var xDocument = new XDocument();

        var xMoviesElement = new XElement("Movies");

        var xMovieCollection = GetRegistredMovies()
            .Select(movie => new XElement(
                "Movie",
                new XAttribute("Name", movie.Name),
                new XAttribute("ReleaseDate", movie.ReleaseDate),
                new XAttribute("Rating", movie.Rating)));

        xMoviesElement.Add(xMovieCollection);
        xDocument.Add(xMoviesElement);

        return xDocument;
    }

    // ... Har klippt bort lite för att spara textrader.
}
```

Template Method Pattern: Klassen MovieRegistry använder Template Method Pattern genom metoden GetAllFromDatabase, där den generella strukturen är definierad och specifika steg (GetRegistredMovies och skapandet av XML-element) lämnas åt subklasser (eller i det här fallet, samma klass i en annan metod).

```Csharp
public class Broadcast : IBroadcaster
{
    // ... Har klippt bort lite för att spara textrader.

    private string ConvertRegistryMoviesToJson()
    {
        XDocument xmlMovies = _movieRegistry.GetAll();

        // ... Har klippt bort lite för att spara textrader.

        IEnumerable<Movie> modelMovies = xmlMovies
            .Element("Movies")
            .Elements("Movie")
            .Select(movie => new Movie
            {
                Name = movie.Attribute("Name").Value,
                ReleaseDate = DateTime.Parse(movie.Attribute("ReleaseDate").Value),
                Rating = Convert.ToDouble(movie.Attribute("Rating").Value),
            });

        string jsonMovies = JsonConvert.SerializeObject(modelMovies, Formatting.Indented);

        return jsonMovies;
    }
}
```

Decorator Pattern: Klassen Broadcast lindar funktionaliteten från _movieRegistry.GetAll() genom att konvertera XML-filmer till JSON innan de sänds. Denna inlindning eller förbättring av beteendet liknar Decorator-mönstret, där ny funktionalitet läggs till en befintlig objekt (_movieRegistry).
