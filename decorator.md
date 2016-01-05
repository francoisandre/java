# Decorator

## Probl�me
Attacher dynamiquement de nouvelles responsabilit�s � un objet.

Lorsqu'on a un objet de base et que l'on souhaite lui ajouter de nouveaux comportements le premier r�flexe du d�veloppeur est l'h�ritage.

Toutefois, parfois cette approche n'est pas la meilleure. Le design pattern *Decorator* est parfois une astucieuse alternative.

## Exemple

Supposons que notre objectif est d'imprimer des factures. Nous avons un objet de base repr�sentant le corps d'une facture.

Nous savons que nous allons devoir ajouter plusieurs comportements � notre impression de facture. En effet, dans certains cas nous allons ajouter un en-t�te, dans d'autre un pied de page, parfois une carte, parfois plusieurs de ces composants.

### Approche brutale  
On pourrait commencer par faire une classe *FactureBasique*, une classe *FactureAvecEntete*, une classe *FactureAvecPiedDePage*, une classe *FactureAvecEnteteEtPiedDePage*...

### Limitations de l'approche brutale
On voit bien que la strat�gie est mauvaise car elle aborde la complexit� du probl�me de la mauvaise mani�re. En effet, les classes vont se multiplier pour couvrir tous les cas et l'ajout d'un nouveau comportement obligerait � cr�er un tr�s grand nombre de classe.

### Le d�corator
Pour �tre le plus efficace, au lieu d'utiliser un ajout de comportement de mani�re verticale avec l'h�ritage, le *Decorator* va proposer une d�marche plus horizontale - bas�e sur la composition -  o� les diff�rents composants vont s�embo�ter de mani�re s�lective en ajoutant chacun un comportement.

## Impl�mentation
### Diagramme UML
![enter image description here](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e9/Decorator_UML_class_diagram.svg/606px-Decorator_UML_class_diagram.svg.png)


Les points importants � noter pour comprendre le m�canisme sont les suivants :

-  On d�finit une interface g�n�rique (*Component*) qui va d�finir le comportement de base de notre objet.
- Il y a au moins une impl�mentation directe de cette interface (*ConcreteComponent*)
- Les autres classes vont d�corer la classe *ConcreteComponent* en d�finissant une chaine de *Component*. Le *Decorator* va stocker la cha�ne des pr�c�dents *Component* et chaque *ConcreteDecorator* va ajouter un traitement � celui retourn� par cette cha�ne.  Chaque *ConcreteDecorator* va recevoir la cha�ne dans son constructeur.

Dans notre cas, cela donne ceci :

###L'interface Facture (Component)

    package designpatterns.decorator;
    
    public interface Facture {
    
    	String print();
    
    }

###L'interface DefaultFacture (ConcreteComponent)

    package designpatterns.decorator;
    
    public class DefaultFacture implements Facture {
    
    	@Override
    	public String print() {
    		StringBuilder sb = new StringBuilder();
    		sb.append("-----------------\n");
    		sb.append("Voici ma facture \n");
    		sb.append("-----------------\n");
    		return sb.toString();
    	}
    
    }

### Le Decorator

    package designpatterns.decorator;
    
    public abstract class AbstractDecoratedFacture implements Facture {
    	protected Facture previousFacture;
    
    	public AbstractDecoratedFacture(Facture previousFacture) {
    		this.previousFacture = previousFacture;
    	}
    }

**Remarque** : le *Decorator* est abstrait et impl�mente le *Component* et introduit le constructeur prenant un *Component* comme argument .

### Quelques exemple de ConcreteDecorator

Nous allons introduire 3 comportements - en-t�te, pied de page et carte. Il nous suffira d'une classe par comportement.

#### FactureAvecEntete 

    package designpatterns.decorator;
    
    public class FactureAvecEntete extends AbstractDecoratedFacture {
    
    	public FactureAvecEntete(Facture previousFacture) {
    		super(previousFacture);
    	}
    
    	@Override
    	public String print() {
    		StringBuilder sb = new StringBuilder();
    		sb.append("-----------------\n");
    		sb.append("Voici l'en-t�te \n");
    		sb.append("-----------------\n");
    		sb.append(previousFacture.print());
    		return sb.toString();
    	}
    }

