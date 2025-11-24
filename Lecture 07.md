









## Opakování - **Abstraktní třídy**























## Práce s vyjímkami

- Při programování **nelze** spoléhat na to, že vše půjde jak doufáme

```java

System.out.println("Zadejte číslo:");

Scanner in = new Scanner(System.in);
String line = in.nextLine();

if(Interger.isParseableToInt(line)) {
	int result = Integer.parseInt(line);
	System.out.println("Cislo je: " + result);
} else {
	sout("Error, zadej cislo!!!")
}




```

```java

var rectangle = new Rectangle(-10.0f, -20.0f);

```

- Takovým situacím se lze jen těžko vyhnout
- Další příklady:
	- Program se pokusí číst soubor, který:
		- neexistuje
		- nemá na něj oprávnění
	- Program komunikuje přes web a vypadne spojení



- Potřebujeme mechanismus na odchytávání chyb, který nám umožní na ně reagovat
- Tímto mechanismem je **systém vyjímky**
	- **Vyjímka** je objekt, který popisuje chybu, která nastala
		- *dědí* ze třídy `Exception`
	- Pokud detekujeme chybu (v konstruktoru `Rectangle` ověříme, že hodnoty jsou `>= 0`), můžeme instanci vyjímky **vyhodit** / `throw`
	- To znamená:
		- aktuální metoda se přestane vykonávat
		- chod se vrátí do metody, která metodu volala
		- ta musí buď:
			- znova vyhodit vyjímku, a tedy přenechat její vyřešení metodě, která ji zavolala
			- vyjímku ošetřit pomocí `try-catch`

```java

public class BadArgumentException extends Exception {
	// ponecháme zatím prázdnou
	String whichArg;
	
	public BadArgumentException(String whichArg) {}
}

// ...

class Rectangle {
	public Rectangle(float width, float height) {
		// následně můžeme ošetřit špatné hodnoty parametrů
		if(width <= 0.0f || height <= 0.0f) {
			// vytvoříme objekt vyjímky
			var exception = new BadArgumentException();
			
			// vyhodíme vyjímku
			throw exception;
		}
	
		this.width = width;
		this.height = height;
	}
}

```



```java

var rect = new Rectangle(-10.0f, 15.0f);

```

```java

try {
	var rect = new Rectangle(-10.0f, 15.0f);
} catch(Exception e) {
	System.out.println("test");
} catch(BadArgumentException exception) {
	exception.print();
	System.out.println("Špatné hodnoty");
} catch(AnoterException exception) {
	System.out.println("jiná vyjímka");
} 

```

### try-catch

- za klíčovým slovem `try` následuje **blok kódu**
- pokud v **bloku kódu** nastane vyjímka, provádění kódu se přeruší, a začne se provádět kód v části `catch`
- Za klíčovým slovem `catch` se uvádí **typ** vyjímky, kterou chceme chytnout
- Díky tomu můžeme na různé chyby reagovat různě
- Chování, že se průběh metody přeruší a v podstatě se state `return`, je žádaný
	- Pokud není možné vykonat jednu z operací, není možné vykonat zbytek


### finally

- Co kdybychom potřebovali něco udělat pokaždé, ať se stane co se stane


```java

try {
	var rect = new Rectangle(-10.0f, 15.0f);
	zivotneDulezitaFunkce();
} catch(BadArgumentException exception) {
	zivotneDulezitaFunkce();
	System.out.println("Špatné hodnoty");
} catch(AnoterException exception) {
	zivotneDulezitaFunkce();
	System.out.println("jiná vyjímka");
}

```

- Můžeme použít klíčové slovo `finally`. Blok za ním se vykoná vždy


```java

try {
	var rect = new Rectangle(-10.0f, 15.0f);
} catch(BadArgumentException exception) {
	System.out.println("Špatné hodnoty");
} catch(AnoterException exception) {
	System.out.println("jiná vyjímka");
} finally {
	zivotneDulezitaFunkce();
}

```












## Balíčky

- Větší projekty mohou mít **tisíce** nebo i **desítky tisíc** tříd
- Při takovém rozsahu nelze mít všechny soubory v jedné složce
- Natož v jednom souboru

- Pokud navíc používáme knihovny, mohou se názvy tříd objevovat vícekrát:
	- Například knihovna bude obsahovat také třídu `Rectangle`
	- Nelze se spoléhat na knihovny, že budou mít OD NÁS různé názvy

- Naštěstí v Javě existují tvz. **balíčky**
	- Z pohledu souborového systému odpovídají jednotlivým složkám
	- Z pohledu programu určují přesné jméno třídy
- Např. třída `Foo` uložena ve složce `src/cz/slachta/jj1/lecture07/Foo.java`
	- Z pohledu programu se třída nachází v balíčku `cz.upol.jj1.lecture07`
	- To je nutné deklarovat i v kódu:

```java

package cz.upol.jj1.lecture07;

class Foo {
	// ...
}

```

- Pokud by složka, ve které třída je, nesouhlasil s balíčkem, dostaneme chybu při sestavení

> Jako název balíčku se obvykle volí doména organizace, kde projekt vznikl 
> Případně i název projektu
> Chceme, aby název balíčku byl co nejunikátnější
















## Práce se soubory

- Třídy pro práci se soubory jsou v balíčku `java.io`
- Tyto třídy lze rozdělit do kategorií:
	- Práce s binárními soubory
	- Práce s textovými soubory

### Práce s textovými soubory



