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



**1. Classe e Objeto**

- **Teoria:**
  - **Classe:** Em Java, uma classe é um modelo ou blueprint a partir do qual objetos são criados. Ela define um conjunto de propriedades (atributos) e comportamentos (métodos) que os objetos daquela classe possuem.
  - **Objeto:** Um objeto é uma instância de uma classe. Ele representa uma entidade concreta que possui estado e comportamento conforme definido pela classe.

- **Aplicação Prática:**
  - **Definição de uma Classe:**
    ```java
    public class Pessoa {
        // Atributos
        private String nome;
        private int idade;

        // Construtor
        public Pessoa(String nome, int idade) {
            this.nome = nome;
            this.idade = idade;
        }

        // Métodos
        public void apresentar() {
            System.out.println("Olá, meu nome é " + nome + " e tenho " + idade + " anos.");
        }
    }
    ```
  - **Criação de um Objeto:**
    ```java
    public class Main {
        public static void main(String[] args) {
            Pessoa pessoa1 = new Pessoa("João", 30);
            pessoa1.apresentar();
        }
    }
    ```
    *Saída:*
    ```
    Olá, meu nome é João e tenho 30 anos.
    ```

- **Quando Usar:**
  - Sempre que você quiser representar entidades do mundo real ou conceitos abstratos em seu programa.
  - Classes e objetos são a base da programação orientada a objetos; portanto, são usados em praticamente todo desenvolvimento Java.

---

**2. Encapsulamento**

- **Teoria:**
  - O encapsulamento é um princípio da POO que consiste em esconder os detalhes internos de uma classe e expor apenas o necessário através de métodos públicos. Isso protege os dados internos de acessos indevidos e alterações não controladas.

- **Aplicação Prática:**
  - **Uso de Modificadores de Acesso:**
    ```java
    public class ContaBancaria {
        // Atributos privados
        private double saldo;

        // Construtor
        public ContaBancaria(double saldoInicial) {
            this.saldo = saldoInicial;
        }

        // Métodos públicos para acessar os atributos
        public double getSaldo() {
            return saldo;
        }

        public void depositar(double valor) {
            if (valor > 0) {
                saldo += valor;
            }
        }

        public void sacar(double valor) {
            if (valor > 0 && valor <= saldo) {
                saldo -= valor;
            }
        }
    }
    ```
  - **Interação com a Classe:**
    ```java
    public class Main {
        public static void main(String[] args) {
            ContaBancaria conta = new ContaBancaria(1000.0);
            conta.depositar(500.0);
            conta.sacar(200.0);
            System.out.println("Saldo atual: " + conta.getSaldo());
        }
    }
    ```
    *Saída:*
    ```
    Saldo atual: 1300.0
    ```

- **Quando Usar:**
  - Sempre que quiser proteger os dados internos de uma classe contra acessos externos não autorizados.
  - Para manter a integridade dos dados e facilitar a manutenção e evolução do código.
  - O encapsulamento permite controlar como os atributos de uma classe são acessados e modificados.

---

**3. Herança**

- **Teoria:**
  - Herança é um mecanismo que permite que uma classe (subclasse) herde atributos e métodos de outra classe (superclasse). Isso promove reutilização de código e estabelece um relacionamento "é um" entre classes.

- **Aplicação Prática:**
  - **Definição de uma Superclasse:**
    ```java
    public class Animal {
        public void comer() {
            System.out.println("O animal está comendo.");
        }
    }
    ```
  - **Definição de uma Subclasse que herda de Animal:**
    ```java
    public class Cachorro extends Animal {
        public void latir() {
            System.out.println("O cachorro está latindo.");
        }
    }
    ```
  - **Uso das Classes:**
    ```java
    public class Main {
        public static void main(String[] args) {
            Cachorro cachorro = new Cachorro();
            cachorro.comer(); // Método herdado da classe Animal
            cachorro.latir(); // Método da classe Cachorro
        }
    }
    ```
    *Saída:*
    ```
    O animal está comendo.
    O cachorro está latindo.
    ```

- **Quando Usar:**
  - Quando há uma relação natural de hierarquia entre classes, e você deseja reutilizar código da superclasse.
  - Para implementar polimorfismo, permitindo que objetos de diferentes classes sejam tratados como objetos de uma classe base comum.
  - Ao modelar sistemas onde objetos compartilham características comuns, mas também têm comportamentos específicos.

---

**Dicas Adicionais para a Prova Prática:**

- **Classe e Objeto:**
  - Esteja preparado para definir classes com atributos e métodos apropriados.
  - Pratique a instânciação de objetos e o uso de seus métodos.

- **Encapsulamento:**
  - Use os modificadores de acesso corretamente (`private`, `public`, `protected`).
  - Implemente métodos getters e setters quando necessário.
  - Pense em como proteger os dados da sua classe contra usos indevidos.

- **Herança:**
  - Identifique oportunidades para criar hierarquias de classes.
  - Lembre-se de usar a palavra-chave `extends` para indicar herança.
  - Esteja ciente de como funciona a chamada de construtores em hierarquias de classes.

**Conselhos Gerais:**

