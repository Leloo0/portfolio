# Worklog

---
Task ID: 6
Agent: main (Z.ai)
Task: Recriar o projeto do zero (ambiente foi resetado) e adicionar os 6 projetos reais com links, com Pyraminx 3D como primeiro card.

Work Log:
- Detectado reset do workspace (apenas upload/download/skills restavam); reinicializado scaffold via init-fullstack.sh
- Reaplicado Prisma (model ContactMessage) + db:push
- Recriados: api/contact (zod), globals.css (tema neon original), layout.tsx (SEO Leonardo Boury), favicon.svg, next.config (devIndicators false)
- Recriados componentes: cube-logo, typewriter (versão corrigida com slice acumulativo), header (menu mobile), footer (copyright), about (avatar LB), projects (novos ícones + badge highlight), contact (form completo), page.tsx
- data.ts: 6 projetos REAIS substituindo os exemplos, na ordem pedida:
  1. Pyraminx 3D (1º por conta própria, badge destacado) — leloo0.github.io/primeiro_projeto
  2. Postura Profissional — leloo0.github.io/Boas-Pr-ticas
  3. Sabor & Arte — bootstrap-seven-omega.vercel.app
  4. CINE-VR — cinevr.vercel.app
  5. Zelo Connect Invest — zelo-connect-invest.vercel.app
  6. Planetas — Atlas Interativo — leloo0.github.io/planetas-5.0
- Descrições/aprendizados escritos a partir do conteúdo real de cada site (visitado via WebFetch); categoria "Ferramentas" renomeada para "Interativos"
- Verificações agent-browser: typewriter "Olá, eu sou Leonardo Boury", stats "10 meses/Tempo de estudo", 6 hrefs corretos, form enviou e persistiu (1 registro no banco), menu mobile OK, zero erros de console

Stage Summary:
- Portfólio reconstruído integralmente com dados reais: nome, contatos, stats e 6 projetos publicados com links funcionais
- Badge "1º projeto por conta própria" em destaque no card do Pyraminx 3D

---
Task ID: 7
Agent: main (Z.ai)
Task: Adicionar a foto de perfil real enviada pelo usuário ao avatar da aba Sobre

Work Log:
- Foto recebida em upload/ (selfie vertical 1200x1600) copiada para public/profile.jpg
- Criado scripts/crop_profile.py: recorte quadrado 786px centrado no rosto (face_cx 44.4%W, folga 13% acima do cabelo), brilho +14%/contraste +6%/cor +5%, redimensionado para 640x640 JPEG q88
- about.tsx: iniciais "LB" substituídas por next/image (/profile.jpg, fill, object-cover, priority, alt descritivo); container ganhou overflow-hidden mantendo borda neon e glow
- layout.tsx: openGraph.images adicionado (/profile.jpg 640x640) para preview ao compartilhar o link
- bun run lint: sem erros
- Verificação agent-browser: foto visível no avatar circular com anel neon, aba Projetos intacta (6 cards, href do primeiro_projeto presente), zero erros de console
- Screenshot de verificação salvo em download/verify-foto-perfil.png

Stage Summary:
- Foto de perfil real integrada ao avatar e aos metadados OG
- Pendências restantes (opcionais): prints dos projetos para os placeholders e CV PDF com botão "Baixar CV"

---
Task ID: 8
Agent: main (Z.ai)
Task: Integrar os 6 screenshots reais dos projetos enviados pelo usuário nos cards da aba Projetos

Work Log:
- Mapeamento upload -> projeto: 383b6780=Pyraminx 3D, 112d226d=Postura Profissional, d3535a38=Sabor & Arte, 503afb3f=CINE-VR, 868b051a=Zelo Connect Invest, 6a7a1d1a=Planetas/Orbital Explorer
- Imagens copiadas para public/projects/{pyraminx-3d,boas-praticas,sabor-e-arte,cine-vr,zelo-connect-invest,planetas-atlas}.jpg (todas 1600px de largura)
- data.ts: campo image?: string adicionado ao tipo Project e preenchido nos 6 projetos (ícone lucide mantido como fallback)
- projects.tsx: next/image com fill + object-cover object-top, zoom sutil no hover do card (group + scale 1.06, 500ms), gradiente escuro na base com chips das tecnologias sobre a imagem; área da imagem usa aspect-[2/1] quando há screenshot (evita corte em tablets); placeholder com ícone preservado para projetos sem imagem
- bun run lint: sem erros
- Verificação agent-browser: 6 cards com prints reais, badge "1º projeto por conta própria" sobre a imagem, chips de tech legíveis, viewport mobile 390px OK, zero erros de console
- Screenshots de verificação em download/verify-projetos-imagens{,2}.png e verify-projetos-mobile.png

