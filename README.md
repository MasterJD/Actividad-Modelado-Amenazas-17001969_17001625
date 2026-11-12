# Modelado de amenazas de EventosU

Repositorio de la tarea de modelado de amenazas del curso de Ciberseguridad, en la cual se aplica un proceso repetible de modelado al sitio web ficticio EventosU, que se encuentra en fase de diseño, y se convierte cada amenaza importante en un requisito verificable.

## Integrantes

| Nombre | Carné |
|--------|-------|
| [Nombre completo del integrante 1] | [Carné integrante 1] |
| [Nombre completo del integrante 2] | [Carné integrante 2] |

## Estructura del repositorio

```
.
├── README.md               # integrantes, alcance y cómo abrir el modelo
├── model/eventosu.json     # modelo de OWASP Threat Dragon (DFD nivel 0, nivel 1 y amenazas)
├── docs/informe.pdf        # informe final
└── docs/amenazas.csv       # tabla de amenazas exportada
```

## Alcance

El modelo cubre los componentes que la universidad diseña y opera, es decir el sitio web en React servido desde un CDN, el panel de administración en `/admin`, la API en Node.js, la base de datos PostgreSQL administrada, el bucket de objetos para afiches y CSV, el almacén de logs y la app móvil de puerta, además de todos los flujos que estos componentes intercambian con los usuarios (visitante, estudiante, organizador, personal de puerta y administrador) y con los proveedores externos (Microsoft Entra ID, pasarela de pagos y servicio de correo).

Quedan fuera la seguridad interna de los proveedores externos, de los cuales solo se analiza su interfaz con EventosU, la infraestructura física del proveedor de nube y del CDN, los sistemas operativos de los dispositivos de los usuarios, la red del campus y los procesos administrativos de la universidad que ocurren fuera del sitio.

Los supuestos utilizados, como la existencia de un almacén de logs de solo inserción o que el QR contiene en el peor caso el identificador de la inscripción, se declaran en la sección 2.3 del informe.

## Resumen del modelo

- DFD de nivel 0 con EventosU como un solo proceso y sus actores externos, y DFD de nivel 1 con todos los componentes del caso, cuatro zonas delimitadas y tres límites de confianza principales (LC1 Internet, LC2 zona de datos privada y LC3 proveedores externos), con cada flujo nombrado según su dato y protocolo.
- 28 amenazas identificadas con STRIDE (6 S, 5 T, 3 R, 7 I, 4 D y 3 E), de las cuales 16 están sobre flujos o elementos que cruzan un límite de confianza, incluyendo el análisis obligatorio del webhook de pago (T01 a T04), la validación del QR (T05 a T08) y la exportación del CSV (T09 a T13).
- Árbol de ataque para el objetivo "entrar a un evento sin pagar", con nodos O/Y y el camino más barato marcado, que corresponde a la falsificación del webhook de pago.
- Matriz de impacto por probabilidad de 3 × 3 con criterios escritos y justificación de cada calificación, con resultado de 4 amenazas críticas, 10 altas, 11 medias y 3 bajas.
- Respuesta y mitigación para cada amenaza (25 mitigadas, 1 eliminada, 1 transferida y 1 aceptada con responsable) y trazabilidad de las cinco amenazas críticas (T01, T05, T09, T17 y T16) hacia OWASP ASVS 4.0.3, historia de usuario o tarea y el control del pipeline que la verifica (SAST, DAST o prueba manual).

## Cómo abrir el modelo

El archivo `model/eventosu.json` está en formato de OWASP Threat Dragon versión 2 y fue verificado con Threat Dragon 2.6.2.

Con la aplicación de escritorio:

1. Descargar e instalar Threat Dragon desde la página de releases del proyecto (https://github.com/OWASP/threat-dragon/releases).
2. Abrir la aplicación y seleccionar **Open an existing threat model**.
3. Elegir el archivo `model/eventosu.json` de este repositorio.
4. En la vista del modelo aparecen los diagramas **DFD nivel 0 - Contexto** y **DFD nivel 1 - Componentes**, y al hacer clic en cualquiera de ellos se abre el editor, donde al seleccionar un elemento o flujo marcado en rojo se muestran sus amenazas en el panel inferior.

Con la versión web:

1. Ingresar a una instancia de Threat Dragon y seleccionar **Login to Local Session**.
2. Elegir **Open an existing threat model**, pegar el contenido de `model/eventosu.json` en el cuadro de texto o arrastrar el archivo sobre él, y presionar **Import**.

El reporte de amenazas por elemento puede generarse desde la opción **Report** de la vista del modelo.

## Informe

El informe final está en `docs/informe.pdf` y contiene la portada con los integrantes, el alcance, los activos y los supuestos, los DFD de nivel 0 y nivel 1 exportados desde Threat Dragon, la tabla de amenazas, el árbol de ataque, la matriz de riesgo, la trazabilidad de las cinco amenazas críticas y las conclusiones sobre los cambios al diseño de EventosU antes de programarlo.
