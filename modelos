clear
clear matrix
cd "V:\CPEA\CEP\Estudos\Artigos para submissão\vio"
import excel using "Base.xlsx" , sh("Base") first  clear
destring, replace dpcomma

rename Quantidadeproduzidatoneladas quant
rename reaHectares area
rename Produtividadetonhectare prod
sort cod_municipio
save prod, replace

***____***
clear
clear matrix
set more off
cd "V:\CPEA\CEP\Estudos\Artigos para submissão\vio"
use "V:\CPEA\CEP\Estudos\Artigos para submissão\vio\Dados_SP_Final_v2.dta"

merge 1:1 cod_municipio using prod
drop _merge

save "V:\CPEA\CEP\Estudos\Artigos para submissão\vio\Dados_SP_Final_v2.dta", replace

***começar aqui***
clear
clear matrix
set more off
cd "V:\CPEA\CEP\Estudos\Artigos para submissão\vio"
use "V:\CPEA\CEP\Estudos\Artigos para submissão\vio\Dados_SP_Final_v2.dta"

*** Aqui é gerado os homicdios e roubos agregados
gen X0001= X002+X004+X012
gen X0002= X016+X017+X018
***Aqui é gerado o log das taxas de criminalidade
gen l_tx_X0001=ln(((X0001+1)/(pop))*100000)
gen l_tx_X0002=ln(((X0002+1)/(pop))*100000)
gen l_tx_X0007=ln(((X007+1)/(pop))*100000)
gen l_tx_X0008=ln(((X008+1)/(pop))*100000)
gen l_tx_X0013=ln(((X013+1)/(pop))*100000)

***
label variable X0001 "HOMICÍDIO DOLOSO" 
label variable X0002 "Roubo" 

***Aqui é gerado a produtividade de café 

g Cafe002 = ln(prod+1)

**** Taxas
summarize l_tx_X0001 
summarize l_tx_X0002 
summarize l_tx_X0007 
summarize l_tx_X0008 
summarize l_tx_X0013 
summarize  Cafe002 
summarize  Cafe001 if [Cafe001 >0]

**** Nível
summarize X0001 
summarize X0002 
summarize X007 
summarize X008 
summarize X013 

***Aqui é gerado as carac. populacionais
summarize gini 
summarize l-renda_pc 
summarize porc_chefe_homens 
summarize prop_negros
summarize prop_15_24_anos
summarize prop_dom_ilum_pub
summarize prop_dom_pav 
summarize prop_dom_lixo_acum
summarize prop_dom_lig_rede_agua 
summarize prop_dom_esgoto 
summarize Cafe if [Cafe >0]
g cafe2= Cafe001 if [Cafe001 >0]
****Decis***
pctile prod_decil = Cafe2, nq(10) genp(dec)
xtile dcile = Cafe2, c(prod_decil)

tab prod_decil 

tab Cafe if [Cafe >0]
***Tabela decis

tab Cafe001 if [Cafe001 <=0.0006]
tab Cafe001 if [Cafe001 >0.0006 & Cafe001<=0.0015] 
tab Cafe001 if [Cafe001 >0.0015 & Cafe001<=0.0032] 
tab Cafe001 if [Cafe001 >0.0032 & Cafe001<=0.0058] 
tab Cafe001 if [Cafe001 >0.0058 & Cafe001<=0.0081] 
tab Cafe001 if [Cafe001 >0.0081 & Cafe001<=0.0173] 
tab Cafe001 if [Cafe001 >0.0173 & Cafe001<=0.0302] 
tab Cafe001 if [Cafe001 >0.0302 & Cafe001<=0.063] 
tab Cafe001 if [Cafe001 >0.063 & Cafe001<=0.24] 
tab Cafe001 if [Cafe001 >0.24] 


***Aqui é gerado as regressões com o Gini instrumentado
 
