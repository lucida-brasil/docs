Últimos ajustes para conversão de GaAPI_2.4 para GaAPI_3.0

# 1. A Necessidade

Apesar de todas as configurações “under the hood” para solicitar informações via API já estarem configuradas para a versão 3.0 identificamos que algumas nomenclaturas não estão no padrão novo.

# 2. Dimensões

## 2.1. Casos Simples

As seguintes dimensões não estão dentro do padrão e estão sendo substituídas por uma nova nomenclatura – somente é necessário substituir os termos:

Nome Antigo | Substituído Diretamente por:
--- | ---
ga:visitorType | ga:userType
ga:visitCount | ga:sessionCount
ga:daysSinceLastVisit | ga:daysSinceLastSession
ga:visitLength | ga:sessionDurationBucket
ga:visitsToTransaction | ga:sessionsToTransaction
ga:visitorAgeBracket | ga:userAgeBracket
ga:visitorGender | ga:userGender
 
## 2.2. Casos Especiais

Nome Antigo | Substituído Diretamente por:
ga:isMobile | ga:deviceCategory
ga:isTablet | 
 
Onde o elemento ga:deviceCategory pode possuir 3 valores:
*  desktop – para computadores e notebooks
*  mobile – para aparelhos celular e smartphones
*  tablet – para tablets
 
# 3. Métricas

## 3.1. Casos Simples

As seguintes métricas não estão dentro do padrão e estão sendo substituídas por uma nova nomenclatura – somente é necessário substituir os termos:

Nome Antigo | Substituido Diretamente por:
--- | ---
ga:visitors | ga:users
ga:newVisits | ga:newUsers
ga:percentNewVisits | ga:percentNewSessions
ga:visits | ga:sessions
ga:entranceBounceRate | ga:bounceRate
ga:visitBounceRate | ga:bounceRate
ga:timeOnSite | ga:sessionDuration
ga:avgTimeOnSite | ga:avgSessionDuration
ga:goalValuePerVisit | ga:goalValuePerSession
ga:pageviewsPerVisit | ga:pageviewsPerSession
ga:searchVisits | ga:searchSessions
ga:percentVisitsWithSearch | ga:percentSessionsWithSearch
ga:appviews | ga:screenviews
ga:uniqueAppviews | ga:uniqueScreenviews
ga:appviewsPerVisit | ga:screenviewsPerSession
ga:visitsWithEvent | ga:sessionsWithEvent
ga:eventsPerVisitWithEvent | ga:eventsPerSessionWithEvent
ga:transactionsPerVisit | ga:transactionsPerSession
ga:transactionRevenuePerVisit | ga:transactionRevenuePerSession
ga:socialInteractionsPerVisit | ga:socialInteractionsPerSession
 
## 3.2. Casos Especiais

As seguintes métricas não possuem mais substituição e serão descontinuadas

Nome Antigo | Substituido Indiretamente Por
--- | ---
ga:ROI | ga:ROAS
ga:margin | 
 
