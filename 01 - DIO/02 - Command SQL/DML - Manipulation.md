# Exemplos de Comandos DML - Data Manipulation Language

## 🔍 Comandos de Busca e Filtragem - HAVING | LIKE | BETWEEN | DISTINCT | EXISTS | NOT EXISTS
- **HAVING**: Filtra os resultados após a aplicação das funções de agregação, normalmente usado com GROUP BY.
  - Exibir as categorias de produtos com preço médio superior a 50 e mais de 10 produtos:
    ```sql
      SELECT Categoria, AVG(Preco) AS Preco_Medio
      FROM Produtos
      GROUP BY Categoria
      HAVING AVG(Preco) > 50 AND COUNT(*) > 10;
    ```

- **LIKE**: Realiza buscas com padrões em colunas de texto, utilizando os caracteres coringa:
  - Buscar clientes cujo nome começa com "João":
    ```sql
      -- `%` : Qualquer sequência de caracteres (incluindo nenhum).
      -- `_` : Um único caractere.

      SELECT * FROM Clientes
      WHERE Nome LIKE "João%"; -- Qualquer caracter depois de joão.
    ```

- **BETWEEN**: Filtra registros dentro de um intervalo específico (inclusive os limites).
  - Buscar clientes com ID entre 10 e 20:
    ```sql
      SELECT * FROM Clientes
      WHERE idCliente BETWEEN 10 AND 20;
    ```

- **DISTINCT**: Remove registros duplicados de uma consulta.
  - Buscar clientes com nomes únicos:
    ```sql
      CT Nome FROM Clientes;
    ```

- **EXISTS**: Verifica a existência de registros que atendem a uma condição.
  - Retorna os nomes dos clientes que têm pedidos registrados:
    ```sql
      SELECT Nome
      FROM Clientes c
      WHERE EXISTS (
          SELECT 1
          FROM Pedidos p
          WHERE p.idCliente = c.id
      );
    ```

- **NOT EXISTS**: Verifica a não existência de registros que atendem a uma condição.
  - Retorna os nomes dos clientes que não têm pedidos:
    ```sql
      SELECT Nome
      FROM Clientes c
      WHERE NOT EXISTS (
          SELECT 1
          FROM Pedidos p
          WHERE p.idCliente = c.id
      );
    ```

## 🖥️ Comandos para CRUD - SELECT | INSERT | UPDATE | DELETE | INNER JOIN
- **SELECT**: Recupera dados de uma tabela.
  - Buscar clientes cujo nome começa com "João":
    ```sql
      SELECT * FROM Clientes
      WHERE Nome LIKE 'João%';
    ```  
  - Usando INNER JOIN: Retorna uma lista com o dados do cliente, produto, quantidade comprada e o total gasto em cada pedido.
    ```sql
      SELECT 
          Clientes.Nome AS Cliente,
          Produtos.NomeProduto AS Produto,
          ItensPedido.Quantidade,
          (ItensPedido.Quantidade * Produtos.Preco) AS TotalGasto
      FROM 
          Clientes
      INNER JOIN Pedidos ON Clientes.idCliente = Pedidos.idCliente -- União de Cliente + Pedido.
      INNER JOIN ItensPedido ON Pedidos.idPedido = ItensPedido.idPedido -- União de pedido e itens do pedido.
      INNER JOIN Produtos ON ItensPedido.idProduto = Produtos.idProduto -- União de cada item do pedido ser vinculado ao seus próprios dados.
      ORDER BY 
          Clientes.Nome, Pedidos.DataPedido; -- Ordenado pelo nome do cliente e pela data do pedido.
    ```

- **INSERT**: Insere novos registros em uma tabela.
  - Inserir um novo cliente:
    ```sql
      INSERT INTO Clientes (id, Nome, CPF)
      VALUES (1, 'João Silva', '12345678901');
    ```

- **UPDATE**: Atualiza registros existentes em uma tabela.
  - Atualizar o telefone de um cliente:
    ```sql
      UPDATE Clientes
      SET Telefone = '11999999999'
      WHERE id = 1;
    ```

