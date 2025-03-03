# 🛠️ Procedure no Banco de Dados

## 📖 O que é uma Procedure?
Uma **procedure** (ou **stored procedure**) é um bloco de código SQL armazenado no banco de dados que pode ser executado para realizar tarefas específicas, como manipular dados, executar cálculos ou aplicar regras de negócio.

É semelhante a uma função em linguagens de programação, mas geralmente não retorna valores diretamente (diferente de uma **function**).

## 🎯 Características Principais de uma Procedure
1. **Armazenada no banco**: O código fica armazenado no servidor do banco de dados, o que melhora a eficiência ao evitar a necessidade de envio repetido de instruções SQL do cliente para o servidor.
2. **Reutilizável**: Procedures podem ser executadas quantas vezes forem necessárias, evitando repetição de código e promovendo a centralização de lógica de negócio.
3. **Aceita parâmetros**: Procedures podem receber valores de entrada (**input parameters**) e, em alguns casos, retornar valores por meio de parâmetros de saída (**output parameters**).
4. **Controle de lógica**: Suporta estruturas de controle como condicionais (`IF`, `CASE`) e loops (`WHILE`, `FOR`), tornando-as poderosas para tarefas complexas.

## 📝 Exemplo: Criação de Procedures
### 1️⃣ Procedure Simples
   ```sql
      CREATE PROCEDURE InserirCliente (IN nome_cliente VARCHAR(100), IN email_cliente VARCHAR(100))
      BEGIN
         INSERT INTO Clientes (nome, email)
         VALUES (nome_cliente, email_cliente);
      END;
   ```

### Explicação:
- Essa procedure insere um novo cliente na tabela Clientes.
- Recebe dois parâmetros de entrada: nome_cliente e email_cliente.

### 2️⃣ Exemplo: Procedure para Calcular Total de Vendas
   ```sql
      CREATE PROCEDURE CalcularTotalVendas (IN id_cliente INT, OUT total_vendas DECIMAL(10, 2))  -- Recebe e Envia
      BEGIN  
         SELECT SUM(valor) INTO total_vendas  
         FROM Vendas  
         WHERE cliente_id = id_cliente;  
      END
   ```

### Explicação:
- Essa **procedure** calcula o total de vendas de um cliente específico.
- **Parâmetros**:
  - `id_cliente`: Parâmetro de entrada para identificar o cliente.
  - `total_vendas`: Parâmetro de saída que retorna o resultado calculado.

## 🚀 Comandos para Gerenciar Procedure
> ### 1️⃣ Procedure sem retorno
   ```sql
      CALL InserirCliente('João Silva', 'joao@email.com')
   ```

> ### 2️⃣ Procedure com parâmetro de saída
   ```sql
      SET @resultado = 0  
      CALL CalcularTotalVendas(1, @resultado)  
      SELECT @resultado AS TotalDeVendas  
   ```

## 🔥 Vantagens do Uso de Procedures
1. **Desempenho**: Como o código SQL é pré-compilado e armazenado, a execução é mais rápida do que enviar instruções SQL repetidamente do cliente.
2. **Segurança**: Usuários podem ter permissão para executar procedures sem precisar acessar diretamente as tabelas, protegendo os dados.
3. **Manutenção Simplificada**: Alterações no código são feitas no banco de dados, sem necessidade de atualizar o lado cliente.
4. **Redução no tráfego de rede**: Ao encapsular várias operações em uma única chamada, reduz a quantidade de comunicação entre o cliente e o servidor.

## 🚫 Limitações das Procedures
- **Complexidade de Manutenção**: Em sistemas grandes, procedures podem se tornar difíceis de gerenciar e depurar, especialmente quando há muitas dependências.
- **Dependência do Banco de Dados**: Procedures são específicas para o SGBD (Sistema de Gerenciamento de Banco de Dados) utilizado, o que reduz a portabilidade do código.
- **Desempenho em Casos Específicos**: Em cenários com alto volume de operações, procedures mal otimizadas podem impactar negativamente o desempenho do banco de dados.
- **Curva de Aprendizado**: Desenvolvedores precisam conhecer a linguagem específica do banco de dados (como PL/pgSQL, PL/SQL ou T-SQL) para criar e manter procedures.

## 📌 Quando Usar Procedures?
- Automatizar tarefas repetitivas, como cálculos financeiros ou relatórios.  
- Aplicar lógica de negócio centralizada e consistente.  
- Realizar operações complexas no banco de dados, como múltiplas consultas ou alterações em massa.  
- Garantir a segurança ao restringir o acesso direto às tabelas.


## 🔄 Alternativas a Procedures
- **Funções (Functions)**:
   - Funções podem retornar valores diretamente e são úteis para cálculos ou transformações de dados.
   - Diferente de procedures, funções podem ser usadas em consultas SQL.
- **Triggers**:
   - Automatizam ações em resposta a eventos específicos (INSERT, UPDATE, DELETE) em tabelas.
   - Úteis para auditoria ou validação de dados em tempo real.
- **Lógica na Aplicação**:
   - Implementar regras de negócio diretamente no código da aplicação, em vez de no banco de dados.
   - Facilita a portabilidade e manutenção, mas pode aumentar o tráfego de rede.
