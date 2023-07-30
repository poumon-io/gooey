?> Prenez note que ce plugin est en pré-release. Le lien partagé pour l'installation, directement depuis mon dropbox, sera supprimé lors de sa sortie officielle. N'UTILISEZ PAS cette version sur vos forums officiels ! Vous pouvez quand même l'essayer et bidouiller sur un forum test en attendant, et bien sûr [me partager vos retours](https://github.com/poumon-io/gooey/issues).

Gooey est un plugin qui permet de réécrire de manière plus flexible le template d'affichage des catégories et des forums sur l'index (index_box). C'est une sorte de [Template Engine](https://en.wikipedia.org/wiki/Template_processor) qui récupère toutes les informations nécessaires de l'index (catégories, forums, sous-forums, etc.) et permet d'en faire ce que vous voulez (de le placer n'importe où, dans n'importe quel ordre).

> Attention, ce plugin n'est pas à portée de tous. Bien qu'il soit simple à installer, il demande une bonne connaissance en JavaScript pour être utilisé et compris pleinement. Et ce, même s'il vient avec quelques exemples, dont une réécriture complète du template par défaut de chaque version !

## Prérequis

- Assurez-vous de séparer les catégories sur l'index de votre forum `Panneau d'administration > Affichage > Page d'accueil > Structure et hiérarchie`. Je conseille également une compression "moyenne" des forums. C'est également à cet endroit que vous pouvez choisir d'afficher ou non le titre et l'avatar du dernier message.
- Les forums orphelins (c'est-à-dire n'appartenant à aucune catégorie, comme la corbeille sur un forum fraichement créé) seront ignorés par le plugin.

## Restrictions

Gooey n'est pas très intelligent. Comme la plupart des Template Engine côté client, il compile le HTML et l'insère dans le DOM avec `innerHTML`. Si vous comptez utiliser/manipuler le DOM compilé à l'aide d'évènement (comme `click` ou `hover`), il faudra :

- Soit déléguer les évènements sur le document (voir: Event Delegation)
- Soit initialiser/appeler vos scripts une fois la fonction `render` appelée à l'aide d'une fonction callback définie lors de l'initialisation du script `app.render(callback)`. Prenez note aussi que la fonction `render` renvoie une promesse et qu'il est possible de faire la même chose grâce à `app.render().then(callback)`.

## Installation

Le script doit être installé dans le template overall_footer_end (Fin du bas de page), juste avant la fermeture de la balise `</body>` comme ceci : 

```html
<script src="https://dl.dropbox.com/s/4b76cltajvc3w16/bundle.js?dl=0"></script>

</body>
```

Ensuite, dans le template index_body (Page d'accueil - affichage des catégories), il faudra le remplacer au complet par ceci : 

```html
<script type="text/template" id="gooey">
  hello !
</script>

<div id="app" hidden>  
  <!-- BEGIN catrow -->
  <!-- BEGIN tablehead -->
  <div class="cat_ref" data-ref="{catrow.tablehead.ID}">
    <var title="title">{catrow.tablehead.L_FORUM}</var>
    <!-- END tablehead -->

    <!-- BEGIN forumrow -->
    <div class="for_ref">
      <var title="url">{catrow.forumrow.U_VIEWFORUM}</var>
      <var title="name">{catrow.forumrow.FORUM_NAME}</var>
      <var title="status">{catrow.forumrow.L_FORUM_FOLDER_ALT}</var>
      <var title="status_img">{catrow.forumrow.FORUM_FOLDER_IMG}</var>
      <var title="description">{catrow.forumrow.FORUM_DESC}</var>
      <var title="topics">{catrow.forumrow.TOPICS}</var>
      <var title="posts">{catrow.forumrow.POSTS}</var>
      <var title="subs">{catrow.forumrow.LINKS}</var>
      <var title="lastpost">
        <!-- BEGIN switch_topic_title -->
        <var title="lastpost-title">{catrow.forumrow.LATEST_TOPIC_NAME}</var>
        <var title="lastpost-url">{catrow.forumrow.U_LATEST_TOPIC}</var>
        <!-- END switch_topic_title -->
        <var title="lastpost-user">{catrow.forumrow.USER_LAST_POST}</var>
        <!-- BEGIN avatar -->
        <var title="lastpost-avatar">{catrow.forumrow.avatar.LAST_POST_AVATAR}</var>
        <!-- END avatar -->
      </var>
    </div>
    <!-- END forumrow -->
    
    <!-- BEGIN tablefoot -->
  </div>
  <!-- END tablefoot -->
  <!-- END catrow --> 
</div>

<script>
document.addEventListener("DOMContentLoaded", function(){

	var app = new Gooey('#app', {
	  template: '#gooey',
	  data: {
	  	excludes: ['']
	  }
	});
	app.render();

});  
</script>
```

Le nouveau template s'écrit à l'intérieur de la balise `<script type="text/template" id="gooey"></script>`.

!> Faites également attention à la position de cette balise ! Le template sera compilé et insérer dans le DOM à l'endroit de celle-ci.

## ForumActif vs Gooey

import MyComponent from "../components/article/FAComparisonCode"

export const PHPBB2 = `{{~ categories:c }}
	{{~ c.forums:f }}
	{{~ }}
 {{~ }}`

Voici l'équivalent 

<FAComparisonCode PHPBB2={PHPBB2} />
