# 🚀 Teste Front-end - App Facilita

## Overview

Este README fornece uma visão geral do aplicativo Todo desenvolvido com Vue.js, destacando suas funcionalidades e aspectos técnicos.

O desafio proposto pode ser visualizado no seguinte link: 
<a>https://bitbucket.org/heliocesar01/teste-front-end-n.2/src/master/</a>

O projeto está hospedado na vercel, caso tenha interesse em visualizar, é só acessar esse link.
<a>https://challenge-front-end-app-facilita.vercel.app/</a>

## Screens

### 1. Login Screen

- Permite o acesso usando o nome de usuário: `admin` e a senha: `password`.

### 2. Dashboard

- Inclui um cabeçalho, uma barra lateral e uma seção principal com um componente TodoList.
- Inicialmente exibe uma lista de tarefas vazia.
- Inclui um botão "Adicionar" no lado direito, que abre um modal para inserir título, descrição e prioridade (urgente ou importante).
- As tarefas são exibidas com opções para marcar como concluída, editar ou excluir.
- Fornece um campo de pesquisa para filtrar tarefas por título ou descrição.

## :gear: Funcionalidades

- **Gestão de Tarefas**
  - Usuários podem adicionar tarefas com título, descrição e prioridade (urgente ou importante).
  - Tarefas podem ser marcadas como concluídas ou não concluídas usando caixas de seleção.
  - Tarefas podem ser editadas ou excluídas através das opções exibidas em cada tarefa.
  - Suporta filtragem de tarefas por categoria (todas, urgentes, importantes, outras, concluídas) e pesquisa por consulta.

## Detalhes Técnicos

- **Frameworks e Bibliotecas**

  - Desenvolvido usando Vue.js com API de composição.
  - Utiliza Vuex para gerenciamento de estado.
  - Estilização feita com Stylus para pré-processamento CSS.

- **Componentes**

  - **RegisterTaskVue**: Componente para adicionar novas tarefas.
  - **Categories**: Componente para filtrar tarefas por categoria.
  - **RemoveTask**: Componente para confirmar a exclusão de tarefas.
  - **EditTask**: Componente para editar tarefas existentes.
  - **Header**: Componente para exibir o header do site.
  - **SideBar**: Componente para navegar entre opções, mas é somente visual.
  - **TodoList**: Componente responsável por praticamente toda a lógica da lista de tarefas.

- **Armazenamento Local**
  - As tarefas são armazenadas localmente usando o `localStorage` do navegador para persistir dados entre sessões.

## Configuração e Instalação

<h2>🔧 Rodando o projeto localmente</h2>
<h3> Clonando o Repositório</h3>

```bash
git clone git@github.com:Luisfp0/challenge-front-end-app-facilita.git
ou
git clone https://github.com/Luisfp0/challenge-front-end-app-facilita.git

cd challenge-front-end-app-facilita
```

<h3>Instalando dependências</h3>

```bash
npm install
```

<h3>Executando o projeto</h3>

```bash
npm run dev
```

Isso iniciará o aplicativo em um ambiente de desenvolvimento. Siga as instruções adicionais que aparecerão no console.
