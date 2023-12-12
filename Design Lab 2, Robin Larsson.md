# Designlab 2, Robin Larsson
<!-- 
- I din PDF ska du lista koderna A till D och för varje först skriva vilket designmönster du anser att varje kod är ett exempel på. Sen ska du också skriva varför du anser detta. Plocka ut någon del eller några delar av koden som du tycker visar på att det är just det designmönster du anser. -->

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
}```
