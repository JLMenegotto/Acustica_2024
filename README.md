# Acustica2024.dll

Acustica2024.dll é um aplicativo para Revit desenvolvido no Departamento de Expressão Gráfica da Escola Politécnica da UFRJ pelo Prof. José Luis Menegotto. 
O objetivo da API é fazer interface com o simulador sonoro BRASS desenvolvido pelo Prof. Julio Cesar Boscher Torres. 
A versão da API atualmente publicada funciona em Revit 2024 e tem algumas diferenças em relação à API desenvolvida em 2018.
https://periodicos.sbu.unicamp.br/ojs/index.php/parc/article/view/8653934

A aplicação pode ser controlada na interface gráfica GUI assim como executada por comandos de voz VUI

![RAIOS6000](https://user-images.githubusercontent.com/9437020/235116960-2306a9f9-e3fb-4e65-9eb4-82d0fb5b5e5c.PNG)

## Funcionalidade da API:
A API faz a leitura de um modelo BIM criado em Revit 2024 para gerar as informações que o sistema BRASS precisa para gerar a simulação sonora de salas (audibilização). O processo é iniciado pelo posicionamento de uma fonte sonora e um receptor (cabeça). É efetuada a operação de traçado de raios que partem da fonte sonora omnidirecional com valor de enegia = 1. Os raios percorrem a sala refletindo nas superfícies e perdendo energia de acordo aos coeficientes de absorção acústica dos materiais que atingem. A percepção sonora vai depender da somatória de energias que passem pela posição definida para a cabeça. 
O processo gera 5 arquivos:

1. **RAYS-S00N-R001.JSON:**  Arquivo estruturado em JSON com os dados do campo acústico da sala, ou seja, com todos os raios sonoros que saíram da fonte e todas as reflexões com as perdas de energia sonora calculadas para nove bandas de frequência (63, 125, 250, 500, 1K, 2K, 4K, 8K, 16K).
2. **RAYS-S00N-R001.TXT:**  Arquivo que contem apenas os raios e reflexões que passaram pela posição da cabeça, ou seja, os raios que têm influência na percepção acústica de um ouvinte posicionado nesse ponto da sala.
2. **T_RAYS-S00N-R001.TXT:**  Arquivo que contem todos os raios e reflexões da sala.
3. **NomeSala_Simulada.BAT:** Arquivo Bat para executar o BRASS e fazer a leitura e o cálculo de audibilização a partir dos dados fornecidos pelo arquivo de raios (TXT). O retorno do processo são arquivos WAV com o resultado da audibilização.
4. **NomeSala_Simulada.TXT:**  Arquivo TXT com outros parâmetros necessários para o BRASS realizar o processo de audibilização.
                               
## Instalação:
 1. Criar as pastas
 
      * **C:\APIBIM\Acustica**
      * **C:\APIBIM\Acustica\Ico**
      * **C:\APIBIM\Acustica\Som**
      * **C:\APIBIM\Acustica\Brass**
      * **C:\APIBIM\Acustica\Lex**  
      * **C:\APIBIM\Acustica\Loc**     
    
   Os arquivos **Acustica2024.dll** e **Parla_2024.dll** devem ser colocados dentro da pasta **C:\APIBIM\Acustica**
   O dll com as funções de Speech Recognition e TTS  estão em **Parla_2024.dll**. O seu funcionamento dependerá do sistema 
   ter carregado e ativado o driver de idioma espanhol (Espanha) para reconhecimento de fala.
   
   ![ConfigWindows](https://github.com/JLMenegotto/Acustica_2024/assets/9437020/1d4aa328-5702-41a6-9bea-646067da512f)
   
 2. Copiar o arquivo **2024_Acustica.addin** para a pasta **C:\ProgramData\Autodesk\Revit\Addins\2024**
 
 Caso decida instalar a API numa outra pasta da sua preferência, deverá alterar o caminho presente no conteúdo do arquivo Acustica_2024.addin
 
 ![Addin](https://user-images.githubusercontent.com/9437020/236694667-1af4dff9-9dda-48c6-8017-9f7ed62e205f.PNG)
  
 3. Na pasta **Som** devem estar os arquivos Waves Mono com sons anecóicos usados como base sonora para a realizar a audibilização em Brass. 
    O resultado será a criação de arquivos Wav estéreo independentes e combinados.
 
      * Soprano.wav: soprano realizando um exercício vocal com uma escala de alturas. 
      * Piano1.wav:  tocando uma escala ascendente e descendente.
      * Piano2.wav:  tocando uma escala descendente.
      * Oboe1.wav:   tocando uma escala ascendente e descendente.
      
    Exemplo de resultado:
      
      * S001-R001.WAV  (resultado da posição da fonte 1 usando o Piano) 
      * S002-R001.WAV  (resultado da posição da fonte 2 usando o Oboé) 
      * SALL-R001.WAV  a combinação da fonte 1 e 2 com Piano e Oboé.
      
4. Na pasta **Ico** colocar os icones gráficos da interface. 
5. Na pasta **Lex** colocar o arquivo Lexico_Acustica_2024.xlsx que contém a estrutura lexical da interface VUI
6. Na pasta **Loc** pode ser deixada vazia. Ao rodar a função será criada a gramática Locuções.XML automaticamente.

## Preparação do Modelo em Revit:

 1. A sala simulada deve ter um objeto ambiente inserido com o nome do compartimento definido, pois esse valor definirá o prefixo dos arquivos BAT e TXT.
 2. Os materiais usados no modelo devem ter o parâmetro **MARK** definido e cadastrado no arquivo **Acustica_Coeficientes.TXT**. 
 Esse arquivo contem os valores dos coeficientes de absorção acústica dos materiais da sala divididos em 9 bandas de frequência.
 Consultar com os fornecedores dos materiais para obter esses valores. Cada linha do arquivo corresponde a um material. 
 O arquivo é fornecido com alguns valores para testagem.
 O primeiro dado textual é o código do material que deve ser associado ao parâmetro MARK.

Exemplo: **A02 0.03 0.03 0.03 0.04 0.10 0.19 0.35 0.35 0.35 - BlocoConcreto**

![Materiais](https://user-images.githubusercontent.com/9437020/235194809-edbf0873-caee-476c-9103-f7472fd9e6cd.PNG)

 1. A API processa as faces atingidas pelos raios. Foram testadas as familias de sistema de Piso, Parede, Forros e Partes. Elas podem estar agrupadas. 
 2. As famílias de componentes que estejam dentro da sala estudada (cadeiras, refletores ou baffles acústicos, etc.) devem ter um parâmetro **instanciado**
    cujo nome possua a palavra "Material" (p.ex. "Material_Acustico", "Material_Frame"...) que permita à API filtrar e alocar o material utilizado.
    
![Materiais00](https://github.com/JLMenegotto/Acustica_2024/assets/9437020/576dd36e-191f-4d97-8567-4b3e4b1107ec)

3. Ao fazer o cálculo acústico da sala, é importante que as superfícies atingidas pelos raios estejam estritamente dentro do volume do compartimento. 
Por exemplo, se uma parede é modelada entre o piso e a laje superior e o compartimento tiver um forro rebaixado, então o acabamento da parede deverá 
terminar embaixo do forro. Para isso, utilizar o comando (Parts) que permite dividir as diversas camadas de materiais da parede. A camada de acabamento 
deverá ser dividida na altura do forro.
  
## Interface:
![Interface](https://user-images.githubusercontent.com/9437020/235352580-44726e4d-9f58-4e51-867b-8c1738b936bd.PNG)

 1. Botão de executar o processo de traçado de raios.
 2. Botão para a leitura do campo acústico (Em desenvolvimento...).
 3. Opções para desenhar ou não os raios calculados.
 4. Quantidade de raios a calcular.
 5. Quantidade de reflexões a calcular.
 6. Número da fonte sonora utilizada, (valor N em `RAYS-S00N-R001)`. Utilizado para nomear e diferenciar os arquivos JSON e TXT com os raios que partem de cada fonte sonora.

Para os campos 4, 5 e 6 Ingressar o valor e Pressionar Enter ou clicar no ícone do enter do campo.

 5. Na linha de Status de Revit serão informadas as interações processadas. Os raios desenhados utilizam a cor amarela que vai sendo clareada e afinada segundo as perdas de energia de cada reflexão. 
 Foi preparada uma gradação de 5 níveis diferenciados de cor e espessura de linha.

![contador](https://user-images.githubusercontent.com/9437020/235193199-33ac6d83-b916-4ef3-aa39-495c9d87b74e.png)

## Estilos:
1. A API incorpora 3 estilos de linha ao arquivo do modelo RVT. Eles têm as seguintes características:

![Estilos](https://user-images.githubusercontent.com/9437020/235129574-902e4f05-dd74-4636-836d-337d615d3aef.PNG)

## Desempenho:
1. Os valores de processamento a seguir foram obtidos rodando a API em um notebook Inspiron com processador I7, 8GB de RAM e disco SSD 1T com placa de video Onboard.
Como os processos de audibilização exigem grande quantidade de raios a serem processados, há um impacto para o processamento gráfico em Revit. 
A API permite optar por gerar os arquivos necessários para o BRASS sem precisar desenhar os raios no modelo.
2. Na tabela a seguir os tempos de procesamento sem desenhar os raios

![tempos](https://user-images.githubusercontent.com/9437020/236032503-6e8efb6d-1003-4413-8a40-c03b4a2ece68.PNG)


## Limitações:
1. Nesta versão, o usuário pode posicionar a fonte e a cabeça livremente no plano XY, mas as alturas foram definidas em 1.20m e 1.80m respectivamente.
2. O aplicativo deve ser rodado numa vista 3D de Revit.
3. A sala deve estar modelada de modo a não deixar frestas que permitam os raios serem lançados ao vazio. 

## Publicação:
Para aprofundar no assunto consulte a seguinte publicação.

https://periodicos.sbu.unicamp.br/ojs/index.php/parc/article/view/8653934
