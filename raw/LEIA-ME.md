# Como usar a pasta `raw/`

Esta pasta é o ponto de entrada de todo conhecimento que vai alimentar a wiki. Coloque aqui qualquer arquivo — o agente vai processar, classificar e compilar em páginas wiki estruturadas quando você rodar `/wiki-ingest`.

---

## Onde colocar cada tipo de arquivo

Usar as subpastas certas melhora a precisão da IA na classificação. Mas não é obrigatório — se estiver em dúvida, use `inbox/`.

| Subpasta | O que vai aqui |
|---|---|
| `inbox/` | Qualquer coisa — quando não souber onde colocar |
| `interviews/` | Notas ou transcrições de entrevistas com usuários |
| `clippings/` | Artigos, relatórios de mercado, posts copiados |
| `data/` | Exports de analytics, CSVs, screenshots de dashboards |
| `competitor/` | Screenshots de produto, pricing, product tours |
| `ideas/` | Notas rápidas, pensamentos soltos, voice memos |

---

## Formatos suportados

| Formato | O que acontece |
|---|---|
| `.md`, `.txt` | Processado diretamente — ideal |
| `.csv`, `.xlsx` | Processado diretamente |
| `.docx` | Processado diretamente |
| `.pdf` (texto) | Processado diretamente |
| `.pdf` (imagens) | Processado parcialmente — conteúdo visual pode ser perdido |
| `.pptx` | Processado parcialmente — imagens nos slides serão ignoradas |
| `.jpg`, `.png` | Processado se a IA tiver visão — pode ser impreciso |
| `.mp3`, `.mp4`, `.m4a`, `.wav`, `.mov` | ⚠️ **Requer conversão antes do ingest** — veja abaixo |

---

## Arquivos de áudio e vídeo

A wiki não processa áudio ou vídeo diretamente. Para incluir o conteúdo dessas gravações:

1. **Transcreva com qualquer ferramenta** — Whisper, Otter.ai, MacWhisper, Descript, ou qualquer serviço online.
2. **Salve o transcript como `.txt`** na mesma subpasta (ex: `interviews/reuniao-produto-abril.txt`).
3. **Rode `/wiki-ingest`** — o arquivo de texto será processado normalmente.

O arquivo de áudio/vídeo original pode permanecer na pasta para referência — o agente ignora os formatos não suportados após avisar.

---

## Dicas de tamanho

Arquivos muito grandes aumentam o custo de processamento e podem reduzir a qualidade:

- Áudio: até **90 minutos** de gravação por arquivo
- PDF: até **80 páginas**
- PPTX: até **40 slides**
- CSV/XLSX: sem limite prático

Arquivos maiores serão processados com um aviso, mas o resultado pode ser parcial.

---

## O que acontece quando você roda `/wiki-ingest`

O agente:
1. Escaneia todos os arquivos não processados em `raw/`
2. Classifica o conteúdo de cada um
3. Cria ou atualiza páginas wiki em `wiki/`
4. Registra o resultado no `wiki/log.md`
5. Marca os arquivos processados internamente (não os move nem apaga)

Arquivos que precisam de conversão ficam como `pending_conversion` no log — com instruções do que fazer.

---

*Este arquivo é mantido pelo sistema. Não remova.*
