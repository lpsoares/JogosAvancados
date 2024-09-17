# Blockmesh

## Motivação

Você acabou de receber as diretrizes de criação de um novo nível do jogo que sua empresa está desenvolvendo. Com isso em mente, você abre sua pasta de assets e começa a criar o mapa nos mínimos detalhes. Você utiliza tudo o que acha que deve conter nele, caixas, móveis, árvores, decoração, entre outros. Passa um bom tempo escolhendo os shaders e texturas, e tem certeza que seu trabalho atende a todos os objetivos do documento que lhe foi entregue.

Ao final de vários dias de trabalho, é feita uma revisão e você descobre que não é divertido se movimentar pelo mapa. Algumas áreas ficaram inacessíveis devido às colisões mal ajustadas, e o fluxo do nível não está funcionando como esperado.

Esse é um problema comum quando se começa a criação de um nível diretamente com detalhes finais, sem um planejamento adequado das mecânicas e do layout básico. Como evitar isso?


## Conceito

Blockmesh, Greybox, Whitebox ou Level Blockout. Como muitas coisas no desenvolvimento de jogos, temos nomes diferentes para nos referirmos ao mesmo conceito. Essa técnica consiste em criar um nível de jogo utilizando formas simples, que frequentemente chamamos de malhas primitivas. Cubos, cápsulas e cilindros são os mais utilizados, mas podemos incluir elementos mais avançados, como escadas e portas, dependendo da engine escolhida.

O uso de formas, cores e texturas simples é essencial nesse estágio, pois nosso foco principal é entender as proporções e distâncias no jogo. Queremos visualizar a composição geral do mapa, testar a jogabilidade básica e garantir que o fluxo do nível esteja adequado antes de adicionar detalhes. Esse processo permite identificar e corrigir problemas de design de forma eficiente, garantindo que o mapa seja divertido e funcional antes de investir tempo na criação dos assets finais.

Ainda durante o processo de blocagem, é possível identificar assets que serão repetidos mais vezes, permitindo ter uma visão melhor do escopo do projeto e onde ele pode ser expansível ou redutível antes de começar a produzir a arte final.

Entenda o `blockmesh` como um croqui do seu nível final, que a cada iteração irá melhorar gradualmente até, por fim, inserir as artes finais. Lembre-se que, nesse primeiro momento, não devemos nos apegar a detalhes de texturas e iluminação. Essas são fases que virão depois e ajudarão muito na composição do nível. Começar com uma base forte é o melhor caminho.

E já que estamos falando de uma base forte, é bom abordarmos um pouco sobre composição 3D. Assim como na composição clássica, podemos pensar em diferentes planos:

- **Primeiro Plano:** O plano que está mais próximo ao observador, serve para enquadrar o foco do jogador. Ele é usado para focar o jogador nos pontos de interesse ou nos pontos mais importantes, além de evitar que o que está mais próximo pareça vazio.

- **Centro de Interesse:** A camada ou as camadas onde a parte predominante da composição se localiza. O ponto focal da composição não deve se confundir com o resto da cena, mas deve se destacar dos outros elementos. Planeje sua composição de forma que o ponto de interesse seja o primeiro que o observador (ou jogador) veja, construindo um objeto de identificação da cena, por exemplo: um castelo nas montanhas com uma torre em chamas. As cores do castelo devem se destacar das cores da montanha e ele deve ser feito para ser visto antes das montanhas.

- **Plano de Fundo:** A camada que fecha a composição. A maioria dos planos de fundo são cenas do horizonte e do céu. O plano de fundo geralmente é feito com um esquema de cores mais claras, sem objetos muito detalhados, para ajudar o observador a focar no que é dominante.

Com isso em mente, já podemos iniciar a construção de nosso primeiro nível. 

## Aplicação

Toda parte de composição do seu jogo é dependente de diversos pontos a serem definidos por um guia de design, uma das opções mais básicas é: Qual o modo de câmera principal que irei utilizar nesse jogo?

Será utilizado o modelo de terceira pessoa para a sequência a seguir, mas fique a vontade para escolher o que fizer mais sentido para seu projeto.


> Recomendamos o uso das templates FPS e TPS pois elas já implementam uma série de algoritmos para a câmera, que de outra forma teriamos de fazer manualmente. 

### Entendendo a ferramenta

![](attachments/Pasted%20image%2020240805111919.png)

Assim que criamos o projeto baseado em um template, a UE5 cria um **Level** padrão. Se quiser pode testar se está tudo correto com o projeto clicando no *Play This Level* no topo do editor. 

