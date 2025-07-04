<div _ngcontent-ng-c713051892="" class="immersive-editor markdown stronger" id="extended-response-markdown-content"><div contenteditable="false" translate="no" class="ProseMirror"><h1>Aplicação Interna - Reutilização por Componentização</h1><p>Este documento detalha a estratégia de <strong>reutilização de caixa-branca</strong> do projeto, focada na criação de nossos próprios ativos reutilizáveis. A principal manifestação dessa estratégia é a criação de um conjunto de funções e componentes que encapsulam lógicas complexas, permitindo sua reutilização em diferentes partes do sistema, como no Planetário e na funcionalidade de Imagem do Dia.</p><h3>1. A Estratégia: Construindo uma Biblioteca de Funções Interna</h3><p>A estratégia da equipe para a reutilização interna foi focada na criação de funções de alta coesão e baixo acoplamento, que servem como "blocos de construção" para as funcionalidades do sistema. Em vez de duplicar código para tarefas recorrentes, como a criação de elementos visuais ou a comunicação com APIs, desenvolvemos um conjunto de "funções utilitárias" parametrizáveis.</p><p>A abordagem seguiu os seguintes princípios:</p><ul><li><p><strong>Separação de Lógica e Apresentação:</strong> Funções encapsulam a lógica de criação ou de negócio, enquanto a configuração e a renderização são gerenciadas separadamente.</p></li><li><p><strong>Responsabilidade Única:</strong> Cada função foi projetada para ter uma responsabilidade clara e bem definida, facilitando seu entendimento, teste e manutenção.</p></li><li><p><strong>Parametrização:</strong> As funções são projetadas para serem genéricas, aceitando parâmetros que permitem adaptar seu comportamento a diferentes contextos sem a necessidade de alterar seu código-fonte.</p></li></ul><h3>2. Estudos de Caso</h3><p>Esta seção apresenta exemplos concretos de reutilização interna, divididos pelas principais funcionalidades do projeto.</p><h4>Parte 1: Planetário e Imagem do Dia</h4><h5>2.1. Estudo de Caso: Função <code>criarAstro</code></h5><ul><li><p><strong>O Problema:</strong>
A necessidade de renderizar múltiplos corpos celestes (planetas, luas) de forma programática, cada um com características únicas (tamanho, cor, órbita, textura) sem duplicar a complexa lógica de criação e animação para cada astro.</p></li><li><p><strong>A Solução (O Código do Ativo):</strong>
Foi criada uma função genérica que centraliza a lógica de criação de um astro, recebendo todas as suas propriedades como parâmetros.</p><pre><code>/**
 * Função genérica para criar astros no sistema solar.
 * @param {Phaser.Scene} scene - A cena onde o astro será criado.
 * @param {number} x - Posição inicial X.
 * @param {number} y - Posição inicial Y.
 * @param {number} tamanho - Raio do astro.
 * @param {number} corHex - Cor do astro em hexadecimal.
 * @param {object} orbita - Configuração da órbita { centro, raio, velocidade }.
 * @param {number} zoom - Nível de zoom ao clicar.
 * @param {string} textura - Chave da imagem de textura.
 * @returns {Phaser.GameObjects.Sprite} O objeto do astro criado.
 */
function criarAstro(scene, x, y, tamanho, corHex, orbita, zoom, textura) {
    // ... lógica de criação do sprite, física e animação da órbita ...
}
<br class="ProseMirror-trailingBreak"></code></pre></li><li><p><strong>A Aplicação (Reutilização no Projeto):</strong>
A mesma função é chamada múltiplas vezes para popular o sistema solar, criando tanto planetas orbitando o sol quanto luas orbitando planetas.</p><pre><code>// Criando a Terra orbitando o Sol
const terra = criarAstro(this, 0, 0, 10, 0x0000ff, { centro: sol, raio: 200, velocidade: astroVel }, 30, 'terra');

// Criando a Lua orbitando a Terra
const lua = criarAstro(this, 0, 0, 2, 0xffffff, { centro: terra, raio: 20, velocidade: 0.01 }, 90, 'lua');
<br class="ProseMirror-trailingBreak"></code></pre></li></ul><h5>2.2. Estudo de Caso: Função <code>criarCinturao</code></h5><ul><li><p><strong>O Problema:</strong>
A necessidade de criar cinturões de asteroides com centenas ou milhares de objetos, o que seria impraticável e ineficiente de se fazer manualmente.</p></li><li><p><strong>A Solução (O Código do Ativo):</strong>
Uma função reutilizável foi desenvolvida para gerar um número configurável de asteroides dentro de um raio definido, distribuindo-os de forma procedural.</p><pre><code>/**
 * Função para criar um cinturão de asteroides.
 * @param {Phaser.Scene} scene - A cena.
 * @param {number} x - Posição X do centro.
 * @param {number} y - Posição Y do centro.
 * @param {number} raioInterno - Raio interno do cinturão.
 * @param {number} raioExterno - Raio externo do cinturão.
 * @param {number} quantidade - Número de asteroides.
 */
