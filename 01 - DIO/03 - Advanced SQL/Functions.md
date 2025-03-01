# 🛠️ Functions no Banco de Dados

## 📖 O que é uma Function?
Uma **function** é um bloco de código SQL armazenado no banco de dados que retorna um valor único após a execução.  
Diferente das procedures, as functions são usadas principalmente para cálculos ou consultas que resultam em um valor que pode ser utilizado em uma instrução SQL, como em um `SELECT`.

## 🎯 Características Principais de uma Function

1. **Retorno de Valor**: Sempre retorna um valor único (scalar ou tabelado).

2. **Chamada Direta**: Pode ser usada dentro de um `SELECT`, `WHERE`, ou até como parte de uma expressão.

3. **Aceita Parâmetros**: Pode receber valores de entrada (input parameters).

4. **Imutabilidade**: Uma function **não** pode modificar dados no banco, ou seja, não pode executar comandos como `INSERT`, `UPDATE` ou `DELETE`.

5. **Centralização da Lógica**: Reúne operações comuns ou cálculos frequentes em um local reutilizável.

## 📝 Exemplo: Function para Calcular Total de Vendas
  ```sql
    CREATE FUNCTION CalcularTotalVendasPorCliente (id_cliente INT)  
    RETURNS DECIMAL(10, 2)  
    DETERMINISTIC  
    BEGIN  
        DECLARE total_vendas DECIMAL(10, 2);  
        SELECT SUM(valor) INTO total_vendas  
        FROM Vendas  
        WHERE cliente_id = id_cliente;  
        RETURN total_vendas;  
    END  
  ```
### Explicação:
- **Objetivo**: Calcula o total de vendas de um cliente específico.  
- **Retorno**: Um valor numérico que representa o total de vendas.  
- **Uso**: Essa função pode ser chamada diretamente dentro de consultas SQL.  

## 🚀 Comandos para Gerenciar Function
> ### Function em um SELECT
  ```sql
    SELECT CalcularTotalVendasPorCliente(1) AS TotalDeVendas  
  ```
> ### Function em uma Condição WHERE
  ```sql
    SELECT nome  
    FROM Clientes  
    WHERE CalcularTotalVendasPorCliente(id) > 1000
  ```

## 🔥 Vantagens do Uso de Functions
1. **Reutilização de Código**: Centraliza operações complexas e evita a repetição de lógica.
2. **Integração Simples**: Pode ser usada diretamente em instruções SQL como parte de consultas.
3. **Desempenho**: Funções predefinidas no banco de dados são otimizadas para execução eficiente.
4. **Imutabilidade e Segurança**: Functions são **read-only**, garantindo que dados não sejam alterados diretamente.

## 🚫 Limitações das Functions
- **Imutabilidade**: Como mencionado, functions não podem modificar dados no banco de dados. Isso limita seu uso em cenários onde é necessário alterar dados.
- **Desempenho em Funções Complexas**: Funções que envolvem operações complexas ou grandes volumes de dados podem impactar o desempenho, especialmente se forem chamadas repetidamente em consultas.
- **Dependência de Parâmetros**: Funções dependem dos parâmetros de entrada para funcionar corretamente. Se os parâmetros forem mal definidos, a função pode retornar resultados incorretos.
- **Manutenção**: Funções armazenadas no banco de dados podem se tornar difíceis de manter, especialmente em bancos de dados grandes com muitas funções.


## 📌 Quando Usar Functions?
- Quando você precisa realizar cálculos ou agregações que serão usados em consultas.  
- Quando deseja encapsular lógica complexa para simplificar consultas SQL.  
- Para criar operações que retornam valores específicos baseados em entradas.


## 🔄 Alternativas ao Uso de Functions
- **Stored Procedures**: Para operações que precisam modificar dados ou executar múltiplas operações, stored procedures podem ser uma alternativa mais adequada.
- **Views**: Para consultas complexas que não precisam de parâmetros de entrada, views podem ser uma opção mais simples e eficiente.
- **Código na Aplicação**: Em alguns casos, a lógica pode ser movida para a camada da aplicação, especialmente se a lógica for específica para um determinado contexto de aplicação.
- **Triggers**: Para operações que precisam ser executadas automaticamente em resposta a eventos no banco de dados, triggers podem ser uma alternativa.
- *Uso de Funções de Agregação Nativas**: Em muitos casos, funções de agregação nativas do SQL (como SUM, AVG, COUNT, etc.) podem ser suficientes para realizar cálculos sem a necessidade de criar uma função personalizada.
