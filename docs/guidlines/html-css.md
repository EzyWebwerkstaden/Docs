### Html
We usally use bootstrap3 html. That's are current standard.

### Css
We try to use .less for all css coding here on ezy. So if you need to change an legacy project.
You should add a new .less file with the same name and copy the .css file to that. So we can start getting .less in.

Also we should always avoid in-line css and use css files for all CSS.

#### Naming

#### Capitalizing
We will always name our classes with lowercase.

for example:
`.my-button { ... }` 

Element id's should be camel cased. 

for example:
`#myButton { ... }` 

#### Semantic naming

We should name our classes on what they do and not a color or what css it contains. For example  
 `.confirm-button` is fine , but not `.blue-button`

## LESS
LESS files should be loaded in a modular way where the partial files is prefixed with a underscore.  
example : 
`@import "_animations.less";`  
This LESS file will not compile into a css file (since we are loading it in a modular way the css file is not needed).

By loading LESS files in this way we get more structure and are able to reuse the files needed in each part of the page.
Keep LESS modules focused. In other words, a less module should only style one specific component. Use common sense, but don't go overboard. ;) Sometimes it's more practical and logical to let a module handle more than one component.

#### LESS nesting
Avoid nesting LESS selectors more than 4 levels. Nesting in LESS is great, but if done incorrectly it could lead to dependencies of a certain DOM structure and lengthy selectors in your CSS document. It also makes it difficult to overwrite the styles in your media queries, since you have to recreate the entire nested structure.

LESS nested classes should not mirror HTML structure. If you find yourself nesting more than 4 levels, stop and re-think. Please read the link below for a more detailed understanding of why nesting selectors can be a bad idea.

The Inception Rule: don’t go more than four levels deep.
http://thesassway.com/beginner/the-inception-rule


#### LESS/CSS naming conventions
Use BEM when naming your CSS classes. There might be some resistance initially since it forces you to add more (and longer) classes in your HTML templates. However, once you get used to working like this, you will see the benefit.
http://getbem.com/introduction/

Key advantages:

- Avoid LESS nesting issues, where you have to recreate the same nesting in order to overwrite the styles.
- Creates a more "modular" CSS structure, where you think of a components root node as the identifier
- Makes it really clear and easy to figure out
 - where the current CSS is used
 - what does each BEM block contain
 - the hierarchy of elements,
- Minimizes conflicing class names, since all class names contain the root identifier.
- Minimizes dependencies to specific DOM elements. Like instead of doing `.flight-info strong` (which forces the HTML structure to look a certain way), you should do `.flight-info__price`. This approach is somehow conflicting with typical usage of favourite grid systems (like bootstrap) where we do most styling & positioning by applying bootstrap classes directly on the HTML level. The way to still use bootstrap while coding the BEM-way is to reference `.strong` inside your `.flight-info__price` element. This requires however to have bootstrap sources available in .less files. As always, there are exceptions to the rules, like some cross-cutting concerns that will just work only when separate class is added in the html level. The example could be a `.caps` class (to capitalize font at specific place). As with everything, try to find a right balance - this is the toughest part though.
- Due to strictly controlled scoping of elements and avoiding writing globally-available-css, we have less fear of refactoring the css. This implies less duplication of code, as when we don't fear refactoring, the less likely we're going to introduce duplication.
- flat css structure and using classess-only for styling makes your css faster to apply by browsers.

DO:

- Try use one component-per-file approach
- use `&--` in .less files to not clutter them with repeated prefixes.
- make each BEM block unique and self-sufficient.

DO NOT:

- override modifiers on an unrelated block inside another block
- make unnecessary parent elements when child can exist happily by itself.
- create more than two levels of elements. There is only one 'element' level in BEM. Do `person__eye` instead of `person__head__eye`
- make your compoments depenent on id, nor tag. Use classes **only** for styling inside your bloks.

**Example 1**
HTML (with some KnockoutJS):
```html
<div class="offer" data-bind="css: {'offer--with-discount': hasDiscount()}">
    <img class="offer__image" src="path/to/promo-image.jpg" />
    <h2 class="offer__headline">Time limited offer!</h2>
    <p class="offer__description">Some description text</p>
    <button class="offer__button">View</button>
</div>
```
CSS:
```less
.offer {
    border: 1px solid rgba(0,0,0,.4);
    padding: 10px;

    &--with-discount {
        background: yellow;
        border: 1px solid red;

        .offer__button {
            background-color: red;
        }
    }

    &__image {
        width: 100%;
    }

    &__headline {
        text-transform: uppercase;
    }

    &__description {
        font-weight: bold;
    }

    &__button {
        background-color: green;
    }
}
```

And remember the "no more than 4 levels" nesting rule. When using BEM, you should keep nesting to a minimum.
Use descriptive class names/selectors. It's better to have a long-ish selector, than to wonder wtf it's doing.

#### Convert Images to svg and make a font file of it.
The main cause to convert these to font files is that icons will be easy to use, you will just add a span-elememnt with your icon and you will be able to color it by using variables and much easier to scale without loosing quality. and you can use it with font sizing which also adepts to the screen size so win win.

1. create svg files over the icons you want to create
2. head to [icomoon iconlibrary helper](https://icomoon.io/app/#/select)
3. import all files you want to merge together
4. select all files.
5. go to the tab for generating font
6. adjust settings to match the library your creating
7. download & implement, font is created enjoy

Sources: 
* https://icomoon.io/app/#/select
* https://rileymacdonald.ca/2013/10/10/how-to-create-custom-glyphicon-fonts/


#### Mobile first
Always start "designing" for mobile first, and then scale up. This will mean that the browser will skip applying styles that are not relevant for the current viewport size.
If you go the other way round (desktop first), the browser will first apply all desktop styles, and then the tablet and mobile styles. This is bad for performance.
It's a good idea to specify LESS variables for different viewport sizes, since it makes understanding the intent of the code easier.

**Example 2**
HTML:
```html
<div class="contact-banner">
    <img class="contact-banner__icon" src="call-us-icon.png" />
    <strong>Call us: 0893 34434</strong>
</div>
```

LESS:
```less
.contact-banner {
    // styles for mobile
    &__icon {
        display: none;
    }

    // styles for tablet (and above)
    @media @viewport-tablet-and-larger {    // using less variable
        float: right;
        max-width: 200px;

        &__icon {
            display: inline-block;
            width: 30px;
        }
    }

    // specific for desktop
    @media @viewport-desktop {
        max-width: 250px;

        &__icon {
            width: 50px;
        }
    }
}
```


#### LESS Selector order
To increase readability and navigation of your code, you might want to consider ordering your CSS classes/selectors in the same way they appear on screen. Might not always be applicable due to different layouts in different viewport sizes. But normally it's good practice to use mobile first order as a basic template. Example 1 uses this approach.
