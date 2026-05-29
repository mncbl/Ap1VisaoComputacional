# Estimativa do Nível de Serviço Viário: Uma Abordagem com Visão Computacional

## Visão Geral do Projeto
Este repositório contém a implementação prática e analítica do estudo "Estimativa do Nível de Serviço Viário: Uma Abordagem com Visão Computacional e Câmeras de Monitoramento". O sistema propõe um framework para a estimativa automatizada do Nível de Serviço (LOS) em rodovias, integrando a arquitetura YOLO (You Only Look Once) com a modelagem de relação fluxo-velocidade. O objetivo primário é substituir a métrica PTSF (Percent-Time-Spent Following), que exige altos custos de infraestrutura para medição, por indicadores viáveis de serem extraídos via visão computacional, especificamente a Densidade Veicular e a Velocidade Média de Percurso (ATS).

## Estrutura do Repositório
Os arquivos essenciais presentes na raiz deste projeto incluem:
* **`AP1VisaoCOmputacional.ipynb`**: Notebook principal que abriga o pipeline de processamento de imagem, a inferência do modelo de inteligência artificial e os cálculos matemáticos para determinação do Nível de Serviço.
* **`last_video (1).mp4`**: Arquivo de vídeo capturado em Niterói, utilizado como amostra de campo empírica para testes, calibração de parâmetros e validação de rastreamento veicular.
* **`README.md`**: Documentação técnica e arquitetural do sistema.

## Arquitetura do Sistema e Processamento (Grayscale Expert)
O projeto utiliza o modelo YOLOv8 integrado ao algoritmo ByteTrack para a detecção, classificação multiobjeto e rastreamento contínuo em altíssima velocidade.

Para lidar com os desafios estocásticos e de iluminação em imagens de tráfego rodoviário (especialmente distorções noturnas), o código implementa um pipeline rigoroso de pré-processamento denominado **Grayscale Expert**:
* **Correção Gamma (2.5):** Responsável por suprimir o estouro de luz (Blooming) gerado pelos faróis dos veículos.
* **Conversão para Escala de Cinza:** Processo focado em focar apenas nas bordas e formas, eliminando completamente os ruídos de crominância.
* **Filtro CLAHE Agressivo:** Aplicação de contraste local forçado para revelar as silhuetas de veículos sobre o asfalto.
* **Suavização Gaussian Blur:** Redução da granulação para fornecer um frame limpo à rede neural.

## Metodologia de Engenharia de Tráfego e Validação
A modelagem teórica adota a relação fluxo-velocidade côncava, baseada no manual alemão HBS e ajustada ao comportamento do tráfego sob a infraestrutura brasileira.

Os parâmetros foram validados utilizando o microssimulador CORSIM calibrado por Algoritmos Genéticos. Os resultados comprovaram a superioridade técnica da nova abordagem:
* A Densidade Veicular ($D_{d,car}$) provou ser o indicador mais robusto, exibindo uma correlação de Pearson de $r=0.99$ com os dados reais em campo e um Erro Absoluto Médio (MANE) de apenas 8%.
* A aplicação prática (evidenciada pelo teste em vídeo) demonstrou capacidade de inferir o fluxo volumétrico e alocar automaticamente a via em classificações de serviço regulamentadas (por exemplo, classificando um trecho dinâmico com sucesso em LOS B).

## Dependências e Execução
As bibliotecas necessárias para a execução do notebook são:
* `ultralytics`
* `opencv-python`
* `numpy`

**Procedimento de Execução:**
1. Instale os pacotes e dependências requisitados via terminal executando: `pip install ultralytics opencv-python numpy`.
2. Abra o arquivo de código estruturado `AP1VisaoCOmputacional.ipynb`.
3. Verifique a variável de carregamento do vídeo (diretório `video_path`) e certifique-se de que ela aponte corretamente para o arquivo local `last_video (1).mp4`.
4. Execute as células de processamento. O algoritmo processará os frames e instanciará um novo vídeo de saída com o dashboard visual, linhas de contagem ativas e a determinação contínua do Nível de Serviço.
