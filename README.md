## Git

### O que é o Git?
Git é um software de versionamento, ou seja, um software que guarda informações sobre o seu projeto num estado específico.

### Por que preciso disso?
Basicamente para controle. Um software de versionamento mantém registros de cada modificação feita no seu projeto, o que significa que, caso algum erro seja cometido no desenvolvimento, é possível retornar para um estado anterior do seu projeto e comparar com versões anteriores, o que auxilia na solução do problema.

### Como ele funciona?
O Git armazena os dados do seu projeto numa base de dados especial, em que cada estado do seu projeto é representado por um _commit_.

Esses estados são chamados de _snapshots_, que são fotografias do seu projeto num momento específico. É dessa forma que o Git consegue recriar o seu projeto caso seja necessário retornar a um estado anterior, o Git simplesmente pega a foto do seu projeto naquele ponto anterior e mostra pra você como era o seu projeto, mostrando arquivos que talvez não existam mais e deixando de mostrar arquivos que foram criados em estados posteriores àquele.

Cada _commit_ ou _snapshot_ é indicada por uma chave, que é como se fosse o nome da foto. Assim, quando é necessário retornar o projeto para aquele momento, basta dizer ao Git esse nome que ele cuida do resto.

__site oficial:__ https://git-scm.com/

### Instalação
#### Onde eu acho o arquivo de instalação?
Para instalar, procure na seção de Downloads ou clique <a href="https://github.com/git-for-windows/git/releases/download/v2.23.0.windows.1/Git-2.23.0-64-bit.exe">aqui</a> para baixar a versão 2.23.0 64 bits para Windows.

#### Processo de instalação
Pode ser um simples "next next next ...", mas tem alguns detalhes interessantes que podem ser alterados para aproveitar melhor a ferramenta.

1. __Default Editor__
É interessante, __mas não obrigatório__, colocar como editor padrão o Git na sua ferramenta de edição de código (seja lá qual for, VSCode, PyCharm, Notepad++, etc.). Facilita o uso dos comandos já que, muito provavelmente, é por lá que os projetos serão feitos e controlados.

![Default Editor](./default_editor.png)

2. __Adição do Git no PATH__
Colocar o Git na variável de ambiente PATH significa que o usuário pode usar os comandos de qualquer terminal (cmd, PowerShell, etc.). Recomendo usar o Git Bash (o terminal que é instalado com o Git) porque é mais parecido com terminal do Linux (os comandos são diferentes do terminal padrão do Windows).

![Git PATH](./git_path.png)

O restante é next, next, next ...

### Comandos Básicos:
#### git init
Cria a estrutura de controle de versão no diretório atual. Também pode ser usado especificando o diretório no formato:

>`git init path`

#### git add \<file1\> \<file2\> ...
Adiciona arquivos na pilha de commit. Costuma-se dizer que esses arquivos estão __staged__.
Para adicionar todos os arquivos do diretório o comando pode ser especificado no formato:

>`git add .`

#### git reset \<file1\> \<file2\> ...
Remove arquivos que foram colocados na pilha de commit. Para remover todos os arquivos, basta escrever o comando:

>`git reset` (sem o ponto)