function criarCinturao(scene, x, y, raioInterno, raioExterno, quantidade) {
    // ... lógica para gerar múltiplos asteroides em posições aleatórias dentro dos raios ...
}
<br class="ProseMirror-trailingBreak"></code></pre></li><li><p><strong>A Aplicação (Reutilização no Projeto):</strong>
A função é usada para criar tanto o cinturão de asteroides principal quanto o cinturão de Kuiper.</p><pre><code>const asteroides = criarCinturao(this, 0, 0, 280, 320, 200);
const kuiper = criarCinturao(this, 0, 0, 700, 800, 600);
<br class="ProseMirror-trailingBreak"></code></pre></li></ul><h5>2.3. Estudo de Caso: Funções de Adaptação e Comunicação com API (APOD)</h5><ul><li><p><strong>O Problema:</strong>
A necessidade de desacoplar a lógica de comunicação com a API APOD da NASA da lógica de apresentação dos dados na interface. Era preciso também adaptar a resposta da API para um formato mais conveniente para a aplicação.</p></li><li><p><strong>A Solução (O Código do Ativo):</strong>
Foram criadas múltiplas funções com responsabilidades únicas: uma para buscar os dados (<code>photoProvider</code>), uma para adaptar o formato (<code>nasaApodAdapter</code>), e outra para orquestrar a busca e exibição (<code>fetchAndDisplayNasaPhotos</code>).</p><pre><code>// Adapta os dados da API para o formato esperado pela aplicação
function nasaApodAdapter(data) {
    // ... lógica de transformação dos dados ...
    return adaptedData;
}

// Abstrai a lógica de busca de fotos, permitindo trocar a fonte de dados no futuro
const photoProvider = {
    getPhotos: async (startDate, endDate) =&gt; {
        // ... lógica de chamada à API usando fetch ...
    }
};
<br class="ProseMirror-trailingBreak"></code></pre></li><li><p><strong>A Aplicação (Reutilização no Projeto):</strong>
Essas funções são combinadas para criar a funcionalidade completa da "Imagem do Dia", podendo ser reutilizadas ou estendidas facilmente.</p><pre><code>async function fetchAndDisplayNasaPhotos(provider, startDate, endDate) {
    const photos = await provider.getPhotos(startDate, endDate);
    const adaptedPhotos = photos.map(nasaApodAdapter);
    // ... lógica para exibir as fotos adaptadas na UI ...
}

// Chamada da função principal
fetchAndDisplayNasaPhotos(photoProvider, '2025-06-01', '2025-06-10');
<br class="ProseMirror-trailingBreak"></code></pre></li></ul><h4>Parte 2: Fórum (A ser adicionado)</h4><p><em>(Nesta seção, você pode adicionar seus estudos de caso seguindo o mesmo modelo acima. Por exemplo: um componente para renderizar um post, um botão de "curtir" reutilizável, uma função para formatar datas, etc.)</em></p><h3>Reflexões Críticas</h3><p>A criação de um conjunto de funções utilitárias e componentes internos foi uma decisão estratégica que trouxe um balanço positivo entre o investimento inicial de tempo e os ganhos de produtividade e qualidade a longo prazo. Funções como <code>criarAstro</code> e <code>criarCinturao</code> foram essenciais para a viabilidade do planetário, enquanto as abstrações para a API da NASA (<code>photoProvider</code>, <code>nasaApodAdapter</code>) tornaram o código mais limpo, testável e fácil de manter.</p><p>O principal desafio foi definir o nível correto de abstração para cada função, evitando que se tornassem complexas demais ou genéricas a ponto de perderem a utilidade.</p><p>Como possíveis melhorias, a modularização poderia ser aprimorada, separando funções utilitárias em arquivos dedicados para melhorar a organização. Além disso, a implementação de testes unitários para funções críticas como <code>criarAstro</code> e <code>nasaApodAdapter</code> aumentaria a robustez e a confiabilidade do sistema.</p><h3>Rastreabilidade &amp; Elos com Outros Artefatos</h3><ul><li><p><strong>Commit de Criação do <code>criarAstro</code>:</strong> <code>[Inserir link para o commit no GitHub/GitLab]</code></p></li><li><p><strong>Commit de Refatoração da API APOD:</strong> <code>[Inserir link para o commit no GitHub/GitLab]</code></p></li><li><p><strong>Protótipo da Interface do Planetário:</strong> <code>[Inserir link para o Figma ou outra ferramenta]</code></p></li></ul><h3>Histórico de Versões</h3><table><tbody><tr><th><p>Versão</p></th><th><p>Data</p></th><th><p>Descrição</p></th><th><p>Autor</p></th><th><p>Revisor</p></th></tr><tr><td><p>1.0</p></td><td><p>01/06/2025</p></td><td><p>Criação do documento</p></td><td><p><a href="https://github.com/manoelmoura" title="null">Manoel Moura</a></p></td><td><p><a href="https://github.com/joaopedrooss" title="null">Joao Pedro</a></p></td></tr><tr><td><p>1.1</p></td><td><p>03/07/2025</p></td><td><p>Adição de reutilizações da pasta <code>fotos</code></p></td><td><p><a href="https://github.com/manoelmoura" title="null">Manoel Moura</a></p></td><td><p><a href="https://github.com/jlucasiqueira" title="null">João Lucas</a></p></td></tr></tbody></table><h3>Referências</h3><ul><li><p>Documentação Oficial do Phaser.js: <a href="https://phaser.io/docs" title="null">https://phaser.io/docs</a></p></li><li><p>Padrão de Projeto Adapter: <a href="https://refactoring.guru/design-patterns/adapter" title="null">https://refactoring.guru/design-patterns/adapter</a></p></li></ul></div></div>