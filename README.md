# Logs Project
Neste projeto o objetivo foi analisar uma base de dados construindo uma ferramenta interna de relatórios

# Requisitos
<a href=https://www.vagrantup.com/downloads.html>Vagrant</a>, <a href=https://www.virtualbox.org/wiki/Downloads>Virtual Box</a> e Python

# Configuração da VM
<a href=https://d17h27t6h515a5.cloudfront.net/topher/2017/June/5948287e_fsnd-virtual-machine/fsnd-virtual-machine.zip>FSND Virtual Machine</a>

# Criando views
Artigos mais populares
~~~
create view artigos_populares as
select title, count(title) as views from log, articles
where log.path like concat ('%', articles.slug)
group by title order by views desc;
~~~
Autores mais populares
~~~
create view autores_populares as
select authors.name, count(articles.author) as views from articles, log, authors
where log.path = concat('/article/',articles.slug) and articles.author = authors.id
group by authors.name order by views desc;
~~~
Dias com mais de 1% de erros
~~~
create view dias_erros as
select date(time),round(100.0*sum(case log.status when '200 OK' 
  then 0 else 1 end)/count(log.status),2) as "Percent Error" from log group by date(time) 
 order by "Percent Error" desc;
~~~

# Execução
Download da base de dados <a href=https://d17h27t6h515a5.cloudfront.net/topher/2016/August/57b5f748_newsdata/newsdata.zip> newsdata.sql</a>
<p>Colocar a base de dados descompactada no diretório vagrant, compartilhado com a máquina virtual
<p>Rodar a máquina virtual previamente instalada com <code>vagrant up</code>
<p>Fazer login <code>vagrant ssh </code>
<p> Carregar os dados com o comando <code>psql -d news -f newsdata.sql</code>
<p> Executar o programa no terminal <code> python log.py </code>  
