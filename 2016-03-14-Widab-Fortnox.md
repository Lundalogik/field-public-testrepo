---
title: Widab Fortnox
layout: post
author: erik.vincelette
permalink: /widab-fortnox/
source-id: 1UpglIfv7tSBoi9A-1-mvbXac9S1FkMfsi4ThS5ircY0
published: true
---
# ConnectToFortnox F3

## Sammanfattning

Kundanpassad mappning för ConnectToFortNoxF3. Mappar extrafält på ärende till fakturarader i FortNox. Använder [RemoteX FortNoxF3Library](https://github.com/remotex/Integration/tree/master/Libraries/FortNoxF3Library).

## System

Integration mellan Fortnox API F3 och RemoteX

## Kunder

Widab

## Kontakter

* Fortnox (Leverantör)

    * [Dokumentation] (http://developer.fortnox.se/documentation)

    * [Forum] (http://developer.fortnox.se/forum/)

    * Support - integration@fortnox.se

## Drift

Schemaläggs

## Logg

* 2016-02-24 - Jonas - Skapat readme för integration.

## Integration

### Beståndsdelar

* ConnectToFortnox - ConsoleApplication som kör med parametrar:

    * import - Importerar kunder och artiklar

    * export - Exporterar fakturaunderlag, fakturakunder och artiklar

### Terminologi

* Contract heter i Fortnox Customer

* Product heter i Fortnox Article

* Case blir Invoice

* UsageQuantity blir InvoiceRow

### Inställningar

* AccessToken - Token för att koppla till installationen, erhålls från kund enligt [detta](http://developer.fortnox.se/documentation/basics/authentication/).

* ClientSecret - Nyckel för att bekräfta att det är den här integrationen.

* OurReferenceUnitTag - Tag key om ett extrafält på ärendets objekt skall hamna i OurReference.

* UqDescriptionToFortnoxDescriptionWithEllipsisFallbackToTitle - True/False Anger om uq.Description skall skickas i ir.Description, klipper isf vid 50tkn. Är uq.Description tomt tas istället uq.Title.

* UqFieldExportedToArticleRowDescription - Title/Description. Ange vilket fält som skall sättas på ir.Description (Används ej om UqDescriptionToFortnoxDescriptionWithEllipsisFallbackToTitle är satt till True).

* DateAsUqDescriptionForMetric - Ange metric som skall vara på artikeln för att Description skall ersättas av RegistrationDate

* NoPriorityAsOurReference - True/False. Om denna är True sätts inte Priority i ir.OurDescription.

* AddDateToDescription - True/False. Om denna är True grupperas fakturaraderna per RegistrationDate med en InvoiceRow med bara RegistrationDate före och en tom InvoiceRow efter varje grupp.

* GroupSimilarTimeProducts - True/False. Om denna är True summeras fakturarader som har samma RegistrationDate och Product (ingen product summeras inte).

* CaseFieldAsRemark - Title/Description/*. Om denna är Title eller Description sätts i.Remarks till det fältet från Case.

* SendAccountFromManufacturerCode - True/False. Om denna är True sätts ir.AccountNumber till uq.ManufacturerCode.

* AppendUnitTitleToRemark - Om denna är satt till True läggs objektets titel på i slutet av Remarks

* CaseTypeToProjectMapper - Mappas genom att fylla i enligt mall: "System.Picklist.Case.Type.Garanti=3000,System.Picklist.Case.Type.Renovering=4000".

* CostCenterTagName - Namn på extrafält på objekt som innehåller vilket kostnadsställe som ska anges på fakturan.

* MailTo - Epostadress hos kund som ska mailas om fel uppstår i integrationen.

* MailSubject - Ämne i mail som skickas till kund om fel uppstår i integrationen.

* CustomersModifiedSince - Senaste lyckade importen av kunder -20 min. I formatet YYYY-MM-DD

* ArticlesModifiedSince - Senaste lyckade importen av artiklar -20 min. I formatet YYYY-MM-DD

* PricesModifiedSince - Senaste lyckade importen av priser -20 min. I formatet YYYY-MM-DD

* UqDescriptionOnSeparateInvoiceRows - Om denna är satt till True kommer artikelradensbeskrivning delas upp i max antal tecken per rad och läggas på separata rader under varje fakturarad.

### Villkor

#### För import

* Artiklar måste vara uppdaterade efter datumet angivet i ArticlesModifiedSince

* Priser måste vara uppdaterade efter datumet angivet i CustomersModifiedSince

* Kunder måste vara uppdaterade efter datumet angivet i PricesModifiedSince

#### För export

* Attesterade artikelrader

* Artikelrader måste ha Code

* Artikelrader får inte ha Activity "System.Picklist.UsageQuantity.Activity.NotIntegrated"

* Artikelrad måste ha en artikel som är importerad från Fortnox

* Ärende får inte ha "Error" i extrafältet "FortnoxError"

* Ärende får inte vara av typ "System.Picklist.Case.Type.NotIntegrated"

* Ärende måste ha en kund som är importerad från Fortnox

### Gotchas

#### Vid import

* Om det inte finns något pris för en artikel som kommer från Fortnox, skickas inte PricePerPiece-parametern på commandet (dvs, priset uppdateras inte/skapas som 0).

* Det kommer inte längre med någon active-flagga på customer från Fortnox, så man måste inaktivera kunder manuellt i RMX.

### Mappningar (Med standardinställningar)

#### Customer (Fortnox => RemoteX)

<table>
  <tr>
    <td>Customer</td>
    <td>Contract</td>
    <td>Kommentar</td>
  </tr>
  <tr>
    <td>CustomerNumber</td>
    <td>CRMSystemId</td>
    <td></td>
  </tr>
  <tr>
    <td>Name</td>
    <td>Title</td>
    <td></td>
  </tr>
  <tr>
    <td>"Active"</td>
    <td>Status</td>
    <td>Hårdkodad</td>
  </tr>
  <tr>
    <td>DeliveryName</td>
    <td>Attention</td>
    <td></td>
  </tr>
  <tr>
    <td>DeliveryAddress1</td>
    <td>Street</td>
    <td></td>
  </tr>
  <tr>
    <td>DeliveryAddress2</td>
    <td>Street2</td>
    <td></td>
  </tr>
  <tr>
    <td>DeliveryCity</td>
    <td>City</td>
    <td></td>
  </tr>
  <tr>
    <td>DeliveryZipCode</td>
    <td>Zip</td>
    <td></td>
  </tr>
  <tr>
    <td>DeliveryCountry</td>
    <td>Country</td>
    <td></td>
  </tr>
  <tr>
    <td>Address1</td>
    <td>BillingStreet</td>
    <td></td>
  </tr>
  <tr>
    <td>Address2</td>
    <td>BillingStreet2</td>
    <td></td>
  </tr>
  <tr>
    <td>City</td>
    <td>BillingCity</td>
    <td></td>
  </tr>
  <tr>
    <td>ZipCode</td>
    <td>BillingZip</td>
    <td></td>
  </tr>
  <tr>
    <td>Country</td>
    <td>BillingCountry</td>
    <td></td>
  </tr>
  <tr>
    <td>Email</td>
    <td>Tag.ContractTag1</td>
    <td></td>
  </tr>
  <tr>
    <td></td>
    <td>LogMessage</td>
    <td>"Uppdaterades från Fortnox" läggs på loggen</td>
  </tr>
</table>


## Customer (RemoteX => Fortnox)

<table>
  <tr>
    <td>Title</td>
    <td>Name</td>
    <td></td>
  </tr>
  <tr>
    <td>CrmSystemId</td>
    <td>CustomerNumber</td>
    <td>CrmSystemId eller RemoteX Id ifall CRMSystemId inte är satt</td>
  </tr>
  <tr>
    <td>Attention</td>
    <td>DeliveryName</td>
    <td></td>
  </tr>
  <tr>
    <td>Street</td>
    <td>DeliveryAddress1</td>
    <td></td>
  </tr>
  <tr>
    <td>Street2</td>
    <td>DeliveryAddress2</td>
    <td></td>
  </tr>
  <tr>
    <td>City</td>
    <td>DeliveryCity</td>
    <td></td>
  </tr>
  <tr>
    <td>Zip</td>
    <td>DeliveryZipCode</td>
    <td></td>
  </tr>
  <tr>
    <td>Country</td>
    <td>DeliveryCountry</td>
    <td></td>
  </tr>
  <tr>
    <td>BillingStreet</td>
    <td>Address1</td>
    <td></td>
  </tr>
  <tr>
    <td>BillingStreet2</td>
    <td>Address2</td>
    <td></td>
  </tr>
  <tr>
    <td>BillingCity</td>
    <td>City</td>
    <td></td>
  </tr>
  <tr>
    <td>BillingZip</td>
    <td>ZipCode</td>
    <td></td>
  </tr>
  <tr>
    <td>BillingCountry</td>
    <td>Country</td>
    <td></td>
  </tr>
  <tr>
    <td>ContractTag1</td>
    <td>Email</td>
    <td></td>
  </tr>
</table>


#### Article (Fortnox => RemoteX)

<table>
  <tr>
    <td>Article</td>
    <td>Product</td>
    <td>Kommentar</td>
  </tr>
  <tr>
    <td>ArticleNumber</td>
    <td>Code</td>
    <td></td>
  </tr>
  <tr>
    <td>Description</td>
    <td>Title</td>
    <td></td>
  </tr>
  <tr>
    <td>Unit</td>
    <td>Metric</td>
    <td></td>
  </tr>
  <tr>
    <td>PurchasePrice</td>
    <td>CostPerPiece</td>
    <td></td>
  </tr>
  <tr>
    <td>LogMessage</td>
    <td>"Uppdaterades från Fortnox" läggs på loggen</td>
    <td></td>
  </tr>
</table>


#### Article (RemoteX => Fortnox)

<table>
  <tr>
    <td>Product</td>
    <td>Article</td>
    <td>Kommentar</td>
  </tr>
  <tr>
    <td>Code</td>
    <td>ArticleNumber</td>
    <td></td>
  </tr>
  <tr>
    <td>Description</td>
    <td>Title</td>
    <td></td>
  </tr>
  <tr>
    <td>Unit</td>
    <td>Metric</td>
    <td></td>
  </tr>
  <tr>
    <td>Product.CostPerPiece</td>
    <td>PurchasePrice</td>
    <td></td>
  </tr>
</table>


#### Case (RemoteX => Fortnox)

<table>
  <tr>
    <td>Case</td>
    <td>Invoice</td>
    <td>Kommentar</td>
  </tr>
  <tr>
    <td>DeliveryDate</td>
    <td>Dagens datum i YYYY-MM-DD format</td>
    <td></td>
  </tr>
  <tr>
    <td>String.Empty</td>
    <td>Comments</td>
    <td></td>
  </tr>
  <tr>
    <td>Note</td>
    <td>YourOrderNumber</td>
    <td></td>
  </tr>
  <tr>
    <td>Priority, Unit.Tag</td>
    <td>OurReference</td>
    <td>Beror på inställningarna "NoPriorityAsOurReference" och "OurReferenceUnitTag"</td>
  </tr>
  <tr>
    <td>Title, Description eller Title + Description</td>
    <td>Remarks</td>
    <td>Beror på inställningarna CaseFieldAsRemark och AddUnitTitleToRemark</td>
  </tr>
  <tr>
    <td>"0"</td>
    <td>RoundOff</td>
    <td>Hårdkodad</td>
  </tr>
  <tr>
    <td>Dagens datum YYYY-MM-DD</td>
    <td>InvoiceDate</td>
    <td></td>
  </tr>
  <tr>
    <td>InvoiceConnector.InvoiceType.INVOICE</td>
    <td>InvoiceType</td>
    <td>Hårdkodad</td>
  </tr>
  <tr>
    <td>"SEK"</td>
    <td>Currency</td>
    <td>Hårdkodad</td>
  </tr>
  <tr>
    <td>"1"</td>
    <td>CurrencyUnit</td>
    <td>Hårdkodad</td>
  </tr>
  <tr>
    <td>"1"</td>
    <td>CurrencyRate</td>
    <td>Hårdkodad</td>
  </tr>
  <tr>
    <td>Typ</td>
    <td>Project</td>
    <td>Invoice.Project (enligt mappning i inställning CaseTypeToProjectMapper)</td>
  </tr>
  <tr>
    <td>Case.Tag => CostCenter</td>
    <td>CostCenterTagName - Namn på extrafält på objekt som innehåller kostnadsställe som ska anges på fakturan.</td>
    <td></td>
  </tr>
  <tr>
    <td>DueDate</td>
    <td>Dagens datum + 30 dagar i YYYY-MM-DD format</td>
    <td></td>
  </tr>
  <tr>
    <td>Note</td>
    <td>YourReference</td>
    <td></td>
  </tr>
  <tr>
    <td>"30"</td>
    <td>TermsOfPayment</td>
    <td>Hårdkodad</td>
  </tr>
  <tr>
    <td>Contract.CrmSystemId</td>
    <td>CustomerNumber</td>
    <td>Ifall kund saknas sätts tom sträng</td>
  </tr>
  <tr>
    <td>BillingAddress.Attention</td>
    <td>CustomerName</td>
    <td></td>
  </tr>
  <tr>
    <td>BillingAddress.Street</td>
    <td>Address1</td>
    <td></td>
  </tr>
  <tr>
    <td>BillingAddress.Street2</td>
    <td>Address2</td>
    <td></td>
  </tr>
  <tr>
    <td>BillingAddress.City</td>
    <td>City</td>
    <td></td>
  </tr>
  <tr>
    <td>BillingAddress.Zip</td>
    <td>ZipCode</td>
    <td></td>
  </tr>
  <tr>
    <td>BillingAddress.Country</td>
    <td>Country</td>
    <td></td>
  </tr>
  <tr>
    <td>Address.Attention</td>
    <td>DeliveryName</td>
    <td></td>
  </tr>
  <tr>
    <td>Address.Street</td>
    <td>DeliveryAddress1</td>
    <td></td>
  </tr>
  <tr>
    <td>Address.Street2</td>
    <td>DeliveryAddress2</td>
    <td></td>
  </tr>
  <tr>
    <td>Address.City</td>
    <td>DeliveryCity</td>
    <td></td>
  </tr>
  <tr>
    <td>Address.Zip</td>
    <td>DeliveryZipCode</td>
    <td></td>
  </tr>
  <tr>
    <td>Address.Country</td>
    <td>DeliveryCountry</td>
    <td></td>
  </tr>
  <tr>
    <td>ExternalInvoiceReference2</td>
    <td>Id</td>
    <td></td>
  </tr>
</table>


#### UsageQuantity (RemoteX => Fortnox)

*OBS* Se inställningen "AddDateToDescription"

<table>
  <tr>
    <td>UsageQuantity</td>
    <td>InvoiceRow</td>
    <td>Kommentar</td>
  </tr>
  <tr>
    <td>Code</td>
    <td>ArticleNumber</td>
    <td></td>
  </tr>
  <tr>
    <td>Title, Description eller RegistrationDate i YYYY-MM-DD format</td>
    <td>Description</td>
    <td>Beror på inställningarna "UqDescriptionToFortnoxDescriptionWithEllipsisFallbackToTitle", "UQFieldExportedToArticleRowDescription" och "DateAsUQDescriptionForMetric"</td>
  </tr>
  <tr>
    <td>String.Empty</td>
    <td>Unit</td>
    <td></td>
  </tr>
  <tr>
    <td>Quantity</td>
    <td>DeliveredQuantity</td>
    <td></td>
  </tr>
  <tr>
    <td>PricePerPiece</td>
    <td>Price</td>
    <td></td>
  </tr>
  <tr>
    <td>Discount * 100</td>
    <td>Discount</td>
    <td>Sätts till 0 ifall discount saknas</td>
  </tr>
  <tr>
    <td>"25"</td>
    <td>VAT</td>
    <td></td>
  </tr>
  <tr>
    <td>DiscountType</td>
    <td>InvoiceConnector.DiscountType.PERCENT</td>
    <td>hårdkodad</td>
  </tr>
  <tr>
    <td>ManufacturerCode eller string.empty</td>
    <td>AccountNumber</td>
    <td>Beror på inställningen "SendAccountFromManufacturerCode"</td>
  </tr>
</table>


#### CaseRows

Dessa är tomma fakturarader bestående av beskrivning, som hämtas från extrafält på ärendet. En fakturarad per extrafält. Observera att ifall extrafältet är tomt skapas inga fakturarad för det extrafältet. De hakas på i följande ordning: 

* "Produkt: " + CaseTag1

* "Fabrikat: " + CaseTag2

* "Produktnr: " + CaseTag3 

* "Typ: " + CaseTag4 

* "Felkod: " + CaseTag5 

* "Övrigt: " + CaseTag6 

* "Inköpsställe: " + CaseTag7 

* "Inköpsdatum: " + CaseTag8 

* "Reparationskod: " + CaseTag9 

* "Modell: " + CaseTag11

*  "Serie nr: " + CaseTag12

