# Decorator

## Problème
Attacher dynamiquement de nouvelles responsabilités à un objet.

Lorsqu'on a un objet de base et que l'on souhaite lui ajouter de nouveaux comportements le premier réflexe du développeur est l'héritage.

Toutefois, parfois cette approche n'est pas la meilleure. Le design pattern *Decorator* est parfois une astucieuse alternative.

## Exemple

Supposons que notre objectif est d'imprimer des factures. Nous avons un objet de base représentant le corps d'une facture.

Nous savons que nous allons devoir ajouter plusieurs comportements à notre impression de facture. En effet, dans certains cas nous allons ajouter un en-tête, dans d'autre un pied de page, parfois une carte, parfois plusieurs de ces composants.

### Approche brutale  
On pourrait commencer par faire une classe *FactureBasique*, une classe *FactureAvecEntete*, une classe *FactureAvecPiedDePage*, une classe *FactureAvecEnteteEtPiedDePage*...

### Limitations de l'approche brutale
On voit bien que la stratégie est mauvaise car elle aborde la complexité du problème de la mauvaise manière. En effet, les classes vont se multiplier pour couvrir tous les cas et l'ajout d'un nouveau comportement obligerait à créer un très grand nombre de classe.

### Le décorator
Pour être le plus efficace, au lieu d'utiliser un ajout de comportement de manière verticale avec l'héritage, le *Decorator* va proposer une démarche plus horizontale - basée sur la composition -  où les différents composants vont s’emboîter de manière sélective en ajoutant chacun un comportement.

## Implémentation
### Diagramme UML
![enter image description here](https://upload.wikimedia.org/wikipedia/commons/thumb/e/e9/Decorator_UML_class_diagram.svg/606px-Decorator_UML_class_diagram.svg.png)


Les points importants à noter pour comprendre le mécanisme sont les suivants :

-  On définit une interface générique (*Component*) qui va définir le comportement de base de notre objet.
- Il y a au moins une implémentation directe de cette interface (*ConcreteComponent*)
- Les autres classes vont décorer la classe *ConcreteComponent* en définissant une chaine de *Component*. Le *Decorator* va stocker la chaîne des précédents *Component* et chaque *ConcreteDecorator* va ajouter un traitement à celui retourné par cette chaîne.  Chaque *ConcreteDecorator* va recevoir la chaîne dans son constructeur.

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

**Remarque** : le *Decorator* est abstrait et implémente le *Component* et introduit le constructeur prenant un *Component* comme argument .

### Quelques exemple de ConcreteDecorator

Nous allons introduire 3 comportements - en-tête, pied de page et carte. Il nous suffira d'une classe par comportement.

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
    		sb.append("Voici l'en-tête \n");
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
    
    		System.out.println("Facture par défaut :");
    		System.out.println("");
    		System.out.println(defaultFacture.print());
    		System.out.println("");
    
    		System.out.println("Facture avec en-tête :");
    		System.out.println("");
    		System.out.println(factureAvecEntete.print());
    		System.out.println("");
    
    		System.out.println("Facture avec pied de page :");
    		System.out.println("");
    		System.out.println(factureAvecPiedDePage.print());
    		System.out.println("");
    
    		System.out.println("Facture avec en-tête et pied de page :");
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

On voit bien l’enchaînement des *ConcreteDecorator* qui se termine par le *ConcreteComponent*.

## Conclusion
Dans Eclipse, le concept de décorateur est très utilisé. L'exemple de l'arbre des projets est très représentatif.

Chaque fichier est associé à une icone - qui est notre *ConcreteComponent* - et chaque module complémentaire (Git, Java, M2E,...) va ajouter des *ConcreteDecorator* qui vont chacun ajouter une surcouche à l'icone de base: une croix sur les fichiers ne compilant pas, un "J" et un "M" sur le projet Java mavenisé... 

![enter image description here](https://raw.githubusercontent.com/francoisandre/java/master/images/arbre.png)

Les décorateurs peuvent même paramétrés dans les préférences d'Eclipse...dans les rubriques *Decorations*.

## Il y a plus...
Dans le JDK, une série de classes assez importante est basée sur le concept de Decorator, Mais pour l'étudiant débutant n'a pas souvent connaissance de ce concept au moment où il aborde ces classes ce qui les rend difficilement compréhensibles...

...à suivre...