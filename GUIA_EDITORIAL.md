# Guia editorial d’El Pedrenyal

Les tres iteracions, els criteris de verificació i els lliurables textuals són els d’[AGENTS.md](AGENTS.md). L’usuari només ha de proporcionar el tema. La producció visual i l’arxivament formen part de la tercera iteració, després de la verificació de l’article.

## Il·lustració

1. El director artístic lliura 08_prompt_imatge.md. El cap de redacció hi adjunta el context, l’època i les fonts verificades. L’il·lustrador demana format panoràmic 16:9, idealment 1920 × 1080, estil semirealista de novel·la gràfica històrica i composició compatible amb un petit retall.
2. L’il·lustrador usa l’eina nativa de la sessió quan la tingui disponible. Si només la pot invocar el cap de redacció, aquest executa la generació i retorna la imatge per revisar-la. Si l’eina retorna bytes o un URL de dades, materialitza’ls com a fitxer de treball i valida el format. Evita imprimir els bytes o la cadena base64. Si l’eina retorna una imatge visible però no permet recuperar-la com a fitxer, registra la limitació abans de l’arxivament.
3. Revisa indumentària, arquitectura, objectes, paisatge, data i lloc amb les fonts. Un anacronisme important justifica un altre intent amb el prompt corregit, fins a **tres generacions en total**. Una imatge amb errors materials pendents queda localment com a esborrany. La fitxa indica quins errors s’han observat.
4. Desa els bytes originals com a 11_imatge_original amb l’extensió real. En una còpia, aplica l’orientació correcta i prepara 12_imatge_blog.webp en sRGB, amb qualitat inicial orientativa de 82. Ajusta el retall a 16:9 només si es preserven els elements necessaris. Si la font té poca resolució, admet una mida 16:9 inferior, sense deformar ni ampliar artificialment. Verifica que el WebP s’obre i que les dimensions coincideixen amb la fitxa. Pillow amb suport WebP és suficient en l’entorn comprovat.
5. Escriu 13_fitxa_imatge.md amb descripció, text alternatiu SEO en català, dimensions reals, intents, referents i resultat de la revisió. Actualitza el text alternatiu de 04_seo.yaml i el seu equivalent al paquet editorial perquè descrigui la imatge seleccionada. Si queda pendent, mantén l’alt com a suggeriment.

## Arxivament a Google Drive

La carpeta d’entrada és [elpedrenyal](https://drive.google.com/drive/folders/1ca8V1Pwj--mVZ-S4aqQOxg5wevZPQ_-2). Per a un article nou, usa `elpedrenyal/AAAA/slug/`, amb l’any editorial de creació segons Europe/Madrid. Mantén el mateix any i slug en revisions.

- Cerca primer l’article per nom exacte dins de l’any i també directament sota elpedrenyal. Si trobes una carpeta històrica, reutilitza-la i informa de la ruta. Abans de crear cap carpeta, confirma que l’article no existeix amb aquell slug. Si coincideixen diverses carpetes, conserva els fitxers locals i informa del conflicte si els materials no permeten determinar quina pertany a l’article.
- Desa els fitxers 01–10, la fitxa d’imatge i les dues imatges que estiguin disponibles. Markdown i YAML es desen com a fitxers de text editables i portables, amb les extensions originals. Mantén el format original de la imatge i la còpia WebP.
- Enumera els fitxers de la carpeta per pare i nom. Si el fitxer coincideix i el contingut ja és equivalent, reutilitza’l. Si correspon al mateix artefacte i s’ha actualitzat, substitueix els bytes amb `files.update` a l’ID existent. Per als fitxers natius de Google Workspace usa l’acció d’edició corresponent. Davant una càrrega amb resposta incerta, comprova primer si existeix abans de repetir-la.
- Verifica identitat, nom, tipus, pare i mida per metadades. Si un fitxer no apareix a la llista general, comprova’n les metadades per l’ID que ha retornat l’eina. Presenta l’enllaç de carpeta i els enllaços dels documents i de les imatges verificades. Les càrregues parcials es poden reprendre amb els mateixos identificadors. Conserva sempre el paquet a `articles/slug/` si una etapa falla, i indica quins fitxers estan pendents.

## Capacitats comprovades el 27 de novembre de 2026

| Capacitat | Resultat |
|---|---|
| Generació nativa d’imatge | `image_gen` disponible en la sessió principal. Disponibilitat per subagent dependent de l’entorn. Cap article ni imatge de prova s’ha generat durant aquesta ampliació. |
| Processament local | Fitxer WebP de 1920 × 1080 creat i reobert amb Pillow. Directori de treball amb permís d’escriptura. |
| Google Drive | Connector autenticat. Lectura de la carpeta verificada i escriptura confirmada amb la càrrega prèvia dels deu fitxers de la Gran Fam d’Irlanda. El connector admet la substitució de fitxers per ID. |
| API d’OpenAI | L’endpoint de la prova ha respost HTTP 401 sense autenticació. No s’ha trobat `OPENAI_API_KEY` a l’entorn ni s’ha comprovat cap accés autenticat a generació per API. |
| API directa de Drive | L’endpoint de la prova ha respost HTTP 403 sense autenticació. No s’han trobat `GOOGLE_APPLICATION_CREDENTIALS` ni `GOOGLE_OAUTH_ACCESS_TOKEN`. El connector usa la seva connexió pròpia. |
| Internet | Connexió comprovada amb els endpoints esmentats i GitHub; altres dominis depenen de la configuració de l’entorn. |

## Configuració manual si una eina deixa d’estar disponible

- **Drive.** Connecta Google Drive a la configuració d’aplicacions de ChatGPT amb el compte que té accés d’edició a elpedrenyal. Si la carpeta és d’un altre compte, el propietari ha de concedir accés d’editor al compte connectat. Verifica que es pot llistar la carpeta i desar-hi fitxers. Aquesta sessió ja disposa del connector autenticat. [Connector de Google Drive](https://help.openai.com/en/articles/11487775-connectors-in-chatgpt)
- **Imatges.** Si una altra sessió no ofereix ImageGen, comprova primer si es pot obrir el projecte en un entorn amb l’eina nativa. Per preparar la via API, crea o tria un projecte a OpenAI Platform, habilita facturació i l’accés a generació d’imatges, crea-hi una clau i configura-la com a secret `OPENAI_API_KEY` de l’entorn d’execució. Comprova l’accés amb el model d’imatge habilitat i guarda els bytes retornats. L’adaptació 16:9 continua aplicant-se si el model només ofereix mides predefinides. Aquesta via queda documentada com a alternativa i necessita una comprovació autenticada abans d’usar-la. [Guia d’imatges d’OpenAI](https://platform.openai.com/docs/guides/image-generation)
- **API directa de Drive.** Si el connector no està disponible i cal continuar per API, habilita Google Drive API en un projecte de Google Cloud, configura consentiment i client OAuth, autoritza el mateix compte i comprova l’accés a la carpeta existent amb l’abast concedit. L’abast `drive.file` només cobreix fitxers autoritzats per a l’aplicació. Desa les credencials i tokens al gestor de secrets de l’entorn. Aquesta via no és necessària amb el connector actual. [Google Drive API i autorització](https://developers.google.com/workspace/drive/api/guides/api-specific-auth)

Cap clau, token, fitxer de credencials o document editorial es desa al repositori com a part d’aquest flux. El paquet s’arxiva a Drive i no es publica automàticament al blog ni a les xarxes.