- **DELETE**: Remove registros de uma tabela.
  - Deletar um cliente específico:
    ```sql
      DELETE FROM Clientes
      WHERE id = 1;
    ```


## 📊 Comandos para Ordenação - ORDER BY | ASC | DESC
- **ORDER BY**: Usado para ordenar os resultados de uma consulta com base em uma ou mais colunas.
  - Exibe os clientes em ordem alfabética pelo nome:
  ```sql
    SELECT * FROM Clientes
    ORDER BY Nome; -- Por padrão já vem na ordem ASC
  ```

- **DESC - Ordenação Descendente**: Ordena os resultados do maior para o menor.
  - Exibe os clientes em ordem decrescente de ID:
  ```sql
    SELECT * FROM Clientes
    ORDER BY idCliente DESC;
  ```

- **Ordenação por Múltiplas Colunas**: Ordena com base em várias colunas.
  - Ordena os clientes pelo nome em ordem alfabética e idade por ordem decrescente:
  ```sql
    SELECT nome, idade, cidade
    FROM pessoas
    ORDER BY cidade ASC, idade DESC;
  ```

- **Usando Alias na Ordenação**: Ordena com base em aliases (nomes temporários) definidos na consulta.
  - É possível ordenar utilizando aliases definidos na consulta:
  ```sql
    SELECT Nome AS Cliente, idCliente AS Identificador
    FROM Clientes
    ORDER BY Identificador DESC;
  ```

## ➕ Funções de Agregação - GROUP BY | COUNT | SUM | MIN | MAX | AVG
- **GROUP BY**: Agrupa os registros com base em uma ou mais colunas e permite a aplicação de funções agregadas sobre os grupos.
  - Agrupar os clientes por cidade e contar quantos clientes há em cada cidade:
    ```sql
      SELECT 
          idProduto, 
          SUM(Quantidade) AS Total_Quantidades, -- Calcula a soma total de produtos vendidos por idProduto.
          SUM(Quantidade * Preco) AS Total_Vendas, -- Calcula o valor total das vendas por produto.
          AVG(Quantidade) AS Media_Quantidades -- Calcula a média de unidades vendidas por transação para cada produto.
      FROM 
          Vendas
      GROUP BY 
          idProduto -- Agrupa as vendas, para que as funções de agregação sejam aplicadas em cada grupo de produto.
      ORDER BY 
          Total_Vendas DESC; -- Ordena os resultados pela coluna Total_Vendas de forma decrescente.
    ```
- **COUNT**: Retorna a contagem de registros em uma consulta.
  - Contar o total de clientes na tabela:
    ```sql
      SELECT COUNT(*) AS Total_Clientes
      FROM Clientes;
    ```
  - Contar apenas clientes com pedidos:
    ```sql
      SELECT COUNT(DISTINCT idCliente) AS Clientes_Com_Pedidos
      FROM Pedidos;
    ```

- **SUM**: Calcula a soma dos valores em uma coluna numérica.
  - Somar o total de vendas realizadas:
    ```sql
      SELECT SUM(Valor) AS Total_Vendas
      FROM Pedidos;
    ```

- **MIN**: Retorna o menor valor em uma coluna.
  - Encontrar o menor salário entre os funcionários:
    ```sql
      SELECT MIN(Salario) AS Menor_Salario
      FROM Funcionarios;
    ```

- **MAX**: Retorna o maior valor em uma coluna.
  - Encontrar o maior salário entre os funcionários:
    ```sql
      SELECT MAX(Salario) AS Maior_Salario
      FROM Funcionarios;
    ```

- **AVG**: Calcula a média dos valores em uma coluna numérica.
  - Calcular a média salarial dos funcionários:
    ```sql
      SELECT AVG(Salario) AS Media_Salarial
      FROM Funcionarios;
    ```

## 🔤 Funções de String - CONCAT | UPPER | LOWER | SUBSTR | LENGTH
- **CONCAT**: Concatena (une) strings.
  - Concatenar o nome e o sobrenome dos clientes:
    ```sql
      SELECT CONCAT(Nome, ' ', Sobrenome) AS Nome_Completo FROM Clientes;
    ```

