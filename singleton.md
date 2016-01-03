# Singleton

## Problème
Restreindre le nombre d'instance d'une classe. Généralement à une seule instance. 

## Exemple
Les bases de données ont souvent une nombre maximal de connexion autorisées.
Afin d'éviter d'épuiser les connexions disponibles - mais aussi pour des raisons de performances - il peut être préférable de laisser ouverte une connexion vers une base de données et de faire en sorte que chaque appel à la base de données utilise cette connexion.

## Implémentation

### Le singleton

    package designpatterns.singleton;
    
    public class Connexion {
    
    	private static Connexion connexionInstance;
    
    	private Connexion() {
    
    	}
    
    	public static Connexion getInstance() {
    		if (connexionInstance == null) {
    			connexionInstance = new Connexion();
    		}
    		return connexionInstance;
    	}
    
    	public void executeSQL(String request) {
    		// Do something
    	}
    }


### Utilisation

    public static void main(String[] args) {
    		Connexion instance = Connexion.getInstance();
    		instance.executeSQL("select * from MA_TABLE");
    	}

### 

## Remarques
 - Dans un Singleton, l'usage veut que la méthode permettant de récupérer l'instance unique soit appelée *getInstance*. Parfois le nom de la classe comporte également le suffixe "Singleton".
 - Il est important de noter l'absence de constructeur public pour empêcher une instanciation directe de la classe 
 - L'impl�mentation ci-dessus est très simple. Elle souffre de certains défauts, notamment la non prise en compte des accès concurrents (multithreading) 

