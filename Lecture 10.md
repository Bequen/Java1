







## Kolekce

- Příště bude písemka


























## Kolekce

- Pokud jsme chtěli mít soubor objektů, museli jsme použít pole
- To je velmi rychlé
	- Ale velmi neflexibilní
- Minule jsme si pomohli tak, že jsme pole obalili do třídy, která ho automaticky zvětšovala
- Takovým třídám se říká **kolekce** a v Javě je jich celá řada


















## Práce s kolekcemi

- V Javě je pro každý typ kolekce: seznam, množina, slovník, definováno rozhraní
	- `interface List, interface Set, ...`
- Pro rozhraní pak máme různé implementace
	- `class ArrayList implements List`
	- `class Stack implements List`
	- https://docs.oracle.com/javase/8/docs/api/java/util/List.html
- V našem kódu tak můžeme pracovat s rozhraními:

```java

List<T> removeDuplicates(List<T> list) { /* ... */ }

```















## Seznam

- Kolekce charakteristická tím, že prvky jsou uloženy za sebou
	- To umožňuje rychlí přístup pomocí indexu
	- Práce se seznamem je pružnější než práce s polem

```java

List<String> names = ...

// vlozi hodnotu na konec seznamu
names.add("alice");
names.add("bozena");
names.add("cyril");
names.add("david");
// [alice, bozena, cyril, david]
names.remove("bozena");
// [alice, cyril, david]
// vlozi hodnotu na pozici 1
names.add(1, "benedikt");
// [alice, benedikt, cyril, david]

names.size() // 4
names.get(2) // "cyril"
names.indexOf("david") // 3
names.contains("david"); // true
names.contains("eva"); // false

```


### ArrayList

- Uchovává prvky ve stejném duchu, jako seznam
- Vnitřně obsahuje pole, které podle potřeby zvětšuje

### LinkedList

- Formou spojového seznamu
- Metody pro `get(index)` budou pomalejší, než u `ArrayList`
- Naopak metody pro insert budou rychlejší

> Metody `contains`, `indexOf` nebo `remove` používájí metody `equals` k nalezení příslušného prvku.

## Foreach

- Prvky můžeme procházet i pomocí `for`

```java

List<String> names = new ArrayList<>();

for (String name : names) {
	System.out.println(name);
}

```


## `List.of`

- Pokud potřebujeme vytvořit kolekci s pěvně danými prvky

```java

List<Integer> numbers = List.of(1, 2, 3);
List<String> strings = List.of("abc", "cde");
List<Object> objects = List.of("abc", 123, true);

```
















## Slovník

- Slovníky ukládají prvky jako dvojice klíč-hodnota
	- Jeden **klíč** se může v kolekci vyskytovat právě jednou
- Funguje to stejně jako klasické **zobrazení**
	- Pro jeden vzor máte právě jeden obraz
	- Ke vzorům přiřazujeme obrazy
- Název rozhraní je `Map<K, V>`, v překladu **zobrazení**



### Hash Tabulka

- První implementací je třída `HashMap`
- Tabulka pevné velikosti
- Hodnoty jsou uspořádany pomocí hashovací funkce objektu `hashCode`
	- To je funkce, která vrátí náhodně vypadající hodnotu
	- Zpětně z ní jen těžko dopočítáme původní objekt
	- Dva objekty se stejnými hodnotami atributů mají stejný **hash code**

```java

Person person1 = new Person("Petr", "Novak", 1995);
Person person2 = new Person("Petr", "Pavel", 1995);

System.out.println(person1.hashCode()); // 13241235

Assert.True(person1 != person2);
Assert.True(person1.equals(person2));
Assert.True(person1.hashCode() == person2.hashCode());

```

- Hashovací tabulka má fixní velikost, například 50 řádků
- Jak z čísla třeba 21425125 (což je platný hash code) dostat index do tabulky?
	- Zbytek po celočíselném dělení



