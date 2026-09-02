# Clara Fit

Calculadora de receitas fit, diário alimentar e meta de calorias — feito para a Clara.

Site publicado em: https://clara-fit-diegotorresza-gmailcoms-projects.vercel.app

## O que o app faz

- **Receitas**: cadastro de receitas ingrediente por ingrediente, com cálculo automático de calorias e macros (proteína, carboidrato, gordura), a partir de um catálogo com ~40 alimentos comuns ou valores personalizados.
- **Hoje (diário alimentar)**: registro do que foi consumido no dia, comparando com a meta diária.
- **Meta de calorias**: calcula Taxa Metabólica Basal (TMB), gasto calórico total (TDEE) e a meta diária de déficit calórico para emagrecimento saudável, com aviso de segurança caso a meta fique baixa demais.
- **Progresso de peso**: registro de peso ao longo do tempo com gráfico de evolução.

## Como funciona o armazenamento dos dados

O app tenta salvar os dados (receitas, diário, peso, meta) em um banco de dados **Supabase** gratuito, para sincronizar entre celular e computador. Se o Supabase não estiver configurado, ele salva automaticamente só no navegador (`localStorage`) — funciona normalmente, só não sincroniza entre aparelhos.

### Configurar o Supabase (opcional, para sincronizar entre aparelhos)

1. Crie uma conta gratuita em [supabase.com](https://supabase.com) e um novo projeto.
2. No projeto, abra **SQL Editor** e rode:

   ```sql
   create table if not exists app_state (
     id text primary key,
     data jsonb not null default '{}'::jsonb,
     updated_at timestamptz not null default now()
   );

   alter table app_state enable row level security;

   create policy "public read" on app_state for select using (true);
   create policy "public insert" on app_state for insert with check (true);
   create policy "public update" on app_state for update using (true);
   ```

3. Vá em **Project Settings → API** e copie a **Project URL** e a chave **anon public**.
4. Abra `index.html` e preencha o bloco no topo do arquivo:

   ```html
   window.CLARA_FIT_CONFIG = {
     supabaseUrl: "https://SEU-PROJETO.supabase.co",
     supabaseAnonKey: "SUA-CHAVE-ANON-AQUI"
   };
   ```

5. Publique de novo (push no GitHub já atualiza o Vercel automaticamente, se estiverem conectados).

## Deploy

O site é um único arquivo HTML estático (`index.html`), sem build, sem dependências — pode ser hospedado em qualquer lugar (Vercel, GitHub Pages, Netlify, etc). Atualmente está publicado no Vercel.
