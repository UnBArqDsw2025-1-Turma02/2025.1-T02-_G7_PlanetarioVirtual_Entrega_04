# Reutilização de Frameworks e Bibliotecas

# FrontEnd

Este documento tem como objetivo demonstrar como o projeto aplicou o princípio da reutilização de caixa-preta através do uso de frameworks e bibliotecas de terceiros. Ao invés de desenvolvermos do zero funcionalidades complexas e comuns a aplicações web modernas, optamos por integrar soluções robustas e consolidadas, como o Next.js para a arquitetura do frontend e o Tailwind CSS para a estilização, focando nossos esforços no desenvolvimento das regras de negócio específicas do Planetário Virtual.

> [Link para o código do FrontEnd](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/tree/main/projeto/grupo1/frontend)
>
> [Link para o projeto em funcionamento](https://2025-1-t02-g7-planetario-virtual-en-seven.vercel.app/home/index.html)

## Next.js: Reutilização da Arquitetura do Frontend

O Next.js foi escolhido como o framework principal para o frontend, permitindo-nos reutilizar uma arquitetura completa e otimizada para a criação de aplicações React. Essa abordagem de reutilização de caixa-preta nos isenta da necessidade de configurar manualmente tarefas complexas de build, roteamento e renderização.

> [Documentação do Next.js](https://nextjs.org/docs)


### Ativos Reutilizados:

* Sistema de Roteamento Baseado em Arquivos: O Next.js cria rotas automaticamente com base na estrutura de diretórios src/app/, eliminando a necessidade de qualquer biblioteca ou configuração manual de roteamento.

* Renderização no Servidor (SSR) e Geração Estática (SSG): Reutilizamos a capacidade do Next.js de pré-renderizar páginas no servidor, melhorando o desempenho e o SEO da aplicação.

* Otimização de Build: Aproveitamos o compilador e o bundler (Webpack/SWC) integrados e otimizados do Next.js, que realizam minificação de código, code splitting e otimização de imagens (next/image) de forma automática.

* Componentes de Alto Nível: Utilizamos componentes como <Link> e <Image> que encapsulam funcionalidades complexas (navegação client-side, otimização de imagens) em interfaces simples.

### Código Comprobatório

> [Ver código completo](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/blob/main/projeto/grupo1/frontend/src/components/forum/PostItem.tsx)

```
// src/components/forum/PostItem.tsx

"use client";

import type { Post } from '@/services/api';
import Link from 'next/link'; // Importação do componente de Link do Next.js
import { MessageCircle, Trash2, User as UserIcon } from 'lucide-react';
import { useAuth } from '@/contexts/AuthContext';

type PostItemProps = {
  post: Post;
  onDelete: (postId: number) => void;
};

export function PostItem({ post, onDelete }: PostItemProps) {
  // ... (lógica do componente)

  return (
    <div className="bg-gray-800 border border-gray-700 rounded-lg mb-6 overflow-hidden relative">
      {/* ... (código do botão de deletar) */}

      <div className="block p-6">
        {/* ... (código do autor e texto) */}
      </div>

      {/* REUTILIZAÇÃO: O componente Link direciona para a página de detalhes do post
          sem a necessidade de configurar a rota em um arquivo centralizado.
          A rota `/postagens/${post.id}` funciona automaticamente porque existe
          uma estrutura de pastas como `src/app/postagens/[id]/page.tsx`. */}
      <Link href={`/postagens/${post.id}#comentar`}>
        <div className="bg-gray-800/50 border-t border-gray-700 px-6 py-3 flex justify-between items-center hover:bg-gray-700/50 transition-colors duration-200">
          <div className="flex items-center gap-2 text-gray-400">
            <MessageCircle size={16} />
            <span className="text-sm font-medium">Comentários</span>
          </div>
        </div>
      </Link>
    </div>
  );
}
```
### Explicação: 
A reutilização é evidente pela ausência de configuração de rotas. Não há um arquivo ou bloco de código no projeto definindo que o caminho /postagens/[id] deve renderizar a PostDetailView. Simplesmente ao criar a estrutura de arquivos src/app/postagens/[id]/page.tsx, o Next.js se encarrega de toda a lógica de roteamento. O uso do componente <Link href={'/postagens/${post.id'}> demonstra a aplicação direta dessa funcionalidade, que otimiza a navegação entre páginas sem um recarregamento completo (Client-Side Navigation).

## Tailwind CSS: Reutilização de um Sistema de Design

Para a estilização da interface, adotamos o Tailwind CSS, uma biblioteca que oferece um sistema de design completo através de classes utilitárias. Em vez de escrever CSS personalizado para cada componente, reutilizamos um conjunto de primitivas de design consistentes e configuráveis.

> [Documentação do Tailwind](https://v2.tailwindcss.com/docs)


### Ativos Reutilizados:

* Classes Utilitárias: Um vasto conjunto de classes (ex: flex, p-4, text-white, rounded-lg) que aplicam estilos CSS específicos diretamente no HTML.

* Sistema de Design Responsivo: Reutilizamos os prefixos responsivos do Tailwind (ex: sm:, md:, lg:) para construir interfaces que se adaptam a diferentes tamanhos de tela sem a necessidade de escrever media queries manualmente.

* Paleta de Cores e Espaçamento: Utilizamos a paleta de cores e a escala de espaçamento pré-definidas e consistentes do framework, garantindo uniformidade visual em toda a aplicação.

* Estados (Hover, Focus, Disabled): Reutilizamos os modificadores de estado (ex: hover:bg-blue-600, focus:ring-2, disabled:opacity-50) para estilizar interações do usuário de forma declarativa.

### Código Comprobatório:

O código do componente CreatePostForm abaixo demonstra claramente como a composição de classes utilitárias do Tailwind é usada para construir uma interface complexa e estilizada.

> [Ver código completo](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/blob/main/projeto/grupo1/frontend/src/components/forum/CreatePostForm.tsx)

```
// src/components/forum/CreatePostForm.tsx

