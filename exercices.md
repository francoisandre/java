
#Exercices
## Exercice 1 : Années Bissextiles

> **Objectif ** Déterminer si une année inférieure à 32000 est bissextile.

- Une projet *Exercice*
- Une classe *TestBissextile* 
- Une classe *Calendrier* qui aura une méthode *estBissextile* 
 *Remarques*
	 - Une année sera stockée dans un entier.

**Cas 1 Etre bissextile** ?  être une année qui est un multiple de 4 (utiliser l'opérateur %). Mais ne pas être un multiple de 100 sauf si c'est un multiple de 400.

(Une année est un mutiple de 4 -> annee % 4 ==0)

*Valeurs à tester* : 
années bissextiles : 2000, 2004,1600, -4
années non bissextiles :  2001,2002,2003,1900,1800,1700

### Diagramme UML

![enter image description here](http://yuml.me/27ad5414)


## Exercice 2 : Palindrome
> **Objectif ** Déterminer si une chaine de caractère est un palindrome (c'est à dire si elle peut se lire de manière identique dans les deux sens).

*Valeurs à tester* : 
	palindromes : kayak, radar, y, oo, Esope reste ici et se repose
	non palindromes : chien, maison

- Une classe *TestPalindrome* 
	- Une classe *Palindrome* qui aura une méthode *estPalindrome* 

> La solution de Gregory (pas mal, pas mal): 
> https://github.com/Greg-Klein/BGE/blob/develop/www/java/exo2/src/Palindrome.java

**Attention**, le cas "Esope reste..." est plus compliqué car il faut:
 - Faire attention aux majuscules/minuscules
 - Ne pas prendre en compte les accents.

## Exercice 3 : Lister les nombres premiers inférieurs à 1000

 - Mettez en place une fonction permettant de savoir si un nombre donné est premier ou non (rappel : un nombre *premier* est un nombre qui n'est divisible que par 1 ou par lui même.
 - En utilisant cette fonction, donnez la liste des 1000 premiers nombre premiers.

Valeurs attendues :  http://mathenjeans.free.fr/amej/glossair/document/nbs_premier/1000_nombres_premiers.html

## Exercice 4 : Trier un tableau d'entiers 

 - Codez un programme permettant de trier par ordre croissant un tableau d'entier passé en paramètres

**Correction** 

**Classe TestTri**


    package exercices;
    import java.util.Arrays;
    
        public class TestTri {
        	
        	public static void main(String[] args) {
        		int[] source = {25,5,13,9,9 ,6,77};
        		int[] resultat = {5,6,9,9,13,25,77};
        		
        		Trieur trieur = new Trieur();
        		int[] tmp1 = trieur.triCroissant(source);
        		boolean result1 = sontEgaux(tmp1, resultat);
        		if (result1 == true) {
        			System.out.println("tmp1 est trié correctement");
        		}
        		else {
        			System.out.println("tmp1 n'est pas trié correctement");
        		}
        		
        		int[] tmp2 = Arrays.copyOf(source, source.length);
        		Arrays.sort(tmp2);
        		boolean result2 = sontEgaux(tmp2, resultat);
        		if (result2 == true) {
        			System.out.println("tmp2 est trié correctement");
        		}
        		else {
        			System.out.println("tmp2 n'est pas trié correctement");
        		}
        		
        	}
        	
        	public static boolean sontEgaux(int[] tableau1, int[] tableau2) {
        		
        		boolean result = true;
        		
        		if (tableau1.length != tableau2.length) {
        			result = false;
        		}
        		else {
        			for (int i = 0; i < tableau1.length; i++) {
        				if (tableau1[i] != tableau2[i]) {
        					result = false;
        					break;
        				}
        			}
        		}
        		return result;
        	}
        
        }


**Classe Trieur**

    package exercices;
    
    public class Trieur {
    
    	public int[] triCroissant(int[] source) {
    		int[] result = new int[source.length];
    		int[] tmp = copy(source);
    		
    		for (int i =0; i <source.length; i++) {
    			int minimumValue = minimumValue(tmp);
    			result[i] = minimumValue;
    			tmp = deleteOneValue(tmp, minimumValue);
    		}
    		
    		return result;
    	}
    	
    	public int minimumValue(int[] tableau) {
    		int result = tableau[0];
    		for (int i = 0; i < tableau.length; i++) {
    			if (tableau[i] < result) {
    				result = tableau[i];
    			}
    		}
    		return result;
    	}
    	
    	public int[] copy(int[] tableau) {
    		int[] result = new int[tableau.length];
    		for (int i = 0; i < tableau.length; i++) {
    				result[i] = tableau[i];
    		}
    		return result;
    	}
    	
    	public int[] deleteOneValue(int[] tableau, int value) {
    		boolean found = false;
    		int[] result = new int[tableau.length-1];
    		for (int i=0; i<result.length;i++) {
    			if (found == true) {
    				result[i] = tableau[i+1];
    			}
    			else {
    				if (tableau[i] == value) {
    					found = true;
    					result[i] = tableau[i+1];
    				}
    				else {
    					result[i] = tableau[i];
    				}
    				
    			}
    		}
    		return result;
    	}
    
    }

## Exercice 5 : Gestion d'étudiants

Un organisme de formation veut suivre les résultats de ses étudiants.
Un étudiant est défini de la manière suivantes :
 
 - Un nom
 - Un prénom
 - Une liste d'au maximum 20 notes.

Le logiciel doit 

 - permettre de calculer la moyenne de chaque élève, indiquer l'élève ayant la plus haute moyenne, l'élève ayant la plus basse moyenne.
 - montrer son efficacité en simulant une classe d'étudiants

## Exercice 5 : Moyenne pondérée
 
 Fait en sorte que dans l'exercice précédent les notes puissent être pondérées par un coefficient (valeurs possibles du coefficient : 1, 2 et 3).
