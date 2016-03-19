Taitan (API)
============

Taitan är det system som tar hand om "översättningen" från Markdown-dokument
som ligger på GitHub, till ett HTTP RESTful JSON API som går att hämta data från.

Taitan tar hand om att:

1. Läsa uppdaterat källmaterial från GitHub (se Installation och konfiguration nedan)
2. Konvertera den Markdown som dokumenten skrivs i till HTML-kod
3. Kombinera alla filer (body.md, sidebar.md, meta.toml) för varje dokument
4. Skicka all dokument- och metadata som JSON vid request

## Dokumentstruktur (källmaterial)

Varje dokument består av tre filer: body.md, sidebar.md och meta.toml.
Body.md innehåller dokumentets brödtext och innehåll.
Sidebar.md innehåller kompletterande information som lämpligast läggs i just en sidebar.
Titeln på sidan specificeras i meta.toml-filen som kompletterar
varje body.md och sidebar.md-filpar.

All dokumentation ligger enligt denna struktur i undermappar.
Ett GitHub-repo med dokumentation kan alltså ha denna struktur:

	- body.md
	- sidebar.md
	- meta.toml
	* namnder/
	  - body.md
	  - sidebar.md
	  - meta.toml
	  * dkm/
	    - body.md
	    - sidebar.md
	    - meta.toml
	  * ior/
	    - body.md
	    - sidebar.md
	    - meta.toml
	  * ...
	* sektionen/
	  - body.md
	  - sidebar.md
	  - meta.toml
	  * formalia/
	  * grafik/
	  * funktionarer/
	  * ...

Varje mapp, inklusive rotmappen, *måste ha* alla tre filer; body.md, sidebar.md, meta.toml.
Om inte alla tre finns kommer Taitan inte att behandla mappen som en giltig path.

## API och användning

För att hämta ett dokument med tillhörande metadata:

`GET /:path`

## Exempelrespons

```json
{
  "title": "Om Oss",
  "slug": "om-oss",
  "updated_at": "2015-11-06T02:04:58Z",
  "image": "http://s3.datasektionen.se/imagex.png",
  "body": "<h1>...",
  "sidebar": "<ul>...",
  "anchors": [{"id":"vilka-vi-är", "value":"Vilka vi är"}],
  "children": [
    { "slug":"/om-oss/kontakt", "title":"Kontakt", "children": [
      {"slug":"/om-oss/kontakt/nlg","title":"Näringslivsgruppen"}
    ]},
    {"slug":"/om-oss/hitta-hit","title":"Hitta hit"}
  ]
}
```

Förklaring av responsens olika fält:

|  Attribut  |                                                                                        Förklaring                                                                                       |
|:----------:|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
| title      | Title från meta.toml                                                                                                                                                                    |
| slug       | Den URL-slug som efterfrågats, exempelvis `/api-er/methone`                                                                                                                             |
| updated_at | Datum och tid då body.md uppdaterades, specificerad enligt ISO 8601-standarde                                                                                                           |
| image      | URL till ev. bildfil för dokumentet, från meta.toml                                                                                                                                     |
| body       | Data från body.md, HTML-konverterad                                                                                                                                                     |
| sidebar    | Data från sidebar.md, HTML-konverterad                                                                                                                                                  |
| anchors    | Array med objekt bestående av id (HTML anchor-id, ex. #om-oss) samt titel (Om oss) som länkar till olika delar av dokumentet (baseras på nivå 2-headings, dvs `##` eller --- i Markdown |
| children   | Array med sidor som kan läggas i navigationsmeny, i form av objekt bestående av slug samt sidtitel. Varje objekt kan också ha en children-key, med samma utseende (rekursivt)           |

## Kör och utveckla lokalt

```bash
$ go get -u github.com/datasektionen/taitan
$ taitan -v
INFO[0000] Our root directory                            Root=dummy-data/
INFO[0000] Starting server.
INFO[0000] Listening on port: 4000
...
```

## Installation och konfiguration

#### Flera instanser av Taitan

Taitan är byggt för att hantera ett GitHub-repository. För att låta Taitan serva
innehåll från flera olika repositories behöver man därför köra flera instanser av
Taitan. Tack vare att varje app i Dokku är en Docker-container, och därmed
någorlunda isolerad från resten av systemet, är detta inget problem.

#### Environment-variabler

Taitan kräver att två environment-variabler är satta vid körning:

| Variabelnamn |                                           Värde/förklaring                                          |
|--------------|-----------------------------------------------------------------------------------------------------|
| CONTENT_URL  | URL till Git-repo; exempelvis https://github.com/dsekt/docs.git                                     |
| TOKEN        | Token från GitHub, genereras under Settings > Personal access tokens. Välj public_repo-rättigheten. |

#### Webhooks

För att Taitan ska få notifieringar när ett repo uppdateras behöver man lägga till
en webhook till sin Taitan-installation under (repo) > Settings > Webhooks.

Tryck på Add Webhook.
Om din Taitan-instans ligger på `http://styrdokument-taitan.datasektionen.se`, fyller
du i denna URL under `Payload URL`. Content-type ska vara JSON, Secret key kan lämnas tom,
välj `Just the push event` under event-valet och spara. Detta pingar Taitan så fort
en push görs till GitHub-repot, så att Taitan-datan alltid är uppdaterad.