# 🍫 Calendário de Cursos — Maria Chocolate

> Sistema de agendamento de cursos para instrutores, com painel administrativo de aprovação, notificações por e-mail e armazenamento em nuvem.

---

## 📋 Visão Geral

Este projeto é uma aplicação web completa para gerenciar o agendamento de datas de cursos ministrados por instrutores parceiros da **Maria Chocolate**. O sistema permite que instrutores solicitem datas diretamente pelo calendário, enquanto a equipe administrativa revisa, aprova ou reprova cada solicitação — tudo com notificações automáticas por e-mail.

---

## ✨ Funcionalidades

### Portal do Instrutor (`instrutores.html`)
- Calendário visual com disponibilidade em tempo real
- Seleção de período: **manhã**, **tarde**, **dia inteiro** ou **2 dias consecutivos**
- Regras automáticas por dia da semana (ex: tarde apenas seg–sex)
- Prazo mínimo de **20 dias** para agendamento
- Bloqueio visual de datas já reservadas com indicação de status:
  - 🟠 Laranja — pendente de aprovação
  - 🟢 Verde — confirmado
  - 🔴 Vermelho — data bloqueada pelo admin
- Upload de **2 a 5 fotos** de divulgação com validação de resolução mínima (1080×1080 px)
- Descrição do curso com mínimo de 500 caracteres
- Notificação automática por e-mail ao enviar solicitação

### Painel Administrativo (`admin.html`)
- Acesso protegido por senha
- Listagem de solicitações com filtros por status e categoria
- Filtro por mês para fácil navegação
- Aprovação e reprovação com e-mail automático para o instrutor
- Cancelamento de aprovações com liberação imediata da data
- Galeria de fotos por solicitação com download individual ou em lote
- Calendário visual idêntico ao do instrutor para visão geral do mês
- Bloqueio de datas específicas (feriados, eventos)
- Liberação antecipada de datas dentro do prazo de 20 dias

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Uso |
|---|---|
| **HTML / CSS / JavaScript** | Interface completa sem frameworks |
| **Firebase Firestore** | Banco de dados em tempo real |
| **Cloudinary** | Armazenamento e entrega de imagens |
| **EmailJS** | Envio de e-mails automáticos sem backend |
| **GitHub Pages** | Hospedagem estática gratuita |

---

## 📁 Estrutura do Projeto

```
calendario-cursos-maria-chocolate/
├── instrutores.html     # Portal de agendamento para instrutores
├── admin.html           # Painel administrativo
├── logo.png             # Logomarca Maria Chocolate
└── README.md            # Este arquivo
```

---

## ⚙️ Configuração e Deploy

### Pré-requisitos
- Conta no [Firebase](https://console.firebase.google.com)
- Conta no [Cloudinary](https://cloudinary.com)
- Conta no [EmailJS](https://emailjs.com)
- Repositório no GitHub com GitHub Pages ativo

### 1. Firebase (Firestore)
1. Crie um projeto no Firebase Console
2. Ative o **Firestore Database** na região `southamerica-east1`
3. Crie as seguintes coleções:
   - `agendamentos`
   - `datas_bloqueadas`
   - `datas_liberadas`
4. Registre um app web e copie as credenciais para `instrutores.html` e `admin.html`

### 2. Cloudinary
1. Crie uma conta no Cloudinary
2. Em **Settings → Upload**, crie um upload preset:
   - **Nome:** `maria_chocolate_unsigned`
   - **Signing mode:** `Unsigned`
   - **Pasta:** `maria-chocolate`
3. Atualize `CLOUDINARY_CLOUD` em `instrutores.html`

### 3. EmailJS
1. Crie uma conta no EmailJS e conecte seu Gmail
2. Crie 2 templates:
   - **Template 1** (`template_XXXXX`) — Notificação de nova solicitação para o admin
   - **Template 2** (`template_XXXXX`) — Resultado (aprovação/reprovação) para o instrutor
3. Atualize `service_id`, `template_id` e `public_key` nos arquivos HTML

### 4. GitHub Pages
1. Suba os arquivos `instrutores.html`, `admin.html` e `logo.png` no repositório
2. Vá em **Settings → Pages**
3. Selecione a branch `main` e pasta `/ (root)`
4. Acesse via:
   - `https://usuario.github.io/calendario-cursos-maria-chocolate/instrutores.html`
   - `https://usuario.github.io/calendario-cursos-maria-chocolate/admin.html`

---

## 🔐 Segurança

- O painel administrativo é protegido por senha com hash **SHA-256** verificado no navegador
- As credenciais de API ficam no lado do cliente — recomenda-se configurar **regras de segurança no Firestore** para restringir escrita/leitura conforme o uso
- O upload preset do Cloudinary é `unsigned` com pasta restrita
- A senha do admin pode ser alterada gerando um novo hash SHA-256 e atualizando a constante `ADMIN_PWD_HASH` no `admin.html`

---

## 📅 Regras de Agendamento

| Período | Horário | Dias disponíveis |
|---|---|---|
| Somente manhã | 09h às 11h30 | Segunda a Sábado |
| Somente tarde | 13h30 às 17h | Segunda a Sexta |
| Dia inteiro | 09h às 17h | Segunda a Sexta |
| 2 dias consecutivos | 09h às 17h | 1º dia: Segunda a Quinta |

> O prazo mínimo para agendamento é de **20 dias** a partir da data atual. O admin pode liberar datas antecipadas de forma excepcional.

---

## 📬 Fluxo de Notificações por E-mail

```
Instrutor envia solicitação
        ↓
Admin recebe e-mail com os dados do curso
        ↓
Admin aprova ou reprova no painel
        ↓
Instrutor recebe e-mail com o resultado
```

---

## 🗂️ Categorias de Cursos

- 🍫 Chocolateria
- 🎂 Confeitaria
- 🥐 Salgados
- 🏪 Fornecedor

---

## 📸 Requisitos para Fotos de Divulgação

- Formato: **JPG ou PNG**
- Resolução mínima: **1080 × 1080 px**
- Quantidade: **mínimo 2, máximo 5 fotos** por solicitação

---

## 🚀 Roadmap Futuro

- [ ] Autenticação de instrutores com login individual
- [ ] Notificações push no navegador
- [ ] Exportação do calendário para Google Calendar
- [ ] Relatórios mensais em PDF
- [ ] Página pública de visualização dos cursos confirmados

---

## 👩‍💼 Sobre

Desenvolvido para **Maria Chocolate** — *Ensinar, criar, celebrar.*

---

*Projeto mantido pela equipe Maria Chocolate.*
