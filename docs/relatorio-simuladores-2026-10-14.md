# Revisão dos simuladores — 14/10/2026

**Site:** <https://ghostnomzer.github.io/Projeto-integrador-1.2/>

**Repositório:** <https://github.com/Ghostnomzer/Projeto-integrador-1.2>

Foram testados aquecimento solar, economia de água, automação e o comparativo solar/elétrico. Os testes básicos anteriores passaram, mas casos adicionais revelaram **seis falhas**, corrigidas nesta revisão. O estado inicial dos simuladores no novo endereço foi comparado ao código local e correspondia aos mesmos arquivos.

## Falhas encontradas e corrigidas

| Falha | Como reproduzir no estado anterior | Comportamento corrigido |
|---|---|---|
| Bomba ligada com leitura inválida | Na automação, apagar a temperatura da piscina; mesmo marcando falha de sensor, o painel mantinha “Ligada” e “Aquecendo” do cenário anterior. | Dados inválidos bloqueiam a bomba simulada, exibem o motivo e interrompem a memória de bomba ligada. A recuperação exige novamente diferença de pelo menos 4 °C. |
| Validação sem acesso ao campo oculto | Abrir “Eficiência e perdas”, apagar a eficiência, fechar a seção e enviar o formulário. | A validação abre as seções que contêm campos inválidos e foca o primeiro campo a corrigir, inclusive quando há vários erros. |
| Rótulos sobrepostos no gráfico | Usar 100.000 L, coletor de 1 m², temperaturas de 5 °C e 40 °C, irradiância de 50 W/m², eficiência de 10% e perdas de 60%, em tela estreita. | O eixo usa notação científica para tempos elevados e adapta a quantidade de rótulos à largura. A tabela mantém os cinco pontos e seus valores completos. |
| Vantagem de custo inexistente | Informar tarifa zero no comparativo. Os dois custos eram R$ 0,00, mas a explicação dizia “a favor do solar”. | A explicação informa que os custos operacionais são iguais. O consumo de energia continua apresentado separadamente. |
| Automação transbordando em celular | Abrir a automação em 320 px com alto contraste, OpenDyslexic, espaçamento e texto a 200%. O indicador de estado ultrapassava a largura da página. | O cabeçalho do controlador permite quebra de linha, mantendo título e estado dentro da tela. |
| Abas escondidas pelo cabeçalho | Abrir um link como `simuladores.html#aquecimento`, inclusive ao visitar novamente o mesmo fragmento. | A posição de chegada considera a altura real do cabeçalho e das abas. Os controles permanecem visíveis e são revelados ao receber foco pelo teclado. |

As mudanças afetam validação, apresentação e navegação. As fórmulas físicas dos modelos não foram alteradas.

## Resultados finais

| Verificação | Resultado |
|---|---:|
| Testes independentes dos modelos científicos | 15 aprovados de 15 |
| Grupos funcionais dos simuladores e comparativo | 18 aprovados de 18 |
| Cenários numéricos comparados com cálculos independentes | 132 aprovados de 132 |
| Combinações de painel, navegador, largura e leitura | 120 aprovadas de 120 |
| Auditorias axe dos estados normal/alto contraste | 12, sem violações detectadas nas regras executadas |
| Erros JavaScript nos testes finais | 0 |

Os grupos funcionais foram executados em **Chromium 151, Firefox 153 e WebKit 26.5**. Cobrem abas e teclado, todos os parâmetros, exemplos, restauração, campos inválidos, Sol zero, meta atingida, cobertura, tabelas, gráfico, falha de sensor, histerese, histórico e comparativo com tarifa zero/pouca irradiância. Os casos de regressão incluem os seis problemas listados acima.

A matriz numérica alterou cada parâmetro entre seus limites, além de cenários de referência e combinações de extremos. Foram conferidos energia, potência, tempo, balanço de água, consumo e custos, respeitando o arredondamento apresentado pela interface.

A matriz de leitura usou quatro painéis — aquecimento, água, automação e comparativo — nos três navegadores, em **320, 390, 768, 1.024 e 1.440 px**, nos perfis padrão e texto a 200% com alto contraste, OpenDyslexic e espaçamento ampliado. O aquecimento incluiu o cenário de tempo elevado, e a automação incluiu dados inválidos.

As 12 auditorias axe-core 4.10.3 foram executadas em Chromium, em dois temas e seis estados: aquecimento de referência, aquecimento extremo, água, automação normal, automação com leitura inválida e comparativo com tarifa zero. As regras WCAG A/AA e boas práticas executadas não detectaram violações no resultado final. Itens que exigem inspeção manual, incluindo textos de SVG, permanecem registrados.

## Endereços atualizados

Os links do repositório, endereço principal, links canônicos, sitemap, robots.txt e documentação passaram a usar os endereços Ghostnomzer informados. A configuração das páginas concentra os endereços principais em `scripts/layout.mjs`.

## Evidências e reprodução

- [Resultados estruturados e evidências do estado inicial](relatorio-simuladores-2026-10-14.json)
- [Casos funcionais e de regressão](../testes/revisao.py)
- [Testes científicos](../testes/modelos.test.cjs)
- [Publicações no GitHub Actions](https://github.com/Ghostnomzer/Projeto-integrador-1.2/actions/workflows/pages.yml)

Com o servidor local em execução e Playwright instalado:

```bash
node --test testes/modelos.test.cjs
node scripts/verificar.mjs
python3 testes/revisao.py --navegadores chromium firefox webkit --casos 8 9 10 11 12 13 --saida test-results/simuladores
```

Os seis grupos são executados em cada navegador, totalizando 18 grupos funcionais. A matriz numérica adicional e as auditorias estão registradas no JSON; a suíte versionada acima reproduz os casos de regressão. Para instalar as ferramentas, consulte [verificacao-acessibilidade.md](verificacao-acessibilidade.md).

Esta revisão avaliou simulações didáticas no navegador, sem equipamentos conectados ou medições de campo. A avaliação humana contínua com leitores de tela não foi realizada.