- **Prática:** Codifique exemplos que utilizem esses conceitos para fixar o conhecimento.
- **Leitura de Código:** Seja capaz de ler um código que usa esses conceitos e explicar o que está acontecendo.
- **Aplicação em Problemas Reais:** Pense em como esses conceitos podem ser aplicados para resolver problemas práticos que possam ser apresentados na prova.

Boa sorte na sua prova de Programação Orientada a Objetos em Java!


**4. Polimorfismo**

- **Teoria:**
  - O polimorfismo é um dos pilares da programação orientada a objetos que permite que objetos de diferentes classes sejam tratados como objetos de uma classe base comum. Em Java, isso significa que uma referência de tipo da superclasse pode apontar para objetos de suas subclasses, permitindo que métodos sejam chamados de forma polimórfica.

- **Aplicação Prática:**
  - **Exemplo com Classes de Animais:**
    ```java
    // Classe base
    public class Animal {
        public void fazerSom() {
            System.out.println("O animal faz um som.");
        }
    }

    // Subclasse Cachorro
    public class Cachorro extends Animal {
        @Override
        public void fazerSom() {
            System.out.println("O cachorro late.");
        }
    }

    // Subclasse Gato
    public class Gato extends Animal {
        @Override
        public void fazerSom() {
            System.out.println("O gato mia.");
        }
    }

    // Uso do Polimorfismo
    public class Main {
        public static void main(String[] args) {
            Animal meuAnimal1 = new Cachorro();
            Animal meuAnimal2 = new Gato();

            meuAnimal1.fazerSom(); // Saída: O cachorro late.
            meuAnimal2.fazerSom(); // Saída: O gato mia.
        }
    }
    ```
    - Neste exemplo, embora `meuAnimal1` e `meuAnimal2` sejam do tipo `Animal`, eles referenciam objetos das subclasses `Cachorro` e `Gato`, respectivamente. Ao chamar `fazerSom()`, o método específico da subclasse é executado.

- **Quando Usar:**
  - Quando você deseja escrever código que pode trabalhar com classes diferentes de forma uniforme.
  - Ao implementar interfaces ou herança, permitindo que métodos possam ser substituídos (overridden) em subclasses para fornecer comportamentos específicos.
  - Para facilitar a extensão e manutenção do código, permitindo adicionar novas classes que se comportam de maneira previsível dentro do sistema existente.

---

**5. Abstração**

- **Teoria:**
  - A abstração é o processo de ocultar os detalhes complexos de implementação e mostrar apenas as funcionalidades essenciais de um objeto. Em Java, isso é alcançado através de classes abstratas e interfaces, que definem métodos abstratos que devem ser implementados pelas subclasses.

- **Aplicação Prática:**
  - **Exemplo com Classe Abstrata:**
    ```java
    public abstract class Forma {
        public abstract double calcularArea();
    }

    public class Circulo extends Forma {
        private double raio;

        public Circulo(double raio) {
            this.raio = raio;
        }

        @Override
        public double calcularArea() {
            return Math.PI * raio * raio;
        }
    }

    public class Retangulo extends Forma {
        private double largura;
        private double altura;

        public Retangulo(double largura, double altura) {
            this.largura = largura;
            this.altura = altura;
        }

        @Override
        public double calcularArea() {
            return largura * altura;
        }
    }

    // Uso da Abstração
    public class Main {
        public static void main(String[] args) {
            Forma forma1 = new Circulo(5);
            Forma forma2 = new Retangulo(4, 6);

            System.out.println("Área do Círculo: " + forma1.calcularArea());
            System.out.println("Área do Retângulo: " + forma2.calcularArea());
        }
    }
    ```
    - Aqui, `Forma` é uma classe abstrata que define o método `calcularArea()`. As subclasses `Circulo` e `Retangulo` implementam esse método de maneira específica.

- **Quando Usar:**
  - Quando você deseja definir um contrato para subclasses, garantindo que certos métodos sejam implementados.
  - Para modelar conceitos abstratos onde a implementação específica será fornecida pelas subclasses.
  - Ao trabalhar com sistemas complexos, onde a abstração ajuda a gerenciar a complexidade escondendo detalhes não essenciais.

---

**6. Modificadores de Acesso**

- **Teoria:**
  - Os modificadores de acesso em Java controlam a visibilidade de classes, métodos e atributos. Os principais modificadores são `public`, `protected`, `default` (sem modificador) e `private`.

- **Aplicação Prática:**
  - **Modificadores:**
    - `public`: Acesso liberado para todas as classes.
    - `protected`: Acesso dentro do mesmo pacote ou em subclasses.
    - `default` (sem modificador): Acesso apenas dentro do mesmo pacote.
    - `private`: Acesso somente dentro da própria classe.

  - **Exemplo:**
    ```java
    public class Pessoa {
        public String nomePublico;         // Acessível de qualquer lugar
        protected String nomeProtegido;    // Acessível no mesmo pacote ou subclasses
        String nomePadrao;                 // Acessível apenas no mesmo pacote
        private String nomePrivado;        // Acessível apenas dentro da classe

        public void metodoPublico() { }
        protected void metodoProtegido() { }
        void metodoPadrao() { }
        private void metodoPrivado() { }
    }
    ```

  - **Uso em Herança:**
    ```java
    public class Funcionario extends Pessoa {
        public void mostrarNomes() {
            System.out.println(nomePublico);       // Acessível
            System.out.println(nomeProtegido);     // Acessível
            // System.out.println(nomePadrao);     // Acessível apenas se estiver no mesmo pacote
            // System.out.println(nomePrivado);    // Não acessível
        }
    }
    ```