A Métrica ga:ROAS que deverá ser utilizada e é definida através do cálculo
Inline image 1
Lembrando que uma visita pode realizar mais do que uma transação mas no máximo uma conversão por meta e que os dados de Mídia precisam ser inseridos através do processo descrito [neste link] (https://developers.google.com/analytics/devguides/platform/cost-data-import).

# 4. Segmentações Avançadas

## 4.1. Caso Geral

No caso geral, todas as segmentações avançadas que realizamos através do elemento dynamic:: precisam ser alteradas para sessions::condition::.

## 4.2. Novas Possibilidades de Segmentações

### 4.2.1. User vs Session

O 1º elemento da nova taxonomia é relacionado ao tipo de segmentação realizado, se a segmentação é baseada em visitas (sessions::) ou em usuários (users::).
As segmentações baseadas em usuários em sites com login e Universal Analytics permitem o tracking dos users que realizaram login em mais de um browser/device e acabam por permitir, entre outras coisas, um cálculo de user value ou de retorno para conversão mais eficientes.
Porém as segmentações baseadas em usuários não podem ser utilizadas para intervalos superiores a 90 dias.
Para utilizar segmentações que necessitam tanto segmentos baseados em usuários e em visitas é necessário adicionar as segmentações por usuário antes das segmentações por visitas E utilizar o operador AND (;).

### 4.2.2. Condition vs Sequence

O segundo elemento da nova taxonomia é relacionado a possibilidade de segmentação por uma sequência de atividades (sequence::) ou por uma condição específica (condition::).
As segmentações baseadas em sequências são definidas através dos elementos que obrigam a ordenação de precedência (;->>) ou ordenação de imediata precedência (;->). Além disso é possível utilizar o operador de negativa (!) em qualquer elemento da sequência.
As segmentações baseadas em sequência permitem máximo de 10 elementos.
É possível utilizar dentro do primeiro elemento de uma sequência o operador (^) para indicar que este elemento tem que ser necessariamente o 1º hit realizado pelo usuário dentro daquele período.

### 4.2.3. Non-reported issues

Encontramos casos em que a segmentação de usuários associadas com segmentação de visitas que realizaram determinado evento retornou valor “0”, nos casos de resposta “0” recomendamos realizar cross-check com resultado no próprio GA ou realizar segmentação baseada em visitas apenas para verificar inconsistências, uma vez que essencialmente os dois cenários deveriam trazer resultados similares.

# 5. Referência

https://developers.google.com/analytics/devguides/reporting/core/v3/
https://www.googleapis.com/analytics/v3/metadata/ga/columns?pp=1

# 6. Macro de Vba - Substitui Parâmetros antigos

Essa macro pode ser utilizada para substituir todos os casos padrões - garantam que os casos especiais estarão cobertos também.

``` VB
Sub ReplacedBy()
'
'   ReplacedBy Macro
'   Coloque essa Macro em qualquer Modulo
'   Somente na aba Queries Salvas
'   Clique em qualquer célula da aba Queries Salvas para fazer a mudança
'  
	Application.ScreenUpdating = False
    	Application.Calculation = xlManual
   
Cells.Select

        Selection.Replace What:="dynamic::", Replacement:="sessions::condition::", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
        


        Selection.Replace What:="ga:visitorType", Replacement:="ga:userType", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:visitCount", Replacement:="ga:sessionCount", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:daysSinceLastVisit", Replacement:="ga:daysSinceLastSession", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:visitors", Replacement:="ga:users", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:newVisits", Replacement:="ga:newUsers", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:percentNewVisits", Replacement:="ga:percentNewSessions", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:visitLength", Replacement:="ga:sessionDurationBucket", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:visits", Replacement:="ga:sessions", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:entranceBounceRate", Replacement:="ga:bounceRate", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:visitBounceRate", Replacement:="ga:bounceRate", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:timeOnSite", Replacement:="ga:sessionDuration", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:avgTimeOnSite", Replacement:="ga:avgSessionDuration", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:goalValuePerVisit", Replacement:="ga:goalValuePerSession", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:pageviewsPerVisit", Replacement:="ga:pageviewsPerSession", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:searchVisits", Replacement:="ga:searchSessions", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:percentVisitsWithSearch", Replacement:="ga:percentSessionsWithSearch", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:appviews", Replacement:="ga:screenviews", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:uniqueAppviews", Replacement:="ga:uniqueScreenviews", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:appviewsPerVisit", Replacement:="ga:screenviewsPerSession", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:visitsWithEvent", Replacement:="ga:sessionsWithEvent", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:eventsPerVisitWithEvent", Replacement:="ga:eventsPerSessionWithEvent", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:visitsToTransaction", Replacement:="ga:sessionsToTransaction", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:transactionsPerVisit", Replacement:="ga:transactionsPerSession", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:transactionRevenuePerVisit", Replacement:="ga:transactionRevenuePerSession", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:socialInteractionsPerVisit", Replacement:="ga:socialInteractionsPerSession", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:visitorAgeBracket", Replacement:="ga:userAgeBracket", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
 
        Selection.Replace What:="ga:visitorGender", Replacement:="ga:userGender", _
        LookAt:=xlPart, SearchOrder:=xlByRows, MatchCase:=False, SearchFormat:= _
        False, ReplaceFormat:=False
        
    Application.ScreenUpdating = True
    Application.Calculation = xlAutomatic

End Sub
```
