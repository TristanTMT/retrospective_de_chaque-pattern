# retrospective_de_chaque-pattern

### Qu’est-ce qu’un design pattern ?

Un design pattern est un ensemble de bonnes pratiques ayant pour but de rendre le code plus propre, optimisé, robuste, maintenable et évolutif afin de répondre au problèmes récurrents rencontré par les développeurs. La 1ère apparition publique des design pattern vient d’un livre publié en 1977 par l’ingénieur Christopher Alexander nommé « A Pattern Language ».

En 1995 le Gang of four a présenter les 23 design pattern qui font aujourd’hui office de référence dans le domaine de l’informatique.

Il existe 3 types différents de design Pattern :

• Les patterns de créations        

• Les patterns de structuration      

• Les patterns comportementaux

### Le design pattern FlyWeight

Le design pattern FlyWeight appartient au pattern de structuration

Pourquoi FlyWeight ?

Souvent lors de l’utilisation d’un programme on se retrouve à instancier de nombreux objets, chacun prenant une certaine quantité de mémoire on peut donc se demander comment réduire la place des objets instanciés, on va donc utiliser le design pattern FlyWeight qui va en plus accélérer la vitesse d’exécution du programme.

En effet, ce pattern, à l’aide d’une factory si 2 objets différents ont un paramètre en commun (exemple 2 cercle d’une même couleur mais d’une taille différente) on va utiliser ce paramètre déjà construit dans le premier afin de réaliser le deuxième ainsi la place en mémoire sera celle d'un seul cercle pour en créer deux, pour cela on va utiliser les setters de la classe cercle en résumé on va utiliser un type objet pour représenter une gamme de petits objets tous différents.