#### FactureAvecPiedDePage 

    package designpatterns.decorator;
    
    public class FactureAvecPiedDePage extends AbstractDecoratedFacture {
    
    	public FactureAvecPiedDePage(Facture previousFacture) {
    		super(previousFacture);
    	}
    
    	@Override
    	public String print() {
    		StringBuilder sb = new StringBuilder();
    		sb.append(previousFacture.print());
    		sb.append("-----------------\n");
    		sb.append("Voici le pied de page \n");
    		sb.append("-----------------\n");
    		return sb.toString();
    	}
    }

#### FactureAvecCarte 

     package designpatterns.decorator;
    
    public class FactureAvecCarte extends AbstractDecoratedFacture {
    
    	public FactureAvecCarte(Facture previousFacture) {
    		super(previousFacture);
    	}
    
    	@Override
    	public String print() {
    		StringBuilder sb = new StringBuilder();
    		sb.append(previousFacture.print());
    		sb.append("-----------------\n");
    		sb.append("Voici la carte \n");
    		sb.append("-----------------\n");
    		return sb.toString();
    	}
    }

### Utilisation

    package designpatterns.decorator;
    
    public class Test {
    
    	public static void main(String[] args) {
    		Facture defaultFacture = new DefaultFacture();
    		Facture factureAvecEntete = new FactureAvecEntete(new DefaultFacture());
    		Facture factureAvecPiedDePage = new FactureAvecPiedDePage(new DefaultFacture());
    		Facture factureAvecEnteteEtPiedDePage = new FactureAvecPiedDePage(new FactureAvecEntete(new DefaultFacture()));
    		Facture factureAvecCarteEtPiedDePage = new FactureAvecPiedDePage(new FactureAvecCarte(new DefaultFacture()));
    
    		System.out.println("Facture par d�faut :");
    		System.out.println("");
    		System.out.println(defaultFacture.print());
    		System.out.println("");
    
    		System.out.println("Facture avec en-t�te :");
    		System.out.println("");
    		System.out.println(factureAvecEntete.print());
    		System.out.println("");
    
    		System.out.println("Facture avec pied de page :");
    		System.out.println("");
    		System.out.println(factureAvecPiedDePage.print());
    		System.out.println("");
    
    		System.out.println("Facture avec en-t�te et pied de page :");
    		System.out.println("");
    		System.out.println(factureAvecEnteteEtPiedDePage.print());
    		System.out.println("");
    
    		System.out.println("Facture avec carte et pied de page :");
    		System.out.println("");
    		System.out.println(factureAvecCarteEtPiedDePage.print());
    		System.out.println("");
    	}
    }

 
La philosophie centrale du *Decorator* qui apparait clairement lors de l'instanciation :

    Facture factureAvecEnteteEtPiedDePage = new FactureAvecPiedDePage(new FactureAvecEntete(new DefaultFacture()));

On voit bien l�encha�nement des *ConcreteDecorator* qui se termine par le *ConcreteComponent*.

## Conclusion
Dans Eclipse, le concept de d�corateur est tr�s utilis�. L'exemple de l'arbre des projets est tr�s repr�sentatif.

Chaque fichier est associ� � une icone - qui est notre *ConcreteComponent* - et chaque module compl�mentaire (Git, Java, M2E,...) va ajouter des *ConcreteDecorator* qui vont chacun ajouter une surcouche � l'icone de base: une croix sur les fichiers ne compilant pas, un "J" et un "M" sur le projet Java mavenis�... 

![enter image description here](https://raw.githubusercontent.com/francoisandre/java/master/images/arbre.png)

Les d�corateurs peuvent m�me param�tr�s dans les pr�f�rences d'Eclipse...dans les rubriques *Decorations*.

## Il y a plus...
Dans le JDK, une s�rie de classes assez importante est bas�e sur le concept de Decorator, Mais pour l'�tudiant d�butant n'a pas souvent connaissance de ce concept au moment o� il aborde ces classes ce qui les rend difficilement compr�hensibles...

...� suivre...