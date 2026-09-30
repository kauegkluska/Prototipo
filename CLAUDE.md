# Protótipo: site do Caminhos da Neve Clube de Tiro

Protótipo da home do **Clube Recreativo Caça e Tiro Caminhos da Neve** (nome curto: Caminhos da Neve Clube de Tiro), clube de tiro esportivo em **São Joaquim, SC**, fundado em 2016. O clube fica **só em São Joaquim**: não citar Lages nem outras cidades como sede ou local de provas.

Todo o conteúdo é em português do Brasil.

## Arquivos

- `index.html`: a home, página única com âncoras.
- `css/style.css`: todo o estilo, com os tokens em `:root` no topo.
- `js/main.js`: menu mobile, contagem de dias até o próximo evento (`data-countdown`), destaque no menu da seção visível e a mensagem do WhatsApp de cada produto.
- `img/`: todas as fotos são locais e já otimizadas.
  - `brasao.png`: o brasão real, 512×315 com fundo transparente, usado no cabeçalho e no rodapé.
  - Fotos reais do clube:
    - `atirador.jpg`: 640×1138, retrato. Foto do hero: atirador de costas, com boné e abafador, mirando com pistola na pista outdoor, com araucárias ao fundo. Veio do WhatsApp, já comprimida, e no desktop é ampliada cerca de 1,25x: se aparecer o original em resolução maior, trocar. A mesa, embaixo, tem logos de patrocinadores e fica fora do recorte.
    - `linha-de-tiro.jpg`: 768×1024, retrato. A mesa do posto 9 com a carabina, os alvos da pista e as araucárias. Foi o hero antes do `atirador.jpg` e agora não é usada: boa candidata para a galeria.
    - `sede.jpg`: 1200×960, recortada em 5:4.
    - `pista-de-ar.jpg`: 900×1200, retrato.
    - `tiro-ao-prato.jpg`: 1024×768.
  - Fotos de banco (Unsplash, licença de uso comercial livre, sem crédito obrigatório). Não mostram o clube:
    - `noticia-*.jpg`: as três fotos das notícias.
    - `cartaz-*.jpg`: fundos dos cartazes das competições.
    - `galeria-*.jpg`: as seis fotos da galeria, a maioria paisagens de serra com araucárias.
    - `produto-*.jpg`: fotos dos produtos, 800×800, recortadas ao centro (`object-fit: cover`; no tablet, em 16:10). Não mostram os produtos do clube: o boné é cinza e o moletom é verde, ao contrário das descrições. O usuário não quer ilustrações geradas no lugar delas.
- `_v1/`: versão antiga, feita em Tailwind com outras fontes. Serve só de referência: não editar nem copiar estilos dela.

O site é estático: HTML, CSS e JS puros, sem build, framework ou dependências. Para ver, basta abrir o `index.html` no navegador.

## Ordem da home

1. **Hero** (`#inicio`): nome "Caminhos da Neve" na Archivo larga, "Clube de tiro", texto de apoio e botões; à direita, a foto do atirador na linha de tiro (`.hero__media`, `atirador.jpg`). Foi escolhida no lugar da `linha-de-tiro.jpg` por mostrar gente praticando e o equipamento de segurança, e por render mais na faixa do celular. Na base, a faixa do próximo evento (`#proximo-evento`) e, fechando o hero, a serra do brasão (`.hero__ridge`, SVG inline). A grade do hero tem três áreas: `main` (texto), `event` e `ridge`.
   - A foto vai do topo do hero até a serra, colada na borda direita, com largura `--foto-w` (58%, no máximo 50rem: a foto tem só 640px de largura). O recorte (`object-position`) guarda o boné, a pistola e as araucárias, e o degradê da esquerda cai nas costas do atirador. O usuário prefere o degradê (`.hero__media::after`): a foto se desfaz no verde à esquerda, onde está o texto, em cima, junto ao cabeçalho, e embaixo, sob o próximo evento e a serra. Não trocar por foto com borda reta. O texto pode entrar no começo do degradê (`padding-right` do `.hero__body`).
   - A serra é o perfil da montanha do brasão: camada de trás em `--pinho`, a da frente em `--geada` (a cor da página, então a seção seguinte começa "na neve") e os fios das encostas em `--pinho`, como no desenho do brasão. Usa `preserveAspectRatio="xMidYMax slice"`: no celular corta as pontas e mantém o pico principal. O desenho foi achatado para 70% a pedido do usuário, para o hero não ficar apertado, e a base lisa de neve foi cortada (viewBox 1440×100; os caminhos descem até 112 e o excesso some). A primeira seção depois do hero tem margem de cima menor (`.hero + .section`), para não sobrar muito branco liso sob as montanhas. Para mudar a altura, mexer no desenho e no `--ridge-h` juntos: só baixar o `--ridge-h` corta os picos, por causa do `slice`.
   - Até 899px, o botão "Ver competição" desce para uma linha própria.
   - Até 719px, a foto vira uma faixa no topo que escurece embaixo, e a primeira linha do nome sobe para a parte escura. Foto, nome e botões ficam num bloco (`.hero__first`, que no desktop é `display: contents`) com a altura da primeira tela (`100svh` menos o cabeçalho): a foto fica com a altura que sobra, então os dois botões aparecem em qualquer altura de celular e o próximo evento começa logo abaixo da dobra.
   - Até 639px, o texto de apoio mostra só a primeira frase (a segunda fica em `.hero__lead-more`) e a data do próximo evento fica empilhada.
