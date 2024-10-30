# SQL Practice Hub

---

# Consultas SQL

Este documento contém uma coleção de consultas SQL para gerenciamento de filmes, países, clientes e outras operações relacionadas ao sistema de locação de filmes, propostas pela professora fiama brenda do senai suiço no curso de ads.

## Consultas

**1. Quais os países cadastrados?**
```sql
SELECT * FROM PAIS;
```
**2. Quantos países estão cadastrados?**
```sql
SELECT COUNT(*) FROM PAIS;
```

**3. Quantos países que terminam com a letra "A" estão cadastrados?**
```sql
SELECT COUNT(*) 
FROM PAIS 
WHERE PAIS LIKE '%A';
```
**4. Listar, sem repetição, os anos que houveram lançamento de filme.**
```sql
SELECT DISTINCT ANO_DE_LANCAMENTO 
FROM FILME;
```
**5. Alterar o ano de lançamento igual 2007 para filmes que iniciem com a Letra "B".**
```sql
UPDATE FILME 
SET ANO_DE_LANCAMENTO = 2007 
WHERE TITULO LIKE 'B%';
```

**6. Listar os filmes que possuem duração do filme maior que 100 e classificação igual a "G".**
```sql
SELECT TITULO 
FROM FILME 
WHERE DURACAO_DO_FILME > 100 
AND CLASSIFICACAO = 'G';
```

**7. Alterar o ano de lançamento igual 2008 para filmes que possuem duração da locação menor que 4 e classificação igual a "PG".**
```sql
UPDATE FILME 
SET ANO_DE_LANCAMENTO = 2008 
WHERE DURACAO_DA_LOCACAO < 4 
AND CLASSIFICACAO = 'PG';
```

**8. Listar a quantidade de filmes que estejam classificados como "PG-13" e preço da locação maior que 2.40.**
```sql
SELECT COUNT(*) 
FROM FILME 
WHERE CLASSIFICACAO = 'PG-13' 
AND PRECO_DA_LOCACAO > 2.40;
```

**9. Quais os idiomas que possuem filmes cadastrados?**
```sql
SELECT DISTINCT I.NOME 
FROM IDIOMA I
JOIN FILME F ON I.IDIOMA_ID = F.IDIOMA_ID;
```

**10. Alterar o idioma para "GERMAN" dos filmes que possuem preço da locação maior que 4.**
```sql
UPDATE FILME 
SET IDIOMA_ID = (SELECT IDIOMA_ID FROM IDIOMA WHERE NOME = 'GERMAN')
WHERE PRECO_DA_LOCACAO > 4;
```

**11. Alterar o idioma para "JAPANESE" dos filmes que possuem preço da locação igual 0.99.**
```sql
UPDATE FILME 
SET IDIOMA_ID = (SELECT IDIOMA_ID FROM IDIOMA WHERE NOME = 'JAPANESE')
WHERE PRECO_DA_LOCACAO = 0.99;
```

**12. Listar a quantidade de filmes por classificação.**
```sql
SELECT COUNT(*), CLASSIFICACAO 
FROM FILME 
GROUP BY CLASSIFICACAO;
```
**13. Listar, sem repetição, os preços de locação dos filmes cadastrados.**
```sql
SELECT DISTINCT PRECO_DA_LOCACAO 
FROM FILME;
```

**14. Listar a quantidade de filmes por preço da locação.**
```sql
SELECT COUNT(*), PRECO_DA_LOCACAO 
FROM FILME 
GROUP BY PRECO_DA_LOCACAO;
```

**15. Listar os preços da locação que possuam mais de 340 filmes.**
```sql
SELECT PRECO_DA_LOCACAO, COUNT(*)
FROM FILME 
GROUP BY PRECO_DA_LOCACAO 
HAVING COUNT(*) > 340;
```

