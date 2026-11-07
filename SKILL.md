---
name: catalogo-cardapio
description: Referência de padrão de catalogação e estrutura de cardápio/catálogo de produtos de delivery (página da loja, destaques, categorias, itens, variações de tamanho, combos, opções, preço promocional "de/por", "a partir de", descrições, imagens). Use esta skill SEMPRE que a tarefa envolver cardápio, menu, catálogo, categorias de produtos, combos, variações, promoções no preço do item, modelagem de produtos, organização de itens de loja ou restaurante, telas de listagem de produtos, ou quando o usuário mencionar iFood, delivery, açaí, ou pedir para "estruturar como um bom cardápio". Use também para auditar um catálogo existente ou decidir qual padrão de catalogação se encaixa melhor em um caso. Complementa skills de engenharia: esta skill decide COMO o catálogo deve ser organizado; as skills de engenharia decidem COMO implementar.
---

# Catálogo de Cardápio — padrão de referência

Esta skill dá noção de **boa estrutura de catalogação**. Ela não escreve código sozinha: define o modelo mental, as convenções e os critérios de qualidade que as outras skills (engenharia, front-end, banco de dados etc.) devem seguir quando o assunto for cardápio ou catálogo.

Ordem de uso: primeiro esta skill (decidir a estrutura), depois as skills de engenharia (implementar).

A referência usada aqui é a página completa de uma loja de açaí no iFood (seção 4). Ela é um exemplo de estrutura bem organizada, mas também tem falhas; as falhas estão listadas de propósito (seção 11) para a IA não copiá-las.

---

## 1. Modo de observação (iFood aberto em segundo plano)

O iFood pode estar aberto no computador do usuário e a IA pode ter acesso remoto à tela. Use isso **como referência visual de estrutura**, não como fonte de conteúdo.

Regras:

- **Somente leitura.** Observe, tire screenshot, leia o que está visível. Não clique em "adicionar", não finalize pedido, não altere endereço, conta, carrinho ou formas de pagamento, não faça login/logout.
- **Aprenda a estrutura, não copie o conteúdo.** Textos, fotos, nomes de produtos e preços de um restaurante são do restaurante. Extraia o padrão (hierarquia, nomenclatura, regras de preço, layout) e aplique com conteúdo do próprio projeto.
- **Sem coleta em massa.** Não percorra dezenas de lojas para montar uma base. Uma a três lojas do mesmo nicho costumam bastar para entender o padrão.
- **Ignore dados pessoais** que apareçam na tela (endereço de entrega, nome, carrinho, pedidos anteriores). Não os repita em respostas, arquivos ou código.
- **Diferencie o que viu do que inferiu** (seção 4).
- Se a tela do iFood não estiver acessível, trabalhe com os screenshots/descrições que o usuário fornecer e diga que está assumindo isso.

Como observar, em ordem:
1. Percorra a página de cima a baixo e anote a **ordem das seções** (identidade da loja, destaques, categorias).
2. Para cada categoria, anote nome, quantidade de itens e o que os itens têm em comum.
3. Compare itens "irmãos" (mesmo produto em tamanhos diferentes) e itens "primos" (sabores do mesmo tamanho) para descobrir como a variação é modelada.
4. Anote: nomenclatura, campos visíveis, regra de preço e inconsistências.

---

## 2. Quando a skill se aplica

Aplique quando houver qualquer um destes:
- Criar ou migrar um cardápio/catálogo (planilha, banco, API, tela).
- Decidir entre "um item com seletor de tamanho" e "um item por tamanho".
- Definir como representar combos, adicionais, complementos, preço "a partir de" e preço promocional.
- Padronizar nomes e descrições de itens.
- Auditar um cardápio existente e apontar o que está mal estruturado.
- Desenhar a página da loja, o card de produto ou a listagem por categoria.

Não aplique para: pagamento, logística de entrega, avaliações, cupons (a menos que afetem diretamente o preço exibido do item).

---

## 3. Hierarquia do catálogo

```
Loja                       (identidade, status, horário, nota)
├── Destaques              (curadoria: aponta para itens que já existem)
└── Categoria              (seção com título, ordem e intenção)
    └── Item               (o que o cliente vê como um card)
        └── Grupo de opções    (escolhas: sabor, adicional, camada, etc.)
            └── Opção          (acréscimo de preço, disponibilidade)
```

Princípios:
- A **categoria** responde "o que eu quero agora?".
- O **item** responde "qual é exatamente este produto?".
- O **grupo de opções** responde "como eu personalizo?".
- **Destaques não duplicam itens**: apontam para eles.

---

## 4. Padrão de referência (página completa de uma loja de açaí)

