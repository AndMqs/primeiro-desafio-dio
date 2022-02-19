##Sistema Distribuído Seguro

​	O Git foi projetado com a integridade do código-fonte gerenciado como uma prioridade. O conteúdo dos arquivos, bem como os verdadeiros relacionamentos entre arquivos e diretórios, versões, tags e commits, todos esses objetos no repositório do Git são protegidos com um algoritmo de hash de criptografia seguro chamado SHA1. Isso protege o código e o histórico de alterações contra alterações acidentais e maliciosas e garante que o histórico tenha rastreabilidade total.

- ###SHA1

​	O **SHA (Secure Hash Algorithm)** é uma função de hash criptográfica, que foi criada na década de 1990 como uma espécie de chave para proteger as informações inseridas em um site ou servidor.

​	Um certificado digital SSL funciona como uma espécie de “corredor” onde as informações trafegam entre usuário e servidor. Neste caminho, a informação é criptografada e só pode ser acessada por uma chave com duas versões, uma no servidor e outra com o usuário. O SHA é exatamente esta chave, que criptografa as informações e as interpreta, isso tudo em frações de segundos

​	O git faz uso desse algoritmo de criptação para identificar os arquivos de uma forma segura e curta através do conjunto de caracteres identificador de 40 dígitos. Sempre que rodamos o arquivo, o sha1 trás uma chave diferente.

https://suporte.siteblindado.com.br/hc/pt-br/articles/115002297391-O-que-%C3%A9-SHA-



##Objetos Internos do Git

​	Existem 4 tipos de objetos que são armazenados no git: commit objects, annotaded tag objects, blobs and tree objects, para iniciar o detalhamento desses objetos considere a seguinte estrutura de arquivos:

![img](https://deviniciative.files.wordpress.com/2019/06/arvore_arquivos.png)

Cada arquivo no Git é armazenado como um objeto blob, por exemplo, a partir do conteúdo do arquivo logo.png ele gera um hash que será armazenado em algum lugar endereçável como *aa1b2fb696a831c89c53f787e03d863691d2b671 *. O mesmo ocorre com o arquivo app.css que é armazenado em *4c511f16ef2644854d04cabebfcecc82be0eb04f *e assim também acontece com o arquivo app.js. Ótimo, e como o Git relaciona o arquivo logo.png com o hash *aa1b2fb696a831c89c53f787e03d863691d2b671*?

Os arquivos logo.png e app.css estão no mesmo diretório assets, e este diretório é representado como um tree object, ele trabalha como um mapa, ele relaciona o nome do arquivo ao hash gerado a partir de seu conteúdo, como exemplificado abaixo:



![img](https://deviniciative.files.wordpress.com/2019/06/tree_um_diretorio.png)



Atrás de um hash pode existir um simples arquivo, armazenado como blob, ou outro tree object, como nessa representação “gitiana” da estrutura de arquivos completa:

![img](https://deviniciative.files.wordpress.com/2019/06/tree_completa-1.png)

​	A pasta assets e o arquivo app.js estão no mesmo diretório e num nível superior aos arquivos logo.png e app.css, e novamente o mapeamento é aplicado fazendo a associação do nome asset (diretório) ao hash *7cf2a17f3345635d59e063cffddd23573b6e4a75*, repare que aqui o conteúdo não é de um arquivo, mas de um tree object filho, pois ele está debaixo do diretório raiz “.” . E o arquivo app.js é associado ao hash *29bfcf9fa5824331081b31f0c307806c6f6b6f06*. E, se subirmos ao topo da estrutura veremos que o diretório raiz também está associado a um hash, *9c435a86e664be00db0d973e981425e4a3ef3f8d*.

​	Quando você executa o comando *git add* o git bate uma foto (snapshot) do working directory e a armazena em sua memória interna, quando você executar o comando *git commit* o seguinte objeto de commit num pseudo código será gerado:

```
sha1(
 commit message => feature commit”
 committer => Lilian Lima < lilian.lima@mail.com>
 commit date => Sat Jun 8 09:15:59 2019 +0100
 author => Lilian Lima < lilian.lima@mail.com>
 author date => Sat Jun 8 09:15:59 2019 +0100
 tree => 508222e4b5ba555711173ff140a6263bef867166
 parents => [d0957cc8c91520eedcd3807baa96170020812f0e]
)
```

​	Só reforçando, o objeto de commit é composto pelo metadata do commit+ tree object (snapshot do working directory) + identificador do commit antecessor/ancestral/pai

​	E adivinhem o que o Git faz com esse objeto de commit?!? Gera um hash dele também e é esse identificador que visualizamos ao executarmos um *git log*. O Git utiliza essa abordagem de criptografia para assegurar a integridade dos commits, caso um bit seja alterado no commit realizado, mesmo que seja um espaço em branco num arquivo, um novo identificador será gerado.



Bibliografia:

[Developer Initiative](https://deviniciative.wordpress.com/2019/06/28/a-anatomia-de-um-commit-no-git/#:~:text=Cada%20arquivo%20no%20Git%20%C3%A9,ocorre%20com%20o%20arquivo%20app.)

[Site blindado](https://suporte.siteblindado.com.br/hc/pt-br/articles/115002297391-O-que-%C3%A9-SHA-)

[Dio](https://www.dio.me/articles/introducao-ao-git-e-ao-github-material-curso-dio)