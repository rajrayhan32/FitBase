# FitBase — Guia de Publicação

O que já foi feito automaticamente nesta rodada:

- ✅ Sessão persistente (recarregar a página não desloga mais)
- ✅ Contas de teste removidas da tela de login
- ✅ Edge Function `criar-aluno` implantada no Supabase (recepção/gerente criam contas de alunos sem expor a chave de serviço)
- ✅ Formulário "Criar conta de aluno" no painel da recepção
- ✅ Migração de segurança aplicada: removidas 2 policies de teste que davam acesso total à tabela `alunos` para qualquer usuário logado, e criadas as policies que o painel da Sofia precisa (excluir academias e dados vinculados)
- ✅ Rascunho da política de privacidade (LGPD) em `politica-de-privacidade.md`
- ✅ Edge Function `criar-staff` implantada: a Sofia cria o primeiro gerente/recepcionista/professor de cada academia nova direto pelo painel dela
- ✅ Gestão de aulas coletivas no painel da recepção (criar/remover aulas da grade, com as policies RLS correspondentes) — a agenda do aluno agora tem conteúdo

## O que falta — passos manuais (±30 min no total)

### 1. Hospedar o app (10 min)
1. Crie uma conta gratuita em https://app.netlify.com (ou Vercel/Cloudflare Pages).
2. Arraste o arquivo `fitbase.html` renomeado para `index.html` para a área "Deploy".
3. Pronto: você recebe um link `https://seuapp.netlify.app` com HTTPS. Depois dá para plugar um domínio próprio (ex.: `fitbase.com.br`).
4. No celular do aluno: abrir o link → menu do navegador → "Adicionar à tela de início" → vira um ícone de app.

### 2. Painel do Supabase (5 min)
No projeto FitBase (sfttqcxdbshdoqlyctgx):
1. **Authentication → Sign In / Providers → Email**: desative "Allow new users to sign up" (cadastro público) se estiver ligado — contas só nascem pela Edge Function.
2. **Authentication → Settings**: ative **Leaked password protection**.
3. **Authentication → Users**: apague as contas `*@demo.com` quando terminar os testes (a Academia Demo pode ser excluída pelo próprio painel da Sofia, que apaga tudo em cascata).

### 3. Teste de ponta a ponta (15 min)
Roteiro com uma academia descartável, na ordem:
1. Sofia cria a "Academia Teste" com plano válido.
2. Sofia cria o gerente e a recepcionista dessa academia pelo card "Criar conta de funcionário".
3. Recepção: cria a conta de um aluno pelo novo formulário → matricula ele → cadastra uma aula coletiva e confirma que ela aparece na agenda do aluno.
4. Professor: monta fichas "Treino A" e "Treino B" para o aluno.
5. Aluno: entra, escolhe "A" na pergunta do primeiro acesso, conclui → sai e entra de novo → deve abrir direto no B. Recarregue a página: deve continuar logado.
6. Sofia: suspende a academia → todos os usuários dela devem cair na tela "Acesso suspenso"; a criação de contas também é bloqueada.
7. Sofia: exclui a academia → confirme que alunos, treinos, históricos e perfis sumiram, sem erro.

### 4. Antes de vender
- Preencha os campos entre colchetes da política de privacidade e publique-a junto do app.
- No contrato com a academia, deixe claro que ela é a controladora dos dados dos alunos (LGPD) e o FitBase é o operador.
- Defina o processo de onboarding de cada academia nova: quem cria a conta do gerente/recepção (por ora, você, no painel do Supabase).

## Pendências conhecidas
Nenhuma. Todas as lacunas de funcionalidade foram fechadas — restam apenas os passos manuais acima.
