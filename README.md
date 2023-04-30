# Acústica2024.dll

Acustica2024.dll é um aplicativo para Revit v.2024 desenvolvido no Departamento de Expressão Gráfica da Escola Politécnica da UFRJ pelo Prof. José Luis Menegotto. O objetivo da API é fazer interface com o simulador sonoro BRASS desenvolvido pelo Prof. Julio Cesar Boscher Torres.

![RAIOS6000](https://user-images.githubusercontent.com/9437020/235116960-2306a9f9-e3fb-4e65-9eb4-82d0fb5b5e5c.PNG)

# Funcionalidade da API:
A API faz a leitura de um modelo BIM criado em Revit v. 2024 para gerar informações que o sistema BRASS precisa para gerar uma simulação sonora de salas. O processo inicia com o posicionamento de uma fonte sonora e um receptor (cabeça) para que seja efetuado uma operação de traçado de raios que partem de uma fonte sonora ideal e omnidirecional com enegia = 1. Os raios percorrem a sala refletindo nas superfícies e perdem energia de acordo aos coeficientes de absorção acústica dos materiais.
Eles são recebidos numa posição definida pelo usuário (cabeça). O processo gera 4 arquivos:

1. RAYS-S001-R001.JSON:        Arquivo estruturado em JSON com os dados do campo acústico com todos os raios sonoros que saíram da fonte e todas as reflexões                                          com as perdas de energia sonora calculadas para nove bandas de frequência.
2. RAYS-S001-R001.TXT:         Arquivo que contem apenas os raios e reflexões que passaram pela posição da cabeça, ou seja, os raios que tem influência na audição de                                  um ouvinte posicionado no ponto da sala definido.
3. Sala-S001-R001.BAT:         Arquivo Bat para executar o BRASS e fazer a leitura e o cálculo de audibilização da sala a partir dos dados do arquivo 
                               RAYS-S001-R001.TXT. O retorno do processo são arquivos WAV com o resultado da audibilização.
4. Sala-S001-R001.TXT:         Arquivo TXT com parâmetros necessários para o BRASS realizar o processo de audibilização da sala.
                               
# Instalação:
 1. Descompactar para a pasta                         C:\APIBIM\Acustica
 3. Copiar o arquivo 2024_Acustica.addin para a pasta C:\ProgramData\Autodesk\Revit\Addins\2024
 
 3. Entre os arquivos instalados na pasta Brass, há um arquivo soprano.wav. Esse arquivo é o som anecóico de uma soprano realizando um exercício vocal com uma 
    escala de alturas. O arquivo Wav resultante da simulação da API e da audibilização calculada no Brass, retornará a mesma sequência cantada com a resposta 
    acústica da sala. 

# Preparação do Modelo:
 1. Os materiais devem ter o parâmetro MARK definido e cadastrado no arquivo Acustica_Coeficientes.TXT.
 Esse arquivo contem os valores dos coeficientes de absorção acústica dos materiais usados na sala divididos em 9 bandas de frequência.
 Consultar com os fornecedores dos materiais para obter esses valores. Cada linha do arquivo corresponde a um material.
 O primeiro dado textual é o código do material que deve ser associado ao parâmetro MARK.

Exemplo:  A02 0.03 0.03 0.03 0.04 0.10 0.19 0.35 0.35 0.35 - BlocoConcreto

![Materiais](https://user-images.githubusercontent.com/9437020/235194809-edbf0873-caee-476c-9103-f7472fd9e6cd.PNG)

 1. A API processa as faces atingidas pelos raios. Foram testadas as familias de sistema de Piso, Parede, Forros e Partes. Elas podem estar agrupadas. 
 2. As famílias de componentes que estejam dentro da sala estudada (cadeiras, refletores ou baffles acústicos, etc.) devem ter um parâmetro instanciado 
    definido como "Material_Acustico" que permita alocar o material utilizado.

![Familia](https://user-images.githubusercontent.com/9437020/235192990-612e1f3e-1af5-45c0-befa-a70cbd703047.PNG)
  
# Interface:
![Interface](https://user-images.githubusercontent.com/9437020/235352580-44726e4d-9f58-4e51-867b-8c1738b936bd.PNG)

 1. Botão de executar o processo de traçado de raios.
 2. Botão para a leitura do campo acústico (Em desenvolvimento...).
 3. Opções para desenhar ou não os raios calculados.
 4. Quantidade de raios a calcular.
 5. Quantidade de reflexões a calcular.
 6. Número da simulação. Será o valor N (RAYS-S00N-R00N.JSON) para nomear os arquivos de JSON, TXT e BAT de cada simulação.

Para os campos 4,5 e 6 Ingressar o valor e Pressionar Enter ou clicar no ícone do enter do campo.

 5. Na linha de Status de Revit serão informadas as interações processadas. Os raios desenhados utilizam a cor amarela que vai sendo clareada e afinada segundo as perdas de energia de cada reflexão. Foi preparada uma gradação de 5 níveis com cor e espessuras de linha diferenciadas.

![contador](https://user-images.githubusercontent.com/9437020/235193199-33ac6d83-b916-4ef3-aa39-495c9d87b74e.png)

# Estilos:
1. A API incorpora 3 estilos de linha ao arquivo do modelo RVT. Eles têm as seguintes características:

![Estilos](https://user-images.githubusercontent.com/9437020/235129574-902e4f05-dd74-4636-836d-337d615d3aef.PNG)

# Desempenho:
1. Os valores de processamento a seguir foram obtidos rodando a API em um notebook Inspiron com processador I7, 8GB de RAM e disco SSD 1T com placa de video Onboard.
Como os processos de audibilização exigem grande quantidade de raios a serem processados, há um impacto para o processamento gráfico em Revit. 
A API permite optar por gerar os arquivos necessários para o BRASS sem precisar desenhar os raios no modelo.
2. Na tabela a seguir os tempos de procesamento sem desenhar os raios

![tempos](https://user-images.githubusercontent.com/9437020/235238074-9609ee4f-8a13-4eed-b684-a295099974db.PNG)

# Limitações:
1. Nesta versão, a altura da fonte e da cabeça foram definidas a 1.20 m e 1.80 m respectivamente.
 
# Publicação:
Para aprofundar no assunto consulte a seguinte publicação.

https://periodicos.sbu.unicamp.br/ojs/index.php/parc/article/view/8653934
