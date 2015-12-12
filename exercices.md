
#Exercices
## Exercice 1 : Ann�es Bissextiles

> **Objectif ** D�terminer si une ann�e inf�rieure � 32000 est bissextile.

- Une projet *Exercice*
- Une classe *TestBissextile* 
- Une classe *Calendrier* qui aura une m�thode *estBissextile* 
 *Remarques*
	 - Une ann�e sera stock�e dans un entier.

**Cas 1 Etre bissextile** ?  �tre une ann�e qui est un multiple de 4 (utiliser l'op�rateur %). Mais ne pas �tre un multiple de 100 sauf si c'est un multiple de 400.

(Une ann�e est un mutiple de 4 -> annee % 4 ==0)

*Valeurs � tester* : 
ann�es bissextiles : 2000, 2004,1600, -4
ann�es non bissextiles :  2001,2002,2003,1900,1800,1700

### Diagramme UML

![enter image description here](http://yuml.me/27ad5414)


## Exercice 2 : Palindrome
> **Objectif ** D�terminer si une chaine de caract�re est un palindrome (c'est � dire si elle peut se lire de mani�re identique dans les deux sens).

*Valeurs � tester* : 
	palindromes : kayak, radar, y, oo, Esope reste ici et se repose
	non palindromes : chien, maison

- Une classe *TestPalindrome* 
	- Une classe *Palindrome* qui aura une m�thode *estPalindrome* 

> La solution de Gregory (pas mal, pas mal): 
> https://github.com/Greg-Klein/BGE/blob/develop/www/java/exo2/src/Palindrome.java

**Attention**, le cas "Esope reste..." est plus compliqu� car il faut:
 - Faire attention aux majuscules/minuscules
 - Ne pas prendre en compte les accents.

## Exercice 3 : Lister les nombres premiers inf�rieurs � 1000

## Exercice 4 : Gestion d'�tudiants