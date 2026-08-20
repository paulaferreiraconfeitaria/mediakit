# Media Kit · Paula Ferreira Confeitaria

Media kit em página única (`media-kit-paula.html`), pensado para ser enviado como **link** para marcas.
É um arquivo HTML autossuficiente: todo o CSS e o JavaScript estão dentro dele. A única dependência
externa são as fontes do Google Fonts (Playfair Display + Inter).

- Mobile-first, testado em **380px**, **768px** e **1200px**
- Gráficos em CSS e SVG puros, sem bibliotecas
- Não usa `localStorage` nem qualquer armazenamento de navegador
- Imprime bem em PDF (Ctrl+P → "Salvar como PDF", com "gráficos de plano de fundo" ativado)

---

## 1. Como abrir

Dê **duplo clique** no arquivo `media-kit-paula.html` — ele abre em qualquer navegador
(Chrome, Edge, Safari, Firefox). Não precisa de servidor nem de internet, exceto para
carregar as fontes (sem internet, o navegador usa fontes de sistema parecidas).

## 2. Como editar os dados

Abra o arquivo em um editor de texto (VS Code, Notepad++, ou o Bloco de Notas).
Todos os pontos que costumam mudar estão marcados com comentários `<!-- EDITÁVEL · ... -->`.
Use `Ctrl+F` e busque por `EDITÁVEL` para pular de um para o outro.

### Foto da criadora

**A foto precisa estar salva na mesma pasta do HTML, com o nome `paula.png`.**
Enquanto o arquivo não existir, o header mostra um retângulo tracejado avisando isso.

Use um **PNG recortado, com fundo transparente**, em formato retrato. A foto aparece
à esquerda do header, encostada na base da faixa vinho — sem recorte redondo.
Para usar outro nome ou formato, troque só o `src` (busque por `EDITÁVEL · FOTO DA CRIADORA`).

**Tamanho e posição** são controlados por 4 variáveis no bloco `:root`, no início do `<style>`:

| Variável      | O que faz                                    | Mobile | Tablet | Desktop |
|---------------|----------------------------------------------|--------|--------|---------|
| `--photo-w`   | largura da foto (altura acompanha)           | 300px  | 340px  | 620px   |
| `--photo-col` | coluna reservada para a foto (768px pra cima) | — | 74% de `--photo-w` | idem |
| `--photo-x`   | deslocamento horizontal (aceita negativo)    | 0      | 0      | 0       |
| `--photo-y`   | deslocamento vertical (aceita negativo)      | 0      | 0      | 0       |

**Como a foto se comporta em cada tamanho de tela:**

- **Desktop (1200px+)** — a foto tem 620px, o topo dela alinha com o topo do texto e
  ela **não estica o header**: quem manda na altura é o texto. O que sobra da foto
  para baixo fica escondido atrás da seção de números (o header tem `overflow:hidden`).
  Aumentar `--photo-w` deixa a foto maior sem aumentar o header — só muda onde ela é cortada.
  Para mostrar **mais** da foto, aumente o `padding-bottom` da regra `.hero-copy`.
- **Tablet (768–1199px)** — a foto tem 340px e fica encostada na base do header, inteira,
  sem corte (nesse tamanho o corte deixaria um vão vazio embaixo dela).
- **Mobile** — a foto aparece abaixo do texto, inteira, alinhada à esquerda.

Para testar valores ao vivo, abra o console do navegador (F12) e rode:

```js
document.documentElement.style.setProperty('--photo-w','520px')
```

Ajuste até gostar, anote os valores e troque no arquivo (ou peça o ajuste).
A coluna acompanha a largura da foto sozinha, então na maioria dos casos
basta mexer em `--photo-w`. O header tem `overflow:hidden`, então a foto
nunca invade as seções seguintes, mesmo com deslocamentos negativos.

**Por que a coluna é menor que a foto (o `0.74`):** neste PNG a silhueta da
Paula ocupa só **77% da largura da imagem** — os 23% da direita são
transparentes. A coluna reservada é 74% da largura da foto, então o texto
avança por cima dessa faixa vazia e fica encostado nela (sobra ~20px de
folga até o ombro). Quem permite isso é o `max-width:none` na regra
`.hero-photo` de 768px pra cima: sem ele a foto encolheria para caber na
coluna em vez de transbordar.

Para deixar o texto **ainda mais perto** (ou mais longe), mude só esse fator
no `:root`: `--photo-col:calc(var(--photo-w) * 0.74)`. Menor = mais perto;
abaixo de ~0.72 o texto começa a passar por cima do braço dela.
**Se trocar a foto, revise esse número** — ele depende de quanta
transparência a nova imagem tem do lado direito.