**16. Listar a quantidade de atores por filme ordenando por quantidade de atores crescente.**
```sql
SELECT F.TITULO, COUNT(A.ATOR_ID) AS QTD_ATORES
FROM FILME F
JOIN FILME_ATOR FA ON F.FILME_ID = FA.FILME_ID
JOIN ATOR A ON A.ATOR_ID = FA.ATOR_ID
GROUP BY F.TITULO
ORDER BY QTD_ATORES ASC;
```

**17. Listar a quantidade de atores para os filmes que possuem mais de 5 atores ordenando por quantidade de atores decrescente.**
```sql
SELECT F.TITULO, COUNT(A.ATOR_ID) AS QTD_ATORES
FROM FILME F
JOIN FILME_ATOR FA ON F.FILME_ID = FA.FILME_ID
JOIN ATOR A ON A.ATOR_ID = FA.ATOR_ID
GROUP BY F.TITULO
HAVING COUNT(A.ATOR_ID) > 5
ORDER BY QTD_ATORES DESC;
```

**18. Listar o título e a quantidade de atores para os filmes que possuem o idioma "JAPANESE" e mais de 10 atores ordenando por ordem alfabética de título e ordem crescente de quantidade de atores.**

```sql
SELECT F.TITULO, COUNT(A.ATOR_ID) AS QTD_ATORES
FROM FILME F
JOIN FILME_ATOR FA ON F.FILME_ID = FA.FILME_ID
JOIN ATOR A ON A.ATOR_ID = FA.ATOR_ID
JOIN IDIOMA I ON F.IDIOMA_ID = I.IDIOMA_ID
WHERE I.NOME = 'JAPANESE'
GROUP BY F.TITULO
HAVING COUNT(A.ATOR_ID) > 10
ORDER BY F.TITULO ASC, QTD_ATORES ASC;
```

**19. Qual a maior duração da locação dentre os filmes?**

```sql
SELECT MAX(DURACAO_DA_LOCACAO) 
FROM FILME;
```

**20. Quantos filmes possuem a maior duração de locação?**
```sql
SELECT COUNT(*)
FROM FILME
WHERE DURACAO_DA_LOCACAO = (SELECT MAX(DURACAO_DA_LOCACAO) FROM FILME);
```

**21. Quantos filmes do idioma "JAPANESE" ou "GERMAN" possuem a maior duração de locação?**
```sql
SELECT COUNT(*)
FROM FILME F
JOIN IDIOMA I ON F.IDIOMA_ID = I.IDIOMA_ID
WHERE F.DURACAO_DA_LOCACAO = (SELECT MAX(DURACAO_DA_LOCACAO) FROM FILME)
AND (I.NOME = 'JAPANESE' OR I.NOME = 'GERMAN');
```

**22. Qual a quantidade de filmes por classificação e preço da locação?**
```sql
SELECT COUNT(*), CLASSIFICACAO, PRECO_DA_LOCACAO 
FROM FILME 
GROUP BY CLASSIFICACAO, PRECO_DA_LOCACAO;
```

**23. Qual o maior tempo de duração de filme por categoria?**
```sql
SELECT C.NOME, MAX(F.DURACAO_DO_FILME)
FROM CATEGORIA C
JOIN FILME_CATEGORIA FC ON C.CATEGORIA_ID = FC.CATEGORIA_ID
JOIN FILME F ON F.FILME_ID = FC.FILME_ID
GROUP BY C.NOME;
```

**24. Listar a quantidade de filmes por categoria.**
```sql
SELECT C.NOME, COUNT(F.TITULO) AS QTD_FILMES
FROM CATEGORIA C
JOIN FILME_CATEGORIA FC ON C.CATEGORIA_ID = FC.CATEGORIA_ID
JOIN FILME F ON F.FILME_ID = FC.FILME_ID
GROUP BY C.NOME;
```

**25. Listar a quantidade de filmes classificados como "G" por categoria.**
```sql
SELECT C.NOME, COUNT(F.TITULO) AS QTD_FILMES
FROM CATEGORIA C
JOIN FILME_CATEGORIA FC ON C.CATEGORIA_ID = FC.CATEGORIA_ID
JOIN FILME F ON F.FILME_ID = FC.FILME_ID
WHERE F.CLASSIFICACAO = 'G'
GROUP BY C.NOME;
```