### 4.1 Observado diretamente

**Ordem da página, de cima para baixo**
1. Cabeçalho global da plataforma (navegação, busca, endereço, sacola). Não faz parte do catálogo da loja.
2. Banner da loja com status: "Aceita agendamento · Abre amanhã às 14:15".
3. Identidade: logo, nome, selo, nota (4,9) e link "Ver mais".
4. Barra de ferramentas: busca no cardápio, modo (Entrega) e janela de entrega com taxa (ex.: "Sábado 15:30–16:00 · R$ 19,99").
5. **Destaques**: carrossel horizontal com 4 cards visíveis.
6. Categorias, na ordem abaixo.

**Destaques**
- Quatro itens que também existem nas categorias abaixo: Duo 480ml, Duo 360ml, Açaí Morango Crocante 480ml, Açaí Banana Brasileirinho 480ml.
- Todos têm preço promocional (de/por). Ou seja, o carrossel funciona como vitrine de ofertas.
- Card vertical: imagem em cima, depois nome, descrição de 2 linhas, "Serve N pessoas" (quando aplicável) e preço na base.

**Categorias, na ordem em que aparecem**

| # | Categoria | Itens | Padrão do grupo |
|---|---|---|---|
| 1 | Potes de açaí mais recheados da cidade! | 1 ("Favorite nossa loja!", R$ 50,00) | Nome de slogan; o conteúdo não é um produto de comida |
| 2 | Monte seu pote! | 1 ("Monte seu açaí!", a partir de R$ 28,99) | Produto montável; 360ml, 2 camadas, 3 acompanhamentos e 1 fruta |
| 3 | Bragas do Mês! | 4 sabores de 480ml | Preço promocional fechado (de/por); tamanho no nome; sem "Serve" |
| 4 | Campeões De Vendas! | 4 | Individuais, "Serve 1 pessoa", "A partir de" |
| 5 | Autorais do Braga! | 6 | Individuais, "Serve 1 pessoa", "A partir de" |
| 6 | Duo! | 3 (360ml, 480ml, 720ml) | Combo para 2, um item por tamanho |
| 7 | Combos pra reunir a galera! | 4 (Trio e Família, 360ml e 480ml) | Combo para 3 e 4, um item por tamanho |
| 8 | Bebidas | 1 (Shake de Açaí 500ml, R$ 24,00) | Preço fechado, descrição com ingredientes |

**Preços dos combos**

| Item | Serve | Preço atual | Preço original | Por unidade |
|---|---|---|---|---|
| Duo 360ml | 2 | A partir de R$ 42,00 | R$ 54,90 | R$ 21,00 |
| Duo 480ml | 2 | A partir de R$ 49,00 | R$ 64,90 | R$ 24,50 |
| Duo 720ml | (não exibe) | A partir de R$ 59,00 | (não exibe) | R$ 29,50 |
| Trio 360ml | 3 | A partir de R$ 63,00 | — | R$ 21,00 |
| Trio 480ml | 3 | A partir de R$ 73,50 | — | R$ 24,50 |
| Família 360ml | 4 | A partir de R$ 84,00 | — | R$ 21,00 |
| Família 480ml | 4 | A partir de R$ 98,00 | — | R$ 24,50 |

Em todos os combos, o preço por unidade é constante dentro do tamanho (R$ 21,00 / R$ 24,50 / R$ 29,50): o preço do combo é `unidade × quantidade`, sem desconto progressivo. O único desconto visível é o de/por do Duo.

**Anatomia do card (listagem por categoria)**
1. Nome em uma linha.
2. Descrição curta, truncada em 2 linhas com reticências.
3. Metadado com ícone: "Serve 1 pessoa" (ícone de uma pessoa) ou "Serve N pessoas" (ícone de grupo).
4. Preço na base do card. Três variações:
   - `R$ 24,00` (fechado),
   - `A partir de R$ 26,90` (varia conforme escolhas),
   - `R$ 29,99` em verde + `R$ 60,00` riscado (promoção, preço fechado). Com opções, vira `A partir de R$ 42,00` em verde + preço original riscado.
5. Foto à direita, proporção próxima de 4:3, cerca de um terço da largura do card.
6. Grade de 2 colunas em tela larga; a última linha pode ficar com 1 card.