No mobile a foto aparece **abaixo** do texto. Para colocá-la acima, mude
`order:2` para `order:0` na regra `.hero-photo`.

### E-mail
Aparece em **dois lugares** (o botão dourado e a lista de contatos). Busque por
`contato@paulaferreiraconfeitaria.com` e substitua em todas as ocorrências
(no `href="mailto:..."` **e** no texto visível).

### WhatsApp comercial
Busque por `EDITÁVEL · WHATSAPP`. Troque a linha do placeholder por um link real:

```html
<a href="https://wa.me/5563999999999"><span class="ico" aria-hidden="true">&#9990;</span> (63) 99999-9999</a>
```

O número no `wa.me` vai sem espaços, parênteses ou traços: `55` + DDD + número.

### Instagram
Busque por `paulaferreiraconfeitariaa` — aparece no topo (hero) e na área de contato.

### Valores da tabela
Busque por `EDITÁVEL · TABELA DE VALORES`. Cada serviço é um `<li class="price-row">`:

```html
<li class="price-row" data-reveal>
  <span class="svc">1 Reels</span>
  <span class="val">R$ 700 – 1.200</span>
</li>
```

Para mudar **qual item fica destacado em vinho**, mova a classe `featured` e a linha
`<span class="badge">Mais pedido</span>` para outro `<li>`.

### Números e gráficos
- **Cards de destaque**: busque `EDITÁVEL · NÚMEROS DE DESTAQUE`. O `<small>` é o sufixo
  em fonte menor (`mil`, `mi`, `%`).
- **Barras (idade, cidades, formatos)**: cada barra tem dois valores — o rótulo visível
  (`<span class="v">40,0%</span>`) e a largura da barra (`style="--w:100%"`).
  A largura é **proporcional ao maior valor do grupo**, não o número em si.
  Exemplo: se o maior é 40% e você quer desenhar 25,8%, use `--w:64.5%` (25,8 ÷ 40 × 100).
- **Donut de gênero**: o atributo `style="--dash:17"` controla a fatia preenchida.
  A conta é `339.3 − (porcentagem ÷ 100 × 339.3)`. Para 95% → `17`. Para 90% → `34`.

### Textos e cores
Os textos ficam direto no HTML, na ordem em que aparecem na página.
Para mudar a paleta, edite as variáveis no início da tag `<style>`, no bloco `:root`:

| Variável       | Uso                                  | Valor atual |
|----------------|--------------------------------------|-------------|
| `--wine`       | fundo das faixas escuras             | `#4A1420`   |
| `--cream`      | fundo principal                      | `#F4EDE3`   |
| `--gold`       | destaques sobre vinho, botão CTA     | `#D97A34`   |
| `--gold-deep`  | números grandes sobre fundo claro    | `#C4661F`   |
| `--ink`        | texto sobre creme                    | `#3A2018`   |

> As variações `--gold-deep` e `--gold-ink` existem para garantir contraste de leitura
> (acessibilidade) sobre o fundo claro. Se mudar o dourado, mude as três juntas.

## 3. Como publicar

Qualquer opção abaixo gera um link para enviar às marcas. Todas são gratuitas:

**Netlify Drop** (mais rápido, sem cadastro para testar)
1. Acesse `app.netlify.com/drop`
2. Arraste a pasta com o HTML (e a foto, se houver)
3. Copie o link gerado

**GitHub Pages**
1. Crie um repositório e envie os arquivos
2. Em *Settings → Pages*, escolha a branch `main` e a pasta raiz
3. O link fica em `https://SEU-USUARIO.github.io/NOME-DO-REPO/media-kit-paula.html`

**Vercel** — importe o repositório em `vercel.com/new` e publique.

**Hospedagem própria** — envie o arquivo por FTP para a pasta pública do site.

> Dica: se renomear o arquivo para `index.html`, o link fica mais curto e limpo
> (`seusite.com` em vez de `seusite.com/media-kit-paula.html`).

Depois de publicar, atualize a URL na tag `og:url` (no topo do HTML, bloco
`EDITÁVEL · COMPARTILHAMENTO EM REDES`) para o preview aparecer certo no
WhatsApp e no Instagram.

## 4. Observação sobre os dados

O bloco "Momento de ascensão" trata o crescimento de seguidores de forma **qualitativa**
("crescimento acelerado", "perfil em ascensão"), sem exibir porcentagem exata — o dado
não está verificado. Os demais números são os do período informado; ao atualizar as
métricas, vale registrar a janela de datas usada, caso a marca pergunte.
