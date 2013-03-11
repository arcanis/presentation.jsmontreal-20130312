title: Frameworks actuels
subtitle: De nombreux paradigmes
class: big

- Model / View (Backbone)

- Routing (Ember)

- Layout (ExtJS)

- Knockout, Batman, Spine, ...

---

title: Problèmes récurrents
class: big

- Chacun introduit de nouveaux concepts

- Courbe d'apprentissage écrasante

- Manque de convention de conception

- Complexe à configurer ou étendre

---

title: Angular à la rescousse

- Chacun introduit de nouveaux concepts
  <div style="color:red">Angular étend principalement l'HTML</div>

- Courbe d'apprentissage écrasante
  <div style="color:red">Peu de concepts nouveaux</div>

- Manque de convention de conception
  <div style="color:red">'The angular way' est clairement définie</div>

- Complexe à configurer ou étendre
  <div style="color:red">Très peu de code pour ajouter des fonctionnalités</div>

---

title: Concepts clefs
class: big

- HTML étendu

- DOM scoppé

- Mise à jour automatique

- Injection de dépendances

---

title: HTML étendu
class: segue dark

---

title: Variables

<pre class="prettyprint" data-lang="html">
&lt;h1&gt;{{ name }}&lt;/h1&gt;
&lt;h2&gt;By {{ author }}&lt;/h2&gt;
&lt;p&gt;{{ content }}&lt;/p&gt;
</pre>

<pre class="prettyprint" data-lang="html">
&lt;ul&gt;
  &lt;li&gt;{{ price }} excluding taxes&lt;/li&gt;
  &lt;li&gt;{{ price * 1.197 }} including taxes&lt;/li&gt;
&lt;/ul&gt;
</pre>

---

title: Logical blocks

<pre class="prettyprint" data-lang="html">
&lt;div&gt;Hi {{ name }} !&lt;/div&gt;

&lt;div ng-show="name == 'maël'"&gt;
  Did you know that you're especially awesome ?
&lt;div&gt;
</pre>

<pre class="prettyprint" data-lang="html">
&lt;div ng-repeat="post in posts"&gt;
  &lt;h1&gt;{{ post.name }}&lt;/h1&gt;
  &lt;h2&gt;By {{ post.author }}&lt;/h2&gt;
  &lt;p&gt;{{ post.content }}&lt;/p&gt;
&lt;/div&gt;
</pre>

---

title: Filtres

<pre class="prettyprint" data-lang="html">
&lt;div ng-repeat="post in posts"&gt;
  &lt;h1&gt;{{ post.name }}&lt;/h1&gt;
  &lt;h2&gt;Published on {{ post.publish_date | date }} by {{ post.author }}&lt;/h2&gt;
  &lt;p&gt;{{ post.content }}&lt;/p&gt;
&lt;/div&gt;
</pre>

<pre class="prettyprint" data-lang="html">
&lt;h1&gt;Some names containing '{{ query | uppercase }}'&lt;/h1&gt;
&lt;ul&gt;
  &lt;li ng-repeat="name in names | filter:query | limitTo:10"&gt;
    {{ name }}
  &lt;/li&gt;
&lt;/ul&gt;
</pre>

Pas énormément de filtres standard, cependant.

---

title: DOM scoppé
class: segue dark

---

title: Controlleur

Chaque élément peut avoir un controlleur associé

<pre class="prettyprint" data-lang="javascript">
function MyController( $scope ) {
  $scope.users = [ 'jean', 'jules', 'sylvain' ];
}
</pre>

<pre class="prettyprint" data-lang="html">
&lt;ul ng-controller="MyController"&gt;
  &lt;li ng-foreach="user in users"&gt;
    {{ user }}
  &lt;/li&gt;
&lt;/ul&gt;
</pre>

---

title: Sous-controlleurs

Les blocs logiques peuvent aussi avoir leurs controlleurs

<pre class="prettyprint" data-lang="javascript">
function MyController( $scope ) {
  $scope.users = [ 'jean', 'jules', 'sylvain' ];
}

function MySubController( $scope ) {
  $scope.value = Math.random( );
}
</pre>

<pre class="prettyprint" data-lang="html">
&lt;ul ng-controller="MyController"&gt;
  &lt;li ng-foreach="user in users" ng-repeat="MySubController"&gt;
    {{ user }} picked the value {{ rnd }}
  &lt;/li&gt;
&lt;/ul&gt;
</pre>

---

title: Mise à jour automatique
class: segue dark

---

title: Modèles

<pre class="prettyprint" data-lang="html">
&lt;input type="text" ng-model="username" /&gt;
&lt;div&gt;Vous appelez-vous {{ username }} ?&lt;/div&gt;
</pre>

<pre class="prettyprint" data-lang="html">
&lt;input type="text" ng-model="query" /&gt;

