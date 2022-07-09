# Desenvolvimento geral de pacotes *Bioconductor*

## Versão do *Bioconductor* e <i class="fab fa-r-project"></i>

Os criadores dos pacotes devem usar sempre a \[devel version of *Bioconductor*\]\[‘devel’ version\] e \[*Bioconductor* packages\]\[biocViews\] quando desenvolvem e testam pacotes a serem contribuídos.

Dependendo do ciclo de lançamento <i class="fab fa-r-project"></i>, usar \[*Bioconductor*\]\[\] devel pode ou não envolver também o uso da versão do devel <i class="fab fa-r-project"></i>. Veja o guia prático em \[using devel version of Bioconductor\]\[‘devel’ version\] para informações atualizadas.

## Exatidão, Espaço e Hora

### Construção de CMD R

Os pacotes \[*Bioconductor*\]\[\] devem passar minimamente a compilação `R CMD build` (ou `R CMD INSTALL --build`) e passar `R CMD check` sem erros e sem avisos usando um R-devel recente. Os autores também devem tentar resolver todos os erros, avisos e notas que surgiram durante a compilação ou verificação.

### BiocCheck

Os pacotes também devem passar `BiocCheck::BiocCheckGitClone()` e `BiocCheck::BiocCheck('new-package'=TRUE)` sem erros e sem avisos. O pacote *[BiocCheck](https://bioconductor.org/packages/3.15/BiocCheck)* é um conjunto de testes que envolvem \[*Bioconductor*\]\[\] Melhores Práticas. Todos os esforços devem ser feitos para corrigir quaisquer erros, avisos e notas que surjam durante esta compilação ou verificação.

### ERRO, AVISOS e NOTAS

O membro da equipa \[*Bioconductor*\]\[\] atribuído para rever o pacote durante o processo de envio esperará que todos os ERROS, AVISOS e NOTAS sejam tratados tanto na compilação R CMD (R CMD build), verificação R de CMD (R CMD check) e BiocCheck. Se há qualquer remanescente, uma justificação do porquê de não serem corrigidos será de esperar.

### Nomes de ficheiro

Não use nomes de arquivos que diferem apenas na capitalização, pois nem todos os sistemas de arquivos são sensíveis a maiúsculas e minúsculas.

### Tamanho de pacote

O pacote fonte resultante da execução `R CMD build` deve ocupar menos de 5 MB em disco.

### Duração da verificação

O pacote deve demorar menos de 10 minutos para executar `R CMD check --no-build-vignettes`. Usando a opção `--no-build-vignettes` garante que a vinheta seja construída apenas uma vez. [1]

### Memória

Os exemplos de páginas de vignette e man não devem usar mais do que 3 GB de memória pois <i class="fab fa-r-project"></i> não pode alocar mais do que isso no Windows 32-bit.

### Tamanho de ficheiro individual

Para pacotes de software, os ficheiros individuais têm de ser &lt;= 5MB. Esta restrição existe mesmo depois de o pacote ser aceite e adicionado ao repositório \[*Bioconductor*\]\[\]. Ver [seção de dados](#data) para conselhos sobre pacotes que usam grandes arquivos de dados.

### Ficheiros indesejados

O directório de pacotes em bruto (raw) não deve conter ficheiros desnecessários, ficheiros de sistema, ou ficheiros ocultos como `.DS_Store`, `.project`, `.git`, ficheiros cache, ficheiros de registo (log), `*.Rproj`, `*.so`, etc. Estes ficheiros podem estar presentes no diretório local, mas não devem ser enviados (commited) para o git (veja <span
id="gitignore">`.gitignore`</span>). Todos os arquivos ou diretórios para outras aplicaticações (Github Actions, devtool, etc.) idealmente devem estar em um branch diferente e não devem ser enviados para a versão do *Bioconductor* do pacote.

## Ambiente de Verificação R CMD

É possível ativar ou desativar várias opções em `R CMD build`  e `R CMD check`. As opções podem ser definidas como variáveis de ambiente individuais ou podem ser [listadas em um ficheiro ](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Checking-and-building-packages). As descrições de todas as diferentes opções disponíveis podem ser encontradas [aqui](https://cran.r-project.org/doc/manuals/r-devel/R-ints.html#Tools). \[*Bioconductor*\]\[\] optou por personalizar algumas dessas opções para submissões recebidas durante a `R CMD check`. O ficheiro de flags utilizadas pode ser descarregado do [<i class="fab fa-github"></i>
GitHub](https://github.com/Bioconductor/packagebuilder/blob/master/check.Renviron). O ficheiro pode ser colocado num directório por defeito (default directory), conforme as instruções [aqui](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Checking-and-building-packages) ou pode ser definida através da variável de ambiente `R_CHECK_ENVIRON` com um comando semelhante a:

    export R_CHECK_ENVIRON = <path to downloaded file>

[1] Isto é verdadeiro apenas para pacotes de software. Para Dados da Experiência (Experiment Data), Anotação, e pacotes de Workflow é permitido espaço adicional e verificação de tempo.
