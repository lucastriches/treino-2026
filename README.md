# Treino 2026

App de execução do plano de treino 2026 (4 treinos + bloco goleiro). Roda no navegador,
funciona offline depois da primeira abertura e pode ser instalado na tela inicial do Android.

## Arquivos

| Arquivo | Papel |
|---|---|
| `index.html` | O app inteiro: HTML, CSS e JS embutidos, sem dependências externas |
| `manifest.json` | Metadados do PWA (nome, cores, ícones, modo standalone) |
| `sw.js` | Service worker — cache do app para uso offline |
| `icon-192.png` / `icon-512.png` | Ícones do app |
| `icon-maskable-512.png` | Ícone adaptável (Android recorta em círculo/squircle) |

## Publicar no GitHub Pages

1. Crie um repositório **público** chamado `treino-2026` (público é o necessário para o
   Pages no plano gratuito).
2. Envie os arquivos: no repositório vazio, **Add file → Upload files**, arraste os 6
   arquivos desta pasta (não a pasta — os arquivos precisam ficar na raiz) e faça o commit.
3. **Settings → Pages**: em *Build and deployment*, escolha *Deploy from a branch*,
   selecione a branch `main` e a pasta `/ (root)`. Salve.
4. Aguarde ~1 minuto. O endereço fica
   `https://SEU-USUARIO.github.io/treino-2026/`.

Pela linha de comando, se preferir:

```bash
git init
git add .
git commit -m "Treino 2026 v3.0"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/treino-2026.git
git push -u origin main
```

## Instalar no celular

1. Abra `https://SEU-USUARIO.github.io/treino-2026/` no **Chrome do Android**.
2. Menu (⋮) → **Adicionar à tela inicial** / *Instalar app*.
3. O app abre em tela cheia, sem barra de endereço, e funciona sem internet.

O HTTPS do GitHub Pages é o que habilita o service worker, o `localStorage` persistente
e o compartilhamento nativo dos relatórios.

## Atualizar depois

Suba o novo `index.html` e **incremente a versão do cache** na primeira linha útil do
`sw.js` (`const CACHE = "treino2026-v3.1"`). Sem isso o celular continua servindo a
versão antiga do cache. No app há também *Ajustes → Buscar atualização*, que limpa o
cache e recarrega.

## Dados

Tudo fica no `localStorage` do navegador, preso à origem onde o app é aberto — trocar de
endereço ou limpar os dados de navegação apaga o histórico. Use *Ajustes → Exportar
backup* periodicamente; o `.json` restaura cargas, histórico e preferências.

## Observações sobre o conteúdo

- Séries, repetições e pausas vieram do **mapa v5** (`mapa-treino-2026.html`, versão v5).
- Cronograma v5: SEG=A · TER=GK · QUA=B · QUI=C · SEX=D · SÁB=OFF · DOM=Cardio.
- **Exceção:** o mapa v5 mantém o Bloco Goleiro na terça mas **não reimprime a tabela
  de exercícios** dele. Os exercícios do GK no app vêm do mapa anterior (v4) — confira
  se ainda valem e ajuste em `PLAN.GK` no `index.html`.
- Cargas de exercícios renomeados na v5 migram sozinhas (ver `ALIAS_CARGA`):
  Cross Over/Voador → Voador, Rodinha → Abdominal Supra, Heel Touch → Abdominal Heel
  Touch, Vela → Abdominal Vela, Chin-up → Barra Chin-up. **Supino Máquina → Supino
  Inclinado não migra**: o mapa diz explicitamente que começa do zero.
- Um treino que estivesse em andamento sob o plano antigo é descartado na primeira
  abertura da v3.0, porque os exercícios mudaram de posição e de conteúdo.

## Peso levantado (volume de carga)

- Fórmula: **carga × repetições**, somada série a série, usando a carga vigente no
  momento em que cada série foi fechada.
- Repetições por série saem do próprio plano: `3×10` → 10+10+10; `4×12/10/8/6` →
  12, 10, 8 e 6; `3×8+12` → 20 por série (as duas faixas somadas).
- Exercícios por tempo ou rodada (`3×40s`, `2 min`, `3 voltas`) e os que estão sem
  carga registrada contam **zero**. Para a Barra Chin-up entrar na conta, registre o
  peso corporal no `+/−` do exercício.
- Totais de semana, mês e acumulado aparecem no Histórico e no relatório "Treinos feitos".

## Limite de duração

- Treino não encerrado tem a duração **registrada no teto de 1:05** (`DUR_MAX_MS`),
  para que um treino esquecido aberto não distorça os acumulados. O relógio da sessão
  continua contando e fica âmbar depois desse ponto, avisando.
- O limite também é aplicado na leitura, então registros antigos com duração absurda
  entram nos relatórios já limitados.
- O 1RM dos relatórios é **estimado** pela fórmula de Epley — `carga × (1 + reps/30)` —
  aplicada sobre a menor repetição prescrita (a série mais pesada). É uma projeção
  estatística, não um teste: perde precisão acima de ~10 repetições, por isso o app
  não exibe 1RM para séries mais longas.
