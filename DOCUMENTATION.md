# Votre système de nomenclature CSS (pourquoi ce choix?)

J’ai choisi d’utiliser la méthode BEM (Block, Element, Modifier) parce qu’elle rend le code plus facile à lire et à organiser. On comprend rapidement la structure du site juste en lisant les classes CSS.

Le principe est simple :

- le bloc représente une section (ex : header)
- l’élément est une partie de ce bloc (ex : header__nav)
- le modificateur permet de changer le style (ex : contents--dark)

Par exemple, au lieu d’avoir un nom compliqué comme:
> .hero__containerbutton_secondary,

j’ai utilisé **.hero__btn--outline**, qui est plus clair :

> hero → le bloc <br>
> btn → l’élément <br>
> --outline → la variante du bouton

# Vos variables CSS et design tokens (logique d'organisation)

Les variables CSS sont classées par catégories (couleurs, texte, espacements, etc.) pour garder un code organisé et cohérent. Au lieu d’utiliser des valeurs fixes partout, j’utilise des variables. Ça permet de changer une valeur une seule fois et que tout le site se mette à jour automatiquement. 

Par exemple, si je modifie:

> `--color-accent`

tous les éléments qui l’utilisent changent en même temps.

J’utilise aussi des tailles comme `xs`, `sm`, `md`, `lg`, `xl` pour mieux m’y retrouver et garder une logique claire. Les valeurs fixes sont seulement utilisées dans des cas très précis, quand elles ne sont pas réutilisées ou rarement réutilisées.

# Liste des composants réutilisables

Les composants suivants ont été créés :

- header
- hero
- contents
- list
- reviews
- form
- footer

Le composant le plus réutilisable est **contents** car il est utilisé à la fois pour la section "À propos" et "Services". Le modificateur contents--dark permet d'adapter l'apparence sans dupliquer le code CSS. Ce qui permet de faciliter la maintenance.

# Décisions Flexbox pour les layouts complexes

Le code Flexbox de base provient du Dev Mode de Figma et a été adapté lors de l'intégration :

- `.header__logo` : Ajout de `display: flex`, `justify-content: center` et `align-items: center` pour centrer la lettre B dans le carré du logo

- `.header__brand-title` : Changement de `flex-direction` de `column` à `row` pour que le titre s'affiche horizontalement

- `.contents__row` : Retrait de `align-items: center` pour que les cartes s'étirent à la même hauteur

- `.contents__card-image` : Ajout de `justify-content: center` et `align-items: center` pour centrer les images dans leur carré

- `.contents__avatar-group` : Retrait de `flex: 1 0 0` pour que l'avatar reste aligné à gauche

# Fonctions CSS fluides (clamp/calc)

Le `clamp()` a été utilisé dans :

- Dans `--text-xl: clamp(24px, 4vw, 40px)`
> Pour que la taille du titre principal s'adapte à la largeur de l'écran. Elle ne sera jamais plus petite que 24px ni plus grande que 40px.

- Dans `--section-padding: var(--space-3xl) clamp(20px, 10vw, 170px)`
> Le padding horizontal des sections s'adapte à la largeur de l'écran. Il réduit automatiquement sur les petits écrans au lieu de rester figé à 170px.

&

Le `calc()` à été utilisé dans :

- `.form__field { width: calc((100% - 80px * 2) / 3) }`
> Pour qu'il calcule la largeur de chaque champ en prenant la largeur totale de la ligne, en soustrayant les deux espaces de 80px entre les champs, puis en divisant par 3. Cela remplace la valeur `313.333px` de départ.

# Défis techniques rencontrés et solutions

J'ai rencontré **énorméments** de défis techniques lors de la réalisation de ce projet donc voici les plus mémorables:

- La feuille du logo se positionnait par rapport à la page entière au lieu du logo. <br>
> Solution : ajout de `position: relative` sur `.header__logo` pour que l’élément en `position: absolute` se place correctement par rapport à lui.

--

- Les spans du titre se collaient ensemble. <br>
> Solution : Ajout du caractère coréen vide (ㅤ) (U+3164) dans le HTML 
`<span class="header__brand-title--accent">Actionㅤ</span>` et `<span class="header__brand-title--accent">èteㅤ</span>`

--

- Les icônes des cartes n'étaient pas centrées. <br>
> Solution : ajout de `justify-content: center` et `align-items: center` sur `.contents__card-image`, et `object-fit: contain` sur l'image.

--

- Les cartes n'avaient pas la même hauteur. <br>
> Solution : retrait de `align-items: center` sur `.contents__row` qui empêchait les cartes d'avoir la même taille et hauteur.

--

- La section *Témoignages* ne rétrécissait pas. <br>
> Solution : retrait de `white-space: nowrap` sur `.reviews__comment`.