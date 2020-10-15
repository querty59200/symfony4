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
12. Créer une vue `show.html.twig` permettant d'afficher un bagage
13. Ajouter une route dans le `PagesController`
```php
/**
*@Route("/show/12", path="show_bagage")
*/
```
14. Modifier le bouton en ajoutant `href="{{ show_bagage }}`
On travailler ici avec les `paths` et non les URL car la maintenance est facilitée

00. Renseigner les variables  `DATABASE_URL` dans le fichier `.env`
  * db_user (root)
  * db_password ()
  * db_name (rentabag)
00. Grâce à l'ORM (Object Relation Mapping) Php Doctrine, créer la db
  * `php bin/console doctrine database:create`
00. Créer les tables (ou Entity ou Classe) grâce à `php bin/console doctrine make:entity`
