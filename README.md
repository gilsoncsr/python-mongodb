# 🚀 Python MongoDB - Sistema de Delivery

Um sistema de delivery robusto construído com Python, Flask e MongoDB, seguindo princípios de Clean Architecture e Design Patterns.

## 📋 Índice

- [Visão Geral](#visão-geral)
- [Arquitetura](#arquitetura)
- [Funcionalidades](#funcionalidades)
- [Tecnologias](#tecnologias)
- [Instalação](#instalação)
- [Configuração](#configuração)
- [Uso](#uso)
- [API Endpoints](#api-endpoints)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Testes](#testes)
- [Contribuição](#contribuição)

## 🎯 Visão Geral

Este projeto implementa um sistema de delivery com foco em pedidos, utilizando uma arquitetura limpa e bem estruturada. O sistema permite o registro de pedidos com validação de dados, persistência em MongoDB e tratamento robusto de erros.

## 🏗️ Arquitetura

O projeto segue os princípios da **Clean Architecture** com as seguintes camadas:

- **Presentation Layer**: Rotas HTTP e controladores
- **Application Layer**: Casos de uso (Use Cases)
- **Domain Layer**: Entidades e regras de negócio
- **Infrastructure Layer**: Repositórios e conexão com banco de dados

### Padrões Utilizados

- **Repository Pattern**: Para abstração do acesso a dados
- **Dependency Injection**: Para injeção de dependências
- **Factory Pattern**: Para criação de instâncias
- **Strategy Pattern**: Para tratamento de erros
- **Interface Segregation**: Para contratos bem definidos

## ✨ Funcionalidades

- ✅ Registro de pedidos de delivery
- ✅ Validação de dados de entrada
- ✅ Persistência em MongoDB
- ✅ Tratamento robusto de erros
- ✅ API RESTful
- ✅ Estrutura escalável e testável

## 🛠️ Tecnologias

### Backend

- **Python 3.x**
- **Flask**: Framework web
- **PyMongo**: Driver oficial do MongoDB para Python
- **Cerberus**: Validação de dados

### Banco de Dados

- **MongoDB**: Banco de dados NoSQL

### Desenvolvimento

- **Pytest**: Framework de testes
- **Pylint**: Análise estática de código
- **isort**: Organização de imports

## 📦 Instalação

### Pré-requisitos

- Python 3.8+
- MongoDB 4.4+
- Git

### Passos

1. **Clone o repositório**

```bash
git clone <url-do-repositorio>
cd python-mongodb
```

2. **Crie um ambiente virtual**

```bash
python -m venv venv
source venv/bin/activate  # Linux/Mac
# ou
venv\Scripts\activate     # Windows
```

3. **Instale as dependências**

```bash
pip install -r requirements.txt
```

## ⚙️ Configuração

### MongoDB

1. **Instale e inicie o MongoDB**

```bash
# Ubuntu/Debian
sudo apt-get install mongodb

# macOS
brew install mongodb-community

# Windows
# Baixe e instale do site oficial
```

2. **Configure as credenciais**
   Edite o arquivo `src/models/connection/connection_handler.py`:

```python
self.__connection_string = "mongodb://{}:{}@{}:{}/?authSource=admin".format(
    "seu_usuario",      # Usuário do MongoDB
    "sua_senha",        # Senha do MongoDB
    "localhost",        # Host do MongoDB
    "27017"            # Porta do MongoDB
)
self.__database_name = "nome_do_seu_banco"  # Nome do banco
```

### Variáveis de Ambiente (Opcional)

Crie um arquivo `.env` na raiz do projeto:

```env
MONGODB_USER=admin
MONGODB_PASSWORD=password
MONGODB_HOST=localhost
MONGODB_PORT=27017
MONGODB_DATABASE=rocket_db
FLASK_ENV=development
```

## 🚀 Uso

### Executar o Servidor

```bash
python run.py
```

O servidor estará disponível em: `http://localhost:3000`

### Executar Testes

```bash
pytest
```

### Análise de Código

```bash
pylint src/
```

## 🔌 API Endpoints

### POST `/delivery/order`

Registra um novo pedido de delivery.

**Request Body:**

```json
{
  "data": {
    "name": "João Silva",
    "address": "Rua das Flores, 123 - São Paulo, SP",
    "cupom": 15.5,
    "items": [
      {
        "item": "Pizza Margherita",
        "quantidade": 2
      },
      {
        "item": "Refrigerante Cola",
        "quantidade": 1
      }
    ]
  }
}
```

**Response (Sucesso - 200):**

```json
{
  "data": {
    "type": "Order",
    "count": 1,
    "registry": true
  }
}
```

**Response (Erro de Validação - 422):**

```json
{
  "errors": [
    {
      "title": "HttpUnprocessableEntityError",
      "detail": "Erros de validação específicos"
    }
  ]
}
```

## 📁 Estrutura do Projeto

```
python-mongodb/
├── requirements.txt              # Dependências do projeto
├── run.py                       # Ponto de entrada da aplicação
└── src/                         # Código fonte
    ├── __init__.py
    ├── errors/                  # Tratamento de erros
    │   ├── __init__.py
    │   ├── error_handler.py     # Manipulador central de erros
    │   └── types/               # Tipos de erro específicos
    │       ├── __init__.py
    │       ├── http_not_found.py
    │       └── http_unprocessable_entity.py
    ├── main/                    # Camada de apresentação
    │   ├── __init__.py
    │   ├── composer/            # Composição de dependências
    │   │   ├── __init__.py
    │   │   └── register_order_composer.py
    │   ├── http_types/          # Tipos HTTP
    │   │   ├── __init__.py
    │   │   ├── http_request.py
    │   │   └── http_response.py
    │   ├── routes/              # Rotas da API
    │   │   ├── __init__.py
    │   │   └── delivery_routes.py
    │   └── server/              # Configuração do servidor
    │       ├── __init__.py
    │       └── server.py
    ├── models/                  # Camada de infraestrutura
    │   ├── __init__.py
    │   ├── connection/          # Conexão com banco
    │   │   ├── __init__.py
    │   │   └── connection_handler.py
    │   └── repository/          # Repositórios de dados
    │       ├── __init__.py
    │       ├── interfaces/      # Interfaces dos repositórios
    │       │   ├── __init__.py
    │       │   └── orders_repository.py
    │       ├── orders_repository.py
    │       ├── orders_repository_test.py
    │       └── repository_test.py
    ├── use_cases/               # Casos de uso (regras de negócio)
    │   ├── __init__.py
    │   └── registry_order.py
    └── validators/              # Validação de dados
        ├── __init__.py
        ├── registry_order_validator.py
        └── registry_order_validator_test.py
```

## 🧪 Testes

O projeto inclui testes para garantir a qualidade do código:

- **Testes de Repositório**: `src/models/repository/repository_test.py`
- **Testes de Validação**: `src/validators/registry_order_validator_test.py`
- **Testes de Casos de Uso**: `src/models/repository/orders_repository_test.py`

### Executar Testes Específicos

```bash
# Testes de validação
pytest src/validators/

# Testes de repositório
pytest src/models/repository/

# Todos os testes
pytest
```

## 🔧 Funcionalidades do Repositório

O `OrdersRepository` oferece operações CRUD completas:

- **Inserção**: `insert_document()`, `insert_list_of_documents()`
- **Consulta**: `select_one()`, `select_many()`, `select_by_object_id()`
- **Atualização**: `edit_registry()`, `edit_many_registry()`, `edit_registry_with_increment()`
- **Exclusão**: `delete_registry()`, `delete_many_registries()`
- **Consultas Avançadas**: `select_many_with_properties()`, `select_if_property_exists()`

## 🚨 Tratamento de Erros

O sistema implementa um tratamento robusto de erros:

- **HttpNotFoundError**: Para recursos não encontrados
- **HttpUnprocessableEntityError**: Para dados inválidos
- **Error Handler Centralizado**: Tratamento uniforme de exceções
- **Logging**: Preparado para integração com sistemas de log

## 📈 Melhorias Futuras

- [ ] Implementar autenticação JWT
- [ ] Adicionar cache com Redis
- [ ] Implementar rate limiting
- [ ] Adicionar documentação com Swagger/OpenAPI
- [ ] Implementar testes de integração
- [ ] Adicionar monitoramento e métricas
- [ ] Implementar sistema de notificações
- [ ] Adicionar suporte a múltiplos idiomas

## 🤝 Contribuição

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

### Padrões de Código

- Siga o estilo PEP 8
- Use type hints
- Escreva testes para novas funcionalidades
- Mantenha a cobertura de testes acima de 80%

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

---

⭐ Se este projeto foi útil para você, considere dar uma estrela!