![](attachments/Pasted%20image%2020240805112239.png)

Mas para a atividade crie um **Level** novo. Para isso abra seu *Content Drawer*, clique em uma área livre dele e selecione `Create Basic Asset > Level`, ou ainda `File > New Level`

![](attachments/Pasted%20image%2020240805112540.png)

Nomeie-o como melhor lhe agradar, e dê um duplo clique para abrir esse novo cenário. Caso a engine lhe peça para salvar-lo, basta clicar em `Save Selected`

![](attachments/Pasted%20image%2020240805112742.png)

Neste novo nível, temos apenas o nó raiz, visivel no outliner, assim não é possível visualizar nada. Vamos assim primeiro construir a base de iluminação para nosso nível. Precisamos colocar alguns _**Actors**_, para abrir a janela de `Place Actors` vá em `Window > Place Actors` ou se preferir utilize o menu de `Quickly add to the project`

![](attachments/Pasted%20image%2020240805113715.png)


![](attachments/Pasted%20image%2020240805113646.png)


- Light - **Directional Light**: Luz direcional simulando o sol, ilumina toda a cena de maneira uniforme e paralela.
- Visual Effects - **Sky Atmosphere**: Sistema que simula a dispersão atmosférica da luz.
- Visual Effects - **Exponential Height Fog**: Névoa que se acumula exponencialmente com a altura, criando profundidade atmosférica na cena.

Esses elementos não são obrigatórios, mas com eles conseguimos ter uma experiência mais conhecida ao navegar pelo editor. Na imagem abaixo conseguimos visualiza a diferença sem e com esses **actors** inseridos na cena. 

![](attachments/Pasted%20image%2020240805114150.png)

Vamos começar a popular a cena com alguns elementos. Selecionando Shapes temos algumas opções de malhas "primitivas"

![](attachments/Pasted%20image%2020240805114422.png)

Basta arrasta-los para a cena, e então, quando estiver selecionados teremos acesso aos seus detalhes. 

![](attachments/Pasted%20image%2020240805114601.png)

Para montar o nível com essas formas podemos alterar sua escala no componente transform

![](attachments/Pasted%20image%2020240805115539.png)

Uma outra opção para montar criar o nível é utilizando os *Brushes* de Geometria da Unreal. 

![](attachments/Pasted%20image%2020240805115706.png)

Diferente das shapes, que são *Static Meshes*, esses brushes de geometria são elementos procedurais, que podemos modificar diretamente os parâmetros que os definem: 

![](attachments/Pasted%20image%2020240805120017.png)

Para além disso temos mais formas possíveis, cubos, cones, cilindro, escadas diversas e outros.

Abaixo os parâmetros de outro tipo de geometria: 

![](attachments/Pasted%20image%2020240805120356.png)

![](attachments/Pasted%20image%2020240805120800.png)

Perceba que um dos parâmetros na aba de detalhes é o Brush Type, que por padrão é aditivo, mas podemos alterar para subtrativo, para fazer um rasgo em outro brush: 

![](attachments/Pasted%20image%2020240805121127.png)

Por fim, a UE tem um modo dedicado a edição desse brushes para rápida prototipação de níveis, logo abaixo no nome da aba, temos um *dropdown* que no momento está como *__Selection Mode__* , existe a opção **_Brush Editing_** que então podemos modificar faces, bordas e vértices diretamente. 

![](attachments/Pasted%20image%2020240805121828.png)

Podemos também utilizar a função de extrusão de elementos para criar objetos mais complexos: 

![](attachments/Pasted%20image%2020240805122143.png)

> Quando utilizamos a escala do transform de um objeto, sempre que possível é preferível  manter uma escala uniforme, ou seja, o mesmo valor em todos os campos de escala, pois isso evita uma série de problemas que podem vir a ocorrer, principalmente se esse objeto for modificado dentro de jogo de alguma forma.

### Blocagem de um cenário

Agora que você já conhece a ferramenta, consegue fazer a blocagem de um cenário. 

Para melhor organização vou criar uma pasta no Outliner com nossa luz e efeitos especiais: 

![](attachments/Pasted%20image%2020240805132523.png)

Feito isso o primeiro passo é colocar um ponto de inicio para o jogador com o *Actor* ***Player Start***, e algum objeto para ele ficar em cima: 

![](attachments/Pasted%20image%2020240805132909.png)

![](attachments/Pasted%20image%2020240805134719.png)

