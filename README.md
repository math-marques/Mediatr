Facilitando Comunicação entre Componentes com Mediatr em C#
#C#
Introdução:
Mediatr é uma biblioteca popular no ecossistema .NET que permite uma abordagem mais limpa e desacoplada para o envio de comandos e eventos entre componentes. Com o ele, conseguimos simplificar a comunicação dentro da nossa aplicação, sem precisar de dependências diretas entre classes. Neste artigo, exploro o que é o Mediatr, funcionamento e como podemos utilizá-lo em projetos de back-end em C#. Vamos criar uma aplicação simples para entender como integrar Mediatr de forma eficaz.



O que é o Mediatr?
Biblioteca que segue o padrão Mediator, que permite que objetos se comuniquem entre si sem que haja dependências diretas entre eles. Em vez de fazer chamadas diretas de uma classe para outra, um componente envia uma mensagem para um mediador, que então é responsável por encaminhar essa mensagem para o destinatário adequado. Exemplificando, em uma aplicação de integração de APIs, você pode usar o Mediatr para desacoplar a lógica de negócios da lógica de controle de fluxo.

Por que usar o Mediatr?
Desacoplamento: É como se os componentes da aplicação não precisassem ter conhecimento uns dos outros. Isso facilita manutenção e evolução do código. Essa é a principal intenção dos frameworks, desacoplar, oferecer independência para seguir com as outras características abaixo.
Legibilidade: Centralizando a lógica de comunicação, o código fica mais organizado e fácil de entender.
Escalabilidade: A tão famosa e mencionada escalabilidade permite que adicionar novas funcionalidades sem ter que modificar muitas classes existentes. É bom ter uma noção sucinta sobre o SOLID e como melhorar e ter boas práticas.
Manutenção: Facilita alterações futuras, já que cada comando e consulta é encapsulado em uma classe separada.


Mão na massa: Instalando o Mediatr no Projeto
Para começar, precisamos adicionar o pacote do Mediatr ao projeto. Se estiver usando o NuGet, basta rodar o seguinte comando:

Install-Package MediatR
Ou, se você estiver usando o .NET CLI, execute:

dotnet add package MediatR
Usando o Mediatr - Exemplo Prático
Agora, vamos ver como podemos usar o Mediatr em uma aplicação simples.

Criação do Comando e do Handler
No Mediatr, as mensagens que são enviadas entre os componentes podem ser de dois tipos: Comandos (para realizar ações) e Consultas (para retornar dados).

Vamos criar um comando simples que registra um usuário:

public class RegisterUserCommand : IRequest<bool>
{
  public string Name { get; set; }
  public string Email { get; set; }

  public RegisterUserCommand(string name, string email)
  {
      Name = name;
      Email = email;
  }
}
Agora, criamos o Handler para esse comando:

public class RegisterUserCommandHandler : IRequestHandler<RegisterUserCommand, bool>
{
  public Task<bool> Handle(RegisterUserCommand request, CancellationToken cancellationToken)
  {
      // Aqui, normalmente você faria algo como salvar os dados em um banco de dados
      Console.WriteLine($"Registrando usuário: {request.Name}, {request.Email}");
      return Task.FromResult(true);
  }
}
Configurando o Mediatr na Aplicação
Agora, vamos configurar o Mediatr dentro da nossa aplicação. Em uma aplicação ASP.NET Core, você adicionaria o Mediatr no Startup.cs ou Program.cs (no caso do .NET 6+):

public class Startup
{
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddMediatR(Assembly.GetExecutingAssembly()); // Registra os handlers
  }
}
Enviando o Comando
Finalmente, vamos ver como enviar o comando para o Mediatr, que vai encaminhá-lo para o handler:

public class UserController : ControllerBase
{
  private readonly IMediator _mediator;

  public UserController(IMediator mediator)
  {
      _mediator = mediator;
  }

  [HttpPost("register")]
  public async Task<IActionResult> RegisterUser([FromBody] RegisterUserCommand command)
  {
      bool result = await _mediator.Send(command);
      if (result)
      {
          return Ok("Usuário registrado com sucesso!");
      }
      return BadRequest("Falha no registro.");
  }
}
Conclusão
Mediatr é uma ferramenta massa para desacoplar os componentes de uma aplicação. Ele facilita a comunicação entre diferentes partes do sistema sem criar dependências diretas, o que torna o código mais limpo e de fácil manutenção.

image

Bibliografia:

ASP .NET Core - Usando o padrão Mediator com MediatR (CQRS)

C# Mediator Pattern
