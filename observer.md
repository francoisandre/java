# Observer

## Problème
Communiquer avec des objets lointains ou inconnus...

Lorsqu'un objet A invoque une méthode s'une objet B, on dit qu'il y a dialogue, qu'un message est envoyé de A à B.

Toutefois la communication classique peut être inadaptée dans deux cas que l'on rencontre fréquemment 

### A et B sont séparés par de multiples objets

On peut envisager que dans notre application A ne possède pas une référence directe sur B et que les objets A et B sont séparés par de multiples objets: C, D, E, F.
Pour que A puisse dialoguer avec B, le message va devoir passer par C, D, E, F avant d'atteindre B.

Une des conséquences est que l'on doit alors ajouter dans C, D, E et F des méthodes uniquement pour faire passer la communication entre A et B sans que C, D, E et F ne soient forcément concernées par la nature du message. Un peu comme si les tuyaux reliant la chaudière à la baignoire passaient au beau milieu du salon...

### A et B ne se connaissent pas.

Il se peut qu'au moment de son codage, A ne connaissent pas les différents objets B avec lesquels il devra communiquer.

## Exemple
Notre application comporte une page de connexion via laquelle un utilisateur administrateur indique son login et son mot de passe.

En cas de réussite, plusieurs partie de notre application doivent se mettre à jour:

- La barre de menu doit afficher la partie dédiée aux administrateurs
- La barre d'en-tête doit afficher le nom de l'utilisateur 

### Approche brutale  
Notre objet connectionPage pourrait avoir une référence sur l'instance de l'objet MenuBar et une autre vers l'objet HeaderBar/

A l'issue d'une connexion réussie, connectionPage pourrait appeler explicitement des méthodes de ces objets:

 - menuBar.displayAdministrationMenu()
 - header.displayUserName();

### Limitations de l'approche brutale
L'approche brutale est limitée car elle suppose que notre objet connectionPage 
 - possède une référence vers tous les objets qui sont concernés par l’événement *un utilisateur s'est connecté* ce qui peut mener à du code spaghetti.
 - sache quel est la manière dont chacun de ces objets réagissent à l’événement (l'un affiche un menu, l'autre un nom..) 
 
 De plus, si un nouvel objet veut réagir à la connexion d'un utilisateur, il est nécessaire de modifier la classe ConnectionPage ce qui n'est pas forcément possible, celle-ci pouvant par exemple venir d'une librairie tierce.

### L'Observer
Le design pattern Observer ressemble à la notion de l'abonnement à une revue: 
Lorsqu'un client veut recevoir une revue, 

1. il s'inscrit au préalable auprès de l'éditeur de la revue. 
2. chaque une qu'une revue est publiée, l'éditeur l'envoie dans la boite aux lettres de tous les abonnées
3. Les abonnées décident de ce qu'ils doivent faire de la revue (la lire de suite, l'amener au bureau,...)

On voit bien que ce le mécanisme d'abonnement apporte :
 -Il définit une interface, *l'abonné*, qui a une méthode *deposeBoiteAuxLettres*.  
 -L'éditeur ne connait de l'abonné que cette méthode *deposeBoiteAuxLettres* et ne sait pas ce que vont faire les abonnées de la revue 

## Implémentation

### Diagramme UML

L'implémentation de l'*Observer** est la suivante :

![enter image description here](https://upload.wikimedia.org/wikipedia/commons/8/8d/Observer.svg)

![enter image description here](https://raw.githubusercontent.com/francoisandre/java/master/images/iterator.png)

Dans notre cas, Notre abonné correspond à Observer et la méthode *deposeBoiteAuxLettres* correspond à *notify()*. 

Le Subject correspond à l'éditeur. Il maintient une liste d'abonnés. Les abonnées peuvent s'abonner (*registerObserver*) ou se désabonner (*unregisterObserver*).

Enfin, la méthode notifyObservers parcourt les différents Observers afin de les notifier de l'événement.

###Exercice : Application dans le la page de connexion

#### Classe User

    package designpatterns.observer;
    
        public class User {
        
        	private String name;
        
        	public String getName() {
        		return name;
        	}
        
        	public void setName(String name) {
        		this.name = name;
        	}
        
        }

#### Classe UserConnectionObserver 

    package designpatterns.observer;
    
    public interface UserConnectionObserver {
    	public void userConnection(User user);
    }

#### Classe ConnectionPage 

    package designpatterns.observer;
    
    import java.util.ArrayList;
    import java.util.Iterator;
    
    public class ConnectionPage {
    
    	ArrayList<UserConnectionObserver> observers = new ArrayList<>();
    
    	public void registerUserConnectionObserver(UserConnectionObserver observer) {
    		observers.add(observer);
    	}
    
    	public void notifyUserConnection(User user) {
    		for (Iterator<UserConnectionObserver> iterator = observers.iterator(); iterator.hasNext();) {
    			UserConnectionObserver userConnectionObserver = iterator.next();
    			userConnectionObserver.userConnection(user);
    		}
    	}
    
    }

#### Classe UserConnectionObserver 

    package designpatterns.observer;
    
    public class Header implements UserConnectionObserver {
    
    	@Override
    	public void userConnection(User user) {
    		displayUserName(user.getName());
    
    	}
    
    	private void displayUserName(String name) {
    		System.out.println("L'utilisateur qui s'est connecté est " + name);
    	}
    
    }


#### Classe Header 

    package designpatterns.observer;
    
    public class Header implements UserConnectionObserver {
    
    	@Override
    	public void userConnection(User user) {
    		displayUserName(user.getName());
    
    	}
    
    	private void displayUserName(String name) {
    		System.out.println("L'utilisateur qui s'est connecté est " + name);
    	}
    
    }

#### Classe MenuBar 

    package designpatterns.observer;
    
    public class MenuBar implements UserConnectionObserver {
    
    	@Override
    	public void userConnection(User user) {
    		displayAdministrationMenu();
    
    	}
    
    	private void displayAdministrationMenu() {
    		System.out.println("J'affiche le menu d'administration");
    	}
    
    }

#### Classe Test

    package designpatterns.observer;
    
    public class Test {
    
    	public static void main(String[] args) {
    
    		ConnectionPage connectionPage = new ConnectionPage();
    		Header header = new Header();
    		MenuBar menuBar = new MenuBar();
    		connectionPage.registerUserConnectionObserver(header);
    		connectionPage.registerUserConnectionObserver(menuBar);
    
    		User fakeUser = new User();
    		fakeUser.setName("Ringo");
    		connectionPage.notifyUserConnection(fakeUser);
    
    	}
    
    }

   

## Remarques
La notion d'Observer, dans lequel des événements vont être transmis à un ensemble d'abonnés est extrêmement utilisée dans le développement d'interfaces graphiques. On parle dans ce cas de programmation événementielle.

Le terme *listener* est parfois  utilisé à la place de *Observer*

Java inclut dans sont SDK un framework mettant en place le mécanisme d'Observer: 
https://docs.oracle.com/javase/tutorial/uiswing/events/propertychangelistener.html