- **Quando Usar:**
  - **`public`**: Quando você deseja que a classe, método ou atributo seja acessível de qualquer lugar.
  - **`protected`**: Quando deseja permitir acesso a subclasses ou classes do mesmo pacote, mas não para o resto do mundo.
  - **`default`**: Quando o acesso deve ser restrito ao pacote, sem expor a membros de outros pacotes.
  - **`private`**: Para encapsular totalmente membros dentro da classe, impedindo acesso externo.
  - O uso adequado dos modificadores de acesso é essencial para o encapsulamento e segurança do código.

---

**7. Interfaces e Classes Abstratas**

- **Teoria:**
  - **Interfaces:**
    - Uma interface em Java é um contrato que especifica um conjunto de métodos que uma classe deve implementar. Interfaces não podem conter implementação de métodos (até Java 7; a partir do Java 8, métodos default e static são permitidos).
  - **Classes Abstratas:**
    - Uma classe abstrata é uma classe que não pode ser instanciada e pode conter métodos abstratos (sem implementação) e métodos concretos (com implementação).

- **Aplicação Prática:**

  - **Exemplo de Interface:**
    ```java
    public interface Voavel {
        void voar();
    }

    public class Passaro implements Voavel {
        @Override
        public void voar() {
            System.out.println("O pássaro está voando.");
        }
    }

    public class Aviao implements Voavel {
        @Override
        public void voar() {
            System.out.println("O avião está voando.");
        }
    }
    ```

  - **Exemplo de Classe Abstrata:**
    ```java
    public abstract class Veiculo {
        public abstract void mover();

        public void parar() {
            System.out.println("O veículo parou.");
        }
    }

    public class Carro extends Veiculo {
        @Override
        public void mover() {
            System.out.println("O carro está se movendo.");
        }
    }
    ```

  - **Diferenças entre Interfaces e Classes Abstratas:**
    - **Interfaces:**
      - Não podem conter estado (atributos de instância) até o Java 7.
      - Uma classe pode implementar múltiplas interfaces.
      - Usadas para definir um contrato que classes diversas podem implementar.
    - **Classes Abstratas:**
      - Podem conter estado (atributos) e métodos com implementação.
      - Uma classe só pode estender uma única classe abstrata.
      - Usadas quando há uma relação mais forte entre classes e uma implementação parcial compartilhada é desejada.

- **Quando Usar:**
  - **Interfaces:**
    - Quando você deseja definir um contrato que pode ser implementado por classes não relacionadas.
    - Para alcançar a flexibilidade de múltiplas heranças de tipo, já que uma classe pode implementar várias interfaces.
    - Quando diferentes classes compartilham comportamentos, mas não uma hierarquia comum.
  - **Classes Abstratas:**
    - Quando há uma relação de herança clara entre classes.
    - Quando você deseja fornecer uma implementação padrão para alguns métodos, deixando outros como abstratos para serem implementados pelas subclasses.
    - Quando você deseja compartilhar código comum entre subclasses.

---

**Dicas Adicionais para a Prova Prática:**

- **Polimorfismo:**
  - Pratique a substituição de métodos (overriding) e entenda como o Java decide qual método chamar em tempo de execução.
  - Compreenda a diferença entre vinculação estática e dinâmica.

- **Abstração:**
  - Esteja preparado para criar classes abstratas e interfaces, entendendo quando cada uma é apropriada.
  - Lembre-se que métodos abstratos não têm corpo e devem ser implementados nas subclasses.

- **Modificadores de Acesso:**
  - Conheça bem o escopo de cada modificador e como ele afeta a visibilidade de classes, métodos e atributos.
  - Entenda como os modificadores interagem com a herança e o encapsulamento.

- **Interfaces e Classes Abstratas:**
  - Pratique a implementação de interfaces e a extensão de classes abstratas.
  - Entenda as limitações e vantagens de cada uma.
  - Saiba que a partir do Java 8, interfaces podem ter métodos default e static com implementação.

**Conselhos Gerais:**

- **Exemplos Práticos:** Codifique exemplos que utilizem polimorfismo, abstração, modificadores de acesso e interfaces/classes abstratas.
- **Leitura e Escrita de Código:** Esteja confortável tanto em ler código que utiliza esses conceitos quanto em escrevê-lo do zero.
- **Aplicação em Problemas Reais:** Pense em cenários práticos onde esses conceitos são aplicáveis e como eles podem melhorar a estrutura e manutenção do código.

Boa sorte na sua prova! Compreender e saber aplicar esses conceitos fundamentais da programação orientada a objetos em Java será essencial para o seu sucesso.
