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
git commit -m "Treino 2026 v2.0"
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
`sw.js` (`const CACHE = "treino2026-v2.1"`). Sem isso o celular continua servindo a
versão antiga do cache. No app há também *Ajustes → Buscar atualização*, que limpa o
cache e recarrega.

## Dados

Tudo fica no `localStorage` do navegador, preso à origem onde o app é aberto — trocar de
endereço ou limpar os dados de navegação apaga o histórico. Use *Ajustes → Exportar
backup* periodicamente; o `.json` restaura cargas, histórico e preferências.

## Observações sobre o conteúdo

- Séries, repetições e pausas vieram de `mapa-treino-2026.html`.
- **Exceção:** as pausas dos extras (Rosca Martelo e Chin-up) não constam no mapa
  original; foram adotadas como 1:00 → 1:30. Ajuste em `const EXTRAS` no `index.html`.
- O 1RM dos relatórios é **estimado** pela fórmula de Epley — `carga × (1 + reps/30)` —
  aplicada sobre a menor repetição prescrita (a série mais pesada). É uma projeção
  estatística, não um teste: perde precisão acima de ~10 repetições, por isso o app
  não exibe 1RM para séries mais longas.
