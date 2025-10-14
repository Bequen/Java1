









# Rozhraní a polymorfismus

## https://github.com/Bequen/Java1























## Písemka

- Odkládá se na příště
- Bude ale tím pádem zahrnovat i věci ze 3. lekce


































# Rozhraní a Polymorfismus

- Už víme, co je to zapozdření
	- Všechny atributy jsou skryty -> ostatní objekty k nim nemají přístup
	- Ostatní objekty MUSEJÍ použít volání metod:
		- V objektu `Car` neupravují atribut `speed`, ale volají `addGas`
		- Nenastavují atribute `passenger[0] = somePerson;`, ale volají `getIn(somePerson)`
- Veřejné metody představují pomyslné **rozhraní**
- Rozhraní je důležité --> **nemusíme vědět, co se děje uvnitř**
	- Umíte řídit auto, i bez toho, aniž byste znali, jak funguje






















### Rozhraní

- Soubor všech veřejných metod, který daný objekt má
- Zároveň také prvek jazyka: `interface`
	- Umožňuje přesně určit, jaké metody má objekt mít


```java

// Zakladní rozhraní objektů typu Car

class Car {
	public void turnLeft(float degrees) {
		// ...
	}
	
	public void turnRight(float degrees) {
		// ...
	}
	
	public void speedUp(float percentage) {
		// ...
	}
	
	public void slowDown(float percentage) {
		// ...
	}
}

```

Kdybychom ale potřebovali trochu přesnější modely, např.:

```java

class Truck {
	private float maxWeight;

	public void turnLeft(float degrees)
	
	public void turnRight(float degrees)
	
	public void speedUp(float percentage)
	
	public void slowDown(float percentage)
}

class SUV {
	public void turnLeft(float degrees)
	
	public void turnRight(float degrees)
	
	public void speedUp(float percentage)
	
	public void slowDown(float percentage)
}

class Offroad {
	private bool is4x4;

	public void turnLeft(float degrees)
	
	public void turnRight(float degrees)
	
	public void speedUp(float percentage)
	
	public void slowDown(float percentage)
}


```

Zjistíme, že objekt libovolného typu se ovládá stejně.
- Rozhraní je stejné
	- Atributy se klidně mohou lišit
- Nemusíme tedy znát to, jestli se jedná o SUV nebo Offroad
	- Pokud umíme používat `turnLeft`, `turnRight`, ... a potřebujeme pouze řídit, tak nám toto rozhraní stačí
- V reálném světě to znamená, že Vám je jedno jestli se jedná o Červené Škoda CityGo nebo Modrý Mercedes Benz
	- Pokud mají stejné rozhraní: **volant**, **pedály**, apod., tak dané auto umíte řídit


```java

Mercedes mercedes = new Mercedes();

Skoda skodovka = new Skoda();

// mercedes i skodovka se ovladaji stejne
mercedes.speedUp(100.0f);

skodovka.speedUp(100.0f);

```

Z trochu reálnějšího soudku:

```java

JpgCompressor jpg = new JpgCompressor();

jpg.load("/path/to/image.jpg");
jpg.compress();




PngCompressor png = new PngCompressor();

png.load("/path/to/image.png");
png.compress();

```


### Definice `interface` 

- Hodilo by se definovat vlastnost `Driveable` nebo `Compressor` a to, jaké metody musí třída mít, aby tyto vlastnosti měla
- Díky tomu se můžeme na Černou Škodovku Fabia i na Mercedes Benz koukat jako na objekt, který je `Driveable` a využívat metody, které toto rozhraní má
- K tomu slouží definice `interface` v Javě

```java

interface Driveable {
	void turnLeft(float degrees);
	
	void turnRight(float degrees);
	
	void speedUp(float percentage);
	
	void slowDown(float percentage);
}

// Pomocí klíčového slova `implements` a názvu rozhraní oznámíme,
// které rozhraní bude objekt implementovat

// Musíme implementovat VŠECHNY metody z rozhraní -> jinak nám Java nedovolí program sestavit
// Implementaci začínáme `@Override` -> "Původní metodu přepiš"

class SkodovkaCityGo implements Driveable {
	@Override
	public void turnLeft(float degrees) {
		// ...
	}
	
	@Override
	public void turnRight(float degrees) {
		// ...
	}
	
	@Override
	public void speedUp(float percentage) {
		// ...
	}
	
	@Override
	public void slowDown(float percentage) {
		// ...
	}
}

class MercedesBenz implements Driveable {
	@Override
	public void turnLeft(float degrees) {
		// ...
	}
	
	@Override
	public void turnRight(float degrees) {
		// ...
	}
	
	@Override
	public void speedUp(float percentage) {
		// ...
	}
	
	@Override
	public void slowDown(float percentage) {
		// ...
	}
}

// ...

{
	// Objekt je typu našeho rozhraní
	// Díky tomu mu můžeme přiřadit referenci na libovolný objekt, který toto rozhraní implementuje
	Driveable car = new MercedesBenz();
	
	car = new SkodovkaCityGo();
}

// ...

// Umíme vytvořit metodu, která používá rozhraní a je jí jedno, co je to za objekt (dokud dané rozhraní implementuje)
void driveToCity(Driveable what) {
	what.turnLeft(90.0f);
	what.speedUp(100.0f);
	
	what.slowDown(80.0f);
	what.turnRight(90.0f);
	
	// ...
}

```

