---
title: Test av tjeneste
description: Teste autorisasjon av datakonsument og samtykketjeneste
weight: 340
---

## Test av tjeneste i Altinn sitt testmiljø
Tjenesten må testes ut i Altinn sitt testmiljø TT02: https://tt02.altinn.no

Forutsetninger for å teste:

1.  Må være etablert en samtykketjeneste (lenketjeneste) i TUL. Tjenesten må være migrert til TT02.
2.  For å teste henting av token via REST-tjeneste trenger man ApiKey knyttet til organisasjonsnummeret man skal teste med. Kan bestilles hos Altinn via selvbetjeningsportalen eller via [*tjenesteeier@altinn.no*](mailto:tjenesteeier@altinn.no)
3.  Man må ha fiktive testpersoner som kan benyttes i testen. Dette har
    i de fleste tilfeller tjenesteeier allerede tilgang til men dersom man ikke har
    dette må man sende en henvendelse til Altinn.    
4.  For å verifisere det signerte tokenet må datakilden benytte Altinn
    sitt offentlige sertifikat. Dette får man ved å henvende seg til
    Altinn


## Registrere en datakonsument i tjenesteeierstyrt rettighetsregister 
For å få testet samtykketjenesten må man først
registrere en test-datakonsument i tjenesteeierstyrt rettighetsregister
(SRR). Dette gjøres ved å benytte webtjenesten
"RegisterSRRAgencyExternal":

https://tt02.altinn.no/RegisterExternal/RegisterSRRAgencyExternalBasic.svc?wsdl

Denne har operasjonene `AddRights`, `DeleteRights` og `GetRights`.

Eksempel på en request for å legge til rettigheter (her testet ved bruk av SoapUI):  

{{<figure src="add-rights.png" title="Legge til rettighet i tjenesteeierstyrt rettighetsregister" >}}

**ServiceCode** er tjenestekoden og **ServiceEditionCode** er tjenesteutgavekoden for lenketjenesten. Disse hentes fra TUL.  

**Reportee** angir hvilken organisasjon (eller person) som skal få lov å hente ut data gjennom tjenesten. I Lånesøknadscasen må organisasjonsnummeret til banken som skal få lov til å hente data fra Skatteetaten legges inn. I test legger man inn organisasjonsnummeret til en fiktiv organisasjon man kan teste med.  

**AllowedRedirectDomain** angir hvilket domene sluttbruker kan sendes til etter at sluttbruker har gitt/ikke gitt samtykke. Dette er en sikkerhetsmekanisme som sørger for at Altinn ikke kan utnyttes til redirects vilkårlig. Angi kun domene/host (ikke path) og bruk wildcard (*) for å støtte flere sub-domener. Wildcard skal kun brukes på subdomene eller lavere nivå (for mer informasjon se [her](../../datakonsument/komme-i-gang/#før-man-kan-ta-i-bruk-tjenesten-må-følgende-være-på-plass).  

Det er mulig å legge inn flere domener per org.nr. ved å skille de med semikolon. 

Eksempel på å fjerne en gitt rettighet:

{{<figure src="delete-rights.png" title="Fjerne rettighet fra tjenesteeierstyrt rettighetsregister" >}}

Eksempel på uthenting av gitte rettigheter:

{{<figure src="get-rights.png" title="Uthenting av gitte rettigheter" >}}


Det kan hentes pr. tjeneste eller pr. organisasjonsnummer.


## Teste samtykketjenesten 
Etter å ha registrert en test-datakonsument (fiktivt
organisasjonsnummer) i tjenesteeierstyrt rettighetsregister kan man
teste ut samtykketjenesten. Dette krever at tjenesten er migrert til
TT02 i TUL. En beskrivelse av hvordan man kan opptre som datakonsument
for å få testet tjenesten og innveksling av autorisasjonskode i token
finnes [her](../../datakonsument/test-tjeneste/#test-av-samtykketjeneste-i-altinn-sitt-testmiljø).


Se [her](../bruk-av-token/#bruk-av-self-contained-oauth-token) for informasjon om oppbygging og verifikasjon av token.