"use client";

import { useState } from 'react';

// ... (definição de tipos)

export function CreatePostForm({ onPostSubmit }: CreatePostFormProps) {
  const [text, setText] = useState('');

  // ... (lógica do componente)

  return (
    // REUTILIZAÇÃO: Cada classe é um estilo pré-construído.
    // mb-8: margem inferior | bg-gray-800: cor de fundo | border/border-gray-700: borda e sua cor | p-4: preenchimento | rounded-lg: cantos arredondados
    <form onSubmit={handleSubmit} className="mb-8 bg-gray-800 border border-gray-700 p-4 rounded-lg">
      <h2 className="text-xl font-semibold text-white mb-3">Criar Nova Postagem</h2>
      <textarea
        // ... (props)
        // w-full: largura total | p-2: preenchimento | bg-gray-700: fundo | text-gray-200: cor do texto
        // rounded-md: cantos | border/border-gray-600: borda
        // focus:outline-none/focus:ring-2/focus:ring-blue-500: estilos para o estado de foco
        className="w-full p-2 bg-gray-700 text-gray-200 rounded-md border border-gray-600 focus:outline-none focus:ring-2 focus:ring-blue-500"
        placeholder="O que você está pensando sobre o universo?"
        rows={4}
      />
      <div className="text-right mt-3">
        <button
          type="submit"
          // bg-blue-500: cor de fundo | hover:bg-blue-600: cor no hover | text-white: cor do texto
          // font-bold: peso da fonte | py-2/px-6: preenchimento vertical/horizontal | rounded-lg: cantos
          // transition/duration-300: animação suave | disabled:opacity-50: estilo para estado desabilitado
          className="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-6 rounded-lg transition duration-300 disabled:opacity-50"
          disabled={!text.trim()}
        >
          Publicar
        </button>
      </div>
    </form>
  );
}
```

### Explicação: 
A reutilização é demonstrada pela composição de classes. Em vez de um arquivo .css com seletores e regras (.create-post-form { margin-bottom: 2rem; background-color: #1f2937; ... }), nós aplicamos diretamente os estilos pré-fabricados do Tailwind no atributo className. Classes como bg-blue-500, hover:bg-blue-600 e disabled:opacity-50 encapsulam regras CSS e media queries, permitindo-nos construir um componente visualmente consistente e funcional de forma rápida e declarativa, reutilizando o sistema de design do Tailwind em sua totalidade.


## React-Toastify: Reutilização de um Sistema de Notificações

Para fornecer feedback visual e imediato ao usuário após a conclusão de ações importantes (como criar um post, deletar um comentário, etc.), o projeto reutiliza a biblioteca react-toastify. Esta abordagem nos poupa do trabalho de desenvolver um sistema complexo de notificações do zero, permitindo-nos exibir mensagens de sucesso, erro e aviso de forma padronizada e com mínimo esforço de implementação.

> [Documentação do Toastfy](https://fkhadra.github.io/react-toastify/introduction/)

### Ativos Reutilizados

* Funções de Notificação: Reutilizamos diretamente as funções toast.success(), toast.error() e toast.warn() para disparar notificações com semânticas e aparências distintas, de acordo com o resultado da ação do usuário.

* Componente ToastContainer: Embora não esteja no trecho abaixo, a configuração inicial do projeto inclui o <ToastContainer>, um componente que é renderizado uma única vez e gerencia a fila, a posição e o comportamento de todas as notificações na tela.

* Estilização e Animações Padrão: Aproveitamos os estilos e as animações de entrada e saída que vêm pré-configurados com a biblioteca, garantindo uma experiência de usuário consistente e profissional sem a necessidade de CSS personalizado.

* Sistema de Fila e Temporizador: Reutilizamos a lógica interna da biblioteca que gerencia o empilhamento de múltiplas notificações e seu desaparecimento automático após um período de tempo.

### Código comprobatório: 

O trecho de código abaixo, extraído do componente PostDetailView, demonstra como as notificações são disparadas em resposta a ações do usuário, como submeter ou deletar um comentário.

> [Ver código completo](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/blob/main/projeto/grupo1/frontend/src/components/forum/PostDetailView.tsx)

```
// src/components/forum/PostDetailView.tsx
"use client";
// ... outras importações
import { toast } from 'react-toastify'; // <-- Importação da funcionalidade