Para facilitar, vamos pensar no início de um jogo, vamos colocar um objetivo: 

![](attachments/Pasted%20image%2020240805134922.png)

Por enquanto não parece nada, mas aplicando os princípios de planos, conseguimos criar uma composição que seja interessante. 

Melhorando o ponto de interesse

![](attachments/Pasted%20image%2020240805135635.png)

Criando um primeiro plano

![](attachments/Pasted%20image%2020240805141502.png)

Adicionando elemento secundário no centro de interesse.  

![](attachments/Pasted%20image%2020240805141337.png)

Inserindo elementos no plano de fundo

![](attachments/Pasted%20image%2020240805142520.png)

Aos poucos e em um processo iterativo o nível começa a tomar forma. 


# Missão Blockmesh:

Sua tarefa é criar a blocagem (blockmesh) de um nível de jogo. Esta etapa é crucial para garantir que o nível tenha um bom fluxo e que os principais problemas de design sejam identificados e corrigidos antes de investir tempo nos detalhes finais. 

- Comece com um esboço no papel ou digitalmente. Pense na experiência do jogador e nos desafios que deseja criar.
- Utilize formas primitivas (cubos, cilindros, etc.) para construir o layout básico.
- Inclua elementos essenciais como plataformas, escadas, portas e obstáculos.
- Mantenha cores e texturas simples para focar na jogabilidade e no layout.
- Convide colegas para testar sua blocagem e fornecer feedback

### Entrega: 
Para entrega é necessário enviar uma build executável do seu mapa no Blackboard. 

#### Fazendo a Build: 
Para fazer a build vá em `Edit > Project Settings`, procure por `Maps & Modes` e em `Default Map` selecione o mapa que você criou: 

![](attachments/Pasted%20image%2020240805144828.png)

Feito isso vá ao ícone Platforms e selecione `Windows > Package Project`

![](attachments/Pasted%20image%2020240805145053.png)

Crie uma pasta para a build de seu projeto e selecione. Você deverá enviar essa pasta zipada. 







# Referências

## Artigos

[📑 The Level Design Book: Blockout](https://book.leveldesignbook.com/process/blockout)  
[📑 Level Design Process #01: Cube to Temple](https://www.gamedeveloper.com/design/-level-design-process-01-cube-to-temple)
[📺 GDC 2018: Level Design Workshop: Blockmesh and Lighting Tips](https://www.youtube.com/watch?v=09r1B9cVEQY)  
[📺 GDC 2013: Ten Principles for Good Level Design](https://www.youtube.com/watch?v=iNEe3KhMvXM)  
[📺 GDC 2014: Level Design in a Day: A Series of First Steps - Overcoming the Digital Blank Page](https://www.youtube.com/watch?v=R75g3elj7y4)  
[📺 GMTK: How Level Design Can Tell a Story](https://www.youtube.com/watch?v=RwlnCn2EB9o)  
[📺 Blizzcon 2017 - DesignCraft: Building Blocks of Level Design Panel](https://www.youtube.com/watch?v=K-D2vuKMQhk)  
[📺 Keeping Your Blockouts Simple](https://www.youtube.com/watch?v=syZ0Gd3PZD8)  
[📺 UE5: Cube Grid](https://www.youtube.com/watch?v=-GyDjoyBQ4o&t=16s)  
[📺 UE5: 3 Methods for Blocking Out Environments and Level Designs in UE5](https://www.youtube.com/watch?v=ZfalgxLtNyI)
[📖 Totten, C. W. (2017). Level Design: Processes and Experiences. United Kingdom: CRC Press.](https://www.google.com.br/books/edition/Level_Design/CT-EDgAAQBAJ?hl=en&gbpv=1)  
[📖 Bauer, B. (n.d.). A Practical Guide to Level Design: From Theory to Practice, Diplomacy and Production. United States: CRC Press.](https://www.google.com.br/books/edition/A_Practical_Guide_to_Level_Design/DaSmEAAAQBAJ?hl=en&gbpv=1)  
[📖 Rogers, S. (2010). Level Up! The Guide to Great Video Game Design. United Kingdom: Wiley.](https://www.google.com.br/books/edition/Level_Up/8w_ETFmHrewC?hl=en&gbpv=1)

[#blocktober no Twitter/X](https://x.com/search?q=%23Blocktober&src=typed_query&f=top)  
[Blocktober no Artstation](https://www.artstation.com/search?sort_by=relevance&query=blocktober)