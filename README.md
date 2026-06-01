# BISA × Hendrick — Video Scroll Edition

Hero com **vídeo controlado pelo scroll** (técnica Apple): você scrolla e o vídeo do
terrão → blueprint avança frame a frame. Depois a página segue com campo pronto, stats,
galeria e investimento.

## Como rodar

Precisa de servidor (vídeo não toca via duplo-clique):
```bash
npx serve
```
Ou Live Server no VS Code.

## Estrutura
```
bisa-video/
├── index.html
└── public/
    ├── hero.mp4          ← seu vídeo otimizado (keyframe em todo frame, sem áudio)
    └── images/           ← imagens das outras seções
```

## Como funciona o scroll-scrub

- A seção do vídeo tem `height:400vh` — ou seja, são ~4 telas de scroll para o vídeo de 8s tocar inteiro
- Conforme você scrolla, o `currentTime` do vídeo é setado proporcionalmente
- Scrolla pra baixo = avança; pra cima = volta. Tudo suave porque o vídeo tem keyframe em cada frame
- As legendas (01 Today's Ground → 02 The Blueprint → 03 The Plan Is Set) trocam conforme o progresso

## Ajustes rápidos

- **Velocidade do scrub**: muda `height:400vh` na `.video-section`. Maior = scroll mais longo/lento. Menor = mais rápido.
- **Texto do hero some**: controlado em `copyOp` (some nos primeiros 15% do scroll)
- **Trocar o vídeo**: substitui `public/hero.mp4`. Pra re-otimizar outro vídeo pro scroll:
  ```
  ffmpeg -i entrada.mp4 -an -g 1 -keyint_min 1 -sc_threshold 0 -c:v libx264 -crf 20 -pix_fmt yuv420p -movflags +faststart hero.mp4
  ```

## Nota de performance

O vídeo está com keyframe em todo frame (por isso ~14MB) — é o que deixa o scroll liso.
Em conexões lentas pode demorar a carregar; o `preload="auto"` ajuda. Para produção,
hospedar em Vercel/Netlify resolve o carregamento.