**26. Listar a quantidade de filmes classificados como "G" OU "PG" por categoria.**
```sql
SELECT C.NOME, COUNT(F.TITULO) AS QTD_FILMES
FROM CATEGORIA C
JOIN FILME_CATEGORIA FC ON C.CATEGORIA_ID = FC.CATEGORIA_ID
JOIN FILME F ON F.FILME_ID = FC.FILME_ID
WHERE F.CLASSIFICACAO IN ('G', 'PG')
GROUP BY C.NOME;
```

**27. Listar a quantidade de filmes por categoria e classificação.**
```sql
SELECT C.NOME, F.CLASSIFICACAO, COUNT(F.TITULO) AS QTD_FILMES
FROM CATEGORIA C
JOIN FILME_CATEGORIA FC ON C.CATEGORIA_ID = FC.CATEGORIA_ID
JOIN FILME F ON F.FILME_ID = FC.FILME_ID
GROUP BY C.NOME, F.CLASSIFICACAO;
```

**28. Qual a quantidade de filmes por Ator ordenando decrescente por quantidade?**
```sql
SELECT A.PRIMEIRO_NOME, A.ULTIMO_NOME, COUNT(F.TITULO) AS QTD_FILMES
FROM ATOR A
JOIN FILME_ATOR FA ON A.ATOR_ID = FA.ATOR_ID
JOIN FILME F ON F.FILME_ID = FA.FILME_ID
GROUP BY A.PRIMEIRO_NOME, A.ULTIMO_NOME
ORDER BY QTD_FILMES DESC;
```

** 29. Qual a quantidade de filmes por ano de lançamento ordenando por quantidade crescente?**
```sql
SELECT F.ANO_DE_LANCAMENTO, COUNT(F.TITULO) AS QTD_FILMES
FROM FILME F
GROUP BY F.ANO_DE_LANCAMENTO
ORDER BY QTD_FILMES ASC;
```

**30. Listar os anos de lançamento que possuem mais de 400 filmes.**
```sql
SELECT ANO_DE_LANCAMENTO, COUNT(TITULO) AS QTD_FILMES
FROM FILME
GROUP BY ANO_DE_LANCAMENTO
HAVING COUNT(TITULO) > 400;
```

**31. Listar os anos de lançamento dos filmes que possuem mais de 100 filmes com preço da locação maior que a média do preço da locação dos filmes da categoria "Children".**
```sql
SELECT F.ANO_DE_LANCAMENTO, COUNT(F.TITULO) AS QTD_FILMES
FROM FILME F
JOIN FILME_CATEGORIA FC ON F.FILME_ID = FC.FILME_ID
JOIN CATEGORIA C ON FC.CATEGORIA_ID = C.CATEGORIA_ID
WHERE F.PRECO_DA_LOCACAO > (
SELECT AVG(FILME.PRECO_DA_LOCACAO) 
FROM FILME 
JOIN FILME_CATEGORIA FC ON FILME.FILME_ID = FC.FILME_ID
JOIN CATEGORIA C ON FC.CATEGORIA_ID = C.CATEGORIA_ID
WHERE C.NOME = 'CHILDREN')
GROUP BY F.ANO_DE_LANCAMENTO
HAVING COUNT(F.TITULO) > 100;
```

**32. Quais as cidades e seu país correspondente que pertencem a um país que inicie com a Letra “E”?**
```sql
SELECT C.CIDADE, P.PAIS 
FROM CIDADE C
JOIN PAIS P ON C.PAIS_ID = P.PAIS_ID
WHERE P.PAIS LIKE 'E%';
```

