# Notas gerais de CSS, conselhos e diretrizes

---

## Traduções

* [Inglês](https://github.com/csswizardry/CSS-Guidelines)
* [Russo](https://github.com/matmuchrapna/CSS-Guidelines/blob/master/README%20Russian.md)
* [Chines](https://github.com/chadluo/CSS-Guidelines/blob/master/README.md)
* [Francês](https://github.com/flexbox/CSS-Guidelines/blob/master/README.md)
* [Portugues](https://github.com/msodeveloper/CSS-Guidelines)

---

Trabalhando em um projeto grande, com dezenas de desenvolvedores, é importante
que trabalhemos de um jeito unificado, para:

* Manter a folha de estilo sustentável
* Manter o código transparente e legível
* Manter a folha de estilo escalável

Existe uma variedade de técnicas que precisamos seguir para satisfazer esses
objetivos.

A primeira parte desse documento lida com sintaxe, formatação e anatomia
do CSS, a segudna parte lida com a abordagem, mindframe, e a atitude em relação
a escrita e arquitetura do CSS. Existante né?

## Conteúdo

* [Anatomia do documento CSS](#css-document-anatomy)
  * [Geral](#general)
  * [Um arquivo vs. Muitos arquivos](#one-file-vs-many-files)
  * [Tabela de conteúdos](#table-of-contents)
  * [Títulos da seção](#section-titles)
* [Ordem do código](#source-order)
* [Anatomia do conjunto de regras](#anatomy-of-rulesets)
* [Convenções para nomes](#naming-conventions)
  * [JS hooks](#js-hooks)
  * [Internacionalização](#internationalisation)
* [Comentários](#comments)
  * [Comments on steroids](#comments-on-steroids)
    * [Quasi-qualified selectors](#quasi-qualified-selectors)
    * [Tagging code](#tagging-code)
    * [Object/extension pointers](#objectextension-pointers)
* [Escrevendo CSS](#writing-css)
* [Construindo novos componentes](#building-new-components)
* [OOCSS](#oocss)
* [Layout](#layout)
* [Tamnho das UIs](#sizing-uis)
  * [Tamanho das fontes](#font-sizing)
* [Taquigrafia](#shorthand)
* [IDs](#ids)
* [Seletores](#selectors)
  * [Seletores super qualificados](#over-qualified-selectors)
  * [Performance de Seletores](#selector-performance)
* [CSS selector intent](#css-selector-intent)
* [`!important`](#important)
* [Números mágicos e absolutos](#magic-numbers-and-absolutes)
* [Stylesheet condicional](#conditional-stylesheets)
* [Debugando](#debugging)
* [Preprocessadores](#preprocessors)

---

## Anatomia do documento CSS

Não importa o documento, nós precisamos seguir um formato padrão. Isso
significa comentários consistentes, sintaxe consistente e nomeação consistente.

### Geral

Limite suas Stylesheets para o máximo 80 caracteres de largura onde possível.
Excessões podem ser sintaxes de gradiente, e URLS nos comentários. Ai tudo bem, não
tem nada que possamos fazer sobre isso.

Eu prefiro quatro (4) espaços de identação ao invés de tabs, e escrever CSS em multiplas linhas.

### Um arquivo vs. Varios arquivos

Algumas pessoas preferem trabalhar com um único, e gigante arquivo. Não tem problema, e seguindo
os próximos guidelines você não vai encontrar nenhum problema. Desde que mudei
para Sass, eu comecei a quebrar meus stylesheets em varios e pequenos pedaços.
Isso também não tem problema... Seja qual for o método que você escolher, as seguintes regras e
guidelines se aplicam. A única diferença notável é com relação a nossa tabela de
conteúdos e os nossos títulos de seção. Continue lendo para mais explicação...

### Tabela de conteúdos

No topo da nossa folha de estilo, Eu mantenho a tabela de conteúdos a qual vai 
detalhar a seção contida no documento, por exemplo:

    /*------------------------------------*\
        $CONTEUDOS
    \*------------------------------------*/
    /**
     * CONTEUDOS............Você esta lendo isso!
     * RESET...............Padroniza nosso reset
     * FONT-FACE...........Importar arquivos de fonte
     */

Isso vai avisar o próximo(s) desenvolvedor(es) exatamente o que eles esperam achar
nesse arquivo. Cada item na tabela de conteúdos mapeia diretamente para a seção do título.

Se você está trabalhando com uma única e grande folha de estilo, a seção correspondente
também vai estar nesse arquivo. Se você está trabalhando com multiplos arquivos, então
cada item na tabela de conteúdos vai mapear para um include que se refere a aquela seção.

### Títulos de Seções

A tabela de conteúdos não teria uso, a menos que ela tenha seções correspondentes
a seus titulos. Escreva uma seção assim:

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/

Prefixando o nome da seção com `$` nos permite rodar um comando de busca ([Cmd|Ctrl]+F)
pelo `$[NOME-DA-SEÇÃO]` e **limita nossa pesquisa para somente títulos de seções**.

Se você já trabalha em uma folha de estilo grande, Deixe cinco (5) quebras de linhas
entre cada seção

    /*------------------------------------*\
        $RESET
    \*------------------------------------*/
    [Nossos
    estilos
    do reset]





    /*------------------------------------*\
        $FONT-FACE
    \*------------------------------------*/

Essa grande separação de quebras de linha é facilmente notavel quando
você está escrollando em um arquivo grande.

Se você trabalha com multiplos arquivos, folhas de estilo incluidas, comece
cada um desses arquivos com um título de seção, e não tem porque você fazer
quebras de linhas.

## Ordem do código

Tente escrever as folhas de estilo em uma ordem específica. Isso garante que você 
tire o máximo proveito de heranças e o primeiro C do CSS; a cascata.

Uma folha de estilo bem organizada parece com isso:

1. **Reset** – o marco zero.
2. **Elementos** – `h1` sem classe, `ul` sem classe, etc...
3. **Objetos e abstrações** — generico, pardões de design subjacentes.
4. **Componentes** – componentes completos construidos dos objetos e suas extensões.
5. **Estilos com !important** – erros, estados, etc.

This means that—as you go down the document—each section builds upon and
inherits sensibly from the previous one(s). There should be less undoing of
styles, less specificity problems and all-round better architected stylesheets.

Para mais informaçãos, eu recomendo altamente o [SMACSS](http://smacss.com)
do Jonathan Snook.

## Anatomia do conjunto de regras

    [seletor]{
        [propriedade]:[valor];
        [<- Declaracao ->]
    }

Eu tenho alguns padrões quando estou estruturando conjuntos de regras

* Use hífem para delimitar nomes de classes(exceto por notações BEM, [veja abaixo](#naming-conventions))
* 4 espaços de identação
* Multi-linhas
* Declarações em ordem de relevancia (NÃO alfabetica)
* Idente declarações prefixadas, para que seus valores estejam alinhados
* Idente seus conjuntos de regra para ser um espelho do DOM
* Sempre inclua o ponto e virgula no final de um conjunto de regra

Um pequeno exemplo:

    .widget{
        padding:10px;
        border:1px solid #BADA55;
        background-color:#C0FFEE;
        -webkit-border-radius:4px;
           -moz-border-radius:4px;
                border-radius:4px;
    }
        .widget-heading{
            font-size:1.5rem;
            line-height:1;
            font-weight:bold;
            color:#BADA55;
            margin-right:-10px;
            margin-left: -10px;
            padding:0.25em;
        }

Aqui podemos ver que `.widget-heding` deve ser uma filha de `.widget` como nós
identamos o `.widget-heading` um level mais a dentro do que `.widget`. Isso é 
uma informação útel para desenvolvedores que podem se achar apenas por um olhar 
na identação de regras.

Nós podemos ver também que as declarações de `.widget-heading` estão ordenadas
por relevância; `.widget-heading` deve ser um elemento contextual entao nós
começamos por regras de texto, seguido por todo o resto.

Uma exceção para nossa regra multi-linha pode ser a seguinte:

    .t10    { width:10% }
    .t20    { width:20% }
    .t25    { width:25% }       /* 1/4 */
    .t30    { width:30% }
    .t33    { width:33.333% }   /* 1/3 */
    .t40    { width:40% }
    .t50    { width:50% }       /* 1/2 */
    .t60    { width:60% }
    .t66    { width:66.666% }   /* 2/3 */
    .t70    { width:70% }
    .t75    { width:75% }       /* 3/4*/
    .t80    { width:80% }
    .t90    { width:90% }

Nesse exemplo (de [inuit.css’s table grid system](https://github.com/csswizardry/inuit.css/blob/master/inuit.css/partials/base/_tables.scss#L88))
faz mais sentido usar uma única linha do nosso CSS.

## Convenções de Nome

Para a maioria das partes eu simplesmente uso classes delimitadas por hífem (ex. `.foo-bar`, não 
`.foo_bar` ou `.fooBar`), Entretanto em certas circunstancias eu uso notação BEM (Block,
Element, Modifier).

<abbr title="Block, Element, Modifier">BEM</abbr> é uma metodologia para nomear
e seletores de CSS de um jeito que os deixem mais restritos, transparentes e informativos

Essa convenção de nomes segue esse padrão:

    .block{}
    .block__element{}
    .block--modifier{}

* `.block` representa o maior level de abstração ou componente.
* `.block__element` representa um descendente de `.block` que ajuda a formar `.block`
    como um todo.
* `.block--modifier` representa um estado diferente ou versão de `.block`.

Uma **analogia** de como as classes BEM podem funcionar:

    .person{}
    .person--woman{}
        .person__hand{}
        .person__hand--left{}
        .person__hand--right{}

Aqui podemos ver que o objeto basico que estamos descrevendo é a pessoa (person), e que um
tipo diferente de pessoa poderia ser a mulher (woman). Nós podemos perceber que people tem 
mãos (hands); esses são sub-partes de pessoa (people), e ha diferentes variações,
como esquerda(left) e direita (right).

Nós podemos nomear nossos seletores baseados na base de seus objetos e nós podemos também
comunicar que trabalho o seletor faz; ele é um sub-componente (`__`) ou uma variação
(`--`)?

Então, `.page-wrapper` é um seletor autônomo; ele não faz parte de uma abstração
ou componente  and as such it named correctly (traduzir). `.widget-heading`,
entretanto, _é_ relacionado a um componente; ele é um filho de `.widget`
então nós renomeariamos essa classe para `.widget__heading`.

BEM parece umpouco feio, e é muito mais detalhado, mas ele nos fornece muito
poder para que possamos pegar as funções e relações de elementos de suas classes 
independentes. A sintaxe do BEM vai tipicamente comprimir (gzip) muito bem já 
que a compressão favorece/trabalha bem com repetições.

Independentemente se precisa usar o BEM ou não, sempre garanta que as classes
sejam sensatamente nomeadas; as mantenha o quao curtas quanto possível mas quão longas
quanto necessário. Garanta que qualquer objeto ou abstração sejam vagamente nomeadas (ex. `ui-list`, `.media`)
para garantir o reuso. Extensões de objetos devem ser bem mais explicitamente
nomeadas (ex. `user-avatar-link`). Não se preocupe com o tamanho das classes;
gzip vai sempre comprimir códigos bem escritos _incredibly_ well.

### Classes no HTML

Em uma tentativa de tornar as coisas masi fácil de ler, separar classes no seu HTML em 
dois (2) espaço, assim:

    <div class="foo--bar  bar__baz">

Esse espaço em branco deve facilitar para detectar e ler multiplas classes.

### JS hooks

**Nunca use o estilo das classes de CSS como um gancho de Javascript. Atribuir
comportamentos de Javascript para uma classe de estilo significa que nós nunca poderemos
ter uma sem a outra.

Se você precisa vincular algum marcação use uma classe específica de Javascript.
Isso é simplesmente uma classe nomeada com `.js-`, ex. `js-toggle`, `.js-drag-and-drop`.
Isso significa que nós podemos atribuir classes JS e CSS na nossa marcação e nunca 
haverá algum sobreposição problemática.

    <th class="is-sortable  js-is-sortable">
    </th>

A marcação acima contem 2 classes; uma o qual você pode atribuir algum estilo
para colunas de tabelas classíficaveis e outra o qual te permite te adicionar
algum tipo de funcionalidade.

### Internacionalização

Apesar de ser um desenvolvedor britânico-e passado toda minha vida escrevendo <i>colour</i>
ao invés de <i>color</i>—Eu sinto que, por uma questão de consistência, é melhor 
sempre usar o US-English (inglês americano) em CSS. CSS, como eu muitas (se não todas) oturas linguas,
é escrita em US-English (inglês americano), entao misturar sintaxes como `color:red;` com classes como
`colour-picker{}` carece de consistência. Eu já sugeri e defendi escrever classes
bilinguas, como por exemplo:

    .color-picker,
    .colour-picker{
    }

Entretanto, tendo trabalhado em um projeto bem grande em Sass onde haviam duzias
de variaves de cor (ex. `$brand-color`, `$highlight-color` etc.), 
manter duas versões de cada variavel logo se tornou cansativa. Isso também significa
duas vezes o trabalho com coisas como Find e Replace.

Em uma questão de coerência, sempre nomeie as classes e variaveis na linguagem
do local em que você está trabalhando.

## Comentários

Eu uso o esquema de estilo de comentários docBlock, o qual eu limito a 80 caracteres de comprimento:

    /**
     * Este é um estilo de comentário docBlock
     *
     * Este é uma descrição masi longa do comentário, descrevendo o código em mais
     * detalhe. Nós limitamos as linhas para no máximo 80 caracteres de comprimento.
     *
     * Nós podemos ter marcação no comentário, e é bom faze-lo:
     *
       <div class=foo>
           <p>Lorem</p>
       </div>
     *
     * Nós não prefixamos linhas de código com um asterisco, pois faze-lo inibiria
     * copy and paste.
     */

Você deve documentar e comentar seu código tanto quanto você pode, o que pode
parecer transparente e auto explicatório para você pode nao ser para outro desenvolvedor.
Escreva seu código depois comente sobre ele.

### Comments on steroids

Existe varias técnicas avançadas que você pode empregar no que diz respeito
a comentários, 
There are a number of more advanced techniques you can employ with regards
comments, nomeadamente:

* Seletores quasi-qualificados
* Código Taggeado
* Ponteiros de Objetos/extensões

#### Seletores Quasi-qualificados

Você nunca deve qualificar seus seletores; isto é, nós nunca devemos escrever
`ul.nav{}` se você pode ter `.nav`. Seletores qualificados diminui a performance
do seletor, inibe o potencial de reusar a classe em diferentes tipos de 
elemento e aumenta a especificidade do seletor. Todas essas coisas devem ser 
evitadas a todo custo.

Porém, algumas vezes é útil comunicar o próximo desenvolvedor onde
você quer que uma classe seja usada. Vamos ver `.product-page` por exemplo; essa
classe parece que deve ser usada em um container de alto level, talvés o elemento
`html` ou o elemento `body`, mas com `.product-page` sozinho é impossivel de dizer.

Por quasi-qualificar esse seletor (ex. comentando o seletor de liderança)
nós conseguimos comunicar one nós desejamos que nossa classe seja aplicada, assim:

    /*html*/.product-page{}

Agora nós podemos ver exatamente onde aplicar essa classe mas sem nenhuma
especificação ou não reutilização inconveniente.

Outros exemplos podem ser:

    /*ol*/.breadcrumb{}
    /*p*/.intro{}
    /*ul*/.image-thumbs{}

Aqui podemos ver onde nós pretendemos que cada uma dessas classes sejam usadas
sem impactar a especificação dos seletores.

#### Taggeando código

Se você escrever um novo componente deixe alguma tag pertencente a essa funcao em
um comentário acima dele, por exemplo:

    /**
     * ^navigation ^lists
     */
    .nav{}

    /**
     * ^grids ^lists ^tables
     */
    .matrix{}

Essas tags permitem que outros desenvolvedores achem snippets do código pesquisando por
função; se um desenvolvedor precisar trabalhar com listas eles podem rodar um pesquiasr por
`^lists` e achar os objets `.nav` e `.matrix` (e possivelmente mais).

#### Apontadores de Objetos/Extensões

Ao trabalhar de forma orientada a objetos você vai frequentemente ter dois grupos de CSS
(um sendo o esqueleto (o objeto) e o outro sendo a pele (a extensão)) que sao relacionadas,
mas que vivem em lugares diferentes. A fim de estabelecer uma conexão concreta entre o objeto
e sua extensão use <i>apontadores objeto/extensão</i>. Eles são simplesmente comentarios
que trabalham assim:

Na sua folha de estilo base:

    /**
     * Extender `.foo` em theme.css
     */
     .foo{}

Na sua folha de estilo do tema:

    /**
     * Extender `.foo` em base.css
     */
     .bar{}

Aqui nós estabilizamos uma relação concreta entre dois pedaços bem diferentes
de código.

---

## Escrevendo CSS

A seção anterior lida com como devemos estruturar e formar nosso CSS; Elas eram
regras muito quantificáveis. A próxima seção é umpouco mais teorica e lida
com nossa atitudo e abordgem.

## Escrevendo novos componentes

Quando você está construindo um novo componente escreva a marcação **antes** do CSS.
Isso significa que você pode visualmente ver qual propriedade de CSS é naturalmente herdada
e entao evitar reaplicar redundancia de estilo.

Escrevendo a marcação antes você pode focar na data, conteúdo e semantica e depois
aplicar somente as classes relevantes e o css _depois_.

## OOCSS

Eu trabalho em uma maneira orientada objeta de CSS; Eu separo componentes em uma estrutura de (objetos) e
pele (extensões). Como uma analogia (note, não um exemplo) segue o seguinte:

    .room{}

    .room--kitchen{}
    .room--bedroom{}
    .room--bathroom{}

Nós temos vários tipos de quartos em uma casa, mas eles compartilham traços similares;
todos eles tem pisos, tetos, paredes e portas. Nós podemos compartilhas essa informação
em uma classe abstrata `.room{}`. Entretato nós temos tipos específicos de quartos que
podem ter carpetes, um banheiro pode não ter uma janela mas um quarto quase sempre vai,
cada quarto pode ter paredes coloridas. OOCSS nos ensina a abstrair os estilos compartilhados
em uma base de objeto e que _extende_ essa informação com mais classes específicas para adicionar
tratamento(s) específico(s).

Então, ao invés de construir dezenas de componentes únicos, tente e localize padrões de design
repetidos por eles e abstraia eles em uma classe reusável;
construa esses esqueletos como ‘objetos‘ base e depois transforme em classes para extender
seus estilos para circunstancias mais únicas.

Se você tiver que construir um novo componente, divida ele em estrutura e pele; construia a
estrutura do componente usando classes bem genéricas para que possamos reusar essa construção
e depois usar classes específicas criar a pele e depois adicionar o tratamento do design.

## Layout

Todos os componentes que você construir não deve conter widths; eles devem
sempre ser fluidos e seus widths devem ser governado por um sistema de grid.

Heights **nunca** devem ser aplicados a elementos. Heights devem somente ser aplicados
a coisas que tem dimensões _antes_ de eles terem entrado no site(ex. 
imagens e sprites). Nunca use heights em `p`s, `ul`s, `div`s, nada.
Você pode frequentemente alcançar o efeito desejado usando `line-height` o que é bem mais flexível.

Sistemas grid devem ser pensados como prateleiras. Eles contem o conteúdo mas não
são o conteúdo em sí mesmos. Você coloca suas prateleiras e depois as preenche com suas coisas.
Configurando nossos grids separadamentes de nossos componentes, você pode mover componentes
ao redor muito mais facíl do que se você tivesse dimensões aplicadas a eles; isso faz
nosso front-end muito mais adaptavel e rapido de trabalhar.

You nunca deve aplicar qualquer estilo para um item de grid, eles sao fins de layout
apenas. Aplicar estilo para um conteúdo _dentro_ de um item de grid. Nunca, sob _qualquer_ 
circunstancias, aplique propriedades de modelo-box a um item de grid.

## Dimensionando UIs

Eu uso uma combinação de métodos para dimensioamento de UIs. Porcentagens, pixels, ems, rems
e nada.

Sistemas de Grid deveriam, idealmente, ser configurados em porcentagens. Porque eu uso sistemas de grid
para administrar widths das colunas e paginas, eu posso deixar componentes totalmente livres de
qualquer dimensão (como discutido a acima).

Tamanhos de fontes eu uso em rems com fallbacks em pixels. Isso da os benefícios 
de acessibilidade de ems com confidencia de pixels. Aqui é uma mistura de Sass para
trabalhar com rem e fallback em pixels para você (assumindo que você configura o tamanho de fonte
base em variaveis em algum lugar);

    @mixin font-size($font-size){
        font-size:$font-size +px;
        font-size:$font-size / $base-font-size +rem;
    }

Eu só uso pixels para itens o qual dimensões foram definidas antes de entrar no site.
Isso inclui coisas como imagens e sprites o qual dimensões são inerentemente 
configuradas em pixels

### Dimensionamento da fonte

I define a series of classes akin to a grid system for sizing fonts. These
classes can be used to style type in a double stranded heading hierarchy. For a
full explanation of how this works please refer to my article
[Pragmatic, practical font-sizing in CSS](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css)

## Shorthand

**Shorthand CSS needs to be used with caution.**

It might be tempting to use declarations like `background:red;` but in doing so
what you are actually saying is ‘I want no image to scroll, aligned top-left,
repeating X and Y, and a background colour of red’. Nine times out of ten this
won’t cause any issues but that one time it does is annoying enough to warrant
not using such shorthand. Instead use `background-color:red;`.

Similarly, declarations like `margin:0;` are nice and short, but
**be explicit**. If you actually only really want to affect the margin on
the bottom of an element then it is more appropriate to use `margin-bottom:0;`.

Be explicit in which properties you set and take care to not inadvertently unset
others with shorthand. E.g. if you only want to remove the bottom margin on an
element then there is no sense in setting all margins to zero with `margin:0;`.

Shorthand is good, but easily misused.

## IDs

A quick note on IDs in CSS before we dive into selectors in general.

**NEVER use IDs in CSS.**

They can be used in your markup for JS and fragment identifiers but use only
classes for styling. You don’t want to see a single ID in any stylesheets!

Classes come with the benefit of being reusable (even if we don’t want to, we
can) and they have a nice, low specificity. Specificity is one of the quickest
ways to run into difficulties in projects and keeping it low at all times is
imperative. An ID is **255** times more specific than a class, so never ever use
them in CSS _ever_.

## Selectors

Keep selectors short, efficient and portable.

Heavily location-based selectors are bad for a number of reasons. For example,
take `.sidebar h3 span{}`. This selector is too location-based and thus we
cannot move that `span` outside of a `h3` outside of `.sidebar` and maintain
styling.

Selectors which are too long also introduce performance issues; the more checks
in a selector (e.g. `.sidebar h3 span` has three checks, `.content ul p a` has
four), the more work the browser has to do.

Make sure styles aren’t dependent on location where possible, and make sure
selectors are nice and short.

Selectors as a whole should be kept short (e.g. one class deep) but the class
names themselves should be as long as they need to be. A class of `.user-avatar`
is far nicer than `.usr-avt`.

**Remember:** classes are neither semantic or insemantic; they are sensible or
insensible! Stop stressing about ‘semantic’ class names and pick something
sensible and futureproof.

### Over-qualified selectors

As discussed above, qualified selectors are bad news.

An over-qualified selector is one like `div.promo`. You could probably get the
same effect from just using `.promo`. Of course sometimes you will _want_ to
qualify a class with an element (e.g. if you have a generic `.error` class that
needs to look different when applied to different elements (e.g.
`.error{ color:red; }` `div.error{ padding:14px; }`)), but generally avoid it
where possible.

Another example of an over-qualified selector might be `ul.nav li a{}`. As
above, we can instantly drop the `ul` and because we know `.nav` is a list, we
therefore know that any `a` _must_ be in an `li`, so we can get `ul.nav li a{}`
down to just `.nav a{}`.

### Selector performance

Whilst it is true that browsers will only ever keep getting faster at rendering
CSS, efficiency is something you could do to keep an eye on. Short, unnested
selectors, not using the universal (`*{}`) selector as the key selector, and
avoiding more complex CSS3 selectors should help circumvent these problems.

## CSS selector intent

Instead of using selectors to drill down the DOM to an element, it is often best
to put a class on the element you explicitly want to style. Let’s take a
specific example with a selector like `.header ul{}`…

Let’s imagine that `ul` is indeed the main navigation for our website. It lives
in the header as you might expect and is currently the only `ul` in there;
`.header ul{}` will work, but it’s not ideal or advisable. It’s not very future
proof and certainly not explicit enough. As soon as we add another `ul` to that
header it will adopt the styling of our main nav and the the chances are it
won’t want to. This means we either have to refactor a lot of code _or_ undo a
lot of styling on subsequent `ul`s in that `.header` to remove the effects of
the far reaching selector.

Your selector’s intent must match that of your reason for styling something;
ask yourself **‘am I selecting this because it’s a `ul` inside of `.header` or
because it is my site’s main nav?’**. The answer to this will determine your
selector.

Make sure your key selector is never an element/type selector or
object/abstraction class. You never really want to see selectors like
`.sidebar ul{}` or `.footer .media{}` in our theme stylesheets.

Be explicit; target the element you want to affect, not its parent. Never assume
that markup won’t change. **Write selectors that target what you want, not what
happens to be there already.**

For a full write up please see my article
[Shoot to kill; CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)

## `!important`

It is okay to use `!important` on helper classes only. To add `!important`
preemptively is fine, e.g. `.error{ color:red!important }`, as you know you will
**always** want this rule to take precedence.

Using `!important` reactively, e.g. to get yourself out of nasty specificity
situations, is not advised. Rework your CSS and try to combat these issues by
refactoring your selectors. Keeping your selectors short and avoiding IDs will
help out here massively.

## Magic numbers and absolutes

A magic number is a number which is used because ‘it just works’. These are bad
because they rarely work for any real reason and are not usually very
futureproof or flexible/forgiving. They tend to fix symptoms and not problems.

For example, using `.dropdown-nav li:hover ul{ top:37px; }` to move a dropdown
to the bottom of the nav on hover is bad, as 37px is a magic number. 37px only
works here because in this particular scenario the `.dropdown-nav` happens to be
37px tall.

Instead you should use `.dropdown-nav li:hover ul{ top:100%; }` which means no
matter how tall the `.dropdown-nav` gets, the dropdown will always sit 100% from
the top.

Every time you hard code a number think twice; if you can avoid it by using
keywords or ‘aliases’ (i.e. `top:100%` to mean ‘all the way from the top’)
or&mdash;even better&mdash;no measurements at all then you probably should.

Every hard-coded measurement you set is a commitment you might not necessarily
want to keep.

## Conditional stylesheets

IE stylesheets can, by and large, be totally avoided. The only time an IE
stylesheet may be required is to circumvent blatant lack of support (e.g. PNG
fixes).

As a general rule, all layout and box-model rules can and _will_ work without an
IE stylesheet if you refactor and rework your CSS. This means you never want to
see `<!--[if IE 7]> element{ margin-left:-9px; } < ![endif]-->` or other such
CSS that is clearly using arbitrary styling to just ‘make stuff work’.

## Debugging

If you run into a CSS problem **take code away before you start adding more** in
a bid to fix it. The problem exists in CSS that is already written, more CSS
isn’t the right answer!

Delete chunks of markup and CSS until your problem goes away, then you can
determine which part of the code the problem lies in.

It can be tempting to put an `overflow:hidden;` on something to hide the effects
of a layout quirk, but overflow was probably never the problem; **fix the
problem, not its symptoms.**

## Preprocessors

Sass is my preprocessor of choice. **Use it wisely.** Use Sass to make your CSS
more powerful but avoid nesting like the plague! Nest only when it would
actually be necessary in vanilla CSS, e.g.

    .header{}
    .header .site-nav{}
    .header .site-nav li{}
    .header .site-nav li a{}

Would be wholly unnecessary in normal CSS, so the following would be **bad**
Sass:

    .header{
        .site-nav{
            li{
                a{}
            }
        }
    }

If you were to Sass this up you’d write it as:

    .header{}
    .site-nav{
        li{}
        a{}
    }
