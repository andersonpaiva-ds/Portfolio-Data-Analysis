Tendo em mãos um arquivo com dados de pacientes que desenvolveram ou não diabetes. O desafio é gerar uma amostra de dados com os pacientes com mais de 50 anos e para
cada um deles indicar em uma nova coluna se o paciente está normal (índice de massa corpórea
menor que 30) ou obeso (índice de massa corpórea maior ou igual a 30). Então devemos gerar
um novo arquivo CSV.


A resolução deste problema foi feita utilizando Banco de Dados, Python e SQL através da plataforma Jupyter Notebook. 



Os dados foram inicialmente carregados com utilizando Python.
```python
$ df = pd.read_csv(’dataset/diabetes.csv’)
```



Foi então uma cópia da tabela utilizando bandas e enviada para um banco de dados.
```python
df.to_sql(’diabetes’, cnn)
```


Com uso da Linguagem SQL, foram feitas as transformações necessárias. 

Primeiramente, uma nova tabela vazia foi criada para armazenar apenas os dados dos pacientes com mais de 50 anos.
```SQL
%%sql
CREATE TABLE pacientes (Pregnnancies INT,
Glucose INT,
BloodPressure INT,
Insulin INT,
BMI DECIAL(8, 2),
DiabetesPedigreeFunction
DECIMAL(8, 2),
Age INT,
Outcome INT);
```


Então, uma cópia dos dados de todos os pacientes com mais de 50 anos foram transferidos para essa tabela vazia.
```SQL
%%sql
INSERT INTO pacientes (Pregnancies, Glucose, BloodPressure, SkinThickness, Insulin, BMI, DiabetesPedigreeFunction, Age, Outcome)
```
```SQL
SELECT Pregnancies, Glucose, BloodPressure, SkinThickness, Insulin, BMI, DiabetesPedigreeFunction, Age, Outcome

FROM diabetes WHERE Age > 50;
```


Uma nova coluna denominada “Perfil” foi adicionada à tabela e preenchida com a informação de se o paciente é ou não obeso de acordo com seu BMI ( Onde BMI < 30 = Normal, BMI ≥ 30 = Obeso.)
```SQL
%%sql

UPDATE pacientes

SET Perfil = ‘Normal’

WHERE BMI < 30;
```
```SQL
%%sql

UPDATE pacientes

SET Perfil = ‘Obeso’

WHERE BMI ≥ 30;
```


Por fim, os dados foram copiados e transformados de volta para um dataframe do Pandas para salvar o resultado em formato CSV.


Realizo uma consulta a tabela “pacientes” utilizando SQL e armazeno os dados dessa consulta no cursor.

```SQL
query = cnn.execute(”SELECT * FROM pacientes”)

query
```


Utilizando python, criei um looping para percorrer todas as colunas da query e armazenar todos os dados no objeto “cols” .
```python
cols = [coluna[0] for coluna in query.desciption]

cols
```



Gerar o dataframe
```python
resultado = pd.DataFrame.from_records(data = query.fetchall(), columna = cols)
```


Por fim, os dados foram salvos em um arquivo CSV
```python
resultado.to_csv(’dataset/pacientes.csv’, index = False)
```
