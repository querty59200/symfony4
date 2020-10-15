1. Vérifier la version de Php (Version > 7.1.9) pour utiliser Symfony `php -v`
2. Vérifier la présence de Composer `composer -V`
3. Installer le CLI (Command Line Interface) de Symfony `https://symfony.com/download`
4. Créez le projet `RentABag` dans le dossier `Symfony` avec `symfony new RentABagProject --full --version=lts`
5. Se placer dans le dossier `rentabag`
6. Lancer le server `symfony serve`
7. Créer le controller `php bin/console make:controller` et le nommé `PagesController`
8. Créer la fonction `home()` redirigeant les internautes vers la page 
```php
// home.html.twig
/**
*@Route("/", name="home")
*/
public function home()
{
return $this->render('pages/home.html.twig');
}
```
9. Adapter le fichier `base.html.twig` en y mettant tout le code redondant
  * le css `<link rel="stylesheet" href="https://bootswatch.com/4/flatly/bootstrap.min.css">`
  * le navbar, le footer
10. Dans la vue `base.html.twig`, mettre le `{% block body %}{% endblock %}` dans un `<div class="container"></div>`
11. Faire hériter `home.html.twig` grâce à 
```php
{% extends 'base.html.twig' %} 
  {% block body %} 
  (...) 
  {% endblock %}
```
et adaptez `home.html.twig` à votre besoin
  * un titre
  * une image de bagage
  * un descriptif du bagage
  * un bouton `value="Plus de détails"`
12. Créer une vue `show.html.twig` héritant de `base.html.twig` permettant d'afficher un bagage
13. Ajouter une route dans le `PagesController`
```php
/**
*@Route("/show/12", path="show_bagage")
*/
```
14. Modifier le bouton en ajoutant `href="{{ path('show_bagage') }}`
On travailler ici avec les `paths` et non les URL car la maintenance est facilitée
15. Grâce à l'ORM Php (Object Relational Mapping) Doctrine, CRUD une db est facilité
  * `Entity` = classe métier
  * `Manager` = classe permettant de manipuler une entrée (CUD)
  * `Repository` = classe permettant de lire un table (R)
  * `Migration` = classe permettant de faire le lien entre les fichiers et SQL
  * `Fixture` = script créant de fausses entrées dans la db
  * `php bin/console doctrine database:create`
16. Renseigner les variables  `DATABASE_URL` dans le fichier `.env`
  * db_user
  * db_password
  * db_name
  * ex : `DATABASE_URL=mysql://`root`:`azerty`@127.0.0.1:3306/`rentabag`?serverVersion=5.7`
17. Créer les tables (ou Entity ou Classe) grâce à `php bin/console make:entity`
  * Renseigner les attributs de l'entité en précisant le nom et le type
Attention un attribut comme `createAt` devient `create_at` dans la table
18. Créer le fichier de script préalable à la création de la db (plus tard à la mise à jour)
  * `php bin/console make:migration`
Le framework compare la db aux classes du projet
19. Grâce à Doctrine, execute les scripts 
`php php/bin doctrine:migrations:migrate`
20. Ajouter des entrées fausses (fixtures) dans la table
  * Au préalable, télécharger la librairie `composer require orm-fixtures --dev` disponible uniquement en mode `dev`
  * Le fichier `composer.json` a été mis à jour
  * Créer une fixture `php php/bin make:fixtures`
  * Editer `BagageFixtures`
  ```php
  // Ajouter use App\Entity\Bagage
class BagageFixtures extends Fixture
{
    public function load(ObjectManager $manager)
    {
        for($i = 0; $i < 50 ; $i++){
            $bagage = new Bagage();
            $bagage->getCreateAt(new \DateTime())
                ->getHeigth(1 + $i * 10)
                ->getLength(2 + $i * 10)
                ->getWeigth(1 + $i * 20)
                ->getWidth(3 + $i * 5);

            $manager->persist($bagage);
        }

        $manager->flush();
    }
}
```
  * Charger la fixture dans la db `php bin/console doctrine:fixtures:load`
  * Il est possible de s'aider de la librairie Faker `composer require fzaninotto/faker`
  Nettoyer le cache de composer si nécessaire avant d'installer Faker `composer clearcache`
  ````php
  // Ajouter use App\Entity\Bagage
  class BagageFixtures extends Fixture {
    public function load(ObjectManager $manager)
    {
      $faker = Factory::create('fr_FR');

      for($i = 0; $i < 50; $i++){
          $user = new User();
          $user->setFirstname($faker->lastName());
          $user->setLastname($faker->firstName());
          $user->setCity($faker->city());
          $manager->persist($user);
      }

      $manager->flush();
   }
}
```