**Nomenclatura**
- Combos: `[Apelido] - [Qtd]x Açaí de [Tamanho]!` (ex.: "Trio - 3x Açaí de 480ml!").
- Sabores individuais: `Açaí [Nome do sabor]` (ex.: "Açaí Tradicional", "Açaí Ninhotella"). Sabores autorais usam trocadilho com o ingrediente (Paçocatella, Morangotella).
- Itens promocionais do mês levam o tamanho no nome (`... 480ml`); os individuais de linha não.
- Tamanhos usados: 360ml, 480ml, 720ml (açaí) e 500ml (shake).
- Descrições de sabor listam os ingredientes principais e usam tom de marca ("combinação dos deuses", "Nosso saboroso açaí...").

**Imagens**
- Fotos de produto com fundo e iluminação padronizados por categoria: fundo roxo para os individuais, madeira para os combos. Isso dá identidade visual ao cardápio.
- Itens irmãos (360ml, 480ml, 720ml do mesmo combo) compartilham a mesma foto.

### 4.2 Inferido (confirmar antes de tratar como regra)

- **O cardápio usa dois padrões de tamanho ao mesmo tempo.**
  - Combos (Duo, Trio, Família): um item por tamanho, porque tamanho e quantidade definem o produto.
  - Sabores individuais ("Açaí Crocante", "Açaí Tradicional"): sem tamanho no nome e com "A partir de", o que sugere seletor de tamanho dentro do item, com o preço exibido sendo o do menor tamanho.
- **"A partir de" indica preço que muda conforme as escolhas** (tamanho, sabor, adicionais). Preço fechado aparece quando nada muda o valor.
- **"Bragas do Mês" é uma categoria sazonal/promocional**: itens fixos em 480ml, com preço riscado, rotativos por mês.
- **"Monte seu pote!" é um item configurável**, com regras do tipo "3 acompanhamentos + 1 fruta" (grupos de opções com mínimo e máximo).
- **Destaques são curadoria manual ou por regra** (por exemplo, "itens em promoção"); não é uma categoria própria com itens próprios.
- **A categoria #1 e o item "Favorite nossa loja!" parecem uma ação de engajamento**, não um produto. Não há como confirmar só pelo print.

---

## 5. Modelo de dados recomendado

Ponto de partida; adapte à stack do projeto (SQL, NoSQL, planilha, API).

```ts
// Valores monetários sempre em centavos (inteiro). Nunca float nem string.

type Loja = {
  id: string;
  nome: string;
  logoUrl?: string;
  nota?: number;
  status: { aberta: boolean; proximaAbertura?: string; aceitaAgendamento: boolean };
  modos: ("entrega" | "retirada")[];
  destaques: string[];     // ids de Item (referências, não cópias)
  categorias: Categoria[];
};

type Categoria = {
  id: string;
  nome: string;            // pode ter tom de marca: "Combos pra reunir a galera!"
  ordem: number;
  descricao?: string;
  ativa: boolean;
  sazonal?: { inicio: string; fim: string }; // ex.: "Bragas do Mês"
};

type Item = {
  id: string;
  categoriaId: string;
  nome: string;
  descricao: string;       // 1–2 linhas úteis; ingredientes em produto preparado
  imagemUrl?: string;
  precoBaseCentavos: number;
  precoOriginalCentavos?: number; // quando existe, exibe "de/por"
  grupos: GrupoOpcao[];    // vazio = preço fechado
  serve?: number;          // nº de pessoas (1 = individual)
  tamanho?: { valor: number; unidade: "ml" | "g" | "kg" | "un" };
  tags?: string[];         // "novo", "sem-lactose", "vegano"
  alergenos?: string[];
  disponivel: boolean;
  ordem: number;
  grupoDeVariacaoId?: string; // liga itens irmãos (360ml ↔ 480ml ↔ 720ml)
};

type GrupoOpcao = {
  id: string;
  nome: string;            // "Escolha o tamanho", "Escolha 3 acompanhamentos", "Fruta"
  minimo: number;          // 0 = opcional
  maximo: number;
  opcoes: Opcao[];
};

type Opcao = {
  id: string;
  nome: string;
  acrescimoCentavos: number; // 0 = sem custo
  disponivel: boolean;
};
```

**Campos derivados (não armazenar):**

```ts
// menor preço realmente pagável: base + menor escolha obrigatória de cada grupo
const precoMinimo = (item: Item) =>
  item.precoBaseCentavos +
  item.grupos.reduce((soma, g) => soma + menorCustoObrigatorio(g), 0);

// "A partir de" quando o valor final pode variar conforme as escolhas
const exibeAPartirDe = (item: Item) =>
  item.grupos.some(g => g.opcoes.some(o => o.acrescimoCentavos > 0));

// promoção ativa quando há preço original maior que o atual
const emPromocao = (item: Item) =>
  item.precoOriginalCentavos !== undefined &&
  item.precoOriginalCentavos > precoMinimo(item);

// preço por unidade de um combo (serve como verificação de coerência)
const precoPorUnidade = (item: Item, qtd: number) => precoMinimo(item) / qtd;
```

