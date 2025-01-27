# Normalização de Banco de Dados

A normalização é o processo para evitar redundâncias e melhorar a integridade dos dados. 

A ideia é dividir as informações em tabelas menores e mais eficientes, de forma que cada tabela tenha uma única responsabilidade.

O objetivo é eliminar anomalias de inserção, atualização e exclusão.

A normalização é dividida em formas normais (1FN, 2FN, 3FN, etc.). Cada forma normal resolve um tipo específico de problema de redundância e dependência.

## 1ª Forma Normal (1FN) - Valores únicos dentro da célula.
A 1FN exige que:
- Todos os valores nas colunas sejam atômicos (não podem ter múltiplos valores em uma única célula).
- Cada campo da tabela deve conter apenas um valor (sem listas, arrays ou grupos de dados).

  - **Exemplo: Tabela sem normalização:**
  ```sql
  | id | Nome  | Telefones        |
  |____|_______|__________________| 
  | 1  | João  | 12345, 67890     | -- Dois telefones no mesmo atributo
  | 2  | Maria | 11223            |
  ```
  - **Tabela em 1FN:**
  ```sql
  | id | Nome  | Telefone |
  |____|_______|__________|
  | 1  | João  | 12345    | -- É possível normalizar novamente para retirada de duplicidade.
  | 1  | João  | 67890    |
  | 2  | Maria | 11223    |
  ```

## 2ª Forma Normal (2FN) - Elimina dependências parciais.
A 2FN exige que a tabela esteja em 1FN e que todas as colunas dependam inteiramente da chave primária. Ou seja, se a tabela possui uma chave composta (duas ou mais colunas), todas as colunas devem depender da chave primária.

Eliminando dependências parciais.

  - **Exemplo: Tabela em 1FN, mas não em 2FN:**
    ```sql
    | idCliente | idPedido | NomeCliente | Produto   | -- Tabela Cliente e Produto misturados.
    |___________|__________|_____________|___________|
    | 1         | 101      | João        | Produto A |
    | 1         | 102      | João        | Produto B |
    | 2         | 103      | Maria       | Produto A |
    ```
  - **Tabela em 2FN:**
    - **Tabela de Clientes:**
      ```sql
      | idCliente | NomeCliente | -- Removido as dependências parciais.
      |___________|_____________|
      | 1         | João        |
      | 2         | Maria       |
      ```
    - **Tabela de Pedidos:**
      ```sql
      | idPedido | idCliente | Produto   | -- Tabela pedidos tem apenas informações referente ao pedido.
      |__________|___________|___________|
      | 101      | 1         | Produto A |
      | 102      | 1         | Produto B |
      | 103      | 2         | Produto A |
      ```

## 3ª Forma Normal (3FN) - Elimina dependências transitivas
A 3FN exige que a tabela esteja em 2FN e que não haja dependências transitivas. Ou seja, as colunas não podem depender de outras colunas.

  - **Dependências transitivas**: Uma dependência transitiva ocorre quando uma coluna não-chave depende de outra coluna não-chave. Em outras palavras, se uma coluna A depende de uma coluna B, e a coluna B depende de uma coluna C, então a coluna A depende transitivamente da coluna C. Esse tipo de dependência é eliminado na 3FN.


## '4ª' Forma Normal (BCNF) - Refinamento da 3FN
A Forma Normal de Boyce_Codd (BCNF), resolve problemas relacionados a dependências de junção. Uma tabela está em BCNF quando não pode ser dividida ainda mais sem perder informações.

  - **Exemplo: Tabela não normalizada em BCNF:**
    ```sql
    | Departamento | Gerente | Projeto    |
    |______________|_________|____________|
    | Vendas       | João    | Projeto X  |
    | Vendas       | João    | Projeto Y  |
    | Marketing    | Maria   | Projeto X  |
    ```

    **Exemplo: Tabela normalizada em BCNF**
      ```sql
      | Departamento | Gerente |
      |______________|_________|
      | Vendas       | João    |
      | Marketing    | Maria   |
      ```

      ```sql
      | Departamento | Projeto |
      |______________|_________|
      | Vendas       | P1      |
      | Vendas       | P2      |
      | Marketing    | P3      |
      ```

## Resumo das Formas Normais:
- **1FN**: Elimina dados repetidos e exige valores atômicos (cada célula tem um único valor).
- **2FN**: Elimina dependências parciais em relação à chave primária.
- **3FN**: Elimina dependências transitivas (colunas não-chave não dependem de outras colunas não-chave).
- **4FN**: Elimina dependências de junção e garante que os dados não possam ser divididos ainda mais sem perda de informações.

## Vantagens da Normalização:
- **Redução de Redundâncias**: Minimiza a repetição de dados em tabelas.
- **Melhor Integridade**: Facilita a manutenção e integridade dos dados.
- **Facilidade de Atualização**: Simplifica a atualização dos dados.

## Desvantagens da Normalização:
- **Complexidade**: Pode aumentar a complexidade do banco de dados e consultas, pois são necessárias junções (joins) em várias tabelas.
- **Desempenho**: Em alguns casos, a normalização excessiva pode afetar o desempenho das consultas, especialmente em sistemas com grandes volumes de dados.
- **BCNF**: Em alguns casos, pode ser indesejável aplicar a BCNF, pois pode levar a um número excessivo de tabelas e complicar as consultas.

> Em resumo, a normalização é fundamental para criar um banco de dados eficiente e sem redundâncias, mas deve ser balanceada com o desempenho e a necessidade de consultas rápidas.