**33. Qual a quantidade de cidades por país em ordem decrescente?**
```sql
SELECT P.PAIS, COUNT(C.CIDADE) AS QTD_CIDADES
FROM PAIS P
JOIN CIDADE C ON P.PAIS_ID = C.PAIS_ID
GROUP BY P.PAIS
ORDER BY QTD_CIDADES DESC;
```

**34. Qual a quantidade de cidades que iniciam com a Letra “A” por país em ordem crescente?**
```sql
SELECT P.PAIS, COUNT(C.CIDADE) AS QTD_CIDADES
FROM PAIS P
JOIN CIDADE C ON P.PAIS_ID = C.PAIS_ID
WHERE C.CIDADE LIKE 'A%'
GROUP BY P.PAIS
ORDER BY P.PAIS ASC;
```

** 35. Quais os países que possuem mais de 3 cidades que iniciam com a Letra “A”?**
```sql
SELECT P.PAIS, COUNT(C.CIDADE) AS QTD_CIDADES
FROM PAIS P
JOIN CIDADE C ON P.PAIS_ID = C.PAIS_ID
WHERE C.CIDADE LIKE 'A%'
GROUP BY P.PAIS
HAVING COUNT(C.CIDADE) > 3;
```

**36. Quais os países que possuem mais de 3 cidades que iniciam com a Letra “A” ou tenham "R" ordenando por quantidade crescente?**
```sql
SELECT P.PAIS, COUNT(C.CIDADE) AS QTD_CIDADES
FROM PAIS P
JOIN CIDADE C ON P.PAIS_ID = C.PAIS_ID
WHERE C.CIDADE LIKE 'A%' OR C.CIDADE LIKE '%R%'
GROUP BY P.PAIS
HAVING COUNT(C.CIDADE) > 3
ORDER BY COUNT(C.CIDADE) ASC;
```

**37. Quais os clientes que moram no país “United States”?**
```sql
SELECT C.PRIMEIRO_NOME, C.ULTIMO_NOME
FROM CLIENTE C
JOIN ENDERECO E ON C.ENDERECO_ID = E.ENDERECO_ID
JOIN CIDADE CI ON E.CIDADE_ID = CI.CIDADE_ID
JOIN PAIS P ON CI.PAIS_ID = P.PAIS_ID
WHERE P.PAIS = 'UNITED STATES';
```

**38. Quantos clientes moram no país “Brazil”?**
```sql
SELECT COUNT(C.CLIENTE_ID) AS QTD_CLIENTES
FROM CLIENTE C
JOIN ENDERECO E ON C.ENDERECO_ID = E.ENDERECO_ID
JOIN CIDADE CI ON E.CIDADE_ID = CI.CIDADE_ID
JOIN PAIS P ON CI.PAIS_ID = P.PAIS_ID
WHERE P.PAIS = 'BRAZIL';
```

**39. Qual a quantidade de clientes por país?**
```sql
SELECT P.PAIS, COUNT(C.CLIENTE_ID) AS QTD_CLIENTES
FROM CLIENTE C
JOIN ENDERECO E ON C.ENDERECO_ID = E.ENDERECO_ID
JOIN CIDADE CI ON E.CIDADE_ID = CI.CIDADE_ID
JOIN PAIS P ON CI.PAIS_ID = P.PAIS_ID
GROUP BY P.PAIS;
```

**40. Quais países possuem mais de 10 clientes?**
```sql
SELECT P.PAIS, COUNT(C.CLIENTE_ID) AS QTD_CLIENTES
FROM CLIENTE C
JOIN ENDERECO E ON C.ENDERECO_ID = E.ENDERECO_ID
JOIN CIDADE CI ON E.CIDADE_ID = CI.CIDADE_ID
JOIN PAIS P ON CI.PAIS_ID = P.PAIS_ID
GROUP BY P.PAIS
HAVING COUNT(C.CLIENTE_ID) > 10;
```

**41. Qual a média de duração dos filmes por idioma?**
```sql
SELECT I.NOME, AVG(F.DURACAO_DO_FILME) AS MEDIA_DURACAO
FROM IDIOMA I
JOIN FILME F ON I.IDIOMA_ID = F.IDIOMA_ID
GROUP BY I.NOME;
```

