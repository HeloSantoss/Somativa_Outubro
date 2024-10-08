Controle de Cadastro de Livros

-- Criação do banco de dados
CREATE DATABASE gerenciador_livros;

-- Seleciona o banco de dados criado
\c gerenciador_livros;

-- Criação da tabela de usuários
CREATE TABLE usuarios (
    id SERIAL PRIMARY KEY, -- Chave primária
    username VARCHAR(50) NOT NULL UNIQUE, -- Nome de usuário único
    senha VARCHAR(255) NOT NULL, -- Senha do usuário
    email VARCHAR(100) NOT NULL UNIQUE, -- Email único do usuário
    criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP -- Data de criação
);

-- Criação da tabela de livros
CREATE TABLE livros (
    id SERIAL PRIMARY KEY, -- Chave primária
    titulo VARCHAR(100) NOT NULL, -- Título do livro
    autor VARCHAR(100) NOT NULL, -- Autor do livro
    ano_publicacao INT NOT NULL CHECK (ano_publicacao > 0), -- Ano de publicação
    genero VARCHAR(50), -- Gênero do livro
    usuario_id INT REFERENCES usuarios(id) ON DELETE CASCADE, -- Chave estrangeira para a tabela de usuários
    criado_em TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- Data de criação
    imagem BYTEA -- Coluna para armazenar a imagem do livro
);

-- Criação de índices para melhorar o desempenho das consultas
CREATE INDEX idx_titulo ON livros(titulo); -- Índice no título do livro
CREATE INDEX idx_autor ON livros(autor); -- Índice no autor do livro
CREATE INDEX idx_genero ON livros(genero); -- Índice no gênero do livro

-- (Opcional) Criação de uma tabela para registrar o histórico de empréstimos (se necessário)
CREATE TABLE emprestimos (
    id SERIAL PRIMARY KEY, -- Chave primária
    livro_id INT REFERENCES livros(id) ON DELETE CASCADE, -- Chave estrangeira para a tabela de livros
    usuario_id INT REFERENCES usuarios(id) ON DELETE CASCADE, -- Chave estrangeira para a tabela de usuários
    data_emprestimo TIMESTAMP DEFAULT CURRENT_TIMESTAMP, -- Data do empréstimo
    data_devolucao TIMESTAMP, -- Data de devolução
    CONSTRAINT uq_emprestimos UNIQUE (livro_id, usuario_id, data_emprestimo) -- Garantir que um livro não seja emprestado ao mesmo usuário mais de uma vez na mesma data
);