2. **Próximas competições** (`#competicoes`): lista de provas, no padrão data | cartaz | texto | status.
   - O cartaz (`.poster`) é HTML: foto escurecida, filete de ouro em cima, modalidade em ouro e o nome curto da prova em caixa alta condensada. É decorativo (`aria-hidden`).
     - De 720 a 899px, fica numa coluna estreita ao lado do texto.
     - Até 719px, vira uma faixa baixa acima do título, só com a modalidade: o nome sai porque o título logo abaixo já o repete.
3. **Faixa "Faça parte"** (`#faca-parte`, verde-escura): dois caminhos lado a lado, "Quero ser atleta (CAC)" e "Quero me filiar ao clube". Os botões ficam alinhados na mesma linha.
4. **Nossa estrutura** (`#sobre`, no menu aparece como "O clube"), em duas camadas:
   - em cima, foto da sede, texto de abertura, lista de fatos e os botões "Agende uma visita" (leva a `#contato`) e "Estatuto social (PDF)";
   - embaixo, pista de ar e tiro ao prato em duas colunas iguais, as duas fotos em 4:3. Até 719px, viram um carrossel de arrastar, só com CSS (scroll-snap): a faixa vai até a borda da tela e o segundo cartão aparece pela metade.
   - Até 899px, o título abre a seção, antes da foto (áreas da grade em `.about`).
5. **Últimas notícias** (`#noticias`).
6. **Produtos do clube** (`#produtos`; no menu, "Produtos", entre "Notícias" e "Fale conosco", na ordem dos requisitos). Fica depois das notícias e antes da galeria, a pedido do usuário. Fundo branco (`.section--white`), e a galeria passou para o fundo `--geada`, para manter a alternância.
   - Vitrine de quatro produtos em colunas iguais (`.products` > `.product`): foto, nome, descrição e, na base, preço e disponibilidade (`.status`, como nas competições), tamanhos e o botão "Pedir pelo WhatsApp". Um fio separa a descrição da compra. Preço, tamanhos e botão ficam alinhados entre os vizinhos: produto sem tamanhos mostra "Tamanho único" na mesma altura.
   - O preço usa o número condensado das datas das competições.
   - Tamanhos são rádios (`.product__sizes`); tamanho esgotado fica `disabled` e riscado na diagonal.
   - O botão abre o WhatsApp da secretaria com a mensagem "Olá! Quero fazer um pedido: [produto], tamanho [X] ([preço])." O `main.js` monta o texto com `data-produto`, `data-preco` e o tamanho marcado. Sem JavaScript, o `href` do HTML já leva produto e preço.
   - De 720 a 1099px, duas colunas com foto recortada em 16:10. Até 719px, carrossel de arrastar como o das pistas, com o próximo cartão aparecendo.
   - O cartão tem `position: relative` de propósito: os textos só para leitor de tela (`.visually-hidden`) precisam ficar presos nele, senão escapam da rolagem do carrossel e alargam a página no celular.
7. **Galeria** (`#galeria`).
8. **Fale conosco** (`#contato`, fundo branco): contatos, mapa e "Como chegar".
9. **Rodapé**: navegação, modalidades, horário e o lema "Brasil acima de tudo! Deus acima de todos!".

## Sistema visual (manter)

- **Cores**, tiradas do brasão:
  - `--pinho` #1A4E3D;
  - `--pinho-900` #0E2C22 (hero, faixa, rodapé);
  - `--ouro` #E3A210 (botões principais e detalhes);
  - `--ouro-texto` #7F5A05 (ouro sobre fundo claro);
  - `--geada` #F1F3F0 (fundo da página);
  - `--liquen` #55635C (texto secundário);
  - `--linha` #D3DAD5.

  Não criar cores novas sem necessidade.
- **Fontes:** duas, do Google Fonts.
  - **Archivo** (variável) em quase tudo, mudando a largura:
    - nome no hero e `h2` de seção largos, com `font-stretch: var(--larga)` (125%), peso 800 e só a primeira letra maiúscula: firmes como o letreiro do brasão, sem gritar;
    - subtítulos (`h3`) e título do próximo evento com `var(--semi)` (84%), peso 700;
    - números de data, preços, cartazes das competições e lema do rodapé condensados (62–75%);
    - texto normal com largura 100%.
  - **Piazzolla** (`--font-marca`), serifada, só na marca do cabeçalho (650), ao lado do brasão. O usuário gosta dela ali, mas achou que no nome do hero e nos títulos ela passava "passeio na serra", não clube de tiro: não voltar a usá-la neles.

  A escala de tamanhos está em `--step-*`. Na Archivo larga, "Caminhos" mede cerca de 6 vezes o tamanho da letra: `--step-hero` acompanha a largura da coluna do hero para o nome não invadir a foto, e até 719px tem uma escala própria.
