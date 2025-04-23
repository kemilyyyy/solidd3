// Entidades
public class Livro
{
    public string Titulo { get; set; }
    public string Autor { get; set; }
    public string ISBN { get; set; }
    public bool Disponivel { get; set; } = true;
}

public class Usuario
{
    public string Nome { get; private set; }
    public int ID { get; private set; }

    pulic 
}

public class Emprestimo
{
    public Livro Livro { get; set; }
    public Usuario Usuario { get; set; }
    public DateTime DataEmprestimo { get; set; }
    public DateTime DataDevolucaoPrevista { get; set; }
    public DateTime? DataDevolucaoEfetiva { get; set; }
}

public class EmprestimoService
{
    private List<Emprestimo> emprestimos = new List<Emprestimo>();
    private readonly INotificador notificador;

    public EmprestimoService(INotificador notificador)
    {
        this.notificador = notificador;
    }

    public bool RealizarEmprestimo(Livro livro, Usuario usuario, int diasEmprestimo)
    {
        if (livro == null || usuario == null || !livro.Disponivel)
            return false;

        livro.Disponivel = false;

        var emprestimo = new Emprestimo
        {
            Livro = livro,
            Usuario = usuario,
            DataEmprestimo = DateTime.Now,
            DataDevolucaoPrevista = DateTime.Now.AddDays(diasEmprestimo)
        };

        emprestimos.Add(emprestimo);

        notificador.Notificar(usuario.Nome, "Empréstimo Realizado", $"Você pegou o livro: {livro.Titulo}");

        return true;
    }

// Interfaces de notificação
public interface INotificador
{
    void Notificar(string destinatario, string assunto, string mensagem);
}

public class EmailNotificador : INotificador
{
    public void Notificar(string destinatario, string assunto, string mensagem)
    {
        Console.WriteLine($"E-mail enviado para {destinatario}. Assunto: {assunto}");
    }
}

public class SmsNotificador : INotificador
{
    public void Notificar(string destinatario, string assunto, string mensagem)
    {
        Console.WriteLine($"SMS enviado para {destinatario}: {mensagem}");
    }
}

public class NotificacaoComposta : INotificador
{
    private readonly List<INotificador> notificadores;

    public NotificacaoComposta(List<INotificador> notificadores)
    {
        this.notificadores = notificadores;
    }

    public void Notificar(string destinatario, string assunto, string mensagem)
    {
        foreach (var notificador in notificadores)
        {
            notificador.Notificar(destinatario, assunto, mensagem);
        }
    }
}

// Serviços
public class LivroService
{
    private List<Livro> livros = new List<Livro>();

    public void AdicionarLivro(string titulo, string autor, string isbn)
    {
        livros.Add(new Livro { Titulo = titulo, Autor = autor, ISBN = isbn });
    }

    public Livro BuscarPorISBN(string isbn) => livros.Find(l => l.ISBN == isbn);
    public List<Livro> ListarTodos() => livros;
}

public class UsuarioService
{
    private List<Usuario> usuarios = new List<Usuario>();
    private readonly INotificador notificador;

    public UsuarioService(INotificador notificador)
    {
        this.notificador = notificador;
    }

    public void AdicionarUsuario(string nome, int id)
    {
        usuarios.Add(new Usuario { Nome = nome, ID = id });
        notificador.Notificar(nome, "Bem-vindo à Biblioteca", "Você foi cadastrado!");
    }

    public Usuario BuscarPorId(int id) => usuarios.Find(u => u.ID == id);
    public List<Usuario> ListarTodos() => usuarios;
}

public class EmprestimoService
{
    private List<Emprestimo> emprestimos = new List<Emprestimo>();
    private readonly INotificador notificador;

    public EmprestimoService(INotificador notificador)
    {
        this.notificador = notificador;
    }

    public double RealizarDevolucao(string isbn, int usuarioId)
    {
        var emprestimo = emprestimos.Find(e => e.Livro.ISBN == isbn && e.Usuario.ID == usuarioId && e.DataDevolucaoEfetiva == null);
        if (emprestimo == null) return -1;

        emprestimo.DataDevolucaoEfetiva = DateTime.Now;
        emprestimo.Livro.Disponivel = true;

        double multa = 0;
        if (DateTime.Now > emprestimo.DataDevolucaoPrevista)
        {
            TimeSpan atraso = DateTime.Now - emprestimo.DataDevolucaoPrevista;
            multa = atraso.Days * 1.0;
            notificador.Notificar(emprestimo.Usuario.Nome, "Multa por Atraso", $"Multa de R$ {multa}");
        }

        return multa;
    }

    public List<Emprestimo> ListarTodos() => emprestimos;
}

// Classe de Execução
public class Program
{
    public static void Main(string[] args)
    {
        var notificador = new NotificacaoComposta(new List<INotificador> { new EmailNotificador(), new SmsNotificador() });

        var livroService = new LivroService();
        var usuarioService = new UsuarioService(notificador);
        var emprestimoService = new EmprestimoService(notificador);

        livroService.AdicionarLivro("Clean Code", "Robert C. Martin", "978-0132350884");
        livroService.AdicionarLivro("Design Patterns", "Erich Gamma", "978-0201633610");

        usuarioService.AdicionarUsuario("João Silva", 1);
        usuarioService.AdicionarUsuario("Maria Oliveira", 2);

        var livro = livroService.BuscarPorISBN("978-0132350884");
        var usuario = usuarioService.BuscarPorId(1);

        emprestimoService.RealizarEmprestimo(livro, usuario, 7);

        // Simular atraso
        System.Threading.Thread.Sleep(1000);
        double multa = emprestimoService.RealizarDevolucao("978-0132350884", 1);
        Console.WriteLine($"Multa por atraso: R$ {multa}");

        Console.ReadLine();
    }
}