**42. Qual a quantidade de atores que atuaram nos filmes do idioma “English”?**
```sql
SELECT COUNT(A.ATOR_ID) AS QTD_ATORES, F.TITULO
FROM ATOR A
JOIN FILME_ATOR FA ON A.ATOR_ID = FA.ATOR_ID
JOIN FILME F ON FA.FILME_ID = F.FILME_ID
JOIN IDIOMA I ON F.IDIOMA_ID = I.IDIOMA_ID
WHERE I.NOME = 'ENGLISH'
GROUP BY F.TITULO;
```

**43. Quais os atores do filme “BLANKET BEVERLY”?**
```sql
SELECT A.PRIMEIRO_NOME, A.ULTIMO_NOME
FROM ATOR A
JOIN FILME_ATOR FA ON A.ATOR_ID = FA.ATOR_ID
JOIN FILME F ON FA.FILME_ID = F.FILME_ID
WHERE F.TITULO = 'BLANKET BEVERLY';
```

**44. Quais categorias possuem mais de 60 filmes cadastrados?**
```sql
SELECT C.NOME, COUNT(F.FILME_ID) AS QTD_FILMES
FROM CATEGORIA C
JOIN FILME_CATEGORIA FC ON C.CATEGORIA_ID = FC.CATEGORIA_ID
JOIN FILME F ON F.FILME_ID = FC.FILME_ID
GROUP BY C.NOME
HAVING COUNT(F.FILME_ID) > 60;
```

**45. Quais os filmes alugados (sem repetição) para clientes que moram na “Argentina”?**
```sql
SELECT DISTINCT F.TITULO
FROM FILME F
JOIN INVENTARIO I ON F.FILME_ID = I.FILME_ID
JOIN ALUGUEL A ON I.INVENTARIO_ID = A.INVENTARIO_ID
JOIN CLIENTE C ON A.CLIENTE_ID = C.CLIENTE_ID
JOIN ENDERECO E ON C.ENDERECO_ID = E.ENDERECO_ID
JOIN CIDADE CI ON E.CIDADE_ID = CI.CIDADE_ID
JOIN PAIS P ON CI.PAIS_ID = P.PAIS_ID
WHERE P.PAIS = 'ARGENTINA';
```

**46. Qual a quantidade de filmes alugados por clientes que moram no “Chile”?**
```sql
SELECT COUNT(F.TITULO) AS QTD_FILMES
FROM FILME F
JOIN INVENTARIO I ON F.FILME_ID = I.FILME_ID
JOIN ALUGUEL A ON I.INVENTARIO_ID = A.INVENTARIO_ID
JOIN CLIENTE C ON A.CLIENTE_ID = C.CLIENTE_ID
JOIN ENDERECO E ON C.ENDERECO_ID = E.ENDERECO_ID
JOIN CIDADE CI ON E.CIDADE_ID = CI.CIDADE_ID
JOIN PAIS P ON CI.PAIS_ID = P.PAIS_ID
WHERE P.PAIS = 'CHILE';
```

**47. Qual a quantidade de filmes alugados por funcionário?**
```sql
SELECT FUN.PRIMEIRO_NOME, COUNT(F.TITULO) AS QTD_FILMES
FROM FILME F
JOIN INVENTARIO I ON F.FILME_ID = I.FILME_ID
JOIN ALUGUEL A ON I.INVENTARIO_ID = A.INVENTARIO_ID
JOIN FUNCIONARIO FUN ON A.FUNCIONARIO_ID = FUN.FUNCIONARIO_ID
GROUP BY FUN.PRIMEIRO_NOME;
```