***Café***
ivreg gini Cafe002 
outreg2 using mono.xls ,  replace addstat(N, e(N)) dec(3)  excel ctitle(1)
ivreg gini Cafe002 l_renda_pc 
outreg2 using mono.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(2)
ivreg gini Cafe002  l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos 
outreg2 using mono.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(3)
ivreg gini Cafe002  l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos prop_dom_ilum_pub prop_dom_pav prop_dom_lixo_acum prop_dom_lig_rede_agua prop_dom_esgoto 
outreg2 using mono.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(4)


***Homicídios***
ivreg l_tx_X0001 (gini=Cafe002), robust first
outreg2 using mono1.xls ,  replace addstat(N, e(N)) dec(3)  excel ctitle(1SEM CONT)
ivreg l_tx_X0001 l_renda_pc (gini=Cafe002), robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(1CONT RENDA)
ivreg l_tx_X0001 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos (gini=Cafe002), robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(1CONT RENDA E POP)
ivreg l_tx_X0001 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos prop_dom_ilum_pub prop_dom_pav prop_dom_lixo_acum prop_dom_lig_rede_agua prop_dom_esgoto (gini=Cafe002), robust first
outreg2  using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(1CONT TODOS)

****Roubo***
ivreg l_tx_X0002 (gini=Cafe002), robust first
outreg2 using mono1.xls , addstat(N, e(N)) dec(3)  excel ctitle(2SEM CONT)
ivreg l_tx_X0002 l_renda_pc (gini=Cafe002), robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(2CONT RENDA)
ivreg l_tx_X0002 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos (gini=Cafe002), robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(2CONT RENDA E POP)
ivreg l_tx_X0002 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos prop_dom_ilum_pub prop_dom_pav prop_dom_lixo_acum prop_dom_lig_rede_agua prop_dom_esgoto (gini=Cafe002), robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(2CONT TODOS)

***Tentativa de Homicídio***
ivreg l_tx_X007 (gini=Cafe002), robust first
outreg2 using mono1.xls , addstat(N, e(N)) dec(3)  excel ctitle(7SEM CONT)
ivreg l_tx_X007 l_renda_pc (gini=Cafe002), robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(7CONT RENDA)
ivreg l_tx_X007 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos (gini=Cafe002), robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(7CONT RENDA E POP)
ivreg l_tx_X007 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos prop_dom_ilum_pub prop_dom_pav prop_dom_lixo_acum prop_dom_lig_rede_agua prop_dom_esgoto (gini=Cafe002), robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(7CONT TODOS)

***Lesão corporal***
ivreg l_tx_X0008 (gini=Cafe002), robust first
outreg2 using mono1.xls , addstat(N, e(N)) dec(3)  excel ctitle(8SEM CONT)
ivreg l_tx_X0008 l_renda_pc (gini=Cafe002), robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(8CONT RENDA)
ivreg l_tx_X0008 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos (gini=Cafe002), robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(8CONT RENDA E POP)
ivreg l_tx_X0008 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos prop_dom_ilum_pub prop_dom_pav prop_dom_lixo_acum prop_dom_lig_rede_agua prop_dom_esgoto (gini=Cafe002), robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(8CONT TODOS)

***Estupro***
ivreg l_tx_X0013 (gini=Cafe002), robust first
outreg2 using mono1.xls , addstat(N, e(N)) dec(3)  excel ctitle(13SEM CONT)
ivreg l_tx_X0013 l_renda_pc (gini=Cafe002), robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(13CONT RENDA)
ivreg l_tx_X0013 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos (gini=Cafe002), robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(13CONT RENDA E POP)
ivreg l_tx_X0013 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos prop_dom_ilum_pub prop_dom_pav prop_dom_lixo_acum prop_dom_lig_rede_agua prop_dom_esgoto (gini=Cafe002),robust first
outreg2 using mono1.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(13CONT TODOS)



***Regressões sem GINI instrumentado***

***Homicídios***
reg l_tx_X0001 gini, robust
outreg2 using mono2.xls ,  replace addstat(N, e(N)) dec(3)  excel ctitle(1SEM CONT)
reg l_tx_X0001 l_renda_pc gini, robust
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(1CONT RENDA)
reg l_tx_X0001 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos gini, robust
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(1CONT RENDA E POP)
reg l_tx_X0001 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos prop_dom_ilum_pub prop_dom_pav prop_dom_lixo_acum prop_dom_lig_rede_agua prop_dom_esgoto gini, robust
outreg2  using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(1CONT TODOS)

