# Página de vocabulario · léxico rotatorio (Unidad 1: La amistad)

Nota técnica de la página interactiva «Vocabulario de la amistad».
Complementa `claude/Unidad1_La_amistad_notas.md`, que describe el resto de la unidad.

- **Artifact (copia del docente):** https://claude.ai/artifact/ULfm6ArU6RRqEmPyQ7heEz (versión 3)
- **Archivo para los estudiantes:** `Vocabulario_La_amistad.html` (67 KB, autónomo, sin red)
- **Fuente en sesión:** `/home/claude/vocab-amistad.html`; el archivo suelto se genera con `wrap.py`

## El cambio del 21 de septiembre: las palabras rotan

Antes, cada bloque tenía una lista fija copiada de la p. 37–38. Ahora hay **un solo léxico de 48 palabras** y cada palabra declara los papeles que puede desempeñar:

| clave | papel | palabras disponibles |
|---|---|---|
| `fam` | palabra de la misma familia | 42 |
| `ant` | antónimo | 36 |
| `sin` | sinónimo | 45 |
| `ctx` | oración original en contexto | 33 |

En cada **serie** se sortean 10 palabras para el bloque I, 6 para el II, 6 para el III y 6 para el IV. Así *el cariño* puede tocar en sinónimos un día (respuestas: afecto, amor, aprecio, ternura, apego…), en antónimos otro (odio, desprecio, rencor…), en familia otro (cariñoso, acariciar, la caricia…) y en oración propia otro. Igual con *confiar*: familia (confianza, confidente…), antónimo (desconfiar, dudar, sospechar…), sinónimo (fiarse, creer en, contar con…) u oración.

El **bloque V** (huecos del artículo) **no rota**: son citas literales con referencia de líneas, y el orden del artículo es el que da sentido al ejercicio.

Cada papel tiene su propia **lista de respuestas aceptadas** y su propia **pista** — la pista de *la apertura* como antónimo («el sustantivo de quien no deja entrar ninguna idea nueva») no sirve como pista de sinónimo, así que están escritas por separado.

## Cómo funciona la serie

- PRNG `mulberry32` con semilla + Fisher–Yates. El mismo número de serie **siempre** produce el mismo sorteo, en cualquier ordenador.
- Prioridad de la semilla: `#serie=1234` en la dirección → la serie guardada en la pestaña → una nueva al azar (1000–9999).
- El número se muestra arriba a la derecha y aparece en la cabecera del repaso impreso.
- Botón **Mezclar**: sortea otra serie, borra las respuestas (avisa antes si ya había trabajo hecho) y actualiza el `#serie=` de la dirección.
- Botón **Empezar de nuevo**: borra las respuestas y **conserva** las mismas palabras.
- Al recargar la página, la serie se mantiene y el trabajo no se pierde. Si la serie cambia, las respuestas se descartan: las posiciones ya no corresponden a las mismas palabras.
- Se evita que una palabra salga dos veces **en el mismo bloque** (garantizado: comprobado en 400 series seguidas). Entre bloques distintos se evita en lo posible pero se permite — la hoja original ya repetía *el cariño* en antónimos y sinónimos.

### Usos didácticos de la serie
- **Toda la clase con las mismas palabras:** repartir el enlace con `#serie=1234` al final. Útil para corregir en voz alta o para un examen práctico.
- **Cada estudiante con palabras distintas:** repartir el enlace sin `#serie=`.
- **Repetir en casa con otro sorteo:** el estudiante pulsa *Mezclar*.
- **Reproducir el conjunto de un estudiante:** basta el número de serie que aparece en su repaso impreso.

## Verificación automática hecha

Sobre el archivo suelto, con la red completamente bloqueada:
- 400 series consecutivas: ningún bloque incompleto, ninguna palabra repetida dentro de un bloque.
- Ninguna lista de respuestas aceptadas contiene la propia palabra de la consigna.
- Las 33 regex del bloque IV casan con su propia oración modelo.
- Comprobación de respuestas indiferente a acentos, mayúsculas y artículos (`ODIO`, `el afecto`, `homogeneas` → correctas).
- Sin errores de JavaScript; las fuentes de Google degradan a las pilas de reserva.

## Si hay que ampliar el léxico

Añadir una entrada al array `LEXICO`, en el `<script>` de la página:

```js
{w:"la lealtad",
 fam:{ok:["leal","lealmente","desleal"], p:"El adjetivo de quien la practica."},
 ant:{ok:["traición","deslealtad","infidelidad"], p:"Lo que hace quien rompe la confianza."},
 sin:{ok:["fidelidad","fiabilidad","devoción"], p:"Un sustantivo para la firmeza en el afecto."},
 ctx:{t:"lealtad", modelo:"…", p:"…"}}
```

Solo hacen falta los papeles que se quieran; los demás se omiten y esa palabra simplemente no se sortea para ese bloque. Después hay que regenerar el archivo suelto con `wrap.py` y volver a subirlo a Canvas **con el mismo nombre**.
