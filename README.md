#Refatorção de Sistemas de Biblioteca

Violação 1
- Onde ocorre: Classe GerenciadorBiblioteca – Viola o SRP (Single Responsibility Principle).
- Descrição: A classe faz muitas coisas - gerencia livros, usuários, empréstimos, envia e-mails e SMS.
- Justificativa: Classes com múltiplas responsabilidades são difíceis de manter, testar e reutilizar.

Violação 2
- Onde ocorre: Método AdicionarUsuario – Viola o SRP + acoplamento forte.
- Descrição: Cadastra o usuário e envia e-mail de boas-vindas diretamente.
- Justificativa: Mistura responsabilidades e cria dependência direta com o mecanismo de notificação.

Violação 3
- Onde ocorre: Método RealizarEmprestimo – Viola SRP e DIP (Dependency Inversion Principle)
- Descrição: Faz lógica de negócio, altera estado do livro, e envia notificações via e-mail e SMS diretamente.
- Justificativa: Torna o método rígido e difícil de testar ou estender.

Violação 4
- Onde ocorre: Método RealizarDevolucao – Viola SRP e Clean Code (nome de função e retorno).
- Descrição: Calcula multa, altera estado do livro e envia notificação.
- Justificativa: Deveria haver uma classe específica para o cálculo de multas

Violação 5
- Onde ocorre: Falta de abstração para notificações – Viola OCP e DIP.
- Descrição: Notificações são feitas diretamente via métodos EnviarEmail e EnviarSMS.
- Justificativa: Se quiser trocar para outro serviço de notificação, será necessário alterar várias partes do código.
