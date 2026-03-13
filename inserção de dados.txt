--Criando ENUM..

CREATE TYPE categoria_produto AS ENUM ('eletrônico','vestimenta','brinquedo','alimento','acessório','cosmético','ferramenta','limpeza','higiene','esportivo','móvel','outros');
CREATE TYPE tipo_pagamento_enum AS ENUM ('crédito','débito','dinheiro','boleto','pix');
CREATE TYPE status_pagamento_enum AS ENUM ('cancelado','pago','pendente');
CREATE TYPE status_pedido_enum AS ENUM ('cancelado','confirmado','em andamento');

-- TABELA CLIENTE

CREATE TABLE cliente(
	id_cliente SERIAL PRIMARY KEY,
	data_nascimento date not null,
	primeiro_nome VARCHAR(15) NOT NULL,
	nome_do_meio CHAR(3),
	sobrenome VARCHAR(20) NOT NULL,
	cpf CHAR(11) NOT NULL,
	bairro VARCHAR(30) NOT NULL,
	estado VARCHAR(20) NOT NULL,
	nacionalidade VARCHAR(20) NOT NULL,
	CONSTRAINT unique_cpf_cliente UNIQUE (cpf)
);

-- TABELA PRODUTO

CREATE TABLE produto(
	id_produto SERIAL PRIMARY KEY,
	nome_produto VARCHAR(15) NOT NULL,
	categoria_infantil BOOLEAN DEFAULT FALSE,
	categoria categoria_produto NOT NULL,
	fabrica VARCHAR(20) NOT NULL,
	qualidade NUMERIC(5,1)
);

-- TABELA PEDIDO

CREATE TABLE pedido(
	id_pedido SERIAL PRIMARY KEY,
	id_cliente INT NOT NULL,
	status_pedido status_pedido_enum NOT NULL default 'em andamento',
	descritivo VARCHAR(255),
	frete FLOAT DEFAULT 10,
	pagamento_boleto BOOLEAN DEFAULT FALSE,
	CONSTRAINT fk_pedido_cliente FOREIGN KEY (id_cliente) REFERENCES cliente(id_cliente)
);

-- TABELA PAGAMENTO

CREATE TABLE pagamento(
	id_pagamento SERIAL PRIMARY KEY,
	id_pedido INT NOT NULL,
	tipo_pagamento tipo_pagamento_enum,
	valor NUMERIC(10,2) NOT NULL,
	status_pagamento status_pagamento_enum,
	CONSTRAINT fk_pagamento_pedido FOREIGN KEY (id_pedido) REFERENCES pedido(id_pedido)
);

-- TABELA ESTOQUE

create table estoque(
	id_produto_estoque SERIAL PRIMARY KEY,
	id_produto int,
	local_estoque varchar(255) NOT NULL,
	quantidade int default 0,
	constraint fk_estoque_produto foreign key (id_produto) references produto(id_produto)
);

--TABELA FORNECEDOR

create table fornecedor(
	id_fornecedor serial primary key,
	cnpj char(15) not null,
	nome_social varchar(255) not null,
	contato char(11) not null,
	constraint unique_fornecedor unique (cnpj)
);

--TABELA VENDEDOR 

create table vendedor(
	id_vendedor serial primary key,
	cnpj char(15) not null,
	nome_social varchar(255) not null,
	bairro VARCHAR(30) NOT NULL,
	estado VARCHAR(20) NOT NULL,
	nacionalidade VARCHAR(20) NOT NULL,
	contato char(11),
	data_nascimento date not null,
	cpf char(11),
	constraint unique_cnpj_vendedor unique (cnpj),
	constraint unique_cpf_vendedor unique (cpf)
);

-- TABELA DE VENDA
create table venda(
	id_venda SERIAL PRIMARY KEY,
	id_produto int,
	quantidade int default 1,
	id_vendedor int,
	constraint fk_venda_vendedor foreign key (id_vendedor) references vendedor(id_vendedor),
	constraint fk_venda_produto foreign key (id_produto) references produto(id_produto)
);