








# Objektové programování v Java


- Minule jsme udělali nástřel objektového programování
- Procedurální programování je příliš **zdlouhavé** a **nebezpečné**
- V objektovém programování náš program skládáme z objektů
- Objekty se mohou skládat z jiných objektů
- Rozdíl je v tom, že nad objekty se daleko snáz uvažuje

























## Co by měl takový objekt v Javě mít?

- Vlastnosti
	- Věci z reálného světa mají také vlastnosti
		- **Auto** má barvu, značku, reg. značku, cenu
		- **Džus** má příchuť, značku, cenu
		- **Faktura** má splatnost, cenu, plátce, bankovní účet
	- Ty budeme reprezentovat daty -> ukládat je v paměti
		- Každá vlastnost tak bude mít datový typ

- Kód, který s vlastnostmi pracuje
	- Tzv. **metody**
	- **Metoda** je posloupnost příkazů, která pracuje s vlastnostmi objektu
		- -> mění, nebo je čte
	- Metoda je **povel**, který má daný objekt vykonat
	- Džus může mít povel: 
		- `expire` 
			- -> nastaví vlastnost `příchuť = "nechutná";`
			- -> nastaví vlastnost `cena = 0;`
	- Zvenku volám jednotlivé povely
		- Nezajímá mě jejich implementace

- Vlastnosti + metody = členové třídy











## Třída

- Třída je **šablona, která definuje podobu objektu**
- Určuje: 
	- **data**
	- **kód, který s daty pracuje**
- Jazyk Java pomocí definice třídy konstruuje objekty
	- Objektu se proto říká **instance třídy**

> Třídu můžeme chápat jako výkres a objekt jako finální výrobek. Podle jednoho výkresu můžeme vyrobit libovolný počet výrobků.
> -> Zároveň bez jediného vyrobeného výrobku víme, jak bude vypadat a jaké bude mít vlastnosti


















- Třída je deklarována pomocí klíčového slova `class`

```

class <class_name> {
	<typ> <vlastnost1>;
	<typ> <vlastnost2>;
	// ...
	<typ> <vlasnostN>;
	
	<návratový-typ> <název-metody-1>(<list-parametrů>) {
		// tělo metody
	}
}

```

```java

// definice třídy Auto
class Auto {
	// vlastnosti
	int pocetCestujicich;
	double objemNadrze;
	double litryNa100Km;
}

```

- Tato definice ale zatím žádný objekt nevytvořila -> to musíme udělat my pomocí:

```java

// vytvoří instanci třídy Vozidlo s názvem minivan
Auto minivan = new Auto();
	
// promněnná `minivan` s datovým typem `Auto`

```


```java

public static void main(String[] args) {
	// vytvoří instanci třídy Vozidlo s názvem minivan
	Auto minivan = new Auto();

	minivan.objemNadrze = 60; // k atributum pristupujeme pres tecku
	minivan.pocetCestujicich = 7;
	minivan.litryNa100Km = 10;



	// Podle jedné třídy můžu vytvořit objektů kolik chci
	Auto sportak = new Auto();

	sportak.objemNadrze = 40; // k atributum pristupujeme pres tecku
	sportak.pocetCestujicich = 2;
	sportak.litryNa100Km = 20;
	
	

	// vyraz, ktery vypocita dojezd
	double dojezdMinivan = 100 * (minivan.objemNadrze / minivan.litryNa100Km);
	double dojezdSportak = 100 * (sportak.objemNadrze / sportak.litryNa100Km);

	// Vypis vysledek
	System.out.println("Minivan má dojezd " + dojezdMinivan + " km");
	System.out.println("Sporťák má dojezd " + dojezdSportak + " km");
}

```



### Metody

- Prvky tříd
- Povely, které objekt umí vykonávat
- Posloupnost příkazů, která pracuje s daty objektu

```java

class Auto {
	// vlastnosti
	int pocetCestujicich;
	double objemNadrze;
	double litryNa100Km;
	
	double dojezd() {
		return 100 * (objemNadrze / litryNa100Km);
	}
}

```