Por que derivar: campos digitados à mão ("a partir de", "em promoção") desatualizam e passam a mostrar um preço que o cliente não consegue pagar.

**`grupoDeVariacaoId`:** mantém cada tamanho de combo como item próprio (preço e porção próprios) sem perder o vínculo entre eles. Serve para relatórios, para trocar a descrição de todos de uma vez e para um futuro seletor de tamanho.

---

## 6. Regras de decisão

### Tamanho: item separado ou seletor?
Use a regra que o próprio cardápio de referência sugere:
- **Item separado** quando tamanho ou quantidade **define o produto** (Duo, Trio, Família; "serve 2", "serve 3") ou quando preço, foto e porção mudam de forma relevante.
- **Seletor dentro do item** quando o produto é o mesmo e só muda o acréscimo (sabor individual em 360ml/480ml/720ml). O card mostra "A partir de" com o menor tamanho.
- **Tamanho fixo no nome** para itens promocionais ou sazonais vendidos em um único tamanho.
- Em dúvida, prefira item separado + `grupoDeVariacaoId`: é mais fácil juntar depois do que separar.

### Combo
- Defina `serve`, quantidade e tamanho no nome e no metadado.
- Registre a regra de preço: soma simples (como no print), desconto fixo ou percentual. Valide com `precoPorUnidade`.
- Itens escolhíveis do combo entram como grupos de opções com `minimo = maximo = 1`.

### Preço
- **Fechado** quando nenhuma escolha altera o valor.
- **"A partir de"** quando existe qualquer acréscimo possível. Mostre o **mínimo realmente pagável**.
- **De/por** somente com `precoOriginalCentavos` real e vigente. O preço atual aparece em destaque (verde) e o original riscado ao lado.

### Destaques
- Máximo de poucos itens (o carrossel mostra 4 por vez).
- Referenciam itens existentes; nunca criam item novo.
- Critério claro e revisável: promoção ativa, mais vendidos ou lançamento.

### Ordem das categorias
1. Destaques (curadoria).
2. Ofertas sazonais ou do mês.
3. Produtos principais, do mais pedido para o menos pedido (campeões antes dos autorais, por exemplo).
4. Produto montável ("monte o seu"), perto dos produtos principais.
5. Combos para grupos, em ordem crescente de quantidade (Duo, Trio, Família).
6. Acompanhamentos e adicionais.
7. Bebidas e sobremesas por último.

---

## 7. Convenções de texto

**Nome do item**
- Combo: `[Apelido] - [Qtd]x [Produto] de [Tamanho]`.
- Sabor individual: `[Produto] [Nome do sabor]`.
- Tamanho com unidade colada ao número: `360ml`, `720ml`, `1kg`.
- Um único padrão de pontuação e de caixa em todo o cardápio. Se os combos usam "!" no fim, todos os combos usam.
- Sem CAIXA ALTA corrida e sem emoji no lugar de informação.

**Descrição**
- Primeira frase: o que é e o que vem. Segunda: o gancho.
- Produto preparado: liste os ingredientes principais.
- Cada descrição deve ser compreensível nas 2 primeiras linhas, porque o card trunca.
- Detalhes longos (camadas, regras de montagem) ficam na tela do item.
- Alérgenos relevantes (leite, glúten, amendoim, castanhas) como dado estruturado, não só em texto.

**Nome de categoria**
- Curto e com intenção clara. Tom de marca é permitido (como "Bragas do Mês!"), desde que o cliente entenda o que há dentro sem abrir.
- Se a categoria tem nome de slogan, o conteúdo precisa cumprir a promessa do nome.

---

## 8. Convenções de UI

**Card de listagem**
- Horizontal: texto à esquerda, imagem à direita.
- Hierarquia: nome > preço > metadado > descrição.
- Nome em 1 linha; descrição em 2 linhas com reticências.
- Preço fixado na base do card, para alinhar cards de alturas de texto diferentes.
- Imagem com proporção fixa (4:3), `object-fit: cover`, com placeholder se não houver foto.
- Todo o card é clicável.

**Card de destaque (carrossel)**
- Vertical: imagem em cima, texto embaixo, mesma lógica de preço.
- Setas de navegação nas laterais; mostrar parte do próximo card ajuda o usuário a perceber que há mais.

**Preço**
- Fechado e "a partir de": cor neutra.
- Promoção: preço atual em cor de destaque (verde), original riscado e em tom mais fraco.