export function PostDetailView({ initialPost }: PostDetailViewProps) {
  // ... (state e outras lógicas)

  const handleCommentSubmit = async (commentText: string) => {
    if (!user) {
      // REUTILIZAÇÃO: Dispara um aviso se o usuário não estiver logado.
      toast.warn("Você precisa estar logado para comentar.");
      return;
    }
    // ...

    try {
      // ... (lógica da API)
      
      // REUTILIZAÇÃO: Notificação de sucesso após a criação do comentário.
      toast.success("Comentário adicionado com sucesso!");
    } catch (error) {
        // ...
        // REUTILIZAÇÃO: Notificação de erro em caso de falha.
        toast.error(`Falha ao criar o comentário: ${msg}`);
    }
  };

  const handleDeleteComment = async (commentId: number) => {
    // ... (lógica de confirmação)

    try {
      const resultado = await deleteCommentAPI(commentId, user.id);
      if (resultado.success) {
        // ...
        // REUTILIZAÇÃO: Notificação de sucesso após deletar o comentário.
        toast.success("Comentário deletado com sucesso!");
      } else {
        toast.error(resultado.message || "Falha ao deletar o comentário.");
      }
    } catch (error) {
        // ...
        const msg = error instanceof Error ? error.message : "Erro desconhecido ao tentar deletar.";
        toast.error(`Falha ao deletar o comentário: ${msg}`);
    }
  };

  // ... (resto do JSX)
}
```

### Explicação:

A reutilização de caixa-preta é evidente na simplicidade das chamadas de função. Ao invés de criar e gerenciar elementos DOM, controlar timers com setTimeout, aplicar classes de CSS para estilização e animação, o desenvolvedor simplesmente importa o objeto toast e invoca métodos como toast.success(). Cada chamada é uma interface simples para um subsistema complexo, encapsulando toda a lógica de renderização, enfileiramento e remoção das notificações. Isso demonstra uma reutilização eficaz, permitindo que o código da aplicação se concentre na lógica de negócios (chamar a API, atualizar o estado) e delegue a responsabilidade de feedback visual para a biblioteca.

# BackEnd

Este documento tem como objetivo demonstrar como o projeto aplicou o princípio da reutilização de caixa-preta no backend da aplicação externa, por meio da integração de frameworks e bibliotecas consolidadas. Essa abordagem permitiu acelerar o desenvolvimento, garantir maior confiabilidade e concentrar os esforços na lógica de negócio do Planetário Virtual, evitando reinventar soluções já bem estabelecidas.

> [Link para o código do Backend](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/tree/main/projeto/grupo1/backend)

## FastAPI: Reutilização da Arquitetura para APIs Web Assíncronas

O backend do Planetário Virtual reutiliza a arquitetura fornecida pelo framework **FastAPI**, uma solução moderna, performática e fortemente baseada em tipagem. A equipe aplicou o princípio da reutilização de **caixa-preta**, adotando recursos prontos como roteadores com `APIRouter`, tratamento de erros, integração com OpenAPI, e suporte a middlewares.

Essa abordagem permitiu que o foco se mantivesse na lógica do domínio do sistema (como postagens, comentários e usuários), enquanto o framework se encarrega de aspectos estruturais da aplicação web.

> [Documentação oficial do FastAPI](https://fastapi.tiangolo.com/)

### Ativos Reutilizados

- Roteadores com `APIRouter` para modularizar os endpoints.
- Tipagem automática e validação com Pydantic.
- Suporte automático à documentação OpenAPI/Swagger.
- Tratamento de erros com `HTTPException` e códigos de status prontos.
- Integração com decoradores personalizados, como `VerificadorPermissaoFactory`.

---

### Código Comprobatório

> [Ver código completo](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/blob/main/projeto/grupo1/backend/app/routers/postagem_router.py)


```python
# backend/app/routers/postagem_router.py

  from fastapi import APIRouter, HTTPException, status, Request
  from typing import List
  from ..services.postagem_service import post_service
  from ..models.postagem_model import PostCreate, PostResponse
  from app.decorators.auth_decorator import VerificadorPermissaoFactory

  router = APIRouter(
      prefix="/postagens",
      tags=["Postagens"]
  )

  @router.get("/", response_model=List[PostResponse])
  async def listar_postagens():
      return post_service.get_all_posts()

  @router.post("/", response_model=PostResponse, status_code=status.HTTP_201_CREATED)
  async def criar_postagem(post: PostCreate):
      return post_service.create_post(post)
