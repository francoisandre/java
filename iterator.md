# Singleton

## Problème
Parcourir un ensemble de données 

## Exemple
Supposons qu'une classe Thermomètre stocke les différentes valeurs de la température toute les heures dans une chaîne de caractères où chaque température est séparée par une virgule :

ex: 21,22,23,25,22,20 ...

### Approche brutale  
Si une classe, Graphique, veut tracer un graphique des températures une première approche est que Graphique récupère la chaîne des températures et la décompose en se basant sur la virgule afin de pouvoir effectuer le tracé.

### Limitations de l'approche brutale
L'approche brutale est limitée car elle va à l'encontre de l'encapsulation des données en programmation objet. En effet, avec cette approche il est nécessaire pour la (ou les ) classes utilisatrices de connaitre la manière interne dont Thermomètre stocke ses données.

Ainsi, le traitement de Graphique ne va plus fonctionner si Thermomètre change le caractère de séparation ou s'il change sa manière de stocker (utilisation d'un tableau).

De plus si une autre classe veut également récupérer les données, on peut s'attendre à ce qu'elle mette en place le même genre de code que Graphique et nous aurons sûrement des doublons.

### L'Iterator
Pour être le plus efficace possible, il faut que Thermomètre ne permettent plus l'accès directe à son mécanisme de données mais via un objet qui aura la seule responsabilité d’égrener les données. Cet objet va implémenter l'interface Itérator

## Implémentation

L'interface Iterator est la suivante : 

![enter image description here](https://raw.githubusercontent.com/francoisandre/java/master/images/iterator.png)

Elle est présente nativement dans Java (https://docs.oracle.com/javase/7/docs/api/java/util/Iterator.html) et est le moyen privilégié de parcourir les objets de type *List* ou *Set*. Cette version de l'Iterator utilise la notion de *generics* non encore étudiée. Elle rajoute également la méthode *remove()* qui est optionnelle.

### Diagramme UML
![enter image description here]

###La classe Thermomètre

    package designpatterns.iterator;
    
    import java.util.Iterator;
    
    public class Thermometre {
    
    	private String values = "";
    	private final static String SEPARATOR = ",";
    
    	public Thermometre() {
    
    	}
    
    	public Thermometre(int fakeValueNumbers) {
    		for (int i = 0; i < fakeValueNumbers; i++) {
    			addValue(randInt(20, 30));
    		}
    		System.out.println("The values are " + values);
    	}
    
    	public void addValue(int value) {
    		if (values.length() != 0) {
    			values = values + SEPARATOR;
    		}
    		values = values + value;
    	}
    
    	public static int randInt(int min, int max) {
    		return min + (int) (Math.random() * ((max - min) + 1));
    	}
    
    	public Iterator getChronologicalIterator() {
    		String[] aux = values.split(SEPARATOR);
    		return new ArrayIterator(aux);
    	}
    
    	public Iterator getAntiChronologicalIterator() {
    		String[] aux = values.split(SEPARATOR);
    		// Reverse array
    
    		for (int i = 0; i < aux.length / 2; i++) {
    			String temp = aux[i];
    			aux[i] = aux[aux.length - i - 1];
    			aux[aux.length - i - 1] = temp;
    		}
    		return new ArrayIterator(aux);
    	}
    
    }

### L'Iterator

package designpatterns.iterator;

    import java.util.Iterator;
    
    public class ArrayIterator implements Iterator {
    
    	private String[] values;
    	private int index = 0;
    
    	public ArrayIterator(String[] values) {
    		this.values = values;
    	}
    
    	@Override
    	public boolean hasNext() {
    		return (index < values.length);
    	}
    
    	@Override
    	public Object next() {
    		String result = values[index];
    		index++;
    		return result;
    	}
    
    	@Override
    	public void remove() {
    		// TODO Auto-generated method stub
    
    	}
    
    }

**Remarque** : normalement, cette classe devrait être une classe interne de la classe Thermomètre mais je l'ai extraite pour simplifier la lecture.

### Utilisation

    package designpatterns.iterator;
    
    import java.util.Iterator;
    
    public class Test {
    
    	public static void main(String[] args) {
    		Thermometre thermometre = new Thermometre(10);
    		System.out.println("Chronological");
    		Iterator iterator = thermometre.getChronologicalIterator();
    		while (iterator.hasNext()) {
    			System.out.println(iterator.next());
    		}
    		System.out.println("Anti-Chronological");
    		iterator = thermometre.getAntiChronologicalIterator();
    		while (iterator.hasNext()) {
    			System.out.println(iterator.next());
    		}
    	}
    
    }

**Remarque** : on voit bien l’intérêt de l'Iterator qui conserve un traitement identique dans la classe utilisatrice, *Test*, alors que le parcours des données peut être totalement différent et à la charge de la classe connaissant les données et la manière la plus efficace de les traverser.