**Página**
- Ordem: status da loja → identidade → busca/modo/janela de entrega → destaques → categorias.
- Grade de 2 colunas em desktop, 1 coluna em mobile, espaçamento constante.
- Considere barra de categorias fixa e âncoras quando houver mais de 6 categorias (o print tem 8 categorias e nenhuma navegação rápida visível).
- Item indisponível continua visível, esmaecido e sinalizado.

---

## 9. Fluxo de trabalho

1. **Entender o contexto:** nicho, quantidade de itens, onde o catálogo vai viver (planilha, banco, tela).
2. **Observar a referência** (seção 1) e registrar o que foi visto.
3. **Mapear para o modelo** (seção 5): loja, destaques, categorias, itens, grupos, regras de preço.
4. **Decidir os pontos de variação e preço** (seção 6) e justificar em uma linha cada.
5. **Padronizar os textos** (seção 7).
6. **Passar o checklist** (seção 10).
7. **Entregar** no formato pedido e só então acionar as skills de engenharia.

Se o pedido for apenas auditoria, pule os passos 5 e 7 e entregue a lista de problemas.

---

## 10. Checklist de qualidade

- [ ] Toda categoria tem nome claro, ordem definida e pelo menos um item ativo.
- [ ] O conteúdo de cada categoria cumpre a promessa do nome.
- [ ] Nenhum "item" é, na verdade, uma ação (favoritar, avisar) com preço.
- [ ] Nomes seguem o mesmo molde, a mesma pontuação e a mesma caixa.
- [ ] Tamanhos têm unidade e formato consistentes.
- [ ] Preço em centavos inteiros.
- [ ] "A partir de" e "em promoção" são derivados, não digitados.
- [ ] De/por usa preço original real e vigente.
- [ ] Preço por unidade dos combos é coerente (ou o desconto está documentado).
- [ ] Itens irmãos têm o mesmo conjunto de metadados ("Serve N", promoção) e estão ligados por `grupoDeVariacaoId`.
- [ ] Destaques apontam para itens existentes.
- [ ] Descrições dizem algo que o nome não diz e funcionam truncadas em 2 linhas.
- [ ] Ingredientes e alérgenos estão estruturados.
- [ ] Todo item tem imagem ou placeholder, com padrão visual por categoria.
- [ ] Grupos de opções têm `minimo <= maximo`.
- [ ] Indisponibilidade não quebra a listagem.

---

## 11. Anti-padrões (vários aparecem na própria referência)

- **Item irmão com metadado faltando.** No Duo, os itens de 360ml e 480ml mostram "Serve 2 pessoas" e preço original; o de 720ml não mostra nenhum dos dois.
- **Categoria de slogan com conteúdo que não bate com o nome.** "Potes de açaí mais recheados da cidade!" contém só "Favorite nossa loja!", que não é um pote.
- **Ação de engajamento catalogada como produto**, com preço (R$ 50,00). Isso polui busca, relatórios e ticket médio.
- **Categorias com um único item** (Monte seu pote!, Bebidas) sem plano de crescimento. Se for intencional, tudo bem; se não, agrupe.
- **Inconsistência de caixa e pontuação** ("Campeões De Vendas!" com "De" maiúsculo; "Duo!" com exclamação no título e "Bebidas" sem).
- **Tamanho às vezes no nome, às vezes escondido em opção**, sem regra documentada.
- **Oito categorias sem navegação rápida** em uma página longa.
- **"A partir de" manual**, que desatualiza.
- **Preço em texto** guardado como string.
- **Categoria "Outros" ou "Diversos"** acumulando itens sem lar.
- **Combo sem regra de preço registrada**, que vira dúvida a cada reajuste.
- **Mesmo produto duplicado sem vínculo** entre as variações.
- **Descrições copiadas de outro estabelecimento**: risco de direitos autorais e perda de identidade da loja.
- **Fotos de proporções diferentes** quebrando a grade.
- **Mais de dois níveis de opção** (grupo dentro de grupo dentro de grupo).

---

## 12. Formato de saída

Quando o usuário pedir uma proposta de estrutura, responda nesta ordem:

1. **Resumo do padrão escolhido** (2–4 linhas), com a justificativa.
2. **Árvore do catálogo** (loja → destaques → categorias → itens → grupos), em texto.
3. **Decisões de variação e preço**, uma linha cada.
4. **Modelo de dados** adaptado à stack do projeto.
5. **Pendências e inferências a confirmar** com o usuário.

Quando for auditoria, entregue uma tabela com: problema, onde ocorre, impacto e correção sugerida.

Quando a observação vier do iFood aberto, indique qual loja/tela serviu de base e marque cada achado como **observado** ou **inferido**.