- Obrovská výhoda je rychlost
	- Klíč může být libovolný objekt (ne jen integer)
	- Zároveň ale není třeba procházet celé pole (rovnou víme index)


```java

Map<String, Double> g = new HashMap<>();
g.put("methan", 16.04);
g.put("ethan", 30.07);
g.put("propan", 44.09);
g.put("butan", 58.12);
g.get("ethan"); // ==> 30.07
g.get("hexan"); // ==> null (nepˇr´ıtomen)
g.containsKey("butan"); // ==> true
g.remove("methan");

```

- Ze zdrojového kódu hashovací funkce je patrné, že nemá žádný konkrétní význam

```java

public int hashCode() {
	return plateNo.hashCode() ^ color.hashCode() ^ (7 * speed);
}

```


> Předpokládáme, že když se změní hodnota atributu, změní se i hashcode

















## Slovník jako vyhledávací strom

- Druhou implementací `Map<K, V>` je `TreeMap<K, V>`
- Objekty této třídy uspořádají dvojice klíč-hodnota do **binárního stromu**
	- K tomu ale potřebujeme umět objekty porovnávat
		- Jak na to?























- Typ pro klíč tak musí implementovat rozhraní `Comparable<ToWhat>` s metodou `compareTo(T o)`
	- vrátí záporné číslo, pokud je objekt **menší** než `o`
	- vrátí kladné číslo, pokud je objekt **větší** než `o`
	- vrátí 0, pokud se objekty rovnají
```java

public class Car implements Comparable<Car> {
	/* porovnání na základě rychlosti */
	public int compareTo(Car o) {
		if (this.speed < o.speed) return -1;
		if (this.speed > o.speed) return 1;
		
		// jedou stejne rychle, proto porovname SPZ
		return this.plateNo.compareTo(o.plateNo);
	}
}

```



> Třídy jako Double, String, Integer, apod. už toto rozhraní implementují a odpovídá to přirozenému uspořádání hodnot. Řetězce jsou seřazeny lexikograficky





















- Co když ale typ nevlastníme a nemůžeme tak implementovat rozhraní?

```java
public class StringLengthComparator implements Comparator<String> {
	public int compare(String s1, String s2) {
		if (s1.length() < s2.length()) return -1;
		if (s1.length() > s2.length()) return 1;
		// jsou stejne dlouhe, proto je porovname
		// prirozene podle abecedy
		return s1.compareTo(s2);
	}
}

// ...

Map<String, Integer> m = new TreeMap<>(new StringLengthComparator());
m.put("abc", 10);
m.put("a", 2);
m.put("b", 3);
m.put("de", 15);
m.get("a") // 10
```





















## Množina

- Posledním typem je množina
- Nezachovává pořadí prvků
- Každý objekt se může v množině nacházet právě jednou


### Implementace

- `HashSet<V>` a `TreeSet<V>`
- Obě přímo vycházejí z `HashMap<K, V>` a `TreeMap<K, V>`
	- Pro hodnoty, které vkládáme do množin, platí stejná pravidla, jako pro klíče ve slovnících
- Pro množiny je charakteristické: průnik, sjednocení a množinový rozdíl
	- Protějšky v Javě jsou:
		- `addAll(s)` - přidá všechny prvky z množiny `s` (sjednocení)
		- `retainAll(s)` - ponechá pouze prvky, které jsou i v množině `s` (průnik)
		- `removeAll(s)` - odstraní objekty z množiny, které jsou v kolekci `s` (rozdíl)

-  Můžeme použít foreach, ale nemůžeme předpokládat žádné pořadí
```java

Set<Integer> nums = new HashSet<>();

for (int i = 0; i < 10; i++)
	nums.add(i * 10);

for (int i : nums)
	System.out.print(i + " ");
// 0 80 50 20 70 40 10 90 60 30

```


```java

Set<Integer> numbers = Set.of(1, 2, 3);
Set<String> strings = Set.of("abc", "cde");
Set<Object> objects = Set.of("abc", 123, true);

```