- Pro zápis:
	- třída `Writer`
		- obsahuje metodu `write` pro zápis řetězců nebo znaků
	- třída `Reader`:
		- obsahuje metodu `read` pro čtení znaků ze souboru
- Třídy `Writer` a `Reader` jsou abstraktní
	- Implementace poskytují třídy: `FileWriter` a `FileReader`, kterým už předáme cestu k souboru
- rozhraní `read` a `write` je dost omezující
	- Proto existují třídy, které toto rozhraní **rozšiřují** 
		- Např. `PrintWriter`, která obsahuje metodu `print`
	- Popř. třída `BufferedWriter` a `BufferedReader` optimalizují zápis a čtení ze souboru
		- Nezapisují ihned, ale bufferují si zápisy, a poté zapíšou na disk větší buffer



- Vytvořme třídu, která drží informace o zaměstnanci, a zkusme ji zapsat do souboru

```java

public record Employee(String name, int age, double salary) { }

Employee[] employees = {
		new Employee("Alice", 20, 25000),
		new Employee("Bob", 42, 145000),
		new Employee("Chuck", 37, 40230.56)
};

// deklarujeme mimo `try` blok, abychom se k nim dostali i z `catch` a `finally` bloku
FileWriter writer = null;
PrintWriter pwr = null;

try {
	// konstruktor s cestou k souboru už chceme zavolat v `try` bloku
	writer = new FileWriter("/tmp/employees.csv");
	pwr = new PrintWriter(writer);

	for(Employee emp : employees) {
		pwr.print(emp.name());
		pwr.print(",");
		pwr.print(emp.age());
		pwr.print(",");
		pwr.print(emp.salary());
		pwr.println();
	}
}
// konstruktor `FileWriter` obsahuje `throws IOException`, proto jsme nuceni vyjímku vyhodit
catch (IOException e) {
	System.out.println("Unable to write a file.");
	e.printStackTrace();
}
// větev `finally` se vykoná vždy -> ať už jsme dostali vyjímku nebo ne
// ideální pro zavírání souborů, se kterými jsme pracovali (chceme je zavřít, ať se stalo cokoliv)
finally {
	try {
		// objekty, které pracují se soubory je třeba zase zavřít
		if (pwr != null) pwr.close();
		if (writer != null) writer.close();
	}
	// metoda close má opět `throws IOException`
	catch (IOException e) {
		System.out.println("Unable to close a file.");
		e.printStackTrace();
	}
}

```



- větev `finally` budeme psát velmi často
- Proto existuje syntaktický cukr:
	- můžeme, mezi klíčovým slovem `try` a jeho tělem, vytvořit objekty, na které se automaticky zavolá metoda `close`
	- Všechny objekty musejí implementovat rozhraní `Closeable`


## Čtení ze souboru

- Čtení textu ze souboru vypadá dost podobně
- Rozhraní, které bychom asi od implementace `Reader` očekávali, je `readLine`
- Tuto metodu obsahuje třída `BufferReader`

```java

try(Reader reader = new FileReader("/tmp/employees.csv");
	BufferedReader brd = new BufferedReader(reader)) {

	String line;

	line = brd.readLine();
	while(line != null) {
		String[] parts = line.split(",");
		String name = parts[0];
		int age = Integer.parseInt(parts[1]);
		double salary = Double.parseDouble(parts[1]);

		Employee employee = new Employee(name, age, salary);
		System.out.println(employee);

		line = brd.readLine();
	}

} catch(IOException e) {
	System.out.println("Unable to read a file.");
	e.printStackTrace();
}

```























## Práce s binárními soubory

- opět máme dvě abstraktní třídy: `StreamReader` a `StreamWriter`
- Narozdíl od stringů, pracujeme s `byte`s

- Pro zápis a čtení bytů do a ze souborů, použijeme potomky `FileStreamWriter` a `FileStreamReader`

- Abychom nemuseli pracovat pouze s čistými byty, můžeme použít:
	- `DataOutputStream` a `DataInputStream`
	- obsahují metody jako `writeInt`, `readDouble` apod. 

```java

try (DataOutputStream dos = new DataOutputStream(output)) {
	for (Employee emp : employees) {
		dos.writeUTF(emp.name());
		dos.writeInt(emp.age());
		dos.writeDouble(emp.salary());
	}
}

```



## Práce se soubory s objekty

- Tuto funkcionalitu, kterou jsem si tu nasimulovali (můžeme zaměstnance ukládat a číst ze souboru), už nám Java přímo nabízí
- Třída `ObjectOutputStream` obsahuje metodu `writeObject`, která zapíše objekt binárně do `OutputStream`
- Třída `ObjectInputStream` obsahuje metodu `readObject`, která přečte objekt z `InputStream`

- aby tyto třídy uměli takto s objektem pracovat, musí náš objekt implementovat rozhraní `Serializable`
	- Výhoda je, že toto rozhraní nemá žádnou metodu
	- Slouží jen jako signalizace, že je objekt možné převést do binární podoby


```java

try (OutputStream output = new FileOutputStream("/tmp/employees.obj");
	ObjectOutputStream oos = new ObjectOutputStream(output)) {
	oos.writeObject(employees);
}

try (InputStream input = new FileInputStream("/tmp/employees.obj");
	ObjectInputStream ois = new ObjectInputStream(input)) {
	Employee[] employees = (Employee[]) ois.readObject();
	System.out.println(Arrays.toString(employees)); // nebo jina vhodna operace
}

```