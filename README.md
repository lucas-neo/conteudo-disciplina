# conteudo-disciplina
Claro! Aqui está a classe `Main` que demonstra o uso das classes `Evento`, `Workshop`, `Reuniao` e `EventoCorporativo`:

```java
package org.example;

public class Main {
    public static void main(String[] args) {
        // Criando um Workshop e tentando inscrever 31 participantes
        Workshop workshop = new Workshop("Workshop Java", "2024-10-10");
        for (int i = 1; i <= 31; i++) {
            boolean inscrito = workshop.inscreverParticipante("Participante " + i);
            if (inscrito) {
                System.out.println("Participante " + i + " inscrito com sucesso no Workshop.");
            } else {
                System.out.println("Não foi possível inscrever o Participante " + i + ". Limite de vagas atingido.");
            }
        }
        System.out.println("Total de participantes inscritos no Workshop: " + workshop.getParticipantes().size());
        System.out.println();

        // Criando uma Reunião Pública
        Reuniao reuniaoPublica = new Reuniao("Reunião Pública", "2024-11-10", false, null);
        System.out.println("Acessando Reunião Pública sem senha: " + reuniaoPublica.acessarReuniao(null));
        System.out.println("Acessando Reunião Pública com senha qualquer: " + reuniaoPublica.acessarReuniao("senha"));
        System.out.println();

        // Criando uma Reunião Privada
        Reuniao reuniaoPrivada = new Reuniao("Reunião Privada", "2024-11-10", true, "1234");
        System.out.println("Acessando Reunião Privada sem senha: " + reuniaoPrivada.acessarReuniao(null));
        System.out.println("Acessando Reunião Privada com senha errada: " + reuniaoPrivada.acessarReuniao("0000"));
        System.out.println("Acessando Reunião Privada com senha correta: " + reuniaoPrivada.acessarReuniao("1234"));
        System.out.println();

        // Criando um Evento Corporativo
        EventoCorporativo eventoCorporativo = new EventoCorporativo("Evento Corporativo", "2024-12-10", "Sala A", "Refeitório");
        System.out.println("Evento Corporativo criado:");
        System.out.println("Nome: " + eventoCorporativo.getNome());
        System.out.println("Data: " + eventoCorporativo.getData());
        System.out.println("Sala: " + eventoCorporativo.getSala());
        System.out.println("Refeitório: " + eventoCorporativo.getRefeitorio());
        System.out.println("Evento é privado? " + eventoCorporativo.isPrivada());
    }
}
```

**Explicação:**

- **Workshop:**
  - Tentamos inscrever 31 participantes em um workshop que tem um limite de 30.
  - Exibimos mensagens indicando se a inscrição foi bem-sucedida ou não.
  - Exibimos o total de participantes inscritos.

- **Reunião Pública:**
  - Criamos uma reunião pública que não requer senha.
  - Testamos o acesso sem senha e com uma senha aleatória.
  - Como é pública, ambos os acessos devem retornar `true`.

- **Reunião Privada:**
  - Criamos uma reunião privada que requer a senha "1234".
  - Testamos o acesso sem senha, com senha incorreta e com a senha correta.
  - Apenas o acesso com a senha correta deve retornar `true`.

- **Evento Corporativo:**
  - Criamos um evento corporativo com informações adicionais como sala e refeitório.
  - Exibimos os detalhes do evento.
  - Confirmamos que o evento é privado por padrão.

**Observação:**

Certifique-se de que todas as classes estejam no pacote `org.example` e que os métodos utilizados no `Main` estejam acessíveis. Se estiver usando um IDE como o Eclipse ou o IntelliJ IDEA, organize os arquivos em seus respectivos pacotes e certifique-se de que não haja conflitos de nomes.

**Estrutura de Diretórios:**

```
src/
├── org/example/
│   ├── Evento.java
│   ├── Workshop.java
│   ├── Reuniao.java
│   ├── EventoCorporativo.java
│   └── Main.java
```

Agora, você pode executar a classe `Main` para ver o resultado dos testes na saída do console.

Claro! Vamos criar a classe pai `Evento` e as três subclasses `Workshop`, `Reuniao` e `EventoCorporativo` para que passem nos testes fornecidos.

**Classe `Evento`:**

```java
package org.example;

public class Evento {
    private String nome;
    private String data;
    private boolean privada;

    public Evento(String nome, String data) {
        this.nome = nome;
        this.data = data;
    }

    public String getNome() {
        return nome;
    }

    public String getData() {
        return data;
    }

    public boolean isPrivada() {
        return privada;
    }

    protected void setPrivada(boolean privada) {
        this.privada = privada;
    }
}
```

**Classe `Workshop`:**

```java
package org.example;

import java.util.ArrayList;
import java.util.List;

public class Workshop extends Evento {
    private static final int MAX_PARTICIPANTES = 30;
    private List<String> participantes;

    public Workshop(String nome, String data) {
        super(nome, data);
        this.participantes = new ArrayList<>();
    }

    public boolean inscreverParticipante(String nomeParticipante) {
        if (participantes.size() < MAX_PARTICIPANTES) {
            participantes.add(nomeParticipante);
            return true;
        } else {
            return false;
        }
    }

    public List<String> getParticipantes() {
        return participantes;
    }
}
```

**Classe `Reuniao`:**

```java
package org.example;

public class Reuniao extends Evento {
    private String senha;

    public Reuniao(String nome, String data, boolean isPrivada, String senha) {
        super(nome, data);
        setPrivada(isPrivada);
        this.senha = senha;
    }

    public boolean acessarReuniao(String senhaTentativa) {
        if (isPrivada()) {
            if (senha == null) {
                return senhaTentativa == null;
            }
            return senha.equals(senhaTentativa);
        } else {
            return true;
        }
    }
}
```

**Classe `EventoCorporativo`:**

```java
package org.example;

public class EventoCorporativo extends Evento {
    private String sala;
    private String refeitorio;

    public EventoCorporativo(String nome, String data, String sala, String refeitorio) {
        super(nome, data);
        this.sala = sala;
        this.refeitorio = refeitorio;
        setPrivada(true); // EventoCorporativo é privado por padrão
    }

    public String getSala() {
        return sala;
    }

    public String getRefeitorio() {
        return refeitorio;
    }
}
```

Essas classes devem passar nos testes fornecidos. Note que a lógica em `Reuniao` considera que se a reunião é privada, ela requer uma senha para acesso; se for pública, pode ser acessada sem senha. A propriedade `isPrivada()` reflete corretamente o estado da reunião.
