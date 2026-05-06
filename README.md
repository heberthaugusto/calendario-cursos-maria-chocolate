# 🍫 Sistema de Agendamento de Cursos — Maria Chocolate

> Aplicação web completa desenvolvida para gerenciar o agendamento de cursos presenciais ministrados por instrutores parceiros da escola de gastronomia **Maria Chocolate**.

[![HTML](https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![CSS](https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/CSS)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![Firebase](https://img.shields.io/badge/Firebase-FFCA28?style=flat&logo=firebase&logoColor=black)](https://firebase.google.com)
[![Cloudinary](https://img.shields.io/badge/Cloudinary-3448C5?style=flat&logo=cloudinary&logoColor=white)](https://cloudinary.com)

---

## 🌐 Demo ao Vivo

| Portal | Link |
|---|---|
| 🗓️ Portal do Instrutor | [instrutores.html](https://heberthaugusto.github.io/portfolio-calendario-agendamento-cursos/instrutores.html) |
| 🔧 Painel Administrativo | [admin.html](https://heberthaugusto.github.io/portfolio-calendario-agendamento-cursos/admin.html) |

> ℹ️ O painel administrativo requer credenciais de acesso — configure suas próprias via Firebase Authentication para testar.

---

## 🎯 Sobre o Projeto

Este sistema foi desenvolvido do zero para resolver um problema real: a escola **Maria Chocolate** precisava de uma forma organizada para que seus instrutores parceiros agendassem datas para ministrar cursos — sem depender de planilhas, WhatsApp ou agendamento manual.

O resultado foi uma aplicação com dois portais distintos: um **calendário interativo para os instrutores** e um **painel administrativo completo** para a equipe da escola.

---

## ✨ Funcionalidades Implementadas

### 🗓️ Portal do Instrutor
- Calendário visual com disponibilidade em tempo real, sincronizado via Firebase
- Fluxo guiado em 4 etapas: data → período → informações → confirmação
- Regras de negócio automáticas:
  - Prazo mínimo de 20 dias para agendamento
  - Limite de 5 cursos por instrutor por mês
  - Segundas-feiras de manhã bloqueadas (reservadas para lives)
  - Dias da semana específicos por período (manhã, tarde, dia inteiro, 2 dias)
- Indicação visual de status por cor: 🟠 pendente · 🟢 confirmado · 🔴 bloqueado
- Upload de 2 a 5 fotos com validação de resolução mínima (1080×1080 px)
- Validação de formulário completa com contador de caracteres (mín. 500)
- Notificação automática por e-mail ao enviar

### 🔧 Painel Administrativo
- Autenticação segura via **Firebase Authentication**
- Aprovação e reprovação com e-mail automático para o instrutor
- Cancelamento de aprovações com liberação imediata da data no calendário
- Filtros por status, categoria e mês
- Exportação de planilha **CSV** com os cursos aprovados do mês
- Galeria de fotos com download individual ou em lote
- Bloqueio e liberação de datas específicas (feriados, eventos) — persistido no Firestore
- Calendário visual idêntico ao do instrutor para visão geral

---

## 🏗️ Arquitetura e Decisões Técnicas

| Decisão | Justificativa |
|---|---|
| **Vanilla JS sem frameworks** | Projeto de escopo definido, sem necessidade de reatividade complexa. Menor bundle, mais rápido. |
| **Firebase Firestore** | Banco de dados em tempo real sem necessidade de backend próprio. Gratuito para o volume de uso. |
| **Firebase Authentication** | Solução segura para proteger o painel admin sem implementar backend de autenticação. |
| **Cloudinary** | Upload direto do browser com URL pública para as fotos, sem servidor intermediário. |
| **EmailJS** | Disparo de e-mails transacionais direto do frontend, sem backend. |
| **GitHub Pages** | Hospedagem estática gratuita, ideal para aplicações sem servidor. |
| **SHA-256 → Firebase Auth** | Migração da autenticação por hash local para autenticação real em nuvem, aumentando a segurança. |

---

## 🔐 Segurança Implementada

- Regras do **Firestore** que garantem escrita restrita a usuários autenticados
- **Firebase Authentication** com e-mail e senha para o painel admin
- Restrição da API Key do Firebase ao domínio de produção
- Upload preset **unsigned** do Cloudinary com pasta e tipo de arquivo restritos
- Sem exposição de secrets no frontend — apenas chaves públicas necessárias

---

## 📐 Regras de Negócio

```
Manhã:           09h às 12h30  |  Terça a Sábado
Tarde:           13h30 às 17h  |  Segunda a Sexta
Dia inteiro:     09h às 17h    |  Segunda a Sexta
2 dias seguidos: 09h às 17h    |  1º dia: Segunda a Quinta

Prazo mínimo:    20 dias de antecedência
Limite mensal:   5 cursos por instrutor
Aprovação:       até 5 dias úteis
Fotos:           2 a 5 · JPG ou PNG · mín. 1080×1080 px
```

---

## 🛠️ Stack Tecnológica

```
Frontend:        HTML5 · CSS3 · JavaScript (ES Modules)
Banco de dados:  Firebase Firestore
Autenticação:    Firebase Authentication
Storage:         Cloudinary
E-mail:          EmailJS
Hospedagem:      GitHub Pages
```

---

## ⚙️ Como Rodar Localmente

1. Clone o repositório:
```bash
git clone https://github.com/heberthaugusto/portfolio-calendario-agendamento-cursos.git
```

2. Configure suas credenciais nos arquivos HTML substituindo os placeholders:
```javascript
// instrutores.html e admin.html
const firebaseConfig = {
  apiKey: "SUA_FIREBASE_API_KEY",
  authDomain: "SEU_PROJETO.firebaseapp.com",
  projectId: "SEU_PROJETO_ID",
  ...
};
const CLOUDINARY_CLOUD = 'SEU_CLOUDINARY_CLOUD_NAME';
```

3. Configure o Firebase:
   - Ative o **Firestore** e crie as coleções: `agendamentos`, `datas_bloqueadas`, `datas_liberadas`
   - Ative o **Authentication → E-mail/senha** e cadastre um usuário admin
   - Aplique as regras de segurança do Firestore (ver seção abaixo)

4. Configure o Cloudinary:
   - Crie um upload preset `Unsigned` com pasta `maria-chocolate`

5. Configure o EmailJS:
   - Crie 2 templates e atualize os IDs nos arquivos HTML

6. Abra `instrutores.html` no navegador ou publique via GitHub Pages

### Regras do Firestore
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /agendamentos/{id} {
      allow create: if true;
      allow read: if true;
      allow update, delete: if request.auth != null;
    }
    match /datas_bloqueadas/{id} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /datas_liberadas/{id} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

---

## 📁 Estrutura do Projeto

```
calendario-cursos-maria-chocolate/
├── instrutores.html     Portal de agendamento para instrutores
├── admin.html           Painel administrativo
├── logo.png             Logomarca
└── README.md            Este arquivo
```

---

## 📸 Screenshots

### Calendário dos Instrutores
![Calendário](print%201.png)

### Seleção de Período
![Período](print%202.png)

### Painel Administrativo
![Admin](print%203.png)

### Detalhes da Solicitação
![Detalhes](print%204.png)

---

## 👤 Autor

Desenvolvido por **Heberth Augusto**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/heberth-dornela-ti/)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=flat&logo=github&logoColor=white)](https://github.com/heberthaugusto)

---

*Projeto desenvolvido utilizando a **Claude IA** como assistente de desenvolvimento.*
