# Praticando SQL Curso Proz Banco de dados Simples...

Lembre-se, cada desafio é uma chance de crescer. Não se desanime com os erros; eles são degraus no caminho do aprendizado. E acima de tudo, divirta-se! 
O aprendizado mais eficaz acontece quando nos engajamos e nos interessamos pelo que estamos fazendo.

 Vamos criar um banco de dados para uma concessionária de carros. Teremos tabelas para Carros, Fabricantes, Vendas e RelatorioVendas. Além disso, criaremos um trigger que será acionado após uma inserção na tabela Vendas.

#### Criando uma base de banco de dados;
```sql
CREATE DATABASE Concessionaria;

```

#### Criando tabelas na base de dados (Fabricantes);
```sql
CREATE TABLE Fabricantes (
    FabricanteID INT PRIMARY KEY AUTO_INCREMENT,
    Nome VARCHAR(100),
    Pais VARCHAR(50)
);

```
#### Tabela carros
```sql
CREATE TABLE Carros (
    CarroID INT PRIMARY KEY AUTO_INCREMENT,
    Modelo VARCHAR(150),
    FabricanteID INT,
    Preco DECIMAL(10, 2),
    Ano INT,
    FOREIGN KEY (FabricanteID) REFERENCES Fabricantes(FabricanteID)
);
```

#### Tabela (Vendas)
```sql
CREATE TABLE Vendas (
    VendaID INT PRIMARY KEY AUTO_INCREMENT,
    CarroID INT,
    Quantidade INT,
    DataVenda DATE,
    FOREIGN KEY (CarroID) REFERENCES Carros(CarroID)
);
```

#### Tabela (RelatorioVendas)
```sql
CREATE TABLE RelatorioVendas (
    VendaID INT,
    CarroID INT,
    PrecoTotal DECIMAL(10, 2)
);
```

#### Inserindo dados em cada tabela (Fabricante)
```sql
INSERT INTO Fabricantes (Nome, Pais) VALUES
('Toyota', 'Japão'),
('Ford', 'Estados Unidos'),
('Volkswagen', 'Alemanha');
```

#### Inserindo dados em cada tabela (Carros)
```sql
INSERT INTO Carros (Modelo, FabricanteID, Preco, Ano) VALUES
('Corolla', 1, 90000.00, 2022),
('Mustang', 2, 300000.00, 2023),
('Golf', 3, 120000.00, 2021);
```

#### Criando trigger

Vamos criar um trigger que será acionado após uma inserção na tabela Vendas. Esse trigger atualizará o preço total da venda na tabela RelatorioVendas.
```sql
DELIMITER //

CREATE TRIGGER AtualizaRelatorioVendas AFTER INSERT ON Vendas
FOR EACH ROW
BEGIN
    DECLARE Preco DECIMAL(10, 2);
    SELECT Preco INTO Preco FROM Carros WHERE CarroID = NEW.CarroID;
    INSERT INTO RelatorioVendas (VendaID, CarroID, PrecoTotal)
    VALUES (NEW.VendaID, NEW.CarroID, NEW.Quantidade * Preco);
END //

DELIMITER ;

```

#### Adicionando o trigger na tabela Vendas
```sql
INSERT INTO Vendas (CarroID, Quantidade, DataVenda) VALUES
(1, 2, '2024-07-16'),
(2, 1, '2024-07-16');
```

#### Realizando consulta na tabela RelatorioVendas para vermos os resultados do trigger
```sql
SELECT * FROM RelatorioVendas;
```

