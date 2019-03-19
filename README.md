# analise_IPTU

Análise dos dados do IPTU de São Paulo



```python
import pandas as pd
import re
```


```python
df = pd.read_csv(r'C:\_RAW\TESTE\IPTU_2017.csv',nrows = 15, encoding='latin', sep=';')
```


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NUMERO DO CONTRIBUINTE</th>
      <th>ANO DO EXERCICIO</th>
      <th>NUMERO DA NL</th>
      <th>DATA DO CADASTRAMENTO</th>
      <th>TIPO DE CONTRIBUINTE 1</th>
      <th>CPF/CNPJ DO CONTRIBUINTE 1</th>
      <th>NOME DO CONTRIBUINTE 1</th>
      <th>TIPO DE CONTRIBUINTE 2</th>
      <th>CPF/CNPJ DO CONTRIBUINTE 2</th>
      <th>NOME DO CONTRIBUINTE 2</th>
      <th>...</th>
      <th>ANO DA CONSTRUCAO CORRIGIDO</th>
      <th>QUANTIDADE DE PAVIMENTOS</th>
      <th>TESTADA PARA CALCULO</th>
      <th>TIPO DE USO DO IMOVEL</th>
      <th>TIPO DE PADRAO DA CONSTRUCAO</th>
      <th>TIPO DE TERRENO</th>
      <th>FATOR DE OBSOLESCENCIA</th>
      <th>ANO DE INICIO DA VIDA DO CONTRIBUINTE</th>
      <th>MES DE INICIO DA VIDA DO CONTRIBUINTE</th>
      <th>FASE DO CONTRIBUINTE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0010030001-4</td>
      <td>2017</td>
      <td>1</td>
      <td>14/01/17</td>
      <td>PESSOA FISICA (CPF)</td>
      <td>XXXXXX0214XXXX</td>
      <td>MARCIO MOURCHED</td>
      <td>NaN</td>
      <td></td>
      <td>NaN</td>
      <td>...</td>
      <td>1924</td>
      <td>1</td>
      <td>13,00</td>
      <td>Loja</td>
      <td>Comercial horizontal - padrão B</td>
      <td>De esquina</td>
      <td>0,20</td>
      <td>1963</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0010030002-2</td>
      <td>2017</td>
      <td>1</td>
      <td>14/01/17</td>
      <td>PESSOA FISICA (CPF)</td>
      <td>XXXXXX0214XXXX</td>
      <td>MARCIO MOURCHED</td>
      <td>NaN</td>
      <td></td>
      <td>NaN</td>
      <td>...</td>
      <td>1944</td>
      <td>1</td>
      <td>6,00</td>
      <td>Loja</td>
      <td>Comercial horizontal - padrão B</td>
      <td>Normal</td>
      <td>0,20</td>
      <td>1963</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0010030003-0</td>
      <td>2017</td>
      <td>1</td>
      <td>14/01/17</td>
      <td>PESSOA FISICA (CPF)</td>
      <td>XXXXXX0214XXXX</td>
      <td>MARCIO MOURCHED</td>
      <td>NaN</td>
      <td></td>
      <td>NaN</td>
      <td>...</td>
      <td>1965</td>
      <td>2</td>
      <td>7,85</td>
      <td>Loja</td>
      <td>Comercial horizontal - padrão B</td>
      <td>Normal</td>
      <td>0,35</td>
      <td>1963</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0010030004-9</td>
      <td>2017</td>
      <td>1</td>
      <td>14/01/17</td>
      <td>PESSOA FISICA (CPF)</td>
      <td>XXXXXX2094XXXX</td>
      <td>AUGUSTO CESAR DE MATTOS JUNIOR</td>
      <td>NaN</td>
      <td></td>
      <td>NaN</td>
      <td>...</td>
      <td>1944</td>
      <td>1</td>
      <td>6,05</td>
      <td>Loja</td>
      <td>Comercial horizontal - padrão B</td>
      <td>Normal</td>
      <td>0,20</td>
      <td>1963</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0010030005-7</td>
      <td>2017</td>
      <td>1</td>
      <td>14/01/17</td>
      <td>PESSOA FISICA (CPF)</td>
      <td>XXXXXX2094XXXX</td>
      <td>AUGUSTO CESAR DE MATTOS JUNIOR</td>
      <td>NaN</td>
      <td></td>
      <td>NaN</td>
      <td>...</td>
      <td>1944</td>
      <td>1</td>
      <td>6,70</td>
      <td>Loja</td>
      <td>Comercial horizontal - padrão B</td>
      <td>Normal</td>
      <td>0,20</td>
      <td>1963</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 35 columns</p>
</div>



# Catálogo de Tipologia dos Logradouros

Obtido o XLSX no metadados do Geosampa, com as categorias dos logradouros