- **Návratové typy**
	- Metody mohou vracet hodnotu
	- `dojezd` vrací hodnotu typu `double` pomocí klíčového slova `return`











- **Parametry**
	- Metody mohou mít parametry
	- Např. přidejme metodu, která vypočítá potřebné palivo pro parametr `pocetKm


```java

class Auto {
	// vlastnosti
	int pocetCestujicich;
	double objemNadrze;
	double litryNa100Km;
	
	double dojezd() {
		return 100 * (objemNadrze / litryNa100Km);
	}
	
	// metoda s parametrem `pocetKm`
	double potrebnePalivo(double pocetKm) {
		return litryNa100Km * (pocetKm / 100);
	}
}

```



## Nekonzistentní stav

- Momentálně můžeme zvenku objektu upravovat hodnoty atributů

```java

Auto minivan = new Auto();

// snadno dostaneme objekt do nekonzistentniho stavu
minivan.pocetCestujicich = -10;

// popr. tady je tikajici bomba
minivan.litryNa100Km = 0;

```



## Princip zapouzdření

- S hodnotami atributů objektu bychom měli pracovat výhradně přes metody
	- Metoda vyjadřuje logický a intuitivní povel
	- Metoda nám zajistí, že se objekt nedostane do nekonzistentního stavu
	- Případné úpravy atributů (přidání nebo odebrání atributu) bude potřeba zohlednit jen v metodách a nikde jinde
- Princip zapouzdření je jeden ze základních principů objektového programování
- V Javě proto používáme klíčová slova `public` a `private`
	- `public` označí člena třídy jako veřejný -> přístupný z venku přes tečku
	- `private` označí člena třídy jako soukromý -> přístupný pouze z metod téže třídy

![[Pasted image 20250930181501.png]]



```java

class Quad {
	private double width;
	private double height;
	
	// neumozni nastavit šířku ani výšku na zápornou hodnotu
	public void setWidth(double width) {
		if(width < 0.0) {
			// pokud máme atribut se stejným názvem jako parametr,
			// můžeme použít klíčové slovo `this` -> to značí aktuální 
			// instanci
			this.width = 0.0;
		} else {
			this.width = width;
		}
	}
	
	public void setHeight(double height) {
		if(value < 0.0) {
			this.height = 0.0;
		} else {
			this.height = value;
		}
	}
	
	public void setSize(double value) {
		if(value < 0.0) {
			// zde uz nemusime pouzit `this`
			width = 0.0;
			height = 0.0;
		} else {
			width = value;
			height = value;
		}
	}
}

// ...

Quad quad = new Quad();

quad.width = 10.0; // error
quad.setWidth(-10.0); // ok

```


## Konstruktor

```java

// initializace hodnot atributů může být takto dost zdlouhavá
Quad quad1 = new Quad();
quad1.setWidth(10.0);
quad1.setHeight(10.0);

Quad quad2 = new Quad();
quad2.setWidth(32.0);
quad2.setHeight(16.0);

```

- Naštěstí lze definovat metodu, která bude fungovat jako prvotní initializace
- Tzv. **konstruktor**

```java

class <nazev> {
	// není třeba definovat návratový typ ani název této metody
	<nazev>(<list-parametru>) {
		// tělo
	}
}

```


```java

class Quad {
	private double width;
	private double height;
	
	public Quad(double width, double height) {
		setWidth(width);
		setHeight(height);
	}
	
	public void setWidth(double value) {
		// ...
	}
	
	public void setHeight(double value) {
		// ...
	}
}

// ...

Quad quad1 = new Quad(10.0, 10.0);
Quad quad2 = new Quad();

```



## Reference

- Když vytvoříme instanci, do promněnné se uloží pouze reference na instanci

**Implikace**:

```java

Quad quad1 = new Quad(10.0, 10.0);

Quad quad2 = new Quad(32.0, 16.0);

		

```