```

### Explicação

- A instância `APIRouter` permite dividir as rotas da aplicação em módulos reutilizáveis — cada arquivo (`postagem_router.py`, `comentario_router.py`, etc.) atua como uma *feature isolada*, o que reduz acoplamento e melhora a manutenção.
- O uso de tipos e validações automáticas (como `PostCreate` e `PostResponse`) é possível graças à integração com o Pydantic, uma biblioteca reutilizada como parte da arquitetura da FastAPI.
- Os códigos de status HTTP (como `201_CREATED`) e as exceções padronizadas com `HTTPException` representam a reutilização de constantes e estruturas para respostas padronizadas.
- O roteamento assíncrono é configurado automaticamente pela FastAPI, sem a necessidade de gerenciamento manual de threads ou sockets, o que simplifica a implementação e melhora o desempenho.

## Reutilização da Lógica de Negócio com Serviços Python

O backend do Planetário Virtual reutiliza a arquitetura de **serviços** para isolar a lógica de negócio do restante da aplicação. Essa estratégia se encaixa no modelo de reutilização **caixa-branca**, pois o código-fonte dos serviços é acessado, modificado e reutilizado em múltiplas rotas (camada de controle).

Esses serviços manipulam diretamente os dados armazenados em `db.json`, aplicando regras como controle de IDs, vínculos entre usuários, comentários e postagens, validações e persistência.

> [PEP 8 – Guia de estilo para Python](https://peps.python.org/pep-0008/)

### Ativos Reutilizados

- Serviços reutilizáveis por entidade (`post_service`, `comment_service`, `forum_service`)
- Funções de carregamento e salvamento de banco local (_load_db, _save_db) reaproveitadas internamente
- Instâncias globais reutilizadas em toda a aplicação
- Padronização das funções de CRUD, com retorno consistente entre entidades

---

### Código Comprobatório

> [Ver código completo](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/blob/main/projeto/grupo1/backend/app/services/postagem_service.py)


```python
# backend/app/services/postagem_service.py

