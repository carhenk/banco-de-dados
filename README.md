# banco-de-dados
CREATE DATABASE PetFelizDB


Como criar o banco (CREATE DATABASE PetFelizDB;).

Como rodar os scripts (source script.sql no Workbench ou PGAdmin).

-- Inserindo Clientes
INSERT INTO Cliente (id_cliente, nome, cpf, telefone, email, endereco)
VALUES (1, 'Maria Silva', '12345678901', '85999999999', 'maria@email.com', 'Rua das Flores, 123');

INSERT INTO Cliente (id_cliente, nome, cpf, telefone, email, endereco)
VALUES (2, 'João Souza', '98765432100', '85988888888', 'joao@email.com', 'Av. Central, 456');

-- Inserindo Animais
INSERT INTO Animal (id_animal, nome, especie, raca, sexo, data_nascimento, peso_atual, fk_cliente)
VALUES (1, 'Rex', 'Cachorro', 'Labrador', 'M', '2020-05-10', 30.5, 1);

INSERT INTO Animal (id_animal, nome, especie, raca, sexo, data_nascimento, peso_atual, fk_cliente)
VALUES (2, 'Mimi', 'Gato', 'Persa', 'F', '2021-03-15', 4.2, 2);

-- Inserindo Veterinários
INSERT INTO Veterinario (id_veterinario, nome, crmv, especialidade, telefone, email)
VALUES (1, 'Dr. Carlos Mendes', 'CRMV1234', 'Clínico Geral', '85977777777', 'carlos@vet.com');

-- Inserindo Produtos
INSERT INTO Produto (id_produto, nome, tipo_produto, forma_farmaceutica, fk_fabricante)
VALUES (1, 'Vacina Antirrábica', 'Medicamento', 'Injetável', NULL);

INSERT INTO Produto (id_produto, nome, tipo_produto, forma_farmaceutica, fk_fabricante)
VALUES (2, 'Ração Premium', 'Alimento', 'Granulado', NULL);

-- 1. Listar todos os clientes em ordem alfabética
SELECT nome, cpf, telefone FROM Cliente
ORDER BY nome ASC;

-- 2. Consultar animais de um cliente específico
SELECT a.nome AS Animal, a.especie, a.raca
FROM Animal a
JOIN Cliente c ON a.fk_cliente = c.id_cliente
WHERE c.nome = 'Maria Silva';

-- 3. Listar consultas realizadas por um veterinário
SELECT c.id_consulta, c.data_consulta, a.nome AS Animal
FROM Consulta c
JOIN Animal a ON c.fk_animal = a.id_animal
WHERE c.fk_veterinario = 1
ORDER BY c.data_consulta DESC
LIMIT 5;

-- 4. Estoque de produtos com validade próxima
SELECT nome, quantidade, validade
FROM Produto p
JOIN Estoque e ON p.id_produto = e.fk_produto
WHERE e.validade < '2026-01-01';


-- 1. Atualizar telefone de um cliente
UPDATE Cliente
SET telefone = '85966666666'
WHERE id_cliente = 1;

-- 2. Atualizar peso de um animal após consulta
UPDATE Animal
SET peso_atual = 32.0
WHERE id_animal = 1;

-- 3. Atualizar status de uma consulta
UPDATE Consulta
SET status = 'Realizada'
WHERE id_consulta = 10;


-- 1. Excluir um exame específico
DELETE FROM Exame
WHERE id_exame = 5;

-- 2. Excluir um produto sem estoque
DELETE FROM Produto
WHERE id_produto = 2
AND id_produto NOT IN (SELECT fk_produto FROM Estoque);

-- 3. Excluir um cliente e todos os seus animais (ON DELETE CASCADE)
DELETE FROM Cliente
WHERE id_cliente = 2;