- **UPPER**: Converte o texto para letras maiúsculas.
  - Converter o nome dos clientes para maiúsculas:
    ```sql
      SELECT UPPER(Nome) FROM Clientes;
    ```

- **LOWER**: Converte o texto para letras minúsculas.
  - Converter o nome dos clientes para minúsculas:
    ```sql
      SELECT LOWER(Nome) FROM Clientes;
    ```

- **SUBSTRING** ou **SUBSTR**: Extrai parte de uma string.
  - Extrair os primeiros 4 caracteres do nome:
    ```sql
      SELECT SUBSTRING(Nome, 1, 4) AS Inicio_Nome FROM Clientes;
    ```

- **LENGTH** ou **CHAR_LENGTH**: Retorna o comprimento de uma string.
  - Obter o comprimento do nome dos clientes:
    ```sql
      SELECT LENGTH(Nome) FROM Clientes;
    ```

## 🔢 Funções Numéricas - ROUND | FLOOR | CEIL | ABS | MOD
- **ROUND**: Arredonda valores.
  - Arredondar o salário para 2 casas decimais:
    ```sql
      SELECT ROUND(Salario, 2) AS Salario_Arredondado FROM Funcionarios;
    ```

- **FLOOR**: Arredonda para o menor inteiro.
  - Arredondar o valor 123.45 para o menor inteiro:
    ```sql
      SELECT FLOOR(123.45) AS Valor_Arredondado;
    ```

- **CEIL** ou **CEILING**: Arredonda para o maior inteiro.
  - Arredondar o valor 123.45 para o maior inteiro:
    ```sql
      SELECT CEIL(123.45) AS Valor_Arredondado;
    ```

- **ABS**: Retorna o valor absoluto.
  - Retornar o valor absoluto de -100:
    ```sql
      SELECT ABS(-100) AS Valor_Absoluto;
    ```

- **MOD**: Retorna o resto da divisão.
  - Calcular o resto da divisão de 10 por 3:
    ```sql
      SELECT MOD(10, 3) AS Resto;
    ```

## 🕒 Funções de Data e Hora - NOW | CURDATE | DATEDIFF | DATE_FORMAT | YEAR, MONTH, DAY
- **NOW**: Retorna a data e hora atual.
  - Obter a data e hora atual:
    ```sql
      SELECT NOW() AS DataHora_Atual;
    ```

- **CURDATE**: Retorna a data atual.
  - Obter a data atual:
    ```sql
      SELECT CURDATE() AS Data_Atual;
    ```

- **DATEDIFF**: Calcula a diferença entre duas datas.
  - Calcular a diferença em dias entre 31 de dezembro de 2025 e 1º de janeiro de 2025:
    ```sql
      SELECT DATEDIFF('2025-12-31', '2025-01-01') AS Diferenca_Dias;
    ```

- **DATE_FORMAT**: Formata uma data em um formato específico.
  - Formatar a data atual para o formato `dd/mm/yyyy`:
    ```sql
      SELECT DATE_FORMAT(NOW(), '%d/%m/%Y') AS Data_Formatada;
    ```

- **YEAR, MONTH, DAY**: Extrai partes da data.
  - Extrair o ano da data de nascimento dos clientes:
    ```sql
      SELECT YEAR(DataNascimento) AS Ano FROM Clientes;
    ```

## 🔀 Funções Condicionais - CASE | IF
- **CASE**: Cria condições dentro de consultas.
  - Classificar o salário dos funcionários como "Alto" ou "Baixo":
    ```sql
      SELECT 
        Nome,
        CASE 
          WHEN Salario > 5000 THEN 'Alto'
          ELSE 'Baixo'
        END AS Categoria_Salarial
      FROM Funcionarios;
    ```

- **IF**: Verifica uma condição (dependendo do banco de dados, como MySQL).
  - Classificar o salário dos funcionários como "Alto" ou "Baixo" usando a função IF:
    ```sql
      SELECT IF(Salario > 5000, 'Alto', 'Baixo') AS Categoria_Salarial
      FROM Funcionarios;
    ```
