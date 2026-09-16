# subatomizer

Convierte un artículo ya publicado en **cinco notas de Substack**, escritas con la voz del
propio autor y ancladas a fragmentos exactos del texto.

```bash
npx skills add sotoplatero/subatomizer
```

Después, pásale una URL: «sácame notas de este artículo».

## Instalación

**En el proyecto donde estés trabajando:**

```bash
npx skills add sotoplatero/subatomizer
```

**Para tenerlo siempre, en todos tus proyectos:**

```bash
npx skills add sotoplatero/subatomizer -g -y
```

Queda en `~/.agents/skills/subatomizer` y se enlaza solo a Claude Code, Cursor, Codex, Gemini
CLI y una cuarentena de agentes más. Si usas PromptScript, la instalación global no le vale:
instálalo dentro del proyecto, sin `-g`.

**Comprobar que entró:**

```bash
ls ~/.claude/skills/subatomizer
```

**Actualizarlo** cuando cambie:

```bash
npx skills update
```

**A mano**, si prefieres no usar el CLI: copia `SKILL.md` en
`~/.claude/skills/subatomizer/SKILL.md`. Es un solo archivo y no necesita nada más.

### Qué hace falta tener

- **Un agente con skills** — Claude Code, Cursor, Codex o cualquiera de la lista.
- **[agent-browser](https://github.com/vercel-labs/agent-browser)**, recomendado, para bajar el
  artículo literal: `npm i -g agent-browser && agent-browser install`. Sin él, el skill tira de
  WebFetch y te avisa de que en ese modo las citas no quedan comprobadas contra el original.

### Cómo se usa

Pásale la URL y díselo con tus palabras:

```
sácame notas de https://tublog.substack.com/p/tu-articulo
```

También lo dispara «reparte este artículo», «atomiza este post» o «qué notas salen de esto».
No hace falta nombrar el skill.

## El problema que resuelve

Un modelo atomizando un artículo hace dos destrozos. Redondea la cifra, retoca la cita y se
inventa el ejemplo. Y escribe con su propia voz de modelo, que suena a cualquiera. Quien
publica eso lo hace con su nombre encima.

En el registro de skills hay varios que reparten un artículo en piezas —uno saca de quince a
treinta—, y **ninguno comprueba nada contra el original**.

## Cómo funciona

1. **Baja el artículo literal** con el navegador y lo guarda en `~/.claude/subatomizer/`.
2. **Lo estudia**: inventario de lo que da el texto —cifras, escenas, frases citables,
   tensiones— y perfil de voz del autor, cada rasgo con su cita.
3. **Escribe nueve candidatas**, cada una atada a un fragmento exacto.
4. **Las verifica con Grep** contra el fichero: ocho motivos de caída y un aviso. Cada cifra y
   cada cita tienen que estar en el artículo.
5. **Elige cinco** con rúbrica, con cinco objetivos distintos, y entrega también las cuatro
   descartadas con su motivo.

Cada nota sale en su propio bloque, lista para copiar y pegar.

Nueve para tener de dónde elegir. Se entregan cinco, que es lo que un artículo de dos mil
palabras da de sí.

## Lo que no hace

Ni publica, ni programa, ni escribe desde una idea suelta —hace falta un artículo del que
anclar—, ni saca hilos de X ni posts de LinkedIn. Y no rellena: si solo pasan cuatro notas la
verificación, entrega cuatro y dice por qué.

## Skills hermanos

- [**subnotes**](https://github.com/sotoplatero/subnotes) — escribe notas nuevas con tu voz,
  aprendida de tus notas publicadas. Para cuando tienes una idea, no un artículo.
- [**stackchat**](https://github.com/sotoplatero/stackchat) — pregúntale a Claude por tus
  suscriptores, tus aperturas y tus notas.

MIT · [Damian Soto](https://sotoplatero.substack.com)
