---
title: Bilder
description: Hvordan legge til og endre referanser til bilder.
toc: true
weight: 40
---

## Legge til bilder i applikasjonen

Å legge til bilder gjøres i _FormLayout.json_ ved bruk av bildekomponenten. Alternativ tekst for bildet legges til som en tekst ressurs, som er definert i _resource.[språk].json_.

```json
{
  "data": {
    "layout": [
      {
        "id": "616071dc-90b1-4ce5-8d18-492844828a41",
        "type": "Image",
        "textResourceBindings": {
          "altTextImg": "imgAltText"
        },
        "image": {
          "src": {
            "nb": "https://example.com/image_nb.png"
          },
          "width": "100%",
          "align": "center"
        }
      }
    ]
  }
}
```

Bildet kan også ha ulik kilde i forskjellige språk. Standardkilde er _nb_, og denne brukes for språk som ikke har oppgitt en egen kilde for bilder. Eksempel med forskjellig kilde for _nb_ og _en_:

```json
{
  "data": {
    "layout": [
      {
        "id": "616071dc-90b1-4ce5-8d18-492844828a41",
        "type": "Image",
        "textResourceBindings": {
          "altTextImg": "imgAltText"
        },
        "image": {
          "src": {
            "nb": "https://example.com/image_nb.png",
            "en": "https://example.com/image_en.png"
          },
          "width": "100%",
          "align": "center"
        }
      }
    ]
  }
}
```

## Hosting av bilder fra app

Dersom bildet skal lastes fra appen, må man sette opp statisk hosting av filer i applikasjonen.
Dette konfigureres i _App/Startup.cs_, i _Configure_ metoden. Dette vil så hoste alle filer som ligger i `/app/wwwroot` mappen. Om denne mappen ikke finnes må den opprettes.
Ønsker du å referer til filen `app/wwwroot/bilde_nb.png` så vil denne kunne nås med følgende path fra `/org/app-name/bilde_nb.png`

Bytt ut _org/app-name_ med din organisasjon og app navn. Eksempel:

```C# {linenos=false,hl_lines=[5]}
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
  {
    // ...
    app.UseRouting();
    app.UseStaticFiles("/org/app-name");
    app.UseAuthentication();
    // ...
  }
```

I _FormLayout.json_ må referansen til bildet være en relativ url som starter med _/org/app-name_ som ble satt opp i static hosting:

```json
{
  "data": {
    "layout": [
      {
        "id": "616071dc-90b1-4ce5-8d18-492844828a41",
        "type": "Image",
        "textResourceBindings": {
          "altTextImg": "imgAltText"
        },
        "image": {
          "src": {
            "nb": "/org/app-name/bilde_nb.png"
          },
          "width": "100%",
          "align": "center"
        }
      }
    ]
  }
}
```