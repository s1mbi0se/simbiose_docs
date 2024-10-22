# Documentação Simbiose

Aqui você encontrará toda a documentação técnica da Simbiose Ventures. O objetivo deste documento, é fornecer informações que irão padronizar o desenvolvimento.

## Regras atuais

Essas são regras definidas para guiar nossos projetos de uma forma padronizada, afim de manter qualidade e consistência em toda a empresa.

As regras não foram criadas para impôr preferências pessoais de quaisquer membros da equipe.

Como consequência das regras serem usadas para melhorar o trabalho, qualidade e consistência, elas podem ser mudadas, caso uma maneira mais interessante e produtiva apareça, portanto, não são absolutas. Caso tenha sugestões para melhoras de nosso processo, basta entrar em contato e iremos todos discutir em conjunto.

Todos os padrões devem ser seguidos conforme orientado em cada um, para qualquer dúvida, sinta-se a vontade para fazer perguntas no Discord.

## Versionamento e branchs

O modelo que devemos usar para desenvolver nossos projetos e organizar os repositórios é o Git Flow.

O modelo Git Flow é descrito em detalhes [aqui](http://nvie.com/posts/a-successful-git-branching-model/), é necessário ler e entender ele por completo.

Para auxiliar a seguir esse modelo, iremos usar a ferramenta git flow. Você pode ver um guia rápido para instalação e uso dessa ferramenta [aqui](http://danielkummer.github.io/git-flow-cheatsheet/index.pt_BR.html).

Descrição mais detalhada da ferramenta [aqui](https://github.com/nvie/gitflow).

### Padronização dos nomes dos branches

Para manter a organização dos repositórios de forma consistentes entre todos os projetos, devemos aplicar o padrão seguinte (nos baseamos [nesta padronização](https://deepsource.com/blog/git-branch-naming-conventions)):

- Production releases branch: `master`
- Development branch: `develop`
- Feature branches: `feature/123-nome-da-feature`
- Tests branches: `tests/123-nome-do-teste-ou-pacote`
- Fixbugs branches: `fixbug/123-bug-sendo-resolvido`
- Release branches: `release/x.x.x`

Obs.: Note que os números representam os IDs das tarefas sendo desenvolvidas

## Boas práticas de programação

#### Nomes

Para o significado dos nomes, como regra geral, é bom adotar o formato:

- Variáveis e classes são substantivos
- Funções e métodos são verbos

O nome de uma variável deve descrever o que a variável representa de forma clara e não ambígua.

Adote nomes que revelem a intenção da função.

# RUIM

send_http_data()

    # BOM
    post_twitter_status()

SEMPRE coloque nomes descritivos em funções, variáveis, classes, módulos, pacotes, etc.

EVITE abreviações a todo custo.

```{.py3}
# RUIM
x = ['cat', 'dog']
for i in x:
    i.spk()

# BOM
animals = ['cat', 'dog']
for animal in animals:
animal.speak()
```

Respeite o padrão de nomes da linguagem de forma consistente, não misture estilos de programação. Ex: variáveis em python se escrevem com snake_case e em Java com camelCase.

```{.py3}
# RUIM
QtyOfCats = 4
quantt_OF_Dogs = 2

# BOM
quantity_of_cats = 4
quantity_of_dogs = 2
```

#### Espaçamento

SEMPRE use espaço após `,` , `:` e entre operadores como `+` , `-` , `*` , etc.

```{.py3}
# RUIM
some_list = [1,2,3]
some_dict = {'key1':1, 'key2':2}
expression = var1+var2 -var3* var4

# BOM
some_list = [1, 2, 3]
some_dict = {'key1': 1, 'key2': 2}
expression = var1 + var2 - var3 * var4
```

#### Estruturas de controle

A "lógica" inversa dificulta o entendimento grandemente. Dê preferênca a testar pela "verdade" primeiro, e não no else.

```{.py3}
# RUIM
if not ready:
    do_something_else()
else:
    do_something()

# BOM
if ready:
    do_something()
else:
    do_something_else()
```

Evite código muito nested ao fazer validações e retornos no início da lógica. Isso também ajuda a separar o entendimento do que é a parte principal do código e o que são validações, verificações ou coisas que não tem a ver como fluxo principal da função. Isso pode parecer contraditório com o exemplo anterior, mas são situações diferentes.

```{.py3}
# RUIM
def do_something():
    if ready():
        # This is the really important part of the code
        # It's the core of "do_something()"
        # Do a lot of stuff
        # Do a lot of stuff
        # Do a lot of stuff
        return something
    else:
        # This is just some validation
        return not_valid

# BOM
def do_something():
    if not ready:
        # This is just some validation
        return not_valid

    # This is the really important part of the code
    # It's the core of "do_something()"
    # Do a lot of stuff
    # Do a lot of stuff
    # Do a lot of stuff
    return something
```

#### Variáveis globais

Regra geral: Não use, a não ser que seja realmente o único jeito de fazer ou que fazer de outra maneira aumentaria muito a complexidade ou mesmo teria que ser feito através de gambiarra.

Regra especial para Javascript: Nessa linguagem as vezes é mais difícil fugir do uso de variáveis globais. Como maneira de limitar o dano causado por isso, utilize uma única variável global chamada GLOBALS, que deve ser um objeto, e coloque nela quais outras variáveis que você precisaria usar globalmente.

```{.js}
// RUIM
GLOBAL_VARIABLE_A = 'something a'
globalVariableB = 'something b'

console.log(GLOBAL_VARIABLE_A);

// BOM
GLOBALS = {
    'variable_a': 'something a',
    'variable_b': 'something b',
}
console.log(GLOBALS.variable_a);
```

## Boas práticas em Pull Requests

#### Padrão dos nomes dos PRs

Os nomes dos PRs devem ser compostos por uma frase sucinta de até 120 caracteres que resuma bem qual funcionalidade está sendo adicionada ou qual bug está sendo resolvido naquele Pull Request. Abaixo você pode visualizar exemplos de um nome ruim e de um bom nome para determinado PR.

```
// RUIM
feature/create a new tool to the API query flow

// BOM
Create a SQL parser method on the data access flow
```

#### Uso de labels

Nos repositórios da Simbiose é uma regra que sejam definidas labels em seu PR, para facilitar a identificação do status do seu trabalho. Isso ajuda o revisor a compreender o que ele deve ou não revisar e também dá uma visibilidade para toda a equipe sobre o andamento de cada atividade definida. Abaixo você pode ver cada label possível de ser adicionada em um PR separada em dois grupos: tipo e estado. Portanto, todo PR deve conter uma label de cada grupo definido.

Tipo do PR:

- Bug: informa que o PR foi criado para a resolução de um bug do sistema;
- Feature: informa que o PR foi criado para adicionar uma nova funcionalidade ou melhorar alguma já existente;
- Test: informa que o PR foi criado para adicionar, corrigir ou melhorar testes existentes no sistema;
- Documentation: informa que o PR foi criado para adicionar alguma documentação necessária no código;

Estado do PR:

- Changes Requested: informa que o PR foi revisado e algumas mudanças foram requisitadas;
- In progress: informa que o PR ainda possui trabalho em desenvolvimento;
- Ready: informa que o trabalho executado no PR está finalizado e pode ser revisado;
- Temporary: informa que o PR é temporário e será deletado quando não estiver sendo mais usado;
- Waiting: informa que o PR depende de outra tarefa para que seja mergeado;

#### Padrão de mensagem de commit

A motivação de ter um padrão de mensagem advém da necessidade de reler as mesmas para entender o que aconteceu recentemente em um repositório a um nível mais granular.

O padrão de commit que será seguido está disponível online em [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/#specification).

Para toda mensagem de commit deve-se definir um dos prefixos abaixo:

- feat: add translation method to
- fix: solve bug on insertion flow
- test: add test for replication message
- docs: add parameter on query docstring

Outro ponto importante é a língua na qual essas mensagens estão escritas, que deve ser a mesma que o projeto usa, no nosso caso sempre o inglês. O padrão é explicitar claramente e de maneira unitária o que foi alterado naquele commit como `feat: add core data replication structure`.

Exemplos:

```{.py3}
# RUIM
I have changed the data replication structure

# BOM
feat: add core data replication structure
```

## Test Driven Development

[Test driven development](https://en.wikipedia.org/wiki/Test-driven_development) (TDD, ou, em português, [desenvolvimento guiado a testes](https://pt.wikipedia.org/wiki/Test-driven_development)) é um processo de desenvolvimento de código. Nele, deve-se primeiro criar um teste e então escrever código que faça este teste passar. Depois, repete-se para novas funcionalidades. O ciclo está apresentado abaixo.

1. Adicione um teste
2. Execute os testes e veja se o novo teste falha
3. Escreva o código
4. Execute os testes
5. Refatore o código
6. Repita

Ao iniciar pelos testes, o programador deve entender bem os requerimentos para que o teste seja completo, evitando deixar um caso importante passar. Caso os testes falhem na primeira escrita de código, devem ser modificados até que passem, somente. Otimizações serão realizadas na etapa de refatoração do código. O tamanho das etapas deve ser pequeno, de forma que se possa notar rapidamente se alguma modificação quebrou outros testes.

Importante: Leiam a página da wiki que foi passada no primeiro parágrafo, escolha a língua de sua preferência. Há uma explicação para cada uma das etapas apresentadas, o que é importante no desenvolvimento.

## Produtividade

Dicas e ferramentas para ajudar na sua produtividade no dia a dia.

#### Alias

No dia a dia digitamos muitos comandos, alguns bem longos. Para otimizar esse processo uma boa maneira é definir alias no sistema, ou seja, uma string que representa um atalho para um comando completo.

Esses alias funcionam no Linux e no Mac e onde você os define depende do sistema e distribuição, geralmente pode ser no arquivo `~/.bashrc` ou `~/.zshrc`.

**Exemplo 1**

Se você desenvolve em django por exemplo, muitas vezes vai utilizar o comando python manage.py <something>. Para otimizar esse trabalho você pode criar o alias:

    alias pman="python manage.py"

E assim só precisa escrever `pman <something>`.

**Exemplo 2**

Este exemplo serve para facilitar o trabalho de entrar na pasta dos projetos e ativar ambientes virtuais. Você pode resumir dois comandos longos em poucos caracteres:

    alias proj='cd ~/workspace/my_project; source ~/workspace/venvs/my_project/bin/activate'

**Exemplo 3**

Ao usar o git, constantemente você usa diversos comandos, e apesar de serem curtos, podem tomar um tempo e paciência razoável. Criar os seguintes alias pode ajudar muito:

    alias gs='git status'
    alias ga='git add'
    alias gd='git diff'
    alias gda='git diff --staged'
    alias gc='git commit'
    alias gl='git log'
    alias gps='git push'
    alias gpl='git pull'
    alias gco='git checkout'
    alias gf='git fetch'
    alias gr='git rebase'
    alias gpr='git pull --rebase'

É evidente que isso são exemplos e você pode definir como desejar, depende um pouco de seu gosto pessoal também.

**Exemplo 4**

Exemplos de alias para o gitflow:

    alias gffs='git flow feature start'
    alias gfff='git flow feature finish'
    alias gffps='git flow feature publish'
    alias gffpl='git flow feature pull'
    alias gfrs='git flow release start'
    alias gfrps='git flow release publish'
    alias gfrt='git flow release track'
    alias gfrf='git flow release finish'
    alias gfhfs='git flow hotfix start'
    alias gfhff='git flow hotfix finish'

#### ctrl + r

Ainda no sentido de diminuir o esforço de digitar e se lembrar de comandos, uma ferramente muito importante é o ctrl + r. Essa ferramente pode ser usada no terminal do Linux e Mac, e o que faz é buscar no histórico de comandos. Assim você pode achar facilmente aquele comando grep que você usou uma vez e foi muito útil, mas que não lembra mais.

A busca do ctrl + r se limita aos comandos armazenados no seu histórico de comandos, por isso é importante aumentar a quantidade de linhas que podem ser armazenadas, isso pode ser feito adicionando essas duas variáveis de ambiente (no mesmo arquivo que você define os alias, por exemplo):

    export HISTFILESIZE=10000
    export HISTSIZE=10000

Aqui nós definimos que o histórico armazena até 10 mil linhas por vez, sempre apagando as mais antigas.

#### Refactor

Como regra especial, fica definido que você não pode fazer um refactor de mais de 50% de um código sem antes consultar o gestor do projeto e os responsáveis pelo código que você quer refatorar. Isso é importante para dar visibilidade a todos de uma mudança tão grande e também para se avaliar em conjunto a necessidade desse refactor, pois várias horas de produtividade foram gastas ali.

## Complementos Git

Lista de bons materiais e links compartilhados por nossa equipe que dizem respeito à Git (Versionamento de Código) e outros assuntos relacionados.

Links úteis:

- [Git Cheat Sheet](https://www.git-tower.com/blog/git-cheat-sheet/)
- [Tutorial Interativo do git](https://docs.github.com/en/get-started/getting-started-with-git/set-up-git)
- [Documentação oficial do git](https://www.siteground.com/tutorials/getting-started/sg-git/)
- [Guia rápido de comandos do git](https://www.siteground.com/tutorials/getting-started/sg-git/)
- [Screencast do Fabio ASkita sobre Git](https://www.akitaonrails.com/2010/08/17/screencast-comecando-com-git)
- [Pro Git livro gratuito](https://git-scm.com/book/en/v2)

#### Versionamento

"Um sistema de controle de versão (ou versionamento), é um software com a finalidade de gerenciar diferentes versões no desenvolvimento de um documento qualquer. Esses sistemas são comumente utilizados no desenvolvimento de software para controlar as diferentes versões — histórico e desenvolvimento — dos códigos-fontes e também da documentação de software." - Wikipedia O Conceito de versionamento surgiu da necessidade de vários programadores trabalharem simultaneamente na mesma base de código de forma eficiente e facil. O versionamento de código permite que diferentes programadores, através de 'commits', submetam versões ou modificações para um repositório, em que os diferentes commits serão agregados.

#### O Git

O Git é um sistema de versionamento distribuido, ou seja, cada cópia do código fonte é na verdade um repositório em si, permitindo por exemplo que um programador possa submeter uma versão à qualquer outro repositório, sem que este seja necessariamente o repositório central (e.g. um programador X pode submeter uma versão do código à outro programador Y diretamente, sem passar pelo github). Em um sistema Linux baseado em Debian, instalar o git é simples:

`$ sudo apt-get install git`

Na S1mbi0se nós utilizamos o [Github](https://github.com/) como repositório central. Para configurar o acesso ao repositório central (Github) basta seguir as instruções no github: [Generating SSH Keys](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh)

### Conceitos no git

#### Clone

Para começar a usar uma base de código existente, primeiro precisamos clonar um repositório (geralmente o central). Ao clonar um repositório, você terá em seu computador uma cópia exata da base de código, bem como dos metadados do git. Para clonar por exemplo o repositório do AudienceUI, primeiro navegue até a sua pasta de projetos (e.g. ~/projects/):

    $ cd ~/projects/

Depois, com suas chaves SSH configuradas no github, execute o clone:

    $ git clone git@github.com:s1mbi0se/audienceui

Pronto, agora você tem uma cópia da base de código do AudienceUI, e pode navegar e visualizar os arquivos.

### Commit

O commit, é a maneira de criar um 'pacote' com suas modificações, que pode ser submetido a outros repositórios. Por exemplo, você modificou (e/ou criou novos) arquivos na pasta templates relativos à uma nova feature HTML. Primeiro você deve adicionar os arquivos modificados/adicionados ao processo de commit. você pode adicionar arquivos específicos, caso não queira submeter todas as modificações, ou adicicionar todos:

    $ # Adicionando um arquivo específico:
    $ git add caminho/para/arquivo/teste.html
    $ # Adicionando todos os arquivos que foram modificados/adicionados:
    $ git add --all

Com os arquivos adicionados, você pode agora "gerar um pacote" com suas modificações, o tal do commit:

    $ git commit -m "Implementada feature XYZ"

O parâmetro '-m' se refere à mensagem agregada ao commit, ele é obrigatório e essencial para sabermos exatamente quais modifições foram feitas neste commit.

#### Pull

O 'pull' é a maneira padrão de atualizar o repositório local com os commits de um repositório remoto. Ao fazer um 'pull' você estará efetivamente 'puxando' os commits do repositório remoto e aplicando-os ao reporitório local. O pull está intrinsicamente ligado ao conceito de 'Branch' ao qual não entrarei em muitos detalhes neste crash course. Para entender o conceito de branch por favor consulte o link: [O Que é um branch](https://git-scm.com/book/pt-br/v2/Branches-no-Git-Branches-em-poucas-palavras)

A sintaxe do pull é bem simples:

    $ git pull

O pull efetivamente copiará os commits do repositório do github e os aplicará em seu repositório local. Se houver algum arquivo conflitante (que foi modificado por você e por algum outro, de tal modo que o git não consiga juntá-los automaticamente) você receberá um aviso de que os arquivos devem ser 'mergeados'. O Merge será tratado com mais detalhes em um tópico abaixo.

#### Push

O 'push' é maneira padrão de submeter commits do repositório local ao repositório remoto. Ao fazer o 'push' você estará efetivamente 'empurrando' os commits locais ao repositório remoto. A sintaxe do push é identica à do pull:

    $ git push

Nota: Sempre faça um pull antes de fazer um push, para evitar Fast-Forwards.

#### Fast-Forward

O fast-forward é quando dois repositórios divergiram em seus commits e um pull ou push foi acionado. e.g. Um repositório originalmente em estado X foi modificado pelo programador P1 e pelo programador P2, que geraram os commits C1 e C2 respectivamente. O Programador P1 então faz push de seu commit para o github. O Programador P2, fará então um pull para garantir que esteja atualizado com a ultima versão do github, e aí então ocorrerá um fast forward. O commit C1 será trazido do servidor para o computador do progamador P2 e será 'mergeado' com o commit C2. Só depois deste merge (que pode ter sido automático ou manual), o programador P2 fará um push e enviará o commit C2 para o servidor, deixando o github atualizado com os commits C1 e C2. Note que por questões de consistência, o github bloqueia fast-forwards em seus servidores, portanto se o programador P2 não fizesse o pull, e fosse direto ao push, o git mostraria uma mensagem de erro, avisando que o repositório divergiu, e que é necessario fazer um pull e merge locais antes de submeter o seu commit.

#### Merge

O Merge acontece quando duas versões divergentes da base de código precisam ser agregadas. No exemplo do fast-forward acima, o merge ocorre quando P2 faz o pull. O git automaticamente tentará juntar os arquivos e gerar um novo novo commit que é o resultado da combinação C1+C2. O git faz um trabalho muito bom em merges automáticos, mas em alguns casos ele pode não conseguir mergear alguns arquivos, e gerará uma mensagem de conflito. Neste caso precisamos fazer o merge manual, via o comando:

    $ git mergetool

Assim que você tiver resolvido os conflitos pela ferramenta de merge, Você terá de 'terminar' o commit de merge que o git abriu automaticamente, com:

    $ git commit

Note que neste caso, não devemos passar o argumento '-m' para o commit, pois uma mensagem detalhada com o que foi mergeado é gerada pelo git. Assim que acionar o commit acima, o git abrirá um editor de arquivos em seu terminal com a mensagem que foi gerada pelo merge (você pode modificá-la se quiser), geralmente o VIM ou o nano. Salve o arquivo e feche o editor (':wq [enter]' no VIM e ctrl-o ctrl-x no nano), e está pronto o merge.