***Roubo***

reg l_tx_X0002 gini, robust
outreg2 using mono2.xls , addstat(N, e(N)) dec(3)  excel ctitle(2SEM CONT)
xi:reg l_tx_X0002 l_renda_pc gini, robust
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(2CONT RENDA)
reg l_tx_X0002 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos gini, robust
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(2CONT RENDA E POP)
reg l_tx_X0002 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos prop_dom_ilum_pub prop_dom_pav prop_dom_lixo_acum prop_dom_lig_rede_agua prop_dom_esgoto gini, robust
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(2CONT TODOS)

***Tentativa de Homicídio***
reg l_tx_X007 gini
outreg2 using mono2.xls , addstat(N, e(N)) dec(3)  excel ctitle(7SEM CONT)
reg l_tx_X007 l_renda_pc gini, robust
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(7CONT RENDA)
reg l_tx_X007 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos gini
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(7CONT RENDA E POP)
reg l_tx_X007 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos prop_dom_ilum_pub prop_dom_pav prop_dom_lixo_acum prop_dom_lig_rede_agua prop_dom_esgoto gini, robust
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(7CONT TODOS)

***Lesão corporal***
reg l_tx_X0008 gini, robust
outreg2 using mono2.xls , addstat(N, e(N)) dec(3)  excel ctitle(8SEM CONT)
reg l_tx_X0008 l_renda_pc gini, robust
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(8CONT RENDA)
reg l_tx_X0008 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos gini, robust
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(8CONT RENDA E POP)
reg l_tx_X0008 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos prop_dom_ilum_pub prop_dom_pav prop_dom_lixo_acum prop_dom_lig_rede_agua prop_dom_esgoto gini, robust
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(8CONT TODOS)

***Estupro***
reg l_tx_X0013 gini, robust
outreg2 using mono2.xls , addstat(N, e(N)) dec(3)  excel ctitle(13SEM CONT)
reg l_tx_X0013 l_renda_pc gini, robust
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(13CONT RENDA)
reg l_tx_X0013 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos gini, robust
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(13CONT RENDA E POP)
reg l_tx_X0013 l_renda_pc porc_chefe_homens prop_negros prop_15_24_anos prop_dom_ilum_pub prop_dom_pav prop_dom_lixo_acum prop_dom_lig_rede_agua prop_dom_esgoto gini, robust
outreg2 using mono2.xls ,  addstat(N, e(N)) dec(3)  excel ctitle(13CONT TODOS)


***Cortes do mapa***
*Criando os cortes do mapa* *_*
g cafe3= prod if [prod >0]
pctile prod_qcil = cafe3, nq(5) genp(dec)
xtile dcile = cafe3, c(prod_qcil)
tab prod_qcil
*****______________*****

spmap prod using spmuni.dta, id(id) clmethod(custom)fcolor(Blues) clbreak(0 0.18 0.86 1 1.2 1.5 3) ndfcolor(black) legend(label(2 "Municípios que não produzem Café") label(3 "De 0,18 até 0,86") label(4 "De 0,087 até 1")label(5 "De 1,01 até 1,2") label(6 "De 1,21 até 1,5")label(7 "De 1,51 até 3") size(*1.5)) note("Fonte: Pesquisa Agrícola Municipal, 2012 - IBGE", size(*1))

**spmap Cafe using spmuni.dta, id(id) clmethod(custom)fcolor(Blues) clbreak(0 1 15 58 173 630 16330) legtitle("Produtividade do Café, em  toneladas  por  hectare") ndfcolor(black) legend(label(2 "Municípios que não produzem Café") label(3 "De 1 até 15") label(4 "De 15,1 até 58")label(5 "De 58,1 até 173") label(6 "De 173,1 até 630")label(7 "De 630,1 até 16.330") size(*1.25)) note("Fonte: Pesquisa Produção Agrícola, 2012 - IBGE", size(*0.75))
