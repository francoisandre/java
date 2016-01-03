# Singleton

## Probl�me
Restreindre le nombre d'instance d'une classe. G�n�ralement � une seule instance. 

## Exemple
Les bases de donn�es ont souvent une nombre maximal de connexion autoris�es.
Afin d'�viter d'�puiser les connexions disponibles - mais aussi pour des raisons de performances - il peut �tre pr�f�rable de laisser ouverte une connexion vers une base de donn�es et de faire en sorte que chaque appel � la base de donn�es utilise cette connexion.

## Impl�mentation

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
 - Dans un Singleton, l'usage veut que la m�thode permettant de r�cup�rer l'instance unique soit appel�e *getInstance*. Parfois le nom de la classe comporte �galement le suffixe "Singleton".
 - Il est important de noter l'absence de constructeur public pour emp�cher une instanciation directe de la classe 
 - L'impl�mentation ci-dessus est tr�s simple. Elle souffre de certains d�fauts, notamment la non prise en compte des acc�s concurrents (multithreading) 

