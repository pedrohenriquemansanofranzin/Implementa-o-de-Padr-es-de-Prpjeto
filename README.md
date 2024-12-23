# Implementa-o-de-Padrões-de-Projeto
  Neste trabalho foi escolhido 2 padrões de projeto;. Um criacional, estrutural, comportamental, será explicado ao menos 1 exemplo de cada neste texto, com exemplos de código e adrões UML de cada um.
  Começando com um modelo criacional, um modelo criacional busca fornecer vários mecanismos de cração de objetos, que aumentam a flexibilidade e reutilização do código já existente, um método criaconal é o: Factory Method:
    Esse método foi criado com o próposito de criar objetos em uma superclasse, permitindo que as sub-classes alterem esses objetos, esse método busca tentar resolver problemas de logística, por exemplo: Imagine que você está criando uma aplicação de gerenciamento de logística. A primeira versão da sua aplicação pode lidar apenas com o transporte de caminhões, portanto a maior parte do seu código fica dentro da classe Caminhão.

Depois de um tempo, sua aplicação se torna bastante popular. Todos os dias você recebe dezenas de solicitações de empresas de transporte marítimo para incorporar a logística marítima na aplicação.

![image](https://github.com/user-attachments/assets/e30a063f-d9cb-4138-87c0-9669da36400a)

Boa notícia, certo? Mas e o código? Atualmente, a maior parte do seu código é acoplada à classe Caminhão. Adicionar Navio à aplicação exigiria alterações em toda a base de código. Além disso, se mais tarde você decidir adicionar outro tipo de transporte à aplicação, provavelmente precisará fazer todas essas alterações novamente.

Como resultado, você terá um código bastante sujo, repleto de condicionais que alteram o comportamento da aplicação, dependendo da classe de objetos de transporte.


A solução disso pode ser:
  O padrão Factory Method sugere que você substitua chamadas diretas de construção de objetos (usando o operador new) por chamadas para um método fábrica especial. Não se preocupe: os objetos ainda são criados através do operador new, mas esse está sendo chamado de dentro do método fábrica. Objetos retornados por um método fábrica geralmente são chamados de produtos.

  ![image](https://github.com/user-attachments/assets/18aa63a0-bb9c-461d-a8d2-519753e8d945)

  À primeira vista, essa mudança pode parecer sem sentido: apenas mudamos a chamada do construtor de uma parte do programa para outra. No entanto, considere o seguinte: agora você pode sobrescrever o método fábrica em uma subclasse e alterar a classe de produtos que estão sendo criados pelo método.

  Porém, há uma pequena limitação: as subclasses só podem retornar tipos diferentes de produtos se esses produtos tiverem uma classe ou interface base em comum. Além disso, o método fábrica na classe base deve ter seu tipo de retorno declarado como essa interface.

![image](https://github.com/user-attachments/assets/d268ae0d-2035-4792-b573-a4bbb4ef87b4)

  Por exemplo, ambas as classes Caminhão e Navio devem implementar a interface Transporte, que declara um método chamado entregar. Cada classe implementa esse método de maneira diferente: caminhões entregam carga por terra, navios entregam carga por mar. O método fábrica na classe LogísticaViária retorna objetos de caminhão, enquanto o método fábrica na classe LogísticaMarítima retorna navios.

![image](https://github.com/user-attachments/assets/ba66e2e0-330d-4f2c-ae13-caa96ed4c4c5)

ESTRUTURA:
  A classe Criador declara o método fábrica que retorna novos objetos produto. É importante que o tipo de retorno desse método corresponda à interface do produto.

Você pode declarar o método fábrica como abstrato para forçar todas as subclasses a implementar suas próprias versões do método. Como alternativa, o método fábrica base pode retornar algum tipo de produto padrão.

Observe que, apesar do nome, a criação de produtos não é a principal responsabilidade do criador. Normalmente, a classe criadora já possui alguma lógica de negócio relacionada aos produtos. O método fábrica ajuda a dissociar essa lógica das classes concretas de produtos. Aqui está uma analogia: uma grande empresa de desenvolvimento de software pode ter um departamento de treinamento para programadores. No entanto, a principal função da empresa como um todo ainda é escrever código, não produzir programadores.
  Criadores Concretos sobrescrevem o método fábrica base para retornar um tipo diferente de produto.

  Observe que o método fábrica não precisa criar novas instâncias o tempo todo. Ele também pode retornar objetos existentes de um cache, um conjunto de objetos, ou outra fonte.
  ![image](https://github.com/user-attachments/assets/dbc0d1be-7143-4c60-82c9-51fffe5d61d1)
  #PSEUDOCÓDIGO:
    ![image](https://github.com/user-attachments/assets/986785e2-588d-4399-924e-d24190848d25)

    // A classe criadora declara o método fábrica que deve retornar
// um objeto de uma classe produto. As subclasses da criadora
// geralmente fornecem a implementação desse método.
class Dialog is
    // A criadora também pode fornecer alguma implementação
    // padrão do Factory Method.
    abstract method createButton():Button

    // Observe que, apesar do seu nome, a principal
    // responsabilidade da criadora não é criar produtos. Ela
    // geralmente contém alguma lógica de negócio central que
    // depende dos objetos produto retornados pelo método
    // fábrica. As subclasses pode mudar indiretamente essa
    // lógica de negócio ao sobrescreverem o método fábrica e
    // retornarem um tipo diferente de produto dele.
    method render() is
        // Chame o método fábrica para criar um objeto produto.
        Button okButton = createButton()
        // Agora use o produto.
        okButton.onClick(closeDialog)
        okButton.render()


// Criadores concretos sobrescrevem o método fábrica para mudar
// o tipo de produto resultante.
class WindowsDialog extends Dialog is
    method createButton():Button is
        return new WindowsButton()

class WebDialog extends Dialog is
    method createButton():Button is
        return new HTMLButton()


// A interface do produto declara as operações que todos os
// produtos concretos devem implementar.
interface Button is
    method render()
    method onClick(f)

// Produtos concretos fornecem várias implementações da
// interface do produto.
class WindowsButton implements Button is
    method render(a, b) is
        // Renderiza um botão no estilo Windows.
    method onClick(f) is
        // Vincula um evento de clique do SO nativo.

class HTMLButton implements Button is
    method render(a, b) is
        // Retorna uma representação HTML de um botão.
    method onClick(f) is
        // Vincula um evento de clique no navegador web.


class Application is
    field dialog: Dialog

    // A aplicação seleciona um tipo de criador dependendo da
    // configuração atual ou definições de ambiente.
    method initialize() is
        config = readApplicationConfigFile()

        if (config.OS == "Windows") then
            dialog = new WindowsDialog()
        else if (config.OS == "Web") then
            dialog = new WebDialog()
        else
            throw new Exception("Error! Unknown operating system.")

    // O código cliente trabalha com uma instância de um criador
    // concreto, ainda que com sua interface base. Desde que o
    // cliente continue trabalhando com a criadora através da
    // interface base, você pode passar qualquer subclasse da
    // criadora.
    method main() is
        this.initialize()
        dialog.render()
APLIVABILIDADE: 
  Use o Factory Method quando não souber de antemão os tipos e dependências exatas dos objetos com os quais seu código deve funcionar.

 O Factory Method separa o código de construção do produto do código que realmente usa o produto. Portanto, é mais fácil estender o código de construção do produto independentemente do restante do código.

Por exemplo, para adicionar um novo tipo de produto à aplicação, só será necessário criar uma nova subclasse criadora e substituir o método fábrica nela.

 Use o Factory Method quando desejar fornecer aos usuários da sua biblioteca ou framework uma maneira de estender seus componentes internos.

 Herança é provavelmente a maneira mais fácil de estender o comportamento padrão de uma biblioteca ou framework. Mas como o framework reconheceria que sua subclasse deve ser usada em vez de um componente padrão?

A solução é reduzir o código que constrói componentes no framework em um único método fábrica e permitir que qualquer pessoa sobrescreva esse método, além de estender o próprio componente.

Vamos ver como isso funcionaria. Imagine que você escreva uma aplicação usando um framework de UI de código aberto. Sua aplicação deve ter botões redondos, mas o framework fornece apenas botões quadrados. Você estende a classe padrão Botão com uma gloriosa subclasse BotãoRedondo. Mas agora você precisa informar à classe principal UIFramework para usar a nova subclasse no lugar do botão padrão.

Para conseguir isso, você cria uma subclasse UIComBotõesRedondos a partir de uma classe base do framework e sobrescreve seu método criarBotão. Enquanto este método retorna objetos Botão na classe base, você faz sua subclasse retornar objetos BotãoRedondo. Agora use a classe UIComBotõesRedondos no lugar de UIFramework. E é isso!

 Use o Factory Method quando deseja economizar recursos do sistema reutilizando objetos existentes em vez de recriá-los sempre.

 Você irá enfrentar essa necessidade ao lidar com objetos grandes e pesados, como conexões com bancos de dados, sistemas de arquivos e recursos de rede.

Vamos pensar no que deve ser feito para reutilizar um objeto existente:

Primeiro, você precisa criar algum armazenamento para manter o controle de todos os objetos criados.
Quando alguém solicita um objeto, o programa deve procurar um objeto livre dentro desse conjunto.
...e retorná-lo ao código cliente.
Se não houver objetos livres, o programa deve criar um novo (e adicioná-lo ao conjunto de objetos).
Isso é muito código! E tudo deve ser colocado em um único local para que você não polua o programa com código duplicado.

Provavelmente, o lugar mais óbvio e conveniente onde esse código deve ficar é no construtor da classe cujos objetos estamos tentando reutilizar. No entanto, um construtor deve sempre retornar novos objetos por definição. Não pode retornar instâncias existentes.

Portanto, você precisa ter um método regular capaz de criar novos objetos e reutilizar os existentes. Isso parece muito com um método fábrica.


MÉTODO ESTRUTURAL: Estes padrões explicam como montar objetos e classes em estruturas maiores mas ainda mantendo essas estruturas flexíveis e eficientes.
MÉTODO ADAPTER: 
  O Adapter é um padrão de projeto estrutural que permite objetos com interfaces incompatíveis colaborarem entre si.
  PROBLEMA: Imagine que você está criando uma aplicação de monitoramento do mercado de ações da bolsa. A aplicação baixa os dados as ações de múltiplas fontes em formato XML e então mostra gráficos e diagramas maneiros para o usuário.

Em algum ponto, você decide melhorar a aplicação ao integrar uma biblioteca de análise de terceiros. Mas aqui está a pegadinha: a biblioteca só trabalha com dados em formato JSON.

![image](https://github.com/user-attachments/assets/53f1152f-58d1-4180-bab6-5d56150f0485)
Você poderia mudar a biblioteca para que ela funcione com XML. Contudo, isso pode quebrar algum código existente que depende da biblioteca. E pior, você pode não ter acesso ao código fonte da biblioteca para começo de conversa, fazendo dessa abordagem uma tarefa impossível.
SOLUÇÃO:
  Você pode criar um adaptador. Ele é um objeto especial que converte a interface de um objeto para que outro objeto possa entendê-lo.

Um adaptador encobre um dos objetos para esconder a complexidade da conversão acontecendo nos bastidores. O objeto encobrido nem fica ciente do adaptador. Por exemplo, você pode encobrir um objeto que opera em metros e quilômetros com um adaptador que converte todos os dados para unidades imperiais tais como pés e milhas.

Adaptadores podem não só converter dados em vários formatos, mas também podem ajudar objetos com diferentes interfaces a colaborar. Veja aqui como funciona:

O adaptador obtém uma interface, compatível com um dos objetos existentes.
Usando essa interface, o objeto existente pode chamar os métodos do adaptador com segurança.
Ao receber a chamada, o adaptador passa o pedido para o segundo objeto, mas em um formato e ordem que o segundo objeto espera.
Algumas vezes é possível criar um adaptador de duas vias que pode converter as chamadas em ambas as direções.

Vamos voltar à nossa aplicação da bolsa de valores. Para resolver o dilema dos formatos incompatíveis, você pode criar adaptadores XML-para-JSON para cada classe da biblioteca de análise que seu código trabalha diretamente. Então você ajusta seu código para comunicar-se com a biblioteca através desses adaptadores. Quando um adaptador recebe uma chamada, ele traduz os dados entrantes XML em uma estrutura JSON e passa a chamada para os métodos apropriados de um objeto de análise encoberto.

  ESTRUTURA: Essa implementação usa o princípio de composição do objeto: o adaptador implementa a interface de um objeto e encobre o outro. Ele pode ser implementado em todas as linguagens de programação populares.

![image](https://github.com/user-attachments/assets/44431779-cd7b-4d18-ba34-e1fe75f0449f)

#PSEUDOCÓDIGO:
  // Digamos que você tenha duas classes com interfaces
// compatíveis: RoundHole (Buraco Redondo) e RoundPeg (Pino
// Redondo).
class RoundHole is
    constructor RoundHole(radius) { ... }

    method getRadius() is
        // Retorna o raio do buraco.

    method fits(peg: RoundPeg) is
        return this.getRadius() >= peg.getRadius()

class RoundPeg is
    constructor RoundPeg(radius) { ... }

    method getRadius() is
        // Retorna o raio do pino.


// Mas tem uma classe incompatível: SquarePeg (Pino Quadrado).
class SquarePeg is
    constructor SquarePeg(width) { ... }

    method getWidth() is
        // Retorna a largura do pino quadrado.


// Uma classe adaptadora permite que você encaixe pinos
// quadrados em buracos redondos. Ela estende a classe RoundPeg
// para permitir que objetos do adaptador ajam como pinos
// redondos.
class SquarePegAdapter extends RoundPeg is
    // Na verdade, o adaptador contém uma instância da classe
    // SquarePeg.
    private field peg: SquarePeg

    constructor SquarePegAdapter(peg: SquarePeg) is
        this.peg = peg

    method getRadius() is
        // O adaptador finge que é um pino redondo com um raio
        // que encaixaria o pino quadrado que o adaptador está
        // envolvendo.
        return peg.getWidth() * Math.sqrt(2) / 2


// Em algum lugar no código cliente.
hole = new RoundHole(5)
rpeg = new RoundPeg(5)
hole.fits(rpeg) // true

small_sqpeg = new SquarePeg(5)
large_sqpeg = new SquarePeg(10)
// Isso não vai compilar (tipos incompatíveis).
hole.fits(small_sqpeg)

small_sqpeg_adapter = new SquarePegAdapter(small_sqpeg)
large_sqpeg_adapter = new SquarePegAdapter(large_sqpeg)
hole.fits(small_sqpeg_adapter) // true
hole.fits(large_sqpeg_adapter) // false

  APLICABILIDADE:
    Utilize a classe Adaptador quando você quer usar uma classe existente, mas sua interface não for compatível com o resto do seu código.

 O padrão Adapter permite que você crie uma classe de meio termo que serve como um tradutor entre seu código e a classe antiga, uma classe de terceiros, ou qualquer outra classe com uma interface estranha.

 Utilize o padrão quando você quer reutilizar diversas subclasses existentes que não possuam alguma funcionalidade comum que não pode ser adicionada a superclasse.

 Você pode estender cada subclasse e colocar a funcionalidade faltante nas novas classes filhas. Contudo, você terá que duplicar o código em todas as novas classes, o que cheira muito mal.

Uma solução muito mais elegante seria colocar a funcionalidade faltante dentro da classe adaptadora. Então você encobriria os objetos com as funcionalidades faltantes dentro do adaptador, ganhando tais funcionalidades de forma dinâmica. Para isso funcionar, as classes alvo devem ter uma interface em comum, e o campo do adaptador deve seguir aquela interface. Essa abordagem se parece muito com o padrão Decorator.

 Como implementar
Certifique-se que você tem ao menos duas classes com interfaces incompatíveis:

Uma classe serviço útil, que você não pode modificar (quase sempre de terceiros, antiga, ou com muitas dependências existentes).
Uma ou mais classes cliente que seriam beneficiadas com o uso da classe serviço.
Declare a interface cliente e descreva como o cliente se comunica com o serviço.

Cria a classe adaptadora e faça-a seguir a interface cliente. Deixe todos os métodos vazios por enquanto.

Adicione um campo para a classe do adaptador armazenar uma referência ao objeto do serviço. A prática comum é inicializar esse campo via o construtor, mas algumas vezes é mais conveniente passá-lo para o adaptador ao chamar seus métodos.

Um por um, implemente todos os métodos da interface cliente na classe adaptadora. O adaptador deve delegar a maioria do trabalho real para o objeto serviço, lidando apenas com a conversão da interface ou formato dos dados.

Os Clientes devem usar o adaptador através da interface cliente. Isso irá permitir que você mude ou estenda o adaptador sem afetar o código cliente.



  PADRÕES COMPORTAMENTAIS: Estes padrões são voltados aos algoritmos e a designação de responsabilidades entre objetos.
  PADRÃO CHAIN OF RESPONSIBILITY:
    PROPÓSITO: O Chain of Responsibility é um padrão de projeto comportamental que permite que você passe pedidos por uma corrente de handlers. Ao receber um pedido, cada handler decide se processa o pedido ou o passa adiante para o próximo handler na corrente.
    PROBLEMA: Imagine que você está trabalhando em um sistema de encomendas online. Você quer restringir o acesso ao sistema para que apenas usuários autenticados possam criar pedidos. E também somente usuários que tem permissões administrativas devem ter acesso total a todos os pedidos.

Após um pouco de planejamento, você se dá conta que essas checagens devem ser feitas sequencialmente. A aplicação pode tentar autenticar um usuário ao sistema sempre que receber um pedido que contém as credenciais do usuário. Contudo, se essas credenciais não estão corretas e a autenticação falha, não há razão para continuar com outras checagens.
![image](https://github.com/user-attachments/assets/5441c134-a94b-4980-abbd-6e8de9283fa4)
Durante os próximos meses você implementou diversas mais daquelas checagens sequenciais.

Um de seus colegas sugeriu que não é seguro passar dados brutos diretamente para o sistema de encomendas. Então você adicionou uma etapa adicional de validação para limpar os dados no pedido.

Mais tarde, alguém notou que o sistema é vulnerável à ataques de força bruta. Para evitar isso, você prontamente adicionou uma checagem que filtra repetidas falhas vindas do mesmo endereço de IP.

Outra pessoa sugeriu que você poderia agilizar o sistema se retornasse resultados de cache em pedidos repetidos contendo os mesmos dados. Portanto, você adicionou outra checagem que permite que o pedido passe através do sistema apenas se não há uma resposta adequada armazenada em cache.

![image](https://github.com/user-attachments/assets/8b7bb884-a330-41f8-8274-24d2a7948800)
O código das checagens, que já parecia uma bagunça, ficou mais e mais inchado a medida que você foi adicionando novas funcionalidades. Mudar uma checagem às vezes afetava outras. E o pior de tudo, quando você tentou reutilizar as checagens para proteger os componentes do sistema, você teve que duplicar parte do código uma vez que esses componentes precisavam de algumas dessas checagens, mas nem todos eles.

O sistema ficou muito difícil de compreender e caro de se manter. Você lutou com o código por um tempo, até que um dia você decidiu refatorar a coisa toda.

SOLUÇÃO: Como muitos outros padrões de projeto comportamental, o Chain of Responsibility se baseia em transformar certos comportamentos em objetos solitários chamados handlers. No nosso caso, cada checagem devem ser extraída para sua própria classe com um único método que faz a checagem. O pedido, junto com seus dados, é passado para esse método como um argumento.

O padrão sugere que você ligue esses handlers em uma corrente. Cada handler ligado tem um campo para armazenar uma referência ao próximo handler da corrente. Além de processar o pedido, handlers o passam adiante na corrente. O pedido viaja através da corrente até que todos os handlers tiveram uma chance de processá-lo.

E aqui está a melhor parte: um handler pode decidir não passar o pedido adiante na corrente e efetivamente parar qualquer futuro processamento.

Em nosso exemplo com sistema de encomendas, um handler realiza o processamento e então decide se passa o pedido adiante na corrente ou não. Assumindo que o pedido contenha os dados adequados, todos os handlers podem executar seu comportamento principal, seja ele uma checagem de autenticação ou armazenamento em cache.
Contudo, há uma abordagem ligeiramente diferente (e um tanto quanto canônica) na qual, ao receber o pedido, um handler decide se ele pode processá-lo ou não. Se ele pode, ele não passa o pedido adiante. Então é um handler que processa o pedido ou mais ninguém. Essa abordagem é muito comum quando lidando com eventos em pilha de elementos dentro de uma interface gráfica de usuário.

Por exemplo, quando um usuário clica um botão, o evento se propaga através da corrente de elementos GUI que começam com aquele botão, prossegue para seus contêineres (como planilhas ou painéis), e termina com a janela principal da aplicação. O evento é processado pelo primeiro elemento na corrente que é capaz de lidar com ele. Esse exemplo também é notável porque ele mostra que uma corrente pode sempre ser extraída de um objeto árvore.
![image](https://github.com/user-attachments/assets/9564a191-6b4f-4f0a-98de-631ce9e99b39)
É crucial que todas as classes handler implementem a mesma interface. Cada handler concreto deve se importar apenas se o seguinte tem o método executar. Dessa maneira você pode compor correntes durante a execução, usando vários handlers sem acoplar seu código com suas classes concretas.


ESTRUTURA: ![image](https://github.com/user-attachments/assets/5e49ce9e-44bd-4f0c-a6c1-f652b71da5eb)

#PSEUDOCÓDIGO: 
// A interface do handler declara um método para executar um
// pedido.
interface ComponentWithContextualHelp is
    method showHelp()


// A classe base para componentes simples.
abstract class Component implements ComponentWithContextualHelp is
    field tooltipText: string

    // O contêiner do componente age como o próximo elo na
    // corrente de handlers.
    protected field container: Container

    // O componente mostra um tooltip (dica de contexto) se há
    // algum texto de ajuda assinalado a ele. Do contrário ele
    // passa a chamada adiante ao contêiner, se ele existir.
    method showHelp() is
        if (tooltipText != null)
            // Mostrar dica de contexto.
        else
            container.showHelp()


// Contêineres podem conter tanto componentes simples como
// outros contêineres como filhos. As relações da corrente são
// definidas aqui. A classe herda o comportamento showHelp de
// sua mãe.
abstract class Container extends Component is
    protected field children: array of Component

    method add(child) is
        children.add(child)
        child.container = this


// Componentes primitivos estão de bom tamanho com a
// implementação de ajuda padrão.
class Button extends Component is
    // ...

// Mas componentes complexos podem sobrescrever a implementação
// padrão. Se o texto de ajuda não pode ser fornecido de uma
// nova maneira, o componente pode sempre chamar a implementação
// base (veja a classe Component).
class Panel extends Container is
    field modalHelpText: string

    method showHelp() is
        if (modalHelpText != null)
            // Mostra uma janela modal com texto de ajuda.
        else
            super.showHelp()

// ...o mesmo que acima...
class Dialog extends Container is
    field wikiPageURL: string

    method showHelp() is
        if (wikiPageURL != null)
            // Abre a página de ajuda do wiki.
        else
            super.showHelp()


// Código cliente.
class Application is
    // Cada aplicação configura a corrente de forma diferente.
    method createUI() is
        dialog = new Dialog("Budget Reports")
        dialog.wikiPageURL = "http://..."
        panel = new Panel(0, 0, 400, 800)
        panel.modalHelpText = "This panel does..."
        ok = new Button(250, 760, 50, 20, "OK")
        ok.tooltipText = "This is an OK button that..."
        cancel = new Button(320, 760, 50, 20, "Cancel")
        // ...
        panel.add(ok)
        panel.add(cancel)
        dialog.add(panel)

    // Imagine o que acontece aqui.
    method onF1KeyPress() is
        component = this.getComponentAtMouseCoords()
        component.showHelp()

APLICABILIDADE: 
Utilize o padrão Chain of Responsibility quando é esperado que seu programa processe diferentes tipos de pedidos em várias maneiras, mas os exatos tipos de pedidos e suas sequências são desconhecidos de antemão.

 O padrão permite que você ligue vários handlers em uma corrente e, ao receber um pedido, perguntar para cada handler se ele pode ou não processá-lo. Dessa forma todos os handlers tem a chance de processar o pedido.

 Utilize o padrão quando é essencial executar diversos handlers em uma ordem específica.

 Já que você pode ligar os handlers em uma corrente em qualquer ordem, todos os pedidos irão atravessar a corrente exatamente como você planejou.

 Utilize o padrão CoR quando o conjunto de handlers e suas encomendas devem mudar no momento de execução.

 Se você providenciar setters para um campo de referência dentro das classes handler, você será capaz de inserir, remover, ou reordenar os handlers de forma dinâmica.

 Como implementar
Declare a interface do handler e descreva a assinatura de um método para lidar com pedidos.

Decida como o cliente irá passar os dados do pedido para o método. A maneira mais flexível é converter o pedido em um objeto e passá-lo para o método handler como um argumento.

Para eliminar código padrão duplicado nos handlers concretos, pode valer a pena criar uma classe handler base abstrata, derivada da interface do handler.

Essa classe deve ter um campo para armazenar uma referência ao próximo handler na corrente. Considere tornar a classe imutável. Contudo, se você planeja modificar correntes no tempo de execução, você precisa definir um setter para alterar o valor do campo de referência.

Você também pode implementar o comportamento padrão conveniente para o método handler, que vai passar adiante o pedido para o próximo objeto a não ser que não haja mais objetos. Handlers concretos irão ser capazes de usar esse comportamento ao chamar o método pai.

Um por um crie subclasses handler concretas e implemente seus métodos handler. Cada handler deve fazer duas decisões ao receber um pedido:

Se ele vai processar o pedido.
Se ele vai passar o pedido adiante na corrente.
O cliente pode tanto montar correntes sozinho ou receber correntes pré construídas de outros objetos. Neste último caso, você deve implementar algumas classes fábrica para construir correntes de acordo com a configuração ou definições de ambiente.

O cliente pode ativar qualquer handler da corrente, não apenas o primeiro. O pedido será passado ao longo da corrente até que algum handler se recuse a passá-lo adiante ou até ele chegar ao fim da corrente.

Devido a natureza dinâmica da corrente, o cliente deve estar pronto para lidar com os seguintes cenários:

A corrente pode consistir de um único elo.
Alguns pedidos podem não chegar ao fim da corrente.
Outros podem chegar ao fim da corrente sem terem sido tratados.

  