class PostService:
    def __init__(self, db_path: Path = DB_PATH):
        self.db_path = db_path

    def _load_db(self) -> dict:
        if not self.db_path.exists():
            return {"usuarios": [], "postagens": []}
        with open(self.db_path, "r", encoding="utf-8") as f:
            return json.load(f)

    def _save_db(self, db: dict):
        with open(self.db_path, "w", encoding="utf-8") as f:
            json.dump(db, f, indent=2, ensure_ascii=False)

    def create_post(self, post_data: PostCreate) -> PostData:
        db = self._load_db()
        postagens = db.get("postagens", [])

        existing_ids = [int(p["id"]) for p in postagens if str(p["id"]).isdigit()]
        next_id = max(existing_ids) + 1 if existing_ids else 1

        nome_autor = next((u["nome"] for u in db.get("usuarios", []) if u["id"] == post_data.autor_id), "Autor Desconhecido")

        new_post = {
            "id": next_id,
            "conteudo": post_data.conteudo,
            "autor_id": post_data.autor_id,
            "data_criacao": datetime.now().isoformat(),
            "numero_comentarios": len(self.get_comments_by_post_id(next_id)),
            "nome_autor": nome_autor
        }

        db["postagens"].append(new_post)
        self._save_db(db)

        return PostData(**new_post)
```
### Explicação

- O padrão Service Layer isola a lógica de manipulação de dados em arquivos dedicados, facilitando a reutilização das funções `create_post`, `get_all_posts`, `delete_post`, etc.
- Funções como `_load_db()` e `_save_db()` são reutilizadas por todas as operações de leitura e escrita no arquivo `db.json`.
- A função `create_post()` encapsula várias regras de negócio:
  - Geração de ID sequencial
  - Inclusão da data de criação
  - Associação com o nome do autor
  - Inicialização do contador de comentários
- Esse padrão é repetido de forma semelhante nos serviços de usuários (`forum_service.py`) e comentários (`comentario_service.py`), promovendo consistência e reutilização de estrutura entre os módulos.

## Pydantic: Reutilização de Modelos de Dados e Validação

O projeto reutiliza a biblioteca **Pydantic** para definir os modelos de entrada e saída das requisições. Esses modelos são usados tanto nas rotas quanto nas camadas de serviço, garantindo validação automática, documentação via Swagger e segurança contra dados inválidos.

Essa abordagem representa uma forma de reutilização de **caixa-preta**, pois usamos a estrutura do Pydantic para validar e serializar dados, sem precisar implementar manualmente verificações, casting de tipos ou validação de campos obrigatórios.

> [Documentação do Pydantic](https://docs.pydantic.dev/)

### Ativos Reutilizados

- **Modelos reutilizáveis com Pydantic** (`PostCreate`, `PostResponse`, `CommentCreate`, `UserCreate`, etc.)
- **Validação automática nas rotas FastAPI**
- **Tipagem forte nos serviços**
- **Geração automática de schemas JSON e documentação Swagger**

---

### Código Comprobatório

> [Ver código completo](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/blob/main/projeto/grupo1/backend/app/models/postagem_model.py)

```python
# backend/app/models/postagem_model.py