&lt;h1&gt;Names matching '{{ query | uppercase }}'&lt;/h1&gt;
&lt;ul&gt;
  &lt;li ng-repeat="name in names | filter:query"&gt;
    {{ name }}
  &lt;/li&gt;
&lt;/ul&gt;
</pre>

<pre class="prettyprint" data-lang="html">
&lt;input type="number" ng-model="price" /&gt;
&lt;div&gt;{{ price | currency }} excluding taxes&lt;/div&gt;
&lt;div&gt;{{ price * 1.197 | currency }} including taxesHT&lt;/div&gt;
</pre>

---

title: Scopes

<pre class="prettyprint" data-lang="javascript">
function MyController( $scope ) {
  $scope.value = 0;

  $scope.add = function ( ) {
    $scope.value += 1;
  };

  $scope.sub = function ( ) {
    $scope.value -= 1;
  };
}
</pre>

<pre class="prettyprint" data-lang="html">
&lt;div ng-controller="MyController"&gt;
  &lt;input type="button" ng-click="add()" &gt;
  &lt;input type="button" ng-click="sub()" &gt;
  &lt;div&gt;Current value : {{ value }}&gt;
&lt;/div&gt;
</pre>

---

title: $Watch !

<pre class="prettyprint" data-lang="javascript">
function MyController( $scope ) {
  $scope.$watch( 'text', function ( ) {
    $scope.hash = md5( $scope.text );
  } );
}
</pre>

<pre class="prettyprint" data-lang="html">
&lt;div ng-controller="MyController"&gt;
  &lt;input type="text" ng-model="text" /&gt;
  &lt;div&gt;MD5 hash : {{ hash }}&lt;/div&gt;
&lt;/div&gt;
</pre>

Pour cet usage, notez qu'un filtre aurait été suffisant :)

<pre class="prettyprint" data-lang="html">
&lt;div&gt;
  &lt;input type="text" ng-model="text" /&gt;
  &lt;div&gt;MD5 hash : {{ text | md5 }}&lt;/div&gt;
&lt;/div&gt;
</pre>

---

title: Avertissement

<span style="color:red">Ce code ne fonctionne pas !</span>

<pre class="prettyprint" data-lang="javascript">
function MyController( $scope ) {
  window.setInterval( function ( ) {
    $scope.value += 1;
  }, 1000 );
}
</pre>

<pre class="prettyprint" data-lang="html">
&lt;div ng-controller="MyController"&gt;
  &lt;div&gt;Current value : {{ value }}&lt;/&gt;
&lt;/div&gt;
</pre>

La vue ne sera pas modifiée.

---

title: Injection de dépendances
class: segue dark

---

title: Injections dans les controlleurs

<pre class="prettyprint" data-lang="javascript">
function MyController( $scope ) {
  // ...
}
</pre>

Angular a analysé le code de `MyController` et déduit que le service nommé `$scope` était requis.

Autre façon de faire :

<pre class="prettyprint" data-lang="javascript">
var MyController = [ '$scope', function ( $scope ) {
  // ...
} ];
</pre>

Ceci évite qu'uglifyjs ou autre ne brise les noms des arguments.

---

title: Quelques services existants

- `$scope`

- `$timeout`, pour émuler `window.setTimeout`

- `$http`, pour les requêtes Ajax

- `$location`, pour l'URL actuelle

- ...

---

title: Définir son propre service

Trois façons, trois noms :

- `app.service(...)`

- `app.factory(...)`

- `app.provider(...)`

Chacun plus complexe mais plus puissant que le précédent.

Référez-vous à [ce gist](https://gist.github.com/Mithrandir0x/3639232) pour un exemple.

---

title: 'The angular way'

<pre class="prettyprint" data-lang="javascript">
Application.provider( 'quotes', function ( ) {
    this.$get = [ '$http', function ( $http ) { return {
        items : [ ],
        fetch : function ( ) {
            $http( {
                method : 'GET', '/quotes.json'
            } ).then( function ( res ) {
                this.items // we want to keep the same array !
				  .splice( 0, this.items.length )
				  .push.apply( this.items, res.data.items );
            }.bind( this ), function ( res ) {
                console.error( res.data.error );
            } );
        }
    }; } ];
} );
</pre>

---

title: Utilisation du service

<pre class="prettyprint" data-lang="javascript">
function MyController( $scope, quotes ) {
  $scope.quotes = quotes.items;
  $scope.refresh = function ( ) {
    quotes.fetch( );
  };
}
</pre>

<pre class="prettyprint" data-lang="html">
&lt;div ng-controller="MyController"&gt;
  &lt;button ng-click="refresh()" value="Refresh quotes" /&gt;
  &lt;ul&gt;
    &lt;li ng-repeat="quote in quotes"&gt;
      "{{ quote.text }}" - by {{ quote.author }}
    &lt;/li&gt;
  &lt;/ul&gt;
&lt;/div&gt;
</pre>
