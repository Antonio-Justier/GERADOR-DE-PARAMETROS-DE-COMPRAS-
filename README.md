# Parâmetros de Compras

Gerador web de PDF para controle de estoque e parâmetros de compra. Desenvolvido para o **El Dourado by Sapore** com o objetivo de simplificar o processo do estoquista ao percorrer o estoque registrando produtos, quantidade atual em mãos e quantidade sugerida para compra.

O resultado é um PDF pronto para ser impresso, assinado ou enviado por e-mail aos responsáveis.

---

## Funcionalidades

- Interface web em tema claro, responsiva (desktop e mobile).
- Adição e remoção dinâmica de linhas de produto.
- Geração de PDF em formato A4 com layout padronizado:
  - Logo da empresa no canto superior direito
  - Título `PARAMETROS DE COMPRAS` no topo
  - Cabeçalho da tabela em cinza (`#898989`) com colunas `PRODUTO | QUANT. ATUAL | COMPRAR`
  - Apenas as linhas dos produtos efetivamente preenchidos, sem linhas em branco extras
- Campos vazios em linhas preenchidas parcialmente são substituídos automaticamente por `-` no PDF.
- Linhas totalmente em branco são ignoradas no documento final.
- Contador de linhas preenchidas em tempo real.
- Nome do arquivo PDF inclui a data de geração: `parametros-de-compras_DD-MM-AAAA.pdf`.
- Funciona 100% offline — logo embutida em base64 e bibliotecas carregadas via CDN.

---

## Como usar

1. Abra o arquivo `gerador-parametros-compras.html` no navegador.
2. Preencha cada linha com:
   - **Produto** — nome do item
   - **Quant. atual** — quantidade encontrada no estoque
   - **Comprar** — quantidade sugerida de compra
3. Use o botão **+** verde para adicionar novas linhas.
4. Use o botão de **lixeira** vermelha para remover uma linha.
5. Clique em **Gerar PDF** para baixar o documento.

> Dica: campos deixados em branco aparecem como `-` no PDF, então não é obrigatório preencher todas as colunas em todas as linhas. Linhas completamente vazias são descartadas.

---

## Tecnologias

- **HTML5 + CSS3** — estrutura e estilo
- **JavaScript (Vanilla)** — sem frameworks, leve e rápido
- **[jsPDF](https://github.com/parallax/jsPDF) 2.5.1** — geração do PDF no navegador
- **[jsPDF AutoTable](https://github.com/simonbengtsson/jsPDF-AutoTable) 3.8.2** — renderização da tabela no PDF

Ambas as bibliotecas são carregadas via CDN (cdnjs).

---

## Executando localmente

Não há build, dependências de Node ou backend. Basta:

```bash
git clone https://antonio-justier.github.io/GERADOR-DE-PARAMETROS-DE-COMPRAS-/
cd parametros-de-compras
```

E abrir `gerador-parametros-compras.html` no navegador (Chrome, Firefox, Edge ou Safari).

Para servir via HTTP localmente (opcional), você pode usar Python:

```bash
python3 -m http.server 8080
```

E acessar `http://localhost:8080/gerador-parametros-compras.html`.

---

## Estrutura do projeto

```
parametros-de-compras/
├── gerador-parametros-compras.html   # Aplicação completa (HTML + CSS + JS + logo embutida)
└── README.md
```

Toda a lógica, estilo e a logo da empresa estão contidos em um único arquivo HTML, facilitando a distribuição e o uso offline.

---

## Paleta de cores

| Elemento | Cor | Hex / RGB |
|---|---|---|
| Cabeçalho da tabela (site e PDF) | Cinza | `#898989` / `rgb(137, 137, 137)` |
| Texto do cabeçalho | Branco | `#ffffff` |
| Botão primário "Gerar PDF" | Cinza escuro | `#4a4a4a` |
| Botão hover | Cinza médio | `#5a5a5a` |
| Borda de foco dos inputs | Cinza | `#898989` |
| Botão remover (lixeira) | Vermelho | `#c4424c` |
| Botão adicionar (+) | Verde | `#2ea76b` |
| Fundo da página | Branco | `#ffffff` |

---

## Personalização

### Trocar a logo

A logo está embutida em base64 no atributo `src` da tag `<img id="logo">`. Para trocá-la:

1. Converta sua imagem para base64 (qualquer conversor online, ou via terminal):
   ```bash
   base64 -w 0 sua-logo.jpg
   ```
2. Substitua o conteúdo do `src` no HTML por `data:image/jpeg;base64,<seu_base64>`.
3. Se a imagem nova tiver outra proporção, ajuste a constante de proporção no PDF dentro da função `generatePDF()`:
   ```javascript
   const logoW = 42; // mm
   const logoH = logoW * (163 / 539); // troque 163/539 pela proporção altura/largura da sua logo
   ```

### Mudar a cor do cabeçalho

**No site** (`.table-header` no CSS):
```css
background: #898989;
color: #ffffff;
```

**No PDF** (dentro de `doc.autoTable` → `headStyles`):
```javascript
fillColor: [137, 137, 137],  // RGB
textColor: [255, 255, 255],
```

### Reposicionar a logo no PDF

A logo é desenhada antes da tabela, no canto superior direito. Para mover, edite as variáveis `marginRight` e `marginTop` dentro de `generatePDF()`:
```javascript
const marginRight = 14;
const marginTop = 8;
```

---

## Roadmap / Ideias futuras

- [ ] Campo de data e nome do estoquista no cabeçalho do PDF
- [ ] Campo de setor/loja
- [ ] Importação de produtos a partir de CSV
- [ ] Histórico de gerações salvo localmente
- [ ] Coluna adicional de unidade (un, kg, cx, etc.)
- [ ] Modo "auditoria": destacar diferenças entre Quant. atual e Comprar

---

## Histórico de mudanças

- **v1.2** — Cabeçalho do PDF e do site agora em cinza (`#898989`). Logo movida para o canto superior direito do PDF. Removidas as linhas em branco extras: o PDF contém apenas as linhas dos produtos preenchidos.
- **v1.1** — Tema claro com logo da empresa no topo do site.
- **v1.0** — Versão inicial: tema escuro, geração de PDF com cabeçalho laranja.

---

## Licença

Uso interno — El Dourado by Sapore.
