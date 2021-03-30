Criar model ( 
Categoria [ id descricao]


Arquivo Startup:
Método Configure service: Onde adicionamos as injeções de dependências
Configure: Adicionar todos os middlewares que vamos utilizar

Arquivo Program:
Para iniciar o projecto, lá diz que a classe inicial é a STARTUP

Instalar os pacotes: Ferramentas >> gerenciador de Pacotes Nuget >> Console de gerenciador de pacotes
Antes disso instalei o SqlServer Express (versão core)
id da instância : SQLEXPRESS
Instância nomeada : SQLExpress
Instalar o Entityframework para sqlserver
1 - Install-Package Microsoft.EntityFrameworkCore.SqlServer
(Não consegui fazer o link com o SQLServer, usei o Mysql >>> Install-Package MySql.Data.EntityFrameworkCore)
2 - Install-Package Microsoft.EntityFrameworkCore.Tools

Criar na pasta Model a classe Context, para definir configurações do BD

Context : DbContext (fazê-la herdar do dbcontext e importá-lo do entityframeworkCore)
Sobrescrever o método OnConfiguring. (método responsável por configurar o nosse entityframework)
string de conexão sqlserver: @"Server=(localdb)\mssqllocaldb;Database=Cursomvc;Integrated Security=True"
string de conexão mySql: "server=localhost;database=Cursomvc;user=root;password="


Ainda no Context, relacioná-lo à entidade Categorias assim: public DbSet<Categoria> Categorias { get; set; }

Adicionar novo service no método do startup no método ConfigureService assim: services.AddDbContext<Context>(); (Não esquecer de importar a classe que criamos, Context)

Voltar para o console e adicionar o migration assim: Add-Migration InitialCreate (Esse último é o nome que damos ao nosso migrate)
Criou-se uma nova pasta com a data e o nosso initialCreate e tirou um snapshot do nosso migrate

Depois de criar o Migrate, precisamos efetivamente criar o Banco de dados, rodando o comando Update-Database

Criar novo item na pasta controller com o scarfold, clicando com o botão direito na pasta >> Adicionar >> Scaffold
Criar um iitem do tipo Controlador MVC com exibições , usando o Entity Framework.
Indicar o modelo, que neste caso é a classe "Categoria" e o contexto, que é o "Context", deixar todas as três opções marcadas.
Vai ser gerada um novo controller com os métodos de listar, consultar uma categoria, get, post...
Acho que vão ser geradas as Views de Categoria também.
Compilar a solução novamente, mas Categoria ainda não foi adicionada no menu.
Para adicionar Categoria ao menu, precisamos ir na pasta VIEWS >> SHARED >> _Layout
Copiar e colar mais um item da NAVBAR mudar o asp-controller, asp-action e o nome detro da tag, assim:

                        <li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-controller="Categorias" asp-action="Index">Categorias</a>
                        </li>


CRIANDO A MODEL DE PRODUTO

Botão direito em Model >> Adicionar >> Classe
A palavra "prop" + 2 tabs é um atalho para criar uma variável com get e set;
Para relacionar com a categoria, precisamos criar variáveis para id da categoria e uma variável do tipo Categoria:

    public class Produto
    {
        public int Id { get; set; }
        public string Descricao { get; set; }
        public int Quantidade { get; set; }
        public int CategoriaId { get; set; }
        public Categoria Categoria { get; set; }
    }

Voltar na tabela Categoria e adicionar uma lista de Pordutos, assim: public List<Produto> Produtos { get; set; }

Agora precisamos adicionar o Produto no Context para adicionar o DbSet de Produtos, assim: public DbSet<Produto> Produtos { get; set; }

Adicionar mais um Migration, por ter criado a tabela Produto, assim: Add-Migration TabelaProduto

Update-Database para Efetivar as mudanças no banco


Criar novo item na pasta controller com o scaffold, clicando com o botão direito na pasta >> Adicionar >> Scaffold
Criar um iitem do tipo Controlador MVC com exibições , usando o Entity Framework.
Indicar o modelo, que neste caso é a classe "Produto" e o contexto, que é o "Context", deixar todas as três opções marcadas.
Vai ser gerada um novo controller com os métodos de listar, consultar uma categoria, get, post...
Acho que vão ser geradas as Views de Produto também.
Compilar a solução novamente, mas Categoria ainda não foi adicionada no menu.
Para adicionar Categoria ao menu, precisamos ir na pasta VIEWS >> SHARED >> _Layout
Copiar e colar mais um item da NAVBAR mudar o asp-controller, asp-action e o nome detro da tag, assim:
			<li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-controller="Produtos" asp-action="Index">Produtos</a>
                        </li>
No ProdutoController a linha abaixo representa o campo de seleção da categoria, que neste momento só apresenta os ids das cotegorias:
	ViewData["CategoriaId"] = new SelectList(_context.Categorias, "Id", "Id", produto.CategoriaId);
Para mostrar a descrição das categorias, basta mudar o terceiro campo de todos os VIEWDATA do ProdutoController de "Id" para "Descricao" (isso mudará só nas caixas de seleção)

Mas ainda aparecem os IDs nas Views de Listar e Deletar, precisamos ir em VIEW >> PRODUTO e nas views vamos alterar para aparecer a descrição das categorias:
1- View Index: @Html.DisplayFor(modelItem => item.Categoria.Id) [ATERAMOS PARA] @Html.DisplayFor(modelItem => item.Categoria.Descricao)
2- View Details: @Html.DisplayFor(model => model.Categoria.Id) [ATERAMOS PARA] @Html.DisplayFor(model => model.Categoria.Descricao)
3- View Delete: @Html.DisplayFor(model => model.Categoria.Id) [ATERAMOS PARA] @Html.DisplayFor(model => model.Categoria.Descricao)