#### git commit -m "Fix/Add/Remove something something"
Cria o commit no [__branch__](### git branch \<name\>) atual. Um commit é uma foto do estado atual dos arquivos que estavam na pilha (que estavam __staged__). É dessa forma que o Git grava as informações do projeto.

Quando uma mensagem curta não é descritiva o suficiente das alterações é recomendado escrever um pequeno parágrafo sobre o commit no formato:

> git commit -m "Fix/Add/Remove something something
>
> This paragraph describes the changes made  
> more in-depth to the project.

##### Mensagens de commit
É uma boa prática escrever títulos de até 50 caractéres (legibilidade do log) com o formato:
__\<Ação\> \<Objeto\> \<Local\>__

__Ação:__ O que você fez? Correção, Adição, Remoção (evite "modificação", por exemplo, que não transmite logo de cara o conteúdo do commit, prefira um dos três primeiros);

__Objeto:__ Em que você fez? função xyz, mensagem abc (evite longas descrições, deixe elas para o corpo do commit e não para o cabeçalho)

__Local:__ Aonde você fez? Pode até ser omitido dependendo da situação, mas para funções ou códigos mais longos separados por seções pode ser útil apontar aonde está a mudança;


#### git branch \<name\>
Cria um novo branch com nome __\<name\>__.  

Quando um projeto é iniciado, é como marcar um ponto de partida e plantar uma semente de uma árvore. Conforme seu projeto cresce, você __incrementa__ arquivos, __acrescenta novos__ arquivos, pastas, etc. e cada etapa de desenvolvimento que termina é marcada por um __commit__ (um objeto da árvore).  

Assim, um __branch__ (galho) é uma ramificação do projeto, uma forma de __apontar__ (selecionar) um commit específico, sendo que o branch default é o __master__, que não tem nada de especial por ter esse nome, ele é criado com o início do repositório e, na maioria das vezes, ninguém muda o nome dele.  

Cada projeto possui diferentes necessidades de __workflow__, mas é comum adotar a estrutura de branches a seguir:

| __Nome do branch__   | __Descrição__                               |
| ------------------   | ------------------------------------------- |
| `master`             | branch de produção                          |
| `dev` ou `staging`   | branch de desenvolvimento de novas features |
| `topics` ou `issues` | branch de solução de problemas              |

##### Então o que acontece quando eu crio um novo branch?
Você está apenas criando outro apontador de commits para se movimentar pela árvore do seu projeto. Isso é útil para criar diferentes estados do projeto, por exemplo um branch de __staging/desenvolvimento__, outro de __prod__ (que geralmente é o __master__) e, caso necessário, __topic branches__ para solução de bugs do projeto relatados pelos usuários.

##### Como o git sabe para qual branch ele tem que olhar?
Existe outro __apontador__ que é o mestre de todos os branches chamado de __HEAD__, que é quem decide qual branch está comandando o estado atual do projeto que você enxerga.

##### Ok, mas ainda não entendi a utilidade disso tudo:
O melhor cenário para entender isso é pensar num projeto grande e você precisar comandar seu único branch __master__ entre seus commits de __staging__, __prod__ e quaisquer outros. Você teria que procurar o commit toda vez que quisesse mudar para algum estado específico do projeto e então reverter os estados ou fazer um checkout... enfim, é algo trabalhoso. Com os __branches__, você pode usá-los como se fossem marcadores de página ou índices do seu projeto. Além disso, é possível ramificar alterações a partir de qualquer ponto da árvore do projeto para desenvolver novas features sem alterar o estado de produção estável.
Assim, quando necessário, basta trocar o __HEAD__ para o branch desejado (que terá um nome intuitivo ao invés de um hash muito doido) para navegar pela árvore do seu projeto até pontos de interesse.

#### git merge \<branch_name_1\> \<branch_name_2\> ...
Uma das formas de juntar dois ou mais __branches__ (também chamados __histories__) ao __branch__ atual.  
Um exemplo visual que ajuda a entender uma situação de merge é a seguinte:
Digamos que seu __branch__ de produção está no estado __D__ e, paralelamente, você esteve trabalhando na solução de um bug desde a versão __B__:

>
>            E---F---G   iss#1
>           /
>      A---B---C---D     master
>

Então na versão __G__ do bug você resolveu o problema de forma estável e está confiante em associar as mudanças ao __branch__ de produção na versão __D__. Para isso você executa o comando __git merge iss#1__ com o __HEAD__ do projeto no __master__. Assim a árvore do projeto deverá resultar em:

>
>            E---F---G    iss#1
>           /         \
>      A---B---C---D---H  master
>

# Rascunho (ignorar por enquanto)

### Git vs Github:

Não são a mesma coisa.

Github é um site, um serviço que usa Git.

Git é um sistema de versionamento independente do Github. Você pode usar Git e armazenar seu código no google drive, por exemplo. O versionamento é feito pelo Git e o armazenamento é feito pelo drive.

O Github é um serviço que facilita o uso do Git porque acrescenta um front-end ao Git, mostrando número de commits, versões, mudanças em arquivos, entre outros.



### 2. Comandos básicos:

### git status path
Mostra o estado do projeto: qual é o branch, se existem arquivos não rastreados, quais arquivos estão na fila de commit, etc.

### git log
Mostra o histórico de commits o hash do commit, autor, data e mensagem. O hash é o código único do commit, que é o que permite com que o sistema consiga retornar a uma versão anterior do projeto usando o próximo comando.

### git checkout \<commit_hash\>
Retorna o projeto ao estado do commit específico ao hash que foi passado. Caso tenham mudanças que não foram commitadas no projeto, o git lança um erro pedindo que você faça o commit ou que você "empilhe" a sua sujeira, em outras palavras, ele quer que você crie um stash do seu trabalho, que você arrume a mesa com a papelada do projeto atual, guarde numa pasta e pegue seus outros materiais de trabalho que precisam de atenção naquele momento.
Pense no checkout como uma máquina do tempo. É possível voltar a um período no projeto em que alguma pasta ou arquivo não existia e, de fato, quando o checkout é feito esse arquivo não estará mais no repositório, mas ele continua existindo no master branch
A ideia que o Git passa é a de que cada commit representa um corpo diferente e, quando você muda de branch ou para um commit anterior, você desconecta a cabeça do corpo master e liga ela a um corpo anterior para poder movimentar aquele corpo e ter a perspectiva antiga.

### git stash
Cria uma pilha com seu trabalho inacabado. É útil quando você precisa mudar de branch para trabalhar em outra coisa e não quer guardar um commit que não contribui para o projeto, uma vez que o que estava fazendo ainda não foi terminado.
Cada git stash cria uma pilha diferente que pode ser vista com git stash list e, para retomar uma stash usa-se git stash apply stash_id

### git rebase