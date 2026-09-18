# Coordenação da equipe

O agente principal coordena o trabalho e responde pela entrega. As skills em `.agents/skills/` são especialidades sob demanda, não agentes que precisam executar toda tarefa. Leia apenas as necessárias. Pedidos de etapa isolada terminam nessa etapa.

## Entrada e escolha do fluxo

Antes de editar, registre brevemente: objetivo, critérios observáveis de aceite, escopo, risco e como validar. Use o pedido e as convenções existentes; pergunte apenas se uma informação ausente mudar a solução ou impedir trabalho seguro. Não peça confirmação rotineira do nível escolhido.

Classifique pelo impacto, não pelo número de arquivos:

| Situação | Fluxo |
| --- | --- |
| Baixo risco: texto, apresentação ou ajuste local reversível sem lógica sensível | implementação → revisão proporcional → verificação direcionada |
| Funcionalidade ou mudança de comportamento | critérios → implementação → revisão → validação funcional e de extremos relevantes |
| Alto risco: permissões, segurança, dados persistentes, migração, integração crítica ou produção | critérios e rollback → implementação → revisão independente → validação funcional, de extremos e regressão |
| Bug | relato suficiente → diagnóstico → correção → revisão → reprodução original e regressão direcionada |

Acrescente regressão em qualquer nível quando houver consumidores afetados. Avaliação de usabilidade entra quando houver interface ou jornada cuja clareza precise ser avaliada; não é obrigatório em mudanças sem interação humana. Em bugs de causa demonstrada, diagnóstico e correção podem ocorrer na mesma execução. Causa incerta exige investigação dentro de `desenvolvimento` antes do patch.

## Responsabilidades e passagem de trabalho

- Coordenador: critérios de aceite, seleção das etapas, consolidação de achados, contagem de rodadas e decisão final. Não é necessária uma skill de coordenação.
- `desenvolvimento`: funcionalidades, diagnóstico e correção de bugs. Um único responsável edita cada área por vez.
- `revisao`: analisa o diff e os critérios, sem editar o patch revisado.
- `validacao`: cobre aceite, regressão, extremos e usabilidade pertinentes, reaproveitando evidências válidas.
- `analista-suporte` (opcional, fora do fluxo): somente para pedidos de redação de tickets. Relatos de bugs podem ser esclarecidos diretamente por desenvolvimento; achados seguem direto para correção.
- Nenhuma skill dispara outra por conta própria; devolve seu resultado ao coordenador.

Cada passagem contém apenas objetivo/critério, versão ou diff exato, evidências relevantes, pendências e próximo responsável. Para falhas, use um ID estável com severidade, local ou cenário, esperado, observado e reprodução. Atualize o mesmo achado em vez de criar duplicatas.

## Independência e concorrência

Quando ferramentas e instruções do ambiente permitirem, delegue revisão de risco alto a um subagente com contexto novo: pedido, critérios, diff e contexto técnico necessário. Não entregue a conclusão desejada. Para os demais níveis, use revisão independente se o ganho justificar o custo; se o autor revisar, identifique como autorrevisão.

Sem revisor independente disponível, conclua o trabalho local possível e marque a revisão independente pendente no alto risco; não declare entrega validada. Outra marca ou modelo não é requisito nem garantia de independência.

As dimensões da validação são executadas pela mesma skill. Paralelize somente tarefas independentes, sem escrita concorrente nos mesmos arquivos nem testes que disputem dados/ambiente. Não crie agentes apenas para representar cada papel.

## Evidências e decisões

Descubra os comandos e procedimentos reais nos arquivos do projeto e na configuração de validação, quando existir. Registre comando ou passos, ambiente, versão testada, esperado, observado e resultado. Não invente comandos executados, testes aprovados ou capturas. Leitura de código é análise estática, não teste funcional.

Resultados de revisão/validação: **aprovado**, **reprovado**, **bloqueado** (dependência impede avaliar), **não aplicável** (justificado). Teste não executado fica pendente, nunca aprovado. Evidência deixa de cobrir a entrega quando a mudança posterior altera o comportamento testado.

- Bloqueante: risco demonstrado de perda de dados, exposição, acesso indevido ou indisponibilidade grave. Impede avanço.
- Importante: comportamento incorreto demonstrado ou critério de aceite não atendido. Impede conclusão.
- Sugestão: melhoria opcional sem defeito demonstrado. Não reabre a entrega nem exige correção.

## Limite de retrabalho

Agrupe achados conhecidos antes de corrigir. Após a primeira revisão/validação, permita até **duas rodadas de correção e revalidação por demanda**, contando também falhas novas; não reinicie o contador ao trocar de papel, sessão ou achado.

Em cada rodada, corrija os achados impeditivos e reexecute os cenários que falharam e os afetados pelo patch. Amplie a validação se o impacto mudar. Não reinicie todo o fluxo automaticamente. Não repita tentativa idêntica sem evidência nova.

Se houver falha impeditiva após a segunda rodada, pare novas alterações especulativas e entregue estado parcial, evidências, hipóteses restantes e a decisão concreta necessária. Não declare sucesso. Uma nova rodada exige acordo explícito de escopo/plano com o usuário. Bloqueio de ambiente exige diagnóstico do acesso ou recurso ausente, não rodadas de alteração de código.

## Contexto, Git e retomada

Preserve alterações preexistentes e restrições locais. Não registre segredos em código, logs ou tickets. Use ambiente de teste e dados fictícios quando possível. Publicação, envio de mensagens e mutações externas seguem a autorização efetiva do pedido; este arquivo não concede autorização adicional.

Siga o fluxo Git do repositório. Inspecione o diff; inclua somente mudanças da tarefa em commits/PRs autorizados. Não descarte trabalho de terceiros para limpar a árvore. Defina reversão proporcional: restaurar o patch/commit isolado ou, para dados, plano específico de backup/restauração validável; não suponha que reverter código desfaz migração.

Use `HANDOFF.md` somente em interrupção, troca de sessão ou tarefa longa: objetivo, versão, mudanças, evidências, IDs pendentes, rodadas consumidas e próximo passo. Atualize em vez de acumular diários. Registre decisões duráveis no documento de decisões já usado pelo projeto, sem duplicar relatórios de etapas.

## Conclusão

Conclua quando os critérios estiverem atendidos, os achados impeditivos resolvidos e as verificações aplicáveis executadas. Resuma o que mudou, evidências, risco residual e reversão. Distingua claramente entrega validada, implementação com validação pendente e trabalho bloqueado.


