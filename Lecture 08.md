












# Komentáře a jednotkové testy

- Začneme písemkou
- Je možné získat až 5 bodů
	- Ti co už mají 3 body dnes mohou psát poslední písemku
























# Komentáře

- Program by měl být nejen funkční, ale i srozumitelný
- Málokdy napíšeme program, ke kterému se už nikdy nevrátíme
	- Většinou je potřeba program eventuélně rozšířit, upravit, apod.
	- Může se upravit prostředí, ve kterém program používáme

- Důležité je **zvolit názvy tříd, metod a promněnných**
- Pokud kód pořád není dost srozumitelný, jsou na řadě **komentáře**

```java

// Jednořádkový komentář, kde je vše až do konce řádku ignorováno

/* Víceřádkový komentář,
   pro delší texty 
   např. licence */  

```

- **JavaDoc** komentáře - speciální případ víceřádkového komentáře
	- Dokumentující komentáře
	- Lze z nich vygenerovat funkční dokumentaci formou webové stránky
	- Náš editor je umí číst a při použití metody nám ukáže jí příslušný komentář
-  použijeme Markdown (tři lomítka) nebo lomítko a dvě hvězdičky
	
```java

/**
 * Writes employees in to the binary output stream.
 * @param output Open valid output binary stream
 * @param employees Array of employees
 * @throws IOException If the output is closed or become closed while writing.
 * @see Files#writeEmployeesBinary(OutputStream, Employee[])
 * @author Jim Hacker
 * @since 0.7 (lecture 07)
 */
public static void writeEmployeesBinary(OutputStream output, Employee[] employees) throws IOException {


```

- JavaDoc může obsahovat i **tagy**
	- ty blíže specifikují, co se popisujeme
- Např.:
	- `@param` - popis parametru metody
	- `@throws` - popis vyjímky, kterou metoda může vyhodit (proč a kdy)
	- `@see` - odkaz na dodatečné informace
	- `@author` - autor metody. Kdyby Vám nebylo něco jasné, víte, koho se můžete zeptat
	- `@since` - od které verze je metoda k dispozici (můžete cílit na nižší verzi)


- Dokonce lze používat i HTML tagy (vygenerovaná dokumentace bude do HTML)

```java

/**
 * Metoda převede řetězec skládající se z textové reprezentace čísla ve <b>dvojkové</b> soustavě na hodnotu typu <code>int</code>.
 */

```


## Shrnutí

- Komentář by měl vysvětlovat to, co daná část kódu **dělá** - nevysvětlujte **jak**
- Především by měl obsahovat informace, které nejsou na první pohled zjevné

```java

public class Car {
	/** registrační značka */
	private String regNo;
	/** registrační značka dle zákona 361/2000 Sb., o provozu na pozemních komunikacích */
	private String regNo;
}

```

- V prvním případě je komentář zbytečný
	- měl by obsahovat důležité informace

> Žádný komentář je lepší než chybný nebo zastaralý komentář

- Pokud je třeba psát dlouhý komentář, je to náznak toho, že bychom měli kód rozdělit do více metod
- Pokud je v komentáři těžké něco vysvětlit, je to náznak, že bychom měli kód zjednoduššit

- Je-li třeba psát komentář dovnitř metod, měli bychom část raději abstrahovat do metody:

```java
// pokud ma uzivatel nejakou slevu
if (((user.getAge() < 26) && user.isStudent()) || (user.getAge() > 60)) { }

if (hasDiscount(user)) {}

```


















## Testování

- Už jste to dělali
- Java nám k tomu jen **navíc** poskytuje užitečné nástroje
- Často se testy snažíme automatizovat
	- Proklikat si to je fajn, ale jistotu vám to nedá
	- Pokud napíšete automatický test jednou, tak víte, že daná věc už bude fungovat správně pořád
		- Před velkým refactoringem kódu si můžete napsat testy

## Jednotkové testy

- Testují nejmenší jednotky programu (metody, případně třídy)
	 - Zavoláme metodu se zadanými parametry
	 - Testujeme, jestli návratová hodnota odpovídá očekávané hodnotě

```java

double result = Math.sqrt(4);
if(result != 2.0) {
	System.out.println("Program funguje ŠPATNĚ");
} else {
	System.out.println("Program funguje DOBRE");
}
```

### JUnit

- Framework, který nám usnadňuje práci s testy
- Testy jsou testy v konkrétních třídách
	- Jednotlivé metody jsou testy
	- V metodách můžeme klasicky:
		- Vytvářet instance tříd
		- volat metody
		- ověřovat, že hodnoty jsou správné (`assert`)

```java

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertThrows;

public class SquareTests {

    @Test
    public void perimeterTest01() throws BadValueException {
        Square square = new Square(10.0f);

        assertEquals(square.getPerimeter(), 40.0f);
    }

    @Test
    public void areaTest01() throws BadValueException {
        Square square = new Square(10.0f);

        assertEquals(square.getArea(), 100.0f);
    }
}

```



```java

class CarTest {
	private Car car;
	
	@BeforeEach
	public void setUp() {
		car = new Car("AAA 1234", "blue");
		car.accelarate(10);
	}
	
	@Test
	void accelerateTest01() {
		car.accelarate(10);
		car.accelarate(15);
		assertEquals(25, car.getSpeed());
	}
	
	@Test
	void brakeTest01() {
		car.accelarate(10);
		car.brake(6);
		assertEquals(4, car.getSpeed());
	}
	
	@Test
	void brakeTest02() {
		car.accelarate(15);
		car.brake(20);
		assertEquals(0, car.getSpeed());
	}
}
```