**48. Qual a quantidade de filmes alugados por funcionário para cada categoria?**
```sql
SELECT C.NOME, COUNT(F.TITULO) AS QTD_FILMES
FROM FILME F
JOIN INVENTARIO I ON F.FILME_ID = I.FILME_ID
JOIN ALUGUEL A ON I.INVENTARIO_ID = A.INVENTARIO_ID
JOIN FUNCIONARIO FUN ON A.FUNCIONARIO_ID = FUN.FUNCIONARIO_ID
JOIN FILME_CATEGORIA FC ON F.FILME_ID = FC.FILME_ID
JOIN CATEGORIA C ON FC.CATEGORIA_ID = C.CATEGORIA_ID
GROUP BY C.NOME;
```

**49. Quais filmes possuem preço da locação maior que a média de preço da locação?**
```sql
SELECT TITULO, PRECO_DA_LOCACAO
FROM FILME
WHERE PRECO_DA_LOCACAO > (SELECT AVG(PRECO_DA_LOCACAO) FROM FILME);
```

**50. Qual a soma da duração dos filmes por categoria?**
```sql
SELECT C.NOME, SUM(F.DURACAO_DO_FILME) AS SOMA_DURACAO
FROM FILME F
JOIN FILME_CATEGORIA FC ON F.FILME_ID = FC.FILME_ID
JOIN CATEGORIA C ON FC.CATEGORIA_ID = C.CATEGORIA_ID
GROUP BY C.NOME;
```

**51. Qual a quantidade de filmes alugados mês a mês por ano?**
```sql
SELECT MONTH(A.DATA_DE_ALUGUEL) AS MES, YEAR(A.DATA_DE_ALUGUEL) AS ANO, COUNT(F.TITULO) AS QTD_FILMES
FROM ALUGUEL A
JOIN INVENTARIO I ON A.INVENTARIO_ID = I.INVENTARIO_ID
JOIN FILME F ON I.FILME_ID = F.FILME_ID
GROUP BY YEAR(A.DATA_DE_ALUGUEL), MONTH(A.DATA_DE_ALUGUEL)
ORDER BY ANO, MES;
```

**52. Qual o total pago por classificação de filmes alugados no ano de 2006?**
```sql
SELECT F.CLASSIFICACAO, SUM(PAG.VALOR) AS TOTAL_PAGO
FROM FILME F
JOIN INVENTARIO I ON F.FILME_ID = I.FILME_ID
JOIN ALUGUEL A ON I.INVENTARIO_ID = A.INVENTARIO_ID
JOIN PAGAMENTO PAG ON A.ALUGUEL_ID = PAG.ALUGUEL_ID
WHERE YEAR(A.DATA_DE_ALUGUEL) = 2006
GROUP BY F.CLASSIFICACAO;
```
**53. Qual a média mensal do valor pago por filmes alugados no ano de 2005?**
```sql
SELECT MONTH(A.DATA_DE_ALUGUEL) AS MES, AVG(PAG.VALOR) AS MEDIA_PAGA
FROM ALUGUEL A
JOIN PAGAMENTO PAG ON A.ALUGUEL_ID = PAG.ALUGUEL_ID
WHERE YEAR(A.DATA_DE_ALUGUEL) = 2005
GROUP BY MES;
```

**54. Qual o total pago por filme alugado no mês de Junho de 2006 por cliente?**
```sql
SELECT C.PRIMEIRO_NOME, C.ULTIMO_NOME, SUM(PAG.VALOR) AS TOTAL_PAGO
FROM CLIENTE C
JOIN ALUGUEL A ON C.CLIENTE_ID = A.CLIENTE_ID
JOIN INVENTARIO I ON A.INVENTARIO_ID = I.INVENTARIO_ID
JOIN FILME F ON I.FILME_ID = F.FILME_ID
JOIN PAGAMENTO PAG ON A.ALUGUEL_ID = PAG.ALUGUEL_ID
WHERE YEAR(A.DATA_DE_ALUGUEL) = 2006
AND MONTH(A.DATA_DE_ALUGUEL) = 6
GROUP BY C.PRIMEIRO_NOME, C.ULTIMO_NOME;
```

---


