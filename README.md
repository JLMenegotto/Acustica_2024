# Acústica 2024

![RAIOS6000](https://user-images.githubusercontent.com/9437020/235116960-2306a9f9-e3fb-4e65-9eb4-82d0fb5b5e5c.PNG)

Acustica2024.dll é um aplicativo para Revit v.2024 desenvolvido no Departamento de Expressão Gráfica da Escola Politécnica da UFRJ pelo Prof. José Luis Menegotto.
O objetivo da API é fazer interface com o simulador sonoro BRASS desenvolvido pelo Prof. Julio Cesar Boscher Torres.

# Função da API:
A API gera um arquivo com os raios sonoros que partem de uma fonte omnidirecional e são recebidos numa posição definida pelo usuário (cabeça).
O processo gera 3 arquivos.

1. Campo_Acustico.JSON:        Arquivo estruturado em JSON com os dados do campo acústico com todos os raios calculados.
2. RAYS-S001-R001.TXT:         Arquivo com os raios que passam pela posição da cabeça, ou seja, os raios que tem influência na audição de um ouvinte.
3. Gera_Wave_SalaSimulada.BAT: Arquivo Bat para executar o BRASS e fazer o cálculo de audibilização da sala com os dados do RAYS-S001-R001.TXT.
                               O retorno do processo são arquivos WAV com o resultado da audibilização.
                               
# Instalação:

 1. Descompactar para a pasta                         C:\APIBIM\Acustica
 2. Copiar o arquivo 2024_Acustica.addin para a pasta C:\ProgramData\Autodesk\Revit\Addins\2024
           
# Preparação do Modelo:

 1. Os materiais devem ter o parâmetro MARK definido e cadastrado no arquivo Acustica_Coeficientes.TXT
 Esse arquivo contem os valores dos coeficientes de absorção acústica dos materiais usados na sala divididos em 9 bandas de frequência.
 Consultar com os fornecedores dos materiais para obter esses valores. Cada linha do arquivo corresponde a um material.
 O primeiro dado textual é o código do material que deve ser associado ao parâmetro MARK.

Exemplo:

A02 0.03 0.03 0.03 0.04 0.10 0.19 0.35 0.35 0.35 - BlocoConcreto

![Materiais](https://user-images.githubusercontent.com/9437020/235194809-edbf0873-caee-476c-9103-f7472fd9e6cd.PNG)


 2. A API processa as faces atingidas pelos raios. Foram testadas as familias de sistema de Piso, Parede, Forros, Partes que podem estar agrupadas. 
 3. As famílias de componentes que estejam dentro da sala estudada (cadeiras, refletores ou baffles acústicos, etc.) devem ter um parâmetro instanciado 
    definido como "Material_Acustico" para alocar o valor de material. 

![Familia](https://user-images.githubusercontent.com/9437020/235192990-612e1f3e-1af5-45c0-befa-a70cbd703047.PNG)

  
# Interface:
 1. Botão de executar o processo de traçado de raios.
 2. Botão para a leitura do campo acústico. (Em desenvolvimento...).
 3. Opções para desenhar ou não os raios calculados.
 4. Quantidade de raios e quantidade de reflexões a calcular (Ingressar o valor e Pressionar Enter ou clicar no ícone do enter do campo).

![Interface](https://user-images.githubusercontent.com/9437020/235127905-42c1eeeb-3225-4337-9e11-fd8732b48068.PNG)

 5. Na linha de Status de Revit serão informadas as interações processadas. Os raios desenhados utilizam a cor amarela que vai sendo clareada e afinada segundo as perdas de energia de cada reflexão. Foi preparada uma gradação de 5 níveis com cor e espessuras de linha diferenciadas.

![contador](https://user-images.githubusercontent.com/9437020/235193199-33ac6d83-b916-4ef3-aa39-495c9d87b74e.png)

# Estilos:

1. A API incorpora ao arquivo do modelo RVT 3 estilos de linha com as seguintes características.

![Estilos](https://user-images.githubusercontent.com/9437020/235129574-902e4f05-dd74-4636-836d-337d615d3aef.PNG)

# Desempenho:

1. Os valores de processamento a seguir foram obtidos rodando a API em um notebook Inspiron com processador I7, 8GB de RAM e disco SSD 1T com placa de video Onboard.
Como os processos de audibilização exigem grande quantidade de raios a serem processados, isso tem um impacto de processamento gráfico em Revit. O usuário pode optar gerar os arquivos necessários para o BRASS sem ter que desenhar os raios no modelo.
2. Na tabela a seguir os tempos de procesamento sem desenhar os raios

![tempos](https://user-images.githubusercontent.com/9437020/235225832-aa937647-636d-4791-9c3e-87f54ff0b812.PNG)

