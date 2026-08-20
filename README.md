# projeto-DIO-Final

# 📊 Estudo de Caso e Análise Arquitetural - API de Orçamento com Spring AI

**AVISO IMPORTANTE**
Este é um projeto **puramente acadêmico** criado para exercitar e demonstrar análise, conceitos e técnicas de construção de APIs REST com Java, Spring Boot e Spring AI!

O meu trabalho consiste em uma atividade prática analítica de projeto final no curso da **Digital Innovation One (DIO) - Bootcamp Santander Java IA**. 

🔗 **Link para o repositório original sugerido no curso:** [dio-spring-boot-learning-track](https://github.com/digitalinnovationone/dio-spring-boot-learning-track/tree/main/05-spring-ai)

---

### ⚠️ AVISO IMPORTANTE E ISENÇÃO DE RESPONSABILIDADE

*   **Conteúdo Estritamente Acadêmico e Teórico:** Este repositório não contém arquivos de código-fonte, binários ou softwares executáveis. Trata-se puramente de um estudo de caso descritivo e de uma análise arquitetural baseada no curso "Bootcamp Santander Java IA" da Digital Innovation One (DIO).
*   **Créditos ao Projeto Base:** Todo o escopo de código discutido pertence ao repositório original sugerido no curso, disponível publicamente na plataforma GitHub.
*   **Isenção de Responsabilidade sobre as Sugestões:** As notas técnicas e sugestões de melhorias apresentadas neste documento (como o uso de `BigDecimal` ou `AtomicLong`) são propostas conceituais para fins de aprendizado. O autor não assume qualquer responsabilidade por erros, falhas, bugs ou incompatibilidades caso terceiros decidam aplicar essas ideias em seus próprios códigos ou ambientes de trabalho.



---

O projeto em questão demonstra o uso do desenvolvimento guiado por domínio (DDD) em uma aplicação que se conecta a modelos de Inteligência Artificial (LLM). A API é capaz de receber um arquivo de áudio com comandos de voz e utilizá-lo para gerenciar gastos financeiros.

Com o intuito de me resguardar e também proteger o código do programador orginal, vou apenas descrever algumas pequenas sugestões de melhoria nos arquivos do projeto pois até a data da entrega do projeto, não foi percebido no repositório original sugerido como base um arquivo LICENSE que deixe claro os limites de uso do repositório.

## Sugestões;
1. **Documentação e Comentário - Inserir comentários que documentem melhor classes, métodos e principalmente a annotations;**
  A estrutura do projeto é bem elaborada, no padrão DDD com pastas/packages application, domain e infrastructure contendo os arquivos da solução. A pasta application contem subpastas input e output e a pasta infrastructure contém as pastas http(com pastas request e response) e persitence(com pastas entity e repository), no entanto não há comentários nos aquivos de código .java, nem que expliquem a utilidade das classes nem que expliquem a utilidade dos diversos métodos.
2. **Flexibilidade na Persistência - Descrever o uso de outros SGBDR;** Creio que para facilitar, foi usado banco de dados para persistencia de dados que "sobe" via Docker (há um aquivo .yml na raiz do proejto descrevendo isso).  Embora o ambiente em contêiner simplifique o deploy, sugere-se a criação de um perfil (`profile`) contendo a configuração de um banco em memória como o **H2 Database**. Isso remove a barreira do Docker para estudantes iniciantes.
3. **Uso de gerenciadores de dependências;** O foco do projeto foi uso do Gradle mas uma sugestão de configuração do Maven seria de bom grado, em especial para iniciantes
4. **Expansão do Domínio (`Category` Enum) - Inclusão de outras categorias;** Em domain, o arquivo fonte que descreve categorias poderia contemplar mais categorias:
  4.1. UTILITIES: Para contas fixas da casa, como água, luz, internet e gás.
  4.2. ENTERTAINMENT: Para momentos de lazer, como cinema, shows, festas e serviços de streaming.
  4.2. RESTAURANTS: Para alimentação fora de casa, como bares, lanchonetes, delivery e jantares.
  4.4 TRANSPORT: Para custos de locomoção, como passagens de ônibus, metrô, viagens de aplicativos (Uber) e combustível.
5. **Estratégia de Identificadores (UUID vs Long)**; A arquitetura usa UUID. Uma alternativa viável para controle sequencial local em testes seria o uso de `AtomicLong` para controle seguro de concorrência em memória. O AtomicLong resolve o problema de concorrência local na memória da aplicação JVM. No entanto, para ambientes distribuídos ou múltiplos contêineres rodando em paralelo, o UUID original do projeto ainda se sobressai por evitar colisões de ID sem depender de uma centralização.
```java
// Gerador de IDs automáticos e seguros para concorrência
private final AtomicLong contadorId = new AtomicLong(1);
```
6. **Precisão Financeira (`BigDecimal`):** Por se tratar de uma API de orçamento (budgeting), a substituição de tipos de ponto flutuante (`double`/`float`/`long`) por `BigDecimal` nas entidades de transação é uma evolução crítica para evitar erros de arredondamento monetário.
```java
private BigDecimal amount; // Modificado de double para BigDecimal

// No Construtor padrão
// Definindo escala padrão de 2 casas decimais e arredondamento financeiro
  this.amount = amount != null ? amount.setScale(2, java.math.RoundingMode.HALF_UP) : BigDecimal.ZERO;

// Getters e Setters
    public BigDecimal getAmount() {
        return amount;
    }

    public void setAmount(BigDecimal amount) {
        this.amount = amount.setScale(2, java.math.RoundingMode.HALF_UP);
    }

    // Exemplo de exibição limpa formatada em String sem notação científica
    public String getFormattedAmount() {
        return this.amount.toPlainString();
    }

```

### 📝 Nota Final sobre Propriedade Intelectual e Direitos Autorais

Até a data de publicação deste documento (agosto de 2026), observou-se que o repositório original utilizado como base não disponibilizava um arquivo de licença explícito (como `LICENSE.txt`, `MIT`, ou `Apache 2.0`). 

Em estrito respeito aos direitos autorais da **Digital Innovation One (DIO)** e dos autores originais do código, este repositório adotou as seguintes diretrizes:

1. **Nenhum Código Copiado:** Não foi realizada a cópia, reprodução ou distribuição de nenhum arquivo de código-fonte (`.java`), de configuração ou binário do projeto original.
2. **Ausência de Fork:** Optou-se por não realizar o procedimento de *fork* da ferramenta para evitar a replicação não autorizada de material sem uma licença prévia especificada.
3. **Escopo Estritamente Analítico:** Este trabalho não modifica, não estende e não distribui o software original. Ele se ateve de forma exclusiva à formulação de sugestões conceituais, revisões teóricas e propostas arquiteturais na camada de documentação.

Dessa forma, este espaço atua puramente como um registro público de aprendizado, análise crítica e maturidade de engenharia de software para fins de avaliação acadêmica.


## Agradecimentos
Obrigado Deus por me orientar em tudo o que faço e obrigado à minha família pelo suporte de me suportar.
