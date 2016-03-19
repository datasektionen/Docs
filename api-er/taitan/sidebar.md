## Taitan + Gloo = ♥

Taitan är det system som tar det Markdown-innehåll, från GitHub, som bland annat
den här dokumentationen baseras på, och gör om denna till JSON.

Gloo tar den JSON (rådata) som Taitan producerar och kombinerar denna med
mallar, skrivna i något av de 20 template-språk som Gloo stödjer.

Tillsammans producerar GitHub, Taitan och Gloo en färdig site för slutanvändare.

Innehållet redigeras på GitHub, plockas upp och formateras av Taitan, kombineras
med kompletta site-layouter av Gloo och skickas till slutanvändaren.