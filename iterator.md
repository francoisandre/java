# Singleton

## Probl�me
Parcourir un ensemble de donn�es 

## Exemple
Supposons qu'une classe Thermom�tre stocke les diff�rentes valeurs de la temp�rature toute les heures dans une cha�ne de caract�res o� chaque temp�rature est s�par�e par une virgule :

ex: 21,22,23,25,22,20 ...

### Approche brutale  
Si une classe, Graphique, veut tracer un graphique des temp�ratures une premi�re approche est que Graphique r�cup�re la cha�ne des temp�ratures et la d�compose en se basant sur la virgule afin de pouvoir effectuer le trac�.

### Limitations de l'approche brutale
L'approche brutale est limit�e car elle va � l'encontre de l'encapsulation des donn�es en programmation objet. En effet, avec cette approche il est n�cessaire pour la (ou les ) classes utilisatrices de connaitre la mani�re interne dont Thermom�tre stocke ses donn�es.

Ainsi, le traitement de Graphique ne va plus fonctionner si Thermom�tre change le caract�re de s�paration ou s'il change sa mani�re de stocker (utilisation d'un tableau).

De plus si une autre classe veut �galement r�cup�rer les donn�es, on peut s'attendre � ce qu'elle mette en place le m�me genre de code que Graphique et nous aurons s�rement des doublons.

### L'Iterator
Pour �tre le plus efficace possible, il faut que Thermom�tre ne permettent plus l'acc�s directe � son m�canisme de donn�es mais via un objet qui aura la seule responsabilit� d��grener les donn�es. Cet objet va impl�menter l'interface It�rator

## Impl�mentation

L'interface Iterator est la suivante : 

![enter image description here](https://raw.githubusercontent.com/francoisandre/java/master/images/iterator.png)

Elle est pr�sente nativement dans Java (https://docs.oracle.com/javase/7/docs/api/java/util/Iterator.html) et est le moyen privil�gi� de parcourir les objets de type *List* ou *Set*. Cette version de l'Iterator utilise la notion de *generics* non encore �tudi�e. Elle rajoute �galement la m�thode *remove()* qui est optionnelle.

### Diagramme UML
![enter image description here]

###La classe Thermom�tre

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

**Remarque** : normalement, cette classe devrait �tre une classe interne de la classe Thermom�tre mais je l'ai extraite pour simplifier la lecture.

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

**Remarque** : on voit bien l�int�r�t de l'Iterator qui conserve un traitement identique dans la classe utilisatrice, *Test*, alors que le parcours des donn�es peut �tre totalement diff�rent et � la charge de la classe connaissant les donn�es et la mani�re la plus efficace de les traverser.




