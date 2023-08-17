# Modelo de Aprendizado de Máquina: Zyfra

## Descrição da tarefa
Preparar um protótipo de um modelo de aprendizado de máquina para o Zyfra. A empresa desenvolve soluções de eficiência para a indústria pesada.
O modelo deve prever a quantidade de ouro puro extraído do minério de ouro. Temos os dados sobre a extração e a purificação.
O modelo ajudará a otimizar a produção e eliminar parâmetros não rentáveis.

## Processo Tecnológico
Como o ouro é extraído do minério? Vejamos as etapas desse processo.
O minério extraído passa por processamento primário para obter a mistura de minério que é a matéria-prima usada para flotação (que é um método de concentração do minério). Após a flotação, o material passa por uma purificação em duas etapas.

![image](https://github.com/danielbryksil/Model-de-aprendizado-de-maquina-Zyfra/assets/116821863/07abeb0b-3d84-412b-ad5f-9d9274151994)

Vamos detalhar o processo:
1. Flotação:
A mistura de minério de ouro é alimentada nos bancos de flotação para obter concentrado de Au bruto e outros restos de minérios brutos (resíduos de produtos com baixa concentração de metais valiosos).
A estabilidade deste processo é afetada pelo estado físico-químico volátil e não ótimo da polpa de flotação (uma mistura de partículas sólidas e líquidas).
2. Purificação:
O concentrado bruto passa por duas etapas de purificação. Após a purificação, temos o concentrado final e novos restos de minério.

## Descrição de dados
Processo tecnológico
-	Rougher feed — matéria-prima
-	Rougher additions (ou aditivos reagentes) — reagentes de flotação: Xantato, Sulfeto, Depressor
-	Rougher process — flotação
-	Rougher tails — resíduos do produto
-	Float banks — unidade de flotação
-	Cleaner process — purificação
-	Rougher Au — concentrado de ouro bruto
-	Final Au — concentrado de ouro final

Parâmetros das etapas
-	air amount — volume of air — volume de ar
-	fluid levels
-	feed size — tamanho de partícula alimentada
-	feed rate

## Nomeação de características
Veja como são nomeadas as características:
[stage].[parameter_type].[parameter_name]
Exemplo: rougher.input.feed_ag
Valores possíveis para [stage]:
-	rougher — (Minério bruto) flotação
-	primary_cleaner — purificação primária
-	secondary_cleaner — purificação secundária
-	final — características finais
Valores possíveis para [parameter_type]:
-	input — parâmetros de matéria-prima
-	output — parâmetros do produto
-	state — parâmetros que caracterizam o estado atual do processamento
-	calculation — características de cálculo

![image](https://github.com/danielbryksil/Model-de-aprendizado-de-maquina-Zyfra/assets/116821863/5716460e-061f-4929-a3a2-ef068925871e)

## Cálculo de retirada
Para simular o processo de retirada do ouro puro do minério de ouro é utilizada a seguinte fórmula:

![image](https://github.com/danielbryksil/Model-de-aprendizado-de-maquina-Zyfra/assets/116821863/b55542cb-ed16-4bd3-871f-620c2f86010c)

onde:
-	C — proporção de ouro no concentrado logo após a flotação (para encontrar a quantidade retirada do concentrado bruto)/após a purificação (para encontrar a quantidade retirada final do concentrado)
-	F — a proporção de ouro alimentado no sistema antes da flotação (para encontrar a quantidade retirada do concentrado bruto)/ quantidade no concentrado logo após a flotação (para encontrar a quantidade retirada final do concentrado)
-	T — a proporção de ouro nos restos de minério bruto logo após a flotação (para encontrar a quantidade retirada do concentrado bruto)/ quantidade após a purificação (para encontrar a quantidade retirada final do concentrado)

Para prever o coeficiente, é preciso  encontrar a proporção de ouro no concentrado e nos restos de minério. Observe que tanto os concentrados finais quanto os brutos importam.

## Métrica de avaliação
Para resolver o problema, precisaremos de uma nova métrica. É chamado de sMAPE, ou erro percentual absoluto médio simetrico.
É semelhante ao EAM, mas é expresso em valores relativos em vez de absolutos. Por quê é simétrico? Ele leva em conta a escala do objetivo e da previsão.
O sMAPE é calculado assim:

![image](https://github.com/danielbryksil/Model-de-aprendizado-de-maquina-Zyfra/assets/116821863/2576e1fa-dffc-4189-a705-380a2476ab8b)

Denotação:
![image](https://github.com/danielbryksil/Model-de-aprendizado-de-maquina-Zyfra/assets/116821863/2462d2ce-ae0a-441c-8307-4617b5e7d976)
-	Valor do objetivo para a observação com o índice i no conjunto utilizado para medir a qualidade.
![image](https://github.com/danielbryksil/Model-de-aprendizado-de-maquina-Zyfra/assets/116821863/3cbf4bc4-7375-47f4-a982-1f034fddef4b)
-	Valor da predição para a observação com o índice i, por exemplo, na amostra de teste.
![image](https://github.com/danielbryksil/Model-de-aprendizado-de-maquina-Zyfra/assets/116821863/5b88c877-bcb9-434b-ac16-1f98a68d2c73)
- Número de observações na amostra.
![image](https://github.com/danielbryksil/Model-de-aprendizado-de-maquina-Zyfra/assets/116821863/dcf99e98-ff37-489a-9ef6-50f7c8a3adfd)
-	Somatório de todas as observações da amostra (i assume valores de 1 a N).
Precisamos predizer dois valores:
1.	Quantidade retirada do concentrado bruto rougher.output.recovery
2.	Quantidade retirada final do concentrado final.output.recovery

A métrica final inclui os dois valores:
![image](https://github.com/danielbryksil/Model-de-aprendizado-de-maquina-Zyfra/assets/116821863/fbc4feb9-ac4e-41b5-9e5e-4e881a5a9c8f)






