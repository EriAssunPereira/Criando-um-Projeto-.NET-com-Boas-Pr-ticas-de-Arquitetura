# Criando-um-Projeto-.NET-com-Boas-Práticas-de-Arquitetura

Para criar um projeto .NET com boas práticas de arquitetura, vamos seguir um padrão de estruturação modular, utilizando separação de responsabilidades e organização adequada dos arquivos. Vou descrever uma abordagem comum usando o padrão de arquitetura em camadas (camada de apresentação, camada de aplicação/serviço, camada de domínio e camada de infraestrutura) e exemplos de código para cada parte.

### Estrutura do Projeto

1. **Camada de Apresentação (Presentation Layer)**:
   - Responsável pela interação com o usuário. Geralmente contém interfaces gráficas, APIs REST, etc.
   - Exemplo de estrutura:
     ```
     /MyProject.Web (projeto ASP.NET MVC/WebAPI)
         /Controllers
             HomeController.cs
             ApiController.cs
         /ViewModels
             IndexViewModel.cs
             ApiResponse.cs
         /Views
             Home
                 Index.cshtml
                 Error.cshtml
         Startup.cs
     ```

2. **Camada de Aplicação (Application Layer)**:
   - Lida com a lógica de aplicação, orquestração de serviços e mapeamento entre camadas.
   - Exemplo de estrutura:
     ```
     /MyProject.Application (projeto de classe library)
         /Services
             UserService.cs
             ProductService.cs
         /DTOs
             UserDTO.cs
             ProductDTO.cs
         MyProject.Application.csproj
     ```

3. **Camada de Domínio (Domain Layer)**:
   - Contém as entidades de domínio, regras de negócio e interfaces de serviços.
   - Exemplo de estrutura:
     ```
     /MyProject.Domain (projeto de classe library)
         /Entities
             User.cs
             Product.cs
         /Interfaces
             IUserRepository.cs
             IProductService.cs
         MyProject.Domain.csproj
     ```

4. **Camada de Infraestrutura (Infrastructure Layer)**:
   - Fornece suporte técnico para as outras camadas (banco de dados, logging, etc.).
   - Exemplo de estrutura:
     ```
     /MyProject.Infrastructure (projeto de classe library)
         /Data
             ApplicationDbContext.cs
             UserRepository.cs
         /Logging
             Logger.cs
         MyProject.Infrastructure.csproj
     ```

### Exemplos de Código

Aqui estão alguns exemplos de código para cada camada, seguindo o padrão proposto:

#### Exemplo na Camada de Domínio

**User.cs** (Entidade de domínio):
```csharp
namespace MyProject.Domain.Entities
{
    public class User
    {
        public int Id { get; set; }
        public string Username { get; set; }
        public string Email { get; set; }
        // outras propriedades
    }
}
```

**IUserRepository.cs** (Interface do repositório):
```csharp
namespace MyProject.Domain.Interfaces
{
    public interface IUserRepository
    {
        User GetById(int userId);
        void Add(User user);
        void Update(User user);
        void Delete(int userId);
        // outras operações de CRUD
    }
}
```

#### Exemplo na Camada de Aplicação

**UserService.cs** (Serviço de aplicação):
```csharp
namespace MyProject.Application.Services
{
    public class UserService
    {
        private readonly IUserRepository _userRepository;

        public UserService(IUserRepository userRepository)
        {
            _userRepository = userRepository;
        }

        public UserDTO GetUserById(int userId)
        {
            var user = _userRepository.GetById(userId);
            // mapeamento de User para UserDTO
            return new UserDTO { Id = user.Id, Username = user.Username, Email = user.Email };
        }

        // outras operações de negócio
    }
}
```

#### Exemplo na Camada de Apresentação

**HomeController.cs** (Controlador MVC):
```csharp
namespace MyProject.Web.Controllers
{
    public class HomeController : Controller
    {
        private readonly UserService _userService;

        public HomeController(UserService userService)
        {
            _userService = userService;
        }

        public IActionResult Index()
        {
            var userViewModel = _userService.GetUserById(1); // exemplo de uso do serviço
            return View(userViewModel);
        }

        // outras actions do controlador
    }
}
```

### Considerações Finais

Este é um exemplo básico de como organizar um projeto .NET com boas práticas de arquitetura. A estrutura pode variar dependendo do tamanho e complexidade do projeto, mas seguir um padrão claro de separação de responsabilidades ajuda a escalar o projeto de maneira mais organizada e segura. Além disso, o uso de injeção de dependência (DI) e interfaces facilita a manutenção e testabilidade do código.