![alt text](https://www.codingame.com/servlet/fileservlet?id=15922341803398)
Plus précisément :
Le pattern poids mouche est utilisé dans des programmes utilisant un grand nombre d’objets et souhaitant en réduire le poids mémoire. Ce modèle est donc très utile aujourd’hui dans les périphériques mobiles ou les systèmes intégrés.

Attention !! Il faut prendre en en compte plusieurs facteurs avant l’utilisation de cette méthode :
- Le nombre d’objets à créer doit être suffisant.
- La création d’objet est lourde en mémoire et prend du temps.
- Les propriétés de l’objet peuvent être divisées en propriétés intrinsèques et extrinsèques. Les propriétés extrinsèques d’un objet doivent être définies par le programme client.

Quelques définitions :
Propriétés intrinsèques : données internes qui rendent une classe unique.
Propriétés extrinsèques : données externes passées à la classe en tant qu’arguments.

Diagramme de classe UML
Voici le diagramme de classe que nous allons implémenter :

![alt text](https://www.codingame.com/servlet/fileservlet?id=15922351993943)

Première étape.
Premièrement on crée la classe abstraite forme.
```
interface Forme {
	   void dessiner();
}
```
Deuxieme étape
On crée ensuite la classe cercle où on définit les paramètres ansi que les setters :
```
class Cercle implements Forme {
	   private String couleur;
	   private int x;
	   private int y;
	   private int rayon;

	   //Constructeur du cercle
	   public Cercle(String couleur){
	      this.couleur = couleur;		
	   }
	   
	   // X et Y sont les coordonnées du centre du cercle
	   
	   //Setter de X
	   public void setX(int x) {
	      this.x = x;
	   }

	   //Setter de Y
	   public void setY(int y) {
	      this.y = y;
	   }

	   //Setter du rayon
	   public void setRayon(int rayon) {
	      this.rayon = rayon;
	   }
	   
	   //Methode de l'affichage du cercle
	   public void dessiner() {
	      System.out.println("Cercle: Dessiner() [Couleur : " + this.couleur + ", x : " + this.x + ", y :" + this.y + ", Rayon :" + this.rayon);
	   }
	}
```
Troisième étape
On crée ensuite la class FormeFactory qui permet de stocker dans une map les cercles crées et de récuperer de cette même map un cercle déjà existant :
```
import java.util.HashMap;

class FormeFactory {
   private static final HashMap<String, Forme> mapCercles = new HashMap();

   //Methode permettant la création de nouveaux cercles
   public static Forme getCercle(String couleur) {
	   
	  //On récupère le cercle s'il est présent dans la map
      Cercle cercle = (Cercle)mapCercles.get(couleur);
      
      //Dans le cas contraire on fait une nouvelle instance de Cercle
      if(cercle == null) {
         cercle = new Cercle(couleur);
         mapCercles.put(couleur, cercle);
         System.out.println("Création d'un cercle de couleur : " + couleur);
      }
      return cercle;
   }
}
```


### Le design pattern Composite

Il existe donc le design pattern composite qui permet de gérer un ensemble d'objets en tant qu'un seul et même objet, autrement dit un objet composé de plusieurs autres.

Il permet d'additionner les propriétés des différents objets (par exemple un prix) pour en composer un seul et même. Il simplifie les compositions car on peut ajouter ou supprimer des éléments d'un objet composé grâce à des méthodes, sans avoir à modifier le code de leurs classes.

![alt text](https://www.codingame.com/servlet/fileservlet?id=15644443844584)

Première étape.
L'implémentation d'un pattern Composite.

Premièrement on crée l'interface qui sera implémentée par toutes les autres classes.

```
public interface Composant {

    public int getPoids();

}
```

Deuxiéme étape.
Ensuite on crée les différentes classes que l'on pourra composer.

```
public class Remorque implements Composant {

	private int poids;
   
    public Remorque(int poids) {
		this.poids = poids;
    }
    
    @Override
    public int getPoids() {
        return this.poids;
    }

}

public class Tracteur implements Composant {

	private int poids;
   
    public Tracteur(int poids) {
		this.poids = poids;
    }
	
	@Override
    public int getPoids() {
        return this.poids;
    }
} 
```
Troisième étape.
Puis, on implémente la classe composite Composite, avec les méthodes add et remove qui permettront d'ajouter ou de supprimer plusieurs éléments à une composition.

```
public class CamionComposite implements Composant {

	private Collection children;

    public CamionComposite() {
        children = new ArrayList();
    }

  
    public void add(Composant composant){
     
        children.add(composant);
    }

  
    public void remove(Composant composant){
        children.remove(composant);
    }

    public Iterator getChildren() {
        return children.iterator();
    }
	
	@Override
    public int getPoids() {
        int result = 0;
        for (Iterator i = children.iterator(); i.hasNext(); ) {
            Object objet = i.next();
			
            Composant composant = (Composant)objet;

            result += composant.getPoids();
        }
        return result;
    }
}
```

### Le design pattern Command
Il existe 3 types de Design Pattern : Création, Structure et Comportemental. Le Design Pattern Command fait partie de la famille comportementale. Les modèles comportementaux sont des modèles de conception qui identifient les modèles de communication communs entre les objets et réalisent ces modèles. Ce faisant, ces modèles augmentent la souplesse dans l'exécution de cette communication.

#### Son objectif :
Lors d'une conception d'app on a souvent besoin d’émettre des requêtes sur n'importe quel objet, et ce, sans connaître, ni ses caractéristiques, ni ses fonctions/méthodes. C'est ici que le Command Pattern va jouer son rôle, puisque il va l’encapsuler les requêtes.

*Petit rappel : l’encapsulation en POO consiste à cacher l'implémentation de l'objet, en empêchant l'accès aux données par un autre moyen que les services proposés. L'encapsulation permet donc de garantir l'intégrité des données contenues dans l'objet. C'est l'un des principes les plus importants en Java et langages similaires.*

Ce pattern permet en 1er lieu, d'apporter une sécurité dans les applications, grâce à l'encapsulation. Mais il permet aussi de supprimer toutes duplications de code, au moyen d'une abstraction qui va lui permettre de fonctionner sans connaître l'objet cible ! Plus d'abstraction permet donc une meilleure maintenance du code, permettant de facilement le modifier ou l'étendre.

#### L'implémentation du Command Pattern

![alt text](https://www.codingame.com/servlet/fileservlet?id=23452463760020)

Ce diagramme en notation UML nous montre comment fonctionne ce pattern.

Analysons ce diagramme du pattern :

Le client va déclencher une requête, ici c'est ce qu'on apelle le rôle d'Invoker et ici c'est le Caller.
Cette requête à une commande qui lui est associée, dans notre diagramme c'est la ConcreteCommand qui implémente l'interface Commande.
La ConcreteCommande , grâce à la requête, va ensuite déclencher la méthode de l'objet associée, c'est le Receiver de notre diagramme.

#### Exemple d'une application du Command Pattern

Une personne veut aller ou éteindre son ordinateur :

**Avec le pattern command :** La personne actionne l’Invoker de son ordinateur, ici c'est un switch.
La ConcreteCommande « RestartCommand » ou « ShutDownCommand » est exécutée
Cela permet ensuite d'invoquer les méthodes shutDown() et restart() du Computer (Receiver). L'encapsulation permet de le faire fonctionner sur n'importe quel objet qui aurait une méthode restart() et shutDown().

**Sans pattern command :** La personne commande l'arrêt ou le démarrage de son ordinateur. Il va créer un objet Computer et apeller les méthodes restart() et shutDown().

![alt text](https://www.codingame.com/servlet/fileservlet?id=23452475687364)

Grâce à cet exemple, nous pouvons voir ici le rôle majeur du Command Pattern à savoir l'encapsulation. En l'appliquant, à note programme le client ne sait rien de se qui se passe. L’objet Computer, et méthodes qui lui sont appliquées pour l'éteindre ou l'allumer restent encapsulées !




### Le design pattern Proxy

En pratique, le proxy implémente la même interface que l’objet auquel il sert d’écran, car il va se substituer à lui !
Ce design pattern fait état de relations entre des objets, voilà pourquoi on dit qu’il est structurel ! Proxy (ou Procuration ou encore Surrogate en anglais) a des similitudes avec un autre pattern structurel : Décorateur. Cependant, il convient de bien garder à l’esprit que si Décorateur a pour but d’ajouter des fonctionnalités à l’objet décoré, Proxy est souvent là pour effectuer un contrôle d’accès à un objet.
The Proxy provides a surrogate or place holder to provide access to an object. A check or bank draft is a proxy for funds in an account. A check can be used in place of cash for making purchases and ultimately controls access to cash in the issuer's account.

Exemple
Le proxy fournit un substitut ou un détenteur de place pour permettre l'accès à un objet. Un chèque ou une traite bancaire est un proxy pour les fonds d'un compte. Un chèque peut être utilisé à la place de l'argent liquide pour effectuer des achats et contrôle finalement l'accès à l'argent liquide sur le compte de l'émetteur.

![alt text](https://sourcemaking.com/files/v2/content/patterns/Proxy_example1.png?id=e4af092967e77e9f0a38)
