# 🍽 MesaExpress

## 📌 Sobre o Projeto

O **MesaExpress** é um sistema permite que clientes realizem pedidos em restaurantes cadastrados, escolham formas de pagamento, recebam notificações e acompanhem o processo de entrega.
Este projeto foi desenvolvido como atividade acadêmica para demonstrar conceitos de **engenharia de software, modelagem UML e boas práticas de arquitetura de sistemas**.

---

# 🎯 Objetivos do Sistema

O sistema **MesaExpress** tem como objetivo simular o funcionamento básico de uma plataforma de delivery, permitindo:

- Cadastro e autenticação de clientes
- Cadastro de restaurantes
- Visualização de cardápio
- Criação e gerenciamento de pedidos
- Processamento de pagamentos
- Designação de entregadores
- Rastreamento de entregas
- Envio de notificações aos usuários

---
## 📊 Diagrama de Classes

<p align="center">
  <img src="DiagramaDeClasse/Diagrama-MesaExpress.png" width="900">
</p>

---

# 🧩 Estrutura do Sistema

### 👤 Cliente

Representa o usuário da plataforma que realiza pedidos.

**Atributos**

- id: String  
- nome: String  
- email: String  
- endereco: String  

**Métodos**

- autenticar(): Boolean

---

### 🍽 Restaurante

Representa os restaurantes cadastrados no sistema.

**Atributos**

- id: String  
- nome: String  
- endereco: String  
- cardapio: List<ItemCardapio>

**Métodos**

- adicionarItemCardapio(item: ItemCardapio)

---

### 🍔 ItemCardapio

Representa um produto disponível no cardápio de um restaurante.

**Atributos**

- id: String  
- nome: String  
- descricao: String  
- preco: Double

---

### 📦 Pedido

Classe central do sistema responsável por gerenciar os pedidos realizados pelos clientes.

**Atributos**

- id: String  
- cliente: Cliente  
- restaurante: Restaurante  
- itens: List<ItemCardapio>  
- status: String  
- total: Double  

**Métodos**

- criarPedido()
- atualizarStatus(status: String)
- processarPagamento(service: IPagamentoService)
- enviarNotificacao(service: INotificacaoService)
- designarEntrega(service: IEntregaService)

---

### 🛵 Entregador

Representa o profissional responsável pela entrega do pedido.

**Atributos**

- id: String  
- nome: String  
- veiculo: String  
- status: String  

**Métodos**

- atualizarStatus(status: String)

---

# 🔌 Interfaces de Serviço

Para garantir **baixo acoplamento e alta flexibilidade**, o sistema utiliza interfaces para definir comportamentos que podem possuir múltiplas implementações.

---

### 💳 Pagamento

### Interface

**IPagamentoService**

**Método**

- processarPagamento(valor: Double): Boolean

### Implementações

- CartaoCreditoService
- PayPalService
- PixService

Essas implementações permitem que novos métodos de pagamento sejam adicionados sem alterar o funcionamento do sistema.

---

### 🔔 Notificação

### Interface

**INotificacaoService**

**Método**

- enviarNotificacao(destinatario: String, mensagem: String): Boolean

### Implementações

- EmailService
- SMSService
- PushNotificationService

Esse mecanismo permite enviar notificações ao cliente sobre o status do pedido.

---

### 🚚 Entrega

### Interface

**IEntregaService**

**Métodos**

- designarEntrega(pedido: Pedido, entregador: Entregador): Boolean
- rastrearEntrega(pedido: Pedido): String

### Implementações

- EntregaPropriaService
- EntregaTerceirizadaService

Isso permite que o sistema trabalhe tanto com entregadores próprios quanto com serviços terceirizados.

---

# 🗄 Persistência de Dados

O sistema utiliza o padrão **Repository** para gerenciar o acesso aos dados de restaurantes.

### Interface

**IRestauranteRepository**

**Métodos**

- buscarPorId(id: String): Restaurante
- salvar(restaurante: Restaurante)

### Implementação

- RestauranteDatabaseRepository

Esse padrão permite desacoplar a lógica de negócio da lógica de acesso a dados.

---

# 🧠 Aplicação dos Princípios SOLID

O projeto **MesaExpress** foi desenvolvido aplicando os princípios **SOLID**, que promovem boas práticas de design em sistemas orientados a objetos.

---

## S — Single Responsibility Principle

Cada classe possui uma única responsabilidade dentro do sistema.

Exemplos:

- Pedido → gerenciamento de pedidos
- PixService → processamento de pagamentos via Pix
- EmailService → envio de notificações por email

---

## O — Open/Closed Principle

O sistema é aberto para extensão, mas fechado para modificação.

Exemplo: novos métodos de pagamento podem ser adicionados implementando `IPagamentoService` sem alterar o código existente.

---

## L — Liskov Substitution Principle

Qualquer implementação de uma interface pode substituir outra sem alterar o comportamento esperado do sistema.

Exemplo:

- PixService
- PayPalService
- CartaoCreditoService

Todas podem ser utilizadas como `IPagamentoService`.

---

## I — Interface Segregation Principle

As interfaces são específicas para cada funcionalidade, evitando dependências desnecessárias.

Exemplo:

- IPagamentoService
- IEntregaService
- INotificacaoService

---

## D — Dependency Inversion Principle

As classes de alto nível dependem de **abstrações (interfaces)** e não de implementações concretas.

Exemplo:
