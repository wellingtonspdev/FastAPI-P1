# Sistema de Gerenciamento de Produtos e Usuários - Relatório Técnico

Este documento apresenta o relatório técnico do desenvolvimento do Sistema de Gerenciamento de Produtos e Usuários, uma aplicação web construída seguindo o padrão de arquitetura MVC (Model-View-Controller).

## Sumário

1. [Visão Geral](#visão-geral)
2. [Arquitetura MVC](#arquitetura-mvc)
3. [Tecnologias Utilizadas](#tecnologias-utilizadas)
4. [Implementação](#implementação)
5. [Validação de Campos](#validação-de-campos)
6. [Desafios e Soluções](#desafios-e-soluções)
7. [Como Executar](#como-executar)
8. [Endpoints da API](#endpoints-da-api)
9. [Referências](#referências)

## Visão Geral

O Sistema de Gerenciamento de Produtos e Usuários é uma aplicação web desenvolvida para gerenciar o cadastro, visualização, edição e remoção de produtos e usuários. A aplicação oferece uma API RESTful com endpoints bem definidos, além de uma interface de usuário para interação com o sistema.

## Arquitetura MVC

A aplicação foi desenvolvida seguindo o padrão de arquitetura MVC (Model-View-Controller), que separa a aplicação em três componentes principais:

### Model

Os modelos representam a estrutura de dados da aplicação e a lógica de negócios. Eles são responsáveis por:
- Definir a estrutura das entidades (produtos e usuários)
- Interagir com o banco de dados
- Implementar regras de negócio específicas

Exemplo conceitual de implementação de um modelo:

```python
class Produto:
    def __init__(self, id, nome, descricao, preco, quantidade):
        self.id = id
        self.nome = nome
        self.descricao = descricao
        self.preco = preco
        self.quantidade = quantidade
    
    @staticmethod
    def buscar_todos(db):
        # Lógica para buscar todos os produtos no banco de dados
        pass
        
    @staticmethod
    def buscar_por_id(db, id):
        # Lógica para buscar um produto específico
        pass
        
    def salvar(self, db):
        # Lógica para salvar o produto no banco de dados
        pass
```

### View

As views são responsáveis pela apresentação dos dados ao usuário. No nosso sistema, utilizamos:
- Templates HTML para renderizar páginas
- Formulários para entrada de dados
- Elementos de interface para exibição de informações

### Controller

Os controllers gerenciam o fluxo da aplicação, processando requisições, interagindo com os modelos e retornando respostas:

```python
# Exemplo conceitual de um controller
def listar_produtos():
    produtos = Produto.buscar_todos(db)
    return render_template('produtos/lista.html', produtos=produtos)

def cadastrar_produto():
    if request.method == 'POST':
        # Processar dados do formulário
        produto = Produto(None, nome, descricao, preco, quantidade)
        produto.salvar(db)
        return redirect('/produtos')
    return render_template('produtos/cadastrar.html')
```

## Tecnologias Utilizadas

- **Backend**: Framework web para processamento de requisições e respostas
- **ORM**: Mapeamento objeto-relacional para interação com o banco de dados
- **Banco de Dados**: Sistema de gerenciamento de banco de dados relacional
- **Frontend**: HTML, CSS e JavaScript para interface do usuário
- **Validação**: Mecanismos para validação de dados de entrada

## Implementação

### Estrutura do Projeto

```
sistema-gerenciamento/
├── config.py                # Configurações da aplicação
├── run.py                   # Ponto de entrada da aplicação
├── database_setup.sql       # Script de criação do banco de dados
├── requirements.txt         # Dependências do projeto
├── models/                  # Definição dos modelos
│   ├── produto.py
│   └── usuario.py
├── controllers/             # Controladores
│   ├── produto_controller.py
│   └── usuario_controller.py
├── views/                   # Templates e arquivos de interface
│   ├── produtos/
│   └── usuarios/
└── static/                  # Arquivos estáticos (CSS, JS, imagens)
    ├── css/
    ├── js/
    └── img/
```

### Fluxo da Aplicação

1. O usuário acessa uma URL
2. O router direciona a requisição para o controller apropriado
3. O controller processa a requisição e interage com os modelos necessários
4. Os dados são validados antes de serem processados
5. O resultado é renderizado em um template ou retornado como resposta

## Validação de Campos

A validação de campos é um aspecto crucial da aplicação, garantindo que apenas dados válidos sejam processados:

- **Validação de Tipos**: Garantir que os dados estejam no formato correto
- **Validação de Restrições**: Verificar comprimentos mínimos/máximos, valores permitidos
- **Validação de Negócio**: Aplicar regras específicas do domínio

Exemplo de validação:

```python
def validar_produto(nome, preco, quantidade):
    erros = []
    
    if not nome or len(nome) < 3:
        erros.append("Nome deve ter pelo menos 3 caracteres")
    
    if not preco or preco <= 0:
        erros.append("Preço deve ser maior que zero")
    
    if quantidade is None or quantidade < 0:
        erros.append("Quantidade não pode ser negativa")
    
    return erros
```

## Desafios e Soluções

### Persistência de Dados

**Desafio**: Implementar um sistema eficiente de acesso ao banco de dados.

**Solução**: Utilização de um ORM (Object-Relational Mapping) para abstrair a complexidade do acesso ao banco de dados, permitindo manipular registros como objetos Python.

### Segurança

**Desafio**: Proteger a aplicação contra vulnerabilidades comuns.

**Solução**: Implementação de validação rigorosa de entrada, sanitização de dados e proteção contra ataques como SQL Injection e Cross-Site Scripting (XSS).

### Experiência do Usuário

**Desafio**: Criar uma interface intuitiva e responsiva.

**Solução**: Desenvolvimento de uma interface limpa com feedback claro para ações do usuário, validação em tempo real e mensagens de erro informativas.

### Escalabilidade

**Desafio**: Projetar o sistema para crescer com o aumento de usuários e dados.

**Solução**: Adoção de práticas como paginação de resultados, otimização de consultas ao banco de dados e cache de dados frequentemente acessados.

## Como Executar

1. Configure o banco de dados no arquivo `config.py`
2. Execute o script SQL: `database_setup.sql`
3. Instale as dependências: `pip install -r requirements.txt`
4. Execute a aplicação: `python run.py`
5. Acesse a aplicação em `http://localhost:5000`

## Endpoints da API

### Produtos
- GET `/produtos` - Lista todos os produtos
- POST `/produtos/cadastrar` - Cria novo produto
- GET `/produtos/{id}` - Mostra um produto
- POST `/produtos/{id}/editar` - Atualiza produto
- POST `/produtos/{id}` - Deleta produto

### Usuários
- GET `/usuarios` - Lista todos os usuários
- POST `/usuarios/cadastrar` - Cria novo usuário
- GET `/usuarios/{id}` - Mostra um usuário
- POST `/usuarios/{id}/editar` - Atualiza usuário
- POST `/usuarios/{id}` - Deleta usuário

## Referências

- [Padrão MVC (Model-View-Controller)](https://developer.mozilla.org/en-US/docs/Glossary/MVC)
- [Princípios de Design RESTful](https://restfulapi.net/)
- [Boas Práticas de Segurança Web](https://owasp.org/www-project-top-ten/)
- [Otimização de Desempenho Web](https://web.dev/performance-optimizing-content-efficiency/)
- [Princípios de Usabilidade](https://www.nngroup.com/articles/ten-usability-heuristics/)