Stage Summary:
- Cards de projetos agora exibem screenshots reais com overlay de tecnologias, mantendo a identidade dark/neon
- Única pendência restante (opcional): CV em PDF com botão "Baixar CV"

---
Task ID: 9
Agent: main (Z.ai)
Task: Trocar categorias Boas Práticas <-> Zelo Connect Invest e adicionar o currículo (PDF) ao site

Work Log:
- data.ts: boas-praticas category "web" -> "landing"; zelo-connect-invest "landing" -> "web"; descrição do Zelo reescrita ("Site de pitch..." em vez de "Landing page de...")
- CV enviado copiado para public/cv-leonardo-boury.pdf (156K, servido como application/pdf - testado via curl)
- data.ts: profile.cv = "/cv-leonardo-boury.pdf"
- about.tsx: novos CTAs após os badges de tech — "Baixar CV" (neon preenchido, ícone Download, atributo download="CV - Leonardo Boury.pdf") e "Fale Comigo" (outline, dispara CustomEvent open-tab=contato)
- page.tsx: listener do evento "open-tab" adicionado (valida aba e faz scroll top)
- contact.tsx: 4º card "Currículo" (FileText, label "Baixar CV em PDF", download=true) — render ajustado para download/target/rel condicionais
- bun run lint: sem erros
- Verificação agent-browser: filtros "Landing Pages"=2 cards e "Web"=1 card (troca confirmada), href do CV correto, clique em "Fale Comigo" navegou para Contato, 1 elemento a[download] presente, zero erros de console
- Screenshots: download/verify-cv-about.png e verify-cv-contato.png

Stage Summary:
- Categorias corrigidas conforme pedido e CV integrado em dois pontos (CTA na Sobre + card na Contato)
- Todas as pendências de conteúdo do portfólio foram concluídas

---
Task ID: 10
Agent: main (Z.ai)
Task: Recentralizar o recorte da foto de perfil no rosto do usuário

Work Log:
- Tentativa de detecção automática (OpenCV Haar cascade frontal + perfil): nenhum rosto detectado (rosto parcialmente coberto pelo celular, olhando para baixo)
- Recalibração manual: medido no recorte v1 que o rosto estava em (47%, 65%) — deslocado à esquerda e abaixo do centro do círculo
- scripts/crop_profile_v2.py: novo recorte 830px com rosto em (50%, 48.2%) do quadro (ideal para avatar circular), folga de 9% acima do cabelo, mesmo tratamento de brilho/contraste; origem (110,484)
- Problema encontrado: site continuava exibindo a imagem antiga — cache HTTP do Chrome persistente do agent-browser + cache do otimizador next/image em dev; restart do dev server não bastou
- Obstáculo adicional: processos background morrem entre chamadas Bash do harness; resolvido com scripts/daemon_dev.py (double-fork clássico, PPID=1, mesmo padrão do daemon do agent-browser) — servidor agora persiste entre chamadas
- Solução definitiva do cache: import estático da imagem em about.tsx (avatarImg de ../../../public/profile.jpg) — Next gera URL com hash, cache-busting automático a cada troca de foto
- bun run lint: sem erros; zero erros de console
- Verificação final: rosto centrado no círculo (~48% x, ~47% y), screenshots em download/verify-foto-final.png e avatar-zoom3.png

Stage Summary:
- Foto do avatar recentralizada no rosto com cache-busting automático via import estático
- Infra: daemon_dev.py garante dev server persistente entre chamadas