- **Forma:** raio de 2px (`--radius`) e fios finos (`--linha`) separando itens em listas. Nada de cards com sombra.
- **Seções:** alternam entre fundo `--geada` (`.section`) e branco (`.section--white`). As faixas escuras são o hero, a "Faça parte" e o rodapé.
- **Ícones:** sprite SVG inline no topo do `index.html`, usado com `<svg class="icon"><use href="#i-nome"/></svg>`.
- **Classes:** no estilo BEM (`bloco__elemento--variante`). Os comentários do CSS são em português, curtos, e explicam o porquê.
- **Elemento marcante:** a serra no fim do hero é o único momento ousado da página. Não repetir a serra (nem outros divisores de seção) no rodapé ou em outras faixas.
- **Movimento:** uma única entrada animada, no hero (a serra sobe junto), que respeita `prefers-reduced-motion`. Não espalhar animações nas outras seções.
- **Responsivo:** os pontos de quebra usados são 1279 (só o menu do cabeçalho), 1099, 899, 719, 639 e 479px. Todo layout novo precisa funcionar em 390px, sem rolagem lateral.
  - O menu completo, com a marca e a "Área do atleta", precisa de uns 1160px, e abaixo de 1280px recolhe no botão de menu. Um item novo no menu pede medir de novo.

## Textos

- Títulos e rótulos com só a primeira letra maiúscula. Caixa alta só nos cartazes das competições e no lema do rodapé. O nome do clube, no hero e no cabeçalho, vai escrito como se diz ("Caminhos da Neve"): o brasão ao lado já traz o nome em caixa alta.
- Botões dizem aonde levam: "Como obter o CR" e "Como se filiar", nunca dois "Saiba mais".
- Nos textos enviados pelo cliente, fazer só correções (ortografia, pontuação, unidades como "10 m"). Não reescrever.
- Textos curtos e diretos, sem enchimento nem dados inventados (altitude, número de atletas, "segurança em primeiro lugar", "tradição serrana" etc.).

## Fatos confirmados pelo clube

- Pistas outdoor para tiro esportivo ou tático.
- Pista indoor de tiro de ar de 10 m, com 5 baias e transportador de alvo automático.
- Tiro ao prato (trap americano), em estande coberto.
- Pista de **100 m** para tiro de precisão. Não usar 300 m: essa informação foi removida do site.
- Para ser sócio é preciso concluir o curso de tiro. O clube cuida da burocracia do CR para quem quer ser atleta CAC.

## Ainda são conteúdo provisório (confirmar antes de publicar)

- Todas as provas da lista de competições, as notícias e o próximo evento (18/10/2026).
- Todos os produtos: nomes, descrições, preços, tamanhos, disponibilidade e as fotos (`produto-*.jpg`, de banco), que devem virar fotos reais dos produtos. Os pedidos vão para o WhatsApp da secretaria, que também é provisório.
- Autorização do atleta que aparece na foto do hero (`atirador.jpg`), que dá para reconhecer mesmo de costas.
- Imagens:
  - as notícias, os cartazes e a galeria usam fotos de banco, não do clube. O ideal é trocar por fotos reais, principalmente na galeria ("Momentos do Caminhos da Neve").
- Contatos: telefones, e-mail, endereço (SC-114, km 28) e horário de atendimento.
- Links apontando para `#`:
  - "Como obter o CR";
  - "Como se filiar";
  - "Estatuto social (PDF)";
  - "Área do atleta";
  - "Calendário completo";
  - "Ver todos os produtos";
  - notícias e redes sociais.
- O texto do tiro ao prato tem uma frase só. Uma linha a mais (número de postos, dias de treino) equilibraria a coluna.

## Como conferir mudanças

Tirar screenshots com o Chrome headless:

```
"/c/Program Files/Google/Chrome/Application/chrome.exe" --headless=new --disable-gpu --hide-scrollbars \
  --virtual-time-budget=10000 --window-size=1440,7600 --screenshot=saida.png \
  "file:///C:/Users/Kaue%20Kluska/Documents/Prototipo/index.html"
```

No Windows, o Chrome headless não aceita janela com menos de ~500px de largura. Para testar em 390px, abrir o `index.html` dentro de um `<iframe width="390">` num HTML temporário e tirar a screenshot desse HTML. A faixa de foto do hero depende da altura da tela (`svh`): para avaliar o hero, usar um iframe com altura de celular (por exemplo 390×740), não um da altura da página inteira. O Python com Pillow está instalado e serve para recortar as imagens.

## Fluxo de trabalho

- Quando o usuário pedir opções, apresentar alternativas com prévia do layout em ASCII antes de construir.
- O usuário faz os próprios commits. Não commitar sem ele pedir.
