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

* [Anatomia do documento CSS](#anatomia-do-documento-css)
  * [Geral](#geral)
  * [Um arquivo vs. Muitos arquivos](#um-arquivo-vs-varios-arquivos)
  * [Tabela de conteúdos](#tabela-de-conteudos)
  * [Títulos da seção](#titulos-de-secoes)
* [Ordem do código](#ordem-do-codigo)
* [Anatomia do conjunto de regras](#anatomia-do-conjunto-de-regras)
* [Convenções para nomes](#convencoes-para-nomes)
  * [JS hooks](#js-hooks)
  * [Internacionalização](#internacionalizacao)
* [Comentários](#comentarios)
  * [Comments on steroids](#comments-on-steroids)
    * [Quasi-qualified selectors](#seletores-quasi-qualificados)
    * [Tagging code](#aggeando-c%C3%B3digo)
    * [Object/extension pointers](#apontadores-de-objetosextens%C3%B5es)
* [Escrevendo CSS](#escrevendo-css)
* [Construindo novos componentes](#escrevendo-novos-componentes)
* [OOCSS](#oocss)
* [Layout](#layout)
* [Dimensionando UIs](#dimensionando-uis)
  * [Dimensionamento das fontes](#dimensionamento-da-fonte)
* [Taquigrafia](#taquigrafia)
* [IDs](#ids)
* [Seletores](#seletores)
  * [Seletores super qualificados](#seletores-super-qualificados)
  * [Performance de Seletores](#performance-de-seletores)
* [CSS selector intent](#identa%C3%A7%C3%A3o-de-seletores-em-css)
* [`!important`](#important)
* [Números mágicos e absolutos](#n%C3%BAmeros-m%C3%A1gicos-e-absolutes)
* [Folhas de estilo condicionais](#folhas-de-estilo-condicionais)
* [Debugging](#debugging)
* [Pré-processadores](#pr%C3%A9-processadores)

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

Isso significa que -conforme você desce seu documento- cada seção se baseia e
herda sensatamente do anterior(es). Deve haver menos perda de estilos, 
menos problemas de especificidade e uma melhor arquitetura no seu documento.

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

Eu defino uma série de classes semelhante a um sistema de grid pelos dimensioamentos de fonte. Essas
classes podem ser usadas para tipo de estilo em uma hierarquia de titulos de cadeia dupla. Para uma 
explicação completa de como isso funciona acesse meu artigo 
[Pragmatic, practical font-sizing in CSS](http://csswizardry.com/2012/02/pragmatic-practical-font-sizing-in-css)

## Taquigrafia

**Taquigrafia em CSS precisa ser usada com precaução.**

Deve ser tentador usar declarações como `background:red;` mas fazendo isso
o que você realmente diz é Eu não quero imagens para scrolls, alinhado top-left,
repetindo X e Y, e a cor de fundo vermelho‘. Nove vezes de dez isso não vai causar
nenhum problema, mas dessa que ele causa é irritante o suficiente para garantir
para não usar taquigrafia. Ao invés use `background-color: red;`.

Declarações similares como `margin:0;` são boas e curtas, mas **seja explicito**.
Se você realmente quer somente que afete a margin no bottom de um elemento, então
é mais propriado que você use `margin-bottom:0;`.

Seja explicito em qual propriedade você configura e toma cuidado não inadvertidamente desconfigurar
outras com taquigrafia. Ex. se você só quer remover o bottom margin em um elemento
depois não faz sentido em configurar todas as margins para zero com `margin: 0`;

Taquigrafia é bom, mas facilmente mal usada.

## IDs

Uma rápida observação sobre IDs em CSS antes de entrarmos em seletores geral.

**NUNCA use IDs em CSS.**

Eles podem ser usado na sua marcação para JS e identificadores de fragmentos mas use somente
classes para estilizar. Você não quer ver um único ID em qualquer folha de estilo!

As classes bem com o benefício de serem reusáveis (mesmo se não quisermos, nós podemos)
E elas tem uma boa, e baixa especificidade. Especificidade é um dos jeitos mais rápidos
de encontrar dificuldades em projetos. Um ID é **255** vezes mais específico do que uma class,
então nunca use-os em CSS _nunca_.

## Seletores

Mantenha seletores curtos, eficientes e portaveis.

Seletores pesadamente baseados em localidades são ruim por inúmeras razões. Por exemplo,
peque `.sidebar h3 spanP{`. Esse seletor é muito baseado em localidade e entao nós 
não pdoemos mover esse `span` fora de um `h3` e fora de um `.sidebar` e manter o estilo.

Seletores que são muito compridos também nos trazem problemas de performance; quanto mais itens
em um seletor (ex. `.sidebar h3 span` tem tres itens, `.content ul p a` tem quatro), masi o browser
tem que trabalhar.

Certifique-se que estilos não dependam da localização onde possível, e certifique-se que 
seletores sejam bons e pequenos.

Seletores como um todo deveriam ter mantidos pequenos (ex. uma classe de profundidade) mas o nome da classe
em sói deveria ser quao grande quanto necessário. Uma classe de `user-avatar` é muito
melhor do que `.usr-avt`.

**Lembre-se** classes não são nem semanticas ou isemanticas; elas são sensíveis ou
insensíveis! Pare de se estressar sobre  nomes de classes ‘semanticos‘ e escolha algo
sensível e a prova de complicações no futuro.

### Seletores Super-qualificados

Como discutido acima, seletores qualificados são más novidades.

Um seletor super-qualificado é como `div.promo`. You provavelmente poderia ter o mesmo
efeito somente usando `.promo`. Claro você vai _querer_ qualificar uma classe com um elemento
(ex. se você tem uma class `.error` genérica que precise parecer diferente qunado aplicada 
a um elemento diferente (ex. `.error{color:redl}` `div.error{padding: 14px}`)), mas evite
isso quando possível.

Outro exemplo de seletores super-qualificados pode ser `ul.nav li a{}`. Como visto
acima, nós podemos instantaneamente tirar o `ul` e por isso nós sabems que `.nave` é uam lista, 
portante sabemos que qualquer `a` _deve_ estar em uma `li`, para que possamos ter `ul.nav li a{}` 
simplificado para somente `.nav a{}`.

### Performance de Seletores

Enquanto é verdade que browsers sempre irão ficar mais rápido a reinderizar CSS, 
eficiencia é algo que você pode continuar fazendo. Seletores curtos e não aninhados, 
não usando o seletor universal (`*{}`) como seletor chave, e evitando seletores
de CSS3 mais complexos deve ajudar a evitar esses problemas.

## Identação de seletores em CSS

Ao invés de usar seletores para perfurar o DOM até um elemento, é melhor
colocar uma classe em um elemento que você quer estilizar explicitamente. Vamos ver
um exemplo específico com seletores como `.header ul{}`…

Vamos imaginar que o `ul` é o navegador principal do nosso site. Ele live
no header como você deve imaginar e é o único `ul` nele;
`.header ul{}` vai funcionar, mas nao é idealmente aconselhavel. Não é a prova de 
erros futuros e certamente não é esplicito o suficiente. Assim que adicionarmos outra `ul` para 
esse header ele vai adotar os estilos de nosso nav principal e as chances são que não queremos isso.
Isso significa que nós vamos ter que refatorizar muitos códigos _ou_ desfazer muitos estilos 
de `ul` nesse `.header` para remover os efeitos desse seletor.

A intenção de seus seletores devem bater com as razões para estilizar alguma coisa;
pergunte-se a sí mesmo **‘Eu estou selecionando isso porque isso é uma `ul` dentro de 
um `.header` ou porque esse é meu nav principal do meu site?‘**. A resposta para isso vai determinar
seu seletor.

Tenha certeza que a chave do seu seletor nunca é um seletor elemento/tipo ou 
uma classe objeto/abstração. Você nunca quer ver seletores como `.sidebar ul{}` ou
`.footer .media{}` nas folhas de estilo do seu tema.

Seja explicito; localize o elemento que você quer afetar, não o seu pai. Nunca assuma 
que a marcação nunca irá mudar. **Escreva seletores que mirem no que você quer, não no que 
já está lá.**

Para uma escrita completa, por favor veja meu artigo [Shoot to kill; CSS selector intent](http://csswizardry.com/2012/07/shoot-to-kill-css-selector-intent/)


## `!important`

Está tudo bem em usar `!important` em classes de helpers somente. Adicionar `!important`
preventivamente não tem problema, ex. `.error{color: red!important}`, como você deve saber você vai
**sempre** quere que essa regra prevaleça.

Não é um conselho usar `!important` reativamente, ex. para se livrar de situações desagradáveis de 
especificidades. Retrabalhe seu CSS e tente combater esses problemas refatorizando
seus seletores. Mantendo seus seletores curtos e evitando IDs vai te ajudar massivamente.

## Números mágicos e absolutes

Um número mágico é um número o qual é usado porque `ele simplesmente funciona`. Eles são ruins
porque eles raramente funcionam para qualquer razão real, e geralmente não muito a 
prova do futuro ou flexiveis. Eles tendem a consertar sintomas e não problemas.

Por exemplo, usando `.dropdown-nav li:hover ul{{top:37px;}` para mover um dropdown para o fundo
de uma nav em um hover é ruim, já que 37px é um número específico. 37px funciona somente aqui
porque nesse cenário particular o `.dropdown-nav` é 37px de altura.

Ao invés você deveria usar `.dropdown-nav li:hover ul{ top: 100%; }` o que significa que não
importa o quao alto o `.dropdown-nav` fique, o dropdown vai sempre ficar 100% do topo.

Toda vez que você colocar um número pense duas vezes; se você pode evitar isso usando
palavras chaves como `alias` (ex. `top: 100%` para dizer `totalmente afastado desse topo`)
ou&mdash;até melhor&mdash;sem medições, faça isso.

Todo código de medição que você colocar é um compromisso que você talvez não queira manter.

## Folhas de estilo condicionais

Folhas de estilo para IE podem, em geral, ser totalmente evitadas. A única vez que uma folha de estilo
para IE pode ser usada é para algum tipo de suporte (ex. consertar arquivos .PNG)

Como uma regra geral, todo layout e regra de box-model pode e _vai_ funcionar sem uma
folha de estilo para IE se você refatorizar e retrabalhar seu CSS. Isso significa que você nunca
vai querer ver `<!--[if IE 7]> element{ margin-left:-9px; } < ![endif]-->` ou algum outro tipo
de CSS que esta sendo usado arbitrariamente para estilizar e `fazer a coisa funcionar`.

## Debugging

Se você tiver algum problema de css **tire algum código, antes de começar a adicoinar mais** para
consertar isso. O problema existe no CSS que já está escrito, masi CSS não
é a maneira certa!

Delete algumas marcações e CSS até que o problema acabe, depois você 
pode determinar em qual porte do código o problema se encontra.

Pode ser tentador colocar um `overflow:hidden;` em alguma coisa para esconder os efeitos
de um problema de layout, mas overflow provavelmente nunca foi o problema; **arrume o problema, 
não os sintomas.**

## Pré-processadores

Sass é o pré-processador que eu escolhi. **Use-o sabiamente.** Use Sass para fazer seu CSS
mais podreozo, mas evite aninhar como se fosse uma praga! Aninhe somente quando
necessário em CSS vanilla, ex.

    .header{}
    .header .site-nav{}
    .header .site-nav li{}
    .header .site-nav li a{}

Seria totalmente desnecessário em um CSS normal, portanto o seguinte seria **ruim**
Sass:

    .header{
        .site-nav{
            li{
                a{}
            }
        }
    }

Se você usasse o Sass ele escreveria da seguinte maneira:

    .header{}
    .site-nav{
        li{}
        a{}
    }
