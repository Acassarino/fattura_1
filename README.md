# FatturaElettronicaPA.WebServices
Il namespace `FatturaElettronicaPA.WebServices` raccoglie i client che
consentono di consultare i [Web Service per la Fatturazione Elettronica][1]
messi a disposizione dalla Pubblica Amministrazione. 

Sono disegnati in maniera da esporre tutti la stessa interfaccia ed essere al
tempo stesso semplici e leggeri. Al momento lavorano in modalità sincrona ma
l'obiettivo è di renderli tutti asincroni.

## Authorization ID
Per l'utilizzo dei Web Services è necessario richiedere una specifica
autorizzazione. L'Authorization Id è gratuito ed il rilascio è immediato, ma
bisogna compilare un [apposito questionario][2]. 

## Codice Fiscale
Questo servizio web consente di estrarre dall’iPA informazioni su tutti gli 
uffici destinatari di Fatturazione Elettronica associati al Codice Fiscale fornito.

```cs

	var cfWebService = new CodiceFiscaleWebService {
		AuthId = "<auth Id>",
		CodiceFiscale = "00354730392",
	};

	cfWebService.PerformRequest ();
	if (cfWebService.Uffici == null)
		return;

	// "Comune di Ravenna"
	Console.WriteLine (cfWebService.Uffici [0].Denominazione);
```

## Codice Univoco di Fatturazione
Questo client interroga il corrispondente Web Service PA per verificare la
validità di un Codice Univoco di "Ufficio destinatario di Fatturazione
Elettronica e dati SFE". Se il codice è valido vengono anche restitute tutte le
informazioni diponibili sull'ufficio.

```cs
    var codiceUnivocoWebService = new CodiceUnivocoFatturazioneWebService()

    // Authorization Id ricevuto dall'ente.
    codiceWebService.AuthId = "<auth Id>";
    // Codice univoco dell'ufficio che ci interessa
    codiceWebService.CodiceUfficio = "KN3VNW";

    codiceWebService.PerformRequest();
    if (codiceWebService.Ufficio == null) return;

    // "Ravenna"
    Console.WriteLine(codiceWebService.Ufficio.Comune);
```

## Dati di un Ente
Questo client interroga il corrispondente Web Service PA per verificare la
validità di un Codice Ente. Se il codice è valido vengono anche restitute tutte
le informazioni diponibili sull'ente.

```cs
    var enteWebService = new DatiEnteWebService {
    	AuthId = "<auth Id>", 
    	CodiceEnte = "c_h199"
    };

    enteWebService.PerformRequest();
    if (enteWebService.Ente == null) return;

    // "Ravenna"
    Console.WriteLine(enteWebService.Ente.Comune);
```

## Lista informazioni associate ad indirizzo Email
Questo servizio web consente di estrarre dall’iPA informazioni relative ad una entità, sia essa 
un Ente, un’Unità Organizzativa, un’Area Organizzativa Omogenea, un servizio generico, un Servizio 
di Fatturazione Elettronica, un Responsabile o un Referente, associata ad uno specifico indirizzo 
email. Se un indirizzo mail è associato a più entità all’interno di iPA, verranno visualizzate le 
informazioni di dettaglio relative a ciascuna delle entità individuate.

```cs
    var emailWebService = new EmailWebService {
    	AuthId = "<auth Id>", 
    	Email = "comune.ravenna@legalmail.it"
    };

    emailWebService.PerformRequest();
    if (emailWebService.Emails == null) return;

    // "Comune di Ravenna"
    Console.WriteLine(emailWebService.Emails[0].Denominazione);
```

## Lista Servizi di Fatturazione
Questo servizio web consente di estrarre dall’iPA informazioni relativa ad una entità, sia
essa un Ente, un’Unità Organizzativa, un’Area Organizzativa Omogenea, un servizio
generico, un Servizio di Fatturazione Elettronica, un Responsabile o un Referente, associata
ad uno specifico indirizzo email. Se un indirizzo mail è associato a più entità all’interno di
iPA, verranno visualizzate le informazioni di dettaglio relative a ciascuna delle entità
individuate.

```cs
    var serviziWebService = new ServiziFatturazioneWebService {
    	AuthId = "<auth Id>", 
    	CodiceEnte = "c_h199"
    };

    serviziWebService.PerformRequest();
    if (serviziWebService.ServiziFatturazione == null) return;

    // "U.O. LEGALE E CONTENZIOSO"
    Console.WriteLine(
    	serviziWebService.ServiziFatturazione[0].DenominazioneUnitàOrganizzativa);
```

## Lista Unità Organizzative
Questo servizio web consente di estrarre dall’ iPA la lista delle Unità Organizzative 
facenti capo ad uno specifico codice ente iPA.

```cs
    var uoWebService = new UnitaOrganizzativaWebService {
    	AuthId = "<auth Id>", 
    	CodiceEnte = "c_h199"
    };

    uoWebService.PerformRequest();
    if (uoWebService.UnitaOrganizzative == null) return;

    // "Ravenna"
    Console.WriteLine(uoWebService.UnitaOrganizzative[0].Comune);
```
## Lista Aree Organizzative Omogenee
Questo servizio web consente di estrarre dall’ iPA informazioni su tutte le aree organizzative
omogenee associate al codice iPA fornito. Il servizio consente, inoltre, di impostare come
parametro di ricerca anche il codice AOO, oltre il codice iPA. Si tenga presente che nel caso
in cui l’utente fornisca congiuntamente i due codici (codice iPA e codice AOO) dalla ricerca
potrà essere estratta solo ed esclusivamente una Area Organizzativa Omogenea.

```cs
	var aoWebService = new AreeOmogeneeWebService {
		AuthId = "<auth Id>",
		CodiceEnte = "c_h199",
		// opzionale:
		CodiceAreaOmogenea = "AMC-RA"
	};

	aoWebService.PerformRequest ();
	if (aoWebService.AreeOmogenee == null)
		return;
	
	// "Ravenna"
	Console.WriteLine (aoWebService.AreeOmogenee [0].Comune);
```

## Portable Class Library
La libreria gira senza modifiche sui seguenti ambienti:

- .NET Framework 4.0 e superiori,
- Xamarin.iOS
- Xamarin.Android
- Windows Phone 8
- Windows Store apps (Windows 8)
- Silverlight 5.0

## Installazione
FatturaElettronicaPA.WebServices è su [NuGet][nuget] quindi tutto quel che serve è eseguire:

```
	PM> Install-Package FatturaElettronicaPA.WebServices
```
dalla Package Console, oppure usare il comando equivalente in Visual Studio.

## Licenza
FatturaElettronicaPA.WebServices è un progetto open source [Gestionale Amica][ga] rilasciato sotto licenza [BSD][bsd].

			
[1]: http://www.indicepa.gov.it/documentale/webservices.php
[2]: http://www.indicepa.gov.it/registr-user-ws/ws-registrazione-start.php
[ga]: http://gestionaleamica.com
[nuget]: https://www.nuget.org/packages/FatturaElettronicaPA.WebServices/
