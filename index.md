# Dokumentation

## Samlad dokumentation för Datasektionens system, verktyg och miljö
På den här sidan (och det motsvarande GitHub-repositoryt) finns dokumentation för dig som jobbar med eller är intresserad av IT på Datasektionen vid THS.

Dokumentationen specificerar:
* hur du utvecklar i vår tekniska miljö,
* vilka system som finns och vad de gör,
* vilka API-endpoints systemen tillhandahåller,
* vad som gäller för system som tillhandahåller grafiska gränssnitt,
* samt hur du själv bidrar till dokumentationen.

## Snabba fakta
För dig som inväntar en tl;dr kommer här en (mycket) kort sammanfattning:
* Det är Datasektionens nämnd IOR (Informationsorganet), närmare bestämt dess gren *Crash 'n' Bränn*, som har hand om IT på sektionen.
* Du är hjärtligt välkommen att utveckla, speca, testa, designa eller på annat sätt bidra, antingen när du själv har tid och lust eller på någon av våra hackerkvällar i datorsal Grå (E-huset, plan 5, torsdagar 17-).
* Allt hostas eller ska hostas på [GitHub](https://github.com/datasektionen) (inkl. denna dokumentation)
* Allt körs eller ska köras på [Dokku](http://dokku.viewdocs.io/dokku/)
* All kod som vill se dagens ljus i produktion behöver gå att deploya till Dokku. Finns inget fungerande buildpack för din kod, är det upp till dig att lösa ett. För de flesta större miljöer löser detta sig självt.
* Alla system som tillhandahåller grafiska gränssnitt måste använda Datasektionens UI-framework, eller vid egen implementation följa de specifikationer som finns. Använder du ramverket löser mycket sig självt.
* Alla system som tillhandahåller grafiska gränssnitt måste använda vår globala top bar, Methone.
* Vår Dokku har PostgreSQL och MongoDB. Vi föredrar oftast att skriva innehåll i [CommonMark (Markdown)](http://commonmark.org/)
* Om du vill bolla, fråga eller diskutera är IORs [Slack](https://ior.slack.com) ett bra, aktivt forum.

## Hur dokumentationen fungerar
Dokumentationen är relativt simpelt uppbyggd. All dokumentation ligger i [Docs-repositoryt på GitHub](https://github.com/datasektionen/Docs), där detta dokument är rotdokument.

Varje dokument består av en huvudrubrik (h1, kan skrivas med #- eller ===-syntax), följt av lämpliga underrubriker med brödtext. Huvudrubriken (max en får förekomma) blir dokumentets titel, så _håll den kort och koncis_.

Den faktiska dokumentationen ligger sedan som Markdown-dokument i undermappar. Varje undermapp har en index.md-fil som blir avdelningens introducerande dokument. Strukturen är alltså

	index.md
	- ui
	  * index.md
	  * framework.md
	  * typography.md
	  * ...
	- system
	  * methone.md
	  * aurora.md
	  * hyperion.md
	  * ...
 
 Dokumentationssiten (Aurora) är en ASP.NET 5-app som läser Docs-repot enligt strukturen ovan, och parsar denna till vår dokumentationssite. För att lägga till, eller ändra dokumentation gör du lämpligast en pull request till Docs-repositoryt. Har du synpunkter på dokumentationen, eller vill se tillägg, är du välkommen att skapa en issue i Docs-repot. Dokumentationen bör i huvudsak vara på svenska, även om engelsk dokumentation förstås är mycket bättre än ingen alls.