from pydantic import BaseModel
from typing import Optional

class PostCreate(BaseModel):
    conteudo: str
    autor_id: int

class PostResponse(PostCreate):
    id: int
    data_criacao: str
    numero_comentarios: int
    nome_autor: Optional[str] = "Autor Desconhecido"


# backend/app/routers/postagem_router.py

@router.post("/", response_model=PostResponse, status_code=status.HTTP_201_CREATED)
async def criar_postagem(post: PostCreate):
    return post_service.create_post(post)

```

### Explicação

- Os modelos `PostCreate` e `PostResponse` definem de forma declarativa os campos esperados nas requisições e respostas, eliminando a necessidade de validação manual.
- A FastAPI utiliza os modelos do Pydantic para gerar automaticamente a documentação Swagger (OpenAPI), além de retornar mensagens de erro detalhadas quando os dados estão inválidos.
- A herança entre modelos (`PostResponse` estende `PostCreate`) demonstra reutilização direta de estrutura, evitando repetição de código.
- Esses mesmos modelos são utilizados internamente nos serviços (`PostData(**post_dict)`), mantendo padronização em toda a aplicação — desde a entrada do dado até sua persistência.

## Reflexões Críticas

A estratégia adotada no backend do Planetário Virtual combina reutilização de **caixa-preta**, com o uso de frameworks e bibliotecas consolidadas como FastAPI e Pydantic, e **caixa-branca**, com a criação de serviços reutilizáveis dentro do projeto.

Entre os **principais benefícios observados** estão:

- Redução significativa do tempo de desenvolvimento;
- Centralização das regras de negócio, facilitando a manutenção;
- Validação de dados robusta e automática;
- Organização modular por entidades, seguindo boas práticas de design.

Por outro lado, essa abordagem exige atenção à **manutenção dos contratos de dados** (modelos Pydantic), além de exigir familiaridade com conceitos como async/await, tipagem forte e estruturação de projetos em camadas.

Apesar disso, os ganhos em clareza, reaproveitamento e robustez do backend compensaram totalmente a curva inicial de aprendizado.

---

## Rastreabilidade & Elos com Outros Artefatos

- [Repositório completo do backend](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/tree/main/projeto/grupo1/backend)
- [Definição dos roteadores (comentário, postagem, usuário)](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/tree/main/projeto/grupo1/backend/app/routers)
- [Serviços reutilizáveis](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/tree/main/projeto/grupo1/backend/app/services)
- [Modelos e schemas Pydantic](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/tree/main/projeto/grupo1/backend/app/models)

## Histórico de Versões

| Versão | Data       | Descrição                                      | Autor               | Revisor            |
|--------|------------|------------------------------------------------|---------------------|--------------------|
| 1.0    | 03/07/2025 | Criação do documento e documentação do Next.js e Tailwind | [Joao Pedro](https://github.com/joaopedrooss)   |       [Rafael Pereira](https://github.com/rafgpereira)   | 
| 1.1    | 03/07/2025 | Arrumando links e documentando Toastfy | [Joao Pedro](https://github.com/joaopedrooss)   |       [Rafael Pereira](https://github.com/rafgpereira)   | 
| 1.2    | 04/07/2025 | Documentação completa do backend com FastAPI, serviços e Pydantic | [Letícia T. S. Martins](https://github.com/leticiatmartins)                        | [Rafael Pereira](https://github.com/rafgpereira)|
