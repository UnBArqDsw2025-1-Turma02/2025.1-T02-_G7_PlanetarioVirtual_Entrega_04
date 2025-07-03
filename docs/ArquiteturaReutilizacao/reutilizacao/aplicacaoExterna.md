# Reutilização de Frameworks e Bibliotecas

Este documento tem como objetivo demonstrar como o projeto aplicou o princípio da reutilização de caixa-preta através do uso de frameworks e bibliotecas de terceiros. Ao invés de desenvolvermos do zero funcionalidades complexas e comuns a aplicações web modernas, optamos por integrar soluções robustas e consolidadas, como o Next.js para a arquitetura do frontend e o Tailwind CSS para a estilização, focando nossos esforços no desenvolvimento das regras de negócio específicas do Planetário Virtual.

## Next.js: Reutilização da Arquitetura do Frontend

O Next.js foi escolhido como o framework principal para o frontend, permitindo-nos reutilizar uma arquitetura completa e otimizada para a criação de aplicações React. Essa abordagem de reutilização de caixa-preta nos isenta da necessidade de configurar manualmente tarefas complexas de build, roteamento e renderização.

### Ativos Reutilizados:

* Sistema de Roteamento Baseado em Arquivos: O Next.js cria rotas automaticamente com base na estrutura de diretórios src/app/, eliminando a necessidade de qualquer biblioteca ou configuração manual de roteamento.

* Renderização no Servidor (SSR) e Geração Estática (SSG): Reutilizamos a capacidade do Next.js de pré-renderizar páginas no servidor, melhorando o desempenho e o SEO da aplicação.

* Otimização de Build: Aproveitamos o compilador e o bundler (Webpack/SWC) integrados e otimizados do Next.js, que realizam minificação de código, code splitting e otimização de imagens (next/image) de forma automática.

* Componentes de Alto Nível: Utilizamos componentes como <Link> e <Image> que encapsulam funcionalidades complexas (navegação client-side, otimização de imagens) em interfaces simples.

### Código Comprobatório

[Ver código completo](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/blob/main/projeto/grupo1/frontend/src/components/forum/PostItem.tsx)

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
A reutilização é evidente pela ausência de configuração de rotas. Não há um arquivo ou bloco de código no projeto definindo que o caminho /postagens/[id] deve renderizar a PostDetailView. Simplesmente ao criar a estrutura de arquivos src/app/postagens/[id]/page.tsx, o Next.js se encarrega de toda a lógica de roteamento. O uso do componente <Link href={'/postagens/${post.id}'}> demonstra a aplicação direta dessa funcionalidade, que otimiza a navegação entre páginas sem um recarregamento completo (Client-Side Navigation).

## Tailwind CSS: Reutilização de um Sistema de Design

Para a estilização da interface, adotamos o Tailwind CSS, uma biblioteca que oferece um sistema de design completo através de classes utilitárias. Em vez de escrever CSS personalizado para cada componente, reutilizamos um conjunto de primitivas de design consistentes e configuráveis.


## Ativos Reutilizados:

* Classes Utilitárias: Um vasto conjunto de classes (ex: flex, p-4, text-white, rounded-lg) que aplicam estilos CSS específicos diretamente no HTML.

* Sistema de Design Responsivo: Reutilizamos os prefixos responsivos do Tailwind (ex: sm:, md:, lg:) para construir interfaces que se adaptam a diferentes tamanhos de tela sem a necessidade de escrever media queries manualmente.

* Paleta de Cores e Espaçamento: Utilizamos a paleta de cores e a escala de espaçamento pré-definidas e consistentes do framework, garantindo uniformidade visual em toda a aplicação.

* Estados (Hover, Focus, Disabled): Reutilizamos os modificadores de estado (ex: hover:bg-blue-600, focus:ring-2, disabled:opacity-50) para estilizar interações do usuário de forma declarativa.

## Código Comprobatório:

O código do componente CreatePostForm abaixo demonstra claramente como a composição de classes utilitárias do Tailwind é usada para construir uma interface complexa e estilizada.

[Ver código completo](https://github.com/UnBArqDsw2025-1-Turma02/2025.1-T02-_G7_PlanetarioVirtual_Entrega_03/blob/main/projeto/grupo1/frontend/src/components/forum/CreatePostForm.tsx)

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

## Explicação: 
A reutilização é demonstrada pela composição de classes. Em vez de um arquivo .css com seletores e regras (.create-post-form { margin-bottom: 2rem; background-color: #1f2937; ... }), nós aplicamos diretamente os estilos pré-fabricados do Tailwind no atributo className. Classes como bg-blue-500, hover:bg-blue-600 e disabled:opacity-50 encapsulam regras CSS e media queries, permitindo-nos construir um componente visualmente consistente e funcional de forma rápida e declarativa, reutilizando o sistema de design do Tailwind em sua totalidade.

## Histórico de Versões

| Versão | Data       | Descrição                                      | Autor               | Revisor            |
|--------|------------|------------------------------------------------|---------------------|--------------------|
| 1.0    | 03/07/2025 | Criação do documento e documentação do Next.js e Tailwind | [Joao Pedro](https://github.com/joaopedrooss)   |       [Rafael Pereira](https://github.com/rafgpereira)   | 