```python
df_log = pd.read_clipboard('\\t')
```


```python
df_log['Sigla'] = df_log['DADO']
df_log
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DADO</th>
      <th>Sigla</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AL - Alameda</td>
      <td>AL - Alameda</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AV - Avenida</td>
      <td>AV - Avenida</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BC - Beco</td>
      <td>BC - Beco</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CM - Caminho</td>
      <td>CM - Caminho</td>
    </tr>
    <tr>
      <th>4</th>
      <td>CMP - Caminho particular</td>
      <td>CMP - Caminho particular</td>
    </tr>
    <tr>
      <th>5</th>
      <td>CP - Caminho de pedestre</td>
      <td>CP - Caminho de pedestre</td>
    </tr>
    <tr>
      <th>6</th>
      <td>CV - Complexo Viário</td>
      <td>CV - Complexo Viário</td>
    </tr>
    <tr>
      <th>7</th>
      <td>EL - Espaço livre</td>
      <td>EL - Espaço livre</td>
    </tr>
    <tr>
      <th>8</th>
      <td>EPL -  Esplanada</td>
      <td>EPL -  Esplanada</td>
    </tr>
    <tr>
      <th>9</th>
      <td>ES - Estrada</td>
      <td>ES - Estrada</td>
    </tr>
    <tr>
      <th>10</th>
      <td>ESC - Escadaria</td>
      <td>ESC - Escadaria</td>
    </tr>
    <tr>
      <th>11</th>
      <td>ESP - Estrada particular</td>
      <td>ESP - Estrada particular</td>
    </tr>
    <tr>
      <th>12</th>
      <td>EST - estacionamento</td>
      <td>EST - estacionamento</td>
    </tr>
    <tr>
      <th>13</th>
      <td>GL - galeria</td>
      <td>GL - galeria</td>
    </tr>
    <tr>
      <th>14</th>
      <td>JD - Jardim</td>
      <td>JD - Jardim</td>
    </tr>
    <tr>
      <th>15</th>
      <td>LD - Ladeira</td>
      <td>LD - Ladeira</td>
    </tr>
    <tr>
      <th>16</th>
      <td>LG - Largo</td>
      <td>LG - Largo</td>
    </tr>
    <tr>
      <th>17</th>
      <td>PA - Passarela</td>
      <td>PA - Passarela</td>
    </tr>
    <tr>
      <th>18</th>
      <td>PC - Praça</td>
      <td>PC - Praça</td>
    </tr>
    <tr>
      <th>19</th>
      <td>PCR - Praça de retorno</td>
      <td>PCR - Praça de retorno</td>
    </tr>
    <tr>
      <th>20</th>
      <td>PP - Passagem de pedestres</td>
      <td>PP - Passagem de pedestres</td>
    </tr>
    <tr>
      <th>21</th>
      <td>PQ - Parque</td>
      <td>PQ - Parque</td>
    </tr>
    <tr>
      <th>22</th>
      <td>PS - Passagem</td>
      <td>PS - Passagem</td>
    </tr>
    <tr>
      <th>23</th>
      <td>PSP - Passagem particular</td>
      <td>PSP - Passagem particular</td>
    </tr>
    <tr>
      <th>24</th>
      <td>PSS - Passagem subterrânea</td>
      <td>PSS - Passagem subterrânea</td>
    </tr>
    <tr>
      <th>25</th>
      <td>PT - Pátio</td>
      <td>PT - Pátio</td>
    </tr>
    <tr>
      <th>26</th>
      <td>PTE - Ponte</td>
      <td>PTE - Ponte</td>
    </tr>
    <tr>
      <th>27</th>
      <td>PTL - pontilhão</td>
      <td>PTL - pontilhão</td>
    </tr>
    <tr>
      <th>28</th>
      <td>R - Rua</td>
      <td>R - Rua</td>
    </tr>
    <tr>
      <th>29</th>
      <td>RP - Rua particular</td>
      <td>RP - Rua particular</td>
    </tr>
    <tr>
      <th>30</th>
      <td>RPJ - Rua projetada</td>
      <td>RPJ - Rua projetada</td>
    </tr>
    <tr>
      <th>31</th>
      <td>RV - rodovia</td>
      <td>RV - rodovia</td>
    </tr>
    <tr>
      <th>32</th>
      <td>SV - servidão</td>
      <td>SV - servidão</td>
    </tr>
    <tr>
      <th>33</th>
      <td>TN - túnel</td>
      <td>TN - túnel</td>
    </tr>
    <tr>
      <th>34</th>
      <td>TPJ - Travessa Projetada</td>
      <td>TPJ - Travessa Projetada</td>
    </tr>
    <tr>
      <th>35</th>
      <td>TV - Travessa</td>
      <td>TV - Travessa</td>
    </tr>
    <tr>
      <th>36</th>
      <td>TVP - Travessa particular</td>
      <td>TVP - Travessa particular</td>
    </tr>
    <tr>
      <th>37</th>
      <td>VCP - Via de circulação de pedestres</td>
      <td>VCP - Via de circulação de pedestres</td>
    </tr>
    <tr>
      <th>38</th>
      <td>VD - Viaduto</td>
      <td>VD - Viaduto</td>
    </tr>
    <tr>
      <th>39</th>
      <td>VE - Viela</td>
      <td>VE - Viela</td>
    </tr>
    <tr>
      <th>40</th>
      <td>VEL - Via elevada</td>
      <td>VEL - Via elevada</td>
    </tr>
    <tr>
      <th>41</th>
      <td>VEP - Viela particular</td>
      <td>VEP - Viela particular</td>
    </tr>
    <tr>
      <th>42</th>
      <td>VER - Vereda</td>
      <td>VER - Vereda</td>
    </tr>
    <tr>
      <th>43</th>
      <td>VES - Viela Sanitária</td>
      <td>VES - Viela Sanitária</td>
    </tr>
    <tr>
      <th>44</th>
      <td>VIA - Via</td>
      <td>VIA - Via</td>
    </tr>
    <tr>
      <th>45</th>
      <td>VL - Vila</td>
      <td>VL - Vila</td>
    </tr>
    <tr>
      <th>46</th>
      <td>VLP - Vila particular</td>
      <td>VLP - Vila particular</td>
    </tr>
    <tr>
      <th>47</th>
      <td>VP - Via de pedestre</td>
      <td>VP - Via de pedestre</td>
    </tr>
  </tbody>
</table>
</div>




```python
df_log['Sigla'] =  [i.split(' -')[0] for i in df_log['DADO']]
df_log['Tipo'] =  [i.split(' -')[1][1:].title() for i in df_log['DADO']]
df_log
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>DADO</th>
      <th>Sigla</th>
      <th>Tipo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AL - Alameda</td>
      <td>AL</td>
      <td>Alameda</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AV - Avenida</td>
      <td>AV</td>
      <td>Avenida</td>
    </tr>
    <tr>
      <th>2</th>
      <td>BC - Beco</td>
      <td>BC</td>
      <td>Beco</td>
    </tr>
    <tr>
      <th>3</th>
      <td>CM - Caminho</td>
      <td>CM</td>
      <td>Caminho</td>
    </tr>
    <tr>
      <th>4</th>
      <td>CMP - Caminho particular</td>
      <td>CMP</td>
      <td>Caminho Particular</td>
    </tr>
    <tr>
      <th>5</th>
      <td>CP - Caminho de pedestre</td>
      <td>CP</td>
      <td>Caminho De Pedestre</td>
    </tr>
    <tr>
      <th>6</th>
      <td>CV - Complexo Viário</td>
      <td>CV</td>
      <td>Complexo Viário</td>
    </tr>
    <tr>
      <th>7</th>
      <td>EL - Espaço livre</td>
      <td>EL</td>
      <td>Espaço Livre</td>
    </tr>
    <tr>
      <th>8</th>
      <td>EPL -  Esplanada</td>
      <td>EPL</td>
      <td>Esplanada</td>
    </tr>
    <tr>
      <th>9</th>
      <td>ES - Estrada</td>
      <td>ES</td>
      <td>Estrada</td>
    </tr>
    <tr>
      <th>10</th>
      <td>ESC - Escadaria</td>
      <td>ESC</td>
      <td>Escadaria</td>
    </tr>
    <tr>
      <th>11</th>
      <td>ESP - Estrada particular</td>
      <td>ESP</td>
      <td>Estrada Particular</td>
    </tr>
    <tr>
      <th>12</th>
      <td>EST - estacionamento</td>
      <td>EST</td>
      <td>Estacionamento</td>
    </tr>
    <tr>
      <th>13</th>
      <td>GL - galeria</td>
      <td>GL</td>
      <td>Galeria</td>
    </tr>
    <tr>
      <th>14</th>
      <td>JD - Jardim</td>
      <td>JD</td>
      <td>Jardim</td>
    </tr>
    <tr>
      <th>15</th>
      <td>LD - Ladeira</td>
      <td>LD</td>
      <td>Ladeira</td>
    </tr>
    <tr>
      <th>16</th>
      <td>LG - Largo</td>
      <td>LG</td>
      <td>Largo</td>
    </tr>
    <tr>
      <th>17</th>
      <td>PA - Passarela</td>
      <td>PA</td>
      <td>Passarela</td>
    </tr>
    <tr>
      <th>18</th>
      <td>PC - Praça</td>
      <td>PC</td>
      <td>Praça</td>
    </tr>
    <tr>
      <th>19</th>
      <td>PCR - Praça de retorno</td>
      <td>PCR</td>
      <td>Praça De Retorno</td>
    </tr>
    <tr>
      <th>20</th>
      <td>PP - Passagem de pedestres</td>
      <td>PP</td>
      <td>Passagem De Pedestres</td>
    </tr>
    <tr>
      <th>21</th>
      <td>PQ - Parque</td>
      <td>PQ</td>
      <td>Parque</td>
    </tr>
    <tr>
      <th>22</th>
      <td>PS - Passagem</td>
      <td>PS</td>
      <td>Passagem</td>
    </tr>
    <tr>
      <th>23</th>
      <td>PSP - Passagem particular</td>
      <td>PSP</td>
      <td>Passagem Particular</td>
    </tr>
    <tr>
      <th>24</th>
      <td>PSS - Passagem subterrânea</td>
      <td>PSS</td>
      <td>Passagem Subterrânea</td>
    </tr>
    <tr>
      <th>25</th>
      <td>PT - Pátio</td>
      <td>PT</td>
      <td>Pátio</td>
    </tr>
    <tr>
      <th>26</th>
      <td>PTE - Ponte</td>
      <td>PTE</td>
      <td>Ponte</td>
    </tr>
    <tr>
      <th>27</th>
      <td>PTL - pontilhão</td>
      <td>PTL</td>
      <td>Pontilhão</td>
    </tr>
    <tr>
      <th>28</th>
      <td>R - Rua</td>
      <td>R</td>
      <td>Rua</td>
    </tr>
    <tr>
      <th>29</th>
      <td>RP - Rua particular</td>
      <td>RP</td>
      <td>Rua Particular</td>
    </tr>
    <tr>
      <th>30</th>
      <td>RPJ - Rua projetada</td>
      <td>RPJ</td>
      <td>Rua Projetada</td>
    </tr>
    <tr>
      <th>31</th>
      <td>RV - rodovia</td>
      <td>RV</td>
      <td>Rodovia</td>
    </tr>
    <tr>
      <th>32</th>
      <td>SV - servidão</td>
      <td>SV</td>
      <td>Servidão</td>
    </tr>
    <tr>
      <th>33</th>
      <td>TN - túnel</td>
      <td>TN</td>
      <td>Túnel</td>
    </tr>
    <tr>
      <th>34</th>
      <td>TPJ - Travessa Projetada</td>
      <td>TPJ</td>
      <td>Travessa Projetada</td>
    </tr>
    <tr>
      <th>35</th>
      <td>TV - Travessa</td>
      <td>TV</td>
      <td>Travessa</td>
    </tr>
    <tr>
      <th>36</th>
      <td>TVP - Travessa particular</td>
      <td>TVP</td>
      <td>Travessa Particular</td>
    </tr>
    <tr>
      <th>37</th>
      <td>VCP - Via de circulação de pedestres</td>
      <td>VCP</td>
      <td>Via De Circulação De Pedestres</td>
    </tr>
    <tr>
      <th>38</th>
      <td>VD - Viaduto</td>
      <td>VD</td>
      <td>Viaduto</td>
    </tr>
    <tr>
      <th>39</th>
      <td>VE - Viela</td>
      <td>VE</td>
      <td>Viela</td>
    </tr>
    <tr>
      <th>40</th>
      <td>VEL - Via elevada</td>
      <td>VEL</td>
      <td>Via Elevada</td>
    </tr>
    <tr>
      <th>41</th>
      <td>VEP - Viela particular</td>
      <td>VEP</td>
      <td>Viela Particular</td>
    </tr>
    <tr>
      <th>42</th>
      <td>VER - Vereda</td>
      <td>VER</td>
      <td>Vereda</td>
    </tr>
    <tr>
      <th>43</th>
      <td>VES - Viela Sanitária</td>
      <td>VES</td>
      <td>Viela Sanitária</td>
    </tr>
    <tr>
      <th>44</th>
      <td>VIA - Via</td>
      <td>VIA</td>
      <td>Via</td>
    </tr>
    <tr>
      <th>45</th>
      <td>VL - Vila</td>
      <td>VL</td>
      <td>Vila</td>
    </tr>
    <tr>
      <th>46</th>
      <td>VLP - Vila particular</td>
      <td>VLP</td>
      <td>Vila Particular</td>
    </tr>
    <tr>
      <th>47</th>
      <td>VP - Via de pedestre</td>
      <td>VP</td>
      <td>Via De Pedestre</td>
    </tr>
  </tbody>
</table>
</div>



# SQL
Aqui se espera fazer um teste do SQL

```sql
SELECT * FROM dataframe
```
